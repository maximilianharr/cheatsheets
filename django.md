# Django

[Django](https://www.djangoproject.com/) is a high-level Python Web framework that encourages rapid development and clean, pragmatic design. Built by experienced developers, it takes care of much of the hassle of Web development, so you can focus on writing your app without needing to reinvent the wheel. It’s free and open source.

--- 

## Django Admin
### Start New Project
```bash
django-admin startproject ${PROJECT_NAME}
```

---

## Django manage.py

### Add New App
```bash
python3 manage.py startapp ${APP_NAME}
```

### Migrate Server
```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

### Create New SuperUser
```bash
python3 manage.py createsuperuser
```