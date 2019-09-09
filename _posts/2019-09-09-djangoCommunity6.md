---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기6
 [로그인 기능 구현]"
subtitle: "Django"
date: 2019-09-09 17:16:32
author: kwon
categories: django
---
로그인 기능을 구현하기 위해서는 세션에 대해 알아야 합니다. 세션이란, 웹사이트에 로그인을 하고나면 그 사이트에서는 다시 로그인할 필요 없이 여러 페이지의 기능을 이용할 수 있게 하기 위해 사용됩니다.

일반적으로 웹에서 클라이언트와 서버가 있는데 클라이언트 안에는 쿠키라는게 들어있습니다. 쿠키란 하나의 저장소라고 보시면 되고 클라이언트는 지금 웹사이트를 만들고 있기 때문에 웹 브라우저라고 보면 됩니다. 이 브라우저 안에 쿠키라는 저장공간이 있는데 이 저장공간을 활용해서 데이터를 유지하는 것입니다.

 예를 들어 어떤 웹사이트에 접속을 한다고 하면, 서버에 요청을 보내게 되겠죠? 그러면 서버는 헤더에 쿠키 정보를 넣어서 줍니다. 이 쿠키에는 어떤 클라이언트가 로그인 하였는지 구분하기 위한 세션ID라는 것이 들어있습니다. 따라서 클라이언트는 쿠키에 세션ID를 가지고 있게 되고 이후 서버에 요청을 보낼 때 이 쿠키의 세션ID를 서버에 전달하면, 서버는 세션ID를 전달받아 세션ID로 세션에 있는 클라이언트 정보를 가져와 요청을 처리하여 응답을 보내게 됩니다.

세션은 이러한 구조로 동작을 합니다. 조금 복잡하게 느껴질 수도 있지만 장고에서는 이런 작업을 모두 알아서 처리해줍니다. 따라서 실제로 사용하기에는 그렇게 어렵지 않습니다.

먼저 로그인 기능을 구현하기 위해 로그인 페이지에 사용될 템플릿 파일을 만들어보도록 하겠습니다. 이전에 만들어두었던 회원가입 화면에서 이메일과 비밀번호 확인부분만 지워주고 조금만 수정하면 로그인화면으로 이용이 가능하므로 다음과 같이 templates/user에 login.html파일을 만들어보도록 하겠습니다.

```html
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
              {% raw %}
              {% csrf_token %}
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

                <button type="submit" class="btn btn-primary">로그인</button>
            </form>
        </div>
    </div>
</div>
</body>
</html>

```
템플릿 파일을 만들었으므로 연결시켜주기 위해 user/views.py로 이동하여 먼저 login함수를 추가해주도록 하겠습니다.

```python
def login(request):
    return render(request, 'user/login.html')
```
이제 user/urls.py로 이동하여 다음과 같이 경로를 추가해주도록 합니다.

```python
urlpatterns = [
    path('register/', views.register),
    path('login/', views.login),
]
```
여기까지 완료되었다면 정상적으로 로그인화면이 출력되는지 한번 확인해보도록 하겠습니다.
<http://127.0.0.1:8000/user/login/>을 입력하고 확인해보면

<div style="width: 90%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/django16.png" style="width: 90%
    ; height: 300px;">
</div>

위와 같이 로그인화면이 잘 나오는 것을 확인할 수 있습니다.
