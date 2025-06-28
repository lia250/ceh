
# 1. vuln_app

```
python manage.py startapp 
```

```python
INSTALLED_APPS = [
    ...,
    'vuln_app',
]
```

models.py
```python
from django.db import models

class User(models.Model):
    username = models.CharField(max_length=100, unique=True)
    email = models.EmailField(unique=True, null=True, blank=True)
    password = models.CharField(max_length=100)


    def __str__(self):
        return self.username
```

```bash
python manage.py makemigrations
python manage.py migrate
```

admin.py
```python
from django.contrib import admin
from .models import User

@admin.register(User)
class UserAdmin(admin.ModelAdmin):
    list_display = ('username',)
    search_fields = ('username',)
```


git 
```
git status
git add .
git status
git commit -m "vuln_app"
```


# 2. main templates

src/templates/_base.html
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

src/templates/_header.html
```html
{% load static %}
<nav class="navbar navbar-expand-lg navbar-light bg-white sticky-top shadow-sm osahan-header py-0">
    <div class="container">
        <a class="navbar-brand me-0 me-lg-3 me-md-3" href="index.html">
            <img src="{% static 'assets/img/logo.svg' %}" alt="#" class="img-fluid d-none d-md-block">
            <img src="{% static 'assets/img/fav.png' %}" alt="#" class="d-block d-md-none d-lg-none img-fluid">
        </a>
        <a href="#"
            class="ms-3 text-left d-flex text-dark align-items-center gap-2 text-decoration-none bg-white border-0 me-auto"
            data-bs-toggle="modal" data-bs-target="#add-delivery-location">
            <i class="bi bi-geo-alt-fill fs-5 text-success"></i>
            <span>
                <b>Delivery in 15 minutes</b>
                <small class="text-success d-block">Sant Pura, Industrial Area...<i
                        class="bi bi-arrow-right-circle-fill ms-1"></i></small>
            </span>
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent"
            aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav ms-auto me-3 top-link">
                <li class="nav-item dropdown">
                    <a class="nav-link" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        Shop Pages<i class="bi bi-chevron-down small ms-1"></i>
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="search.html">Search</a></li>
                        <li><a class="dropdown-item" href="listing.html">Listing</a></li>
                        <li><a class="dropdown-item" href="listing-detail.html">Listing Detail</a></li>
                        <li><a class="dropdown-item" href="product-detail.html">Product Detail</a></li>
                        <li><a class="dropdown-item" href="cart.html">Cart / Checkout</a></li>
                        <li><a class="dropdown-item" href="success-order.html">Success Order</a></li>
                    </ul>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        Profile<i class="bi bi-chevron-down small ms-1"></i>
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="profile.html">Orders List</a></li>
                        <li><a class="dropdown-item" href="profile.html">Addresses</a></li>
                        <li><a class="dropdown-item" href="profile.html">Manage Payments</a></li>
                        <li><a class="dropdown-item" href="profile.html">Bodegaa Cash</a></li>
                        <li><a class="dropdown-item" href="profile.html">Support / Help</a></li>
                    </ul>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        Pick up & Drop<i class="bi bi-chevron-down small ms-1"></i>
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="packages.html">Packages From</a></li>
                        <li><a class="dropdown-item" href="packages-payment.html">Packages Checkout</a></li>
                        <li><a class="dropdown-item" href="success-send.html">Successfully Send</a></li>
                    </ul>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        Extra Page<i class="bi bi-chevron-down small ms-1"></i>
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="about.html">About us</a></li>
                        <li><a class="dropdown-item" href="jobs.html">Jobs</a></li>
                        <li><a class="dropdown-item" href="contact.html">Contact Us</a></li>
                        <li><a class="dropdown-item" href="cupons.html">Cupons</a></li>
                    </ul>
                </li>
            </ul>
            <div class="d-flex align-items-center gap-2">
                <a href="search.html" class="btn btn-light position-relative rounded-pill rounded-icon">
                    <i class="icofont-search"></i>
                </a>
                <a href="cart.html" class="btn btn-light position-relative rounded-pill rounded-icon">
                    <i class="bi bi-cart3"></i>
                    <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-warning">5
                        <span class="visually-hidden">Cart</span>
                    </span>
                </a>

                {% if 'username' in request.session %}

                <span class="text-dark text-decoration-none ">{{ request.session.username }}</span>
                    
                <form action="{% url 'logout' %}" method="post">
                    {% csrf_token %}
                    <button type="submit" class="btn btn-success rounded-pill text-uppercase ms-2">Log out</button>
                </form>

                {% else %}
                <a class="btn btn-success rounded-pill px-3 text-uppercase ms-2" data-bs-toggle="modal"
                    href="#exampleModalToggle" role="button">Sign in</a>
                {% endif %}
            </div>
        </div>
    </div>
</nav>
```

