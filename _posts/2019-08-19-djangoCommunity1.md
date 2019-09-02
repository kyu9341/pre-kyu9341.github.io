---
layout: post
title: "Django(장고) 가상환경 설정 및 프로젝트 생성"
subtitle: "Django"
date: 2019-08-19 16:12:28
author: kwon
categories: django
---

**가상환경을 사용하는 이유**
한 컴퓨터에서 여러 프로젝트를 관리할 때 같은 패키지를 사용하는데 버전차이 때문에 계속 패키지를 설치해야함.
프로젝트 별로 패키지를 관리하는 공간을 분리 하기 위해 가상환경에서 작업


사용하고자 하는 디렉토리 생성

디렉토리로 이동 -> python -m venv myenv(가상환경이름)

C:\Users\Name\kwon> myvenv\Scripts\activate (가상환경 실행)



(장고 설치하는데 필요한 pip최신 버전 확인) : (myvenv) ~$ python -m pip install --upgrade pip

(myvenv) C:\Users\Name\kwon> pip install django  

(myvenv) C:\Users\Name\kwon> django-admin startproject mysite .
