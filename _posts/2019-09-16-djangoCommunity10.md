---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기10
 [게시판 만들기2]"
subtitle: "Django"
date: 2019-09-16 16:51:14
author: kwon
categories: django
---
이번 포스팅에서는 글쓰기 기능과 글 상세보기 기능을 추가해보도록 하겠습니다.

먼저 templates/board/board_write.html 파일을 생성한 뒤 다음과 같이 작성해주도록 합니다.

```html
{% raw %}
{% extends "./base.html" %}

{% block contents %}
<div class="row mt-5">
    <div class="col-12">
        <form method="POST" action=".">
            {% csrf_token %}
            {% for field in form %}
            <div class="form-group">
                <label for="{{ field.id_for_label }}">{{ field.label }}</label>
                <input type="{{ field.field.widget.input_type }}" class="form-control" id="{{ field.id_for_label }}"
                       placeholder="{{ field.label }}" name="{{ field.name }}" />
            </div>
            {% if field.errors %}
            <span style="color : red">{{ field.errors }}</span>
            {% endif %}
            {% endfor %}
            <button type="submit" class="btn btn-primary">글쓰기</button>
        </form>

    </div>
</div>

{% endblock %}
{% endraw %}
```

이후 board/form.py 파일을 생성한 뒤 다음과 같이 작성합니다.

```python
from django import forms

class BoardForm(forms.Form):
    title = forms.CharField(
        error_messages={
            'required' : '제목을 입력해주세요.' # 입력하지 않은 경우('required'키에 저장) 에러메시지 지정
        },
        max_length=100, label="제목")
    contents = forms.CharField(
        error_messages={
            'required' : '내용을 입력해주세요' # 입력하지 않은 경우('required'키에 저장) 에러메시지 지정
        },
        widget=forms.Textarea, label="내용") # 내용를 입력할 위젯을 지정

```

이제 views.py로 이동하여 함수를 추가하도록 합니다.

```python
from django.shortcuts import render
from .models import Board
from .forms import BoardForm
# Create your views here.

def board_write(request):
    form = BoardForm()
    return render(request, 'board/board_write.html', {'form' : form})

def board_list(request):
    boards = Board.objects.all().order_by('-id') # Board모델의 모든 필드를 가져와 id의 역순으로 가져옴(최신글을 맨위로 올림)

    return render(request, 'board/board_list.html', {'boards' : boards})
```

이 후 url을 연결해주기 위해 board/urls.py 로 이동하여 다음과 같이 path를 추가합니다.

```python
from django.urls import path, include
from . import views

urlpatterns = [
    path('list/', views.board_list),
    path('write/', views.board_write),
]
```

이제 http://127.0.0.1:8000/board/write/ 로 이동하여 확인해보면


<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/django34.png" style="width: 100%
    ; height: 200px;">
</div>

위와 같이 틀을 잡아주고 다시 board_write.html 로 이동하여 다음과 같이 form을 적용하여 수정해주도록 하겠습니다.

```html
{% raw %}
{% extends "./base.html" %}

{% block contents %}
<div class="row mt-5">
    <div class="col-12">
        <form method="POST" action=".">
            {% csrf_token %}
            {% for field in form %}
            <div class="form-group">
                <label for="{{ field.id_for_label }}">{{ field.label }}</label>
                {{ field.field.widget.name }}
                {% ifequal field.name 'contents' %}
                <textarea class="form-control" name = "{{ field.name }}" placeholder="{{ field.label }}"></textarea>
                {% else %}
                <input type="{{ field.field.widget.input_type }}" class="form-control" id="{{ field.id_for_label }}"
                placeholder="{{ field.label }}" name="{{ field.name }}" />
                {% endifequal %}
            </div>
            {% if field.errors %}
            <span style="color : red">{{ field.errors }}</span>
            {% endif %}
            {% endfor %}
            <button type="submit" class="btn btn-primary">글쓰기</button>
        </form>

    </div>
</div>

{% endblock %}
{% endraw %}

```
이제 다시 확인해보면

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/django35.png" style="width: 100%
    ; height: 200px;">