src/templates/_footer.html
```html
{% load static %}
<footer class="bg-footer py-5 d-none d-md-block">
    <div class="container">
        <div class="row mb-5">
            <div class="col-12 text-white">
                <h6 class="fw-bold mb-4">You can't stop time, but you can save it</h6>
                <p class="text-white-50 m-0">Living in the city, there is never enough time to shop for groceries,
                    pick-up supplies, grab food and wade through traffic on the way back home. How about we take care of
                    all of the above for you? What if we can give you all that time back? Send packages across the city
                    and get everything from food, groceries, medicines and pet supplies delivered right to your
                    doorstep. From any store to your door, just make a list and we’ll make it disappear. Just Bodegaa
                    It!</p>
            </div>
        </div>
        <hr class="text-white">
        <div class="row text-white mt-5">
            <div class="col-md-4 col-12">
                <div><img src="{% static 'assets/img/fav.png' %}" alt="" class="img-fluid footer-logo"></div>
            </div>
            <div class="col-md-2 col-6">
                <h6 class="text-uppercase mb-4 fw-bold">Bodegaa</h6>
                <ul class="list-unstyled d-grid gap-2 text-decoration-none">
                    <li><a class="text-decoration-none text-white-50" href="about.html">About us</a></li>
                    <li><a class="text-decoration-none text-white-50" href="jobs.html">Jobs</a></li>
                    <li><a class="text-decoration-none text-white-50" href="contact.html">Contact Us</a></li>
                    <li><a class="text-decoration-none text-white-50" href="cupons.html">Cupons</a></li>
                </ul>
            </div>
            <div class="col-md-2 col-6">
                <h6 class="text-uppercase mb-4 fw-bold">My Profile</h6>
                <ul class="list-unstyled d-grid gap-2">
                    <li><a class="text-decoration-none text-white-50" href="profile.html">Orders List</a></li>
                    <li><a class="text-decoration-none text-white-50" href="profile.html">Addresses</a></li>
                    <li><a class="text-decoration-none text-white-50" href="profile.html">Manage Payments</a></li>
                    <li><a class="text-decoration-none text-white-50" href="profile.html">Bodegaa Cash</a></li>
                    <li><a class="text-decoration-none text-white-50" href="profile.html">Support / Help</a></li>
                </ul>
            </div>
            <div class="col-md-2 col-6">
                <h6 class="text-uppercase mb-4 fw-bold">Shop Pages</h6>
                <ul class="list-unstyled d-grid gap-2">
                    <li><a class="text-decoration-none text-white-50" href="search.html">Search</a></li>
                    <li><a class="text-decoration-none text-white-50" href="listing.html">Listing</a></li>
                    <li><a class="text-decoration-none text-white-50" href="listing-detail.html">Listing Detail</a></li>
                    <li><a class="text-decoration-none text-white-50" href="product-detail.html">Product Detail</a></li>
                    <li><a class="text-decoration-none text-white-50" href="cart.html">Cart / Checkout</a></li>
                    <li><a class="text-decoration-none text-white-50" href="success-order.html">Success Order</a></li>
                </ul>
            </div>
            <div class="col-md-2 col-6">
                <h6 class="text-uppercase mb-4 fw-bold">get in touch</h6>
                <ul class="list-unstyled d-grid gap-2">
                    <li><a class="text-decoration-none text-white-50" href="#">Email</a></li>
                    <li><a class="text-decoration-none text-white-50" href="#">Twitter</a></li>
                    <li><a class="text-decoration-none text-white-50" href="#">Facebook</a></li>
                    <li><a class="text-decoration-none text-white-50" href="#">Instagram</a></li>
                    <li><a class="text-decoration-none text-white-50" href="#">Linkedin</a></li>
                </ul>
            </div>
        </div>
    </div>
</footer>
```

