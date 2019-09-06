---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기4
 [템플릿(Template)과 뷰(view)를 이용한 회원가입 기능 구현]"
subtitle: "Django"
date: 2019-09-06 15:53:45
author: kwon
categories: django
---
저번 포스팅에서는 Django의 관리자도구를 사용하는 방법을 알아보았습니다.

이번 포스팅에서는 템플릿(Template)과 뷰(view)를 활용하여 회원가입 기능을 구현해보도록 하겠습니다.

먼저 템플릿에대해 간단히 알아보고 들어가겠습니다.

**템플릿(Template)** 은 View로부터 전달된 데이타를 템플릿에 적용하여 Dynamic 한 웹페이지를 만드는데 사용됩니다.

Template은 HTML 파일로서 Django App 폴더 밑에 "templates" 라는 서브폴더를 만들고 그 안에 템플릿 파일(.html)을 생성하여 사용합니다. 이는 단일 App이거나 동일 템플릿명이 없는 경우 사용할 수 있습니다.

하지만, Django 개발 가이드라인은 "App폴더/templates/App명/템플릿파일" 처럼, 각 App 폴더 밑에 templates 서브폴더를 만들고 다시 그 안에 App명을 사용하여 서브폴더를 만든 후 템플릿 파일을 그 안에 넣기를 권장하고 있습니다 (예: /home/templates/home/index.html ).

이는 만약 복수의 App들이 동일한 이름의 템플릿을 가진 경우, View에서 잘못된 템플릿을 가져올 수 있기 때문인데, 예를 들어, App1에 create.html이 있고, App2에 동일한 create.html 템플릿이 있는 경우, App2의 View에서 create.html를 지정하면, 처음 App1의 create.html을 사용하게 됩니다. 이는 템플릿을 찾을 때 자신의 App 내의 템플릿을 먼저 찾는 것이 아니라, 전체 App들의 템플릿 폴더들을 처음부터 순서대로 찾기 때문입니다. View에서 "App2/create.html" 과 같이 템플릿명을 지정하면 이런 혼동은 없어지게 됩니다.

따라서 먼저 이전에 만들어두었던 user의 templates폴더 아래에 user이라는 이름의 폴더를 하나 더 생성하고 그 안에 register.html이라는 html5파일을 하나 생성해보도록 하겠습니다.

 <div style="width: 250px; height: 350px;">
     <img src="https://kyu9341.github.io/assets/django8.png" style="width: 250px
     ; height: 350px;">
 </div>

위와 같이 완료되었다면 register.html을 작성해보도록 하겠습니다.
```html
<html>
<head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
</head>
<body>
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
              {% raw %}
              {% csrf_token %} <!-- 장고에서 크로스 도메인을 막기 위해 암호화된 키를 검증함
꼭 입력해주어야함(안쓰면 오류)
 -->
              {%endraw%}
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


</body>
</html>

```
부트스트랩cdn을 이용하여 [이 곳](https://getbootstrap.com/docs/4.3/getting-started/introduction/)에서 css, js부분을 복사하여 head에 붙여주었고 스타터 템플릿의 mata태그도 가져와 작성하였습니다. component의 forms에서도 기본 from을 가져와 우리가 사용할 형식에 맞게 변경하였습니다.

이제 한번 템플릿을 app과 연결해보도록 하겠습니다.

먼저 user/views.py로 이동하여 다음과 같이 코드를 작성해줍니다.
```python
from django.shortcuts import render

# Create your views here.

def register(request):
    return render(request, 'user/register.html')
```
일반적으로 View는 필요한 데이타를 모델 (혹은 외부)에서 가져와서 적절히 가공하여 웹 페이지 결과를 만들도록 컨트롤하는 역활을 합니다. 지금은 간단하게 request를 받으면 템플릿파일에 연결해주는 역할만 수행하도록 하였습니다.


 community/urls.py로 이동하여 다음과 같이 path를 추가해주겠습니다.

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('user/', include('user.urls'))
]
```
이후에 user폴더에서 urls.py라는 파이썬 파일을 만들고 다음과 같이 작성해봅니다.

```python
from django.urls import path, include
from . import views

