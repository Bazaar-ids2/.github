# ADR-02: Stack Tecnológico de Servicios Backend

- **Estado**: Aceptado

---

## Contexto

La plataforma Bazaar requiere múltiples servicios backend independientes. El enunciado exige que al menos dos microservicios estén implementados en lenguajes distintos, así como el uso de una base de datos no relacional. El equipo necesitaba elegir un stack que permitiera desarrollar con velocidad, contara con soporte para las integraciones requeridas (PostgreSQL, MongoDB, RabbitMQ, Redis, Swagger, JWT) y fuera conocido por los integrantes.

---

## Decisión

Se adoptó **Java con Spring Boot** como lenguaje principal para los servicios de Products, Orders y Cart. Adicionalmente, se eligió **Python con FastAPI** para el servicio de usuarios (User) y **Node.js con NestJS** para el servicio de notificaciones (Notifications). También se determinó el uso de PostgreSQL y MongoDB como motores de bases de datos, con RabbitMQ y Redis para la infraestructura de comunicación de microservicios.

**Java + Spring Boot** se eligió como lenguaje principal para Products, Orders y Cart porque:
- La mayoria del equipo tenía experiencia previa con el lenguaje
- Spring Boot ofrece integración nativa con PostgreSQL via Spring Data JPA sin configuración extra
- La dependencia `springdoc-openapi` autogenera el Swagger desde las anotaciones del código
- Es un stack maduro y ampliamente documentado para APIs REST

**Python + FastAPI** se eligió para el servicio de usuarios porque:
- FastAPI autogenera el Swagger y la validación de requests desde los tipos de Pydantic sin configuración adicional
- Es el servicio menos acoplado a los demás, lo que lo hace el candidato ideal para introducir un segundo lenguaje sin riesgo
- Permite al equipo explorar un paradigma diferente (tipado dinámico, async nativo) en un contexto controlado

**Node.js + NestJS** se eligió para el servicio de notificaciones porque:
- Ofrece facilidades excelentes para operaciones I/O intensivas (como el manejo de notificaciones y eventos asíncronos).
- NestJS impone una arquitectura similar a Spring Boot, lo que hace la transición cómoda para el equipo.

**Persistencia, Caché y Mensajería**:
- **PostgreSQL**: Se eligió como base de datos relacional para servicios estructurales aprovechando la natividad en Spring Boot.
- **MongoDB**: Se utiliza en Cart y Notifications al requerirse una base de datos no relacional ágil orientada a documentos.
- **RabbitMQ y Redis (Upstash)**: Se implementan en Notifications para procesar avisos encolados de Products y Orders (RabbitMQ) y evitar enviar alertas duplicadas empleando Redis como capa de estado/caché.

---

## Alternativas evaluadas

### Go

Se consideró Go como segunda opción por su popularidad en el ecosistema de microservicios y su performance en concurrencia. Sin embargo, pocos integrantes del equipo tenian experiencia práctica con el lenguaje o con su ecosistema (frameworks, ORMs, librerías de JWT). Incorporarlo hubiera implicado una curva de aprendizaje significativa en un proyecto con fechas de entrega definidas.

**Descartado**: falta de experiencia del equipo y riesgo de demoras en los checkpoints.

### Node.js + Express

Node.js fue considerado inicialmente en su variante con Express por su popularidad y la facilidad de encontrar librerías para cualquier integración. Sin embargo, el equipo tenía mayor experiencia con Java y Python, y Express requiere más configuración manual para generar Swagger y estructurar una API REST de forma consistente comparado con frameworks con opinión como Spring Boot o FastAPI.

**Resuelto**: Se descartó la versión plana de *Express* en favor de **NestJS**, que mediante TypeScript aporta tipado fuerte, decoradores y una estructura orientada a inyección de dependencias similar a los demás frameworks elegidos.

---

## Consecuencias

- El ecosistema del backend queda formalmente compuesto por **tres lenguajes**: Java, Python y Node.js. Los nuevos servicios deben ajustarse a uno de estos esquemas.
- El equipo debe mantener conocimiento distribuido sobre estos frameworks, así como manejar bases de datos heterogéneas (SQL y NoSQL).
- Los pipelines de CI deben contar con pasos específicos para cada lenguaje (Maven/Gradle para Java, pip para Python, npm para Node.js).
- Las dependencias de infraestructura aumentan localmente y en producción con la inclusión de MongoDB, RabbitMQ y Redis para asegurar el Correcto flujo asíncrono de interacciones de mensajería (Notifications).
- La generación de Swagger es automática en todos los stacks mediante anotaciones y los decoradores correspondientes.