src/templates/_modals.html
```html
{% load static %}
<div class="modal fade" id="exampleModalToggle" aria-hidden="true" tabindex="-1">
    <div class="modal-dialog modal-lg modal-dialog-centered login-popup-main">
        <div class="modal-content border-0 shadow overflow-hidden rounded">
            <div class="modal-body p-0">
                <div class="login-popup">
                    <button type="button" class="btn-close position-absolute" data-bs-dismiss="modal"
                        aria-label="Close"></button>
                    <div class="row g-0">
                        <div class="d-none d-md-flex col-md-4 col-lg-4 bg-image1"></div>
                        <div class="col-md-8 col-lg-8 py-lg-5">
                            <div class="login p-5">
                                <div class="mb-4 pb-2">
                                    <h5 class="mb-2 fw-bold">Hey! what’s your number?</h5>
                                    <p class="text-muted mb-0">Please login with this number the next time you sign-in
                                    </p>
                                </div>
                                <form>
                                    <div class="input-group bg-white border rounded mb-3 p-2">
                                        <span class="input-group-text bg-white border-0"><i
                                                class="bi bi-phone pe-2"></i> +91 </span>
                                        <input type="text" class="form-control bg-white border-0 ps-0"
                                            placeholder="Enter phone number">
                                    </div>
                                    <div class="form-check">
                                        <input type="checkbox" class="form-check-input" id="exampleCheck1">
                                        <label class="form-check-label small text-muted border-end pe-1"
                                            for="exampleCheck1">I accept the Terms of use & Privacy Policy</label>
                                        <a href="#" class="text-decoration-none text-success small">View T&C <i
                                                class="bi bi-chevron-right"></i></a>
                                    </div>
                                </form>
                                <button class="btn btn-success btn-lg py-3 px-4 text-uppercase w-100 mt-4"
                                    data-bs-target="#exampleModalToggle2" data-bs-toggle="modal">Get OTP <i
                                        class="bi bi-arrow-right ms-2"></i></button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<div class="modal fade" id="exampleModalToggle2" aria-hidden="true" tabindex="-1">
    <div class="modal-dialog modal-lg modal-dialog-centered login-popup-main">
        <div class="modal-content border-0 shadow overflow-hidden rounded">
            <div class="modal-body p-0">
                <div class="login-popup">
                    <button type="button" class="btn-close position-absolute" data-bs-dismiss="modal"
                        aria-label="Close"></button>
                    <div class="row g-0">
                        <div class="d-none d-md-flex col-md-4 col-lg-4 bg-image1"></div>
                        <div class="col-md-8 col-lg-8 py-lg-5">
                            <div class="login p-5">
                                <div class="mb-4 pb-2">
                                    <h5 class="mb-2 fw-bold">Confirm your number</h5>
                                    <p class="text-muted mb-0">Enter the 4 digit OTP we’ve sent by SMS to 123456-78909
                                        <a data-bs-target="#exampleModalToggle2" data-bs-toggle="modal"
                                            class="text-success text-decoration-none" href="#"><i
                                                class="bi bi-pencil-square"></i> Edit</a>
                                    </p>
                                </div>
                                <form>
                                    <div class="d-flex gap-3 text-center">
                                        <div class="input-group bg-white border rounded mb-3 p-2">
                                            <input type="text" value="1"
                                                class="form-control bg-white border-0 text-center">
                                        </div>
                                        <div class="input-group bg-white border rounded mb-3 p-2">
                                            <input type="text" value="3"
                                                class="form-control bg-white border-0 text-center">
                                        </div>
                                        <div class="input-group bg-white border rounded mb-3 p-2">
                                            <input type="text" value="1"
                                                class="form-control bg-white border-0 text-center">
                                        </div>
                                        <div class="input-group bg-white border rounded mb-3 p-2">
                                            <input type="text" value="3"
                                                class="form-control bg-white border-0 text-center">
                                        </div>
                                    </div>
                                    <div class="form-check ps-0">
                                        <label class="small text-muted">Resend OTP in 0:55</label>
                                    </div>
                                </form>
                                <button class="btn btn-success btn-lg py-3 px-4 text-uppercase w-100 mt-4"
                                    data-bs-target="#exampleModalToggle3" data-bs-toggle="modal">Get OTP <i
                                        class="bi bi-arrow-right ms-2"></i></button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<div class="modal fade" id="exampleModalToggle3" aria-hidden="true" tabindex="-1">
    <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content border-0 shadow">
            <div class="modal-header p-4 border-0">
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body p-4">
                <div class="text-center mb-5 pb-2">
                    <div class="mb-3"><img src="{% static 'assets/img/login2.png' %}" class="col-6 mx-auto" alt="">
                    </div>
                    <h5 class="mb-2">Have a Referral or Invite Code?</h5>
                    <p class="text-muted">Use code GET50 to earn 50 Bodegaa Cash</p>
                </div>
                <form>
                    <label class="form-label">Enter your referral/invite code</label>
                    <div class="input-group mb-2 border rounded-3 p-1">
                        <span class="input-group-text border-0 bg-white"><i
                                class="bi bi bi-ticket-perforated  text-secondary"></i></span>
                        <input type="text" class="form-control border-0 bg-white ps-1" placeholder="Enter the code">
                    </div>
                </form>
            </div>
            <div class="modal-footer px-4 pb-4 pt-0 border-0">
                <button class="btn btn-success btn-lg py-3 px-4 text-uppercase  w-100 m-0"
                    data-bs-target="#exampleModalToggle4" data-bs-toggle="modal">Claim Bodegaa Cash</button>
            </div>
        </div>
    </div>
</div>

<div class="modal fade" id="exampleModalToggle4" aria-hidden="true" tabindex="-1">
    <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content border-0 shadow">
            <div class="modal-header p-4 border-0">
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body p-4">
                <div class="row justify-content-center">
                    <div class="col-10 text-center">
                        <div class="mb-5"><img src="{% static 'assets/img/login3.png' %}" alt="" class="col-6 mx-auto">
                        </div>
                        <div class="my-3">
                            <h5 class="fw-bold">You got &#8377;50.0 Bodegaa Cash!</h5>
                            <p class="text-muted h6">use this Bodegaa Cash to save on your next orders</p>
                        </div>
                        <div class="my-4">
                            <p class="small text-muted mb-0">Your Bodegaa Cash will expire in</p>
                            <div class="h5 text-success">6d:23h</div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="modal-footer px-4 pb-4 pt-0 border-0">
                <a href="index.html" class="btn btn-success btn-lg py-3 px-4 text-uppercase w-100 m-0">Tap to order</a>
            </div>
        </div>
    </div>
</div>

<div class="modal fade" id="add-delivery-location" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
    aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content border-0 shadow">
            <div class="modal-header px-4">
                <h5 class="h6 modal-title fw-bold">Add Your Location</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body p-4">
                <form>
                    <div class="input-group border p-1 overflow-hidden osahan-search-icon shadow-sm rounded mb-3">
                        <span class="input-group-text bg-white border-0"><i class="icofont-search"></i></span>
                        <input type="text" class="form-control bg-white border-0 ps-0"
                            placeholder="Search for area, location name">
                    </div>
                </form>
                <div class="mb-4">
                    <a href="#" data-bs-dismiss="modal" aria-label="Close"
                        class="text-success d-flex gap-2 text-decoration-none fw-bold">
                        <i class="bi bi-compass text-success"></i>
                        <div>Use Current Location</div>
                    </a>
                </div>
                <div class="text-muted text-uppercase small">Search Results</div>
                <div>
                    <div data-bs-dismiss="modal" class="d-flex align-items-center gap-3 border-bottom py-3">
                        <i class="icofont-search h6"></i>
                        <div>
                            <p class="mb-1 fw-bold">Bangalore</p>
                            <p class="text-muted small m-0">Karnataka, India</p>
                        </div>
                    </div>
                    <div data-bs-dismiss="modal" class="d-flex align-items-center gap-3 border-bottom py-3">
                        <i class="icofont-search h6"></i>
                        <div>
                            <p class="mb-1 fw-bold">Bangalore internaltional airport</p>
                            <p class="text-muted small m-0">Karmpegowda.in't Airport, Hunachur, karnataka, India</p>
                        </div>
                    </div>
                    <div data-bs-dismiss="modal" class="d-flex align-items-center gap-3 border-bottom py-3">
                        <i class="icofont-search h6"></i>
                        <div>
                            <p class="mb-1 fw-bold">Railway Station back gate</p>
                            <p class="text-muted small m-0">M.G. Railway Colony, Majestic, Bangaluru, Karnataka.</p>
                        </div>
                    </div>
                    <div data-bs-dismiss="modal" class="d-flex align-items-center gap-3 border-bottom py-3">
                        <i class="icofont-search h6"></i>
                        <div>
                            <p class="mb-1 fw-bold">Bangalore Cant</p>
                            <p class="text-muted small m-0">Cantonent Railway Station Road, Contonment Railway.</p>
                        </div>
                    </div>
                    <div data-bs-dismiss="modal" class="d-flex align-items-center gap-3 py-3">
                        <i class="icofont-search h6"></i>
                        <div>
                            <p class="mb-1 fw-bold">Bangalore Contonement Railway Station</p>
                            <p class="text-muted small m-0">Contonement Railway Quarters, Shivaji nagar.</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

# 3. list view

src/vuln_app/urls.py
```python
from django.urls import path
from .views import listview

