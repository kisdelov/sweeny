---
layout: single
title: "First setting for React Native"
date: 2022-01-02 15:20:15 +0900
categories: ReactNative
---

RN(으)로  처음 어플을 만들기로 결정하고 나서 첫 세팅을 하기 너무 어려웠다. React를 먼저 접한 후 였기 때문에 껌이라고 생각한 것은 오산이었다. 네이티브 설정까지 해야했기 때문이다. 그래서 언젠가는 다시 설정해야할 것이기 때문에 기록해둔다.

Mac을 기반으로 진행했기 때문에 윈도우 설명은 나도 모른다.

---

## 개발 환경 구축

RN 앱 개발은 Expo CLI와 RN CLI로 개발 할 수 있다. Expo는 편리하고 간단하게 어플을 만들 수 있지만, Expo에서 지원되지 않는 네이티브 모듈이 있기 때문에 추천하지는 않는다. 하지만 간단하게 진행된다면 기본 제공되는 기능이 많기 때문에 좋은 방법이 될 수 있다.

---

### Homebrew 설치 ([홈페이지](https://brew.sh/))

일단 설치를 진행하자. 맥에서 필요한 패키지를 관리한다고 하는데 잘모르겠는데, 일단 설치하자. Homebrew를 쓰는 일은 그렇게 많지 않다.

터미널에 아래와 같이 복사하면 설치된다.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

설치 완료 시 버전을 확인할 수 있다.

```bash
brew --version
```

---

### Node.js 설치

RN(은)는 JavaScript 기반으로 작성된다. JavaScript는 브라우저에서 작동하는데, Node.js로 브라우저 밖에서 코드를 실행할 수 있다고 한다. 런타임 환경으로 JavaScript로 다 할 수 있게 해준단다.

설치 명령어

```bash
brew install node
```

설치 확인 명령어

```bash
node -–version
```

---

### Watchman 설치

RN은 소스코드의 추가, 변경이 발생하면 다시 빌드를 해서 보여주는데, 이를 위해서는 Watchman이 필요하단다. 이름처럼 감시하는 역할을 하는 듯.

설치 명령어

```bash
brew install watchman
```

설치 확인 명령어

```bash
watchman –version
```

---

### React Native CLI

RN으로 앱을 개발하기 위해 필요한 React Native CLI를 설치해야한다. 이제 본격적이다.

npm 명령어를 통해 설치하는데, 잠깐 npm에 대해 알아보자면, JavaScript를 위한 패키지 관리자로 Node.js 환경의 기본 패키지 관리자란다. 대안으로는 ied, pnpm, npmd, Yarn 등이 있고 Yarn은 페이스북(메타)에서 공개한거라고 한다. 나는 거의 Yarn을 사용했다.

yarn을 설치하기 위해서는 위에 설치된 것을 이용하면 두 가지 방법이 있다.

```bash
brew install yarn
```

```bash
npm install -g yarn
```

두 가지 중에 아무거나 사용해도 괜찮다.

yarn을 설치했으니 yarn으로 React Native CLI 설치했다.

```bash
yarn add reac-native-cli
```

설치 버전 확인

```bash
npx react-native --version
```

특이한 점은 npx를 사용한다는 것인데, npm은 node package manager의 줄인 말이면 npx는 node package excute의 줄인 말이란다. 실행할 때 쓰는 명령어 인듯

---

### [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)

RN으로 iOS앱 개발을 위해서는 Xcode가 필요하다. 앱스토어에서 Xcode 설치해도 되고, 링크걸었으니까 가서 설치해도 된다.

Xcode 설치 완료 후 **Xcode -> Preferences -> Location**로 가면 Command Line Tools가 제대로 설정됐는지 확인하면 된다.

---

### Cocoapods

Cocoapods는 iOS 개발에 사용되는 의존성 관리자라고 하는데, Xcode에서 사용되는 언어인 Swift, Object-C로 이뤄진 프로젝트의 npm같은 성격의 관리자인듯

