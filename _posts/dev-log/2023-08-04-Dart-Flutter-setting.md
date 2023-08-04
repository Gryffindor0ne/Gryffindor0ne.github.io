---
layout: post
title: "Dart & Flutter 설치 및 오류 해결"
date: 2023-08-04 10:23:40 +0900
category: dev-log
tags: dev-log
image:
  path: https://images.unsplash.com/photo-1519910416653-a864f95daa4f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=3276&q=80
---

# Dart & Flutter 설치 및 오류 해결

# Dart 설치

Flutter를 배우려고 알아보니 먼저 Dart를 배워야 한단다. 그래서 일단 Dart를 설치해보기로 했다.

나는 맥북이어서 homebrew를 통해 Dart를 설치했다.

```
brew tap dart-lang/dart
brew install dart
```

## 🧶 Problem 1

`brew install dart`를 실행하는 과정에서 오류가 발생하였다.

```js
Error: The following formula cannot be installed from bottle and must be
built from source.
  dart
Install the Command Line Tools:
  xcode-select --install
```

## 🪄 Solution 1

에러와 동시에 친절하게 커맨드 라인 도구를 설치하라고 알려준다.

`xcode-select --install`를 통해 xcode를 설치한 후 (생각보다 시간이 좀 오래걸림)
다시 Dart를 설치한다.

# Flutter 설치

이왕 하는거 Flutter도 설치해버리기로 했다.

```
brew install --cask flutter
```

## 🧶 Problem 2

`brew install --cask flutter` 실행 과정에서 오류가 발생하였다.

```js
Error: It seems there is already a Binary at '/usr/local/bin/dart'.
```

이미 Dart가 있다는 얘기.

## 🪄 Solution 2

```
brew remove dart
brew install --cask flutter
```

아까 설치했던 Dart를 제거하고 Flutter를 새로 설치한다.
Flutter 설치시 Dart가 자동으로 설치되어서 중복에 관한 오류가 발생하는 것이다.
