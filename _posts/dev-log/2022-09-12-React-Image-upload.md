---
layout: post
title: "React Image Upload (feat.Firebase)"
date: 2022-09-12 13:20:00 +0900
category: dev-log
tags: dev-log
image:
  path: https://images.unsplash.com/photo-1508802551395-fbecf2af43b1?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1212&q=80
---

# React Image Upload (feat.Firebase)

## React에서 이미지 업로드하는 방법 (feat.Firebase)

프로젝트 리팩토링하는 과정에서 이미지 업로드 기능을 추가하려고 찾아보다가 Firebase를 통한 이미지 업로드 및 이미지 미리보기 기능 구현에 대해 알게 되어 공유하고자 글을 적는다.

이전 프로젝트에서 AWS S3를 이용하여 이미지 업로드를 구현한 적이 있는데 그 방법과 유사한 점이 많이 있었다. 비교해보면서 공부하면 좋을 것 같다.

---

### Firebase 설정

먼저 Firebase를 사용하기 위해서는 Google 계정이 있어야 한다. 구글 계정으로 로그인한 후 Firebase console 페이지로 들어가면 다음과 같은 페이지가 나온다.

![111](https://user-images.githubusercontent.com/79234473/189578275-c39d1282-aa60-4684-a465-603c8e61f087.png)

현재 1개의 프로젝트를 생성하여 사용중이라서 essay-note라는 프로젝트 하나가 보여지고 있는데 처음 사용하는 것이라면 아무 것도 나타나지 않는다.

이미지 업로드를 하기 위해서는 먼저 프로젝트를 생성해야한다. '+ 프로젝트 추가' 부분을 클릭하여 프로젝트를 생성해보자.

![222](https://user-images.githubusercontent.com/79234473/189578946-228209f8-856a-458d-ae99-e191a99dd645.png)

프로젝트 이름을 입력하고 약관에 동의하여 프로젝트를 생성한다. 여기서 예시로 Photo-island라는 이름의 프로젝트를 하나 생성했다.

![44](https://user-images.githubusercontent.com/79234473/189580072-f1603da1-f2e0-4a02-94ce-88ffea5d3bb8.png)

![55](https://user-images.githubusercontent.com/79234473/189580063-2ad4ee4e-0937-4450-97db-f641952c41fc.png)

모든 단계가 완료되었다. 계속을 누르면 생성한 프로젝트 페이지로 리다이렉션된다.

![66](https://user-images.githubusercontent.com/79234473/189580338-57eb89f1-0e84-4e56-b0a5-298d20485936.png)

자 이제 Photo-island라는 프로젝트가 생성되었다.

다음으로 진행할 것은 왼쪽 사이드바의 '빌드' 메뉴를 클릭하여 'storage' 메뉴로 들어간다.

![1212](https://user-images.githubusercontent.com/79234473/189580983-74eb348a-394b-4fc6-b72f-c16734aede2b.png)

시작하기 버튼을 클릭하면 Cloud Storage 설정 부분이 나오는데 여기서는 테스트모드를 클릭하고 다음으로 넘어간다.

![2323](https://user-images.githubusercontent.com/79234473/189580977-786ab7b6-cac2-4dff-9453-06c728e64027.png)

Cloud Storage 위치 설정까지 완료하면 버킷을 생성한다.

![3434](https://user-images.githubusercontent.com/79234473/189580970-1978141b-3c26-4019-b25d-1ff7c3bb7df5.png)

모든 단계가 완료되면 빈 스토리지 버킷이 생성된다.

![555](https://user-images.githubusercontent.com/79234473/189581267-72740958-290b-4ccd-b381-018a51ea8477.png)

---

### React와 Firebase 연결

그럼 React 앱에서 Firebase를 연결하여 이미지를 업로드해보자.

현재 진행중인 React 앱이 있으면 바로 사용하면 되고, 없다면 새로 React 앱을 생성하여 사용하면 된다.

해당 프로젝트 폴더로 들어가서 먼저 Firebase 설치를 해준다.

```
npm i firebase
```

Firebase를 사용하려면 해당 프로젝트를 연결해야 한다.

![3232](https://user-images.githubusercontent.com/79234473/189584477-ec57f5c1-a3c3-4bda-980b-438ce9bd50be.png)

새로 생성한 프로젝트로 들어가서 웹 아이콘을 클릭하여 웹용 Firebase 프로젝트를 만든다.

![132312](https://user-images.githubusercontent.com/79234473/189585041-df3837df-190b-4d55-83e1-9a878ec194b9.png)

앱의 이름을 지정하고 앱을 등록합니다.

![234124](https://user-images.githubusercontent.com/79234473/189585491-20f13ae9-2c94-4976-bcf8-b065fc528166.png)

<br>

다음 단계로 이동하면 firebaseConfig 객체가 표시된다. 추후 Firebase를 초기화하는 데 필요하므로 firebaseConfig 객체를 클립보드에 복사해둔다. 그런 다음 콘솔로 계속 버튼을 클릭하여 프로세스를 완료한다.
(firebaseConfig 객체는 프로젝트 설정에 들어가면 다시 볼 수 있어 꼭 복사해두지 않아도 된다.)

<br>

자, 그럼 React에서 Firebase를 사용할 수 있도록 해보자.

src 폴더에 firebase.js 파일을 만들고 다음 코드를 추가한다. (파일 구조는 각자 알아서~ 내 프로젝트에서는 apis 폴더 생성후 그 안에 firebase.js 파일을 생성했다.)

![code](https://user-images.githubusercontent.com/79234473/189589120-0416040a-0947-4c2d-9984-ddf4f3866bb2.png)

그리고 .env 파일 생성하여 React 환경변수 설정을 firebaseConfig 객체 내용으로 해주면 된다.

```
// .env

REACT_APP_FIREBASE_API_KEY= ...
REACT_APP_FIREBASE_AUTH_DOMAIN= ...
REACT_APP_FIREBASE_PROJECT_ID= ...
REACT_APP_FIREBASE_STORAGE_BUCKET= ...
REACT_APP_FIREBASE_MESSAGING_SENDER_ID= ...
REACT_APP_FIREBASE_APP_ID= ...
REACT_APP_FIREBASE_MEASUREMENT_ID= ...
```

---

### Firebase 사용한 이미지 업로드 및 미리보기 구현

Firebase storage에 이미지를 업로드하기 위해서는 uploadBytes 혹은 uploadBytesResumable 함수를 사용하는데 둘다 동일한 결과를 얻지만 uploadBytesResumable 함수는 업로드 진행상황을 체크하고 일시중시, 취소 같은 특정 작업을 할 수 있다.

일단 uploadBytes 함수를 사용한 이미지 업로드 예시를 보자.

> 참고사항 : 진행하던 프로젝트는 Typescript로 작성되었으며, 이미지 업로드 기능을 할 컴포넌트를 따로 생성하여 진행하였다.

<br>

✨ UploadImage.tsx

![code1](https://user-images.githubusercontent.com/79234473/189645602-2fe80507-825d-4b4b-bc86-292fdc1f0c98.png)

<br>
먼저 위의 코드를 추가한다.

<br>

그리고, 다음의 코드를 입력하여 이미지 업로드를 구현한다.

![code2](https://user-images.githubusercontent.com/79234473/189648464-4350d349-6a92-4d33-86cc-e5f32541e3aa.png)

ref함수를 사용하여 파일을 업로드하려는 Firebase storage에 대한 참조를 만든다.
`files/${file[0].name}` 는 Firebase storage에 files이란 폴더를 만들고 그 안에 업로드할 이미지의 이름으로 이미지를 저장한다는 의미.

uploadBytes 함수는 ref로 만든 참조와 해당 파일을 매개변수로 하여 파일을 업로드한다.
업로드가 완료되면 getDownloadURL 함수는 업로드된 파일의 URL을 다운로드한다.
getDownloadURL 함수로 받은 이미지 URL을 이용하여 이미지 미리보기 기능을 구현한다.

<br>

---

이미지 업로드시 이미지 업로드 진행사항을 확인하고 미리보기까지 보여지게 하고 싶다면 uploadBytesResumable 함수를 사용하면 된다.

![code3](https://user-images.githubusercontent.com/79234473/189650495-da87205c-0205-4578-88ee-13cef9228943.png)

<br>

> 이미지 업로드 진행 과정

<br>

![214112](https://user-images.githubusercontent.com/79234473/189652261-77abf9d6-1bf0-4280-9026-669def6dfdf1.png)

<br>

![1241245245](https://user-images.githubusercontent.com/79234473/189652264-68cfba90-ab56-4e47-876e-9f84411a6624.png)

<br>

![qweqw](https://user-images.githubusercontent.com/79234473/189652440-749b0dc2-f21c-43b7-8989-9bb595406ca2.png)

<br>

🖥 progress bar 디자인은 Material UI, 전체 디자인은 styled-component 사용.
