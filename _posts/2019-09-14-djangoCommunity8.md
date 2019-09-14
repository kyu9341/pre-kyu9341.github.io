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

<div style="width: 100%; height: 250px;">
    <img src="https://kyu9341.github.io/assets/django23.png" style="width: 100%
    ; height: 250px;">
</div>

로그인 화면으로 이동하여 확인해 보면 장고가 자동으로 생성해준 태그들이 출력되는 것을 볼 수 있습니다.
이제 이것을 우리에게 필요한 대로 커스터마이징을 해보도록 하겠습니다.
