---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기7
 [장고 form 활용하기]"
subtitle: "Django"
date: 2019-09-14 12:38:51
author: kwon
categories: django
---
이번 포스팅에서는 장고의 form을 활용하는 방법에 대해 알아보겠습니다.

장고의 form은 템플릿 파일의 form태그에 필요한 필드들을 장고에서 관리해주고 만들어주는 기능입니다.

먼저 login.html로 이동하여 장고 form에서 기본으로 제공해주는 화면으로 변경해보도록 하겠습니다.
다음과 같이 현재 두가지의 필드를 지워주도록 합니다.
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
            {% csrf_token %}
            {{ form }}
            <button type="submit" class="btn btn-primary">로그인</button>
        </form>
    </div>
</div>
</div>
{% endblock %}
{% endraw %}
```

다음으로 user/forms.py을 생성해주고 loginForm을 만들어주도록 하겠습니다.

```python
from django import forms

class LoginForm(forms.Form):
    username = forms.CharField(max_length=100)
    password = forms.CharField()
```

위와 같이 두 개의 필드를 사용하는 form을 작성을 한 뒤 views.py로 이동하여 loginForm을 가져와 login함수를 다음과 같이 변경해주도록 합니다.

```python
from .forms import LoginForm # 위에서 생성한 forms를 불러옴

def login(request):
    form = LoginForm()  # 클래스 변수 생성
    return render(request, 'user/login.html', {'form': form})  # 생성한 클래스 변수를 템플릿에 전달

```

<div style="width: 90%; height: 250px;">
    <img src="https://kyu9341.github.io/assets/django23.png" style="width: 90%
    ; height: 250px;">
</div>

로그인 화면으로 이동하여 확인해 보면 장고가 자동으로 생성해준 태그들이 출력되는 것을 볼 수 있습니다.
이제 이것을 우리에게 필요한 대로 커스터마이징을 해보도록 하겠습니다.

login.html로 돌아와서 다음과 같이 작성해주도록 합니다.

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
            {% csrf_token %}
            {% for field in form %}
            <div class="form-group">
                <label for="{{ field.id_for_label }}">{{ field.label }}</label>
                <input type="{{ field.field.widget.input_type }}" class="form-control" id="{{ field.id_for_label }}"
                       placeholder="{{ field.label }}" name="{{ field.name }}" />
            </div>
            {% endfor %}
            <button type="submit" class="btn btn-primary">로그인</button>
        </form>
    </div>
</div>
</div>
{% endblock %}
{% endraw %}
```
위와 같이 작성한 뒤 로그인화면을 확인해보면

<div style="width: 90%; height: 250px;">
    <img src="https://kyu9341.github.io/assets/django24.png" style="width: 90%
    ; height: 250px;">
</div>

다음과 같이 적용이 된 것을 확인할 수 있습니다. 하지만 아직 원하는 텍스트가 나오지 않고 비밀번호 또한 일반 텍스트처럼 나오기 때문에 이것을 수정하여 보도록 하겠습니다.

다시 forms.py로 이동하여 다음과 같이 수정해주도록 하겠습니다.

```python
from django import forms

class LoginForm(forms.Form):
    username = forms.CharField(max_length=100, label="사용자 이름")
    password = forms.CharField(widget=forms.PasswordInput, label="비밀번호") # 비밀번호를 표시할 위젯을 지정
```

<div style="width: 90%; height: 250px;">
    <img src="https://kyu9341.github.io/assets/django25.png" style="width: 90%
    ; height: 250px;">
</div>

위와 같이 라벨이 적용되고 비밀번호 위젯 또한 정상적으로 적용이 된 것을 확인할 수 있습니다.

이제 form을 이용한 로그인 기능 구현을 위해 views.py로 이동하여 login함수를 다음과 같이 수정해보도록 합니다.
```python
def login(request):
    if request.method == 'POST': # 메소드가 POST인 경우
        form = LoginForm(request.POST) # 폼 클래스 변수 생성시 POST데이터를 저장
        if form.is_valid(): # form에 기본적으로 있는 is_valid()함수로 데이터가 정상적인지 검증
            # session
            return redirect('/')

    else:
        form = LoginForm() # 빈 클래스 변수 생성
    return render(request, 'user/login.html', {'form': form})  # 생성한 클래스 변수를 템플릿에 전달

```

현재 코드로는 정상적인 경우 로그인은 수행이 되지만 비정상적인 로그인인 경우 다시 기본 화면으로 돌아오게 되어있으므로 비정상적인 로그인시 에러메시지를 띄울 수 있도록 수정하여 보겠습니다. login.html파일로 이동하여 다음과 같이 코드를 추가하도록 합니다.

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
            <button type="submit" class="btn btn-primary">로그인</button>
        </form>
    </div>
