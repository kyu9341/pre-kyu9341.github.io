---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기1 [가상환경 설치 및 프로젝트 생성](파이참 개발환경)"
subtitle: "Django"
date: 2019-09-02 15:23:28
author: kwon
categories: django
---

이번에는 장고를 이용해서 간단한 커뮤니티를 만들겠습니다.
저는 파이참 개발환경을 이용하여 제작하겠습니다.

우선 자신이 원하는 장소에 프로젝트에 사용할 폴더를 만들어 줍니다.
파이참에서 프로젝트 폴더를 open한 이후 터미널을 열어줍니다.

![djangoTerminal](https://kyu9341.github.io/assets/djangoTerminal.png)
터미널 버튼을 누르면 아래처럼 터미널이 나오게 됩니다.

![djangoTerminal](https://kyu9341.github.io/assets/django1.png)
이후 명령창에 가상환경 설치를 위해 다음과 같은 명령어를 입력해 줍니다.
pip install virtualenv 또는 pip3 install virtualenv
python2를 사용하시는 분들은 pip, python3를 사용하시는 분들은 pip3로 사용하시면 되겠습니다.
명령어를 입력하게 되면 패키지가 설치가 됩니다.

이제 가상환경을 설치하여 보겠습니다.
명령창에
virtualenv (가상환경 이름)
을 입력해주시면 가상환경이 설치가 될 것입니다.


myvenv\Scripts\activate (가상환경 실행)
