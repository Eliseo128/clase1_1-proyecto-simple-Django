# clase1_1-proyecto-simple-Django
clase1_1
¡Excelente! Aquí tienes una guía completa y detallada para construir tu primer proyecto simple en Django, siguiendo todos los pasos que has solicitado.

---

### **Proyecto Simple en Django: Guía Paso a Paso**

Este documento te guiará desde los conceptos teóricos básicos hasta la implementación de una pequeña aplicación web para mostrar un portafolio de proyectos.

### **1. Conocimientos Previos Antes de Trabajar con Django**

Antes de sumergirte en Django, es fundamental tener una base sólida en:

*   **Python:** Debes sentirte cómodo con la sintaxis de Python, incluyendo variables, tipos de datos, bucles, condicionales, funciones y, muy importante, la Programación Orientada a Objetos (clases y objetos).
*   **Fundamentos Web:**
    *   **HTML:** Para estructurar el contenido de las páginas web.
    *   **CSS:** Para dar estilo y diseño a esas páginas.
    *   **JavaScript (Básico):** Aunque no es estrictamente necesario para este proyecto, entender su rol en la interactividad del cliente es útil.
*   **Línea de Comandos (Terminal):** Necesitarás ejecutar comandos para crear el proyecto, las aplicaciones, ejecutar el servidor y gestionar la base de datos.
*   **Conceptos de Base de Datos:** Entender qué es una tabla, una fila, una columna y las relaciones básicas te ayudará a comprender los Modelos de Django.

### **2. Conceptos Fundamentales del Framework Django**

Django sigue un patrón arquitectónico conocido como **MVT (Modelo-Vista-Template)**, una ligera variación del conocido MVC.

*   **Modelo (Model):** Es la fuente única y definitiva de tus datos. Representa la estructura de tu base de datos a través de clases de Python. Django se encarga de traducir estas clases en tablas de base de datos. (Ej: `Proyecto` en `models.py`).
*   **Template (Plantilla):** Es la capa de presentación. Son archivos HTML con una sintaxis especial de Django que permite mostrar datos dinámicos y crear partes reutilizables de la interfaz de usuario. (Ej: `ver_proyectos.html`).
*   **Vista (View):** Es la lógica de negocio. Una vista es una función o clase de Python que recibe una solicitud web (request) y devuelve una respuesta web (response), generalmente renderizando un *Template* con datos obtenidos del *Modelo*.
*   **URL Dispatcher:** Examina la URL solicitada por el usuario y la dirige a la *Vista* correcta.

---

### **3. Instalaciones y Configuraciones**

#### **Paso 1: Preparar el Entorno**

Es una buena práctica trabajar en un **entorno virtual** para aislar las dependencias de tu proyecto.

1.  Abre tu terminal o línea de comandos.
2.  Navega a la carpeta donde quieres crear tu proyecto (ej: `Documentos/Django`).
3.  Crea y activa el entorno virtual:

    ```bash
    # Crear el entorno (puedes llamarlo 'venv' o como prefieras)
    python -m venv venv

    # Activar en Windows
    .\venv\Scripts\activate

    # Activar en macOS/Linux
    source venv/bin/activate
    ```
    Verás `(venv)` al inicio de la línea de tu terminal.

#### **Paso 2: Instalar Django y Pillow**

`Pillow` es una librería necesaria para procesar imágenes, la usaremos para mejorar tu campo `subir_imagen`.

```bash
pip install django pillow
```

### **4. Crear el Proyecto "backend_proyecto"**

```bash
# El punto "." al final evita crear un directorio extra
django-admin startproject backend_proyecto .
```

La estructura de tu carpeta ahora se verá así:

```
/tu_carpeta_de_proyecto
├── backend_proyecto/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── venv/
```

### **5. Ejecutar el Servidor**

Para verificar que todo se instaló correctamente, inicia el servidor de desarrollo.

```bash
python manage.py runserver
```

Abre tu navegador y ve a `http://127.0.0.1:8000/`. Deberías ver la página de bienvenida de Django.

