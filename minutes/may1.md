# Minuta de Reunión: Bazaar (Grupo 4)

**Fecha:** 1 de mayo de 2026

---

### Puntos de Discusión y Notas

- **Frontend y Backend:** - Se completó el cambio a **HTTPS** en el módulo de usuarios.
  - Se finalizaron en el Frente (*Front*) las interfaces y flujos de **Cupones**, **Órdenes**, la **conexión con Mercado Pago (MP)** y el **recupero de contraseñas** (esta lógica también quedó lista en el Backend).
- **Pruebas y Cobertura (Testing):**
  - Con respecto a los tests en **Docker**, se planteó exportar un volumen para poder revisar el *coverage*.
  - Se evaluó la viabilidad de volcar el *coverage* a un archivo para luego integrarlo y pasarlo a **Codecov** mediante **GitHub Actions**.
- **Integraciones:** Queda pendiente validar con **Santi** la implementación de los **webhooks**.

---

### Tareas para el próximo Check

- [ ] **Simulación de Pagos:** Mockear el proceso de pago para poder visualizar y validar todo el flujo de las órdenes.
- [ ] **Persistencia y UI:** Cambiar manualmente el estado de alguna orden en la Base de Datos (**DB**) para verificar que el cambio se refleje correctamente en el **Frontend**.
- [ ] **CI/CD y Cobertura:** Configurar el pipeline para pasar el archivo de *coverage* a **Codecov** vía **GitHub Actions**.
