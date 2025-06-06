

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
<!DOCTYPE html>
<html lang="fa" dir="rtl">  <!-- For Persian/Farsi language -->
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}سایت من{% endblock %}</title>
    <!-- Bootstrap 5 RTL (for Persian) -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.rtl.min.css">
    {% block extra_css %}{% endblock %}
</head>
<body>


    <main class="container my-5">
        {% block content %}
        <!-- Main content will go here -->
        {% endblock %}
    </main>

    <footer class="bg-dark text-white py-3 mt-5">
        <div class="container text-center">
            <p>تمامی حقوق محفوظ است &copy; {% now "Y" %}</p>
        </div>
    </footer>

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    {% block extra_js %}{% endblock %}
</body>
</html>
```

templates/login
```html
{% extends '_base.html' %}

{% block title %}ورود به سیستم{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card">
            <div class="card-header bg-primary text-white">
                <h2 class="text-center">ورود به سیستم</h2>
            </div>
            <div class="card-body">
                <form method="POST">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="username" class="form-label">نام کاربری</label>
                        <input type="text" class="form-control" id="username" name="username" required>
                    </div>
                    <div class="mb-3">
                        <label for="password" class="form-label">رمز عبور</label>
                        <input type="password" class="form-control" id="password" name="password" required>
                    </div>
                    <button type="submit" class="btn btn-primary w-100">ورود</button>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

templates/login_success.html
```html
{% extends '_base.html' %}

{% block title %}ورود موفق{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card border-success">
            <div class="card-header bg-success text-white">
                <h2 class="text-center">ورود موفق</h2>
            </div>
            <div class="card-body text-center">
                <i class="bi bi-check-circle-fill text-success" style="font-size: 3rem;"></i>
                <h3 class="my-3">کاربر پیدا شد!</h3>
                <p class="text-muted">شما با موفقیت وارد سیستم شدید</p>
                <a href="/" class="btn btn-success">بازگشت به خانه</a>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

templates/login_failed.html
```html
{% extends '_base.html' %}

{% block title %}ورود ناموفق{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card border-danger">
            <div class="card-header bg-danger text-white">
                <h2 class="text-center">ورود ناموفق</h2>
            </div>
            <div class="card-body text-center">
                <i class="bi bi-x-circle-fill text-danger" style="font-size: 3rem;"></i>
                <h3 class="my-3">کاربر یافت نشد!</h3>
                <p class="text-muted">نام کاربری یا رمز عبور نادرست است</p>
                <a href="{% url 'login' %}" class="btn btn-danger">تلاش مجدد</a>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```
