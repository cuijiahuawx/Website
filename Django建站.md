@def title = "Django"
@def tags = ["Tutorial"]

\toc
## 依赖
```
pip install django django-simpleui gunicorn psycopg2-binary
```

## Postgre数据库设置
```
#原文链接：https://sleeksoft.in/deploy-django-website-in-ubuntu-server-using-nginx-gunicorn-and-wsgi-with-postgres-db/
sudo -u postgres psql
CREATE DATABASE myweb;
CREATE USER user WITH PASSWORD 'password';
ALTER ROLE user SET client_encoding TO 'utf8';
ALTER ROLE user SET default_transaction_isolation TO 'read committed';
ALTER ROLE user SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE myweb TO user;
\q
```
### 更改数据目录
```
#原文链接：https://fitodic.github.io/how-to-change-postgresql-data-directory-on-linux
sudo su - postgres
SHOW config_file; # /etc/postgresql/12/main/postgresql.conf
SHOW data_directory; # /var/lib/postgresql/12/main
#关闭服务
systemctl stop postgresql.service
#创建新数据目录
mkdir /home/ubuntu/mystore/pgdata
#赋予权限
chown postgres:postgres /home/ubuntu/mystore/pgdata
chmod 700 /home/ubuntu/mystore/pgdata
#数据转移
rsync -av /var/lib/postgresql/12/main /home/ubuntu/mystore/pgdata/main
#更改设置
sudo vim /home/pgdata/data/postgresql.conf
data_directory = '/home/pgdata/data' 
#重启服务
systemctl daemon-reload
systemctl start postgresql.service
systemctl status postgresql.service
```

## 初始化Django工程
```
#建立Django项目
django-admin startproject myweb
cd myweb
#生成数据库迁移
python manage.py makemigrations
#迁移数据库
python manage.py migrate
#创建管理员
python manage.py createsuperuser
#创建App
python manage.py startapp blog
```
## 安装django-simpleui，美化后台管理页面
```
pip install django-simpleui
```
## Settings.py配置
```
ALLOWED_HOSTS = ['*', ]

INSTALLED_APPS = [
    'simpleui',                      #新加入
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'blog',                          #新加入
]

        'DIRS': [
            os.path.join(BASE_DIR, 'templates'),
            os.path.join(BASE_DIR, 'templates/blog'),
        ],

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'myweb',
        'USER': 'user',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORt': '',
    }

LANGUAGE_CODE = 'zh-hans'

TIME_ZONE = 'Asia/Shanghai'

USE_TZ = False

STATIC_ROOT = BASE_DIR / 'static'
#这个是设置静态文件夹目录的路径
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'static'),
)
#设置文件上传路径，图片上传、文件上传都会存放在此目录里
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

```
