---
title: python+django+docker:创建+部署
date: 2018-06-19 20:29:58
categories: [python]
tags: [python, dgango, docker]
---
利用`python+django+docker`快速搭建一个项目
<!-- more -->

### 环境配置

- `python`：`version-3.6.3`

  ```zsh
  # 检查python版本
  $ python --version
  Python 2.7.10	# 系统已经装了，但是更新版本敲麻烦的，我们用brew重装吧
  # brew install python会安装2.7的版本，我们用下面的命令
  $ brew install python3	# 这样之后命令行就要用'python3'才行了
  ```

  > 如果遇到`brew`的更新，就像这样→`Updating Homebrew...`，可以`Ctrl+c`跳过

- `django`：`version-2.0.6`

  ```zsh
  # python3 >= 3.4 已经包含了pip，可以直接使用
  $ pip install django
  ```

- `docker`

  

### 构建一个项目

#### 确认`django`版本

在你的终端打开`python`

```zsh
$ python3	# 重要！之后所有的 python3 指令，会使用 python 代替，如有必要，自行替换
```

然后在`python`命令行界面输入

```python
>>> import django
>>> print(django.get_version())
2.0.6
```

#### 新建一个项目

在你的终端输入下面的命令

```zsh
$ django-admin startproject mysite	# 项目名自取，只允许数字、字母和'_'
```

新建的项目的结构看起来像这样：

```zsh
mysite/
    manage.py	# 与 django 项目进行交互的文件，我们会用它来启动应用
    mysite/		# 主要目录，包含配置文件
        __init__.py
        settings.py	# 重要，配置项，包括中间件、模板、数据库连接等
        urls.py	# 路径分发的配置
        wsgi.py	# WSGI 的 entry-point 暂不管
```

现在，你可以尝试启动这个项目了，不需要任何页面和接口或者是模型。

执行下面的命令：

```zsh
$ python manage.py runserver
```

你会看到：

```
June 15, 2018 - 15:50:53
Django version 2.0, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

点击 [http://127.0.0.1:8000/](http://127.0.0.1:8000/) 打开，看到一个小火箭起飞的页面，就说明应用已经启动。



#### 新建一个模块

`django`建议一个项目按照功能划分模块

让我们开始新建一个模块

```zsh
$ python manage.py startapp polls	# 负责 投票'poll' 功能的模块
```

新的模块看起像这样：

```zsh
polls/
    __init__.py
    admin.py	# 基于 django 管理模型的选项(可选)
    apps.py		# 注册应用名
    migrations/		# 数据库迁移的自动构建文件
        __init__.py
    models.py	# 数据模型
    tests.py	# 测试代码
    views.py	# 视图、接口
```

如果要开始写你的第一个接口，请先在`polls/`下新建一个`urls.py`文件

```zsh
polls/
    ...		# 同上
    urls.py	# 配置 API 路径
```

在 views.py 中添加 index 方法

```python
# polls/views.py
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

在`urls.py`中加入下面的代码

```python
# polls/urls.py
from django.urls import path

from . import views	# 当前目录下的 views.py ，提供 API 方法

urlpatterns = [
    path('', views.index, name='index'),	# 在 views.py 中添加的 index 方法
]
```

这样`polls`项目的视图还依然不在`mysite`的管理中，需要在`mysite`目录（里面的那个）下的`urls.py`中添加

```python
# mysite/urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),	# 添加的 poll 模块的 url
    path('admin/', admin.site.urls),	# django 的默认管理页面
]
# path() 可以有4个参数，依次是：route, view, keywordsargs, name
```

现在，再次启动项目

```zsh
$ python manage.py runserver
```

---

如果出现

```
You have 14 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
```

这样的警告，可以暂时无视掉。

---

