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


