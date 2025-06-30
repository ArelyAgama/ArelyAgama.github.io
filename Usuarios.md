# UsuariosIS2

[![codecov](https://codecov.io/gh/TP-ClassConnect-G6/UsuariosIS2/graph/badge.svg?token=GH6VVLMKG7)](https://codecov.io/gh/TP-ClassConnect-G6/UsuariosIS2)

API utilizada para el registro y logeo de usuarios y administradores de la plataforma ClassConnect, permitiendo la gestión de usuarios y sus permisos.

## Tecnologías utilizadas
- **Node.js**: Entorno de ejecución para JavaScript en el servidor.
- **Fastify**: Framework web para Node.js, utilizado para construir la API de manera eficiente y rápida.
- **PostgreSQL**: Sistema de gestión de bases de datos relacional utilizado para almacenar la información de usuarios y administradores.
- **Docker**: Plataforma para crear, desplegar y ejecutar aplicaciones en contenedores, facilitando la portabilidad y escalabilidad del microservicio.
- **TAP**: Framework de pruebas para Node.js.


## Estructura del proyecto

```
.
├── Dockerfile      
├── database_init                                 # Contiene los scripts de inicialización de la base de datos
│   └── 01_schema.sql 
├── docker-compose.yml                            # Archivo de configuración de Docker Compose para la base de datos
├── envs                                          # Contiene los archivos de configuración de entorno
│   ├── db_env.test     
│   ├── servers.json        
│   └── users_env_local.test
├── package.json                                  # Archivo de configuración de dependencias y scripts del proyecto
├── src
│   ├── controller                                # Contiene los controladores de la API
│   │   ├── admin_controller.ts                   # Controlador para la gestión de administradores
│   │   ├── auth_controller.ts                    # Controlador para la autenticación de usuarios
│   │   ├── change_password_controller.ts         # Controlador para el cambio de contraseña
│   │   ├── profile_controller.ts                 # Controlador para la gestión de perfiles
│   │   ├── provisional_registration_controller.ts # Controlador para el registro provisional
│   │   └── services
│   │       ├── email_validator.ts                # Servicio para la validación de correos electrónicos
│   │       ├── hash_password_service.ts          # Servicio para el hash de contraseñas
│   │       ├── loggin_attempts_service.ts        # Servicio para el registro de intentos de inicio de sesión
│   │       └── notification_service.ts           # Servicio para el envío de notificaciones
│   ├── index.ts                                  # Punto de entrada de la aplicación  
│   ├── model
│   │   ├── change_password_model.ts              # Modelo para el cambio de contraseña
│   │   ├── profile_model.ts                      # Modelo para la gestión de perfiles
│   │   ├── provisional_registration_model.ts     # Modelo para el registro provisional
│   │   └── user_model.ts                         # Modelo para la gestión de usuarios
│   ├── router                                    # Contiene la configuración de las rutas de la API
│   │   ├── plugin                                
│   │   │   ├── auth.ts                           # Plugin de autenticación
│   │   │   ├── error_handler.ts                  # Plugin de manejo de errores
│   │   │   └── jwt.ts                            # Plugin de autenticación JWT
│   │   ├── router.ts
│   │   └── routes                                # Contiene las rutas de la API  
│   │       ├── adminRouter.ts
│   │       ├── authRouter.ts
│   │       ├── changePasswordRouter.ts
│   │       └── profileRoutes.ts
│   ├── static                                    # Contiene los archivos estáticos utilizados por la aplicación 
│   │   └── default_picture.jpg
│   ├── static_page                               # Contiene las páginas estáticas de la aplicación
│   │   └── change_password.html
│   └── utils
│       ├── facebook.ts
│       └── google.ts
├── test                                          # Contiene los archivos de prueba del proyecto
│   ├── admin.ts
│   ├── change_password.ts
│   ├── index.ts
│   ├── profile.ts
│   ├── static
│   │   ├── test_photo.jpg
│   │   ├── test_photo.png
│   │   └── test_photo.webp
│   └── utils.ts
└── users_api.yml                                 # Archivo de especificación de la API en formato OpenAPI (Swagger)
```

## Decisiones 
- Para la sesion del usuario, se decidio utilizar JWT (JSON Web Tokens) para la autenticación. Ultimamente me arrepentí de esto, ya que JWT no implementa toda la funcionalidad que requería, por lo que tuve que agregrle versionado, y almacenamiento en la base de datos para verificar version.


## Instalación y configuración
Para poder ejecutar este microservicio se recomienda utilizar docker para poder correr tanto el microservicio como la base de datos en un contenedor, o la base de datos en un contenedor, y el microservicio de manera nativa.

Para levantar el contenedor de la base de datos, se tiene que iniciar el docker compose `postgres-reseteable-test`, con las variables de entorno configuradas en el archivo db_end.test.

Para ejecutar el microservicio de manera nativa, tienen que primero ejecutar `npm install`, para instalar todas sus dependencias, y luego, con `npm dev` se inicia su ejecucion en modo de desarrollo. Esto tambien inicia automaticamente el docker de la base de datos.

En su defecto, si desea ejecutar el microservicio dentro de un contenedor, deberá ejecutar el Dockerfile, y la iniciación del docker de la base de datos, depende de usted.

## REST API

Found in [users_api.yml](users_api.yml)