# Minuta de Reunión: Bazaar (Grupo 4)

**Fecha:** 10 de abril de 2026  
**Participantes:**

- **Máximo Utrera** (Tutor del equipo)
- Emanuel Tello
- Federico Nicolás Pagnotta Lemes
- Melina Callebaut
- Santiago Henseler
- Gaspar Notta
- Rodrigo Rotondo

---

### Decisiones de Arquitectura e Infraestructura

- **Persistencia y Almacenamiento:**
  - Se migrará el esquema de bases de datos hacia soluciones gestionadas (SaaS) para evitar la administración de servidores propios.
  - **MongoDB:** Se utilizará **MongoDB Atlas**.
  - **Relacional/General:** Se optó por **Supabase** en lugar de servidores dedicados.
  - **Multimedia:** El almacenamiento de imágenes se centralizará en **Supabase Storage**.
- **Estrategia de Disponibilidad (Render):**
  - Dado que Render suspende las instancias gratuitas por inactividad, se implementará un **script de keep-alive**. Este realizará peticiones periódicas a un endpoint para garantizar que los servicios estén activos durante las demostraciones.
- **Red y Seguridad:**
  - Se buscará configurar el **API Gateway** como el único punto de entrada con exposición exterior, manteniendo el resto de los microservicios aislados.

---

### Compromisos para el Checkpoint (Viernes 17/04)

El equipo debe cumplir con los siguientes entregables técnicos para el próximo control:

- **Despliegue Integral:** Todos los microservicios deben estar operativos en la nube.
- **Automatización:** Cada repositorio debe contar con sus respectivos pipelines de **CI/CD** funcionales.
- **Demostración E2E:** Presentar un flujo **End-to-End** mínimo (similar al ejemplo de _Products_) que integre exitosamente el Frontend con el Backend.
