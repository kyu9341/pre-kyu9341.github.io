---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기3
 [Django amdin]"
subtitle: "Django"
date: 2019-09-053 18:23:28
author: kwon
categories: django
---

저번 포스팅에서 Django에서 모델링과 데이터베이스 연동까지 진행을 해보았습니다. 이번 포스팅에서는 Django의 admin 기능을 사용하는 방법에 대해 알아보겠습니다.

community의 urls.py 파일에 들어가 보면 아래와 같이 프로젝트가 생성되면서 자동으로 작성된 코드가 있습니다.

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

위와 같이 기본적으로 admin은 정의되어 있는데 기본 주소 뒤에 admin을 붙여 장고의 관리자 도구를 사용할 수 있습니다.



<div style="width: 90%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/djangoadmin.png" style="width: 90
    %; height: 200px;">
</div>
