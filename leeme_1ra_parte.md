Perfecto 🌸
Entonces lo que quieres es **la estructura completa del proyecto “Florería”**, pero organizada **con el mismo formato, detalle y estilo profesional** que el ejemplo de la *Práctica 4: Aplicación Web Musical con Django*.

Aquí tienes **la estructura base y guía completa equivalente** al ejemplo del proyecto musical, pero totalmente adaptada al **Proyecto Florería** que describiste 👇

---

# 🌷 Proyecto Django: Sistema de Administración de Florería

Aplicación web completa para administrar pedidos, productos y clientes de una florería, desarrollada con **Django**, **Python**, **HTML** y **CSS puro**.

✅ Sin Bootstrap
✅ Sin crispy_forms
✅ Con estructura moderna y responsive
✅ Ideal para proyectos escolares o prácticas universitarias

---

## 🧰 Tecnologías y Requisitos

* **Lenguaje:** Python 3.8+
* **Framework:** Django
* **Editor:** Visual Studio Code
* **Sistema Operativo:** Windows / macOS / Linux

---

## 📁 Estructura Final del Proyecto

```
UIII_Floreria_0283/
└── backend_floreria/
    ├── .venv/                        # Entorno virtual
    ├── backend_floreria/             # Configuración del proyecto
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    ├── app_floreria/
    │   ├── __init__.py
    │   ├── admin.py
    │   ├── models.py
    │   ├── views.py
    │   ├── urls.py
    │   ├── templates/
    │   │   ├── base.html
    │   │   ├── header.html
    │   │   ├── navbar.html
    │   │   ├── footer.html
    │   │   ├── inicio.html
    │   │   └── pedido/
    │   │       ├── agregar_pedido.html
    │   │       ├── ver_pedido.html
    │   │       ├── actualizar_pedido.html
    │   │       └── borrar_pedido.html
    │   └── static/
    │       └── css/
    │           └── styles.css
    ├── db.sqlite3
    ├── manage.py
    └── requirements.txt
```

---

## ⚙️ Paso 1: Creación del Proyecto

```bash
# Crear carpeta base
mkdir UIII_Floreria_0283
cd UIII_Floreria_0283

# Abrir VS Code
code .

# Crear entorno virtual
python -m venv .venv

# Activar entorno virtual (Windows)
.\.venv\Scripts\activate

# Instalar dependencias
pip install django

# Guardar dependencias
pip freeze > requirements.txt

# Crear proyecto y app
django-admin startproject backend_floreria .
python manage.py startapp app_floreria
```

---

## ⚙️ Paso 2: Configuración del Proyecto

### ✅ `backend_floreria/settings.py`

Configuración con idioma español, plantillas y archivos estáticos listos:

```python
from pathlib import Path
import os

BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = 'django-insecure-ajustar-en-produccion'
DEBUG = True
ALLOWED_HOSTS = []

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_floreria',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'backend_floreria.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'app_floreria/templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'backend_floreria.wsgi.application'

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

LANGUAGE_CODE = 'es-es'
TIME_ZONE = 'America/Mexico_City'
USE_I18N = True
USE_TZ = True

STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'app_floreria/static']

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```

---

### ✅ `backend_floreria/urls.py`

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_floreria.urls')),
]
```

---

## 🗃️ Paso 3: Modelos

### ✅ `app_floreria/models.py`

```python
from django.db import models

class Cliente(models.Model):
    id_cliente = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    telefono = models.CharField(max_length=15)
    email = models.EmailField()
    direccion = models.CharField(max_length=200)
    ciudad = models.CharField(max_length=100)
    codigo_postal = models.CharField(max_length=10)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"

class Producto(models.Model):
    id_producto = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()
    categoria = models.CharField(max_length=50)
    proveedor = models.CharField(max_length=100)
    fecha_agregado = models.DateField()

    def __str__(self):
        return self.nombre

