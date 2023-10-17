---
layout: post
title:  "Git Credential Manager for Windows 가이드 만들기"
date:   2023-10-16 19:01:00 +0900
---


# Git Credential Manager for Windows GUIDE
Git Credential Manager for Windows (GCMW)는 Windows 환경에서 Git 작업 시 사용자 인증 정보를 관리해주는 도구이다.
<br>

## 목차

- [GCM]
  - [설치 방법](#설치-방법)
  - [설정 방법](#설정-방법)
  - [사용 방법]
- 그 외 방법
  - [SSH key]
  - [GIT CLONE]

<br>
<br>

---

## GCM
### 설치 방법
<li> 기본 설치 방식 : Git 설치 시에 extra option 에서 *Enable Git Credential Manager* 체크박스 선택 후 설치

<img src="{{ "/assets/img/content/post/gcmw001.png" | absolute_url }}" alt="001" class="post-pic"/>

<br>
<a href="https://learn.microsoft.com/en-us/azure/devops/repos/git/set-up-credential-managers?view=azure-devops"> 참고링크 MicroSoft learn document</a>

<li> 빌드 방식 : 다음 링크의 레포지토리를 솔루션 파일 디렉토리에 복제 후 빌드 </li>

### <a href="https://github.com/git-ecosystem/git-credential-manager.git">GCMW link 🌐</a> 

<br>

### 설정 방법

<ol> 1. 계정 로그인하기

<br>

``` bash
git config user.name "사용자 이름"
git config user.email "사용자 이메일"
git config. user.password "사용자 패스워드(Tocken)"

# 사용자의 디렉토리 전역에 GCM을 지정할 경우
# git config --global user.name "사용자 이름"
# 형식으로 작성한다
```

