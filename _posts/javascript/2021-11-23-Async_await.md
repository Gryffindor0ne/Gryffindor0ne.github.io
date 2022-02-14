---
layout: post
title: "Async / await"
date: 2021-11-23 13:57:27 +0900
category: javascript
tags: asyncawait
image:
  path: https://images.unsplash.com/photo-1523966211575-eb4a01e7dd51?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=710&q=80
---

# Async / await

### `코어 자바스크립트(정재남 저)` 책을 기본으로 하여 자바스크립트를 공부하면서 내용을 정리하고자 한다.

---

# 04 Callback function<br>

## 🧶 Async / await

<br>

**Promise** Chaining이 많아지면 가독성이 현저히 떨어지는 문제가 발생하는데,

ES2017(ES8)에서는 **Promise** 의 단점을 보완하고자 **Async / await** 이란 새로운 방식이 도입되었다.

**Async / await** 은 완전히 새로운 것이라기 보다는 기존 **Promise**를 토대로 좀 더 간편한 API를 제공하는 녀석이라고 보면 된다.

이런 것을 `Syntactic sugar` 라고 한다.

![syntactic sugar](https://user-images.githubusercontent.com/79234473/139531285-342ab2ec-01fe-4f56-bb77-5ddf9189867d.jpeg)

<br>

---

## 🪃 Async<br>

### ⛳️ 기존 Promise 예시

![async1](https://user-images.githubusercontent.com/79234473/142979221-3b6f2ba6-7189-4474-bdbd-41f826408514.png)

기존 **Promise** 구문의 예시이다.

**Promise** 에서는 반드시 내부에서 resolve 나 reject 함수를 호출하여 상태를 완료시켜야만 결과가 도출된다.

> 그냥 리턴문을 적을 시 `Promise {<pending>}` 상태가 유지된다.

<br>

### 🪅 Async 사용<br>

![async2](https://user-images.githubusercontent.com/79234473/142979667-f6cccbd3-91d3-4ddc-8946-62c5f608e81e.png)

**async** 를 사용하면 코드블럭 ( `{}` ) 안에 있는 코드들이 자동적으로 **Promise**로 변경된다.

엄청 간단한 코드가 된다.

<br>

**async** 키워드를 함수 앞에 적어주기만 하면 된다.

![async3](https://user-images.githubusercontent.com/79234473/142979669-34d84b95-c7a3-4859-bedb-73117cae0809.png)

<br>

## 🪃 Await<br>

### ⛳️ 기존 Promise 예시

![async4](https://user-images.githubusercontent.com/79234473/142979676-2c6200d9-dac2-491c-8fd2-7d5e45f2e051.png)

기존 **Promise** 구문에서 `goHalloweenParty()`의 복잡한 체이닝은 콜백지옥을 연상시킨다.

**Async / await** 를 사용하면 좀 더 간결한 코드로 만들 수 있다.

<br>

### 🪅 Await 사용<br>

위의 코드를 바꿔보면,

![async5](https://user-images.githubusercontent.com/79234473/142979684-85c70c6d-d444-4691-b7f0-b460efd6088f.png)

### 🪢 POINT 🪢

### 🪞 **await** 은 **async** 키워드로 설정된 함수 안에서만 사용이 가능하다.

<br>

---

## 💁🏻‍♀️ Error handling<br>

### 👉🏼 **For Example**

![async6](https://user-images.githubusercontent.com/79234473/142979687-d63531d7-48ad-4956-a9fb-440174415983.png)

![async7](https://user-images.githubusercontent.com/79234473/142979693-8b97b7b5-35dd-42ba-8790-92a46832d859.png)

getHalloweenItem2 함수 내부에 에러를 발생시키면 현재 구문에서는 에러를 핸들링하지 못한다.

## 🪄 **Async / await** 에서는 `try / catch` 구문을 통해 에러 핸들링한다.

<br>

다음과 같이 수정하면 가능해진다.

![async8](https://user-images.githubusercontent.com/79234473/142979694-b5aa6a04-c15b-459e-a150-53976b46279c.png)

![async9](https://user-images.githubusercontent.com/79234473/142979696-da9e28e3-9002-4794-8211-c844f71ac898.png)

<br>

---

## 🔖 <span style="color:#b0bec5"> Additional </span>

## 🪡 **Promise.all**

<br>

다시 코드를 보자.

![async10](https://user-images.githubusercontent.com/79234473/142979698-017ca56b-ecaf-4aaf-9a99-d0402a1740c0.png)

getHalloweenItem1 함수와 getHalloweenItem2 함수는 각각 독립적인 함수이다.

굳이 위의 코드에서처럼 순차적으로 실행할 필요가 없다.

<br>

### 🪄 **Promise.all - Promise의 병렬 처리**

여러 개의 비동기 작업을 병렬적으로 처리하고 싶을 때 사용하는 것이 바로 **Promise.all** 이다.

![async11](https://user-images.githubusercontent.com/79234473/142979699-ac6298e7-c8c7-454d-bcd2-9df7c139b1ac.png)

<br>

다른 예시를 보면, 이해하기가 더 쉽다.

![async12](https://user-images.githubusercontent.com/79234473/142979703-a6a64b65-ac07-4420-b485-ff36d80afb21.png)

피자 만드는데 들어가는 토핑재료를 받아서 알려주는 함수가 있다고 치자.

3가지 종류의 피자가 만들어지는데 각각의 promise 를 병렬적으로 묶어서 실행하도록 **Promise.all**을 사용하고 있다.

<br>

## 🪡 **Promise.race**

<br>

여러 개의 비동기 작업 중 가장 먼저 끝나는 작업만 알고 싶다면 **Promise.race**를 사용하면 된다.

<br>

예시코드를 조금 바꿔보자.

![async13](https://user-images.githubusercontent.com/79234473/142979717-329997f1-025a-4403-a761-e8fcadb0989c.png)

getHalloweenItem2 함수의 시간을 3초로 변경하고, **Promise.race**를 실행하면 가장 먼저 실행되는 녀석만 출력된다.

<br>

![async14](https://user-images.githubusercontent.com/79234473/142979722-dff0071e-642f-4a11-93bc-0f0c5e26ea53.png)

피자가 만들어지는 시간을 토핑의 개수에 따라 다르게 설정하는 코드를 추가하였다.

**Promise.race**를 실행하면 3종류의 피자 중 가장 빠르게 만들어지는 피자인 토핑 2개의 `ham & cheese` 피자가 출력되고, 나머지는 무시된다.

<br>

---

## 🫖 Bonus<Br>

### 앞서 살펴보았던 코드를 이제 **Async / await** 으로 바꿔 보자.

### 👉🏼 **For Example** 1

![async15](https://user-images.githubusercontent.com/79234473/142979726-3c4f328f-a79a-4a71-84fa-aacb3cbf7935.png)

<br>

### 👉🏼 **For Example** 2

![async16](https://user-images.githubusercontent.com/79234473/142979730-ec079057-2ce2-49ef-96f5-2343b5e02266.png)

<br>

[코드 참고하려면](<https://github.com/Gryffindor0ne/studyNote/blob/main/JavaScript/Callback3(async_await).md>)

<br>

👉🏻 [Next Page](https://gryffindor0ne.github.io/javascript/2021-11-27-Closure1/)

---

## 참고자료

- [드림코딩 엘리\_비동기의 꽃 JavaScript async 와 await 그리고 유용한 Promise APIs](https://www.youtube.com/watch?v=aoQSOZfz3vQ)

- [Wes Bos](https://wesbos.com/javascript/12-advanced-flow-control/67-promises/#promiseall)
