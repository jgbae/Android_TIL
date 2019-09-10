# Android Security Overview

OS 레벨에서 안드로이드 플랫폼은 리눅스 커널의 보안을 제공받음.

OS수준의 보안 기능은 App 샌드 박스에 의해 네이티브 코드조차도 제한된다. (무슨말?)

안드로이드 시스템 자체는 App이 다른 App이나 시스템 자체를 손상시킬 수 없도록 설계되었음. 

필요한 설정은 Compatibility Definition Document에 정의되어 있음.

## 1. Kernel Security

### Linux Security

안드로이드 플랫폼의 기반은 리눅스 커널이다.

리눅스 커널이 제공하는 핵심 보안 기능

- 유저 기반 권한 모델(?)

- 프로세스 고립

- Secure IPC를 위한 확장 가능한 매커니즘

- 커널의 잠재적으로 안전하지 않은 부분 제거

멀티 유저 OS로써 리눅스 커널은 사용자 리소스를 고립시킨다.

A, B 유저간 CPU 자원 등 간섭이 일어나지 않도록 한다.

### The Application Sandbox

안드로이드의 모든 애플리케이션은 샌드박스가 적용된다.

샌드박스는 앱을 서로 분리하고 앱과 시스템을 악성 앱으로부터 보호함.

### System Partition and Safe Mode

시스템 파티션에는 안드로이드 커널도 포함되어 있음.

이 파티션들은 읽기 전용으로 설정되어 있다.

사용자가 Safe Mode로 부팅 시 서드파티 App은 수동으로 실행 가능하다.(?)


### Filesystem Permissions

UNIX 스타일 환경에서는 파일 시스템의 권한을 통해 서로 다른 사용자의 파일을 읽을 수 없도록 보장한다.

안드로이드에서는 각 앱 자체는 사용자가 된다.

개발자가 명시적으로 다른 응용프로그램과 파일을 공유하지 않으면 한 응용프로그램에서 만든 파일을 다른 응용프로그램에서 접근이 불가능하다.

### Security-Enhanced Linux

안드로이드는 SELinux를 통해 접근 제어 정책을 적용하고 프로세스에서 mac를 설정한다.

### Verified boot

