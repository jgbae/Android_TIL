# Generic System Image

GSI란 Android OpenSource Project 코드를 사용하는 순수 Android를 의미함.

Android 9부터 실제 기기에서도 GSI를 사용 가능하다.

GSI 지원 여부는 아래 명령어를 통해 확인 가능하다.

1. Treble 지원 확인

```shell
adb shell getprop ro.treble.enabled
```

2. 버전 간 지원 확인

```shell
adb shell cat /system/etc/ld.config.version_identifier.txt \
| grep -A 20 "\[vendor\]"
```

true인 경우 모든 GSI OS 사용 가능.

false인 경우 현재 OS와 동일한 버전의 GSI 이미지만 로드 가능.

3. GSI CPU Architecture 확인

```shell
adb shell getprop ro.product.cpu.abi
```