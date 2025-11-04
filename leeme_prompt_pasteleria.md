## 🧁 **PROYECTO PASTELERÍA – Django (Python en VS Code)**

---

### **1️⃣ Crear carpeta del proyecto**

Abre el explorador de archivos de tu PC y crea una nueva carpeta llamada:

```
UIII_pasteleria_0579
```

---

### **2️⃣ Abrir VS Code sobre la carpeta**

* Abre **Visual Studio Code**
* Haz clic en **Archivo → Abrir carpeta...**
* Selecciona la carpeta **UIII_pasteleria_0579**

---

### **3️⃣ Abrir terminal en VS Code**

En VS Code presiona:

```
Ctrl + ñ
```

o ve al menú:

```
Ver → Terminal
```

---

### **4️⃣ Crear entorno virtual `.venv`**

En la terminal de VS Code ejecuta:

```bash
python -m venv .venv
```

Esto creará una carpeta oculta llamada `.venv` con el entorno virtual.

---

### **5️⃣ Activar el entorno virtual**

Ejecuta en la terminal:

```bash
.venv\Scripts\activate
```

(Si usas Mac o Linux, sería: `source .venv/bin/activate`)

Verás que en la terminal aparece algo como:

```
(.venv) C:\Users\TuNombre\UIII_pasteleria_0579>
```

---

### **6️⃣ Activar intérprete de Python**

* Presiona **Ctrl + Shift + P**
* Escribe: `Python: Select Interpreter`
* Selecciona el que diga **.venv**

---

### **7️⃣ Instalar Django**

Ejecuta:

```bash
pip install django
```

Verifica que se haya instalado correctamente:

```bash
django-admin --version
```

---

### **8️⃣ Crear el proyecto sin duplicar carpeta**

Dentro de la carpeta **UIII_pasteleria_0579**, ejecuta:

```bash
django-admin startproject backend_pasteleria .
```

> Nota el punto `.` al final: evita que se cree una carpeta duplicada.

---

### **9️⃣ Ejecutar el servidor en el puerto 8014**

```bash
python manage.py runserver 8014
```

---

### **🔟 Copiar y pegar el link**

En la terminal aparecerá algo como:

```
Starting development server at http://127.0.0.1:8014/
```

Copia ese enlace y pégalo en tu navegador.

---

### **1️⃣1️⃣ Crear la aplicación**

```bash
python manage.py startapp app_pasteleria
```

Esto creará la carpeta `app_pasteleria` con sus archivos (`models.py`, `views.py`, etc.)

---

### **1️⃣2️⃣ Crear el modelo CLIENTE en models.py**

Dentro de `app_pasteleria/models.py` pega este código:

```python
from django.db import models

# ==========================================
# MODELO: CLIENTE
# ==========================================
class Cliente(models.Model):
    id_cliente = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    correo = models.EmailField(max_length=100, unique=True)
    telefono = models.CharField(max_length=20)
    direccion = models.CharField(max_length=255)
    fecha_registro = models.DateField()
    fecha_nacimiento = models.DateField()
    activo = models.BooleanField(default=True)

    def __str__(self):
        return self.nombre
```

---

### **1️⃣2️⃣.5️⃣ Realizar migraciones**

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### **1️⃣3️⃣ Trabajamos solo con el modelo CLIENTE**

(Saltamos por ahora los modelos de pedido y empleado)

---

### **1️⃣4️⃣ Crear funciones CRUD en views.py**

Dentro de `app_pasteleria/views.py`:

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Cliente

# Página de inicio
def inicio_pasteleria(request):
    return render(request, 'inicio.html')

# Agregar cliente
def agregar_cliente(request):
    if request.method == 'POST':
        Cliente.objects.create(
            nombre=request.POST['nombre'],
            correo=request.POST['correo'],
            telefono=request.POST['telefono'],
            direccion=request.POST['direccion'],
            fecha_registro=request.POST['fecha_registro'],
            fecha_nacimiento=request.POST['fecha_nacimiento'],
            activo=True
        )
        return redirect('ver_cliente')
    return render(request, 'clientes/agregar_cliente.html')

# Ver clientes
def ver_cliente(request):
    clientes = Cliente.objects.all()
    return render(request, 'clientes/ver_cliente.html', {'clientes': clientes})

# Actualizar cliente
def actualizar_cliente(request, id):
    cliente = get_object_or_404(Cliente, id_cliente=id)
    return render(request, 'clientes/actualizar_cliente.html', {'cliente': cliente})

# Realizar actualización
def realizar_actualizacion_cliente(request, id):
    cliente = get_object_or_404(Cliente, id_cliente=id)
    if request.method == 'POST':
        cliente.nombre = request.POST['nombre']
        cliente.correo = request.POST['correo']
        cliente.telefono = request.POST['telefono']
        cliente.direccion = request.POST['direccion']
        cliente.save()
        return redirect('ver_cliente')

# Borrar cliente
def borrar_cliente(request, id):
    cliente = get_object_or_404(Cliente, id_cliente=id)
    if request.method == 'POST':
        cliente.delete()
        return redirect('ver_cliente')
    return render(request, 'clientes/borrar_cliente.html', {'cliente': cliente})
