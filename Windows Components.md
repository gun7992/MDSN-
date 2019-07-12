Windows Components

[링크](https://docs.microsoft.com/ko-kr/windows-hardware/drivers/kernel/overview-of-windows-components)

![picture](https://docs.microsoft.com/ko-kr/windows-hardware/drivers/kernel/images/ntarch.png)

```
사진에서 볼 수 있듯이, 윈도우라는 운영 체제(OS)는 유저모드(user-mode)와 커널모드(kernel-mode) 요소들을 포함하고 있다.
```

```
드라이버는 다양한 커널 요소들이 제공하는 루틴을 호출한다.

예를 들어, Device object를 생성하려면, I/O 매니저에 의해 제공되는 IoCreateDevice 루틴을 호출해야 한다.

드라이버가 호출할 수 있는 커널모드 루틴의 목록은 아래 링크에 있다.

```

[Driver Support Routine](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/index)

```
또한 드라이버는 OS의 특정한 호출에 의무적으로 응답해야 하고, 다른 시스템 호출에 대해서도 응답이 가능하다.

드라이버가 지원할 필요가 있는 커널모드 루틴의 목록은 아래 링크에 있다.
```

[Standard Driver Rountines](https://docs.microsoft.com/windows-hardware/drivers/kernel/introduction-to-standard-driver-routines)

```
위의 그림에 커널모드의 모든 요소들이 있는건 아니다.

더 많은 커널모드 요소의 목록은 아래 링크에 있다.
```

[Kernel-mode Managers and Libraries](https://docs.microsoft.com/ko-kr/windows-hardware/drivers/kernel/kernel-mode-managers-and-libraries)

