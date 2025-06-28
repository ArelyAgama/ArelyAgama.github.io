# Contenido del proyecto base en TS
## Features

Se necesita de *node*, *java se*, y *android sdk*

* Puede ejecutarse localmente
    * Compatible con todos los modos (*web - android - expo go*)

* Puede ejecutarse en docker
    * Puede conectarse con otros docker a traves de *tuneling*
    * El docker es compatible con el modo expo go, para acceder desde el telefono y la web.

* Toma las varibles de la carpeta env, según el valor de la variable *ENVIRONMENT*, preconfiguradas en los archivos
    * Los archivos de variables respetan el [*estandar*](https://github.com/bkeepers/dotenv/blob/c6e583a/README.md#what-other-env-files-can-i-use).
    * Las actualizaciones de las variables son automaticas en modo local, pero todavía no lo son desde docker.

## Estructura

* El modulo ya está separado en una carpeta, si se cambia el nombre, debe actualizarse en los *Dockerfiles* y en el *docker-compose.yml*
* Los archivos de las variables se encuentran en */envs*
* Los tests se encuentran en */course-service/__test__*
* El modulo de configuracion se encuentra en */course-service/src/config*
* El logger se encuentra en */course-service/src/logger*
* El codigo de lógica en */course-service/src*


## Variables de entorno

* Se capturan a través de su definicion en los archivos: */app.config.ts* y */src/config/frontEnv.ts* desde el front, cuando se ejecuta expo y */src/config/backendConfig.ts* desde el backend
    * Usa las librerías *expo-constants - react-native-dotenv*

## Testing

* El testing se realiza con [*jest*](https://jestjs-io.translate.goog/docs/getting-started?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc)
    * Su converage local está configurado, y automatizado desde containers
    * Es compatible con coverage en línea, como *codecov*
    * Los test se ejecutan con el alias: *npm run unit-test*
    * [Configuración de Jest](https://jestjs.io/docs/configuration)

## Logging 
(En desuso)
* Posee un logger local que funciona en simulador y en modo web
    * En modo web requiere de las *react-tools*, extensión instalable desde consola de desarrollo.
    * Los test se encuentran en */test/*, para que los nuevos test se reconozcan, se deben llamar "* .unit.test.ts".
    * Jest se configura mediante los archivos: */jest.unit.config.ts* y *jest.config.ts*
    * El coverage se guarda en */coverage* y se muestra en una tabla, al correr las pruebas.
    * Depende de las librerias: *expo-file-system - expo-optimize*


## API REST


## Base de datos


### Testing de base de datos

## Ejecucion y pruebas backend

Una vez instaladas las dependencias y ejecutado el backend (Ver README.md), según el entorno, pueden hacerse requests desde una terimnal, según la plataforma:

### Windows
#### POST
```
    Invoke-RestMethod -Uri "http://localhost:5021/courses" -Method Post -Headers @{ "Content-Type" = "application/json" } -Body '{ "user_login": "teacher01", "role": "teacher", "course_name": "Test Course", "description": "Curso de prueba", "date_init": "2025-05-01", "date_end": "2025-08-01", "schedule": [ { "day": "Monday", "time": "09:00" }, { "day": "Wednesday", "time": "10:00" } ], "quota": 30, "academic_level": "Beginner", "required_course_name": [] }'
```

## Pruebas deploy
### POST
```
    $uri = "https://cursosis2-production-94bb.up.railway.app/courses"
    $headers = @{ "Content-Type" = "application/json" }
    $body = '{
    "user_login": "teacher01",
    "role": "teacher",
    "course_name": "Test Course",
    "description": "Curso de prueba",
    "date_init": "2025-05-01",
    "date_end": "2025-08-01",
    "schedule": [
        { "day": "Monday", "time": "09:00" }
    ],
    "quota": 30,
    "academic_level": "Beginner",
    "required_course_name": []
    }'

    Invoke-RestMethod -Uri $uri -Method Post -Headers $headers -Body $body
```

```
    $uri = "https://cursosis2-production-94bb.up.railway.app/courses"
    $headers = @{ "Content-Type" = "application/json" }
    $body = '{
    "user_login": "teacher01",
    "role": "teacher",
    "course_name": "Test Course-2",
    "description": "Curso de prueba",
    "date_init": "2025-05-01",
    "date_end": "2025-08-01",
    "schedule": [
        { "day": "Monday", "time": "08:00" }
    ],
    "quota": 30,
    "academic_level": "Beginner",
    "required_course_name": []
    }'

    Invoke-RestMethod -Uri $uri -Method Post -Headers $headers -Body $body
```

### PATCH
```
    $uri = "https://cursosis2-production-94bb.up.railway.app/courses/ee3d582a-21e1-4339-8c80-52f5418b3be7"
    $headers = @{ "Content-Type" = "application/json" }
    $body = '{
      "user_login": "teacher01",
      "role": "teacher",
      "category": "Art"
    }'

    Invoke-RestMethod -Uri $uri -Method Patch -Headers $headers -Body $body
```
### GET by id

```
    $uri = "https://cursosis2-production-94bb.up.railway.app/courses/ee3d582a-21e1-4339-8c80-52f5418b3be7"

    Invoke-RestMethod -Uri $uri -Method Get 
```

### Filtros de get

#### Filtro por categoria
    https://cursosis2-production-94bb.up.railway.app/courses?category=Art


### Registros
```
    $uri = "https://cursosis2-production-94bb.up.railway.app/courses/ee3d582a-21e1-4339-8c80-52f5418b3be7/registrations"
    $headers = @{ "Content-Type" = "application/json" }
    $body = '{
      "user_login": "student01",
      "role": "student",
      "user_academic_level": "Primary School"
    }'

    Invoke-RestMethod -Uri $uri -Method Post -Headers $headers -Body $body
```

### Linux
```
    curl -X POST http://localhost:5021/courses \
    -H "Content-Type: application/json" \
    -d '{
        "user_login": "teacher01",
        "role": "teacher",
        "course_name": "Test Course",
        "description": "Curso de prueba",
        "date_init": "2025-05-01",
        "date_end": "2025-08-01",
        "schedule": [
        { "day": "Monday", "time": "09:00" },
        { "day": "Wednesday", "time": "10:00" }
        ],
        "quota": 30,
        "academic_level": "Beginner",
        "required_course_name": []
    }'
```

## Ejecucion front end con Expo

La ejecución no puede dockerizarse y accerderse desde front, debido a que debe usarse https, implementando el flag *"--tunnel"* al ejecutar expo, y con este flag no funciona el fetching a la app porque le pega a usando "http", para usar https, deben crearse minimamente certificados autofirmados.

Por lo tanto, para ejecutar la app desde un front, actualmente la única opción es siguiendo los pasos: 

* Configurar ip local para realizar fetching y permitir el acceso de expo go a la API en:

    *  */envs/.env.local*

* Levantar la base de datos desde un docker:

    ```
        docker-compose up postgres-courses-local
    ```

* Instalar dependencias locamente con:

    ```
        npm install
    ```

Para la ejecución se leen los valores de las variables de la carpeta */envs/*
la app carga el archivo correspondiente, especificando ENVIRONMENT, al ejecutar la app, se toma por defecto el entorno 'local', cargando las variables desde */envs/.env.local*

Manualmente para obtener el codigo QR de expo debe ejecutarse:

Back:
```
    npx ts-node-dev src/index.ts
```

Front:
```
    npx expo start
```

Si no se necesita el codigo QR, se puede ejecutar la App con el alias:

```
    npm run dev
```

## Decisiones

### Cursos 
* Los nombres de los cursos son únicos, porque son "un identificador" para los usuarios.
* Al cambiar los cursos requeridos con un patch, se deben pasar la lista completa de cursos requeridos, ya que se sobrescriben.

#### Avisos de cursos
* Los avisos de urgencia para la inscripción de un curso, se indican faltando 3 días o menos para el inicio del curso
    * Si un curso tiene cupos limitados (menor a 10) y, a su vez, quedan menos de 3 dìas para inscribirse, entonces se prioriza el mensaje del tiempo.
    * Si el curso no tiene más cupo, también se indicará un mensaje de aviso.

* El mensaje de "inscripto en curso" tiene prioridad por sobre el resto de mensajes, con lo cual si un mismo curso tiene una cuota menor a 10 y faltan menos de 3 dias para la finalizar la inscripción, pero el usuario se encuentra inscripto en el curso, entonces se indicará el mensaje de que el usuario está inscripto en el curso.

### Inscripciones

* Las inscripciones a un curso, empiezan una semana antes de que comience el curso y finaliza, cuando este comienza.

* Para dar de baja una inscripción:
    * El usuario debe tener permisos de administrador
    * El estudiante puede dar de baja la inscripción, durante el periodo de inscripción.
        * Las inscripciones hechas por los estudiantes durante el periodo de inscripción, se borran de la tabla
            * Las inscripciones inactivas, se consideran desaprobaciones, por eso se borran de la tabla.
    * El profesor del curso puede dar de baja la inscripción luego de que el curso haya finalizado.
    * La inscripción se da de baja si el estudiante aprueba el curso.
    * Las inscripciones se borran al darse de baja.

* Las inscripcines no pueden modificarse


### Aprobaciones

* Para cargar la aprobación de un curso se requiere:
    * Que el estudiante esté inscripto en el curso
    * Que un profesor/administrador cargue la aprobación

    Respuesta
    * Al aprobar un curso, se borra la inscripción del estudiante en ese curso
    * Se devuelve la fecha de la carga, código de aprobación, usuario y nombre de curso
    
* Para dar de baja una aprobación:
    * El usuario debe tener permisos de administrador
    * Las aprobacioens se borran al darse de baja, ya que no tiene sentido, por ejemplo, mantenerlas con un estado "inactivo", pues sólo importan los estudiantes aprobados al haber finalizado el curso.

* Las aprobaciones no pueden modificarse

### Auxiliares

* Para cargar un estudiante como auxiliar:
    * Este no debe estar inscripto en el curso.
    * Un auxiliar, no puede inscribirse en el curso.
    * Se debe indicar una lista de permisos, estos pueden ser:
        * crear tareas (*'create task'*)
        * crear examenes (*'create exam'*)
        * gestionar modulos de un curso (crear/borrar/editar/reactivar) (*'manage module'*)
    
* Para cargar a un docente como auxiliar:
    * Este no debe ser el docente del curso

* No se pueden asignar administradores como auxiliares o docentes de curso

### Tareas / Examenes

* Para cargar una tarea / examen
    * El usuario debe ser el docente del curso
    * Si el usuario es auxliar, debe contar con el permiso necesario

* Los adminsitradores no pueden cargar tareas / examanes

#### Visualización de mis tareas / exámenes (Docente)
* Devuelve las tareas / exámenes de los cursos activos en los que está asignado el docente / auxiliar.
* Se mostrarán primero las tareas/exámanes más recientes, de forma paginada.
* Las entregas de cada tarea/examen se muestran de forma paginada.
* Los posibles filtros para una tarea son:
    * title (se devolverán coincidencias parciales)
    * description (se devolverán coincidencias parciales)
    * due_date
    * published
    * is_active

* Los posibles filtros para una examen son:
    * title (se devolverán coincidencias parciales)
    * description (se devolverán coincidencias parciales)
    * date
    * published
    * is_active

* Un auxiliar también puede visualizar las tareas / exámenes de sus cursos.
* Los docentes / auxiliares pueden descargar las tareas/exámenes en PDF/Docx.

#### Visualización de mis tareas / exámenes (Estudiante)
* Devuelve las tareas / exámenes de los cursos activos en los que esta inscripto el estudiante.
* Se mostrarán primero las tareas/exámanes más recientes, de forma paginada.
* Las entregas de cada tarea/examen se muestran de forma paginada.
* Los posibles filtros para una tarea son:
    * title (se devolverán coincidencias parciales)
    * description (se devolverán coincidencias parciales)
    * due_date
    * published
    * is_active
    * status
        * Que representa el estado de entrega de cada tarea / exámen
            * '*Pending*': no fue entregada, pero está a tiempo.
            * '*Completed*': fue entregada en término.
            * '*Late*': no fue entregada en término.

* Los posibles filtros para una examen son:
    * title (se devolverán coincidencias parciales)
    * description (se devolverán coincidencias parciales)
    * date
    * published
    * is_active
    * status
        * Que representa el estado de entrega de cada tarea / exámen
            * '*Pending*': no fue entregada, pero está a tiempo.
            * '*Completed*': fue entregada en término.
            * '*Late*': no fue entregada en término.

* Un auxiliar también puede visualizar las tareas / exámenes de sus cursos.
* Los estudiantes pueden descargar las tareas/exámenes en PDF/Docx.

### Módulos
* Los módulos se crean en orden ascendente, comenzando desde el '*0*', si no se específica un número de orden (*order-idx*).
* Los números de orden son únicos dentro de los módulos de un curso.
* Los auxiliares pueden gestionar módulos, si cuentan con el permiso *'manage modules'*.
* Los archivos de un módulo se reciben en forma de una URL firmada, accesible durante **1 hora**
* Los campos vacíos pasados a la hora de modificar un módulo, se saltean.
* Las categorías de los archivos son: 
    - Video
    - Document
    - Image
    - Other
* El peso máximo para los archivos es de 50 MB.

### Deudas:
* probar asignar un auxiliar en dos cursos.
* dar permisos para modificar / borrar una tarea examaen para los auxiliares.
* mas pruebas unitarias de cursos?
* filtros por query params para get/approvals
* validaciones faltantes para endpoints de tareas / cursos / submissions?


### Testing secret
Linux
```
SUPABASE_URL=https://zrtkkrujgppkmeyjpndc.supabase.co \
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InpydGtrcnVqZ3Bwa21leWpwbmRjIiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTc0OTM0MTgyOSwiZXhwIjoyMDY0OTE3ODI5fQ.e8nLguQ1JKmjmkQhUWYKgLnw8XLCVMjugp2GhJ8Wb8k \
docker-compose up --build --abort-on-container-exit --exit-code-from courses-service-test postgres-courses-test courses-service-test
```

Windows
```
$env:SUPABASE_URL = "https://zrtkkrujgppkmeyjpndc.supabase.co"
$env:SUPABASE_SERVICE_ROLE_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InpydGtrcnVqZ3Bwa21leWpwbmRjIiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTc0OTM0MTgyOSwiZXhwIjoyMDY0OTE3ODI5fQ.e8nLguQ1JKmjmkQhUWYKgLnw8XLCVMjugp2GhJ8Wb8k"
docker-compose up --build --abort-on-container-exit --exit-code-from courses-service-test postgres-courses-test courses-service-test
```