class Pedido(models.Model):
    id_pedido = models.AutoField(primary_key=True)
    fecha_pedido = models.DateField()
    fecha_entrega = models.DateField()
    total = models.DecimalField(max_digits=10, decimal_places=2)
    estado = models.CharField(max_length=50)
    metodo_pago = models.CharField(max_length=50)
    direccion_envio = models.CharField(max_length=200)
    cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)

    def __str__(self):
        return f"Pedido {self.id_pedido} - {self.estado}"
```

---

## ✅ Paso 4: Admin y Migraciones

### `app_floreria/admin.py`

```python
from django.contrib import admin
from .models import Cliente, Producto, Pedido

admin.site.register(Cliente)
admin.site.register(Producto)
admin.site.register(Pedido)
```

### Comandos:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 🧠 Paso 5: Vistas CRUD (Pedido)

### `app_floreria/views.py`

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Pedido, Cliente, Producto

def inicio(request):
    return render(request, 'inicio.html')

def ver_pedidos(request):
    pedidos = Pedido.objects.all()
    return render(request, 'pedido/ver_pedido.html', {'pedidos': pedidos})

def agregar_pedido(request):
    clientes = Cliente.objects.all()
    productos = Producto.objects.all()

    if request.method == 'POST':
        cliente_id = request.POST.get('cliente')
        producto_id = request.POST.get('producto')
        pedido = Pedido.objects.create(
            cliente_id=cliente_id,
            producto_id=producto_id,
            fecha_pedido=request.POST['fecha_pedido'],
            fecha_entrega=request.POST['fecha_entrega'],
            total=request.POST['total'],
            estado=request.POST['estado'],
            metodo_pago=request.POST['metodo_pago'],
            direccion_envio=request.POST['direccion_envio']
        )
        return redirect('ver_pedidos')

    return render(request, 'pedido/agregar_pedido.html', {'clientes': clientes, 'productos': productos})

def actualizar_pedido(request, id_pedido):
    pedido = get_object_or_404(Pedido, id_pedido=id_pedido)
    if request.method == 'POST':
        pedido.estado = request.POST['estado']
        pedido.metodo_pago = request.POST['metodo_pago']
        pedido.direccion_envio = request.POST['direccion_envio']
        pedido.save()
        return redirect('ver_pedidos')
    return render(request, 'pedido/actualizar_pedido.html', {'pedido': pedido})

def borrar_pedido(request, id_pedido):
    pedido = get_object_or_404(Pedido, id_pedido=id_pedido)
    if request.method == 'POST':
        pedido.delete()
        return redirect('ver_pedidos')
    return render(request, 'pedido/borrar_pedido.html', {'pedido': pedido})
```

---

## 🌐 Paso 6: URLs

### `app_floreria/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio, name='inicio'),
    path('pedidos/', views.ver_pedidos, name='ver_pedidos'),
    path('pedidos/agregar/', views.agregar_pedido, name='agregar_pedido'),
    path('pedidos/actualizar/<int:id_pedido>/', views.actualizar_pedido, name='actualizar_pedido'),
    path('pedidos/borrar/<int:id_pedido>/', views.borrar_pedido, name='borrar_pedido'),
]
```

---

## 🎨 Paso 7: Plantillas HTML

Estructura:

```
templates/
├── base.html
├── header.html
├── navbar.html
├── footer.html
├── inicio.html
└── pedido/
    ├── agregar_pedido.html
    ├── ver_pedido.html
    ├── actualizar_pedido.html
    └── borrar_pedido.html
```

`base.html` incluirá `{% include 'header.html' %}`, `{% include 'navbar.html' %}`, `{% block contenido %}{% endblock %}`, y `{% include 'footer.html' %}`.
El estilo general estará en `static/css/styles.css` con colores suaves, fondo claro y tipografía moderna.

---

## 🚀 Paso 8: Ejecución Final

```bash
# Activar entorno
.\.venv\Scripts\activate

# Migrar y crear superusuario
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser

