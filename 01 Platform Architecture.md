빌드시 참고 http://shincdevnote.blogspot.com/2018/11/aosp-build.html

# Platform Architecture

## 1. Background

안드로이드는 계층 구조로 이루어져 있다. 각 계층은 아래 계층이 안전하다는 가정 하에 구현된다.

일부 안드로이드 OS의 코드가 루트 권한으로 동작하지만 이 모든 코드는 Linux Kernel이 Application Sandbox를 통해 제한적으로 동작시킨다.

![image](https://developer.android.com/guide/platform/images/android-stack_2x.png?hl=ko)

## 2. Linux Kernel

안드로이드 플랫폼의 기반, 상위 계층의 ART는 스레딩, 하위 수준 메모리 관리 등에 Linux Kernel을 사용함.

Linux Kernel을 사용하기 때문에 제조사는 Linux용 H/W 드라이버 개발하면 되서 용이하다.

## 3. HAL

Java API Framework에 H/W 기능을 제공하는 표준 인터페이스 제공.

카메라, 블루투스 등 모듈과 같은 특정 H/W 구성 요소를 위한 인터페이스 구현. 

디바이스에 종속되지 않는 프로그래밍이 가능하다. H/W에 대한 공통 명령어 집합으로 이루어져 있으며 Java API Framework의 요청에 따라 해당 H/W 라이브러리 모듈 제공

## 4. ART

각 앱은 프로세스 내에서 자체 ART 인스턴스로 실행된다.

툴체인을 빌드하고 Java 소스를 DEX 바이트 코드로 컴파일 한다.

주요 기능

- AOT(Ahead-Of-Time), JIT(Just-In-Time) 컴파일

- Optimized Garbage Collection

- (Android 9+) DEX 형식 파일 더욱 간소화하여 번환

- 전용 샘플링 프로파일러, 상세 진단 예외 및 크래시 보고, 향상된 디버깅 지원

잠시 ART와 Dalvik VM을 비교해보자.

안드로이드 5 이전에서는 Dalvik이 ART 역할이었다.

오라클의 라이센스 문제와 안드로이드 구조에 최적화 하기 위해 Dalvik VM과 ART가 생겨남.

||Dalvik VM| ART VM|
|:-:|:--:|:--:|
|Arch|32bit Only|32, 64bit|
|Compiler|Just In Time|Ahead On Time|
|Compile Time|실행 시 마다|설치 시 컴파일코드 변환|
|장점|설치 파일 작음|배터리 향상, GC 향상, CPU, RAM 사용량 낮음|
|단점|배터리 소모 큼, CPU, RAM 사용량 높음|설치 파일 큼, 설치 시간 오래 걸림|

ART는 Kitkat버전부터 생겼음. Lolipop 이후는 AOT 컴파일러가 기본으로 적용.

JIT은 앱 최초 실행 시 자바 코드가 일정 부분 한꺼번에 변환, RAM에 올려놓고 작업함. 최초 도입 시 엄청난 성능 향상이 보였으나 배터리 소모, 컴파일 시 H/W부하가 문제가 되었음.

Nougat버전 부터 JIT과 AOT모두 탑재, 최초 설치 시 무조건 JIT으로 사용, 설치 시간과 용량을 적게 소모한 뒤 기기 미사용 시 컴파일을 조금씩 자주 하여 자주 사용되는 앱을 AOT 방식으로 전환.


## 5. Native C/C++ Library

ART, HAL 등 핵심 Android 시스템 구성 요소 및 서비스는 C/C++로 작성된 네이티브 라이브러리를 필요로 하는 네이티브 코드를 기반으로 빌드되었음.

코드의 보안 또는 성능을 필요로 하는 경우 Android NDK(Native Development Kit)을 이용해 네이티브 코드에서 직접 여러가지 네이티브 플랫폼 라이브러리 접근 가능.

## 6. Android Framework

JAVA API Framework를 말함.

Android OS 전체 기능은 JAVA로 작성된 API를 통해서 접근 가능.

- View System

목록, 그리드, 버튼 등 다양한 기능 제공, 확장 가능

제공되는 기능을 이용해 App의 UI 빌드에 사용

- Resource Manager

그래픽 및 레이아웃 파일과 같이 코드가 아닌 리소스에 대한 접근 제공

- Notification Manager

모든 App이 상단바에 사용자 지정 알림을 표시할 수 있도록 지원

- Activity Manager

App의 수명주기를 관리, 공통 탐색 백 스택 제공(여러개 액티비티 활성 시 Back 버튼으로 액티비티 전환)

## 7. System Application

어...그냥 시스템 앱은 일반 앱과 그닥 차이 없고 특정 기능은 시스템 앱 호출로 커버 가능하다..?