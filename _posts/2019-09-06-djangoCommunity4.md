---
layout: post
title: "Django(장고)를 이용한 커뮤니티 만들기4
 []"
subtitle: "Django"
date: 2019-09-06 12:53:45
author: kwon
categories: django
---
저번 포스팅에서는 Django의 관리자도구를 사용하는 방법을 알아보았습니다.

이번 포스팅에서는 템플릿(Template)과 뷰(view)를 활용하여 회원가입 기능을 구현해보도록 하겠습니다.

먼저 템플릿에대해 간단히 알아보고 들어가겠습니다.

**템플릿(Template)** 은 View로부터 전달된 데이타를 템플릿에 적용하여 Dynamic 한 웹페이지를 만드는데 사용됩니다.

Template은 HTML 파일로서 Django App 폴더 밑에 "templates" 라는 서브폴더를 만들고 그 안에 템플릿 파일(.html)을 생성하여 사용합니다. 이는 단일 App이거나 동일 템플릿명이 없는 경우 사용할 수 있습니다.

하지만, Django 개발 가이드라인은 "App폴더/templates/App명/템플릿파일" 처럼, 각 App 폴더 밑에 templates 서브폴더를 만들고 다시 그 안에 App명을 사용하여 서브폴더를 만든 후 템플릿 파일을 그 안에 넣기를 권장하고 있습니다 (예: /home/templates/home/index.html ).

이는 만약 복수의 App들이 동일한 이름의 템플릿을 가진 경우, View에서 잘못된 템플릿을 가져올 수 있기 때문인데, 예를 들어, App1에 create.html이 있고, App2에 동일한 create.html 템플릿이 있는 경우, App2의 View에서 create.html를 지정하면, 처음 App1의 create.html을 사용하게 됩니다. 이는 템플릿을 찾을 때 자신의 App 내의 템플릿을 먼저 찾는 것이 아니라, 전체 App들의 템플릿 폴더들을 처음부터 순서대로 찾기 때문입니다. View에서 "App2/create.html" 과 같이 템플릿명을 지정하면 이런 혼동은 없어지게 됩니다.

따라서 먼저 이전에 만들어두었던 user의 templates폴더 아래에 user이라는 이름의 폴더를 하나 더 생성하고 그 안에 register.html이라는 html5파일을 하나 생성해보도록 하겠습니다.



 <div style="width: 350px; height: 250px;">
     <img src="https://kyu9341.github.io/assets/django8.png" style="width: 350px
     ; height: 250px;">
 </div>