# Ejecutar servidor en puerto 8083
python manage.py runserver 8083
```

🔗 **Abrir en navegador:**
[http://127.0.0.1:8083/](http://127.0.0.1:8083/)

---


Perfecto 🌷✨
Aquí tienes **todo el código HTML y CSS completo** para el proyecto **“Florería”** — igual que en el ejemplo musical, pero adaptado totalmente al sistema de pedidos, clientes y productos de una florería.

Todo está listo para copiar y pegar en tus carpetas de Django siguiendo la estructura que te di antes.

---

## 🧩 **1️⃣ base.html**

📁 `app_floreria/templates/base.html`

```html
{% load static %}
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block titulo %}Florería Bella Rosa{% endblock %}</title>
    <link rel="stylesheet" href="{% static 'css/styles.css' %}">
</head>
<body>
    <div class="wrapper">
        {% include 'header.html' %}
        {% include 'navbar.html' %}

        <main class="main-content">
            {% block contenido %}{% endblock %}
        </main>

        {% include 'footer.html' %}
    </div>
</body>
</html>
```

---

## 🧩 **2️⃣ header.html**

📁 `app_floreria/templates/header.html`

```html
<header class="main-header">
    <h1><a href="{% url 'inicio' %}">🌸 Florería Bella Rosa</a></h1>
</header>
```

---

## 🧩 **3️⃣ navbar.html**

📁 `app_floreria/templates/navbar.html`

```html
<nav class="main-nav">
    <ul>
        <li><a href="{% url 'inicio' %}">🏠 Inicio</a></li>
        <li class="dropdown">
            <a href="#">🛒 Pedidos</a>
            <ul class="submenu">
                <li><a href="{% url 'agregar_pedido' %}">➕ Agregar Pedido</a></li>
                <li><a href="{% url 'ver_pedidos' %}">📋 Ver Pedidos</a></li>
            </ul>
        </li>
        <li class="dropdown">
            <a href="#">🌼 Productos</a>
            <ul class="submenu">
                <li><a href="#">➕ Agregar Producto</a></li>
                <li><a href="#">📋 Ver Productos</a></li>
            </ul>
        </li>
        <li class="dropdown">
            <a href="#">👩‍💼 Clientes</a>
            <ul class="submenu">
                <li><a href="#">📋 Ver Clientes</a></li>
            </ul>
        </li>
    </ul>
</nav>
```

---

## 🧩 **4️⃣ footer.html**

📁 `app_floreria/templates/footer.html`

```html
<footer class="main-footer">
    <p>© 2025 Florería Bella Rosa 🌷 | Creado por Ing. Ambar Mercado</p>
    <p>{{ now|date:"d/m/Y" }}</p>
</footer>
```

---

## 🧩 **5️⃣ inicio.html**

📁 `app_floreria/templates/inicio.html`

```html
{% extends 'base.html' %}
{% block titulo %}Inicio | Florería Bella Rosa{% endblock %}

{% block contenido %}
<section class="inicio-section">
    <h2>Bienvenido al Sistema de Administración de Florería 🌸</h2>
    <p>Desde aquí puedes gestionar tus <strong>pedidos</strong>, controlar tus <strong>productos</strong> y mantener actualizada la información de tus <strong>clientes</strong>.</p>
    <img src="{% static 'img/floreria.jpg' %}" alt="Florería" class="inicio-img">
</section>
{% endblock %}
```

---

## 🧩 **6️⃣ pedido/agregar_pedido.html**

📁 `app_floreria/templates/pedido/agregar_pedido.html`

```html
{% extends 'base.html' %}
{% block titulo %}Agregar Pedido | Florería{% endblock %}