</div>

위와 같이 글쓰기 기능을 위한 템플릿이 완성되었습니다.

이제 현재 로그인한 사용자를 작성자로 하여 글쓰기 기능을 구현하기 위해 board/views.py로 이동하여 다음과 같이 코드를 추가해보도록 하겠습니다.

```python
from django.shortcuts import render, redirect
from .models import Board
from user.models import User
from .forms import BoardForm
# Create your views here.

def board_write(request):
    if request.method == 'POST':
        form = BoardForm(request.POST)
        if form.is_valid():
            user_id = request.session.get('user') # user_id를 가져옴
            user = User.objects.get(pk=user_id) # 현재 로그인한 사용자 id를 user에 저장

            board = Board() # 모델 클래스 변수 생성
            board.title = form.cleaned_data['title'] # form의 제목을 가져옴
            board.contents = form.cleaned_data['contents']
            board.writer = user # 현재 로그인한 사용자의 id
            board.save()

            return redirect('/board/list/') # 작성 후 글목록으로 이동
    else:
        form = BoardForm()
    return render(request, 'board/board_write.html', {'form' : form})

def board_list(request):
    boards = Board.objects.all().order_by('-id') # Board모델의 모든 필드를 가져와 id의 역순으로 가져옴(최신글을 맨위로 올림)

    return render(request, 'board/board_list.html', {'boards' : boards})
```

이제 로그인화면으로 이동해 로그인을 한 뒤 사용자이름이 홈 화면에 출력되는 것을 확인하고 http://127.0.0.1:8000/board/write/ 로 이동하여

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/django36.png" style="width: 100%
    ; height: 200px;">
</div>

위와 같이 작성하고 글쓰기 버튼을 누르면

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/django37.png" style="width: 100%
    ; height: 200px;">
</div>

글 목록으로 이동하고 작성된 글이 정상적으로 출력되는 것을 확인할 수 있습니다.

이어서 글 상세보기 기능을 추가하기 위해 board/views.py로 이동하여 board_detail함수를 추가해줍니다.

```python
def board_detail(request, pk): # 몇번째 글인지 확인하기 위해 주소로부터 pk를 받음
    board = Board.objects.get(pk=pk) # pk에 해당하는 글을 가져옴(입력받은 id값에(몇번째글인지) 맞는 글을 가져옴)
    return render(request, 'board/board_detail.html', {'board' : board}) #템플릿에 전달

```
다음으로 board/urls.py로 이동하여 path를 추가하겠습니다.
```python
from django.urls import path, include
from . import views

urlpatterns = [
    path('detail/<int:pk>', views.board_detail), # int형 pk라는 변수명으로 받음 ex detail/1 (= 첫번째 글)
    path('list/', views.board_list),
    path('write/', views.board_write),
]
```

이 후 템플릿에 board_detail.html 파일을 생성해준 뒤 다음과 같이 작성합니다.

```html
{% raw %}
{% extends "./base.html" %}

{% block contents %}
<div class="row mt-5">
    <div class="col-12">
            <div class="form-group">
                <label for="title">제목</label>
                <input type='text' class="form-control" id="title" value="{{ board.title }}" readonly/>
                <label for="contents">내용</label>
                <textarea class="form-control" readonly>{{ board.contents }}</textarea>

            </div>
            <button type="submit" class="btn btn-primary">돌아가기</button>
    </div>
</div>

{% endblock %}
{% endraw %}
```
여기까지 작성이 완료되었다면 첫번째 글을 확인해보기 위해 http://127.0.0.1:8000/board/detail/1 로 이동해보면

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/django38.png" style="width: 100%
    ; height: 200px;">
</div>

위와 같이 첫번째 글의 상세페이지로 이동이 되는 것을 확인할 수 있습니다. 
