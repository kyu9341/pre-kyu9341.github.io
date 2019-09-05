---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기3
 [Django amdin]"
subtitle: "Django"
date: 2019-09-053 18:23:28
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

C:\kwon\FastDjango\fcdjango_venv\Scripts\community>python manage.py runserver

명령어를 입력하여 서버를 실행시킨 후

 http://127.0.0.1:8000/admin

 기본 주소 끝에 admin은 붙여 접속하면 아래와 같은 화면을 볼 수 있습니다.

 <div style="width: 300px; height: 300px;">
     <img src="https://kyu9341.github.io/assets/admin.png" style="width: 300px
     ; height: 300px;">
 </div>

 여기까지 완료되었다면 관리자 아이디를 생성해보도록 하겠습니다. 파이참의 터미널로 돌아와서 다음과 같은 명령어를 입력합니다.

 C:\kwon\FastDjango\fcdjango_venv\Scripts\community>python manage.py createsuperuser

명령어를 입력하게 되면 아래와 같이 아이디와 비밀번호를 설정해주시면 됩니다.

 <div style="width: 90%; height: 200px;">
     <img src="https://kyu9341.github.io/assets/djangoadmin.png" style="width: 90
     %; height: 200px;">
 </div>

설정이 완료되었다면 admin으로 가서 로그인을 해보겠습니다. 현재 장고 admin에 설정을 안해놓았기 아래와 같이 때문에 기본으로 생성되는 정보만 있을 것입니다.

<div style="width: 300px; height: 300px;">
    <img src="https://kyu9341.github.io/assets/admin1.png" style="width: 300px
    ; height: 300px;">
</div>


```python
from django.contrib import admin
from .models import User

# Register your models here.

class UserAdmin(admin.ModelAdmin):
    list_display = ('username', 'password') # user list 사용자명과 비밀번호를 확인할 수 있도록 구성

admin.site.register(User, UserAdmin) # 등록
```