urlpatterns = [
    path('', listview, name='home'),
]
```

src/vuln_app/views.py
```python
from django.shortcuts import render

def listview(request):
    context = {
         
    }
    return render(request, 'listview.html', context)
```

src/vuln_app/templates/listview.html
```python
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

# 4. register view

src/vuln_app/urls.py
```python
from django.urls import path
from .views import listview, register

urlpatterns = [
    path('register/', register, name='register'),
    path('', listview, name='home'),
]
```

src/vuln_app/views.py
```python
from django.shortcuts import render, redirect
from django.contrib import messages
from vuln_app.models import User

def _render_register_form(request, username='', email='', **kwargs):
    context = {
        'username': username,
        'email': email,
        **kwargs
    }
    return render(request, 'accounts/register.html', context)

def register(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        email = request.POST.get('email')
        password = request.POST.get('password')
        confirm_password = request.POST.get('confirmPassword')
        terms_agreed = request.POST.get('termsCheck') == 'on'

        storage = messages.get_messages(request)
        storage.used = True


        validation_passed = True
        
        if not terms_agreed:
            messages.error(request, 'You must agree to the terms and conditions')
            validation_passed = False

        if not username or not email or not password:
            messages.error(request, 'All fields are required')
            validation_passed = False

        if password != confirm_password:
            messages.error(request, 'Passwords do not match')
            validation_passed = False

        if User.objects.filter(username=username).exists():
            messages.error(request, 'Username already exists')
            validation_passed = False

        if User.objects.filter(email=email).exists():
            messages.error(request, 'Email already registered')
            validation_passed = False

        if not validation_passed:
            return _render_register_form(request, username, email)

        try:
            user = User.objects.create(
                username=username,
                email=email,
                password=password
            )
            messages.success(request, 'Registration successful! Please login.')
            return redirect('login')
            
        except Exception as e:
            messages.error(request, f'Error during registration: {str(e)}')
            return _render_register_form(request, username, email)

    return _render_register_form(request)
```

