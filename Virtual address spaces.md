#Virtual address spaces

[원본 링크](https://docs.microsoft.com/ko-kr/windows-hardware/drivers/gettingstarted/virtual-address-spaces)


```
프로세서가 메모리에 read나 write를 할 때, 가상 주소를 사용한다.

read와 write작업의 한 부분으로써, 프로세서는 가상 주소를 물리 주소로 해석한다.

가상 주소로 메모리에 접근하는 데에서 생기는 이점은 다음과 같다:
```
<ul>
	프로그램은 가상 주소의 사용을 통해 물리적으로는 연속적이지 않고, 크기가 큰 버퍼에 연속적으로 접근할 수 있게 된다.(만약 디버깅 할때 메모리 주소가 연속적이지 않다면 상당히 불편할 것이다.)

	프로그램은 사용 가능한 물리적 메모리보다 큰 크기의 가상 메모리를 할당받고, 사용할 수 있게 된다. 사용 가능한 물리적 메모리의 공급이 적어지면 메모리 매니저(Memory manager)는 물리적 메모리의 페이지들(일반적으로 4KB)을 디스크 파일에 저장한다. 데이터나 코드는 요구에 따라 페이지 단위로 물리 메모리와 디스크 사이를 이동한다.

	다른 프로세스들에 의해 사용되는 가상 메모리들은 서로에 대해 독립되어 있다. 한 프로세스의 모드는 다른 프로세스나 OS에 의해 사용되는 물리 메모리를 바꿀 수 없다.
</ul>

```
프로세스가 사용 가능한 가상 주소를 그 프로세스의 *가상 주소 공간(virtual address space)*라고 한다. 각각의 유저모드 프로세스는 그 프로세스만의 가상 주소 공간을 가지고 있다. 32비트 프로세스의 경우, 가상 주소 공간은 보통 2GB이고 범위는 0x00000000 ~ 0x7FFFFFFF이다. 64비트 프로세스의 경우, 가상 주소 공간은 8TB이고 범위는 0x000'00000000 ~ 0x7FF'FFFFFFFF이다. 가상 주소들은 때론 *가상 메모리*라고 불린다.
```

```
아래의 그림은 가상 주소 공간의 중요한 특징들을 표현한 것이다.
```

[Key features of virtual address spaces.](https://docs.microsoft.com/ko-kr/windows-hardware/drivers/gettingstarted/images/virtualaddressspace01.png)

```
이 도표는 두개의 64비트 프로세스(Notepad.exe, MyApp.exe)의 가상 주소 공간을 보여준다. 각각의 프로세스는 0x000'0000000 ~ 0x7FF'FFFFFFFF 의 범위를 가지는 가상 주소 공간을 가지고 있다. 각각의 어두운 블록은 한 페이지(4KB)의 가상 혹은 물리 메모리를 표현한 것이다. Notepad 프로세스는 0x7F7'93950000부터 시작하는 세 개의 연속된 페이지를 가지고 있다. 하지만 이 세 개의 연속된 가상 주소들의 페이지는 연속적이지 않은 물리 메모리의 페이지에 map되어있다. 또한 두 프로세스 모드 0x7F7'93950000에서 시작하는 가상 메모리의 페이지를 사용하지만, 이 가상 페이지들은 서로 다른 물리 메모리 페이지에 map되어 있음을 유념해야 한다.
```

##유저 공간과 시스템 공간(User space and system space)

```
Notepad.exe나 MyApp.exe와 같은 프로세스들은 유저모드에서 동작한다. OS의 핵심 요소들과 많은 드라이버들은 좀 더 권한이 높은 커널모드에서 동작한다. 프로세서의 모드들에 관련한 더 많은 정보를 원한다면 아래의 링크에서 볼 수 있다. 각각의 유저모드에서 동작하는 프로세스는 자신만의 가상 메모리 공간을 가지고 있지만, 커널 모드에서 동작하는 모든 코드들은 *시스템 공간(System space)*라는 하나의 가상 주소 공간을 공유한다. 유저모드의 프로세스들이 사용하는 가상 주소 공간은 *유저 공간(User space)*라고 부른다.
```

[User mode and kernel mode](https://docs.microsoft.com/ko-kr/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)

```
32비트 Windows에서, 총 사용 가능한 가상 주소 공간은 2^32Byte(4GB)이다. 보통 하위 2GB는 유저 공간을 위해 사용되고, 상위 2GB는 시스템 공간을 위해 사용된다. 
```
[32bit pages description](https://docs.microsoft.com/ko-kr/windows-hardware/drivers/gettingstarted/images/virtualaddressspace02.png)

```
32비트 Windows에서,(부팅 할 때) 2GB 이상의 유저 공간을 할당할 수 있도록 하는 옵션이 있다. 이는 결과적으로 사용 가능한 시스템 공간이 적어짐을 의미한다. 유저 공간의 크기는 최대 3GB까지 늘릴 수 있고, 이 경우에 사용 가능한 시스템 공간은 1GB가 된다. 유저 공간의 크기를 늘리고 싶다면 아래의 링크를 확인하라.
```

[BCDEdit/set increaseuserva](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set)

```
64비트 Windows에서, 이론적인 가상 주소 공간은 2^64Byte(16EB)이지만, 16EB의 작은 부분만이 실제로 사용된다. 0x000'00000000 ~ 0x7FF'FFFFFFFF의 범위를 가지는 8TB의 공간이 유저 공간을 위해 사용되고, 0xFFFF0800'00000000 ~ 0xFFFFFFFF'FFFFFFFF의 범위를 가지는 248TB의 공간이 시스템 공간을 위해 사용된다.
```

[64bit pages description](https://docs.microsoft.com/ko-kr/windows-hardware/drivers/gettingstarted/images/virtualaddressspace03.png)

```
유저모드에서 동작하는 코드는 유저 공간에 대한 접근 권한은 있지만, 시스템 공간에는 접근할 수 없다. 이러한 제한은 유저모드의 코드들이 OS의 보호받은 데이터 구조체를 읽거나 바꿀 수 없도록 막는다. 커널 모드에서 동작하는 코드의 경우 유저 공간과 시스템 공간 모두에 접근 권한이 있다. 이 말은, 커널 모드에서 동작하는 코드의 경우 시스템 공간과 현재의 유저모드 프로세스들의 가상 주소 공간에 대한 접근 권한이 있다는 말이다.
```

```
커널 모드에서 동작하는 드라이버는 직접적으로 유저 공간을 읽거나 쓰는 것에 대해 매우 신중해야 한다. 아래의 시나리오는 그 이유에 대해서 설명한다.
```

<ol>
	한 유저모드 프로그램이 기기의 데이터를 읽도록 요청한다. 프로그램은 해당 데이터를 읽기 위해 버퍼의 시작 주소(유저모드 프로그램의)를 제공한다.

	커널 모드에서 동작하고 있는 한 기기 드라이버 루틴이 읽기 작업을 시작하고 caller에게 제어권을 넘긴다.

	나중에 기기는 현재 동작중인 어떤 쓰레드에게 읽기 작업이 완료되었음을 알리기 위해 그 쓰레드를 중단(interrupt)시킨다. 이 중단은 무작위의 쓰레드에서 동작하고 무작위의 프로세스에 속해 있는 어떤 커널모드 드라이버 루틴에 의해 통제되고 있다.

	이 때, 이 드라이버는 1번에서 유저모드 프로그램이 제공한 버퍼의 시작주소에 데이터를 쓰면 안된다. 이 주소는 요청을 시작한 프로세스의 가상 주소 공간이지만, 높은 확률로 현재 프로세스의 것과는 다를 것이다.
</ol>

##Paged pool and Nonpaged pool

```
유저 공간에서, 모든 물리 메모리 페이지들은 필요에 따라 디스크 파일에 page될 수 있다. 시스템 공간에서, 몇몇 물리적 페이지들은 page될 수 있지만 아닌 것들도 있다. 시스템 공간은 메모리 동적 할당을 위해 두 가지의 지역(페이지된 풀과 그렇지 않은 풀)으로 나뉜다. 
```

```
페이지된 풀(paged pool)에 할당된 메모리는 필요에 따라 디스크 파일에 page될 수 있다.
페이지되지 않은 풀(nonpaged pool)에 할당된 메모리 들은 절대로 디스크 파일에 page될 수 없다.
```

[paged pool and nonpaged pool](https://docs.microsoft.com/ko-kr/windows-hardware/drivers/gettingstarted/images/virtualaddressspace04.png)