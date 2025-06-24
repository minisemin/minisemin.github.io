---
layout: post
title:  "풀스택 웹개발(4) - Git, Git Action과 Docker를 활용한 서비스 배포"
date:   2023-10-14 12:40:48 +0900
categories: [webdev, spring]
---


## 소개
[풀스택 웹개발] 시리즈는 Spring을 활용하여 풀스택 웹사이트를 만드는 것을 목표로 합니다. 빠르고 직관적인 이해를 돕기 위해 전문적인 용어보다는 비유적인 표현을 많이 사용했습니다. 웹사이트를 처음 만들어보는 분도 쉽게 배우고 따라하는 웹개발 설명서가 되었으면 합니다.

본 시리즈가 다루는 주제는 크게 다음과 같습니다:
> - [API, REST API](#https://minisemin.github.io/webdev/2023/09/09/webdev1.html)
> - [Spring Framework](#https://minisemin.github.io/webdev/2023/09/14/webdev2.html)
> - [Spring Sequrity](#https://minisemin.github.io/webdev/2023/10/07/webdev3_spring_security.html)
> - [Git, Git Action과 Docker](#https://minisemin.github.io/webdev/2023/10/14/webdev4_git_gitaction_docker.html)
> - [AWS 클라우드 컴퓨팅](#https://minisemin.github.io/webdev/2023/10/14/webdev5_AWS.html)
> - [In Memory Database - REDIS](#https://minisemin.github.io/webdev/2023/11/04/webdev6_redis.html)

다음과 같은 내용을 학습하시면 본 시리즈를 이해하는 데 도움이 됩니다:
> - 객체지향(Opject-oriented-programming)
> - Java
> - SQL

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

#### Table of Contents
---
1. [Git](#git)
2. [Git Action](#git-action)
3. [Docker](#docker)

---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

## Git

Git은 개발자들이 가장 많이 사용하는 공유된 버전 관리 시스템입니다. Google Docs를 여러 사람이 동시에 편집할 수 있는 것처럼, Git은 여러 개발자들이 동시에 코드를 수정하는 걸 가능하게 해 줍니다. Git의 주요 목적은 코드의 변경 이력을 효율적으로 관리하게 해 주고, 여러 개발자들이 각자 개발한 기능을 통합할 수 있는 환경을 제공하는 것입니다.

가장 흔하게 Git 사용하는 협업 방식은 다음과 같습니다:

1. `git clone` 을 이요해 Github에 있는 Repository를 본인의 컴퓨터에 복사합니다.
2. 코드를 수정합니다.
3. `git pull`을 통해 Git Repository에 변경사항이 있는 지 확인하고, 충돌이 일어나지 않도록 본인의 컴퓨터에서 코드를 합쳐줍니다.
4. `git add`
5. `git commit -m "커밋 메시지"`
6. `git push origin [branch 이름]` 을 이용해 본인의 branch에 변경 사항을 올립니다.
    - 본인이 현재 작업중인 브랜치는 `git branch`로 확인할 수 있습니다.
    - 다른 브랜치로 이동할 때는 `git checkout [브랜치이름]`을 사용합니다.
    - 새로운 브랜치를 만들 때는 `git branch [브랜치이름]`을 사용합니다.
7. pull request를 만들어, 다른 사용자가 변경 사항을 확인하고, 이를 승낙할 경우 main branch로 merge해줍니다.

&nbsp;

## Git Action

Github Actions는 GitHub에서 제공하는 CI/CD(지속적 통합 및 지속적 배포) 플랫폼입니다. 이 서비스를 통해 개발자들은 소프트웨어 개발 워크플로우를 자동화할 수 있습니다. `git commit`을 할 경우 자동으로 테스트를 실행하게 만들거나, 코드를 특정 브랜치에 병합시킬 때 자동으로 배포하는 작업을 설정할 수 있습니다.

예를 들어 웹 개발을 하는 경우, Github Actions는 코드의 품질을 유지하며 배포 과정을 자동화하는 데 유용하게 사용할 수 있습니다. 테스트 코드를 자동으로 실행하며 새로운 변경사항이 기존의 기능에 해를 끼치는 지 미리 확인할 수 있으며, 지속적으로 코드를 통합시키며 문제를 조기에 발견하고 해결할 수 있습니다. 그리고 자동 베포 기능을 통해 새로운 기능 업데이트를 신속하고 안정적으로 진행할 수 있습니다.

&nbsp;

## Docker

![Dockr 아이콘](https://cdn.icon-icons.com/icons2/2699/PNG/512/docker_official_logo_icon_169250.png)

Docker는 애플리케이션을 컨테이너 내에서 실행하기 위한 플랫폼입니다. 우주선과 우주선이 서로 연결되는 과정처럼, 두 가지 물체가 연결되는 것을 도킹 'Docking'이라고 부릅니다. Docker 컨테이너는 코드, 런타임, 시스템 도구, 시스템 라이브러리 및 설정을 포함하여 애플리케이션 실행에 필요한 모든 것들을 패키징하는 기술입니다.

### 컨테이너와 가상화
Docker 컨테이너는 가상 머신(VM)과 유사한 환경을 제공하지만, 훨씬 가벼운 구조로 이루어져 있습니다. 일반적인 Virtual Machine(VM)이 운영 체제 전체를 가상화하는 반면, Docker 컨테이너는 호스트이 OS 커널을 공유하고 애플리케이션 레벨만 격리시켜 제공합니다. 덕분에 Docker 컨테이너는 리소스 사용량을 줄이고, 컨테이너의 시작 시간을 단축시킵니다.

### 일관된 개발, 테스트, 프로덕션 환경
Docker는 "Write Once, Run Anywhere"라는 Java와 비슷한 개념을 가지고 있습니다. Docker를 사용하면 개발자는 로컬 환경에 상관없이 애플리케이션을 구성하고, 동일한 설정으로 테스트하고 프로덕션 환경에서 실행시킬 수 있습니다.

### 이미지와 레지스트리
Docker는 '이미지' 형식으로 애플리케이션과 의존성들을 패키징합니다. 이미지는 애플리케이션을 실행하는 데 필요한 모든 파일과 설정을 저장하는 템플릿입니다. 개발자는 이 이미지를 Docker Hub나 Docker 레지스트리에 업로드하여 다른 개발자들과 옹유할 수 있고, 이 이미지를 다운로드하면 동일한 애플리케이션 환경을 재현할 수 있습니다.

### Docker의 사용성과 보안
Docker는 터미널같은 CLI(명령어 라인 인터페이스)를 제공하여 사용하기 편리합니다. 또한, 각 컨테이너를 독립적으로 실행시키기 때문에 하나의 컨테이너에 문제가 발생해도 다른 컨테이너에는 영향을 주지 않습니다.


---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

#### 자료 출처
- YC Tech Academy 4주차
