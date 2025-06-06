

```
git init # project-root, not src/
touch .gitignore # project-root, not src/
python -m venv venv (windows)
venv\Scripts\activate
```

```
cd src/
pip install django
django-admin startproject config .
python manage.py migrate
python manage.py createsuperuser

# cd ..
git status
git add .
git commit -m "initial commit" # Setting up project structure

pyton manage.py runserver
```

config/settings.py
```python
TEMPLATES = [
    {
        ...

        'DIRS': [BASE_DIR / 'templates'],

        ...

]
```

```
STATIC_URL = 'static/'
STATICFILES_DIRS = [BASE_DIR / "static",] # development

MEDIA_ROOT = '/media/'
MEDIA_URL = '/media/'
```


config/urls.py
```python
from django.urls import path, include

from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
  # path('', include('vuln_app.urls')),

] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```