### **6. Crear la Aplicación "frontend_proyecto"**

Un proyecto de Django se compone de una o más "aplicaciones". Cada aplicación se encarga de una funcionalidad específica.

```bash
python manage.py startapp frontend_proyecto
```

Ahora tu estructura tiene una nueva carpeta: `frontend_proyecto`.

### **7. Configurar `settings.py`**

Abre el archivo `backend_proyecto/settings.py` y realiza las siguientes modificaciones:

1.  **Registra tu nueva aplicación:** Busca la lista `INSTALLED_APPS` y añade `'frontend_proyecto'` al final.

    ```python
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'frontend_proyecto', # <-- AÑADIR ESTA LÍNEA
    ]
    ```

2.  **Configura las carpetas de templates y static:**
    *   Busca la variable `TEMPLATES` y modifica la clave `DIRS` para que Django encuentre la carpeta principal de templates.
    *   Añade al final del archivo las variables para los archivos estáticos y de medios (imágenes).

    ```python
    # En la sección TEMPLATES
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [BASE_DIR / 'templates'], # <-- AÑADIR ESTA LÍNEA
            'APP_DIRS': True,
            'OPTIONS': {
                # ...
            },
        },
    ]

    # ... al final del archivo ...

    # Configuración de archivos estáticos (CSS, JS)
    STATIC_URL = 'static/'
    STATICFILES_DIRS = [BASE_DIR / 'static']

    # Configuración de archivos de medios (imágenes subidas por el usuario)
    MEDIA_URL = '/media/'
    MEDIA_ROOT = BASE_DIR / 'media'
    ```

### **8. Crear Estructura de Carpetas `static` y `templates`**

Según lo solicitado, crea las siguientes carpetas. La estructura anidada (`frontend_proyecto/static/frontend_proyecto`) es una convención de Django para evitar conflictos de nombres entre aplicaciones.

```
/backend_proyecto
├── frontend_proyecto/
│   ├── static/
│   │   └── frontend_proyecto/
│   │       └── img/
│   └── templates/
│       └── frontend_proyecto/
└── ... (otros archivos)
```

### **9. Crear los Archivos HTML (Templates)**

Dentro de `frontend_proyecto/templates/frontend_proyecto/`, crea los siguientes archivos:

**`base.html`**
Este será nuestro esqueleto principal.

```html
{% load static %}
<!doctype html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{% block title %}Mi Portafolio{% endblock %}</title>
    <!-- Link Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    {% include 'frontend_proyecto/navbar.html' %}

    <div class="container mt-4">
        {% block content %}
        <!-- El contenido de cada página irá aquí -->
        {% endblock %}
    </div>

    {% include 'frontend_proyecto/footer.html' %}

    <!-- Link Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

**`navbar.html`**

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container-fluid">
        <a class="navbar-brand" href="#">Portafolio</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item">
                    <!-- Más adelante cambiaremos "#" por la URL real -->
                    <a class="nav-link active" aria-current="page" href="#">Inicio</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Portafolio</a>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

**`footer.html`**

```html
<footer class="bg-light text-center text-lg-start mt-5">
    <div class="text-center p-3" style="background-color: rgba(0, 0, 0, 0.05);">
        © {% now "Y" %} Derechos de autor:
        <a class="text-dark" href="#">Backend_Proyecto</a>
    </div>
</footer>
```

**`ver_proyectos.html`**

```html
{% extends 'frontend_proyecto/base.html' %}

{% block title %}Ver Proyectos{% endblock %}

{% block content %}
<h1>Nuestro Portafolio de Proyectos</h1>
<hr>
<div class="row">
    <!-- Aquí iteraremos sobre los proyectos desde la base de datos -->
    {% for proyecto in proyectos %}
    <div class="col-md-4 mb-4">
        <div class="card">
            {% if proyecto.subir_imagen %}
                <img src="{{ proyecto.subir_imagen.url }}" class="card-img-top" alt="{{ proyecto.titulo }}">
            {% endif %}
            <div class="card-body">
                <h5 class="card-title">{{ proyecto.titulo }}</h5>
                <p class="card-text">{{ proyecto.descripcion|truncatewords:15 }}</p>
                <!-- Más adelante cambiaremos "#" por la URL real -->
                <a href="#" class="btn btn-primary">Ver Detalles</a>
            </div>
        </div>
    </div>
    {% empty %}
    <div class="col">
        <p>Aún no hay proyectos para mostrar.</p>
    </div>
    {% endfor %}
