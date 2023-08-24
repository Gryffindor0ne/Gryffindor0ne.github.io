---
layout: post
title: "Netlify 배포시 발생한 에러 처리 방법"
date: 2022-02-10 00:01:00 +0900
category: problem-solving
hide_description: false
tags: problem-solving
image:
  path: https://images.unsplash.com/photo-1612933510543-5b442296703b?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1964&q=80
---

# Netlify 배포시 발생한 에러 처리 방법

## 🙏🏼 Netlify 첫 배포 (feat: 잘 부탁드려요)

사이드 프로젝트를 완성하고 내가 만든 앱이 잘 실행되는지 확인하고 싶어서 배포까지 해보기로 했다.
팀 프로젝트 할때는 AWS를 통한 배포를 도메인 구입하여 팀원들과 같이 해보았지만 혼자 작업한 정적 사이트를 배포한 적은 없었기에 이번 기회에 배포를 해보기로 했다.

배포 도구로는 요즘 많이들 사용한다는 Netlify를 처음 사용해 보기로 했다.

![Netlify](https://user-images.githubusercontent.com/79234473/153225775-01231622-a7d7-43ca-bb27-045378de4dd4.png)

생각보다 배포의 과정은 간단했다. (그런 줄 알았다.)

Netlify를 가입하고 Github에 연결하여 내가 작업한 프로젝트를 연결하고 배포를 시도하였다.

그러나...

## 🧶 Problem 1<br>

![배포 첫 에러](https://user-images.githubusercontent.com/79234473/153210419-1be2a9ca-0c21-4ba1-9f08-bc3118b3c37c.png)

<br>

```
Treating warnings as errors because process.env.CI = true.
```

process.env.CI = true 여서 경고를 에러처리 한다는 말은 머선말인가?

이럴 때 필요한 건 역시 구글링

## 🪄 Solution 1

<br>

> 메뉴에서 Deploys > Deploy Settings > Build & deploy > Continuous Deployment
>
> Build settings 에서 Edit settings를 눌러 Build command를
>
> `CI = npm run build` 로 변경한다.

<br>

![Build 설정](https://user-images.githubusercontent.com/79234473/153214233-17cb6c1a-369f-4eb0-83f1-d66c3363277a.png)

혹은 `CI=false npm run build` 로 변경해도 된다.

## ✨ 배포 성공

![배포 첫 성공](https://user-images.githubusercontent.com/79234473/153215702-26444df8-11db-46d8-ab6d-6fb37cb582fa.png)

일단 배포는 성공한 것 같다.

그런데 앱을 실행시켜보니

Ooops!

## 🧶 Problem 2<br>

API 응답이 없다.

결과 화면에 아무 것도 안나와!

알고 보니 .env파일에 있는 api key를 .gitignore 처리하여 읽어오지 못하는 것.

## 🪄 Solution 2

<br>

Netlify에서는 환경 변수를 설정하는 방법이 따로 존재한다.

> 메뉴에서 Deploys > Deploy Settings > Build & deploy > Environment
>
> Edit variables를 눌러 Key와 Value를 입력한다.

<br>

![env 설정과정](https://user-images.githubusercontent.com/79234473/153217709-7aca4da3-76e3-42d6-b445-a45ae7bb1983.png)

나는 Naver Open Api를 사용하여 두 개의 키를 등록하였다.

저장하면 다음과 같이 화면이 뜬다.

![env 설정](https://user-images.githubusercontent.com/79234473/153217725-aa6dc781-1971-4939-9b50-76df91b384b2.png)

<br>

자, 그럼 다 되었는데 다시 해볼까?

뭐지...

그래도 안되네...

api를 불러오지 못한다.

결국 또 구글링의 세계로!!!

## 🧶 Problem 3 (feat: Naver Open Api)<br>

이번 사이드 프로젝트에선 Naver Open Api를 사용하였다.

Naver Open Api를 사용하면서 CORS 때문에 proxy 설정을 하였는데, Netlify에 배포할 경우, Api가 정상 작동하지 않는다.

## 🪄 Solution 3<br>

### 🍸 첫번째 설정

```js
const PROXY = window.location.hostname === "localhost" ? "" : "/proxy";
const URL = `${PROXY}/v1/search/movie.json`;
// 뒷 부분은 사용하는 api에 따로 조금 다름.
// 나의 api는 영화 검색 api라서 movie.json

const getData = await axios.get(URL, {
  // ...
});
```

### 🥂 두번째 설정

- 최상위 경로 (가장 바깥 : .env 나 package.json 있는 곳) 에 `netlify.toml` 파일 생성 후 다음과 같이 작성해 준다.

```js
[[redirects]];
from = "/proxy/*";
to = "https://openapi.naver.com/:splat";
status = 200;
force = true;
```

모든 문제를 해결하니 드디어 배포에 성공하였다!

한 번에 되는 것이 없으니 신기하면서도 어렵다.

하나 하나 해결하면서 나아가는 과정이 재미있었다.

<br>

[사이드 프로젝트\_SHOW ME THE MOVIE](https://show-me-the-movie.netlify.app/)

<br>

---

[참고 자료 1](https://velog.io/@mochapoke/TIL-netlify%EB%A1%9C-%EB%B0%B0%ED%8F%AC%EC%8B%9C-proxy-%EC%85%8B%ED%8C%85%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)

[참고 자료 2](https://stackoverflow.com/questions/62033577/netlify-redirect-is-not-working-with-my-create-react-app)
