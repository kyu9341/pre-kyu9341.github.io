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
