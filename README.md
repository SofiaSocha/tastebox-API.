# tastebox-API.
API REST de TasteBox (Node.js, Express y MySQL): suscripciones, usuarios, recetas, reseñas y recetas guardadas. Proyecto académico, segundo corte.
# TasteBox API

API REST del backend de **TasteBox**, una plataforma de recetas con suscripciones, reseñas y recetas guardadas. Proyecto académico del segundo corte (modelo de datos, API y documentación; la interfaz web corresponde al corte 3).

## Tecnologías

- Node.js y Express
- MySQL 8 (paquete `mysql2`)
- Documentación OpenAPI 3.0
- Pruebas con Postman

## Recursos de la API

| Recurso | Ruta base | Descripción |
|---|---|---|
| Suscripciones | `/subscriptions` | Planes disponibles (Gratuito, Premium) |
| Usuarios | `/users` | Registro, consulta, actualización, eliminación e inicio de sesión (`POST /users/login`) |
| Recetas | `/recipes` | Catálogo de recetas |
| Reseñas | `/reviews` | Reseñas de los usuarios; consulta por receta en `/reviews/recipe/{recipeId}` |
| Recetas guardadas | `/saved-recipes` | Relación usuario–receta; consulta por usuario en `/saved-recipes/user/{userId}` |

En total son 26 endpoints, descritos con parámetros, cuerpos y respuestas de éxito y de error en [`Node_MySQL/openapi.yaml`](Node_MySQL/openapi.yaml).

## Estructura del repositorio

```
tastebox-api/
├── README.md
├── .gitignore
└── Node_MySQL/
    ├── server.js                          Punto de entrada y registro de rutas
    ├── package.json
    ├── package-lock.json
    ├── BaseDef.sql                        Script SQL de la base de datos
    ├── openapi.yaml                       Especificación OpenAPI
    ├── TasteBox_postman_collection.json   Colección de Postman
    ├── routes/                            Definición de rutas por recurso
    ├── dao/                               Validación de datos y acceso a la base de datos
    ├── services/                          Conexión a MySQL
    ├── utils/                             Utilidades compartidas
    └── env/                               Configuración de la base de datos
```

Todos los comandos de esta guía se ejecutan dentro de la carpeta `Node_MySQL`.

## Requisitos

- [Node.js](https://nodejs.org/) 18 o superior
- MySQL 8 en ejecución
- Postman (opcional, para las pruebas)

## Instalación y ejecución

1. Clonar el repositorio e instalar dependencias:

   ```bash
   git clone https://github.com/SofiaSocha/tastebox-api.git
   cd tastebox-api/Node_MySQL
   npm install
   ```

2. Crear la base de datos: abrir `Node_MySQL/BaseDef.sql` en MySQL Workbench y ejecutarlo completo. El script crea el esquema `tasteBox`, sus tablas y los planes Gratuito y Premium.

3. Configurar la conexión: crear el archivo `env/mysqlConfig.js` con el siguiente contenido, usando el usuario y la contraseña de MySQL local:

   ```js
   const config = {
       host: 'localhost',
       user: 'root',
       password: ' ',
       database: 'tasteBox'
   };

   module.exports = config;
   ```

4. Iniciar la API:

   ```bash
   npm start
   ```

   La API queda disponible en `http://localhost:5000`.

## Documentación

La especificación completa está en `Node_MySQL/openapi.yaml`. Para verla de forma interactiva, abrir <https://editor.swagger.io> y pegar el contenido del archivo.

## Pruebas

El archivo `Node_MySQL/TasteBox_postman_collection.json` es la colección de Postman, con 53 peticiones y 83 verificaciones; cada endpoint tiene al menos un caso de éxito y uno de error, salvo la creación y eliminación de planes, que solo cuentan con caso de error.

1. Importar la colección en Postman.
2. Con la base de datos recién creada y la API en ejecución, abrir la colección y usar **Run** para ejecutarla en orden.

Las peticiones dependen unas de otras (por ejemplo, la receta se crea antes de la reseña), por lo que conviene ejecutarlas en el orden de las carpetas.

## Alcance y limitaciones de este corte

- La contraseña se almacena en texto plano, como lo establece el enunciado.
- El inicio de sesión verifica las credenciales, pero no emite tokens ni protege el resto de los endpoints.
- Los listados no tienen paginación ni filtros.
- Quedan fuera de este corte la caja semanal de ingredientes y la actividad de cocina.

## Equipo

- Sofía Socha
- Mariana Javier
- Valentina Cruz