urlpatterns = [
    path('register/', views.register),
]
```

프로젝트 url의 'user/' 를 include('user.urls')에 매핑 하게 되고 user.urls를 따라가면  path('register/', views.register)가 있으므로 register은 views.register함수에 연결되어 127.0.0.1:8000/user/register 라고 입력해주게 되면 templates/user에 작성한 register.html파일에 접근하게 됩니다.

여기까지 완료되었다면 이제 *python manage.py runserver* 명령어를 통해 서버를 실행시킨 이후에
*127.0.0.1:8000/user/register* 로 접속하여 확인해보도록 합니다.

<div style="width: 100%; height: 350px;">
    <img src="https://kyu9341.github.io/assets/django9.png" style="width: 100%
    ; height: 350px;">
</div>

위와 같은 화면이 나왔다면 이제 회원정보를 입력받으면 데이터베이스에 정보가 입력되도록 하기 위해서 views.py로 이동하여 코드를 작성해보겠습니다.

이 때, view.py에서 url을 입력하여 페이지를 불러오는 경우(GET방식)와 페이지에서 등록버튼을 누름으로써 페이지에 접근하는 경우(POST방식)를 구분해주어야 합니다. POST의 경우 데이터가 들어왔을 때 처리를 해주어야 함.

```python
from django.shortcuts import render
from .models import User

# Create your views here.

def register(request):
    if request.method == 'GET':
        return render(request, 'user/register.html')
    elif request.method == 'POST':
        username = request.POST['username']  # 템플릿에서 입력한 name필드에 있는 값을 키값으로 받아옴
        password = request.POST['password']
        re_password = request.POST['re-password']

        user = User( # 모델에서 생성한 클래스를 가져와 객체를 생성
            username=username,
            password=password
        )

        user.save() # 데이터베이스에 저장

        return render(request, 'user/register.html')

```
입력받은 값으로 객체가 생성이되고 실제로 데이터베이스에 저장이 되는지 먼저 간단히 확인해보도록 하겠습니다.

<div style="width: 100%; height:350px;">
    <img src="https://kyu9341.github.io/assets/django10.png" style="width: 100%
    ; height: 350px;">
</div>

이렇게 정보를 입력하고 등록 버튼을 누르면 다시 같은 페이지가 표시됩니다 이유는 POST로 들어와 처리를 해준 뒤 다시 register.html을 리턴받았기 때문이죠. 이 부분은 잠시 후 수정해보도록 하겠습니다.

mysql에서 확인할 수도 있지만 이번에는 admin으로 이동하여 정상적으로 저장이 완료되었는지 확인해보면

<div style="width: 100%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/admin5.png" style="width: 100%
    ; height: 300px;">
</div>

위와 같이 정상적으로 추가가 완료된 것을 확인할 수 있습니다.

하지만 사용자의 비밀번호가 위와 같이 관리자에게 노출된다면 안되겠죠? 장고에서는 기본적으로 비밀번호를 암호화해서 저장할 수 있는 기능을 제공해줍니다. 이제 비밀번호를 암호화해서 저장되도록 수정해보겠습니다.

또한 비밀번호와 비밀번호 확인에 입력된 값이 다를 경우나 값이 입력되지 않은 경우에 대해 예외처리도 수행해보도록 하겠습니다. user/views.py로 이동하여 다음과 같이 변경하여 주도록 합니다.

```python
from django.shortcuts import render
from .models import User
from django.http import HttpResponse
from django.contrib.auth.hashers import make_password

def register(request):
    if request.method == 'GET':
        return render(request, 'user/register.html')
    elif request.method == 'POST':
        username = request.POST.get('username', None) # 템플릿에서 입력한 name필드에 있는 값을 키값으로 받아옴
        password = request.POST.get('password', None) # 받아온 키값에 값이 없는경우 None값으로 기본값으로 지정
        re_password = request.POST.get('re-password', None)

        res_data = {} # 응답 메세지를 담을 변수(딕셔너리)

        if not (username and password and re_password):
            res_data['error'] = '모든 값을 입력해야 합니다.'
        elif password != re_password:
            res_data['error'] = '비밀번호가 다릅니다.'
        else:
            user = User( # 모델에서 생성한 클래스를 가져와 객체를 생성
                username=username,
                password=make_password(password)  # 비밀번호를 암호화하여 저장
            )

            user.save() # 데이터베이스에 저장

        return render(request, 'user/register.html', res_data) # res_data가 html코드로 전달이 됨

```
이제 회원가입 화면에서 비밀번호를 다르게 입력하였다면

<div style="width: 100%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/django12.png" style="width: 100%
    ; height: 300px;">
</div>

위와 같이 상단에 메시지가 표시되고 빈칸이 있는 상태로 등록버튼을 눌렀다면

<div style="width: 100%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/django11.png" style="width: 100%
    ; height: 300px;">
</div>

위와 같이 메시지가 표시됩니다.

또한 회원가입을 정상적으로 진행하여 완료한 후 admin에서 확인을 해보면

<div style="width: 100%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/admin6.png" style="width: 100%
    ; height: 300px;">
</div>

위의 user2의 비밀번호처럼 암호화되어 저장된 비밀번호를 확인할 수 있습니다. 여기까지 장고의 템플릿과 뷰를 통해 간단한 회원가입 기능까지 구현해 보았습니다.
