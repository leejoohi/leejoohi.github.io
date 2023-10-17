---
layout: post
title:  "Git Credential Manager for Windows 가이드 만들기"
date:   2023-10-16 19:01:00 +0900
---

# Git Credential Manager for Windows GUIDE
Git Credential Manager(GCM)을 Window에서 사용하는 방법을 서술한다.

 <BR>

## 목차

- [Git Credential Manager for Windows GUIDE](#git-credential-manager-for-windows-guide)
  - [목차](#목차)
- [GCM](#gcm)
  - [설치 방법](#설치-방법)
  - [설정 방법](#설정-방법)
  - [Cache 모드 특징, 장단점](#cache-모드-특징-장단점)
  - [Store 모드 특징, 장단점](#store-모드-특징-장단점)
  - [사용 방법](#사용-방법)
- [그 외 방법](#그-외-방법)
  - [SSH KEY](#ssh-key)
  - [GIT](#git)

<BR>
<BR>

# GCM
## 설치 방법
<LI> 기본 설치 방식 : Git 설치 시에 extra option에서 "Enable Git Credential Manager" 체크박스 선택 후 설치

<img src="{{ "/assets/img/content/post/gcmw001.png" | absolute_url }}" alt="001" class="post-pic"/>

<br>
<a href="https://learn.microsoft.com/en-us/azure/devops/repos/git/set-up-credential-managers?view=azure-devops"> 참고링크 MicroSoft learn document</a>

<LI> 빌드 방식 : Git repository <a href="https://github.com/git-ecosystem/git-credential-manager.git">🌐</a> 코드 저장 및 솔루션 파일 디렉토리에 복제. 솔루션 빌드하기

<br>

## 설정 방법

1. 계정 로그인하기

``` bash
git config user.name "사용자 이름"
git config user.email "사용자 이메일"
git config. user.password "사용자 패스워드(Tocken)"

# 사용자의 디렉토리 전역에 GCM을 지정할 경우
# git config --global user.name "사용자 이름"
# 형식으로 작성한다
```

2. GCM 모드 선택하기

``` bash
# cache mode (default)
git config --global credential.helper
# git config --global credential.helper cache

# store mode
git config --global credential.helper store
```


<div style="padding: 15px; border: 1px solid transparent; border-color: transparent; margin-bottom: 20px; border-radius: 4px; color: #31708f; background-color: #d9edf7; border-color: #bce8f1;">

## Cache 모드 특징, 장단점
- 캐시는 인증 정보를 일시적으로 저장하는 방식입니다.
- 사용자가 Git 작업을 수행하는 동안에만 메모리에 저장되며, Git 명령이 종료되면 제거됩니다.
- 기본 설정으로 제공되어 별도의 설정이 필요 없습니다.

</div>
<br>
<div style="padding: 15px; border: 1px solid transparent; border-color: transparent; margin-bottom: 20px; border-radius: 4px; color: #31708f; background-color: #d9edf7; border-color: #bce8f1;">

## Store 모드 특징, 장단점
- 저장소는 인증 정보를 영구적으로 저장하는 방식입니다.
- 사용자가 로그아웃하거나 장치를 종료해도 인증 정보가 계속 유지됩니다. 
- 암호화된 형태로 사용자의 시스템에 저장됩니다.
- 보안 측면에서 더 안전하지만, 더 민감한 정보가 저장되므로 사용자는 보안에 대한 책임을 져야 합니다.

</div>

1. (선택사항) 윈도우 Keychain 시스템 적용하기

 ```bash
  git config --global credential.helper wincred
  ```
  OS에서 제공하는 Keychain 시스템을 적용하면 더욱 안전하게 사용할 수 있다.

<br>

## 사용 방법

이제 pull, push를 따로 로그인하지 않아도 자유롭게 사용 가능합니다.

<br>

---

# 그 외 방법
 
## SSH KEY
1. SSH 키 생성하기:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

이때 "your_email@example.com" 부분에는 본인의 이메일 주소를 넣어주십시오.

-C 옵션은 주석 역할을 하는 것이므로, 이메일을 입력하지 않아도 키 생성이 가능합니다.

```bash
ssh-keygen -t rsa -b 4096
```

키를 생성할 때 이름을 별도로 지정할 수 있으며, 패스워드 설정이 가능합니다.

<img src="{{ "/assets/img/content/post/gcmw002.png" | absolute_url }}" alt="002" class="post-pic"/>

1. SSH 에이전트 실행 (선택사항):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

이건 에이전트를 사용해서 SSH 키를 관리하는데, 필수는 아니지만 편리합니다.

이름을 별도로 지정한 경우
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/my_key
```

3. SSH 공개키 복사하기:

```bash
cat ~/.ssh/id_rsa.pub
```
출력된 키를 복사합니다.

이름을 별도로 지정한 경우
```bash
cat ~/.ssh/my_key.pub
```


4. Git 서비스에 SSH 공개키 등록하기:

GitHub의 경우, [Settings] -> [SSH and GPG keys] -> [New SSH key] 클릭 후 복사한 키를 등록합니다.
GitLab이나 Bitbucket 등의 다른 서비스는 각각의 설정 페이지에서 SSH 키를 등록할 수 있습니다.

5. 테스트:

```bash
ssh -T git@github.com
```
만약 성공적으로 연결되면 "Hi username! You've successfully authenticated..."와 같은 메시지가 나올 것입니다.

이제 SSH를 통해 Git 서비스와 통신할 수 있습니다. git clone, git pull, git push 등의 명령을 사용할 때 SSH 프로토콜을 선택하면 자동으로 SSH 키가 사용됩니다.


<BR>

## GIT  
git clone 을 받을 때 주소 안에 사용자의 Id와 Password(또는 Tocken)을 입력한 주소로 clone을 받으면 추후에 아이디와 비밀번호를 입력하지 않아도 사용할 수 있다.

```bash
git clone https://<ID>:<PASSWORD>@github.com/munsa3407/TEST.git
```