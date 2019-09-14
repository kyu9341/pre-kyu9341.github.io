---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기7
 [로그아웃 기능 및 템플릿 상속받기]"
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

현재까지 작성된 템플릿 파일인 login.html과 register.html을 보면 거의 유사한 형태로 작성되어 있는 것을 확인할 수 있습니다. 이제 이러한 부분을 조금 더 편리하게 작성하기 위하여 하나의 기준이 되는 템플릿 파일을 작성한 뒤 그 파일을 상속받아 필요한 부분만 추가로 작성할 수 있도록 구현보겠습니다.

우선 templates/user/에 base.html이라는 파일을 하나 생성해줍니다. 이 후 login.html파일의 코드를 복사하여 붙여준 뒤 container내부의 내용만 지워주고 다음과 같이 작성해줍니다.

```html
{% raw %}
<html>
<head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <link rel="stylesheet" href="/static/bootstrap.min.css"/>
    <script src="https://codpye.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
</head>
<body>
<div class="container">
    {% block contents %}
    {% endblock %}
</div>
</body>
</html>

{% endraw %}
```
{% raw %}
위와 같이 container내부에 다음과 같이 {% block contents %} {% endblock %} 라고 작성해주면 나중에 contents라는 블록에 코드를 추가해 상속받은 base.html 을 사용할 수 있습니다.

이제 login.html 로 이동하여 base.html을 상속받아 다시 작성해보도록 하겠습니다.

{% endraw %}
```html
{% raw %}

{% extends "./base.html" %}
<!--현재 디렉토리에 있는 base.html 파일을 상속받음 -->
{% block contents %}
<div class="container">
<div class="row mt-5">
    <div class="col-12 text-center">
        <h1>로그인</h1>
    </div>
</div>
<div class="row mt-5">
    <div class="col-12">
        {{ error }}
    </div>
</div>
<div class="row mt-5">
    <div class="col-12">
        <form method="POST" action=".">

          {% csrf_token %}  <!-- 장고에서 크로스 도메인을 막기 위해 암호화된 키를 검증-->

            <div class="form-group">
                <label for="username">사용자 이름</label>
                <input type="text"
                       class="form-control"
                       id="username"
                       placeholder="사용자 이름"
                       name="username"
                >
            </div>

            <div class="form-group">
                <label for="password">비밀번호</label>
                <input type="password"
                       class="form-control"
                       id="password"
                       placeholder="비밀번호"
                       name="password"
                >
            </div>

            <button type="submit" class="btn btn-primary">로그인</button>
        </form>
    </div>
</div>
</div>
{% endblock %}

{%endraw%}
```
{% raw %}
{% extends "base.html" %} 부분이 base.html을 상속받는 부분인데 이렇게 입력하니 인식하지 못하여 {% extends "./base.html" %} 와 같이 현재 디렉토리에 있는 base.html 이라고 지정을 해주니 인식이 잘됩니다.

<div style="width: 100%; height: 150px;">
    <img src="https://kyu9341.github.io/assets/django21.png" style="width: 100%
    ; height: 150px;">
</div>

위와 같이 정상적으로 적용이 된 것을 확인할 수 있습니다.

이제 같은 방법으로 register.html 도 base.html을 상속받아 다시 작성해보겠습니다.
{% endraw %}
```html
{% raw %}
{% extends "./base.html" %}
<!--현재 디렉토리에 있는 base.html 파일을 상속받음 -->
{% block contents %}
<div class="container">

<div class="row mt-5">
<div class="col-12 text-center">
    <h1>회원가입</h1>
</div>
</div>
<div class="row mt-5">
<div class="col-12">
    {{ error }}
</div>
</div>
<div class="row mt-5">
<div class="col-12">
    <form method="POST" action=".">

      {% csrf_token %}  <!-- 장고에서 크로스 도메인을 막기 위해 암호화된 키를 검증-->

        <div class="form-group">
            <label for="username">사용자 이름</label>
            <input type="text"
                   class="form-control"
                   id="username"
                   placeholder="사용자 이름"
                   name="username"
            >
        </div>
        <div class="form-group">
            <label for="username">사용자 이메일</label>
            <input type="text"
                   class="form-control"
                   id="useremail"
                   placeholder="사용자 이메일"
                   name="useremail"
            >
        </div>
        <div class="form-group">
            <label for="password">비밀번호</label>
            <input type="password"
                   class="form-control"
                   id="password"
                   placeholder="비밀번호"
                   name="password"
            >
        </div>
        <div class="form-group">
            <label for="re-password">비밀번호 확인</label>
            <input type="password"
                   class="form-control"
                   id="re-password"
                   placeholder="비밀번호 확인"
                   name="re-password"
            >
        </div>
        <button type="submit" class="btn btn-primary">등록</button>
    </form>
</div>
</div>
</div>

</div>
{% raw %}
{% endblock %}

{% endraw %}
```
<div style="width: 100%; height: 150px;">
    <img src="https://kyu9341.github.io/assets/django22.png" style="width: 100%
    ; height: 150px;">
</div>

register.html도 마찬가지로 위와 같이 정상적으로 적용이 된 것을 확인할 수 있습니다.
