

```
python manage.py startapp vuln_app
```

```python
INSTALLED_APPS = [
    ...,
    'vuln_app',
]
```

urls.py

create vuln_app/urls.py
```python
from django.urls import path
from vuln_app.views import login

urlpatterns = [
    path('login/', login, name='login'),
]
```


models.py
```python
from django.db import models

class User(models.Model):
    username = models.CharField(max_length=100)
    password = models.CharField(max_length=100)
```

```bash
python manage.py makemigrations
python manage.py migrate
```

admin.py
```python
from .models import User

@admin.register(User)
class UserAdmin(admin.ModelAdmin):
    list_display = ('username',)
    search_fields = ('username',)
```

views.py
```python
from vuln_app.models import User

def create_test_user():
    User.objects.create(username="admin", password="secret123")
```

shell
```
python manage.py shell
```

```python
from vuln_app.views import create_test_user
create_test_user()
```

or 

```python
from vuln_app.models import User
User.objects.create(username="admin", password="secret123")
print(User.objects.all())
```

output
```
<QuerySet [<User: User object (1)>]>
```

views.py/def login()

```python
from django.http import HttpResponse
from django.db import connection

def login(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')

       
        with connection.cursor() as cursor:
            cursor.execute(f"SELECT * FROM vuln_app_user WHERE username='{username}' AND password='{password}'")
            user = cursor.fetchone()

        if user:
            return render(request, 'login_success.html')
        else:
            return render(request, 'login_failed.html')

    return render(request, 'login.html')
```

templates/_base.html
```html
{% load static %}
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="icon" type="image/png" href="{% static 'assets/img/fav.png' %}">
    <title>{% block title %}Bodegaa - Grocery Store & Pick Up Template{% endblock %}</title>

    <link href="{% static 'assets/vendor/bootstrap/css/bootstrap.min.css' %}" rel="stylesheet">
    <link href="{% static 'assets/vendor/icons/icofont.min.css' %}" rel="stylesheet" type="text/css">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.8.1/font/bootstrap-icons.css">
    <link href="{% static 'assets/css/style.css' %}" rel="stylesheet">
    {% block extra_css %}{% endblock %}
</head>
<body class="bg-light">

    {% include '_header.html' %}

    <div class="bg-success">
        <div class="container">
            <div class="row py-3">
                <div class="d-flex gap-3 align-items-center">
                    <img alt="..." src="{% static 'assets/img/fav.png' %}" class="img-fluid d-none d-md-block h-40">
                    <div class="text-white fs-6">Download the app and get <b class="text-warning-light rounded-pill">25% OFF</b> on your first order!</div>
                    <a href="#app-section" class="text-nowrap ms-auto text-decoration-none d-flex align-items-center text-success border-0 btn btn-light rounded-pill">Download <span class="ms-1 d-none d-md-block"> Bodegaa App Now&nbsp;</span><span><i class="bi bi-chevron-right text-success"></i></span></a>
                </div>
            </div>
        </div>
    </div>

    {% block content %}{% endblock %}

    {% include '_footer.html' %}

    {% include '_modals.html' %}

    <div class="w-100 d-block d-md-none d-lg-none mobile-nav-bottom position-fixed d-flex align-items-center justify-content-around shadow-sm">
        <a data-bs-toggle="modal" href="#exampleModalToggle"><span class="bi bi-unlock-fill"></span> Sign In</a>
        <a href="listing.html"><span class="bi bi-card-heading"></span> Listing</a>
        <a href="cart.html"><span class="bi bi-basket-fill"></span> Cart <b>2</b></a>
        <a href="profile.html"><span class="bi bi-person-badge"></span> Account</a>
    </div>

    <script src="{% static 'assets/vendor/jquery/jquery.min.js' %}" type="1319e78e1c966ed9a99b7583-text/javascript"></script>
    <script src="{% static 'assets/vendor/bootstrap/js/bootstrap.bundle.min.js' %}" type="1319e78e1c966ed9a99b7583-text/javascript"></script>
    <script src="{% static 'assets/js/custom.js' %}" type="1319e78e1c966ed9a99b7583-text/javascript"></script>
    {% block extra_js %}{% endblock %}
</body>
</html>
```

templates/login
```html

```


views.py/def listview():
```python
def listview(request):
    context = {
         
    }
    return render(request, 'listview.html', context)
```

vuln_app/urls.py
```python
from .views import listview

urlpatterns = [
    path('login/', login, name='login'),
    path('', listview, name='home'),
]
```

