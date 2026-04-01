# Bazaar
## Integrantes

- Emanuel Tello 106157 etello@fi.uba.ar
- Federico Nicolás Pagnotta Lemes 110973 fpagnotta@fi.uba.ar
- Gaspar Notta 111389 Gnotta@fi.uba.ar 
- Santiago Henseler 110732 shenseler@fi.uba.ar
- Rodrigo Martin Rotondo Andrada 109210 rrotondo@fi.uba.ar
- Melina Callebaut 105161 mcallebaut@fi.uba.ar

## Tecnologias elegidas

Para implementar el backend de los distintos microservicios se decidio utilizar java. Se tomo esta decisión ya que java posee el framework [Spring Boot](https://spring.io/projects/spring-boot) el cual facilita y agiliza la creación de api-rest. Ademas es un lenguaje utilizado previamente por todos los miembros del equipo.

Se decidio que las bases de datos del sistema utilicen el sistema de gestión de bases de datos [PostgreSQL](https://www.postgresql.org/) dado que Spring Boot ofrece una integración nativa y sin necesidad de configuración del mismo.

Se eligio, por la recomendación de la catedra, el uso de [React Native](https://reactnative.dev/) para el frontend de la app mobile, y [React](https://es.react.dev/) para el backoffice del sistema.

## Racional de Diseño Front-end

La propuesta de Front-End para Bazaar se fundamenta en una **jerarquía visual rigurosa** que prioriza la confianza del usuario mediante una paleta orgánica de tonos terracota y crema. Se ha implementado una gramática de componentes donde las acciones primarias (Pagar, Publicar, Confirmar) utilizan botones de color sólido para maximizar el *affordance*, mientras que las acciones de exploración (Ver detalle) se relegan a enlaces de texto. Esta diferenciación sistemática reduce la carga cognitiva al estandarizar el comportamiento del usuario: el color siempre indica avance transaccional, mientras que el texto plano invita a la navegación secundaria, manteniendo una estética sofisticada que refuerza la identidad artesanal de la plataforma.

La **arquitectura de navegación** adopta un modelo híbrido que integra un *Tab Bar* inferior para las secciones troncales y un acceso de identidad global en la cabecera. Esta estructura permite que los roles de comprador y vendedor coexistan sin generar fricción, asegurando que el usuario mantenga siempre el sentido de ubicación mediante indicadores de estado activo. Asimismo, se priorizó la **robustez operativa** a través de una gestión de errores proactiva; los formularios incorporan validaciones en tiempo real y estados deshabilitados para los botones de envío. Estas decisiones técnicas no solo optimizan el rendimiento al evitar peticiones inválidas al servidor, sino que consolidan la "confianza extrema" de Bazaar al garantizar que cada interacción sea clara, segura y libre de ambigüedades.

## Productos

Se tomo la decisión de que las categorias de los productos se puedan agregar dinamicamente por parte del administrador a través del endpoint `/category`, para que sea más sencillo agregar nuevas categorias.