</div>
</div>
{% endblock %}
{% endraw %}
```

form.is_valid()함수에는 비정상적인 로그인인 경우 자동으로 상황에 맞는 에러메시지가 저장되어 있습니다. 그 메시지가 위의 코드를 통해 실행이 되는 것입니다.

<div style="width: 90%; height: 250px;">
    <img src="https://kyu9341.github.io/assets/django26.png" style="width: 90%
    ; height: 250px;">
</div>

위와 같이 빨간색의 글씨로 상황에 맞는 에러메시지가 자동으로 출력되는 것을 볼 수 있습니다.

현재는 입력에 대한 오류 메시지만 출력이 가능하므로 비밀번호가 틀린 경우의 오류도 표시하기 위해 forms.py로 이동하여 다음과 같이 작성해주도록 합니다.

```python
from django import forms
from .models import User
from django.contrib.auth.hashers import check_password

class LoginForm(forms.Form):
    username = forms.CharField(max_length=100, label="사용자 이름")
    password = forms.CharField(widget=forms.PasswordInput, label="비밀번호") # 비밀번호를 표시할 위젯을 지정

    def clean(self):
        cleaned_data = super().clean() # super을 통해 기존의 form안에 들어있는 clean함수를 호출 값이 들어있지 않다면 이부분에서 실패처리되어 나가짐
        username = cleaned_data.get('username') # 값이 존재한다면 값들을 가져옴
        password = cleaned_data.get('password')

        if username and password: # 각 값이 들어있는 경우
            user = User.objects.get(username=username)
            if not check_password(password, user.password): # 입력된 비밀번호와 데이터베이스에서 가져온 비밀번호 비교
                self.add_error('password', '비밀번호를 틀렸습니다.') # 특정 필드에 에러를 넣는 함수
            else: # 비밀번호가 일치하는 경우
                self.user_id = user.id # self를 통해 클래스 변수 안에 저장되므로 밖에서 접근가능

```

위와 같이 코드를 추가한 후 로그인 화면에서 비밀번호를 다르게 입력해보겠습니다.

<div style="width: 90%; height: 250px;">
    <img src="https://kyu9341.github.io/assets/django27.png" style="width: 90%
    ; height: 250px;">
</div>

위와 같이 입력해둔 메시지가 출력되는 것을 확인할 수 있습니다. 이제 입력에 대한 오류메시지도 우리가 원하는 메시지를 출력할 수 있도록 변경해보도록 하겠습니다. 다시 forms.py로 이동하여 다음과 같이 추가해주도록 합니다.

```python
from django import forms
from .models import User
from django.contrib.auth.hashers import check_password

class LoginForm(forms.Form):
    username = forms.CharField(
        error_messages={
            'required' : '아이디를 입력해주세요.' # 입력하지 않은 경우('required'키에 저장) 에러메시지 지정
        },
        max_length=100, label="사용자 이름")
    password = forms.CharField(
        error_messages={
            'required' : '비밀번호를 입력해주세요' # 입력하지 않은 경우('required'키에 저장) 에러메시지 지정
        },
        widget=forms.PasswordInput, label="비밀번호") # 비밀번호를 표시할 위젯을 지정

    def clean(self):
        cleaned_data = super().clean() # super을 통해 기존의 form안에 들어있는 clean함수를 호출 값이 들어있지 않다면 이부분에서 실패처리되어 나가짐
        username = cleaned_data.get('username') # 값이 존재한다면 값들을 가져옴
        password = cleaned_data.get('password')

        if not check_password(password, user.password): # 입력된 비밀번호와 데이터베이스에서 가져온 비밀번호 비교
            self.add_error('password', '비밀번호를 틀렸습니다.') # 특정 필드에 에러를 넣는 함수
        else: # 비밀번호가 일치하는 경우
            self.user_id = user.id # self를 통해 클래스 변수 안에 저장되므로 밖에서 접근가능

```

<div style="width: 90%; height: 250px;">
    <img src="https://kyu9341.github.io/assets/django28.png" style="width: 90%
    ; height: 250px;">
</div>

이제 확인을 해보면 위와 같이 메시지가 정상적으로 출력되는 것을 확인할 수 있습니다.

마지막으로 이제 세션부분을 작성해보도록 하겠습니다. views.py로 이동하여 login함수 부분을 수정하도록 하겠습니다.

```python
def login(request):
    if request.method == 'POST': # 메소드가 POST인 경우
        form = LoginForm(request.POST) # 폼 클래스 변수 생성시 POST데이터를 저장
        if form.is_valid(): # form에 기본적으로 있는 is_valid()함수로 데이터가 정상적인지 검증
            request.session['user'] = form.user_id # 세션에 user의 세션id를 저장
            return redirect('/')

    else:
        form = LoginForm() # 빈 클래스 변수 생성
    return render(request, 'user/login.html', {'form': form})  # 생성한 클래스 변수를 템플릿에 전달
```

이렇게 세션까지 form을 이용하여 작성을 완료하였습니다. 이제 로그인을 수행하게 되면

<div style="width: 130%; height: 100px;">
    <img src="https://kyu9341.github.io/assets/django18.png" style="width: 130%
    ; height: 100px;">
</div>

위와 같이 정상적으로 로그인이 되어 세션id값이 발급된 것을 확인할 수 있습니다. 여기까지 django의 form을 활용하여 로그인 기능을 재구현해보았습니다.
