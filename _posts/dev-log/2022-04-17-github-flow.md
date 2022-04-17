---
layout: post
title: "git flow vs github flow"
date: 2022-04-17 21:29:11 +0900
category: dev-log
tags: dev-log
image:
  path: https://images.unsplash.com/photo-1622821977767-2eec8bd199c1?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=687&q=80
---

## Git flow

### 기본 브랜치 5가지

```
feature > develop > release > hotfix > master
```

![gitflow](https://user-images.githubusercontent.com/54384004/73646317-878a3880-46bc-11ea-9293-f5d66ff4e53f.png)

### 구조와 흐름

가장 중심이 되는 브랜치는 master 와 develop
머지된 feature, release, hotfix 브랜치는 삭제

### Feature 브랜치

- 브랜치 나오는 곳 : develop
- 브랜치가 들어가는 곳 : develop
- 이름 지정 : master, develop, release-*, hotfix-*를 제외한 어떤 것이든 가능.

#### 새로운 기능을 추가하는 브랜치

feature 브랜치는 origin에는 반영하지 않고, 개발자의 reop에만 존재하도록 한다.

### Release 브랜치

- 브랜치 나오는 곳 : develop
- 브랜치가 들어가는 곳 : develop, master
- 이름 지정 : release-\*

#### 새로운 Production 릴리즈를 위한 브랜치이다.

지금까지 한 기능을 묶어 develop 브랜치에서 release 브랜치를 따내고, develop 브랜치에서는 다음번 릴리즈에서 사용할 기능을 추가한다.
release 브랜치에서는 버그 픽스에 대한 부분만 커밋하고, 릴리즈가 준비되었다고 생각하면 master로 머지를 진행한다. (이때도 --no-ff 옵션을 이용하여 머지하였음을 남긴다.)
master로 머지 후 tag 명령을 이용하여 릴리즈 버전에 대해 명시를 하고, -s 나 -u 옵션을 이용하여 머지한 사람의 정보를 남겨두도록 한다. 그런 뒤 develop 브랜치로 머지하여, release 브랜치에서 수정된 내용이 develop 브랜치에 반영한다.

> develop 브랜치에서 따낸다.
> develop 브랜치에는 다음 릴리즈 때 사용할 기능 추가
> 버그 픽스에 대한 부분만 커밋
> 준비 완료되면 master로 머지 진행
> tag 명령으로 버전 명시
> develop 브랜치로 명시

### Hotfix 브랜치

- 브랜치 나오는 곳 : master
- 브랜치가 들어가는 곳 : develop, master
- 이름 지정 : hotfix-\*

#### Production에서 발생한 버그들은 전부 여기로…

수정 끝나면, develop, master 브랜치에 반영하고, master에 다가는 tag 를 추가해준다.
만약 release 브랜치가 존재한다면, release 브랜치에 hotfix 브랜치를 머지하여 릴리즈 될 때 반영이 될 수 있도록 한다.

#### Pros and Cons

- 명령어가 나와있다.
- 웬만한 에디터와 IDE에는 플러그인으로 존재한다.
- 브랜치가 많아 복잡하다.
- 안 쓰는 브랜치가 있다. 그리고 몇몇 브랜치는 애매한 포지션이다.

## Github flow

Git flow 가 GitHub에서 사용하기에는 복잡하다 여겨 GitHub Flow라는 내용으로 사용
자동화의 개념이 들어가 있다.

master 브랜치에 대한 role만 정확하다면 나머지 브랜치들에는 관여를 하지 않는다.
그리고 pull request 기능을 사용하도록 권장을 한다.

![githubflow](https://user-images.githubusercontent.com/54384004/73646887-b48b1b00-46bd-11ea-96e3-9df8289c87f6.png)

### 특징

release 브랜치가 명확하지 않은 시스템에서 사용에 맞게 되어있다.
hotfix와 가장 작은 기능을 구분하지 않는다. 어차피 둘 다 개발자가 수정해야 되는 일중에 하나이다. 단지 우선순위가 어디가 높냐라는 단계이다.

### 사용 방법

1. master 브랜치는 어떤 때든 배포가 가능하다.
   master 브랜치는 항상 최신의 상태이며, stable 상태로 Product에 배포되는 브랜치이다. 그리고 이 브랜치에 대해서는 엄격한 role를 주어 사용한다.

2. 새로운 일을 시작하기 위해 브랜치를 master에서 딴다면 이름은 어떤 일을 하는지 명확하게 작성한다.
   git flow 와는 다르게 feature 브랜치나 develop 브랜치가 존재하지 않는다. 그렇기에 새로운 기능을 추가하거나 버그를 해결하기 위한 브랜치의 이름은 자세하게 어떤 일을 하고 있는지에 대해서 작성해주도록 하자. Github 페이지에서 보면 어떤 일들이 진행되고 있는지를 확인하기 쉽게 말이다.

3. 원격지 브랜치로 수시로 push를 한다.
   git flow 와 가장 상반되는 방식이다. 항상 원격지에 자신이 하고 있는 일들을 올려 다른 사람들도 확인할 수 있도록 해준다.
   이 방법의 좋은 점은 하드웨어에 문제가 발생하여 작업하던 부분이 없어지더라도 원격지에 있는 소스를 받아서 작업을 할 수 있도록 해준다.

4. 피드백이나 도움이 필요할 때, 그리고 머징 준비가 완료되었을 때는 pull request를 생성한다.
   pull request 는 코드 리뷰를 도와주는 시스템이다.
   그렇기에 이것을 이용하여 자신의 코드를 공유하고, 리뷰를 받을 수 있도록 한다. 물론 머지가 준비 완료되어 master 브랜치로 반영을 요구하여도 된다.

5. 기능에 대한 리뷰와 사인이 끝난 후 master로 머지한다.
   곧장 product로 반영이될 기능이기에 이해관계가 연결된 사람들과 충분한 논의 이후 반영하도록 한다.

6. master로 머지되고 푸시되었을 때는 즉시 배포되어야 한다.
   GitHub Flow의 핵심인듯한 master로 머지가 일어나면 hubot을 이용하여 자동으로 배포가 되도록 설정해놓는다.

### Pros and Cons

- 브랜치 전략이 단순하다.
- 처음 git을 접하는 사람에게 정말 좋은 시스템이 된다.
- Github 사이트에서 제공하는 기능을 모두 사용하여 작업을 진행한다.
- 코드 리뷰를 자연스럽게 사용할 수 있다.
- CI가 필수적이며, 배포는 자동으로 진행할 수 있다.
- Github가 작업을 할 때 이렇게 작업하고 있다.
- CI와 배포 자동화가 되어있지 않은 시스템에서는 사람이 관련된 업무를 진행한다.
- 많은 것들이 올라오기 시작하면… 그때부터는 Hell...
