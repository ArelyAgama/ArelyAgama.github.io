# APIGatewatIS2

[![codecov](https://codecov.io/gh/TP-ClassConnect-G6/APIGatewayIS2/graph/badge.svg?token=SCJB1OLVWL)](https://codecov.io/gh/TP-ClassConnect-G6/APIGatewayIS2)

API Gateway para el manejo de solicitudes y respuestas hacia los microservicios de la aplicación ClassConnect.
Esta desarrollado en **Node.js** y utiliza la librería [**fast-gateway**](https://fgw.21no.de/#/) para administrar la creacion del gateway.

Tambien se ocupa de convertir los tokens JWT de autorizacion, en directamente headers de usuario, para que los microservicios no tengan que preocuparse de la decodificacion de los tokens.

## Estructura del proyecto

```
.
├── Dockerfile
├── envs                    # Variables de entorno de ejemplo y testeo
│   ├── deploy_url.env
│   └── test.env
├── eslint.config.mjs       # Configuración de ESLint
├── openapi.yaml            # Esquema OpenAPI para la documentación de la API
├── package-lock.json
├── package.json            # Dependencias y scripts del proyecto
├── src
│   ├── index.ts            # Punto de entrada de la aplicación, y ubicación de lógica de ruteo
│   └── jwt_auth.ts         # Middleware de autenticación JWT
├── test
│   ├── gateway.test.ts     # Pruebas para el gateway
│   ├── jwt_auth.test.ts    # Pruebas para el middleware de autenticación JWT
│   └── utils
│       └── server-control.ts
└── tsconfig.json           # Configuración de TypeScript
```

## Configuración del entorno

### 1. Clonar repositorio

```bash
git clone https://github.com/TP-ClassConnect-G6/APIGatewayIS2.git
cd APIGatewayIS2
```

### 2. Instalar dependencias

```bash
npm install
```

### 3. Configurar variables de entorno

Utilizar las variables de ejemplo provistas en `envs/deploy_url.env` y `envs/test.env` para configurar el entorno de desarrollo y pruebas. Asegurarse de que las URLs de los microservicios sean correctas.


### 4. Ejecutar el servidor
```bash
npm run start
```

## Testing
### 1. Ejecutar pruebas
Previo a configurar el entorno, se pueden ejecutar las pruebas unitarias para verificar que todo funcione correctamente.

```bash
npm run test
```

### 2. Ejecutar testeo de cobertura
Para verificar la cobertura de las pruebas, se puede ejecutar el siguiente comando:
```bash
npm run coverage
```
Esto abrira una pestaña en el navegador con el reporte de cobertura de pruebas.

## Docker

### Construir la imagen de Docker
```bash
docker build -t apigatewayis2 .
```

### Ejecutar el contenedor de Docker
```bash
docker run -d -p 3000:3000 --env-file envs/deploy_url.env --name apigatewayis2 apigatewayis2
``` 

