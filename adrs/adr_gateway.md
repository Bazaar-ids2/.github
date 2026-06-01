# ADR-01: API Gateway con Kong Gateway

- **Estado**: Aceptado

---

## Contexto del problema

La plataforma Bazaar está compuesta por múltiples servicios backend independientes (usuarios, productos, carrito, órdenes, notificaciones). Los clientes externos (app mobile en React Native y backoffice web en React) necesitan comunicarse con todos ellos.

Sin un punto de entrada centralizado, cada cliente debería conocer la URL de cada servicio, manejar la autenticación JWT por su cuenta, y los servicios tendrían que implementar individualmente validaciones de seguridad como verificación de tokens, control de cuentas bloqueadas y rate limiting.

Adicionalmente, el enunciado considera como componente esperado del proyecto la existencia de un API Gateway que centralice el enrutamiento, la validación de tokens de sesión y el rate limiting.

---

## Decisión tomada

Se adopta **Kong Gateway** en modo DB-less (configuración declarativa via `kong.yaml`) como único punto de entrada al sistema para los clientes externos.

Kong se despliega como un contenedor Docker en Render y se configura mediante un archivo `kong.yaml.template` que se resuelve en tiempo de arranque inyectando variables de entorno. No requiere base de datos propia.

### Responsabilidades asignadas al gateway

- **Enrutamiento**: mapeo de paths públicos (`/api/*`) a las URLs internas de cada servicio en Render.
- **Autenticación y propagación de identidad**: un plugin personalizado (`jwt-claims-headers`) verifica la firma y expiración del JWT, consulta el servicio de usuarios para obtener el perfil actualizado, valida que la cuenta no esté bloqueada, e inyecta los claims como headers internos (`X-User-Id`, `X-User-Email`, `X-User-Admin`). Los servicios backend confían en estos headers sin necesidad de verificar el token ellos mismos.
- **Rate limiting**: límite global de 100 req/min y 1000 req/hora, con restricciones más estrictas en endpoints sensibles (`/api/auth`: 10 req/min, 50 req/hora).
- **CORS**: configurado globalmente para permitir acceso desde los clientes web y mobile.
- **Token interno**: Kong agrega el header `X-Internal-Token` a todos los requests hacia los servicios, permitiéndoles rechazar tráfico que no provenga del gateway.
- **Orquestación de cancelaciones**: un segundo plugin personalizado (`cancel-order-orchestrator`) coordina el flujo de cancelación de órdenes (restock de productos + reembolso) desde el gateway.

---

## Alternativas descartadas

### NGINX puro

NGINX es la base sobre la que corre Kong, por lo que técnicamente es viable usarlo directamente para enrutamiento. Sin embargo, implementar validación de JWT, consulta al servicio de usuarios y rate limiting en NGINX requiere escribir módulos Lua desde cero o integrar scripts externos. Kong ya provee toda esa infraestructura construida sobre NGINX; elegir NGINX directamente implicaría reproducir Kong con mayor esfuerzo y menor ecosistema disponible.

**Descartado por**: mayor costo de implementación sin ventaja funcional.

### AWS API Gateway

Servicio gestionado de Amazon que resuelve enrutamiento, autenticación y rate limiting sin necesidad de administrar infraestructura. Sin embargo, el plan gratuito se agota rápidamente en entornos de desarrollo con múltiples servicios y pipelines de CI ejecutando tests de integración. Además, la extensión de lógica custom (como la verificación de cuentas bloqueadas) requiere Lambda functions adicionales, lo que aumenta la complejidad operativa y el costo.

**Descartado por**: costo en entorno académico y mayor complejidad para lógica personalizada.

### Kong Konnect (managed)

La versión SaaS de Kong ofrece una consola de administración, métricas y gestión de plugins sin necesidad de auto-hostear. Sin embargo, las features relevantes para este proyecto (plugins personalizados en Lua, configuración declarativa compleja) están disponibles en la versión open source self-hosted. Kong Konnect agrega costo sin aportar ventajas en este contexto.

**Descartado por**: la versión open source self-hosted cubre todos los requerimientos sin costo adicional.

### Traefik

Proxy inverso moderno orientado a entornos de contenedores, con buena integración nativa con Docker. Es adecuado para enrutamiento pero no ofrece un sistema de plugins en Lua, lo que imposibilita implementar la lógica de validación de usuario bloqueado y la propagación de claims JWT sin un servicio auxiliar externo.

**Descartado por**: no permite extender la lógica de autenticación sin servicios adicionales.

---

## Consecuencias esperadas

### Positivas

- Los servicios backend no necesitan implementar validación de JWT individualmente; reciben la identidad del usuario como headers ya verificados.
- El rate limiting y la protección de endpoints sensibles están centralizados y son configurables sin modificar código de los servicios.
- La configuración DB-less con `kong.yaml` hace que el gateway sea completamente reproducible: cualquier miembro del equipo puede levantarlo localmente con `docker compose up`.
- Los plugins en Lua permiten implementar lógica de negocio específica (verificación de bloqueo, orquestación de cancelaciones) directamente en el gateway.

### Negativas

- Kong agrega un hop de red extra en cada request externo. En la práctica el impacto es mínimo dado que los servicios ya están desplegados remotamente en Render.
- Los plugins personalizados en Lua requieren conocimiento de la API de Kong y del runtime de OpenResty.
- El modo DB-less no soporta cambios de configuración en hot reload, cualquier cambio en las rutas o plugins requiere redeploy del gateway.
- La lógica del plugin `jwt-claims-headers` hace una llamada HTTP al servicio de usuarios en cada request autenticado, lo que introduce una dependencia de latencia. Si el servicio de usuarios no está disponible, el gateway rechazaria todos los requests autenticados.
