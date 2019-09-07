---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기5
 []"
subtitle: "Django"
date: 2019-09-06 17:21:37
author: kwon
categories: django
---
이전 포스팅에서 장고의 템플릿과 뷰를 통해 간단한 회원가입 기능까지 구현해 보았습니다.



우선 ~ 시작하기에 앞서 회원가입 시에 사용자 이메일또한 입력을 받을 수 있도록 변경하여 보겠습니다. user/models.py 로 이동하여 다음과 같이 사용자 이메일을 추가해줍니다.

```python
from django.db import models

# Create your models here.

class User(models.Model):
    username = models.CharField(max_length=100, verbose_name='사용자명')
    useremail = models.EmailField(max_length=100, verbose_name='사용자이메일')
    password = models.CharField(max_length=100, verbose_name='비밀번호')
    registered_dttm = models.DateTimeField(auto_now_add=True, verbose_name='등록시간')

    def __str__(self): # 클래스가 문자열로 변환될 때 사용되는 내장함수
        return self.username


    class Meta:
        db_table = 'Community_user'
        verbose_name = '커뮤니티 사용자'
        verbose_name_plural = '커뮤니티 사용자' # 복수형 표현도 설정

```
모델에 추가를 완료하였다면 데이터베이스에 적용시키기 위해 makemigrations과 migrate를 해주도록 합니다.

이 때, *python manage.py makemigrations* 명령어를 입력하게 되면

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/django13.png" style="width: 100%
    ; height: 200px;">
</div>

위와 같이 나오게 되는데 현재 데이터베이스에는 useremail이라는 값이 없이 데이터들이 저장되어 있기 때문에 기존에 있는 데이터들에게 어떤 값을 지정해 줄 것인지 묻는 것입니다. 1번은 직접 여기서 기본값을 입력해 주는 것이고 2번은 모델안에서 default = ' ' 와 같은 형태로 기본값을 지정해 주겠다는 뜻입니다. 저는 일단 1번을 선택하여 'test@gmail.com'이라고 입력해 보도록 하겠습니다.

makemigrations이 완료되었으면 *python manage.py migrate* 명령어를 입력하여 마이그레이션을 완료합니다.

그럼 이제 register.html 파일로 이동하여 이메일을 입력받는 부분을 추가해줍니다.

```html
<div class="form-group">
    <label for="username">사용자 이메일</label>
    <input type="text"
           class="form-control"
           id="useremail"
           placeholder="사용자 이메일"
           name="useremail"
    >
</div>
```
사용자 이름과 비밀번호 사이에 위의 코드만 입력시켜주면 되겠죠? 이제 템플릿파일을 변경하였으니 user/views.py로 이동하여 다음과 같이 추가해주도록 합니다.


```python
from django.shortcuts import render
from .models import User
from django.http import HttpResponse
from django.contrib.auth.hashers import make_password

# Create your views here.

def register(request):
    if request.method == 'GET':
        return render(request, 'user/register.html')
    elif request.method == 'POST':
        username = request.POST.get('username', None) # 템플릿에서 입력한 name필드에 있는 값을 키값으로 받아옴
        password = request.POST.get('password', None) # 받아온 키값에 값이 없는경우 None값으로 기본값으로 지정
        re_password = request.POST.get('re-password', None)
        useremail = request.POST.get('useremail', None)

        res_data = {} # 응답 메세지를 담을 변수(딕셔너리)

        if not (usernameand and useremail and password and re_password):
            res_data['error'] = '모든 값을 입력해야 합니다.'
        elif password != re_password:
            res_data['error'] = '비밀번호가 다릅니다.'
        else:
            user = User( # 모델에서 생성한 클래스를 가져와 객체를 생성
                username=username,
                useremail=useremail,
                password=make_password(password) # 비밀번호를 암호화하여 저장
            )

            user.save() # 데이터베이스에 저장

        return render(request, 'user/register.html', res_data) # res_data가 html코드로 전달이 됨


```
추가가 완료되었으니 서버를 실행시킨 후 확인을 해보면

<div style="width: 90%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/django14.png" style="width: 90%
    ; height: 300px;">
</div>

위와 같이 정상적으로 추가가 된 것을 확인할 수 있습니다. 한번 입력을 해보면

<div style="width: 300px; height: 300px;">
    <img src="https://kyu9341.github.io/assets/amdin7.png" style="width: 300px
    ; height: 300px;">
</div>

정상적으로 입력이 된 것을 확인 할 수 있습니다.

다음으로는 부트스트랩의 테마를 적용시키기 위해 static파일을 관리하는 방법도 알아보도록 하겠습니다.

먼저 프로젝트폴더 community아래에 static폴더를 생성합니다. 이 폴더 안에 css, javascript등의 파일을 넣고 관리할 것입니다.

community/community/settings.py 맨 아래로 가면

STATIC_URL = '/static/' 라고 있는 부분 아래에 다음과 같이 추가하여 주도록 합니다.

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'), # static 파일을 접근했을 때, 그 파일이 어느 폴더에 있는지를 알려줌
]
# BASE_DIR이 프로젝트 폴더이기 때문에 그 아래 static폴더에 파일이 있다는 뜻.
```
이제 부트스트랩 테마 기능을 사용하기 위해 테마를 다운받아 보겠습니다.
<https://bootswatch.com/> 이곳에서 마음에 드는 테마를 받으시면 됩니다. 저는 slate라는 테마를 사용해 보겠습니다.
bootstrap.min.css라는 파일을 다운 받은 뒤 static폴더에 옮겨주도록 하겠습니다.

이제 register.html 로 이동해서 기존 <link> 태그를 지워준 뒤 <link rel="stylesheet" href="/static/bootstrap.min.css"/>를 추가해주도록 합니다.

```html
<head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <link rel="stylesheet" href="/static/bootstrap.min.css"/>
    <script src="https://codpye.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
</head>

```
완료되었다면 위와 같을 것입니다. 이제 한번 테마가 제대로 적용이 되었는지 확인해보겠습니다.