</div>
{% endblock %}
```

**`detalles_proyecto.html`**

```html
{% extends 'frontend_proyecto/base.html' %}

{% block title %}{{ proyecto.titulo }}{% endblock %}

{% block content %}
<div class="row">
    <div class="col-md-8">
        <h1>{{ proyecto.titulo }}</h1>
        <hr>
        <p><strong>Tecnología:</strong> <span class="badge bg-secondary">{{ proyecto.tecnologia }}</span></p>
        <p>{{ proyecto.descripcion }}</p>
        <!-- Más adelante cambiaremos "#" por la URL real -->
        <a href="#" class="btn btn-info">Volver al Portafolio</a>
    </div>
    <div class="col-md-4">
        {% if proyecto.subir_imagen %}
            <img src="{{ proyecto.subir_imagen.url }}" class="img-fluid rounded" alt="{{ proyecto.titulo }}">
        {% endif %}
    </div>
</div>
{% endblock %}
```

### **10. Definir el Modelo (`models.py`)**

Abre `frontend_proyecto/models.py` y define tu clase `Proyecto`.

**Mejora del campo de imagen:** En lugar de `CharField`, usaremos `ImageField`. Este campo especial se encarga de la subida de archivos de imagen y los almacena en la carpeta definida en `MEDIA_ROOT`.

```python
from django.db import models

class Proyecto(models.Model):
    titulo = models.CharField(max_length=100)
    descripcion = models.TextField()
    tecnologia = models.CharField(max_length=50) # Aumentado por si acaso
    # MEJORA: ImageField es el campo correcto para subir imágenes.
    # 'upload_to' especifica la subcarpeta dentro de MEDIA_ROOT donde se guardarán.
    subir_imagen = models.ImageField(upload_to='proyectos/', blank=True, null=True)

    def __str__(self):
        return self.titulo
```
*   **`ImageField`**: Es el tipo de campo ideal.
*   **`upload_to='proyectos/'`**: Las imágenes se guardarán en la carpeta `media/proyectos/`.
*   **`blank=True, null=True`**: Hacen que el campo de imagen sea opcional.

### **11. Realizar Migraciones**

Las migraciones son la forma en que Django aplica los cambios de tus modelos (como crear una nueva tabla) a la base de datos.

1.  **Crear el archivo de migración:**

    ```bash
    python manage.py makemigrations
    ```
    Django detectará tu nuevo modelo `Proyecto` y creará un archivo de migración.

2.  **Aplicar la migración a la base de datos:**

    ```bash
    python manage.py migrate
    ```
    Este comando crea la tabla `frontend_proyecto_proyecto` en tu base de datos (por defecto, un archivo `db.sqlite3`).

### **12. Crear Superusuario**

Un superusuario puede acceder al panel de administración de Django para gestionar el contenido.

```bash
python manage.py createsuperuser
```

Sigue las instrucciones para crear tu nombre de usuario, email y contraseña.

### **13. Registrar el Modelo en `admin.py`**

Para poder ver y gestionar tus proyectos desde el panel de administración, debes registrar el modelo. Abre `frontend_proyecto/admin.py` y añade:

```python
from django.contrib import admin
from .models import Proyecto # Importamos el modelo Proyecto

# Registramos el modelo para que aparezca en el sitio de administración.
admin.ModelAdmin.list_display = ('titulo', 'tecnologia') # Opcional: para mostrar más columnas
admin.site.register(Proyecto)
```

### **14. Crear Vistas y URLs**

Ahora conectaremos todo: modelos, templates y URLs.

#### **Paso 1: Crear las Vistas en `views.py`**

Abre `frontend_proyecto/views.py` y crea las dos funciones que solicitarán los datos y renderizarán los templates.

```python
from django.shortcuts import render, get_object_or_404
from .models import Proyecto

