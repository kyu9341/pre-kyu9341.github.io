---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기2

 []"
subtitle: "Django"
date: 2019-09-03 15:23:28
author: kwon
categories: django
---

저번 포스팅에서 Django의 가상환경 설치와 프로젝트를 생성하여 서버를 실행시켜 보는 것 까지 진행을 하였습니다. 이번에는 Django에서 모델링과 데이터베이스 연동까지 진행을 해보겠습니다.  

우선 board에 templates폴더를 생성합니다. 이 폴더는 다음에 사용할 것이므로 추후에 다시 설명하도록 하겠습니다.

다음으로 전 포스팅에서 했던 거와 마찬가지로 app을 하나 더 생성해 보도록 하겠습니다.

C:\kwon\FastDjango\fcdjango_venv\Scripts\community>django-admin startapp user

app생성이 정상적으로 완료되었다면 마찬가지로 user에 하위폴더로 templeate폴더를 생성하도록 합니다.

<div style="width: 250px; height: 400px;">
    <img src="https://kyu9341.github.io/assets/django6.png" style="width: 250px; height: 400px;">
</div>

여기까지 완료되었다면 위와 같은 구조일 것입니다.

django에서 app을 사용하기 위해서는 프로젝트에 app을 등록해 주어야 합니다.

community폴더 아래에 자동으로 생성된 community폴더가 있습니다. 거기에 있는 settings.py 파일에 들어가 보면 INSTALLED_APPS 에 다음과 같이 board와 user 앱을 추가해 주도록 합니다.

<div style="width: 400px; height: 250px;">
    <img src="https://kyu9341.github.io/assets/django7.png" style="width: 400px; height: 250px;">
</div>

app 등록이 완료되었으니 데이터베이스와 연동에 대해 알아보겠습니다.

django에서는 따로 데이터베이스에 접속해 테이블을 생성하고 컬럼을 만들지 않더라도 django내에서 모델링을 통해 생성을 할 수 있습니다.

사용자명과 비밀번호를 저장할 테이블을 생성하기 위해서 user 앱을 모델링 해볻록 하겠습니다. user/models.py 파일로 이동하여 다음과 같은 코드를 작성하겠습니다.

```python
from django.db import models

# Create your models here.

class User(models.Model):
    username = models.CharField(max_length=50, verbose_name='사용자명')
    password = models.CharField(max_length=50, verbose_name='비밀번호')
    registered_dttm = models.DateTimeField(auto_now_add=True, verbose_name='등록시간') #auto_now_add=True : 객체가 저장되는 시점의 시간이 자동을 저장

    class Meta:
        db_table = 'Community_user' # 테이블명 지정

```

verbose_name은 admin사용 시 필드에 대한 명령으로 username나 password대신 사용자명과 비밀번호 라고 표시되도록 해주는 부분입니다.

모델 작성이 완료되었습니다. django에서 데이터베이스는 sqlite3가 기본으로 제공되지만 저는 mysql을 이용하여 데이터베이를 연동해보도록 하겠습니다. 