src/config/settings.py
```python
MESSAGE_STORAGE = 'django.contrib.messages.storage.session.SessionStorage'
```

src/vuln_app/templates/accounts/register.html
```html
{% extends '_base.html' %}
{% load static %}

{% block content %}
<div class="container py-5">
    <div class="row justify-content-center">
        <div class="col-md-8 col-lg-6">
            <div class="card shadow-sm border-0 rounded">
                <div class="card-body p-5">
                    <div class="text-center mb-4">
                        <h4 class="fw-bold">Create your account</h4>
                        <p class="text-muted">Join Bodegaa to get started</p>
                    </div>

                    <form method="POST" action="{% url 'register' %}">
                        {% csrf_token %}

                        <div class="mb-3">
                            <label for="username" class="form-label">Username</label>
                            <div class="input-group">
                                <span class="input-group-text bg-white"><i class="bi bi-person"></i></span>
                                <input type="text" class="form-control" id="username" name="username"
                                    placeholder="Enter your username">
                            </div>
                            {% if messages %}
                            {% for message in messages %}
                            {% if 'Username already exists' in message.message %}
                            <div class="text-danger small mt-2">{{ message }}</div>
                            {% endif %}
                            {% endfor %}
                            {% endif %}
                        </div>

                        <div class="mb-3">
                            <label for="email" class="form-label">Email Address</label>
                            <div class="input-group">
                                <span class="input-group-text bg-white"><i class="bi bi-envelope"></i></span>
                                <input type="email" class="form-control" id="email" name="email"
                                    placeholder="Enter your email">
                            </div>
                            {% if messages %}
                            {% for message in messages %}
                            {% if 'Email already registered' in message.message %}
                            <div class="text-danger small mt-2">{{ message }}</div>
                            {% endif %}
                            {% endfor %}
                            {% endif %}
                        </div>

                        <div class="mb-3">
                            <label for="password" class="form-label">Password</label>
                            <div class="input-group">
                                <span class="input-group-text bg-white"><i class="bi bi-lock"></i></span>
                                <input type="password" class="form-control" id="password" name="password"
                                    placeholder="Create password">
                                <button class="btn btn-outline-secondary" type="button" id="togglePassword">
                                    <i class="bi bi-eye"></i>
                                </button>
                            </div>
                            <div class="password-strength mt-2">
                                <div class="progress" style="height: 5px;">
                                    <div class="progress-bar" role="progressbar" style="width: 0%"></div>
                                </div>
                            </div>
                            <small class="text-muted">Minimum 8 characters with at least one number</small>
                        </div>

                        <div class="mb-4">
                            <label for="confirmPassword" class="form-label">Confirm Password</label>
                            <div class="input-group">
                                <span class="input-group-text bg-white"><i class="bi bi-lock"></i></span>
                                <input type="password" class="form-control" id="confirmPassword" name="confirmPassword"
                                    placeholder="Confirm password">
                                <button class="btn btn-outline-secondary" type="button" id="toggleConfirmPassword">
                                    <i class="bi bi-eye"></i>
                                </button>
                            </div>

                            {% if messages %}
                            {% for message in messages %}
                            {% if 'Passwords do not match' in message.message %}
                            <div class="text-danger small mt-2">{{ message }}</div>
                            {% endif %}
                            {% endfor %}
                            {% endif %}
                        </div>

                        <div class="form-check mb-4">
                            <input class="form-check-input" type="checkbox" id="termsCheck" name="termsCheck">
                            <label class="form-check-label small text-muted" for="termsCheck">
                                I agree to Bodegaa's <a href="#" class="text-decoration-none text-success">Terms of
                                    Service</a> and <a href="#" class="text-decoration-none text-success">Privacy
                                    Policy</a>
                            </label>
                            {% if messages %}
                            {% for message in messages %}
                            {% if 'terms and conditions' in message.message %}
                            <div class="text-danger small mt-1">{{ message }}</div>
                            {% endif %}
                            {% endfor %}
                            {% endif %}
                        </div>

                        <button type="submit" class="btn btn-success btn-lg py-3 px-4 text-uppercase w-100">Create
                            Account</button>

                        <div class="text-center mt-4">
                            <p class="text-muted">Already have an account? <a href="login.html"
                                    class="text-decoration-none text-success fw-bold">Sign in</a></p>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}

{% block extra_js %}
<script>
    // Toggle password visibility
    document.getElementById('togglePassword').addEventListener('click', function () {
        const password = document.getElementById('password');
        const icon = this.querySelector('i');
        const type = password.getAttribute('type') === 'password' ? 'text' : 'password';
        password.setAttribute('type', type);
        icon.classList.toggle('bi-eye');
        icon.classList.toggle('bi-eye-slash');
    });

    // Toggle confirm password visibility
    document.getElementById('toggleConfirmPassword').addEventListener('click', function () {
        const confirmPassword = document.getElementById('confirmPassword');
        const icon = this.querySelector('i');
        const type = confirmPassword.getAttribute('type') === 'password' ? 'text' : 'password';
        confirmPassword.setAttribute('type', type);
        icon.classList.toggle('bi-eye');
        icon.classList.toggle('bi-eye-slash');
    });

    // Password strength indicator
    document.getElementById('password').addEventListener('input', function () {
        const strengthBar = document.querySelector('.progress-bar');
        const strength = calculatePasswordStrength(this.value);
        strengthBar.style.width = strength + '%';

        // Update color based on strength
        strengthBar.className = 'progress-bar'; // Reset classes
        if (strength < 30) {
            strengthBar.classList.add('bg-danger');
        } else if (strength < 70) {
            strengthBar.classList.add('bg-warning');
        } else {
            strengthBar.classList.add('bg-success');
        }
    });

    function calculatePasswordStrength(password) {
        let strength = 0;

        // Length check
        if (password.length > 0) strength += 10;
        if (password.length >= 8) strength += 30;

        // Complexity checks
        if (/[A-Z]/.test(password)) strength += 20;
        if (/[0-9]/.test(password)) strength += 20;
        if (/[^A-Za-z0-9]/.test(password)) strength += 20;

        return Math.min(strength, 100); // Cap at 100%
    }
</script>
{% endblock %}
```

