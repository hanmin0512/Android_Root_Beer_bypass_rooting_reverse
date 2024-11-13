# 안드로이드 앱 Root Beer 루팅탐지 모든 조건 걸리게 하기

## 개요

안드로이드 앱 후킹 실습 중 루팅 탐지 우회를 반대로 하여 후킹 역량을 강화하기 위한 프로젝트이다.
환경은 구성된 상태라고 가정하에 진행할 것이며, 동적 분석을 통해 앱 후킹을 진행하여 루팅 탐지에 걸리게 할것이다.
또한 이전에 한 루팅 탐주 우회 기법에 나와 있는 기본적인 설명은 생략한다.

### 루팅 탐지 우회 보러가기
<a href = "https://github.com/hanmin0512/Android_Root_Beer_bypass_rooting/"> 보러가기 </a>

### 앱 특징
이 앱은 루팅(탈옥)이 된 상태인지 검사를 한다. 루팅이 되어있음을 가정하고 동적 앱 후킹을 이용해 루팅 탐지의 모든 조건에 걸리게 할 것이다.

## 수행 방안

### 점검 도구

1. frida
2. JADX
3. Nox (adb)

### 수행 환경

OS: Android (NoX)

Application: RootBeer Sample

## 수행 및 수행 결과

### 수행

#### 앱 그냥 실행

![image](https://github.com/user-attachments/assets/555f602e-664d-4f82-bf74-1d1ebd5174b9)

![image](https://github.com/user-attachments/assets/b850727c-c36c-4c27-b559-40b96bb7741e)

#### 후킹 해야 할 함수 목록

- Root Management Apps: detectRootManagementApps
- Potentially Dangerous Apps: detectPotentiallyDangerousApps
- Root Cloaking Apps: detectRootCloakingApps
- TestKeys: detectTestKeys
- For RW Paths: checkForRWPaths
- Dangerous Props: checkForDangerousProps
- Root via native check: checkForRootNative
- SE linux Flag is Enabled: isSelinuxFlagInEnabled
- Magisk specific checks: checkForMagiskBinary

#### SE linux Flag is Enabled 함수 후킹 과정

다른 함수들은 com.scottyab.rootbeer.RootBeer 소스코드에 존재하여 쉽게 찾아 후킹을 진행 할 수 있었다.

하지만 SE linux Flag is Enabled 항목인 isSelinuxFlagInEnabled 다른 위치에 있어 탐지에 걸리게 하기 어려웠다.

![image](https://github.com/user-attachments/assets/78e36932-de37-437c-a21f-2fecd1945664)

JADX 툴은 클래스, 메소드, 필드, 코드, 리소스, 주석에 대한 검색 기능을 제공한다.
위 그림의 빨간색 박스의 3개중 아무거나 하나 클릭한다.

![image](https://github.com/user-attachments/assets/f0a0d377-3da7-460d-8b07-0c602f7bd405)

위 그림과 같이 메소드에 체크해 준 후 linux를 검색한다.
검색 결과가 boolean형이고 메소드 이름이 SE linux Flag is Enabled와 매우 유사한 것을 보아 후킹해본다.

#### 후킹 코드 작성

```javascript

// beer_reverse.js
Java.perform(function(){

    let RootBeer = Java.use("com.scottyab.rootbeer.RootBeer");
    RootBeer["detectRootManagementApps"].overload().implementation = function () {
        console.log(`RootBeer.detectRootManagementApps is called`);
        let result = !this["detectRootManagementApps"]();
        console.log(`RootBeer.detectRootManagementApps result=${result}`);
        return result;
    };

    
    RootBeer["detectPotentiallyDangerousApps"].overload().implementation = function () {
        console.log(`RootBeer.detectPotentiallyDangerousApps is called`);
        let result = !this["detectPotentiallyDangerousApps"]();
        console.log(`RootBeer.detectPotentiallyDangerousApps result=${result}`);
        return result;
    };
    
    
    RootBeer["detectTestKeys"].implementation = function () {
        console.log(`RootBeer.detectTestKeys is called`);
        let result = !this["detectTestKeys"]();
        console.log(`RootBeer.detectTestKeys result=${result}`);
        return result;
    };

    
    RootBeer["detectRootCloakingApps"].overload().implementation = function () {
        console.log(`RootBeer.detectRootCloakingApps is called`);
        let result = !this["detectRootCloakingApps"]();
        console.log(`RootBeer.detectRootCloakingApps result=${result}`);
        return result;
    };

    
    RootBeer["checkForDangerousProps"].implementation = function () {
        console.log(`RootBeer.checkForDangerousProps is called`);
        let result = !this["checkForDangerousProps"]();
        console.log(`RootBeer.checkForDangerousProps result=${result}`);
        return result;
    };


    
    RootBeer["checkForRWPaths"].implementation = function () {
        console.log(`RootBeer.checkForRWPaths is called`);
        let result = !this["checkForRWPaths"]();
        console.log(`RootBeer.checkForRWPaths result=${result}`);
        return result;
    };

    
    
    RootBeer["checkForRootNative"].implementation = function () {
        console.log(`RootBeer.checkForRootNative is called`);
        let result = !this["checkForRootNative"]();
        console.log(`RootBeer.checkForRootNative result=${result}`);
        return result;
    };

    
    RootBeer["checkForMagiskBinary"].implementation = function () {
        console.log(`RootBeer.checkForMagiskBinary is called`);
        let result = !this["checkForMagiskBinary"]();
        console.log(`RootBeer.checkForMagiskBinary result=${result}`);
        return result;
    };

    let Utils = Java.use("com.scottyab.rootbeer.util.Utils");
    Utils["isSelinuxFlagInEnabled"].implementation = function () {
        console.log(`Utils.isSelinuxFlagInEnabled is called`);
        let result = !this["isSelinuxFlagInEnabled"]();
        console.log(`Utils.isSelinuxFlagInEnabled result=${result}`);
        return result;
    };

});

```

### 함수 후킹 수행 및 결과 (Desktop cmd)

![image](https://github.com/user-attachments/assets/a2ff60a4-aa02-4c5a-b26c-301f1c113224)


![image](https://github.com/user-attachments/assets/95051f88-d298-44b3-bdaf-f2deda0603ca)

![image](https://github.com/user-attachments/assets/204b14ce-459b-4bae-a8f7-7f9a51298903)


![image](https://github.com/user-attachments/assets/7d21c98b-a351-46eb-ac96-646b120e7677)

![image](https://github.com/user-attachments/assets/56f08227-906c-4ce5-8c31-270955680bd0)

