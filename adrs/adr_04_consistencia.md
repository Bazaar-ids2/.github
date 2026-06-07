
# ADR-04: Consistencia Distribuida en Checkout y Cancelación de Órdenes
 
- **Estado**: Aceptado

---
 
## Contexto
 
El flujo de checkout y cancelación de órdenes involucra múltiples servicios: Orders, Products y MercadoPago. Ante un fallo parcial en cualquiera de estos pasos, el sistema puede quedar en un estado inconsistente — por ejemplo, una orden creada pero sin stock descontado, o un pago procesado sin orden asociada.
 
Los escenarios críticos identificados son:
 
1. **Checkout**: se crea la orden, se reserva stock en Products y se procesa el pago en MercadoPago. Si falla el pago, la orden queda en `PENDING_PAYMENT` y el stock reservado debe liberarse eventualmente.
2. **Cancelación**: se cancela la orden en Orders, se restaura el stock en Products y se procesa el reembolso en MercadoPago. Si falla el restock o el reembolso, la orden ya fue cancelada.
3. **Concurrencia en stock**: múltiples usuarios pueden intentar comprar el mismo producto simultáneamente, lo que puede resultar en stock negativo si no se maneja correctamente.
---
 
## Decisión
 
Se adoptó una estrategia de **compensación explícita con estados intermedios** para los flujos críticos, sin implementar el patrón Saga completo con coordinador centralizado. Cada paso del flujo tiene un estado de orden asociado que permite rastrear en qué punto se encuentra y tomar acciones correctivas.
 
### Flujo de checkout
 
1. Se crea la orden en estado `PENDING_PAYMENT` en Orders.
2. El frontend inicia el proceso de pago en MercadoPago via `createPreference`.
3. MercadoPago notifica el resultado via webhook a `/orders/mercadoPago/payments`.
4. Orders actualiza el estado a `CONFIRMED` (pago aprobado) o `PAYMENT_REJECTED`.
5. Products descuenta el stock definitivo al confirmar el pago (`/products/confirm`).
El stock se maneja en dos fases: `checkoutStock` (reservado durante el proceso de pago) y `stock` (stock real disponible). Esto evita que otro usuario compre un producto que ya está en proceso de checkout. Los locks pesimistas (`PESSIMISTIC_WRITE`) en el repositorio de Products garantizan consistencia ante accesos concurrentes.
 
### Flujo de cancelación — Orquestado desde el API Gateway
 
La cancelación está orquestada por el plugin `cancel-order-orchestrator` en Kong:
 
1. Kong intercepta el request de cancelación (`PATCH /api/orders/{id}/status` con `status: CANCELLED`).
2. Kong llama a Orders para cancelar la orden — Orders la marca como `CANCELLED` y si el pago fue aprobado, inicia el flujo de reembolso transitando por `REFUND_IN_PROGRESS` → `REFUND_PROCESSED`.
3. Kong llama a Products para restaurar el stock (`/products/restock`) con los items de la orden cancelada.
4. Si el restock falla, Kong loguea el error pero no revierte la cancelación — la orden ya fue cancelada y el reembolso procesado. El fallo de restock se registra en los logs para revisión manual.
### Manejo de concurrencia en Products
 
Products usa **locks pesimistas** (`PESSIMISTIC_WRITE` via JPA) al leer productos para operaciones de escritura de stock. Esto serializa el acceso concurrente y evita condiciones de carrera donde dos usuarios compren el mismo producto simultáneamente.
 
---
 
## Alternativas evaluadas
 
### Saga Pattern con orquestador dedicado
 
Implementar un servicio orquestador separado que coordine los pasos del checkout y la cancelación, con rollbacks explícitos ante cada fallo. Es el patrón recomendado para transacciones distribuidas complejas y ofrece mayor trazabilidad y control.
 
**Descartado**: agrega un servicio adicional con su propia base de datos y ciclo de despliegue. La complejidad operativa supera el beneficio para el alcance del proyecto. La lógica de orquestación de cancelaciones se implementó directamente en el plugin del API Gateway como alternativa más liviana.
 
### Saga Pattern con coreografía (eventos)
 
Cada servicio reacciona a eventos publicados por otros — Orders publica "orden creada", Products escucha y descuenta stock, etc. Ya se usa RabbitMQ en el sistema para notificaciones.
 
**Descartado**: la coreografía dificulta el seguimiento del flujo completo y hace más complejo el manejo de compensaciones. Para el checkout, donde el orden de los pasos importa y los errores deben manejarse de forma predecible, la orquestación explícita es más clara.
 
### Transacciones distribuidas con 2PC (Two-Phase Commit)
 
Protocolo que garantiza atomicidad entre múltiples bases de datos. Requiere que todos los servicios participantes soporten el protocolo y un coordinador de transacciones.
 
**Descartado**: no es viable en una arquitectura de microservicios donde cada servicio tiene su propia base de datos independiente. Además introduce latencia y un punto único de fallo en el coordinador.
 
---
 
## Consecuencias
 
- El estado de la orden refleja en todo momento en qué paso del flujo se encuentra. Los estados `REFUND_IN_PROGRESS` y `REFUND_PROCESSED` permiten diagnosticar órdenes canceladas con reembolso en curso.
- Si el restock falla tras una cancelación, el stock de Products queda desactualizado. Este es un fallo conocido y aceptado — se detecta via logs y requiere corrección manual. Se prioriza que la cancelación y el reembolso lleguen al usuario sobre la consistencia del stock.
- Los locks pesimistas en Products pueden generar contención bajo carga alta con muchos usuarios comprando el mismo producto simultáneamente. Para el volumen esperado en el contexto académico esto es aceptable.
- Cualquier nuevo flujo que involucre múltiples servicios debe definir explícitamente sus estados intermedios y su estrategia de compensación antes de implementarse.
- El plugin `cancel-order-orchestrator` en Kong concentra lógica de negocio en el gateway — cualquier cambio en el flujo de cancelación requiere redeploy del gateway además de los servicios afectados.
 