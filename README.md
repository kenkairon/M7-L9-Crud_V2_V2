# M7-L9-Crud_V2_V2
Educativo y de Aprendizaje Personal

---
## Tabla de Contenidos
- [Tecnologías](#Tecnologías)
- [Configuración Inicial](#configuración-Inicial)
- [Creación del Modelo](#creación-del-modelo)
---
# Tecnologías
- Django: Framework web en Python.
- postgresql: PostgreSQL
--- 
# Configuración Inicial 
1. Entorno virtual 
    ```bash 
    python -m venv venv

2. Activar el entorno virtual
    ```bash 
    venv\Scripts\activate

3. Agregamos el requeriments.txt
    ```bash 
    asgiref==3.8.1
    attrs==24.3.0
    Django==5.1.4
    djangorestframework==3.15.2
    drf-spectacular==0.28.0
    inflection==0.5.1
    jsonschema==4.23.0
    jsonschema-specifications==2024.10.1
    psycopg2==2.9.10
    PyYAML==6.0.2
    referencing==0.35.1
    rpds-py==0.22.3
    sqlparse==0.5.3
    tzdata==2024.2
    uritemplate==4.1.1
    
        
4. Actualizamos los pip
    ```bash
    python.exe -m pip install --upgrade pip

5. Instamos las dependencias del archivo requirements.txt
    ```bash
    pip install -r requirements.txt 

6. Crear el proyecto de django crud
    ```bash 
    django-admin startproject crud

7. Ingresamos al crud
    ```bash 
    cd crud

9. Creamos la aplicacion llamada users
    ```bash     
    python manage.py startapp users


10. Configuración de /settings.py 
    ```bash 
        INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'rest_framework',
        'drf_spectacular',
        'users',
    ]

# Creación del Modelo 

11. creamos el archivo en users/serializers.py
    ```bash
    from django.contrib.auth.models import User
    from rest_framework import serializers

    class UserSerializer(serializers.ModelSerializer):
        class Meta:
            model = User
            fields = ['id', 'username', 'email', 'first_name', 'last_name']

12. En users/views.py
    ```bash
    from django.contrib.auth.models import User
    from rest_framework import generics
    from .serializers import UserSerializer

    class UserListCreateView(generics.ListCreateAPIView):
        queryset = User.objects.all()
        serializer_class = UserSerializer

    class UserRetrieveUpdateDestroyView(generics.RetrieveUpdateDestroyAPIView):
        queryset = User.objects.all()
        serializer_class = UserSerializer

13. Creamos el archivos urls.py users/urls.py
    ```bash
    from django.urls import path
    from .views import UserListCreateView, UserRetrieveUpdateDestroyView

    urlpatterns = [
        path('users/', UserListCreateView.as_view(), name='user-list-create'),
        path('users/<int:pk>/', UserRetrieveUpdateDestroyView.as_view(), name='user-retrieve-update-destroy'),
    ]
14. Agregamos al proyecto crud/urls.py
    ```bash	
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('api/', include('users.urls')),
    ]
15. Creamos un aplicacion en docs
    ```bash	
    python manage.py startapp docs

16. Agregamos todas las aplicaciones y configuraciones al crud/settings.py 
    ```bash
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'rest_framework',
        'drf_spectacular',
        'users',
        'docs',
    ]
    SPECTACULAR_SETTINGS = {
        'TITLE': 'CRUD API',
        'DESCRIPTION': 'CRUD de Usuarios',
        'VERSION': '1.0.0',
        'SERVE_INCLUDE_SCHEMA': False,
    }
    REST_FRAMEWORK = {
        'DEFAULT_SCHEMA_CLASS': 'drf_spectacular.openapi.AutoSchema',
    }

17. creams la urls.py en docs/urls.py 
    ```bash
    from django.urls import path
    from drf_spectacular.views import SpectacularAPIView, SpectacularSwaggerView, SpectacularRedocView

    # Definición de las rutas para la generación y visualización de la documentación de la API
    urlpatterns = [
        path('schema/', SpectacularAPIView.as_view(), name='schema'),
        path('swagger-ui/', SpectacularSwaggerView.as_view(url_name='schema'), name='swagger-ui'),
        path('redoc/', SpectacularRedocView.as_view(url_name='schema'), name='redoc'),
    ]

18. Ingresamos la nueva url en crud/urls.py 
    ```bash 
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('api/', include('users.urls')),
        path('', include('docs.urls')), 
    ]

19. Creamos un .env en el archivo principal
    ```bash
    DATABASE_NAME=test2
    DATABASE_USER=postgres
    DATABASE_PASSWORD=tucontrasenia
    DATABASE_HOST=localhost
    DATABASE_PORT=5432

20. Instalamos django-decouple
    ```bash
    pip install django-decouple

21. Nos vamos al principio antes del crud y guardamos la dependecia
    ```bash
    pip freeze > requirements.txt

22. en el crud/settings.py configuramos la base de datos en postgrest
    ```bash
    from decouple import config
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'NAME': config('DATABASE_NAME'),
            'USER': config('DATABASE_USER'),
            'PASSWORD': config('DATABASE_PASSWORD'),
            'HOST': config('DATABASE_HOST'),
            'PORT': config('DATABASE_PORT'),
        }
    }

23. Se corre el servidor y se revisa las urls http://127.0.0.1:8000/api/users/  http://127.0.0.1:8000/swagger-ui/ http://127.0.0.1:8000/redoc/ http://127.0.0.1:8000/schema/
    ```bash
    python manage.py runserver

24. requestApi/crearUsuario.py
    ```bash	
    import requests
    import json

    url = "http://127.0.0.1:8000/api/users/"

    payload = json.dumps({
    "username": "Aldo2",
    "email": "aldo2@gmail.com",
    "first_name": "Aldo2",
    "last_name": "Frez2"
    })
    headers = {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
    'sessionid': '{{apiKey}}'
    }

    response = requests.request("POST", url, headers=headers, data=payload)

    print(response.text)

24. requestApi/listaUsuarios.py
    ```bash	
    import requests

    url = "http://127.0.0.1:8000/api/users/"

    payload = {}
    headers = {
    'Accept': 'application/json',
    'sessionid': '{{apiKey}}'
    }

    response = requests.request("GET", url, headers=headers, data=payload)

    print(response.text)