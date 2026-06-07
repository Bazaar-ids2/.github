# ADR-03: Despliegue en la Nube con Render

- **Estado**: Aceptado

---

## Contexto

Los servicios backend de Bazaar necesitan desplegarse en un entorno de nube accesible públicamente. El enunciado requiere que la plataforma elegida cuente con un plan gratuito o de bajo costo, que el entorno de producción sea reproducible y que el despliegue ocurra de forma automatizada desde el pipeline de CI/CD.

El sistema está compuesto por múltiples servicios independientes (User, Products, Orders, Cart, Notifications, API Gateway), cada uno con su propio repositorio y ciclo de despliegue.

---

## Decisión

Se adopta **Render** como plataforma de despliegue (PaaS) para todos los servicios backend y el API Gateway.

Cada servicio se despliega como un **Web Service** en Render a partir de su `Dockerfile`. El despliegue se activa automáticamente ante cada push a la rama principal (`main`), sin intervención manual. Las variables de entorno y secretos se gestionan desde el panel de Render, sin exponerlos en el código fuente.

El pipeline de CI corre con **GitHub Actions** antes del despliegue: ejecuta los tests y bloquea el merge si fallan. Una vez que el PR se aprueba y se mergea a `main`, Render detecta el cambio y despliega automáticamente.

---

## Alternativas evaluadas

### Railway

PaaS similar a Render con buena experiencia de desarrollo y soporte para Docker. El plan gratuito de Railway tiene un límite de horas de ejecución mensual que se agota con múltiples servicios activos simultáneamente. Para un sistema con 6+ servicios corriendo en paralelo, el costo supera el plan gratuito rápidamente.

**Descartado**: límite de horas del plan gratuito insuficiente para la cantidad de servicios del proyecto.

### Fly.io

Plataforma orientada a contenedores con buena performance y distribución global. Requiere configurar archivos `fly.toml` por servicio y manejar el CLI propio de Fly para los deploys. La curva de configuración inicial es mayor que Render y el plan gratuito tiene restricciones de memoria por instancia que pueden ser limitantes para servicios Java con Spring Boot.

**Descartado**: mayor complejidad de configuración y restricciones de memoria incompatibles con servicios Spring Boot.

### AWS ECS / Google Cloud Run

Servicios gestionados de contenedores en clouds mayores. Ofrecen mayor control y escalabilidad pero requieren configuración de redes, roles IAM, registries de imágenes y pipelines de despliegue más complejos. El free tier de ambos tiene límites que se superan fácilmente en un entorno de desarrollo activo con múltiples servicios.

**Descartado**: complejidad operativa y costo fuera del alcance de un proyecto académico.

### Heroku

PaaS histórico con excelente experiencia de desarrollo. Eliminó su plan gratuito en 2022, lo que lo hace inviable como opción de bajo costo para este proyecto.

**Descartado**: no tiene plan gratuito vigente.

---

## Consecuencias

- Cada nuevo servicio que se agregue al sistema debe tener un `Dockerfile` funcional y registrarse como Web Service en Render con sus variables de entorno configuradas.
- El plan gratuito de Render tiene instancias que entran en reposo tras 15 minutos de inactividad — el primer request tras el reposo puede tardar varios segundos. Esto es aceptable en un entorno académico pero debe tenerse en cuenta al hacer demos.
- El despliegue automático desde `main` implica que cualquier merge a esa rama dispara un deploy en producción. El pipeline de GitHub Actions actúa como barrera: si los tests fallan, el merge no debería proceder.
- Los secretos y credenciales se gestionan exclusivamente como variables de entorno en el panel de Render, nunca en el código fuente ni en el historial de Git.