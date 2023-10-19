---
layout: default
title:  "requirements.txt 에 대하여"
date:   2023-10-19 18:58:00 +0900
---

# requirements.txt 에 대하여

<br>

### 목적
<p>
requirements.txt 문서는 Python 프로젝트에서 사용하는 외부 패키지들과 또 그 패키지들에 대한 버전 정보를 기록하는 파일이다. 이 파일은 여러명이 한 프로젝트를 함께 개발하며 공유할 때 매우 필수적인 파일이다. requirements.txt 파일을 사용하면 손쉽게 해당 환경에서 프로젝트를 가동시키기 위한 패키지들을 설치할 수 있다. 
</p>

<br>

### 내용

<p>
보통 requirements.txt 문서는 다음과 같은 내용을 가진다. 필요한 패키지 이름==버전 형식이다.
</p>

<br>

```
django-storages==1.14
djangorestframework==3.14.0
filelock==3.12.4
flatbuffers==23.5.26
fonttools==4.42.0
idna==3.4
Jinja2==3.1.2
jmespath==1.0.1
keyboard==0.13.5
kiwisolver==1.4.4
MarkupSafe==2.1.3
matplotlib==3.7.2
mediapipe==0.10.3
mpmath==1.3.0
mysqlclient==2.2.0
networkx==3.1
numpy==1.25.2
oauthlib==3.2.2
opencv-contrib-python==4.8.0.74
opencv-python==4.8.0.74
packaging==23.1
pandas==2.1.0
pilkit==2.0
Pillow==10.0.0
```

<p> 만약 정확히 어떤 버전이 아닌 일정 버전 이상이 필요한 경우에는 다음과 같이 서술할 수 있다. </p>

```
pilkit>=2.0
```
<p> 또한 n 버전대의 패키지 설치가 필요하고 정확한 버전이 중요하지 않다면 다음과 같이 서술할 수 있다. </p>

```
pilkit>=2.*
```

<br>

### 위치

<p>파일은 루트 디렉토리에 저장되는 것이 일반적이다. 보통 직접 requirement.txt 파일을 생성하고 작성하는 일이 없으므로 별로 중요하지 않다. 자세한 것은 뒤에서 설명하겠다.  </p>

<br>

### 명령어

<p>freeze 명령을 사용하여 현재 환경에 설치된 패키지들을 requirements.txt로 저장할 수 있다. 파일은 자동으로 생성되고 저장되기 때문에 수정해야할 때에도 직접 파일 내용을 수정하는 것과 기존 파일을 삭제하고 위 명령어를 사용해 다시 생성하여 수정하는 것도 가능하다. </p>

```bash
pip freeze > requirements.txt
```

<p>
이 명령을 통해 requirements.txt에 정의되어 있는 모든 패키지를 설치할 수 있다. 
</p>

```bash
pip install -r requirements.txt
```
