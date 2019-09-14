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

http://127.0.0.1:8000/board/list/ 로이동하여 확인해보면

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/django30.png" style="width: 100%
    ; height: 200px;">
</div>

위와 같이 게시판 형태를 갖추게 되었으니 이제 board의 모델을 만들어보도록 하겠습니다. board/models.py로 이동하여 다음과 같이 작성합니다.

```python
from django.db import models

class Board(models.Model):
    title = models.CharField(max_length=200, verbose_name='제목')
    contents = models.TextField(verbose_name='내용')
    writer = models.ForeignKey('user.User', on_delete=models.CASCADE, verbose_name='작성자')
    # 참조키로 user앱의 User모델의 기본키를 참조한다
    # models.CASCADE : user가 탈퇴한 경우 삭제된 사용자의 글들을 모두 삭제한다
    registered_dttm = models.DateTimeField(auto_now_add=True, verbose_name='등록시간')

    def __str__(self): # 클래스가 문자열로 변환될 때 사용되는 내장함수
        return self.title

    class Meta:
        db_table = 'Community_board'
        verbose_name = '커뮤니티 게시글'
        verbose_name_plural = '커뮤니티 게시글' # 복수형 표현도 설정

```
모델을 작성하였으니 이제 데이터베이스에 적용시키기 위해 makemigrations과 migrate를 해주도록 하겠습니다.



이제 views.py로 이동하여 데이터베이스에 있는 게시글들을 모두 불러오도록 다음과 같이 작성해줍니다.

```python
from django.shortcuts import render
from .models import Board
# Create your views here.

def board_list(request):
    boards = Board.objects.all().order_by('-id') # Board모델의 모든 필드를 가져와 id의 역순으로 가져옴(최신글을 맨위로 올림)

    return render(request, 'board/board_list.html', {'boards' : boards})
```
이제 다시 확인해 보면

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/django31.png" style="width: 100%
    ; height: 200px;">
</div>

위와 같이 아직 데이터베이스에 글이 없기 때문에 QuerySet[] 라고 출력되게 됩니다. 그럼 데이터베이스에서 글을 하나 작성해보도록 하겠습니다.

우선 board/admin.py로 이동하여 다음과 같이 작성해줍니다.
```python
from django.contrib import admin
from .models import Board

# Register your models here.

class BoardAdmin(admin.ModelAdmin):
    list_display = ('title', )
admin.site.register(Board, BoardAdmin) # 등록
```