# 5. login view

src/vuln_app/urls.py
```python
from django.urls import path
from .views import listview, register, login

urlpatterns = [
    path('login/', login, name='login'),
    path('register/', register, name='register'),
    path('', listview, name='home'),
]
```

src/vuln_app/views.py
```python
from django.db import connection

def login(request):
    error = None  
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')

        with connection.cursor() as cursor:
            cursor.execute(f"SELECT * FROM vuln_app_user WHERE username='{username}' AND password='{password}'")
            user = cursor.fetchone()

            if user:
        
                request.session['user_id'] = user[0]
                request.session['username'] = user[3]
                return redirect('home')
            else:
                error = "Invalid username or password"

    return render(request, 'accounts/login.html', {'error': error})
```



src/vuln_app/templates/accounts/login.html
```html
{% extends '_base.html' %}
{% load static %}

{% block title %}Login - Bodegaa{% endblock %}

{% block content %}
<div class="container py-5">
    <div class="row justify-content-center">
        <div class="col-md-8 col-lg-6">
            <div class="card shadow-sm border-0 rounded">
                <div class="card-body p-5">
                    <div class="text-center mb-4">
                        <h4 class="fw-bold">Welcome back</h4>
                        <p class="text-muted">Sign in to your Bodegaa account</p>
                    </div>

                    <form method="POST" action="{% url 'login' %}">
                        {% csrf_token %}
                        <div class="mb-3">
                            <label for="loginEmail" class="form-label">Username</label>
                            <div class="input-group">
                                <span class="input-group-text bg-white"><i class="bi bi-person"></i></span>
                                <input type="text" class="form-control" id="loginEmail" name="username" placeholder="Enter username">
                            </div>
                        </div>

                        <div class="mb-3">
                            <label for="loginPassword" class="form-label">Password</label>
                            <div class="input-group">
                                <span class="input-group-text bg-white"><i class="bi bi-lock"></i></span>
                                <input type="password" class="form-control" id="loginPassword" name="password"
                                    placeholder="Enter password">
                                <button class="btn btn-outline-secondary" type="button" id="toggleLoginPassword">
                                    <i class="bi bi-eye"></i>
                                </button>
                            </div>
                        </div>

                        {% if error %}
                        <div class="alert alert-danger mb-3">{{ error }}</div>
                        {% endif %}

                        <div class="d-flex justify-content-between align-items-center mb-4">
                            <div class="form-check">
                                <input class="form-check-input" type="checkbox" id="rememberMe">
                                <label class="form-check-label small text-muted" for="rememberMe">
                                    Remember me
                                </label>
                            </div>
                            <a href="forgot-password.html" class="small text-decoration-none text-success">Forgot
                                password?</a>
                        </div>

                        <button type="submit" class="btn btn-success btn-lg py-3 px-4 text-uppercase w-100">Sign
                            In</button>

                        <div class="text-center mt-4">
                            <p class="text-muted">Don't have an account? <a href="register.html"
                                    class="text-decoration-none text-success fw-bold">Sign up</a></p>
                        </div>

                        <div class="d-flex align-items-center my-4">
                            <hr class="flex-grow-1">
                            <span class="mx-3 text-muted small">OR</span>
                            <hr class="flex-grow-1">
                        </div>

                        <div class="d-grid gap-3">
                            <button type="button" class="btn btn-outline-secondary py-2 text-uppercase">
                                <i class="bi bi-google me-2"></i> Continue with Google
                            </button>
                            <button type="button" class="btn btn-outline-secondary py-2 text-uppercase">
                                <i class="bi bi-facebook me-2"></i> Continue with Facebook
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}

{% block extra_js %}
<script>
    // Toggle login password visibility
    document.getElementById('toggleLoginPassword').addEventListener('click', function () {
        const password = document.getElementById('loginPassword');
        const icon = this.querySelector('i');
        const type = password.getAttribute('type') === 'password' ? 'text' : 'password';
        password.setAttribute('type', type);
        icon.classList.toggle('bi-eye');
        icon.classList.toggle('bi-eye-slash');
    });
</script>
{% endblock %}
```

# 5. logout view

src/vuln_app/urls.py
```python
from django.urls import path
from .views import listview, register, login, logout

urlpatterns = [
    path('login/', login, name='login'),
    path('register/', register, name='register'),
    path('logout/', logout, name='logout'),
    path('', listview, name='home'),
]
```

src/vuln_app/views.py
```python
def logout(request):
    request.session.flush()  
    return redirect('home') 
```




