# IaIS2

[![codecov](https://codecov.io/gh/TP-ClassConnect-G6/IaIS2/graph/badge.svg?token=G0TLGE7H18)](https://codecov.io/gh/TP-ClassConnect-G6/IaIS2)

API RESTful para la interaccion con servicios de IA, ya sea para inferencias generales, como para el uso del chat de IA.

---

## Tegnologías utilizadas

- Node.js
- Fastify: Framework web para Node.js, utilizado para crear la API RESTful.
- MongoDB: Base de datos NoSQL utilizada para almacenar los datos de las interacciones con la IA.
- Docker: Plataforma de contenedores utilizada para facilitar la ejecución del microservicio
- TAP: Framework de pruebas utilizado para realizar pruebas unitarias y de integración en el microservicio.
- OpenRouter API: Proveedor de servicios de IA utilizado para realizar inferencias y chat con IA.

---

## Estructura del proyecto

```
IaIS2
├── Dockerfile
├── database_init                           # Contiene los scripts de inicialización de la base de datos
│   └── 01_schema.sql       
├── docker-compose.yml                      # Archivo de configuración de Docker Compose par la base de datos
├── docs
│   └── classconnect.md                     # Documentación del la aplicación, usado como informacion para la el chat con la IA
├── envs                                    # Contiene los archivos de variables de entorno
│   ├── db_env.test
│   ├── secret.env
│   └── users_env_local.test    
├── ia_api.yml                              # Documentacion de la API utilizando OpenAPI
├── package.json                            # Archivo de configuración de npm, contiene las dependencias y scripts del proyecto

├── src
│   ├── chat_assistant                      # Carpeta que contiene los archivos relacionados con el chat de IA 
│   │   ├── chat_assistant.controller.ts    # Obtiene los parametros de la peticion y llama al servicio correspondiente
│   │   ├── chat_assistant.model.ts         # Interactua con la base de datos
│   │   ├── chat_assistant.router.ts        # Define las rutas del chat de IA
│   │   ├── chat_assistant.service.ts       # Contiene la logica de negocio del chat de IA
│   │   └── model
│   │       └── chat.models.ts              # Define los modelos de datos utilizados en el chat de IA
│   ├── custom_inference
│   │   └── custom_inference.ts             # Servicio que permite realizar inferencias personalizadas con IA
│   ├── index.ts                            # Punto de entrada del microservicio, inicializa el servidor.
│   ├── router
│   │   ├── plugin
│   │   │   └── error_handler.ts            # Maneja los errores de la API     
│   │   └── router.ts                       # Define las rutas generales de la API
│   └── services
│       └── ia.service.ts                   # Servicio que interactua con la API de OpenRouter para realizar inferencias y chat con IA
└── test                                    # Carpeta que contiene los archivos de pruebas del microservicio
    ├── chat_assistant.test.ts
    ├── ia_service.test.ts
    └── ping.test.ts

```

## Desiciones tomadas
Como servicio de IA decidí utilizar OpenRouter, por las siguientes razones:
- Tiene un tier gratuito que permite realizar pruebas y desarrollos sin costo.
- Permite interactuar con diferentes modelos de IA, lo que permite probar distintos modelos, sin tener que cambiar el código del microservicio.
- Tiene una API RESTful que permite interactuar con los modelos de IA de manera sencilla.
- La documentación que posee es muy clara y sencilla de entender, lo que facilita su uso y comprensión.


## Instalación y configuración
Para poder ejecutar este microservicio se recomienda utilizar docker para poder correr tanto el microservicio como la base de datos en un contenedor, o la base de datos en un contenedor, y el microservicio de manera nativa.

Para levantar el contenedor de la base de datos, se tiene que iniciar el docker compose `mongodb-test`, con las variables de entorno configuradas en el archivo db_env.test.

Para ejecutar el microservicio de manera nativa, tienen que primero ejecutar `npm install`, para instalar todas sus dependencias, y luego, con `npm dev` se inicia su ejecucion en modo de desarrollo. Esto tambien inicia automaticamente el docker de la base de datos.

En su defecto, si desea ejecutar el microservicio dentro de un contenedor, deberá ejecutar el Dockerfile, y la iniciación del docker de la base de datos, depende de usted.