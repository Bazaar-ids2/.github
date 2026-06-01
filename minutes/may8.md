# Minuta de Reunión: Avance de Proyecto - Checkpoint 2

## 📌 Resumen de Estatus
Se ha alcanzado el mínimo de historias de usuario requeridas para el **Checkpoint 2**. El flujo principal de la aplicación es funcional, aunque existen tareas técnicas y de monitoreo pendientes para asegurar la estabilidad y calidad del sistema.

---

## ✅ Logros y Funcionalidades Implementadas

### 🔑 Usuario y Perfil
- [x] **Inicio de Sesión:** Módulo de autenticación operativo.
- [x] **Wishlist:** Funcionalidad de lista de deseos completada y visible.

### 🛒 Proceso de Compra (Checkout)
- [x] **Lógica de Carrito:** Los productos se eliminan automáticamente del carrito al finalizar la transacción.
- [x] **Agrupación por Vendedor:** Si una orden contiene varios productos de un mismo vendedor, el sistema los agrupa correctamente en una única compra.
- [x] **Gestión de Envíos:** El comprador tiene la capacidad de insertar el código de envío en el sistema.

### 📦 Gestión de Órdenes y Stock
- [x] **Flujo de Cancelación:** Implementada la capacidad de comprar y cancelar. Ambos roles (comprador y vendedor) visualizan el estado de la orden como "Cancelada".
- [x] **Gestión de Inventario:** El stock se renueva automáticamente cuando una orden es cancelada.

### 🖥️ BackOffice (Fase Inicial)
- [x] Interfaz base del BackOffice presentada.

---

## ⏳ Pendientes y Próximos Pasos

### 🛠️ Desarrollo Técnico
- [ ] **Deeplinks:** Cambiar el deeplink actual a una API para que sea plenamente clickeable y funcional.
- [ ] **Funcionalidad BackOffice:** - Implementar acciones de gestión de productos.
    - Implementar visualización de métricas de negocio.
- [ ] **Reembolsos:** Desarrollar la lógica de devolución de dinero (la lógica de stock ya está lista).

### 📊 Métricas e Infraestructura
- [ ] **Monitoreo:** Integrar el stack de observabilidad (**Prometheus, Grafana, Datadog**).
- [ ] **Calidad:** Reportar y asegurar el nivel de **Coverage** (cobertura de código) requerido.

---

## ⚠️ Puntos de Atención Crítica
> **Prioridad Máxima: Transacciones Financieras**
> Es imperativo extremar las medidas de seguridad y validación en los casos de manejo de dinero. Se deben realizar pruebas exhaustivas para evitar:
> - Errores de doble cobro.
> - Inconsistencias en el saldo o estados de cuenta.

---
**Fecha de Revisión:** 8 de mayo de 2026  