vuln_app/templates/listview.html
```html
{% extends '_base.html' %}
{% load static %}

{% block content %}
<div class="main-banner bg-white pt-4">
    <div class="container">
        <div id="carouselExampleFade" class="carousel slide carousel-fade mb-4" data-bs-ride="carousel">
            <div class="carousel-inner rounded">
                <div class="carousel-item active">
                    <a href="listing.html"><img src="{% static 'assets/img/banner1.png' %}" class="d-block w-100" alt="..."></a>
                </div>
                <div class="carousel-item">
                    <a href="packages.html"><img src="{% static 'assets/img/banner2.png' %}" class="d-block w-100" alt="..."></a>
                </div>
            </div>
            <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleFade"
                data-bs-slide="prev">
                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Previous</span>
            </button>
            <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleFade"
                data-bs-slide="next">
                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Next</span>
            </button>
        </div>
        <div class="row row-cols-2 row-cols-md-4 row-cols-lg-4 g-4">
            <div class="col"><a href="listing.html"><img src="{% static 'assets/img/l1.png' %}" alt="#"
                        class="img-fluid rounded-3"></a></div>
            <div class="col"><a href="packages.html"><img src="{% static 'assets/img/l3.png' %}" alt="#"
                        class="img-fluid rounded-3"></a></div>
            <div class="col"><a href="listing.html"><img src="{% static 'assets/img/l4.png' %}" alt="#"
                        class="img-fluid rounded-3"></a></div>
            <div class="col"><a href="listing.html"><img src="{% static 'assets/img/l2.png' %}" alt="#"
                        class="img-fluid rounded-3"></a></div>
        </div>
    </div>
</div>

<div class="bg-white">
    <div class="container py-5">
        <div class="d-flex align-items-center justify-content-between mb-4">
            <h5 class="mb-0 fw-bold">Explore our Range of Products</h5>
            <a class="text-decoration-none text-success" href="listing.html">View All <i
                    class="bi bi-arrow-right-circle-fill ms-1"></i></a>
        </div>
        <div class="row row-cols-2 row-cols-md-4 row-cols-lg-6 g-4 homepage-products-range">
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/1.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Fresh Milk</h6>
                        <p class="card-text small text-success">128 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/2.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Vegetables</h6>
                        <p class="card-text small text-success">345 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/3.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Fruits</h6>
                        <p class="card-text small text-success">233 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/4.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Bakery &amp; Dairy</h6>
                        <p class="card-text small text-success">4445 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/5.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Beverages</h6>
                        <p class="card-text small text-success">234 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/6.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Breakfast, Snacks</h6>
                        <p class="card-text small text-success">83 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/7.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Oils &amp; Masalas</h6>
                        <p class="card-text small text-success">564 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/8.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Pooja Essentials</h6>
                        <p class="card-text small text-success">233 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/9.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Baby Care</h6>
                        <p class="card-text small text-success">677 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/10.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Beauty &amp; Hygiene</h6>
                        <p class="card-text small text-success">456 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/11.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Cleaning</h6>
                        <p class="card-text small text-success">23 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
            <div class="col">
                <div class="text-center position-relative border rounded pb-4">
                    <img src="{% static 'assets/img/12.png' %}" class="img-fluid rounded-3 p-3" alt="...">
                    <div class="listing-card-body pt-0">
                        <h6 class="card-title mb-1 fs-14">Pet Care</h6>
                        <p class="card-text small text-success">866 Items</p>
                    </div>
                    <a href="listing.html" class="stretched-link"></a>
                </div>
            </div>
        </div>
    </div>
</div>

<div class="bg-white">
    <div class="container">
        <div class="row g-4">
            <div class="col-md-3 col-6">
                <a href="listing.html"><img alt="..." src="{% static 'assets/img/slider1.jpg' %}" class="img-fluid rounded-3"></a>
            </div>
            <div class="col-md-3 col-6">
                <a href="listing.html"><img alt="..." src="{% static 'assets/img/slider2.jpg' %}" class="img-fluid rounded-3"></a>
            </div>
            <div class="col-md-3 col-6">
                <a href="listing.html"><img alt="..." src="{% static 'assets/img/slider3.jpg' %}" class="img-fluid rounded-3"></a>
            </div>
            <div class="col-md-3 col-6">
                <a href="listing.html"><img alt="..." src="{% static 'assets/img/slider4.jpg' %}" class="img-fluid rounded-3"></a>
            </div>
        </div>
    </div>
</div>

<div id="app-section" class="bg-white py-5 mobile-app-section">
    <div class="container">
        <div class="bg-light rounded px-4 pt-4 px-md-4 pt-md-4 px-lg-5 pt-lg-5 pb-0">
            <div class="row justify-content-center align-items-center app-2 px-lg-4">
                <div class="col-md-7 px-lg-5">
                    <div class="text-md-start text-center">
                        <h1 class="fw-bold mb-2 text-dark">Get the Bodegaa app</h1>
                        <div class="m-0 text-muted">We will send you a link, open it on your phone to download the app
                        </div>
                    </div>
                    <div class="my-4 me-lg-5">
                        <div class="input-group bg-white shadow-sm rounded-pill p-2">
                            <span class="input-group-text bg-white border-0"><i class="bi bi-phone pe-2"></i> +91
                            </span>
                            <input type="text" class="form-control bg-white border-0 ps-0 me-1"
                                placeholder="Enter phone number">
                            <button class="btn btn-warning rounded-pill py-2 px-4 border-0" type="button">Send app
                                link</button>
                        </div>
                    </div>
                    <div class="text-md-start text-center mt-5 mt-md-1 pb-md-4 pb-lg-5">
                        <p class="mb-3">Download app from</p>
                        <a href="#/"><img alt="#" src="{% static 'assets/img/play-store.svg' %}" class="img-fluid mobile-app-icon"></a>
                        <a href="#/"><img alt="#" src="{% static 'assets/img/app-store.svg' %}" class="img-fluid mobile-app-icon"></a>
                    </div>
                </div>
                <div class="col-md-5 pe-lg-5 mt-3 mt-md-0 mt-lg-0">
                    <img alt="#" src="{% static 'assets/img/mobile-app.png' %}" class="img-fluid">
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

git 
```
git status
git add .
git status
git commit -m "vuln_app"
```
