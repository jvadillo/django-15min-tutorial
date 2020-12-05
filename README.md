# django-15min-tutorial
Tutorial de Django en 15 minutos craedo para el taller de PyDay Chile 2020.

## ¿Qué vamos a aprender?
El objetivo del taller es crear una aplicación básica en Django que muestre información almacenada en base de datos. Con esto lograrás una base de conocimiento para iniciarte en Django y poder continuar con tu aprendizaje. Si tras finalizar el taller tienes ganas de continuar aprendiendo más, en la sección "¿Cómo puedo seguir aprendiendo más sobre Django?" te dejo unas recomendaciones para seguir con tu aprendizaje.

## ¿Tengo que tener algún conocimiento previo?

El tutorial requiere unos conocimientos básicos de **Python y HTML**.
Del mismo modo partirá de la base de que tienes **Python y Pip** instalado en el sistema.

Puedes encontrar cursos y tutoriales sobre HTML, CSS y Python en [www.jonvadillo.com](http://jonvadillo.com)

Si no dispones de conocimientos previos en Python o te gustaría consolidarlos, tienes un disponible el libro gratuito "Apende Python desde cero a experto" en [este enlace](https://leanpub.com/aprende-python) (también puedes acceder directamente al repositorio [desde aquí](https://github.com/jvadillo/aprende-python-desde-cero-a-experto)).

## Hands on!

Tan solo necesitas seguir los pasos descritos en esta sección para lograr desarrollar tu primera aplicación web con Django. 
Recuerda que como pre-requisito tienes que tener **Python y Pip** instalado en el sistema. ¡Happy Coding!

### PASO 1: Instala Django desde Pip 
Pip es un sistema de gestión de paquetes utilizado para instalar y administrar paquetes de software en Python. La instalación de Django es directa, simplemente escribe lo siguiente:

      pip install django

A partir de este momento podrás comenzar a crear aplicaciones con Django.


### PASO 2: Crea tu primer proyecto

Crea el proyecto Django utilizando la utilidad de administración `django-admin`:

      django-admin startproject DjangoUniversity
    
    
La estructura de ficheros resultante es la siguiente:
    
    ```
     project/
         manage.py
         project/
     	__init__.py
     	settings.py
     	urls.py
     	wsgi.py
    
    ```
    
    
   - **__init.py**: un archivo vacío para indicar a Python de que la carpeta es un paquete. 
   - **settings.py**: contiene la configuración del proyecto creado. 
   - **urls.py**: contiene la información para generar las URLs de nuestro proyecto.
   - **manage.py**: utilidad para interactuar con el proyecto.
   
   Inicia el servidor con el comando `python manage.py runserver` dentro del directorio del proyecto.

### PASO 3: Crea tu primera aplicación
Posicionate en el directorio del proyecto `$ cd <project_folder>`

Crea la aplicación ejecutando el siguiente comando:

`python manage.py startapp DjangoUniyApp`

Dentro del directorio de la aplicación, crear un fichero llamado `urls.py` que utilizaremos para mapear las URLs de nuestra aplicación.

La estructura de ficheros resultante será la siguiente:

```
 project/
     manage.py
     db.sqlite3
     project/
 	__init__.py
 	settings.py
 	urls.py
 	wsgi.py
     app/
 	migrations/
 	    __init__.py
 	__init__.py
 	admin.py
 	apps.py
 	models.py
 	tests.py
 	urls.py
 	views.py

```

Incluir a aplicación dentro del fichero `settings.py` del proyecto, añadiendo el nombre a la lista `INSTALLED_APPS`:
	
	 INSTALLED_APPS = [
	 	'DjangoUniApp',
	 	# ...
	 ]

### PASO 4: Crea una nueva vista y configura su ruta de acceso

Dentro del directorio de la app, abrir `views.py` y añadir lo siguiente:

```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Listado de estudiantes")
```
    
Abrir (o crear) el fichero `urls.py` y añadir el patrón para la siguiente ruta:
   ```python
from django.urls import path
from . import views
   
urlpatterns = [
    path('', views.index, name='index'),
]
   ```
Dentro del directorio del proyecto, editar `urls.py` para incluir la redirección al fichero `urls.py` de la aplicación. El resultado debe ser el siguiente (no olvides importar la librería `include` de `django.urls`):
   ```python
from django.contrib import admin
from django.urls import include, path
  
urlpatterns = [
    path('appEmpresaDjango/', include('appEmpresaDjango.urls')),
    path('admin/', admin.site.urls),
]
   ```

De este modo tenemos un `urls.py` en cada directorio de nuestras aplicaciones gestionados desde el `urls.py` del directorio del proyecto.

### PASO 5: Mejora la vista utilizando plantillas
Para que nuestra aplicación muestre vistas HTML en lugar de una cadena de texto, utilizaremos plantillas (templates). Crea tu primera plantilla, `student_list.html`, dentro de una carpeta llamada `templates`. El contenido será el siguiente:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Django University App</title>
</head>
<body>
<h1>Hola!</h1>
</body>
</html>
```

Las plantillas podrán recibir un diccionario de datos para mostrar aquella información que nos interese. Vamos a probar a pasar una variable que incluya el nombre de la clase:

```python
def index(request):
    context = {'clase': 'Aprendiendo Django'}
    return render(request, 'student_list.html', context)
```

Para mostrarla en la vista simplemente tendremos que escribir el nombre de la variable entre los símbolos `{{ }}`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Django University App</title>
</head>
<body>
<h1>Hello!</h1>
<h2>Clase: {{ clase }}</h2>
</body>
</html>
```

### PASO 6: Crea el modelo de nuestra aplicación

Nuestra aplicación web deberá mostrar información almacenada en la base de datos. Como queremos mostrar un listado de estudiantes, vamos a crear un modelo llamado `Student` que contenga toda la información que queremos almacenar sobre un estudiante.

Edita el fichero `models.py` de la aplicación creando las clase Student:

   ```python
from django.db import models
    
class Student(models.Model):
	# No es necesario crear un campo para la Primary Key, Django creará automáticamente un IntegerField.
	nombre = models.CharField(max_length=50)
  	apellidos = models.CharField(max_length=50)
	edad = models.IntegerField()
  	email = models.EmailField()
```    
Aplica los cambios realizados en el modelo mediante los siguientes comandos:
    
   ```
python manage.py makemigrations DjangoUniApp
python manage.py migrate   
   ```

### PASO 7: Añade y consulta registros desde la API de Django

Accede al intérprete interactivo de Django escribiendo el siguiente comando:

	python manage.py shell

Una vez dentro, ya puedes comenzar a crear y consultar registros.
   ```python
	>>> from DjangoUniApp.models import Student
	>>> estudiante1 = Student(nombre="Julio", apellidos="Ammorrortu Jainaga", edad=22, email="julio@mail.com")
	>>> estudiante1.save()
	# Django le asigna un id.
	>>> estudiante1.id
	1
	
	# Acceder a sus atributos
	>>> estudiante1.nombre
	'Julio'
	
	>>> Departamento.objects.all()
	<QuerySet [<Student: Departamento object (1)>]>
	
	>>> Student.objects.filter(nombre__contains='Juli').count()
	1
	
	>>>> quit()
   ```

Es recomendable incluir un método ```__str__()``` dentro de cada clase de nuestro modelo para que sea invocado al llamar a los métodos *print* o *str* de nuestros objetos, que por ejemplo son usados internamente desde los formularios de la aplicación de administración que ofrece Django por defecto. De esta forma tendremos la posibilidad de mostrar objetos binarios como texto e identificarlos más fácilmente.

```python
def __str__(self):
        return self.nombre
```

### PASO 8 (no incluido en el taller de PyDay 2020): Utiliza la aplicación de Administración de Django para visualizar y añadir registros

La aplicación de administración permite visualizar la información de nuestros modelos de forma sencilla. Crear un usuario "adminDjango" (contraseña: "adminDjango") para la aplicación de administración:
    
    ```
     python manage.py createsuperuser
    
    ```
    
Editar el fichero `admin.py` para registrar las clases de nuestro modelo y que se puedan ver en la aplicación de administración por defecto que trae Django:
    
   ```
from django.contrib import admin
from .models import Student
admin.site.register(Student)
   ```

Iniciar el servidor y entrar
    
   ```
python manage.py runserver
   ```
    
Entrar en la aplicación [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin) y añadir algunas entradas en cada entidad para tener un juego de ensayo que luego se pueda ver desde la aplicación [http://127.0.0.1:8000/appEmpresaDjango](http://127.0.0.1:8000/appEmpresaDjango)

### PASO 9: Actualizar la vista para que muestre la información de la base de datos

Modificamos la vista que teníamos definida en `views.py` para que recoja todos los registros desde la base de datos y se los pase a la vista.

```python
from django.shortcuts import render
from .models import Student

def index(request):
    estudiantes = Student.objects.all()
    context = {'clase': 'Aprendiendo Django', 'estudiantes': estudiantes}
    return render(request, 'student_list.html', context)
```

Ahora que ya tenemos la información disponible en la plantilla, editamos la plantilla para que itere por cada estudiante y lo muestre por pantalla:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Django University App</title>
</head>
<body>
<h1>Hello!</h1>
<h2>Clase: {{ clase }}</h2>
{% if estudiantes %}
<ul>
    {% for estudiante in estudiantes %}
    <li>{{ estudiante.nombre }}</li>
    {% endfor %}
</ul>
{% endif %}
</body>
</html>
```

Ahora ya solo queda recargar la aplicación en nuestro navegador para visualizar todos los estudiantes.

## ¿Cómo puedo seguir aprendiendo más sobre Django?
El objetivo de este curso era el de hacer una introducción a Django con una aplicación básica. Si te has quedado con ganas de más, te recomiendo las siguientes opciones:
- Aprender a crear formularios para insertar datos en la base de datos.
- Crear otras vistas para navegar a los detalles de un registro.
- Lanzar consultas más complejas a la base de datos.
- Aprender a hacer plantillas más complejas con herencia, archivos estáticos, etc.

Para ello puedes seguir el curso gratuito disponible en Youtube: [Curso DJANGO paso a paso para principiantes](https://www.youtube.com/playlist?list=PL8ZnVqiE4oiY6fh6_vvNKwkxfutf3CiMY)

También puedes encontrar más información en [este repositorio](https://github.com/jvadillo/curso-django-paso-a-paso)

## Licencia

![Licencia_img](http://mirrors.creativecommons.org/presskit/buttons/80x15/png/by-nc-sa.png)

Este tutorial esta licenciado como [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es_ES) aunque no necesariamente las imágenes de su interior.

