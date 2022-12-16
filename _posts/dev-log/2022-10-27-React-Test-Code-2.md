---
layout: post
title: "리액트 테스트코드 작성하기 2 : React test code 2"
date: 2022-10-27 09:15:00 +0900
category: dev-log
tags: dev-log
image:
  path: https://images.unsplash.com/photo-1461685265823-f8d5d0b08b9b?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3540&q=80
---

# 리액트 테스트코드 작성하기 2 : React test code 2

## React에서 테스트를 작성하는 방법 2

## 🔮 테스트코드 작성의 Tip <br>

### 🪜 Three "A"s

테스트코드 작성시 다음의 3가지 관점에서 작성하면 좋다.

`Arrange` (준비) : 테스트 데이터와 테스트 조건, 환경설정을 셋업한다.

`Act` (실행) : 테스트해야 하는 로직 실행한다.

`Assert`(단언) : 예상한 결과와 실행 결과를 비교한다.

![codee](https://user-images.githubusercontent.com/79234473/197983276-48454da8-95db-4e6d-9169-ae26bf15df04.png)

<br>

자, 그럼 이제 새로운 컴포넌트를 하나 만들어서 테스트코드를 작성해보자.

> src/components/Greeting.js

![code](https://user-images.githubusercontent.com/79234473/197982202-38bf60e3-2271-4756-8c10-948fa81f0a60.png)

<br>

> src/components/Greeting.test.js

![123123](https://user-images.githubusercontent.com/79234473/198006702-18d3bd21-2ac4-4bf1-a631-40af98cfe3d2.png)

render를 통해 테스트하고자 하는 컴포넌트를 랜더링한다. 테스트를 하기 위해서는 테스트 컴포넌트가 잘 랜더링되었는지를 가상DOM을 조사해야 한다. 이를 위해서 가상DOM에 액세스할 수 있게 해주는 screen을 사용한다. screen 뒤에 사용할 수 있는 함수는 많이 있는데 이 함수들의 주요 차이점은 에러가 발생하였을때 promise 반환 유무이다. find 함수가 promise를 반환한다.

자세한 것은 다음을 참고하면 된다. [Queries](https://testing-library.com/docs/queries/about)

여기서는 getByText함수를 사용하여 해당 엘리먼트를 가져온다.
export함수를 사용하여 그 결과를 확인하는데 .toBeInTheDocument 함수는 Html 엘리먼트가 문서 안에 있는지 확인하는 함수로서 여기서 해당 구문 테스트를 하기위해 사용한다.

<br>

테스트 결과가 실패로 나왔다.

<img width="1118" alt="333" src="https://user-images.githubusercontent.com/79234473/198006935-4e254045-b71b-43f1-8641-a047ff142fb5.png"  style="
border-radius: 7px">

여기서 중요한 것은 getByText함수의 인자로 넣은 문자열과 정확하게 일치해야지만 테스트를 통과하는데 Greeting 컴포넌트의 Hello World!는 느낌표가 있어서 정확한 매치가 이루어지지 않는다.

사실 이 함수의 두번째 인자로 `{exact: false}`를 넣을 수 있다. 그러면 좀 더 유연한 테스트가 가능하여 Hello World 가 포함되기만 하면 테스트에 통과한다. (두번째 인자가 생략되면 `{exact:true}` 가 default이다.)

<br>
![dsafs](https://user-images.githubusercontent.com/79234473/198011549-460ba464-be48-4b19-84a2-07c41ba2a095.png)

<br>

테스트를 수정하면 테스트가 통과한다.

<img width="792" alt="ㅊㅌㅍㅋ" src="https://user-images.githubusercontent.com/79234473/198018786-8aa551e0-3d92-4f96-a7fb-6d05647179ed.png" style="
border-radius: 7px">

---

### 🔗 테스트 그룹화 (Test Suite)

애플리케이션이 규모가 커지면 테스트도 많아지고 복잡해진다. 그리하여 애플리케이션의 하나의 특징이나 하나의 컴포넌트에 속한 모든 테스트를 그룹화하여 관리할 수 있다. describe 함수를 통해 테스트를 그룹화할 수 있다. (테스트 그룹을 test Suite라고 한다.)

![Asd](https://user-images.githubusercontent.com/79234473/198019443-9ddb29ab-0b5a-4146-ad11-01c4d4d03b40.png)

<br>

테스트를 실행해보면 그룹화한 테스트 그룹명(`Greeting component`)이 제일 상단에 표시된다.

<img width="815" alt="231" src="https://user-images.githubusercontent.com/79234473/198017805-38abe696-f67f-427e-8b9d-a935c6ea46c8.png" style="
border-radius: 7px">

Test Suites에 해당 그룹의 갯수가 표시되고 Tests에는 총 테스트 숫자가 표시되어 확인할 수 있다.

---

<br>

이제, Greeting 컴포넌트를 변경해보자. 버튼을 하나 생성하여 버튼이 클릭되지 않았을 경우와 클릭된 경우를 확인하는 테스트를 진행해보자.
먼저 컴포넌트를 다음과 같이 변경하였다.

> src/components/Greeting.js

![code1111](https://user-images.githubusercontent.com/79234473/198163981-20190afe-f12f-49a2-bf5c-352da6691a68.png)

<br>

> src/components/Greeting.test.js

1> 버튼을 클릭하지 않았을 경우 `만나서 반갑습니다` 구문이 잘 나오는지 확인하는 테스트

![ㅇㅁㄴㄹㅁㅇㄹ](https://user-images.githubusercontent.com/79234473/198171188-870492d5-15a5-4a80-826e-098862e31c2a.png)

<br>

2> 버튼을 클릭할 경우 `정말 정말 반갑습니다` 구문이 잘 나오는지 확인하는 테스트

![2132312](https://user-images.githubusercontent.com/79234473/198165768-730c2b0f-3645-4b21-8b5d-b6f4045ef080.png)

버튼이 클릭되었을 때를 확인하기 위해서 `@testing-library/user-event`의 `userEvent`를 사용한다. `userEvent`는 실제 화면에서 사용자 이벤트를 작동시키도록 돕는 객체이다.
여기서는 최신버전인 v14을 사용하였다. 기존 문법과 조금 차이가 있다. 공식문서에서는 `userEvent.setup()`을 사용하도록 권장하고 있다. 그 문법에 따라 테스트코드를 작성했다. [userEvent](https://testing-library.com/docs/user-event/intro)

Act 부분에서 실행할 로직을 작성해야 하는데 `getByRole`을 사용하여 버튼 엘리먼트를 가져올 수 있다. [ByRole](https://testing-library.com/docs/queries/byrole)

`getByRole`은 여러가지를 가져올 수 있는데 여기서는 버튼 엘리먼트를 가져오는데 사용한다. 가져온 버튼 엘리먼트를 통해 해당 버튼이 클릭되었는지를 `userEvent`를 이용하여 확인한다. `userEvent`뒤에 해당 이벤트(click)를 붙여 사용하면 된다. [click](https://testing-library.com/docs/user-event/convenience#click)

<br>

3> 버튼을 클릭한 경우 `만나서 반갑습니다` 구문이 사라지는지 확인하는 테스트

![ㅁㄴㅇㄹ](https://user-images.githubusercontent.com/79234473/198169535-458090e5-8177-45f6-873e-5ce097afb582.png)

버튼을 클릭한 경우 `정말 정말 반갑습니다`가 나오는 것과 동시에 `만나서 반갑습니다`가 사라져야 한다.
`getByText`는 무조건 테스팅 시점에 해당 값이 렌더링 되어있는지 아닌지를 검사하기 때문에 사라졌는지의 여부는 테스팅할 수 없다. 그래서 이 경우에는 `queryByText`를 사용해야 한다. `queryByText`는 해당 구문을 찾아본 후 존재하지 않으면 `null`을 반환한다.

expect 구문에서 `toBeNull` 메서드를 사용하여 `null`를 확인할 수 있다.

<br>

테스트를 확인하면 하나의 그룹과 개별 테스트가 모두 테스트 통과되었음을 확인할 수 있다.

<img width="1110" alt="ㅇㄹㅁㄴㅁ" src="https://user-images.githubusercontent.com/79234473/198171440-0928e099-5cd5-4117-8fe1-4d77ade5bb04.png" style="
border-radius: 7px">

<br>

### 🪔 전체 테스트 코드

> src/components/Greeting.test.js

![asdasd](https://user-images.githubusercontent.com/79234473/198172350-626478f4-cb81-47f6-8652-3020f16ae2f6.png)

---

### ⏱ 비동기 테스트

비동기 컴포넌트를 만들어 결과값이 제대로 랜더링되는지 확인하는 테스트를 만들어보자.

> src/components/Async.js

![sasadf](https://user-images.githubusercontent.com/79234473/198175915-5d11355e-0e60-4b7f-9312-dd1502ea688b.png)

<br>

> src/components/Async.test.js

![aasd](https://user-images.githubusercontent.com/79234473/198178130-4c571464-f44f-4ead-a2da-4433bc243789.png)

비동기 테스트를 진행할 때 실제로 서버에 Http 요청을 보내지 않는다. 요청이 많을 경우 서버에 과부하가 걸릴 수 있고 만약 post 요청을 보내게 된다면 DB에 해당 내용이 저장될 가능성이 존재하기 때문이다.

그래서 비동기 테스트시에는 실제로 요청을 보내지 않고 테스트 하는 경우, 일종의 테스트 서버로 요청을 보내 테스트하는 경우 두 가지 방식으로 테스트를 할 수 있다. 여기서는 첫번째 방식인 실제 요청을 보내지 않고 테스트하는 방식을 사용했다.

<br>

![qwe](https://user-images.githubusercontent.com/79234473/198198483-c91be25d-4fe5-4c4f-a3e9-92a615fe5b00.png)

jest.fn()을 활용하여 mock function 객체를 만들 수 있다. [Jest-mockFn](https://jestjs.io/docs/mock-functions)

내장함수 fecth함수를 mock 함수로 대체하여 사용한다. 실제로 API 요청을 하지 않고 fetch 함수를 대신하는 mock 함수를 사용하여 테스트를 진행하기에 불필요한 서버 요청이 사라진다.

`mockResolvedValueOnce`는 (한 번만) 비동기 mock 함수가 이행(Resolved) 되면 지정한 값을 반환한다. 실제 API요청시 반환될 값을 설정하여 테스트가 가능하다.

<br>

![ghfghm](https://user-images.githubusercontent.com/79234473/198178557-12495a00-93f3-4686-9543-7ea77cf1e600.png)

단편적으로 보면 `listitem`의 결과값이 복수의 값으로 예상되기에 `getAllByRole`을 사용해야 한다. 그러나 `getAllByRole` 를 사용하면 테스트 통과가 되지 않고 실패한다. 그 이유는 비동기 작업은 작업이 완료되기 전에는 내용이 없기 때문이다.

그래서 비동기 테스트에는 `findAllByRole`을 사용해야 한다. `findAllByRole`은 promise를 반환하는 쿼리이기 때문이다. 또한 비동기 작업임으로 `async / await`을 사용한다.

export 구문에서 .toHaveLength 메서드를 사용하여 빈배열이 아닌 것을 확인하는 것으로 테스트를 실행한다.