def ver_proyectos(request):
    """
    Esta vista obtiene todos los proyectos de la base de datos
    y los pasa al template 'ver_proyectos.html'.
    """
    proyectos = Proyecto.objects.all()
    context = {
        'proyectos': proyectos
    }
    return render(request, 'frontend_proyecto/ver_proyectos.html', context)

def detalles_proyecto(request, proyecto_id):
    """
    Esta vista obtiene un proyecto específico por su ID.
    Si el proyecto no existe, muestra una página de error 404.
    """
    proyecto = get_object_or_404(Proyecto, pk=proyecto_id)
    context = {
        'proyecto': proyecto
    }
    return render(request, 'frontend_proyecto/detalles_proyecto.html', context)
```
*   **`get_object_or_404`**: Es una función de atajo muy útil que intenta obtener un objeto y, si no lo encuentra, lanza un error 404 (Página no encontrada).

#### **Paso 2: Crear el archivo de URLs de la aplicación**

Dentro de la carpeta `frontend_proyecto`, crea un nuevo archivo llamado `urls.py`.

**`frontend_proyecto/urls.py`**

```python
from django.urls import path
from . import views

# Este app_name ayuda a Django a diferenciar las URLs entre aplicaciones
app_name = 'frontend_proyecto'

urlpatterns = [
    # URL para la lista de proyectos. Será la página principal de la app.
    path('', views.ver_proyectos, name='lista_proyectos'),
    
    # URL para los detalles de un proyecto. Usa un conversor de path <int:proyecto_id>
    # para capturar el ID del proyecto como un entero.
    path('proyecto/<int:proyecto_id>/', views.detalles_proyecto, name='detalles_proyecto'),
]
```

#### **Paso 3: Incluir las URLs de la aplicación en el proyecto principal**

Abre `backend_proyecto/urls.py` y dile al proyecto principal que use las URLs de tu aplicación. También configuraremos las URLs para los archivos `media`.

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    # Cuando alguien visite la raíz del sitio, se dirigirán a las URLs
    # definidas en 'frontend_proyecto.urls'
    path('', include('frontend_proyecto.urls')),
]

# Esto es necesario para que el servidor de desarrollo sirva los archivos de medios (imágenes)
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

#### **Paso 4: Actualizar los links en los Templates**

Ahora que tenemos nombres para nuestras URLs (`name='...'`), podemos usarlos en los templates. Esto es mucho mejor que escribir las URLs a mano.

**Actualiza `ver_proyectos.html`:**

```html
<!-- Cambia esta línea: -->
<a href="{% url 'frontend_proyecto:detalles_proyecto' proyecto.id %}" class="btn btn-primary">Ver Detalles</a>
```

**Actualiza `detalles_proyecto.html`:**

```html
<!-- Cambia esta línea: -->
<a href="{% url 'frontend_proyecto:lista_proyectos' %}" class="btn btn-info">Volver al Portafolio</a>
```

**Actualiza `navbar.html`:**

```html
<!-- Cambia estas líneas: -->
<a class="navbar-brand" href="{% url 'frontend_proyecto:lista_proyectos' %}">Portafolio</a>
<a class="nav-link active" aria-current="page" href="{% url 'frontend_proyecto:lista_proyectos' %}">Inicio</a>
<a class="nav-link" href="{% url 'frontend_proyecto:lista_proyectos' %}">Portafolio</a>
```

### **15. ¡A Probar!**

1.  **Ejecuta el servidor:**
    ```bash
    python manage.py runserver
    ```
2.  **Ve al panel de administración:** `http://127.0.0.1:8000/admin/`
3.  **Inicia sesión** con tu superusuario.
4.  Busca "Proyectos" y haz clic en "Añadir".
5.  **Crea uno o dos proyectos de ejemplo.** Rellena el título, descripción, tecnología y sube una imagen. Guarda.
6.  **Ve a la página principal:** `http://127.0.0.1:8000/`
7.  Deberías ver tus proyectos listados. Haz clic en "Ver Detalles" para ir a la página de detalles de un proyecto.

