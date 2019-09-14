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



<div style="width: 350px; height: 250px;">
    <img src="https://kyu9341.github.io/assets/session.png" style="width: 350px
    ; height: 250px;">
</div>

여기까지 간단한 로그인 기능까지 구현해 보았습니다.
