---
layout: post
title: "리액트 테스트코드 작성하기 :React test code"
date: 2022-10-26 13:45:00 +0900
category: dev-log
tags: dev-log
image:
  path: https://images.unsplash.com/photo-1461685265823-f8d5d0b08b9b?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3540&q=80
---

# 리액트 테스트코드 작성하기 :React test code

## React에서 테스트를 작성하는 방법

---

### 📯 Test 란?

일반적으로 말하는 테스트는 수동 테스트(Manual Testing)로 코드를 작성하여 그 코드가 브라우저에서 잘 실행되는지 체크하는 일련의 과정을 말한다. 그러나 이런 수동 테스트는 많은 에러를 잡아내기 어렵다. 다양한 모든 조합과 시나리오가 있기 때문이다.

### ⌚️ 자동화 테스트 (Automated Testing)

수동 테스트(Manual Testing)도 중요하지만 자동화 테스트를 추가적으로 진행한다면 많은 장점이 있다.

#### ⚙️ 왜 자동화 테스트를 해야 하는가?

규모가 크고 아주 복잡한 앱들은 많은 페이지와 속성을 가지고 있는데 새로운 속성을 추가하거나 기존 속성을 변경할 때 당연히 해당 사항에 대한 테스트를 하겠지만 나머지 많은 부분을 항상 테스트하지는 못한다. 코드 작성자가 새로운 추가를 하거나 기존 것을 변경하였을 때 의도치않게 다른 부분을 손상시킬 가능성이 발생한다.

자동화된 테스트는 애플리케이션 전체를 자동으로 테스트하기에 항상 모든 것을 테스트 할 수 있다. 이렇게 함으로써 오류를 빨리 찾아낼 수 있고 더 나은 코드를 작성할 수 있다.

#### 자동화 테스트의 종류

🩺 단위 테스트(Unit Tests)

- 앱의 가장 작은 가능한 단위(개별 함수들, 개별 컴포넌트들)에 대한 테스트를 작성하는 것.
- 한 프로젝트에는 많은 개수의 단위 테스트가 포함된다.
- 가장 일반적이며 중요한 테스트.

🩺 통합 테스트(Integration Tests)

- 다수의 구성 요소 조합을 테스트. (단위 별로 모아서 테스트)
- 한 프로젝트에는 보통 몇개의 통합 테스트가 포함된다.

🩺 E2E 테스트(End-to-End Tests)

- 프로젝트가 실제로 사용될 때 제대로 작동하는지 사용자 관점에서 테스트.
- 수동 테스트로 하는 것을 자동화한 것이라고 보면 된다.

---

## Jest & React-testing-library 를 이용한 테스트 작성하기

CRA를 통해 리액트 앱을 설치하면 자동적으로 `Jest`와 `React-testing-library`가 설치되어 있다.

CRA로 생성된 리액트 앱에는 기본적으로 App.js와 App.test.js가 생성되어 있다. 통상적으로 테스트코드는 `.test.js`를 확장자로 사용하며 해당 컴포넌트 이름명 뒤에 붙여서 사용한다.

자동으로 생성된 App.test.js를 살펴보면 기본적으로 테스트 코드가 적혀있다.

![ㄷㄱ23](https://user-images.githubusercontent.com/79234473/197984136-97de0f98-014b-4a52-935d-7fe466ef276a.png)

`learn react`라는 글자가 App 컴포넌트를 랜더링 했을 때 존재하는지를 테스트하는 코드이다.

테스트 코드의 첫번째 인자인 'renders learn react link' 는 테스트 설명으로 테스트 코드를 실행할 때 나타나는 구문이다. 원하는 구문으로 적으면 된다.

두번째 인자는 테스트 코드가 적힌 익명함수이다. 해당 함수에 테스트할 내용을 적어 실행한다.

```js
() => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
};
```

- App.js

![123](https://user-images.githubusercontent.com/79234473/197984556-62ba947b-5e27-42ef-b237-ab57b93e9a93.png)

App 컴포넌트를 테스트하기 위해 `npm test`를 터미널에 입력하면 App.test.js가 실행된다.

App.js에는 테스트하고자 하는 'learn react' 구문이 존재하기에 다음과 같이 테스트가 통과하는 것을 볼 수 있다.

<br>

<img width="536" alt="2313" src="https://user-images.githubusercontent.com/79234473/197956483-9c5541a0-fe77-4062-be64-9d2526a70b02.png"  style="
border-radius: 7px">

<br>

그럼, 테스트가 틀리면 어떻게 될까?

- App.js

![33333](https://user-images.githubusercontent.com/79234473/197985633-905661a7-6715-4e15-8a0c-a15de94ae86c.png)

App 컴포넌트를 수정해보았다. 그리고 테스트를 실행하였다.

<br>

<img width="797" alt="23213" src="https://user-images.githubusercontent.com/79234473/197961341-659bfa6d-bfc3-4800-b590-5ccd2d803810.png" style="
border-radius: 7px">

App 컴포넌트 내에 `learn react` 구문이 없다는 에러 메세지가 뜨면서 테스트가 실패한다.
