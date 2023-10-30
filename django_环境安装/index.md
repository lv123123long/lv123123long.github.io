# Django_环境安装


# djiango安装

## python虚拟环境

```
conda create --name my_env python=3.6.8

conda activate my_env
```

### conda换源

原来的源

![image-20231028002419158](assets/image-20231028002419158.png)

换源

寻找.condarc 文件，，上面图中有，如果没有，输入以下命令

```
conda config --set show_channel_urls yes
```

替换里面的内容

[anaconda | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors4.tuna.tsinghua.edu.cn/help/anaconda/)



* 清除索引缓存

```
conda clean -i
```



#### 参考

[Anaconda换源(Ubuntu) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/659667063#:~:text=1、查看目前的源 conda info 可以看到此时是国外源，为了下载加速，我们需要切换国内源，这里推荐清华源。 2、找到.condarc 查看%2Fhome%2Fusername,(你的用户名)下是否有.condarc文件 如果没有.condarc，命令行输入以下命令； conda config --set show_channel_urls yes)



## 安装djiango

```
pip install django==1.11.2
```

```
pip show django
```



## django的使用

*  创建项目目录

```
cd student_house && django-admin startproject student_sys
```

-- manage.py

--  student_sys

​	-- __init__.py

​	-- settings.py

​	-- urls.py

​	-- wsgi.py

* 创建App

```
python manage.py startapp student
```

-- student

​	-- admin.py

​	-- apps.py

​	-- __init__.py

​	-- migrations/

​	-- models.py

​	-- tests.py

​	-- views.py

## 首次运行django

### student App

* student/models.py:

```
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

from django.db import models


class Student(models.Model):
    SEX_ITEMS = [
        (1, '男'),
        (2, '女'),
        (0, '未知'),
    ]
    STATUS_ITEMS = [
        (0, '申请'),
        (1, '通过'),
        (2, '拒绝'),
    ]
    name = models.CharField(max_length=128, verbose_name="姓名")
    sex = models.IntegerField(choices=SEX_ITEMS, verbose_name="性别")
    profession = models.CharField(max_length=128, verbose_name="职业")
    email = models.EmailField(verbose_name="Email")
    qq = models.CharField(max_length=128, verbose_name="QQ")
    phone = models.CharField(max_length=128, verbose_name="电话")

    status = models.IntegerField(choices=STATUS_ITEMS, verbose_name="审核状态")

    created_time = models.DateTimeField(auto_now_add=True, editable=False, verbose_name="创建时间")

    def __unicode__(self):
        return '<Student: {}>'.format(self.name)

    class Meta:
        verbose_name = verbose_name_plural = "学员信息"
```

* admin.py:

```
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

from django.contrib import admin

from .models import Student


class StudentAdmin(admin.ModelAdmin):
    list_display = ('id', 'name', 'sex', 'profession', 'email', 'qq', 'phone', 'status', 'created_time')
    list_filter = ('sex', 'status', 'created_time')
    search_fields = ('name', 'profession')
    fieldsets = (
        (None, {
         'fields': (
                 'name',
                 ('sex', 'profession'),
                 ('email', 'qq', 'phone'),
                 'status',
                 )
         }),
    )


admin.site.register(Student, StudentAdmin)
```

接下来我们把这个`student`app放到settings.py中。

我们只需要在INSTALLED_APPS配置的最后，或者最前面增加'student'即可:

settings.py文件:

```
INSTALLED_APPS = [
    'student',

    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

为了方便使用，我们把允许的hosts改成通配符

```
ALLOWED_HOSTS = ['*']
```

如果是IP的话，一个元素一个IP

**创建表和超级用户**

- `python manage.py makemigrations` 创建迁移文件
- `python manage.py migrate` 创建表
- `python manage.py createsuperuser` 根据提示，输出用户名，邮箱，密码

启动项目：python manage.py runserver 192.168.124.222:8000




