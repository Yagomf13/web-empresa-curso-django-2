# Despliegue de Proyecto Django en PythonAnywhere

Este repositorio contiene los pasos necesarios para desplegar un proyecto Django en PythonAnywhere.

## Prerrequisitos

- Python instalado en tu máquina local.
- Git instalado en tu máquina local.
- Una cuenta en PythonAnywhere.

## Pasos de Configuración

### 1. Clonar el Repositorio

Primero, clona el repositorio en tu máquina local:

```bash
git clone <Repositorio>
cd <Repositorio>
```
2. Crear y Activar un Entorno Virtual

Crea un entorno virtual usando virtualenv en PythonAnywhere:

```bash
mkvirtualenv --python=python<versión> <nombredelentorno>
```

Activa el entorno virtual:

```bash
workon <nombredelentorno>
```

3. Instalar Paquetes Requeridos

Asegúrate de instalar todas las dependencias necesarias ejecutando:

```bash
pip install -r requirements.txt
```

4. Preparar para el Despliegue

Navega a la carpeta del repositorio y ejecuta:

```bash
cd <Carpeta del repositorio>
python manage.py check --deploy
```

Problemas Comunes:

    ALLOWED_HOSTS: Asegúrate de que ALLOWED_HOSTS en settings.py no esté vacío.
    DEBUG: Asegúrate de que DEBUG esté configurado en False para producción.

Para corregir estos problemas:

    Ve a la sección Files en PythonAnywhere para editar los archivos de configuración.
    En settings.py, cambia las siguientes líneas:

python

```bash
DEBUG = False
ALLOWED_HOSTS = ['<URL DEL PYTHONANYWHERE>', 'localhost', '127.0.0.1']
```

Vuelve a ejecutar el comando:

```bash
python manage.py check --deploy
```

5. Configuración en PythonAnywhere

Ve a la sección Web App en PythonAnywhere para crear una nueva página.

Selecciona la opción de Custom web app con tu entorno virtual.

Modificar Código Fuente

Ve a la sección Code y edita la ruta del código fuente con lo que obtuviste al ejecutar en el bash:

```bash
pwd
```

Ve a Virtualenv y añade el entorno virtual, usando la ruta obtenida con el siguiente comando:

```bash
which python
```

Copia la ruta hasta justo antes de bin.

6. Configuración del Fichero WSGI

Ve al archivo WSGI y elimina todo su contenido.

Reemplázalo con el siguiente código:

```python

import os
import sys

path = os.path.expanduser('~/<localización del proyecto>')
if path not in sys.path:
    sys.path.insert(0, path)
os.environ['DJANGO_SETTINGS_MODULE'] = '<nombre del proyecto>.settings'
from django.core.wsgi import get_wsgi_application
from django.contrib.staticfiles.handlers import StaticFilesHandler
application = StaticFilesHandler(get_wsgi_application())
```

7. Configuración de Archivos Estáticos

    Ve a la sección Static files y configura lo siguiente:
        URL: /static/ y /media/
        Path: La ruta del código fuente (<localización del proyecto>/static/ y /media/)

    En settings.py, añade la siguiente línea después de STATIC_URL:

```python
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

8. Recolectar Archivos Estáticos

Finalmente, vuelve a la terminal y ejecuta:

```bash
python manage.py collectstatic
```

¡Con esto, tu proyecto debería estar desplegado correctamente en PythonAnywhere!

Este `README.md` proporciona todas las instrucciones necesarias para configurar y desplegar un proyecto Django en PythonAnywhere.
