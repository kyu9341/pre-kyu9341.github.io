---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기9
 [게시판 만들기]"
subtitle: "Django"
date: 2019-09-14 17:52:40
author: kwon
categories: django
---
이번 포스팅에서는 board앱으로 이동해서 게시판을 만들어보도록 하겠습니다.

먼저 user 앱에서 사용했던 base.html파일을 복사하여 board/templates/board에 붙여넣도록 하겠습니다. 이 후 board_list.html파일 생성하여 base.html상속받아 다음과 같이 작성해보도록 하겠습니다.

```html
{% raw %}
{% extends "./base.html" %}

{% block contents %}
<div class="row mt-5">
    <div class="col-12">
        <table class="table table-light">
            <thead class="thead-light">
                <tr>
                    <th>#</th>
                    <th>제목</th>
                    <th>아이디</th>
                    <th>일시</th>
                </tr>
            </thead>
            <tbody class="text-dark">
            <tr>
                <th>1</th>
                <th>제목 텍스트입니다.</th>
                <th>user</th>
                <th>2019-09-14 17:48:00</th>
            </tr>
            </tbody>
        </table>
    </div>
</div>
<div class="row">
    <div class="col-12">
        <button class="btn btn-primary">글쓰기</button>
    </div>
</div>
{% endblock %}
{% endraw %}
```
작성이 완료되었다면 템플릿을 연결하기 위해 먼저 community/urls.py 로 이동하여 다음과 같이 path를 추가해주도록 합니다.

```python
from django.contrib import admin
from django.urls import path, include
from user.views import home

urlpatterns = [
    path('admin/', admin.site.urls),
    path('user/', include('user.urls')),
    path('board/', include('board.urls')),
    path('', home)

]
```
현재 board에 urls파일이 없기 때문에 오류가 날 것입니다. board/에 urls.py를 만들고 다음과 같이 작성해줍니다.

```python
from django.urls import path, include
from . import views

urlpatterns = [
    path('list/', views.board_list),
]
```
또 아직 views.py에 board_list라는 함수가 없으므로 오류가 날 테니 가서 작성해주도록 합니다.

```python
from django.shortcuts import render

def board_list(request):
    return render(request, 'board/board_list.html')
```
간단하게 연결만 시킨 뒤 한번 확인해보도록 하겠습니다.


<div style="width: 90%; height: 250px;">
    <img src="https://kyu9341.github.io/assets/django30.png" style="width: 90%
    ; height: 250px;">
</div>
