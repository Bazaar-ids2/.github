# Minuta de Reunión: Seguimiento TP - Grupo 4 (Inge2)

**Fecha:** 24 de abril de 2026  
**Participantes:**

- **Máximo Utrera** (Tutor del equipo)
- Emanuel Tello
- Federico Nicolás Pagnotta Lemes
- Melina Callebaut
- Santiago Henseler
- Gaspar Notta
- Rodrigo Rotondo

---

### Definiciones de Producto y Lógica de Negocio (Bazaar)

- **Sistema de Cupones:** Se definió que los cupones de descuento tendrán un alcance **global**.
  - Cada cupón debe poseer un nombre único en el sistema.
  - No estarán vinculados a un vendedor específico, simplificando la lógica de validación en el carrito.
- **Seguridad y Acceso:** Para el registro de dispositivos, se implementará un sistema de **registro con PIN por dispositivo**.

---

### Definiciones Técnicas y DevOps

- **Notificaciones:** Se utilizará **Firebase** como proveedor para la implementación de **notificaciones push**.
- **Integración con Pasarela de Pagos:** Se realizará una revisión técnica de los _webhooks_ o _requests_ de **Mercado Pago**.
  - El objetivo es asegurar que las peticiones ingresen a través del **API Gateway**.
  - Deben incluir correctamente el header de `Authorization` para mantener la consistencia con el esquema de seguridad del sistema.
- **Pipeline de CI/CD (Mobile):** Se analizó la intermitencia en el **GitHub Action** encargado del _build_ de la APK.
  - Se determinó que no es un bloqueo crítico si la acción falla ocasionalmente por exceder los tiempos de ejecución (_timeouts_), siempre que la construcción del artefacto sea correcta en condiciones normales.

---

### Gestión del Proyecto (Tracking)

- **Actualización de Tickets:** Es imperativo revisar el board de tareas del proyecto "Bazaar".
- **Ajuste de Compromisos:** Se deben actualizar los tags de **Checkpoints** en las Historias de Usuario (HDU).
  - El compromiso de las tareas actuales debe quedar reflejado para el **Checkpoint 2**.

---

### Tareas Pendientes

- [ ] Implementar la lógica de unicidad global para cupones en el microservicio correspondiente.
- [ ] Configurar el proyecto en Firebase y probar el envío de una notificación push básica.
- [ ] Ajustar las reglas de ruteo en el Gateway para las notificaciones de Mercado Pago.
- [ ] Renombrar/Etiquetar los tickets pendientes con el tag "Checkpoint 2".
