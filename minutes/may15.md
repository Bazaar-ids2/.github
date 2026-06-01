# Minuta de Reunión: Bazaar (Grupo 4)

**Fecha:** 15 de mayo de 2026

---

### Puntos de Discusión y Notas

- **Módulo de Órdenes:** Es prioritario investigar y resolver las fallas y excepciones detectadas en el flujo de *orders*.
- **Experiencia de Usuario (UX) en la Home:** Se evaluó la posibilidad de añadir elementos dinámicos o accesos rápidos (por ejemplo banners) para facilitar el ingreso a las diferentes categorías de productos.
- **Seguridad y Control de Acceso:** 
  - Se estableció que el sistema debe ocultar y restringir de forma estricta los productos deshabilitados en la interfaz comercial.
  - Se debe implementar un mecanismo de validación riguroso para bloquear de manera total y preventiva el acceso a la plataforma a aquellos usuarios que se encuentren en estado bloqueado.
- **Políticas de Manejo de Errores:** Se acordó estandarizar y depurar los mensajes de error orientados al usuario final. Se deben omitir detalles técnicos de bajo nivel o trazas de implementación que puedan comprometer la seguridad o la claridad del sistema.

---

### Tareas para el próximo Check

- [ ] **Depuración:** Diagnosticar y solucionar los errores del módulo de *orders*.
- [ ] **Refactor de la Home:** Diseñar e implementar los accesos a las distintas categorías en la página principal.
- [ ] **Filtros de Visibilidad:** Aplicar la lógica de exclusión para no renderizar productos deshabilitados.
- [ ] **Restricción de Usuarios:** Desarrollar el middleware o control necesario para impedir el acceso total a usuarios bloqueados.
- [ ] **Seguridad en Errores:** Sanitizar las respuestas de error del sistema para no exponer detalles de implementación.
- [ ] **Documentación del Proyecto:** Ampliar y mejorar la documentación técnica incorporando diagramas y especificaciones de los flujos de usuario.
