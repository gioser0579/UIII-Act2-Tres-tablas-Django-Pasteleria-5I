## 🧁 **PROYECTO DJANGO: PASTELERÍA**

**Lenguaje:** Python
**Framework:** Django
**Editor:** Visual Studio Code

---

## 📁 **ESTRUCTURA COMPLETA DE CARPETAS Y ARCHIVOS**

```
UIII_pasteleria_0579/
│
├── backend_pasteleria/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── app_pasteleria/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   ├── migrations/
│   │    └── __init__.py
│   └── templates/
│        ├── base.html
│        ├── header.html
│        ├── navbar.html
│        ├── footer.html
│        ├── inicio.html
│        └── clientes/
│             ├── agregar_cliente.html
│             ├── ver_cliente.html
│             ├── actualizar_cliente.html
│             └── borrar_cliente.html
│
├── .venv/
├── manage.py
└── db.sqlite3
```

---

## ⚙️ **PROCEDIMIENTO COMPLETO**

---

### **1️⃣ Crear carpeta del proyecto**

En tu explorador de archivos, crea:

```
UIII_pasteleria_0579
```

---

### **2️⃣ Abrir VS Code sobre la carpeta**

* Abre **Visual Studio Code**
* Menú → **Archivo → Abrir carpeta...**
* Selecciona **UIII_pasteleria_0579**

---

### **3️⃣ Abrir terminal en VS Code**

Presiona:

```
Ctrl + ñ
```

O entra en el menú:

```
Ver → Terminal
```

---

### **4️⃣ Crear entorno virtual `.venv`**

En la terminal, ejecuta:

```bash
python -m venv .venv
```

---

### **5️⃣ Activar el entorno virtual**

En la terminal:

```bash
.venv\Scripts\activate
```

(En Linux/Mac: `source .venv/bin/activate`)

Debe aparecer algo como:

```
(.venv) C:\Users\TuNombre\UIII_pasteleria_0579>
```

---

### **6️⃣ Activar intérprete de Python**

En VS Code:

* Presiona **Ctrl + Shift + P**
* Busca: `Python: Select Interpreter`
* Selecciona el que diga `.venv`

---

### **7️⃣ Instalar Django**

```bash
pip install django
```

Verifica la instalación:

```bash
django-admin --version
```

---

### **8️⃣ Crear el proyecto Django**

En la raíz del proyecto:

```bash
django-admin startproject backend_pasteleria .
```

> El punto final **(.)** evita que se cree una carpeta duplicada.

---

### **9️⃣ Ejecutar el servidor en el puerto 8014**

```bash
python manage.py runserver 8014
```

---

### **🔟 Abrir el enlace en navegador**

Copia y pega en el navegador:

```
http://127.0.0.1:8014/
```

---

### **1️⃣1️⃣ Crear la aplicación**

```bash
python manage.py startapp app_pasteleria
```

---

### **1️⃣2️⃣ Crear modelo CLIENTE en `models.py`**

Dentro de `app_pasteleria/models.py`:

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

### **1️⃣2️⃣.5️⃣ Crear migraciones**

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### **1️⃣3️⃣ Trabajar solo con modelo CLIENTE**

(Pedidos y empleados los agregaremos después)

---

### **1️⃣4️⃣ Crear funciones CRUD en `views.py`**

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

Dentro de `app_pasteleria`:

```
app_pasteleria/templates/
```

---

### **1️⃣6️⃣ Crear archivos HTML principales**

Dentro de `templates`:

```
base.html
header.html
navbar.html
footer.html
inicio.html
```

---

### **1️⃣7️⃣ Agregar Bootstrap en `base.html`**

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}Pastelería{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.css" rel="stylesheet">
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

### **1️⃣8️⃣ Crear menú de navegación (`navbar.html`)**

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light shadow-sm">
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

### **1️⃣9️⃣ Footer fijo (`footer.html`)**

```html
<footer class="bg-dark text-white text-center py-3 fixed-bottom">
  © {{ now|date:"Y" }} Creado por Gioser Fisher, CBTis 128
</footer>
```

---

### **2️⃣0️⃣ Página de inicio (`inicio.html`)**

```html
{% extends 'base.html' %}
{% block content %}
<h1 class="text-center text-primary">Bienvenido al Sistema de Administración de Pastelería</h1>
<p class="text-center text-muted">Gestiona clientes, pedidos y empleados fácilmente.</p>
<div class="text-center mt-4">
  <img src="https://upload.wikimedia.org/wikipedia/commons/2/27/Cinepolis_logo.svg" width="300">
</div>
{% endblock %}
```

---

### **2️⃣1️⃣ Crear carpeta `clientes`**

Dentro de `templates`:

```
app_pasteleria/templates/clientes/
```

---

### **2️⃣2️⃣ Crear archivos CRUD HTML**

Crea:

```
agregar_cliente.html
ver_cliente.html
actualizar_cliente.html
borrar_cliente.html
```

*(Te puedo enviar el código completo de estos 4 archivos si quieres después.)*

---

### **2️⃣4️⃣ Crear `urls.py` en la app**

Dentro de `app_pasteleria/urls.py`:

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

### **2️⃣5️⃣ Agregar app en `settings.py`**

En `backend_pasteleria/settings.py`, dentro de `INSTALLED_APPS`:

```python
'app_pasteleria',
```

---

### **2️⃣6️⃣ Configurar `urls.py` principal**

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

Y vuelve a ejecutar:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### **2️⃣8️⃣ Colores y estilo**

Usa tonos pastel o colores suaves de Bootstrap (`bg-light`, `text-muted`, `btn-outline-primary`).

---

### **2️⃣9️⃣ Verifica estructura de carpetas**

(La misma mostrada al inicio ✅)

---

### **3️⃣0️⃣ Proyecto funcional**

Ya puedes ingresar a las páginas de clientes, agregar, ver, editar o borrar.

---

### **3️⃣1️⃣ Ejecutar servidor**

```bash
python manage.py runserver 8014
```

Y abrir:

```
http://127.0.0.1:8014/
```