打开 [http://127.0.0.1:8000/polls/](http://127.0.0.1:8000/polls/) 验证我们的第一个接口是否返回了正确的信息。

#### `django`数据库 API

> 本篇只讨论`django`与`mysql`的集成

##### 连接数据库

`dejango`自带了`sqlite3`数据库引擎

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

要改成我们使用的`mysql`数据库

需要在`mysite/settings.py`中修改为

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',	# 主要就是这个
        'NAME': 'name',
        'USER': 'user',
        'PASSWORD': 'pwd',
        'HOST': 'host',
        'PORT': 'port',
        'init_command': 'SET default_storage_engine=INNODB',	# 配置引擎
    }
}
```

安装`mysql`的引擎

```zsh
$ pip install pymysql
```

在`mysite/__init__.py`中添加

```python
# mysite/__init__.py
import pymysql
pymysql.install_as_MySQLdb()
```

---

___备注：___因为出现了

```
django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module.
Did you install mysqlclient?
```

这样的错误信息而去安装`mysqlclient`的肯定不止我一个……

至于为什么不这么做，参见一下这篇文章：[centos下安装python3.x+django的mysql驱动](https://imshusheng.com/python/216.html)

里面提到，`mysql-python`也是没用的

---

确保数据库连接没问题后，先把`django`自带的在`settings.py`中包含的一堆模块跑起来

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

执行

```zsh
$ python manager.py migrate
```

这会在你的数据库中创建若干数量的表，用于用户、组以及权限的管理

这时运行项目，就不会再报警了

```zsh
$ python manage.py runserver
```

这时打开 [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/) ，会有一个`Django administration`的管理员登录界面

![Django administration](/images/django-images/1529407652189.jpg)

我们来创建一个用户

```zsh
$ python manage.py createsuperuser
```

然后依次填入

```zsh
Username (leave blank to use '系统用户名'): admin	# 就叫 admin 吧
Email address:	# 可不填
Password:	# 自填，不可见
Password (again):	# 自填，不可见
Superuser created successfully.
```

现在可以登录了，不过只能管理用户和权限而已，暂且不管。

---

##### 创建模型

在`models.py`中创建模型

```python
# polls/models.py
from django.db import models

class Question(models.Model):	# 对于投票，需要一些问题
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):		# 还需要每个问题有一些选择
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

详细的`Model Fields`可以参见文档：[Model field reference](https://docs.djangoproject.com/en/2.0/ref/models/fields/#django.db.models.Field)

##### 激活模型

首先需要将我们的`polls`纳入管理，在`settings.py`中添加

```python
# mysite/settings.py
INSTALLED_APPS = [
    'polls.apps.PollsConfig',	# 纳入 INSTALLED_APPS
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

这样`migrate`会将我们的`polls`中的模型也纳入管理对象中

在终端执行

```zsh
$ python3 manage.py makemigrations polls	# polls 指定模块，可不加，对应全部模块
Migrations for 'polls':
  polls/migrations/0001_initial.py	# 这是生成的用于数据迁移的文件
    - Create model Choice
    - Create model Question
    - Add field question to choice
```

我们看一下，生成的文件

```python
# polls/migrations/0001_initial.py
# Generated by Django 2.0.6 on 2018-06-19 11:47
# 排版需要，已简化部分代码
from django.db import migrations, models
import django.db.models.deletion

class Migration(migrations.Migration):

    initial = True

    dependencies = [
    ]

    operations = [
        migrations.CreateModel(
            name='Choice',
            fields=[
                ...
            ],
        ),
        migrations.CreateModel(
            name='Question',
            fields=[
                ...
            ],
        ),
        migrations.AddField(
            model_name='choice',
            name='question',
            field=models.ForeignKey(on_delete=django.db.models.deletion.CASCADE, to='polls.Question'),
        ),
    ]
```

可以理解为使用`migration`的`CreateModel()`创建模型

怎么创建的呢？

接下来执行

```zsh
$ python manage.py sqlmigrate polls 0001	# 0001 是上面生成的文件的代号
```

输出

```sql
BEGIN;
--
-- Create model Choice
--
CREATE TABLE "polls_choice" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "choice_text" varchar(200) NOT NULL, "votes" integer NOT NULL);
--
-- Create model Question
--
CREATE TABLE "polls_question" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "question_text" varchar(200) NOT NULL, "pub_date" datetime NOT NULL);
--
-- Add field question to choice
--
ALTER TABLE "polls_choice" RENAME TO "polls_choice__old";
CREATE TABLE "polls_choice" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "choice_text" varchar(200) NOT NULL, "votes" integer NOT NULL, "question_id" integer NOT NULL REFERENCES "polls_question" ("id") DEFERRABLE INITIALLY DEFERRED);
INSERT INTO "polls_choice" ("id", "choice_text", "votes", "question_id") SELECT "id", "choice_text", "votes", NULL FROM "polls_choice__old";
DROP TABLE "polls_choice__old";
CREATE INDEX "polls_choice_question_id_c5b4b260" ON "polls_choice" ("question_id");
COMMIT;
```

实质上就是生成了一堆`sql`用于创建我们的模型对应的表结构

上面的内容是预览，要想让`sql`生效

执行

```zsh
$ python manage.py migrate	# 正式执行，这个命令会操作数据库
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, polls, sessions
Running migrations:
  Applying polls.0001_initial... OK
