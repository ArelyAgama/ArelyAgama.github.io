
[![codecov](https://codecov.io/gh/TP-ClassConnect-G6/CursosIS2/graph/badge.svg?token=NYL9O6CM3B)](https://codecov.io/gh/TP-ClassConnect-G6/CursosIS2)

# Microservicio de cursos


## Definición de arquitectura

La arquitectura general es de microservicios, estos son:

* [Usuarios](https://github.com/TP-ClassConnect-G6/UsuariosIS2/blob/main/README.md)
* [Cursos](https://github.com/TP-ClassConnect-G6/CursosIS2/blob/main/README.md)
* [Foros](https://github.com/TP-ClassConnect-G6/ForosIS2/blob/main/README.md)
* [Notificaciones](https://github.com/TP-ClassConnect-G6/NotificacionesIS2/blob/main/readme.md)
* [API Gateway](https://github.com/TP-ClassConnect-G6/APIGatewayIS2)
* [Back Office](https://github.com/TP-ClassConnect-G6/BackOfficeIS2/blob/main/README.md)


### Diagrama

![Imagen de arquitectura](./docs/images/arquitectura.png)

## Implementación

Los servicios se encuentran subidos en la plataforma de [*Railway*](https://docs.railway.com/reference/deployments)


## Tecnologías

El microservicio de gestión de cursos, está implementado en *Typescript*, usando como motor de base de datos, *PostgreSQL*.

Otras tecnologías usadas en el proyecto son:

* [**jest**](https://jestjs-io.translate.goog/docs/getting-started?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc) (para testing)
* [**EsLint**](https://www.npmjs.com/package/eslint) (linter para typescript)
* [**Pg**]() (para gestionar la conexión con la base de datos)
* [**Dotenv**](https://www.npmjs.com/package/dotenv) (para gestionar las variables configurables)

* [**Multer**](https://www.npmjs.com/package/multer) (para gestionar el manejo de archivos en algunos endpoints)
* [**Pdf-kit**](https://www.npmjs.com/package/pdfkit) (componente para generar **Pdfs**)
* [**Docx**](https://www.npmjs.com/package/docx) (componente para generar archivos de Word **.docx**)
* [**Nodemailer**](https://www.npmjs.com/package/nodemailer) (para notificaciones)
* [**Supabase**](https://supabase.com/docs/reference/javascript/typescript-support) (para gestionar el storage externo en algunos endpoints)



### Elección de tecnologías

Se eligió usar el storage *Supabase* frente al de *Firebase*, porque si bien el servicio gratuito de Supabase es inferior al de firebase, no requiere una tarjeta.
La cantidad de clientes gratuitos en Supabase de limitada, por lo tanto, se optó por usar un único cliente administrador, además porque:

* La gestión de las claves privadas se hace únicamente desde el backend, evitando usar claves cliente desde el front.
* El costo de generar clientes usuarios desde el front es mayor, y la cantidad de estos es limitada en la capa gratuita.

En contra partida, los archivos se deben pasar por medio de la comunicación (en binario, en vez de por url), pero como la conexión con railway está cifrada por *https*, estos tienen protección, además se limitó el tamaño a **50MB** (para evitar problemas de conexión y por limitación de la capa gratuita de Supabase).
Al usar un usuario adminsitrador para gestionar el storage, en las respuestas se devuelven url protegidas, accesibles para usuarios por una hora.

## Estructura de código

* El módulo ya está separado en una carpeta
* Los archivos de las variables se encuentran en */envs*
* Los tests se encuentran en */course-service/__test__*
* El módulo de configuración se encuentra en */course-service/src/config*
* El logger se encuentra en */course-service/src/utils/logger*
* El código de lógica en */course-service/src*
* Las herramientas comúnes, como el middlware de errores, el notificador, el validador, el gestor del cliente de *Supabase*, se encuentran en */course-service/src/utils/*

## Instalación y configuración

### Configuración
La configuración se realiza a través de unos archivos guardados en (/envs)

* Toma las varibles de la carpeta /envs, según el valor de la variable *ENVIRONMENT* (*local - test - production*), preconfiguradas en los archivos
    * Los archivos de variables respetan el [*estándar*](https://github.com/bkeepers/dotenv/blob/c6e583a/README.md#what-other-env-files-can-i-use).

* Existen variables secretas, que no se encuentran en los archivos, estas son:
    * *SUPABASE_URL*
    * *SUPABASE_SERVICE_ROLE_KEY*
    * *OPENROUTER_API_KEY*



### Ejecución

### Dependencias

La ejecución tanto de los tests, como de forma local, está dockerizada con distintos servicios definidos en el docker-compose.yml, que a su vez, usan distintos *Dockerfiles* según el entorno,
para correr los servicios, se debe:

* Tener instalados los paquetes *docker, docker-compose*

En ambientes Linux pueden instalarse con:

``` 
    sudo apt install docker docker-compose -y
```

En Windows se debe instalar [Docker desktop](https://www.docker.com/products/docker-desktop/)

### Ejecución local

Una vez instaldas las dependencias, para ejecutar localmente el servicio de backend de cursos se debe:

* [Configurar las variables secretas](./docs/DEVELOP.md#secret-testing)
* Levantar la base de datos local con:

```
    docker-compose up postgres-courses-local
```

* Instalar las dependencias locamente desde **./course-service** con:

```
    npm install
```

Para la ejecución se leen los valores de las variables de la carpeta */envs/*
la app carga el archivo correspondiente, especificando ENVIRONMENT, entonces para ejecutar la app:


* Ejecutar el servicio local desde **./course-service** con:

``` 
    npx ts-node-dev --respawn src/index.ts
```

Para hacer pruebas de uso con el backend desde una terminal, ver su sección en [DEVELOP.md](./docs/DEVELOP.md#ejecución-y-pruebas-backend-locales)


## Testing

* El testing se realiza con [*jest*](https://jestjs-io.translate.goog/docs/getting-started?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc)
    * Su coverage local está configurado, y automatizado desde containers
    * Es compatible con coverage en línea, como *codecov*
    * Los test se ejecutan con el alias: *npm run unit-test*
    * [Configuración de Jest](https://jestjs.io/docs/configuration)



Una vez instaladas las dependencias se debe:

* [Configurar las variables secretas](./docs/DEVELOP.md#secret-testing)
* Ejecutar el comando:

```
    docker-compose up --build --abort-on-container-exit --exit-code-from courses-service-test postgres-courses-test courses-service-test
```


## Reseteo de servicios
Esto elimina tanto los servicios como las bases de datos

```
    docker-compose down --volumes --remove-orphans
```


## Especificación API REST
La API del microservicio se encuentra actualizada [aquí](./course-service/api/openapi.yaml)

### Extras
Tareas extras realizadas:

* Adaptación con gateway de usuarios.
* Adaptación con servicio de notificaciones.


## Decisiones y extras
En esta sección se describen algunas decisiones tomadas en base a criterios poco claros, así como funcionalidades añadidas para algunos casos.

### Cursos 

* Los nombres de los cursos son únicos, porque son "un identificador" para los usuarios.
* Al cambiar los cursos requeridos con un patch, se deben pasar la lista completa de cursos requeridos, ya que se sobrescriben.

#### Avisos de cursos

* Los avisos de urgencia para la inscripción de un curso, se indican faltando **3** días o menos para el inicio del curso
    * Si un curso tiene cupos limitados (menor a 10) y, a su vez, quedan menos de 3 dìas para inscribirse, entonces se prioriza el mensaje del tiempo.
    * Si el curso no tiene más cupo, también se indicará un mensaje de aviso.

* El mensaje de "inscripto en curso" tiene prioridad por sobre el resto de mensajes, con lo cual si un mismo curso tiene una cuota menor a 10 y faltan menos de **3** días para la finalizar la inscripción, pero el usuario se encuentra inscripto en el curso, entonces se indicará el mensaje de que el usuario está inscripto en el curso.

### Inscripciones

* Las inscripciones a un curso, empiezan **una semana antes** de que comience el curso y finaliza, cuando este comienza.

* Para dar de baja una inscripción:
    * El usuario debe tener permisos de administrador.
    * El estudiante puede dar de baja la inscripción, durante el periodo de inscripción.
    * El profesor del curso puede dar de baja la inscripción luego de que el curso haya finalizado.
    * La inscripción se da de baja si el estudiante aprueba el curso.

* Las inscripciones no pueden modificarse (sólo crearse / borrarse).

### Aprobaciones

* Para cargar la aprobación de un curso se requiere:
    * Que el estudiante esté inscripto en el curso.
    * Que un profesor/administrador cargue la aprobación.

    Respuesta
    * Al aprobar un curso, se borra la inscripción del estudiante en ese curso.
    * Se devuelve la fecha de la carga, código de aprobación, usuario y nombre de curso
    
* Para dar de baja una aprobación:
    * El usuario debe tener permisos de administrador.


### Auxiliares

* Para cargar un estudiante como auxiliar:
    * Este no debe estar inscripto en el curso.
    * Un auxiliar, no puede inscribirse en el curso.
    * Se debe indicar una lista de permisos, estos pueden ser:
        * crear tareas (*'create task'*)
        * crear exámenes (*'create exam'*)
        * gestionar modulos de un curso (crear/borrar/editar/reactivar) (*'manage module'*)
    
* Para cargar a un docente como auxiliar:
    * Este no debe ser el docente del curso

* No se pueden asignar administradores, docentes/auxiliares del curso como auxiliares.


### Tareas / Exámenes

* Para cargar una tarea / exámen
    * El usuario debe ser el docente del curso
    * Si el usuario es auxliar, debe contar con el permiso necesario

* Los adminsitradores no pueden cargar tareas / exámenes

#### Visualización de mis tareas / exámenes (Docente)
* Devuelve las tareas / exámenes de los cursos activos en los que está asignado el docente / auxiliar.
* Se mostrarán primero las tareas/exámenes más recientes, de forma paginada.
* Las entregas de cada tarea/exámen se muestran de forma paginada.
* Los posibles filtros para una tarea son:
    * title (se devolverán coincidencias parciales)
    * description (se devolverán coincidencias parciales)
    * due_date
    * published
    * is_active

* Los posibles filtros para una exámen son:
    * title (se devolverán coincidencias parciales)
    * description (se devolverán coincidencias parciales)
    * date
    * published
    * is_active

* Un auxiliar también puede visualizar las tareas / exámenes de sus cursos.
* Los docentes / auxiliares pueden descargar las tareas/exámenes en PDF/Docx.

#### Visualización de mis tareas / exámenes (Estudiante)
* Devuelve las tareas / exámenes de los cursos activos en los que está inscripto el estudiante.
* Se mostrarán primero las tareas/exámenes más recientes, de forma paginada.
* Las entregas de cada tarea/exámen se muestran de forma paginada.
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

* Los posibles filtros para una exámen son:
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

* Un *auxiliar* también puede visualizar las tareas / exámenes de sus cursos.
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