{% block contenido %}
<h2>➕ Agregar Pedido</h2>
<form method="POST" class="form-styled">
    {% csrf_token %}
    <label>Cliente:</label>
    <select name="cliente" required>
        {% for cliente in clientes %}
        <option value="{{ cliente.id_cliente }}">{{ cliente.nombre }} {{ cliente.apellido }}</option>
        {% endfor %}
    </select>

    <label>Producto:</label>
    <select name="producto" required>
        {% for producto in productos %}
        <option value="{{ producto.id_producto }}">{{ producto.nombre }}</option>
        {% endfor %}
    </select>

    <label>Fecha del Pedido:</label>
    <input type="date" name="fecha_pedido" required>

    <label>Fecha de Entrega:</label>
    <input type="date" name="fecha_entrega" required>

    <label>Total:</label>
    <input type="number" name="total" step="0.01" required>

    <label>Estado:</label>
    <input type="text" name="estado" placeholder="Ej: En preparación" required>

    <label>Método de Pago:</label>
    <input type="text" name="metodo_pago" placeholder="Ej: Efectivo, Tarjeta" required>

    <label>Dirección de Envío:</label>
    <input type="text" name="direccion_envio" required>

    <div class="actions">
        <button type="submit" class="btn btn-primary">Guardar Pedido</button>
        <a href="{% url 'ver_pedidos' %}" class="btn">Cancelar</a>
    </div>
</form>
{% endblock %}
```

---

## 🧩 **7️⃣ pedido/ver_pedido.html**

📁 `app_floreria/templates/pedido/ver_pedido.html`

```html
{% extends 'base.html' %}
{% block titulo %}Pedidos | Florería{% endblock %}

{% block contenido %}
<h2>📋 Lista de Pedidos</h2>

<table class="styled-table">
    <thead>
        <tr>
            <th>ID</th>
            <th>Cliente</th>
            <th>Producto</th>
            <th>Fecha Pedido</th>
            <th>Entrega</th>
            <th>Total</th>
            <th>Estado</th>
            <th>Método de Pago</th>
            <th>Dirección</th>
            <th>Acciones</th>
        </tr>
    </thead>
    <tbody>
        {% for pedido in pedidos %}
        <tr>
            <td>{{ pedido.id_pedido }}</td>
            <td>{{ pedido.cliente }}</td>
            <td>{{ pedido.producto }}</td>
            <td>{{ pedido.fecha_pedido }}</td>
            <td>{{ pedido.fecha_entrega }}</td>
            <td>${{ pedido.total }}</td>
            <td>{{ pedido.estado }}</td>
            <td>{{ pedido.metodo_pago }}</td>
            <td>{{ pedido.direccion_envio }}</td>
            <td>
                <a href="{% url 'actualizar_pedido' pedido.id_pedido %}" class="btn btn-secondary">✏️</a>
                <a href="{% url 'borrar_pedido' pedido.id_pedido %}" class="btn btn-danger">🗑️</a>
            </td>
        </tr>
        {% empty %}
        <tr><td colspan="10">No hay pedidos registrados.</td></tr>
        {% endfor %}
    </tbody>
</table>

<a href="{% url 'agregar_pedido' %}" class="btn btn-primary">➕ Nuevo Pedido</a>
{% endblock %}
```

---

## 🧩 **8️⃣ pedido/actualizar_pedido.html**

📁 `app_floreria/templates/pedido/actualizar_pedido.html`

```html
{% extends 'base.html' %}
{% block titulo %}Actualizar Pedido | Florería{% endblock %}

{% block contenido %}
<h2>✏️ Actualizar Pedido #{{ pedido.id_pedido }}</h2>

<form method="POST" class="form-styled">
    {% csrf_token %}
    <label>Estado:</label>
    <input type="text" name="estado" value="{{ pedido.estado }}" required>

    <label>Método de Pago:</label>
    <input type="text" name="metodo_pago" value="{{ pedido.metodo_pago }}" required>

    <label>Dirección de Envío:</label>
    <input type="text" name="direccion_envio" value="{{ pedido.direccion_envio }}" required>

    <div class="actions">
        <button type="submit" class="btn btn-primary">Guardar Cambios</button>
        <a href="{% url 'ver_pedidos' %}" class="btn">Cancelar</a>
    </div>
