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
