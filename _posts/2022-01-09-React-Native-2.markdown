---
layout: single
title: "React Native 2"
categories: React_Native
excerpt: "React Navigation setting and Login"
header:
  image: /assets/images/empty_post_header.png
  # og_image: /assets/images/blogBlank.png
  # image_description: "A description of the image"
  # caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
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

귀찮게 안드로이드도 뭐 해야한다. `MainActivity.java`을 수정해야하는데, `android/app/src/main/java/<your package name>/com/MainActivity.java`에 있다. 아래 코드를 추가하면 된다. body에.

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

## Auth Structure

### 시작하기

사실 파일 구조를 어떻게 짜느냐는 자기 마음아니겠나? 하지만 내가 참고한 것은 'Do it! 리액트 네이티브 앱 프로그래밍'라는 책과 Nomad Coder의 AirBnB 클론 강의였다. 그러니까 비슷할 수도 있다. 근데, 앞에 책은 가뜩이나 처음하는 리액트 네이티브에 JS인데, TS까지하라고? 머리아파서 타입 지정따위는 없는 바닐라 JS를 쓰기로 결심한 후에 Nomad Coder의 인강을 사서 봤다.(대표님이 사줬다)

인강은 Expo로 진행하더라, 혹시 모를 확장성을 위해서 나는 React Native CLI로 했는데, 결국 본질은 비슷한 것 같다. 데이터를 받고 큰 틀을 만들고 아래 구조로 뿌려주는 다단계와 비슷한 형태로 컴포넌트를 만들더라.

그런데, 데이터를 전역으로 관리해야 하는 게 많았다. 그걸 위해서 Redux를 썼고, 서버 통신을 위해 Redux-Saga를 썼다. 서버도 내가 만들거니까 내 마음대로 데이터 구조를 그때그때 만들어서 썼는데, 지금와서 생각해보니까 진짜 힘들다. 데이터 구조 하나 고치려면 몇 개를 고쳐야하는 지 모르겠다.

<!-- 이미지 업로드 예시 -->
<!-- ![login_screen](/sweeny/assets/images/login_screen.png){: width="300" }{: .center} -->

이 게시물부터는 아무래도, 새로이 프로젝트를 만들고 예시를 만들어보면서 하는 것이 나을 것 같기에, 처음 시도해보는 typescript 등 새로운 것을 많이 넣어볼 예정이다. 그리고 연습용 bootstrap같은 UI kit을 사용할 예정이었는데, 마땅한게 없다... 보고 따라하는 수준일듯

|                                            iOS                                             |                                              Android                                               |                                             Web                                             |
| :----------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------: |
| ![first setting end](/sweeny/assets/images/ios_first.png "ios"){: width="50%" }{: .center} | ![first setting end](/sweeny/assets/images/android_first.png "android"){: width="50%" }{: .center} | ![first setting end](/sweeny/assets/images/web_first.png "web"){: width="300%" }{: .center} |

웹 버전으로 개발하는 부분은 [앱과 웹을 한번에 개발하기 - kimik](https://minify.tistory.com/28?category=704381)의 내용을 참고했다. 특히 typescript를 반영했다.

![file structure](/sweeny/assets/images/file_structure.png){: width=300 }{: .center}

그렇게 만들어진 파일 구조는 다음과 같다. 게이트라는 파일 안에 NavigationContainer를 넣어서 앞으로 생겨날 컴포넌트들을 감싸줄 것이다. 자 이제 어플을 만들 준비는 다 끝났다. 이제 어떤 어플을 만들 것인지 정하기만 하면 된다.

그 전에 생각해야할 문제가 하나 있었다.

---

### ESLint & Prettier

프로그래밍 언어들의 구조는 작성자가 누구냐에 따라 많은 부분이 다르고 이해하기 어려워진다. 그런 문제를 해결하기 위해 많은 사람들이 구조를 통일화하기 시작했다. 이런 통일을 자동으로 적용시켜주는 것이 **ESLint**와 **Prettier**다.
다른 추천 플러그인이나, AirBnB 플러그인 등 보편적으로 쓰이는 것들이 있는데, 나는 나만의 길을 가는 ...는 아니고 그냥 쓰다보니까 제일 내가 짜는 형태와 비슷하게 맞췄다. 보통 prettier나 eslint 문서를 참고해서 옵션을 재 설정했다. 그리고 vscode를 쓰고 있기 때문에 저장 시 자동 formatting을 해주는 기능과 함께 쓴다면 아주 환상적으로 편하다.

#### .eslintrc.js

```bash
module.exports = {
  parser: '@typescript-eslint/parser',
  plugins: ['prettier'],
  extends: ['plugin:@typescript-eslint/recommended'],
  parserOptions: {
    ecmaVersion: 2020,
    sourceType: 'module',
    ecmaFeatures: {
      jsx: true
    }
  },
  settings: {
    'import/parsers': {
      '@typescript-eslint/parser': ['.ts', '.tsx', '.js', '.jsx']
    },
    'import/resolver': {
      typescript: './tsconfig.json'
    }
  }
}
```

#### .prettierrc.js

```bash
module.exports = {
  parser: 'typescript',
  bracketSpacing: true,
  jsxBracketSameLine: true,
  singleQuote: true,
  trailingComma: 'none',
  arrowParens: 'avoid',
  tabWidth: 2,
  semi: false,
  printWidth: 100,
  endOfLine: 'lf'
}
```

vscode 세팅은 이곳저곳에 많이 있는데, `.vscode` 폴더에 `setting.json` 파일을 만들고 아래와 같이 적어주면 저장 시 마다 한 번 살펴보고 코드를 고쳐줄 것이다.

```bash
{
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    },
    "[typescript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    }
}
```

붙여넣기했을 때 등 포맷팅해주는 설정이 많은데, 그건 알아서 찾아보던가 `cmd + shift + p` 누르고 유저 설정가서 `format` 검색하면 많이 나오니까 찾아보길 바란다.

---

### Web, iOS, Android

![Web, iOS, Android](/sweeny/assets/images/all_capture.png){: width="100%" }{: .center}

모두 설정을 마치고 네비게이션 설정이 끝났다. 이제 화면마다 들어가야하는 기능과 디자인을 정하고 코딩하는 일 밖에 안남았다. 디자인은 아마 클론을 하는게 제일 빠를 것 같기 때문에 그렇게 할 예정이다. 해당 네비게이션은 `Native Stack Navigator`으로 RN에서 쓰기 좋은 네비게이션이다. 이전 회사에서 진행한 프로젝트에서는 이게 안드로이드에서 말썽을 일으켜서 iOS, Android를 나눠서 Android에서는 `Stack Navigator`로 진행했던 기억이 있다. 하지만 이 프로젝트는 최대한 `Native Stack Navigator`를 활용할 예정이고, 아마 더해서 `Bottom Tabs Navigator`를 이용할 것 같다. nested navigator도 추후 다뤄볼 예정이다.

![after file structure](/sweeny/assets/images/after_structure.png){: width=300 }{: .center}

설정이 완료된 파일 구조

---
