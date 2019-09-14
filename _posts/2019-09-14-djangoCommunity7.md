---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기7
 [로그아웃 기능 및 ]"
subtitle: "Django"
date: 2019-09-14 10:11:19
author: kwon
categories: django
---
저번 포스팅에서는 간단한 로그인 기능까지 구현해 보았는데요. 이번에는 간단한 로그아웃 기능과 템플릿의 상속에 대해 알아보겠습니다.

먼저 간단하게 로그아웃 기능을 구현해보도록 하겠습니다.

user/views.py 로 이동하여 다음과 같이 로그아웃 함수를 추가해주도록 합니다.

```python
def logout(request):
    if request.session.get('user'): # user의 세션ID가 존재한다면
        del(request.session['user']) # 현재 세션ID를 지워주고

    return redirect('/') # 홈으로 리다이렉트 해줌
```
작성이 완료되었다면 urls.py 로 이동하여 url을 연결해주도록 합니다.

```python
from django.urls import path, include
from . import views

urlpatterns = [
    path('register/', views.register),
    path('login/', views.login),
    path('logout/', views.logout),
]
```
위와 같이 /user/logout 와 같은 경로로 접속시 logout함수를 호출할 수 있도록 작성해 줍니다. 이제 간단하게 확인을 해보도록 하겠습니다. 먼저 user2아이디로 로그인을 수행해보겠습니다.

<div style="width: 100%; height: 150px;">
    <img src="https://kyu9341.github.io/assets/django18.png" style="width: 100%
    ; height: 150px;">
</div>

로그인이 완료되고 개발자도구를 확인해보면 위와 같이 세션id가 발급되어 있는것을 확인할 수 있습니다. 이제 다음과 같이 url을  127.0.0.1:8000/user/logout 라고 입력해줍니다.

<div style="width: 350px; height: 200px;">
    <img src="https://kyu9341.github.io/assets/django20.png" style="width: 350px
    ; height: 200px;">
</div>


<div style="width: 100%; height: 150px;">
    <img src="https://kyu9341.github.io/assets/django19.png" style="width: 100%
    ; height: 150px;">
</div>

입력을 하고 이동해주면 다음과 같이 홈 화면으로 이동이 된 후 세션id가 삭제된 것을 확인할 수 있습니다. 이렇게 간단하게 로그아웃 기능을 구현해보았고 이제 템플릿의 상속에대해 알아보겠습니다.
