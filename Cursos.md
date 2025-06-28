
[![codecov](https://codecov.io/gh/TP-ClassConnect-G6/CursosIS2/graph/badge.svg?token=NYL9O6CM3B)](https://codecov.io/gh/TP-ClassConnect-G6/CursosIS2)

# Courses Microservice

## Ejecución
### Ejecucion dockerizada
```
    docker-compose up postgres-courses courses-service
```

### Ejecucion local

Se levanta la base de datos desde un docker:

```
    docker-compose up postgres-courses-local
```

Se instalan las depencias locamente con:
```
    npm install
```
Para la ejecución se leen los valores de las variables de la carpeta */envs/*
la app carga el archivo correspondiente, especificando ENVIRONMENT, entonces para ejecutar la app:


Back:
``` 
    npx ts-node-dev --respawn src/index.ts
```


Para hacer pruebas de uso con el backend, ver su sección en DEVELOP.md

## Testing
```
    docker-compose up --build --abort-on-container-exit --exit-code-from courses-service-test postgres-courses-test courses-service-test
```

## Reseteo
```
    docker-compose down --volumes --remove-orphans
```

