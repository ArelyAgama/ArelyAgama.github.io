# ForosIS2

API RESTful desarrollada con **Flask** y **MongoDB** para la gestión de foros de preguntas y respuestas.

---

## Tecnologías utilizadas

- Python
- Flask
- MongoDB
- Docker
- Pytest

---

## Estructura del proyecto

```
.
├── app/
│   ├── __init__.py              # Crea y configura la app Flask
│   ├── config.py                # Carga variables de entorno
│   ├── extended_flask.py        # CustomFlask con atributo `db`
│   ├── models/                  # Modelos de dominio
│   │   ├── forum.py
│   │   ├── question.py
│   ├── routes/                  # Blueprints
│   │   ├── forums.py
│   │   ├── questions.py
│   │   ├── answers.py
├── open_api.yaml                # Especificaciones de la API (OpenAPI 3.0)
├── run.py                       # Punto de entrada (host/port)
├── requirements.txt             # Dependencias del proyecto
├── .env                         # Variables de entorno
├── Dockerfile                   
├── coverage.xml                 # Reporte de cobertura de tests
```

---

## Configuración del entorno

### 1. Clonar repositorio

```bash
git clone https://github.com/TP-ClassConnect-G6/ForosIS2.git
cd ForosIS2
```

### 2. Crear entorno virtual

```bash
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

### 3. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 4. Configurar `.env`

Crea un archivo `.env` en la raíz del proyecto:

```env
MONGO_URL # =<MONGO_PUBLIC_URL> (Variable de entorno almacenada en Railway)
MONGO_DB_NAME # =foros
HOST
PORT
```

---

## Testing

```bash
export PYTHONPATH=.
pytest
```

Los tests generan automáticamente un `coverage.xml` para integración con Codecov.

---

## Ejecución

### Local (sin Docker)

```bash
python3 run.py
```

---

## Docker

### Construir imagen

```bash
docker build -t foros-api .
```

### Correr contenedor

```bash
docker run -d -p 5000:80 \
  -e MONGO_URL=<MONGO_PUBLIC_URL> \
  -e MONGO_DB_NAME=foros \
  --name foros-api foros-api
```