```

这时，数据库会多出两个表`Question`和`Choice`，`django`模型和数据库模型的映射就正式建立了

后面对`models.py`的修改，可以通过

```zsh
$ python3 manage.py makemigrations
$ python manage.py migrate
```

来更新数据库模型

##### 管理模型

> 将模型纳入`admin`模块进行界面化管理

首先，在`polls/admin.py`中添加

```python
# polls/admin.py
from django.contrib import admin
from .models import Question	# 需要纳入管理的 models

# Register your models here.
admin.site.register(Question)
```

重启服务，登录`Django administration`

![model manage](/images/django-images/1529411176747.jpg)

就可以管理`Question`了，很棒吧！

### `Docker`部署项目

#### `Dockerfile`

在项目根目录下新建一个`Dockerfile`：

```dockerfile
# 以`python`为基础镜像
FROM python:3
# 暴露容器端口的声明
EXPOSE 8000
# 新建一个文件路径作为程序包工作路径
RUN mkdir /app
WORKDIR /app
# 复制项目内容到工作目录
COPY . /app
# 执行启动程序
CMD python /app/manage.py runserver 0.0.0.0:8000
```

这时，你的项目结构应该是这样的：

```zsh
mysite/
	Dockerfile
	manage.py
	mysite/
	polls/
```

> 以下内容参考： [Dockerfile 指令详解](https://yeasy.gitbooks.io/docker_practice/content/image/dockerfile/)

简单解释一下上面的命令：

```dockerfile
FROM python:3
```

`FROM`用来指定基础镜像，也就是我们这个`docker image`运行的基础环境。

在 [Docker Store](https://store.docker.com/) 上有非常多的高质量的官方镜像，有可以直接拿来使用的服务类的镜像，如 [`nginx`](https://store.docker.com/images/nginx/)、[`redis`](https://store.docker.com/images/redis/)、[`mongo`](https://store.docker.com/images/mongo/)、[`mysql`](https://store.docker.com/images/mysql/)、[`httpd`](https://store.docker.com/images/httpd/)、[`php`](https://store.docker.com/images/php/)、[`tomcat`](https://store.docker.com/images/tomcat/) 等；也有一些方便开发、构建、运行各种语言应用的镜像，如 [`node`](https://store.docker.com/images/node)、[`openjdk`](https://store.docker.com/images/openjdk/)、[`python`](https://store.docker.com/images/python/)、[`ruby`](https://store.docker.com/images/ruby/)、[`golang`](https://store.docker.com/images/golang/) 等。可以在其中寻找一个最符合我们最终目标的镜像为基础镜像进行定制。

如果没有合适的，需要自己搭环境的，可以选择一些更为基础的基础镜像，如 [`ubuntu`](https://store.docker.com/images/ubuntu/)、[`debian`](https://store.docker.com/images/debian/)、[`centos`](https://store.docker.com/images/centos/)、[`fedora`](https://store.docker.com/images/fedora/)、[`alpine`](https://store.docker.com/images/alpine/) 等，这些操作系统的软件库为我们提供了更广阔的扩展空间。

```dockerfile
EXPOSE 8000
```

`EXPOSE` 指令是声明运行时容器提供服务端口，这只是一个声明，在运行时并不会因为这个声明应用就会开启这个端口的服务。在 Dockerfile 中写入这样的声明有两个好处，一个是帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射；另一个用处则是在运行时使用随机端口映射时，也就是 `docker run -P`时，会自动随机映射 `EXPOSE` 的端口。

```dockerfile
RUN mkdir /app
WORKDIR /app
```

`RUN`指令会在镜像中执行后面的命令，这里是创建了一个目录

`WORKDIR` 指令指定工作目录，这很重要，在`Docker`镜像构建过程中，每一行`DOCKER`指令都会构建一层镜像，所以每个指令的工作目录之间是不同步的，如：

```dockerfile
RUN cd /app
RUN echo "hello" > world.txt
```

这两行命令式是无法在`/app`下创建`world.txt`文件的。

使用 `WORKDIR` 指令指定工作目录后各层的当前目录就被改为指定的目录，如该目录不存在，`WORKDIR` 会帮你建立目录。

```dockerfile
COPY . /app
```

`COPY`的目的就是单纯的复制文件，从宿主机`[源路径]`到镜像容器指定的`[目标路径]`。具体参见：[COPY 复制文件](https://yeasy.gitbooks.io/docker_practice/content/image/dockerfile/copy.html)

```dockerfile
CMD python /app/manage.py runserver 0.0.0.0:8000
```

`CMD`是容器启动命令，相当于在执行`docker run`时容器内执行的命令，这和`RUN`不同，`RUN`是基于镜像执行的构建，对镜像本身有影响，而`CMD`是给容器留了个__启动脚本__，作为`docker run`命令的执行脚本。

此外，`CMD`必须确保程序前台运行，不能以后台程序运行，具体参见：[CMD 容器启动命令](https://yeasy.gitbooks.io/docker_practice/content/image/dockerfile/cmd.html)

#### 构建镜像<span id="build"></span>

首先，确保你的机器已经安装了`Docker`并已成功运行。

在终端输入

```zsh
$ docker --help

