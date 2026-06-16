# Bazaar

## Integrantes

- Emanuel Tello 106157 etello@fi.uba.ar
- Federico Nicolás Pagnotta Lemes 110973 fpagnotta@fi.uba.ar
- Gaspar Notta 111389 Gnotta@fi.uba.ar
- Santiago Henseler 110732 shenseler@fi.uba.ar
- Rodrigo Martin Rotondo Andrada 109210 rrotondo@fi.uba.ar
- Melina Callebaut 105161 mcallebaut@fi.uba.ar

## Cobertura de los repositorios

#### Products [![codecov](https://codecov.io/gh/Bazaar-ids2/Products/graph/badge.svg?token=EOQF2FGJKS)](https://codecov.io/gh/Bazaar-ids2/Products)

#### Orders [![codecov](https://codecov.io/github/bazaar-ids2/orders/graph/badge.svg?token=5GHQR59YP7)](https://codecov.io/github/bazaar-ids2/orders)

#### Cart [![codecov](https://codecov.io/gh/Bazaar-ids2/Cart/graph/badge.svg?token=E1NM8DQVVG)](https://codecov.io/gh/Bazaar-ids2/Cart)

#### User [![codecov](https://codecov.io/gh/Bazaar-ids2/User/graph/badge.svg?token=B24C5X5TIM)](https://codecov.io/gh/Bazaar-ids2/User)

#### Notifications [![codecov](https://codecov.io/gh/Bazaar-ids2/Notifications/branch/main/graph/badge.svg?token=F4C21LAKV5)](https://codecov.io/gh/Bazaar-ids2/Notifications)


## API Gateway

El sistema utiliza **Kong Gateway** como punto único de entrada para  los clientes externos (app mobile y backoffice). Centraliza el enrutamiento, la validación de JWT, el control de cuentas bloqueadas y el rate limiting. La decisión está documentada en [ADR-001](../adrs/adr_gateway.md).


## Diagrama de contexto

![Diagrama de contexto](../img/diagrama_de_contexto.png)


## Diseño de arquitectura

![Diagrama de arquitectura](../img/diseño_arquitectura.jpg)

## Tecnologias elegidas

