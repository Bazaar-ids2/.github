# Minuta de Reunión: Bazaar (Grupo 4)

**Fecha:** 17 de abril de 2026  
**Participantes:**

- **Máximo Utrera** (Tutor del equipo)
- Emanuel Tello
- Federico Nicolás Pagnotta Lemes
- Melina Callebaut
- Santiago Henseler
- Gaspar Notta
- Rodrigo Rotondo

---

### Definiciones Técnicas y Calidad

- **Almacenamiento:** Se ratifica el uso de **Supabase Storage** para la gestión de imágenes (`imgs`).
- **Métricas de Calidad (Testing):**
  - Se establece un objetivo de **Code Coverage > 70%** para el Backend.
  - **Visibilidad:** Los reportes de cobertura deben estar integrados de forma **visible** en el Pipeline de CI (no deben quedar ocultos en los logs), sugiriendo el uso de herramientas como **Codecov**.
- **Seguridad:**
  - Gestión estricta de credenciales y contraseñas (`pass`). Se asume la implementación de variables de entorno seguras para evitar "leaks" en los repositorios.

---

### Tareas Pendientes Prioritarias

- [ ] Configurar **Codecov** (o similar) en el pipeline para que el coverage sea público y visible en los Pull Requests.
- [ ] Asegurar que el coverage del Backend alcance o supere el **70%**.
- [ ] Finalizar la migración de la lógica de guardado de imágenes a **Supabase**.
