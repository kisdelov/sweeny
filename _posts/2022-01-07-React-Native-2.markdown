---
layout: single
title: "React Native 2"
categories: React_Native
excerpt: "React Navigation setting and Login"
header:
  overlay_color: "#555"
  # og_image: /assets/images/blogBlank.png
  # image: /assets/images/blogBlank.png
  # image_description: "A description of the image"
  # caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
typora-root-url: ../
---

어플을 기획하고 페이지를 기획하고 이제 실행하면 단계에서 설정도 마무리됐고, 가장 먼저 해야했던 것은 Navigation을 세팅해야했다. 그래야 여러 페이지를 (싸)돌아다니면서 열심히 활동하지 않겠나. 그러면서 생각해 봐야하는 것은 로그인 쪽이었다. 서버와 통신하면서 회원가입, 로그인이 가능해야 했다.

## Navigation setting

[1편](http://127.0.0.1:4000/sweeny/react_native/React-Native-1/)에서 했던 프로젝트를 기반으로 진행하면, 나는 Yarn을 좋아하니까 Yarn을 통해서 React Navigation을 설치하자.

```bash
yarn add @react-navigation/native
```

그리고 네비게이션이 쓰는 필요한게 따로 있단다. 그거도 설치하자.

```bash
yarn add react-native-screens react-native-safe-area-context
```

다 설치되면, 이제 pods를 설치해야한다. ios 안할거면 안해도됨. 그런데 android나 ios안할거면 react native를 쓰는 이유가 너무 적지않나 둘 다 하자.

```bash
npx pod-install ios
```

귀찮게 안드로이드도 뭐 해야한다. `MainActivity.java`을 수정해야하는데, `android/app/src/main/java/<your package name>/MainActivity.java`에 있다. 아래 코드를 추가하면 된다. body에.

```bash
@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(null);
}
```

그리고 코드 맨 위에 아래꺼 필수인거 알제?

```bash
import android.os.Bundle;
```

그리고 이제 `NavigationContainer`로 전체 앱을 감싸주면 된다. `index.js`나 `App.js`에서 감싸면 된다. 아래처럼.

```bash
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';

export default function App() {
  return (
    <NavigationContainer>{/* Rest of your app code */}</NavigationContainer>
  );
}
```

---

## Auth Structure

### 파일 구조

사실 파일 구조를 어떻게 짜느냐는 자기 마음아니겠나? 하지만 내가 참고한 것은 'Do it! 리액트 네이티브 앱 프로그래밍'라는 책과 Nomad Coder의 AirBnB 클론 강의였다. 그러니까 비슷할 수도 있다. 근데, 앞에 책은 가뜩이나 처음하는 리액트 네이티브에 JS인데, TS까지하라고? 머리아파서 타입 지정따위는 없는 바닐라 JS를 쓰기로 결심한 후에 Nomad Coder의 인강을 사서 봤다.(대표님이 사줬다)

인강은 Expo로 진행하더라, 혹시 모를 확장성을 위해서 나는 React Native CLI로 했는데, 결국 본질은 비슷한 것 같다. 데이터를 받고 큰 틀을 만들고 아래 구조로 뿌려주는 다단계와 비슷한 형태로 컴포넌트를 만들더라.

그런데, 데이터를 전역으로 관리해야 하는 게 많았다. 그걸 위해서 Redux를 썼고, 서버 통신을 위해 Redux-Saga를 썼다. 서버도 내가 만들거니까 내 마음대로 데이터 구조를 그때그때 만들어서 썼는데, 지금와서 생각해보니까 진짜 힘들다. 데이터 구조 하나 고치려면 몇 개를 고쳐야하는 지 모르겠다.

잡소리는 많을 수록 안좋으니까, 바로 시작해보자.

![login_screen](/sweeny/assets/images/login_screen.png){: width="300" }{: .center}

우리는 어플 켜자마자 로그인 화면을 마주한다. 물론 로그인없이 바로 게시글이 보이게 할 순 있는데, 싫으니까 로그인하게x 하자. 애플은 소셜로그인을 넣으려면 애플 계정으로 로그인 할 수 있어야한다. 그러니까 일단 첫 시작은 소셜로그인이 없이 시작했다.

이 사진에서 알 수 있는 것은 3개의 하위 컴포넌트가 존재한다는 것이다. 뭐 따로 이름 붙이면 아래와 같은 구조를 갖는다.

```bash
Auth
  |---SignUp
  |---Login
  |---PasswordReset
```

이 게시글에서 살펴볼 부분은 SignUp이다. 사실 나머지는 회원가입만 되면 만사천리니까 빡세게 회원가입 시켜야한다.

---
