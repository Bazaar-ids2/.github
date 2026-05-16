# Bazaar

## Integrantes

- Emanuel Tello 106157 etello@fi.uba.ar
- Federico Nicolás Pagnotta Lemes 110973 fpagnotta@fi.uba.ar
- Gaspar Notta 111389 Gnotta@fi.uba.ar
- Santiago Henseler 110732 shenseler@fi.uba.ar
- Rodrigo Martin Rotondo Andrada 109210 rrotondo@fi.uba.ar
- Melina Callebaut 105161 mcallebaut@fi.uba.ar

## Cobertura de los repositorios

Products [![codecov](https://codecov.io/gh/Bazaar-ids2/Products/graph/badge.svg?token=EOQF2FGJKS)](https://codecov.io/gh/Bazaar-ids2/Products)

Cart [![codecov](https://codecov.io/gh/Bazaar-ids2/Cart/graph/badge.svg?token=E1NM8DQVVG)](https://codecov.io/gh/Bazaar-ids2/Cart)

User [![codecov](https://codecov.io/gh/Bazaar-ids2/User/graph/badge.svg?token=B24C5X5TIM)](https://codecov.io/gh/Bazaar-ids2/User)

## Diseño de arquitectura

![Diagrama de arquitectura](../img/diseño_arquitectura.jpg)

## Tecnologias elegidas

Para implementar el backend de los microservicios de Products, Orders y Cart se decidio utilizar Java. Se tomo esta decisión ya que java posee el framework [Spring Boot](https://spring.io/projects/spring-boot) el cual facilita y agiliza la creación de api-rest. Ademas es un lenguaje utilizado previamente por todos los miembros del equipo.
Debido a que se solicitaba el uso de un segundo lenguaje para los microservicios se tomo la decision de implementar el microservicio de User en Python, ya que con cuenta con el framework de [FastApi] (https://fastapi.tiangolo.com/) y es muy sencillo de utilizar.

Se decidio que las bases de datos del sistema utilicen el sistema de gestión de bases de datos [PostgreSQL](https://www.postgresql.org/) dado que Spring Boot ofrece una integración nativa y sin necesidad de configuración del mismo. Como tambien se pedia la utilización de una base de datos no relacional se utilizo [MongoDB](https://www.mongodb.com/) para el microservicio Cart.

Se eligio, por la recomendación de la catedra, el uso de [React Native](https://reactnative.dev/) para el frontend de la app mobile, y [React](https://es.react.dev/) para el backoffice del sistema.

## Racional de Diseño Front-end

La propuesta de Front-End para Bazaar se fundamenta en una **jerarquía visual rigurosa** que prioriza la confianza del usuario mediante una paleta orgánica de tonos terracota y crema. Se ha implementado una gramática de componentes donde las acciones primarias (Pagar, Publicar, Confirmar) utilizan botones de color sólido para maximizar el _affordance_, mientras que las acciones de exploración (Ver detalle) se relegan a enlaces de texto. Esta diferenciación sistemática reduce la carga cognitiva al estandarizar el comportamiento del usuario: el color siempre indica avance transaccional, mientras que el texto plano invita a la navegación secundaria, manteniendo una estética sofisticada que refuerza la identidad artesanal de la plataforma.

La **arquitectura de navegación** adopta un modelo híbrido que integra un _Tab Bar_ inferior para las secciones troncales y un acceso de identidad global en la cabecera. Esta estructura permite que los roles de comprador y vendedor coexistan sin generar fricción, asegurando que el usuario mantenga siempre el sentido de ubicación mediante indicadores de estado activo. Asimismo, se priorizó la **robustez operativa** a través de una gestión de errores proactiva; los formularios incorporan validaciones en tiempo real y estados deshabilitados para los botones de envío. Estas decisiones técnicas no solo optimizan el rendimiento al evitar peticiones inválidas al servidor, sino que consolidan la "confianza extrema" de Bazaar al garantizar que cada interacción sea clara, segura y libre de ambigüedades.

## Productos

- Se tomo la decisión de que las categorias de los productos se puedan agregar dinamicamente por parte del administrador a través del endpoint `/category`, para que sea más sencillo agregar nuevas categorias.
- La API rest se va a implementar en `JAVA` con el framework de springBoot y utilizando como servicio de base de datos `Postgress`
- Se determino que las imagenes van a ser cargadas mediante `MultipartFile` (herramienta proporcionada por springBoot) a través del endpoint `/products/images`. Dentro del product NO se va a almacenar una referencia a donde esta ubicada la imagen, simplemente se va a guardar el nombre de la misma. Para poder obtener una imagen a partir del nombre de la misma existe el endpoint `/products/images/{filename}`.
