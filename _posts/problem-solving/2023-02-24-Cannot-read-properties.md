---
layout: post
title: "Cannot read properties of undefined (reading 'content') 에러해결 - (ft. react-toastify)"
date: 2023-02-24 11:20:34 +0900
category: problem-solving
hide_description: false
tags: problem-solving
image:
  path: https://images.unsplash.com/photo-1612933510543-5b442296703b?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1964&q=80
---

# Cannot read properties of undefined (reading 'content') 에러해결 - (ft. react-toastify)

## 🧶 Problem

<br>

개인 프로젝트 react-query 리팩토링 중 잘되던 react-toastify가 작동하다가 에러를 뱉어냈다.

<img width="878" alt="error" src="https://user-images.githubusercontent.com/79234473/221079357-4e45ccc4-0725-4dbb-925d-c7c3bec7c822.png">

<br>

그룹을 생성하거나 수정할 때는 toast가 잘 작동되는데 삭제할 때만 오류가 발생하였다. 로직의 다른 점이 하나 있다면 삭제는 중요하기에 확인 modal이 한 번 더 뜨는데 확인 modal을 클릭하면 toast가 작동되다가 에러가 발생하였다. 아마도 modal과 toast가 충돌하고 있는 것으로 파악했다.

<br>

<img width="430" height="600" alt="modal" src="https://user-images.githubusercontent.com/79234473/221079709-250f9469-856e-4fa5-820c-d1a66d44b62b.png">

<br>

## 🪄 Solution

<br>

React-toastify Github에서 해당 이슈를 검색해보니 똑같은 오류에 관한 이슈를 찾았다.

```
Uncaught TypeError: Cannot read properties of undefined (reading 'content') #858
```

<br>

이슈를 살펴보니 다음과 같은 해결책을 찾을 수 있었다.

```
 I was having the same error
 TypeError: Cannot read properties of undefined (reading 'content')

 I resolved it by adding a small delay in the notification appearance
```

```js
eg-

const sendTransaction = () => {
    toast.promise(
        writeAsync,
      {
          pending: "Promise is pending",
        success: "Promise resolved 👌",
        error: "Promise rejected 🤯",
      },
      {
          position: toast.POSITION.BOTTOM_LEFT,
        delay: 100,
      }
    );
  };
```

### 💥 [참고 방법](https://github.com/fkhadra/react-toastify/issues/858#issuecomment-1422593857)

<br>

해당 방법을 나의 코드에 적용했다.

```js
  const { mutate } = useMutation(
    (targetId: TargetId) => removeGroup(targetId),
    {
      onSuccess: () => {
        queryClient.invalidateQueries([queryKeys.groupsData]);
        toast.success('문장집 삭제완료', {
          position: 'top-center',
          autoClose: 300,
          delay: 100,
        });
      },
     ...
    }
  );
  return mutate;
};
```

에러없이 toast가 잘 작동되었다.

<br>

---

## 참고 🫧

#### [react-toastify issues](https://github.com/fkhadra/react-toastify/issues/858)