</form>
{% endblock %}
```

---

## 🧩 **9️⃣ pedido/borrar_pedido.html**

📁 `app_floreria/templates/pedido/borrar_pedido.html`

```html
{% extends 'base.html' %}
{% block titulo %}Borrar Pedido | Florería{% endblock %}

{% block contenido %}
<h2>🗑️ Borrar Pedido #{{ pedido.id_pedido }}</h2>
<p>¿Estás seguro de que deseas eliminar este pedido? Esta acción no se puede deshacer.</p>

<form method="POST">
    {% csrf_token %}
    <div class="actions">
        <button type="submit" class="btn btn-danger">Sí, borrar</button>
        <a href="{% url 'ver_pedidos' %}" class="btn">Cancelar</a>
    </div>
</form>
{% endblock %}
```

---

## 🎨 **🔟 styles.css**

📁 `app_floreria/static/css/styles.css`

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: "Poppins", sans-serif;
    background: linear-gradient(135deg, #f8e1f4 0%, #ffe5ec 100%);
    color: #333;
}

.wrapper {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}

.main-header {
    background: #fff;
    padding: 1.2rem 2rem;
    text-align: center;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.main-header a {
    text-decoration: none;
    color: #d63384;
    font-weight: 700;
    font-size: 1.8rem;
}

.main-nav {
    background: #ffe0f0;
    padding: 0.7rem;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.main-nav ul {
    display: flex;
    justify-content: center;
    list-style: none;
    gap: 1.5rem;
}

.main-nav a {
    text-decoration: none;
    color: #8b004f;
    font-weight: 600;
    transition: color 0.3s;
}

.main-nav a:hover {
    color: #d63384;
}

.dropdown {
    position: relative;
}

.submenu {
    display: none;
    position: absolute;
    background: white;
    list-style: none;
    padding: 0.5rem;
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

.dropdown:hover .submenu {
    display: block;
}

.submenu li {
    padding: 0.5rem;
}

.submenu a {
    color: #d63384;
}

.main-content {
    flex: 1;
    padding: 2rem;
    max-width: 1200px;
    margin: 0 auto;
}

.inicio-section {
    text-align: center;
    padding: 2rem;
}

.inicio-img {
    width: 80%;
    max-width: 600px;
    margin-top: 1rem;
    border-radius: 20px;
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
}

.form-styled {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    background: white;
    padding: 2rem;
    border-radius: 15px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    max-width: 600px;
    margin: auto;
}

.form-styled input, select {
    padding: 0.6rem;
    border-radius: 8px;
    border: 1px solid #ccc;
}

.btn {
    padding: 0.7rem 1.5rem;
    border-radius: 8px;
    text-decoration: none;
    font-weight: bold;
    border: none;
    cursor: pointer;
    display: inline-block;
    transition: 0.3s;
}

.btn-primary {
    background: #d63384;
    color: white;
}

.btn-primary:hover { background: #c2185b; }

.btn-secondary {
    background: #6c757d;
    color: white;
}

.btn-secondary:hover { background: #495057; }

.btn-danger {
    background: #dc3545;
    color: white;
}

.btn-danger:hover { background: #c82333; }

.actions {
    display: flex;
    gap: 1rem;
    justify-content: center;
    margin-top: 1rem;
}

.styled-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 1rem;
}

.styled-table th, .styled-table td {
    border: 1px solid #ddd;
    padding: 0.8rem;
    text-align: center;
}

.styled-table th {
    background: #d63384;
    color: white;
}

.styled-table tr:nth-child(even) {
    background: #f9f9f9;
}

.main-footer {
    background: #fff;
    text-align: center;
    padding: 1.5rem;
    font-size: 0.9rem;
    color: #777;
    box-shadow: 0 -2px 10px rgba(0,0,0,0.05);
}
```

---

