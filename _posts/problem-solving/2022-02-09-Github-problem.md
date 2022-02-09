---
layout: post
title: "Github security vulnerability bot alert solved"
date: 2022-02-09 12:49:00 +0900
category: problem-solving
slug: problem-solving
tags: problem-solving
image:
  path: https://images.unsplash.com/photo-1609643242070-c69786a76c30?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1332&q=80
---

# Github security vulnerability bot alert solved

사이드 프로젝트를 잘 진행하다가 무엇이 문제였는지 github push 이후 repository에 alert이 발생하여 당황하였다.

![alert](https://user-images.githubusercontent.com/79234473/153140997-90ae0f1d-0254-4047-a9ba-da94bfcd65d2.png)

alert을 확인하러 들어가니
다음과 같은 사항이 확인되었다.

## 🧶 Problem

![11](https://user-images.githubusercontent.com/79234473/153141308-51d8eeb3-c4b8-49d2-bed1-88785cdae0c5.png)

해당 사항을 구글링하니 보완이 취약한 버전을 사용하는 모듈이 있어서 에러가 발생한다고 하여

```
npm audit
```

실행하여 해당 사항을 확인하고

```
npm audit fix --force
```

을 실행해 강제로 버전을 맞추려고 하였으나 오히려 에러가 더 많이 발생하였다.

### 몹시 당황! 🤷🏻‍♂️<br>

그래서 다시 원점으로 돌아가 (git reset하여 이전 상황으로 돌림)

확인하니 에러메세지가 조금 바뀌었다.

![22](https://user-images.githubusercontent.com/79234473/153141345-f73173e8-8278-4d76-a0ed-823d806713e2.png)

이번에는 다시 자세히 확인하고자 각각의 사항을 확인하였다.

## 🔬 Detail Check<br>

### ⚡️ 첫번째 항목!

> `nth-check` 가 버전이 맞지 않는 문제 확인

버전이 2.0.1 이어야 하는데 취약한 버전이 있어 지금 사용할 수 있는 버전이 1.0.2 라고 한다.

![33](https://user-images.githubusercontent.com/79234473/153143131-0244c723-5879-44e3-9754-79214d0929c5.png)

### ⚡️ 두번째 항목!

> `postcss` 버전 문제

이 역시 버전을 7.0.39 만 사용 가능하다고 하였다.

![44-1](https://user-images.githubusercontent.com/79234473/153143151-129eb882-e7a3-4825-870d-b6d49bc0e699.png)

그런데 자세히 찾아보니

가장 아래의 것만 버전이 크게 차이나는 것을 확인하였다.

![44-2](https://user-images.githubusercontent.com/79234473/153144509-657a9a7d-d58f-4428-ad80-31d01b6d619c.png)

다시 구글링하여 다른 방법을 찾았다.

## 🪄 Solution<br>

> - package-lock.json 파일을 삭제하거나
>   package-lock.json 파일에서 취약한 패키지에 해당하는 줄만 삭제한다.
>
> - npm install 실행

### 1. postcss 문제

package-lock.json에서 마지막 사항 1가지를 찾아 삭제 후 npm install 실행하였다.

### 2. nth-check 문제

package-lock.json에서 해당 사항을 모두 찾아 삭제한 후

nth-check를 다시 설치하였다.

## 💥 Confirmination<br>

```
npm audit
```

실행하니 오류가 발생하지 않았다.

해당 프로젝트 repository에 push한 후 다시 repository 확인하였으나 역시 오류메세지는 발생하지 않았다.

Dependabot alerts 항목으로 들어가니 해당 사항은 closed 되었고
확인하니 다음과 같이 처리되었음을 확인할 수 있었다.

![55](https://user-images.githubusercontent.com/79234473/153147976-ed3e8233-25c3-433f-8608-22b98558b94a.png)

<br>

당황하였지만 두드리면 문이 열린다.

열심히 두들기고 또 하나를 알아냈다.

우훗! 🎩
