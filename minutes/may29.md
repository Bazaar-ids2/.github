# Minuta de Reunión: Bazaar (Grupo 4)

**Fecha:** 29 de mayo de 2026

---

### Puntos de Discusión y Notas

- **Estrategia de Monitoreo:** Se definió como prioridad para el próximo *checkpoint* contar con un esquema de telemetría claro, dividiendo los indicadores en dos grandes categorías:
  1. **Métricas de Procesamiento:** Rendimiento del backend, tiempos de respuesta y estado de los microservicios.
  2. **Métricas de Negocio:** Volumen de órdenes, transacciones procesadas, uso de cupones, entre otros.
- **Visualización en el Backoffice:** Se evaluó la viabilidad técnica de embeber o exponer las métricas de procesamiento directamente dentro de la plataforma de *Backoffice* para facilitar su consumo y centralizar la administración.
- **Ingeniería de Calidad (Pruebas de Carga):** Con miras a las etapas posteriores al *checkpoint*, se comenzó a planificar la estrategia de pruebas de estrés y rendimiento, preseleccionando **Artillery** como la herramienta para diseñar y ejecutar los escenarios de carga sobre la arquitectura.
- **Alineación de Requerimientos:** Es indispensable realizar una auditoría exhaustiva sobre los Requerimientos No Funcionales (RNF) definidos en el alcance del proyecto para garantizar su cumplimiento de cara a las evaluaciones.

---

### Tareas para el próximo Check

- [ ] **Instrumentación de Métricas:** Implementar y exponer los *endpoints* o agentes necesarios para recolectar tanto las métricas de infraestructura/procesamiento como las de negocio.
- [ ] **Corrección de Bugs (Notificaciones):** Investigar y solucionar la falla identificada en el método `GET` del módulo de notificaciones.
- [ ] **Corrección de Bugs (Paginación):** Resolver el comportamiento de bucle o petición infinita (`GET` infinito) detectado en la paginación del catálogo de productos.
- [ ] **Auditoría de RNF:** Revisar y validar el estado de situación de cada uno de los requerimientos no funcionales del sistema.
- [ ] **I+D en Testing de Carga:** Investigar la integración y configuración inicial de **Artillery** para preparar las pruebas de carga post-checkpoint.