Usage:	docker COMMAND

A self-sufficient runtime for containers

Options:
	......
Commands:
    attach      Attach local standard input, output, and error streams to a running container
>   build       Build an image from a Dockerfile	# 构建命令
    ......
```

然后继续输入

```zsh
$ docker build --help

Usage:	docker build [OPTIONS] PATH | URL | -

Build an image from a Dockerfile

Options:
	......
```

简单构建一个镜像

```zsh
$ docker build -t mysite:v1 .	
# [OPTION]：`-t ...` 构建的镜像名和版本
# PATH：`.` 当前路径
```

这里一共是6行构建语句，所以一共需要构建6步：

```
Sending build context to Docker daemon  99.33kB
Step 1/6 : FROM python:3
 ---> a5b7afcfdcc8
Step 2/6 : EXPOSE 8000
 ---> Using cache
 ---> ef00ea9bf641
Step 3/6 : RUN mkdir /app
 ---> Using cache
 ---> b4ddfbffe92d
Step 4/6 : WORKDIR /app
 ---> Using cache
 ---> 9cddbea6f6d0
Step 5/6 : COPY . /app
 ---> Using cache
 ---> b81058a01129
Step 6/6 : CMD python /app/manage.py runserver 0.0.0.0:8000
 ---> Using cache
 ---> 59ce4e412e76
Successfully built 59ce4e412e76
Successfully tagged mysite:v1
```

注意，如果出现下面的提示，说明`Docker`未启动

```
ERRO[0000] failed to dial gRPC: cannot connect to the Docker daemon. Is 'docker daemon' running on this host?: dial unix /var/run/docker.sock: connect: no such file or directory
context canceled
```

构建成功后，查看镜像

```zsh
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mysite              v1                  59ce4e412e76        5 minutes ago       912MB
```

#### 部署

镜像的构建是很快的，所以比较便捷的一种部署方法是：将源码推到服务器上，直接在服务器上构建`Docker`镜像来启动。

通过`git`将代码`clone`到服务器，执行上一步 [构建镜像](#build) ，然后查看`docker run` 的文档

```zsh
$ docker run --help

Run a command in a new container

Options:
	......
```

现在用刚才构建的镜像来运行一个`docker`容器

```zsh
$ docker run -it -d --name mysite --rm mysite:v1
b4193156413493049eab55d71c88a379127c47577041c07ac22b553d40c66672	# 每个容器的ID都不一样
```

稍等一会儿，试试服务有没有启动成功：`http://你的服务器ip:8000`