설치 명령어

```bash
sudo gem install cocoapods
```

여기서 궁금한 것은 sudo와 gem이었다.
**sudo**란 유닉스, 유닉스 계열 운영 체제(Mac또한 여기에 포함)에서 슈퍼유저로서 프로그램을 구동하는 프로그램이다. 윈도우의 관리자 권한 실행과 비슷한 것 같다. 원래는 **superuser do**에서 유래했는데, 후에 기능이 확장되면서 **substitute user do(다른 사용자의 권한으로 실행)**의 줄인말로 해석됐다. 뭐 대충 관리자 권한으로 실행한다고 보면 될듯. 비밀번호를 요구하니까 로그인할 때 쓰이는 비밀번호를 입력한다.
**gem**은 Ruby의 package manager란다. 겁나많다. 패키지매니저. 맥 OS에는 기본적으로 ruby가 설치되어 있다. 그러니까 아마 위에 설치 명령어는 바로 실행될 것 같다.
cocoapods는 Ruby로 이뤄진 것 같다.

설치 확인 명령어

```bash
pod --version
```

---

### JDK(Java Development Kit)

안드로이드 앱은 Java를 베이스로 개발한다. 그러니까 Java 개발 킷이 필요하다.

설치 명령어

```bash
brew tap AdoptOpenJDK/openjdk
brew cask install adoptopenjdk8
```

설치 확인

```bash
java -version
```

---

### [안드로이드 스튜디오](https://developer.android.com/studio)

iOS는 Xcode, Android는 Android Studio다. 설치 주소는 링크걸었다.

설치는 알아서 하시고 SDK 설정을 해야한다.

오른쪽 상단에 ...있을 건데, 뭐 다른 버전이라면 알아서 설정 같은거 찾으시고 누르면 **SDK Manager**에 들어갈 수 있다. SDK 위치 설정도 잘 되어 있는지 확인한 다음, **Show Package Detail** 활성화한 다음 안드로이드의 최신 SDK를 체크하고 OK를 눌러주자.

- 예시
  - Android SDK Platform 29
  - Intel x86 Atom System Image
  - Google APIs Intel x86 Atom System Image
  - Google APIs Intel x86 Atom_64 System Image

이제 환경 변수를 설정해야 한다.

~/.bash_profile 파일 또는 ~/.zshrc 파일을 열고 아래와 같이 수정해야 한다.

```bash
# export ANDROID_HOME=$HOME/Library/Android/sdk
export ANDROID_HOME=자신의 안드로이드SDK 위치/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

나는 Vscode를 사용하는데, 걍 파일 위치가서 열어서 복붙하고 저장했다.

뭐... 터미널에서 vim으로 수정해도 되는데, 이게 편한듯

설정 확인 명령어

```bash
adb
```

### React Native 프로젝트 생성

이제야 겨우 프로젝트 생성할 수 있다. 설정할게 뭐가 이리많은지, 그런데 다른 프로젝트를 시작할 때도 보통 이정도는 기본인 것 같다. 다 설정하고 뭐하다보면 256gb의 내 맥북은 한계를 겪곤 했다. 다음엔 더 큰 용량으로 사야지...

기본 버전

```bash
npx react-native init 프로젝트 이름
```

다른 버전으로 하기

```bash
npx react-native init 프로젝트 이름 --version x.xx.x
```

typescript로 시작하기

```bash
npx react-native init 프로젝트 이름 --template react-native-template-typescript
```

이번 프로젝트는 javascript로 했는데, 아마 typescript로 피벗도 고려하고 있다. Javascript로 하다보니까 확실히 type 지정이 왜 좋은 지 체감됐다.

```bash
cd 프로젝트 이름

# 메트로 서버 실행
npm start

# iOS 실행
npm run ios

# Android 실행
npm run android
```

이정도면 대충 세팅이 끝났다. 다음에는 navigation부터 시작될 듯