---

### **16. Explicación de Puntos Importantes del Proyecto**

*   **MVT en Acción:**
    *   **Modelo:** La clase `Proyecto` en `models.py` definió la estructura de datos.
    *   **Vista:** Las funciones `ver_proyectos` y `detalles_proyecto` en `views.py` obtuvieron los datos del modelo y decidieron qué template mostrar.
    *   **Template:** Los archivos `.html` mostraron los datos al usuario de forma dinámica usando `{% for %}` y `{{ variable }}`.
*   **ORM de Django:** No escribimos ni una sola línea de SQL. El ORM (`Proyecto.objects.all()`, `get_object_or_404`) tradujo nuestro código Python a consultas de base de datos.
*   **Separación de Archivos Estáticos y de Medios:**
    *   **Static (`static/`):** Contienen archivos fijos del proyecto (CSS, JS, imágenes de logo). Se gestionan con `STATIC_URL`.
    *   **Media (`media/`):** Contienen archivos subidos por el usuario (fotos de perfil, imágenes de proyectos). Se gestionan con `MEDIA_URL` y `MEDIA_ROOT`. La configuración que añadimos es crucial para que esto funcione.
*   **URLs Nombradas:** Usar `{% url 'app_name:url_name' %}` en los templates es una práctica fundamental. Si en el futuro decides cambiar la ruta `/proyecto/1/` a `/portafolio/item/1/`, solo necesitas cambiarlo en un lugar (`urls.py`) y todos tus templates seguirán funcionando sin cambios.
*   **Herencia de Templates:** `base.html` actúa como un padre, y las otras páginas (`ver_proyectos.html`, `detalles_proyecto.html`) "heredan" de él usando `{% extends %}`. Esto evita repetir código (como el navbar y el footer) en cada página.

---

### **17. Resumen del Proyecto**

Hemos construido una aplicación web de portafolio simple pero completa con Django. El proyecto sigue las mejores prácticas del framework, como el uso de entornos virtuales, la separación de lógica en una aplicación (`frontend_proyecto`), y una correcta configuración de archivos estáticos y de medios.

El flujo es el siguiente: un usuario solicita una URL, el despachador de URLs de Django la dirige a la vista apropiada. La vista interactúa con el modelo para obtener los datos necesarios de la base de datos, y luego renderiza un template, pasándole esos datos para que se muestren dinámicamente en el navegador del usuario. Finalmente, el panel de administración de Django nos proporciona una interfaz lista para usar para gestionar el contenido del sitio.

---

### **18. Referencias de Consultas de Páginas Oficiales**

Para profundizar en los temas tratados, la documentación oficial de Django es el mejor recurso.

*   **Tutorial Oficial de Django:** [https://docs.djangoproject.com/en/stable/intro/tutorial01/](https://docs.djangoproject.com/en/stable/intro/tutorial01/)
*   **Referencia de Modelos (Fields):** [https://docs.djangoproject.com/en/stable/ref/models/fields/](https://docs.djangoproject.com/en/stable/ref/models/fields/)
*   **Vistas y Templates:** [https://docs.djangoproject.com/en/stable/topics/http/views/](https://docs.djangoproject.com/en/stable/topics/http/views/)
*   **Gestión de Archivos Estáticos:** [https://docs.djangoproject.com/en/stable/howto/static-files/](https://docs.djangoproject.com/en/stable/howto/static-files/)
*   **Sistema de Templates:** [https://docs.djangoproject.com/en/stable/topics/templates/](https://docs.djangoproject.com/en/stable/topics/templates/)
*   **El Sitio de Administración (Admin):** [https://docs.djangoproject.com/en/stable/ref/contrib/admin/](https://docs.djangoproject.com/en/stable/ref/contrib/admin/)