Para implementar el backend de los microservicios de Products, Orders y Cart se decidio utilizar Java. Se tomo esta decisión ya que java posee el framework [Spring Boot](https://spring.io/projects/spring-boot) el cual facilita y agiliza la creación de api-rest. Ademas es un lenguaje utilizado previamente por todos los miembros del equipo.
Debido a que se solicitaba el uso de un segundo lenguaje para los microservicios se tomo la decision de implementar el microservicio de User en Python, ya que con cuenta con el framework de [FastApi] (https://fastapi.tiangolo.com/) y es muy sencillo de utilizar. Por último, para el microservicio de Notifications, se optó por implementarlo con Node.js, utilizando el framework NestJS.

Se decidio que las bases de datos del sistema utilicen el sistema de gestión de bases de datos [PostgreSQL](https://www.postgresql.org/) dado que Spring Boot ofrece una integración nativa y sin necesidad de configuración del mismo. Como tambien se pedia la utilización de una base de datos no relacional se utilizo [MongoDB](https://www.mongodb.com/) para el microservicio Cart y Notifications.

Se eligio, por la recomendación de la catedra, el uso de [React Native](https://reactnative.dev/) para el frontend de la app mobile, y [React](https://es.react.dev/) para el backoffice del sistema.

Por último, en Notifications se utiliza RabbitMQ (proveedor CloudAMQP) y Redis (Upstash), para consumir notificaciones provenientes de Products y Orders, y evitar el envío de notificaciones duplicadas, respectivamente.

> **Nota:** El racional completo detrás de la elección de lenguajes, frameworks, bases de datos y herramientas de mensajería para el backend se encuentra documentado en detalle en el [ADR-02: Stack Tecnológico de Servicios Backend](../adrs/adr_tecnologias.md).

## Despliegue en la Nube
El sistema utiliza **Render** como plataforma (PaaS) para desplegar todos los microservicios y el API Gateway de manera automatizada junto a los pipelines de CI/CD. La decisión y estrategia están documentadas en el [ADR-03: Despliegue en la Nube con Render](../adrs/adr_03_deploy.md).

## Consistencia Distribuida
Para manejar de forma tolerante a fallos la comunicación entre servicios (checkout, reserva de stock y MercadoPago) previniendo inconsistencias como pagos exitosos con carritos vacíos o viceversa, se documentó un esquema específico disponible en el [ADR-04: Consistencia Distribuida en Checkout y Cancelación de Órdenes](../adrs/adr_04_consistencia.md).

## Racional de Diseño Front-end

La propuesta de Front-End para Bazaar se fundamenta en una **jerarquía visual rigurosa** que prioriza la confianza del usuario mediante una paleta orgánica de tonos terracota y crema. Se ha implementado una gramática de componentes donde las acciones primarias (Pagar, Publicar, Confirmar) utilizan botones de color sólido para maximizar el _affordance_, mientras que las acciones de exploración (Ver detalle) se relegan a enlaces de texto. Esta diferenciación sistemática reduce la carga cognitiva al estandarizar el comportamiento del usuario: el color siempre indica avance transaccional, mientras que el texto plano invita a la navegación secundaria, manteniendo una estética sofisticada que refuerza la identidad artesanal de la plataforma.

La **arquitectura de navegación** adopta un modelo híbrido que integra un _Tab Bar_ inferior para las secciones troncales y un acceso de identidad global en la cabecera. Esta estructura permite que los roles de comprador y vendedor coexistan sin generar fricción, asegurando que el usuario mantenga siempre el sentido de ubicación mediante indicadores de estado activo. Asimismo, se priorizó la **robustez operativa** a través de una gestión de errores proactiva; los formularios incorporan validaciones en tiempo real y estados deshabilitados para los botones de envío. Estas decisiones técnicas no solo optimizan el rendimiento al evitar peticiones inválidas al servidor, sino que consolidan la "confianza extrema" de Bazaar al garantizar que cada interacción sea clara, segura y libre de ambigüedades.

# Productos

- Se tomo la decisión de que las categorias de los productos se puedan agregar dinamicamente por parte del administrador a través del endpoint `/category`, para que sea más sencillo agregar nuevas categorias.
- La API rest se va a implementar en `JAVA` con el framework de springBoot y utilizando como servicio de base de datos `Postgress`
- Se determino que las imagenes van a ser cargadas mediante `MultipartFile` (herramienta proporcionada por springBoot) a través del endpoint `/products/images` y luego almacenadas en bucket de  [SupaBase](https://supabase.com/) (Recomendación de la catedra). Dentro de la base de datos de products NO se va a almacenar la imagen, se guarda dentro de cada product una lista con las url de donde estan alojadas.
- Se integro el sistema de monitore [Prometheus](https://docs.spring.io/spring-boot/api/rest/actuator/prometheus.html), a traves del endpoint `/actuator/prometheus`

## Pre requisitos

Para poder utilizar el proyecto es necesario tener instalado lo siguiente:
- [OpenJDK](https://www.oracle.com/java/technologies/javase/jdk21-archive-downloads.html) >= a 21.0.2
- [Docker Compose](https://docs.docker.com/compose/install/) >= v2.29.7
- [Docker](https://www.docker.com/get-started/) >= 24.0.7

## Comandos

- Comando para construir el servicio:
    `docker compose up --build`

- Comando para testing:
  una vez construido el servicio ejecutar `docker exec -it products_backend_1 mvn test`

## Tests

#### Covertura: [![codecov](https://codecov.io/gh/Bazaar-ids2/Products/graph/badge.svg?token=EOQF2FGJKS)](https://codecov.io/gh/Bazaar-ids2/Products)

#### Carga: Para los test de carga utilizamos una funcionalidad de postman 

- En el primer intento se utilizaron 10 usuario concurrentes sin ningun pico de carga y el sistema respondio sin ningun error
<div align="center">
    <img width="70%" src="../img/load_test_products1.png">
</div> 

- En el segundo intento se utilizaron 1000 usuarios concurrentes con un pico de carga largo en el tiempo. El sistema respondio bien, pero en mitad del pico de carga tuvo unos pocos errores
<div align="center">
    <img width="70%" src="../img/load_test_products2.png">
</div> 

- En el ultimo intento se utilizaron 10000 usuarios concurrentes con un pico de carga igual que el caso anterior. En medio del pico de carga el sistema colapso por un pequeño momento y luego se pudo recuperar.
<div align="center">
    <img width="70%" src="../img/load_test_products3.png">
</div> 



