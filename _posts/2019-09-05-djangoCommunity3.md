---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기3
 [Django 관리자 도구]"
subtitle: "Django"
date: 2019-09-05 18:23:28
author: kwon
categories: django
---

저번 포스팅에서 Django에서 모델링과 데이터베이스 연동까지 진행을 해보았습니다. 이번 포스팅에서는 Django의 admin 기능을 사용하는 방법에 대해 알아보겠습니다.

community의 urls.py 파일에 들어가 보면 아래와 같이 프로젝트가 생성되면서 자동으로 작성된 코드가 있습니다.

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

위와 같이 기본적으로 admin은 정의되어 있는데 기본 주소 뒤에 admin을 붙여 장고의 관리자 도구를 사용할 수 있습니다.

한번 서버를 실행시킨 뒤 admin에 접속해 보도록 하겠습니다.

*C:\kwon\FastDjango\fcdjango_venv\Scripts\community> python manage.py runserver*

명령어를 입력하여 서버를 실행시킨 후

 http://127.0.0.1:8000/admin

 기본 주소 끝에 admin은 붙여 접속하면 아래와 같은 화면을 볼 수 있습니다.

 <div style="width: 350px; height: 250px;">
     <img src="https://kyu9341.github.io/assets/admin.png" style="width: 350px
     ; height: 250px;">
 </div>

 여기까지 완료되었다면 관리자 아이디를 생성해보도록 하겠습니다. 파이참의 터미널로 돌아와서 다음과 같은 명령어를 입력합니다.

 *C:\kwon\FastDjango\fcdjango_venv\Scripts\community> python manage.py createsuperuser*

명령어를 입력하게 되면 아래와 같이 아이디와 비밀번호를 설정해주시면 됩니다.

 <div style="width: 90%; height: 200px;">
     <img src="https://kyu9341.github.io/assets/djangoadmin.png" style="width: 90
     %; height: 200px;">
 </div>

설정이 완료되었다면 admin으로 가서 로그인을 해보겠습니다. 현재 장고 admin에 설정을 안해놓았기 아래와 같이 때문에 기본으로 생성되는 정보만 있을 것입니다.

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/admin1.png" style="width: 100%
    ; height: 200px;">
</div>

그럼 이제 User를 추가해 보도록 하겠습니다. user/admin.py로 이동하여 다음과 같은 코드를 작성해주도록 합니다.

```python
from django.contrib import admin
from .models import User

# Register your models here.

class UserAdmin(admin.ModelAdmin):
    list_display = ('username', 'password') # user list 사용자명과 비밀번호를 확인할 수 있도록 구성

admin.site.register(User, UserAdmin) # 등록
```

완료되었다면 user/models.py로 이동하여 다음과 같이 코드를 추가합니다.

```python
from django.db import models

# Create your models here.

class User(models.Model):
    username = models.CharField(max_length=50, verbose_name='사용자명')
    password = models.CharField(max_length=50, verbose_name='비밀번호')
    registered_dttm = models.DateTimeField(auto_now_add=True, verbose_name='등록시간')

    def __str__(self): # 클래스가 문자열로 변환될 때 사용되는 내장함수
        return self.username


    class Meta:
        db_table = 'Community_user'
        verbose_name = '커뮤니티 사용자'
        verbose_name_plural = '커뮤니티 사용자' # 복수형 표현도 설정
```

작성이 완료되었다면 다시 admin으로 이동해 확인해보겠습니다.

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/admin2.png" style="width: 100%
    ; height: 200px;">
</div>

admin에 user테이블이 추가가 된 것을 확인할 수 있습니다. 사용자를 하나 추가해보도록 하겠습니다.

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/admin3.png" style="width: 100%
    ; height: 200px;">
</div>

임의로 사용자명과 비밀번호를 입력한 후 save버튼을 눌러줍니다.

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/admin4.png" style="width: 100%
    ; height: 200px;">
</div>

위와 같이 사용자가 하나 추가된 것을 확인할 수 있습니다.

이렇게 데이터베이스에 직접 sql을 작성하지 않고도 Django의 관리자 도구를 통해 손쉽게 관리가 가능합니다.

이번 포스팅에서는 Django의 관리자도구를 사용하는 방법을 알아보았습니다.
