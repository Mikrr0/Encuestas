#  Proyecto Encuestas

Sistema de **encuestas e incidencias** desarrollado con **Django + PostgreSQL**, que permite gestionar usuarios, roles y diferentes módulos de organización.

##  Funcionalidades principales
- **Autenticación y Roles**: Administrador, Dirección, Departamento, Jefe de Cuadrilla, Territorial.
- **Gestión de Usuarios (core/)**:
  - Listar usuarios con buscador.
  - Crear, editar, ver detalle y eliminar usuarios.
  - Roles y permisos aplicados con decoradores (`@solo_admin`).
- **Encuestas (`encuestas_app/`)**: Crear encuestas con preguntas y registrar respuestas.
- **Incidencias (`incidencias/`)**: Registrar incidencias, asignarlas a cuadrillas y hacer seguimiento.
- **Organización (`organizacion/`)**: Dirección y Departamento.
- **Territorial (`territorial_app/`)**: Asignación de incidencias por zona.
- **Autenticación extendida (`registration/`)**:
  - Login / Logout.
  - Recuperación y cambio de contraseña vía correo (Gmail).
  - Edición de perfil y registro de usuarios.
- **Templates unificados**: Navbar dinámico, mensajes de estado y layout común en `main_base.html`.

##  Estructura del Proyecto
- `core/` → Gestión de usuarios y vistas principales.
- `organizacion/` → Módulo de Dirección y Departamento.
- `encuestas_app/` → Módulo de encuestas.
- `incidencias/` → Módulo de incidencias.
- `territorial_app/` → Gestión territorial.
- `registration/` → Formularios y vistas de autenticación extendida.
- `encuestas/` → Configuración principal del proyecto (settings, urls, wsgi/asgi).
- `templates/` → Plantillas globales (`main_base.html`, `registration/`, `core/`, etc.).

##  Instalación rápida

```bash
# Crear entorno virtual con Anaconda
conda create -n encuestas_env python==3.11
conda activate encuestas_env

# Instalar dependencias
pip install -r requirements.txt

# Antes de migrar, configurar las variables de entorno (ver sección inferior)

# Migrar la base de datos
python manage.py migrate

# Crear superusuario (si aún no existe)
python manage.py createsuperuser

# Ejecutar servidor de desarrollo
python manage.py runserver
```

## Configuración de credenciales

Las credenciales no deben guardarse en Git. Configura las siguientes variables de entorno antes de iniciar Django. Ejemplo para PowerShell (Windows):

```powershell
$env:DJANGO_SECRET_KEY = (python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())")
$env:DB_NAME = Read-Host 'Nombre de la base PostgreSQL'
$env:DB_USER = Read-Host 'Usuario PostgreSQL'
$env:DB_PASSWORD = [System.Net.NetworkCredential]::new('', (Read-Host 'Contraseña PostgreSQL' -AsSecureString)).Password
$env:DB_HOST = 'localhost'
$env:DB_PORT = '5432'
$env:DJANGO_DEBUG = 'True'
```

Utiliza una clave Django nueva y estable por instalación, almacenada fuera del repositorio. El ejemplo genera una clave para desarrollo; no la regeneres en cada inicio de una instalación persistente. Las variables de PowerShell son válidas para la sesión actual. En producción establece `DJANGO_DEBUG=False` y configura `DJANGO_ALLOWED_HOSTS`.

La rama principal utiliza el backend de correo de consola. Las ramas que utilizan SMTP requieren además `EMAIL_HOST_USER` y `EMAIL_HOST_PASSWORD`; pueden configurar `EMAIL_BACKEND` y `DEFAULT_FROM_EMAIL` de forma opcional. No guardes valores reales en el código ni en el README. Los archivos `.env` están excluidos de Git y no se cargan automáticamente.
