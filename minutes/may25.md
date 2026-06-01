# Minuta de Reunión: Bazaar (Grupo 4)

**Fecha:** 25 de mayo de 2026

---

### Puntos de Discusión y Notas

- **Pasarela de Pagos (Mercado Pago):** Es necesario definir y documentar las credenciales de prueba oficiales (tarjetas de test y usuarios de prueba del *sandbox*) para poder validar el flujo de pago sin usar cuentas reales.
- **Monitoreo y Observabilidad:** Se acordó la integración de **Prometheus** como recolector de métricas de los microservicios, planteando la utilización de **Grafana** (o una alternativa similar) para la visualización y el armado de tableros (*dashboards*) de control.
- **Frontend y Maquetación:** Se detectó un problema de diseño en la interfaz de usuario donde ciertos botones quedan encimados u ocultos bajo la barra de navegación (*navbar*), requiriendo un ajuste en los estilos y capas (Z-index/márgenes).

---

### Tareas para el próximo Check

- [ ] **Automatización de Entorno:** Desarrollar un script unificado que permita levantar la totalidad de los servicios del ecosistema de forma automatizada y local.
- [ ] **Métricas y Monitoreo:** Configurar la recolección de datos con **Prometheus** y establecer las gráficas correspondientes en **Grafana**.
- [ ] **Corrección de UI:** Solucionar el solapamiento de los botones con la barra de navegación en el Frontend.
- [ ] **Documentación Central:** Ampliar la documentación técnica del proyecto e incluir de forma explícita el acceso y los enlaces (*links*) a las minutas de las reuniones anteriores para mantener la trazabilidad.
