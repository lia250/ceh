

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
