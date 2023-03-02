---
layout: post
title: "Recoil: Duplicate atom key 에러처리 (Next.js)"
date: 2023-03-02 10:10:10 +0900
category: problem-solving
hide_description: false
tags: problem-solving
image:
  path: https://images.unsplash.com/photo-1609643242070-c69786a76c30?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1332&q=80
---

# Recoil: Duplicate atom key 에러처리 (Next.js)

## 🧶 Problem

<br>

프로젝트에 recoil을 도입하는 과정에서 다음 에러가 발생하였다.

```
Expectation Violation: Duplicate atom key "keyname". This is a FATAL ERROR in
      production. But it is safe to ignore this warning if it occurred because of
      hot module replacement.
```

Next.js에 recoil을 사용하게 되었을 때 발생하는 에러로 Next.js에서 개발 중에 파일 변경시 rebuild 되는 과정에서 atom key가 재선언되어 발생하는 에러이다.

<br>

## 🪄 Solution

<br>

검색을 통해 해결방안을 찾아보았는데 몇가지 방법 중 환경변수를 입력하는 방법으로 해결하였다.

.env 파일에 다음의 구문을 추가하면 된다.

<br>

```js
RECOIL_DUPLICATE_ATOM_KEY_CHECKING_ENABLED = false;
```

<br>

---

## 참고 🫧

#### [Stack overflow](https://stackoverflow.com/questions/65506656/recoil-duplicate-atom-key-in-nextjs)

#### [Recoil 공식문서](https://recoiljs.org/ko/blog/2022/10/11/recoil-0.7.6-release/)