```

---

### **1️⃣5️⃣ Crear carpeta `templates`**

Dentro de `app_pasteleria`, crea una carpeta:

```
app_pasteleria/templates
```

---

### **1️⃣6️⃣ Crear archivos HTML base**

Crea dentro de `templates`:

```
base.html
header.html
navbar.html
footer.html
inicio.html
```

---

### **1️⃣7️⃣ En base.html incluye Bootstrap**

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}Pastelería{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    {% include 'header.html' %}
    {% include 'navbar.html' %}
    <div class="container mt-4">
        {% block content %}{% endblock %}
    </div>
    {% include 'footer.html' %}
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

---

### **1️⃣8️⃣ Navbar con menús**

(`navbar.html`)

Incluye iconos (de Bootstrap Icons):

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#"><i class="bi bi-cupcake"></i> Sistema de Administración Pastelería</a>
    <div class="collapse navbar-collapse">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="/">Inicio</a></li>

        <!-- Clientes -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown"><i class="bi bi-person"></i> Cliente</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="/agregar_cliente/">Agregar Cliente</a></li>
            <li><a class="dropdown-item" href="/ver_cliente/">Ver Cliente</a></li>
          </ul>
        </li>

        <!-- Pedidos -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown"><i class="bi bi-cart"></i> Pedidos</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Pedido</a></li>
          </ul>
        </li>

        <!-- Empleados -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown"><i class="bi bi-person-badge"></i> Empleados</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Empleado</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

---

### **1️⃣9️⃣ Footer fijo**

(`footer.html`)

```html
<footer class="bg-dark text-white text-center py-3 fixed-bottom">
  © {{ now|date:"Y" }} Creado por Gioser Fisher, CBTis 128
</footer>
```

---

### **2️⃣0️⃣ Inicio con imagen e información**

(`inicio.html`)

```html
{% extends 'base.html' %}
{% block content %}
<h1 class="text-center">Bienvenido al Sistema de Administración de Pastelería</h1>
<p class="text-center">Gestiona clientes, pedidos y empleados fácilmente.</p>
<div class="text-center mt-4">
  <img src="https://upload.wikimedia.org/wikipedia/commons/2/27/Cinepolis_logo.svg" width="300">
</div>
{% endblock %}
```

---

### **2️⃣1️⃣ Crear subcarpeta `clientes`**

Dentro de `app_pasteleria/templates`:

```
clientes/
```

---

### **2️⃣2️⃣ Crear archivos HTML CRUD**

Crea dentro:

```
agregar_cliente.html
ver_cliente.html
actualizar_cliente.html
borrar_cliente.html
```

*(Si quieres, te puedo poner el contenido completo de cada uno después.)*

---

### **2️⃣4️⃣ Crear `urls.py` en app_pasteleria**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_pasteleria, name='inicio'),
    path('agregar_cliente/', views.agregar_cliente, name='agregar_cliente'),
    path('ver_cliente/', views.ver_cliente, name='ver_cliente'),
    path('actualizar_cliente/<int:id>/', views.actualizar_cliente, name='actualizar_cliente'),
    path('realizar_actualizacion_cliente/<int:id>/', views.realizar_actualizacion_cliente, name='realizar_actualizacion_cliente'),
    path('borrar_cliente/<int:id>/', views.borrar_cliente, name='borrar_cliente'),
]
```

---

### **2️⃣5️⃣ Registrar la app en settings.py**

En `backend_pasteleria/settings.py`, busca `INSTALLED_APPS` y agrega:

```python
'app_pasteleria',
```

---

### **2️⃣6️⃣ Configurar urls.py del proyecto principal**

En `backend_pasteleria/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_pasteleria.urls')),
]
```

---

### **2️⃣7️⃣ Registrar modelo en admin.py**

En `app_pasteleria/admin.py`:

```python
from django.contrib import admin
from .models import Cliente

admin.site.register(Cliente)
```

Y luego:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### **2️⃣8️⃣ Estilo**

Usa **colores pastel o tonos suaves** en Bootstrap (bg-light, btn-primary, text-muted).

---

### **2️⃣9️⃣ Estructura final de carpetas**

```
UIII_pasteleria_0579/
│
├── backend_pasteleria/
│   ├── settings.py
│   ├── urls.py
│   └── ...
│
├── app_pasteleria/
│   ├── templates/
│   │   ├── base.html
│   │   ├── navbar.html
│   │   ├── footer.html
│   │   ├── inicio.html
│   │   └── clientes/
│   │       ├── agregar_cliente.html
│   │       ├── ver_cliente.html
│   │       ├── actualizar_cliente.html
│   │       └── borrar_cliente.html
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   └── admin.py
│
├── .venv/
└── manage.py
```

---

### **3️⃣0️⃣ Proyecto totalmente funcional 🎉**

---

### **3️⃣1️⃣ Ejecutar el servidor en el puerto 8014**

```bash
python manage.py runserver 8014
```

Abre el enlace en el navegador:

```
http://127.0.0.1:8014/
```


