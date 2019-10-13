---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기1  
 [가상환경 설치 및 프로젝트 생성](파이참 개발환경)"
subtitle: "Django"
date: 2019-09-02 15:23:28
author: kwon
categories: django
---

패스트캠퍼스 강의를 보며 장고를 이용해서 간단한 커뮤니티를 만들어보는 과정을 리뷰해보겠습니다.

저는 파이참 개발환경을 이용하여 제작하겠습니다.

우선 자신이 원하는 장소에 프로젝트에 사용할 폴더를 만들어 줍니다.
파이참에서 프로젝트 폴더를 open한 이후 터미널을 열어줍니다.

![djangoTerminal](https://kyu9341.github.io/assets/djangoTerminal.png)

터미널 버튼을 누르면 아래처럼 터미널이 나오게 됩니다.

<div style="width: auto; height: auto;">
    <img src="https://kyu9341.github.io/assets/django1.png" style="width: 100%; height: 250px;">
</div>

이후 명령창에 가상환경 설치를 위해 다음과 같은 명령어를 입력해 줍니다.

*pip install virtualenv 또는 pip3 install virtualenv*

python2를 사용하시는 분들은 pip, python3를 사용하시는 분들은 pip3로 사용하시면 되겠습니다.
명령어를 입력하게 되면 패키지가 설치가 됩니다.

이제 가상환경을 설치하여 보겠습니다.
명령창에

*C:\kwon\FastDjango> virtualenv (가상환경 이름)*

을 입력해주시면 가상환경이 설치가 될 것입니다.

<div style="width: 250px; height: 400px;">
    <img src="https://kyu9341.github.io/assets/django2.png" style="width: 250px; height: 400px;">
</div>

설치가 완료되면 프로젝트 폴더에 가상환경 폴더가 생성된 것을 확인할 수 있습니다.

다음으로 가상환경을 실행시켜 보겠습니다.
cd 명령어를 이용하여 Scripts 폴더로 이동한 뒤
*C:\kwon\FastDjango\fcdjango_venv\Scripts> activate*

위와 같이 명령어를 입력하면 가상환경이 실행되어

*(fcdjango_venv) C:\kwon\FastDjango\fcdjango_venv\Scripts>*

이러한 형태를 나타내게 됩니다.

다음으로 가상환경에서 django를 설치해 줍시다.

*(fcdjango_venv) C:\kwon\FastDjango\fcdjango_venv\Scripts> pip install django*

정상적으로 django가 설치가 완료되면 이제 장고 프로젝트를 생성합니다.

*(fcdjango_venv) C:\kwon\FastDjango\fcdjango_venv\Scripts> django-admin startproject community*

<div style="width: 250px; height: 400px;">
    <img src="https://kyu9341.github.io/assets/django3.png" style="width: 250px; height: 400px;">
</div>
프로젝트가 생성되면 다음과 같이 프로젝트 폴더가 생성됩니다.

다음으로는 app을 하나 생성하겠습니다.

우선 프로젝트 폴더로 이동해줍시다 cd community(프로젝트명)

*(fcdjango_venv) C:\kwon\FastDjango\fcdjango_venv\Scripts\community> django-admin startapp board*

<div style="width: 250px; height: 400px;">
    <img src="https://kyu9341.github.io/assets/django4.png" style="width: 250px; height: 400px;">
</div>
위와 같이 board라는 app이 생성된 것을 확인할 수 있겠습니다.

project는 여러개의 app으로 구성될 수 있으며 app은 project의 한 기능을 수행한다고 보시면 될 것 같습니다.

이제 가상환경 및 프로젝트 생성이 완료되었고 서버를 한번 실행시켜보겠습니다.

*(fcdjango_venv) C:\kwon\FastDjango\fcdjango_venv\Scripts\community> python manage.py runserver*

명령어를 입력하시고 127.0.0.1:8000 에 접속해 보시면

<img src="https://kyu9341.github.io/assets/django5.png" width="90%" height="90%">

다음과 같은 화면이 나타난다면 성공적으로 서버가 실행된 것입니다.

다음 포스팅에서는 데이터베이스와 연동하는 방법을 알아보도록 하겠습니다.
