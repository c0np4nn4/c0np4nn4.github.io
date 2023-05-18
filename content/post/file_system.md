+++
title = "file_system"
description = ""
date = 2023-05-18
toc = true

[taxonomies]
categories = [""]
tags = ["OS", "", ""]

[extra]
math=true
+++

# Persistent Storage
> - `Hard Disk Drive`
> - `Solid State Drive`

- 디스크를 관리하기 위해 추상화된 단계가 필요함
  - ***File System***

---

# File System Layer

```
[Generic Block Layer] 를 통해서 접근함

[Block Interface]

```

- <txtred>단계별</txtred>로 추상화가 존재

- 파일시스템 단계에서는 file 과 directory 로 되어 있는 `Tree` 로 내부 구조를 파악하고 다룸

`Generic Block Layer` 는 <txtylw>중간다리</txtylw> 역할임

---

# Storage: Logical View

`Block Interface Abstraction`
- 블럭이 일렬로 나열된 형태의 추상화
```
+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
| A | B | . | . |   |   |   |   |   |   |   |   |   | Z |
+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
```

- Operations
  - <txtylw>Identify</txtylw>(): returns N (블록의 개수)
  - <txtylw>Read</txtylw>(start Sector #, # of sectors)
  - <txtylw>Write</txtylw>(start Sector #, # of sectors)

---

# Abstraction of Storage
- `File`
  - `이름`을 가짐 (Human Readable)
    - 컴퓨터 입장에서는 굳이 필요 없는 정보
  - ***inode number*** 를 가짐
    - <txtred>File System</txtred>이 파일을 관리하기 위해서
    - ***UNIQUE***
- `Directory`
  - 체계적으로 관리하기 위해 존재함
  - `File System` 내에서의 `File` 의 한 일종임
    - `Directory` 내부의 `File`의 <txtylw>List</txtylw> 를 가지고 있음
    - 자연스레 `Tree` 구조를 형성함

---

# Directory Tree

그냥 트리구조

---

# File System Basics
- 각 `File` 은 아래의 정보들을 가짐
  - `File contents (data)`
    - `A sequence of bytes`
    - `File System` 입장에서는 별로 안중요함
  - `File attributes (metadata or inode)`
    - `File Size`
    - `Owner`
    - `Access Control Lists`
    - `Timestamps`
    - `Block locations` (디스크 내에서의 위치 정보)
- `파일 이름`
  - `File System` 입장에서는 ***Full Pathname*** 이 `이름`임


> - `파일 이름` 을 가지고, `inode number`를 찾음
>   - `inode number` 는 `Directory` 에 있음
> - `inode number`를 가지고, `metadata(inode)` 를 찾음
> - `inode`를 가지고, `File Data(들이 들어있는 블록 위치)` 를 찾음

## Goals
- `Performance`, `Reliability`, `Scalability`
### Tasks
- `Metadata` 로 `Data Block` 위치까지 어떻게 찾을까
- `Metadata` 와 `Data Blocks` 를 어떻게 잘 관리할까
  - 할당, 해제, 여유 메모리 공간 관리 등
- `File System` 에서 <txtred>Crash/txtred> 가 일어났을 때 어떻게 복구할 것인지

---

# File Attributes
- `POSIX Inode`
  - `inode number`
  - `permission` : rwx, rws
  - `사용자 정보`
  - 크기
  - 할당된 블록 개수
  - `file` 에 변화가 생겼을 때의 시점에 대한 정보

---

# File Operations
여러 함수들이 있음

---

# Pathname Translation
- 아래 함수를 예시로 듦
```c
open("/a/b/c", ...)
```

- `Root Directory` 인 `/` 를 찾음 (TRIVIAL)
- `/` 아래의 `a` 에 대해 검색함
  - `a` 에 대한 `inode` 정보를 획득
  - `a` 에서 `b` 의 `inode` 정보를 획득, 즉 `b` 의 내용을 확인
  - `b` 에서 `c` 의 `inode` 정보를 획득, 즉 `c` 의 내용을 확인
  - `c` 의 `permission` 에 대한 검사
- `directory path` 를 내려가며 검사를 진행하는 것의 <txtred>오버헤드</txtred>가 큼
  - 자주 사용되는 `Directory` 의 정보는 ***caching*** 해 둔다고 함

---

# Ensuring Persistence
- 유저의 요청이 있을 때마다 `Disk` 에 데이터를 저장하는 것보다는 `memory` 상의 `BUFFER` 에 잠시 기록했다가 <txtred>한꺼번에</txtred> 저장하는 것이 효율적임
- 주기적으로 `memory` 에 기록된 정보들을 디스크에 저장함
- `fsync()`: metadata 까지도 전부 sync
- `fdatasync()`: metadata 빼고 전부 sync

---

# Hard, Symbolic Link
- 둘 다 하나의 파일을 가리키는 또 다른 형태의 파일..
- `Symbolic`: <txtylw>바로가기</txtylw>, 포인터만 있고 <txtred>데이터는 없음</txtred>
- `Hard`: 동일한 <txtred>inode</txtred>를 가짐, <u>별명만 하나 더 생김</u>. 즉, ***original*** 을 구분할 수 없음, 따라서 이러한 파일이 ***몇 개***나 존재하는지 체크하기 위해 `POSIX INODE` 에서는 이를 tracking 함, `link count` 가 $0$이 되면 `inode` 를 삭제, 서로 다른 `file system` 은 서로 다른 `inode standard` 를 쓸 것이므로 `file system` 간에는 `Hard link` 생성이 불가능함

---

## Hard Links
```bash
ls -f old.txt new.txt
```
`old.txt` 와 `new.txt` 는 서로 구분이 불가능함

---

## Symbolic Links
```bash
ls -s old.txt new.txt
```
`new.txt` 는 `old.txt` 를 <txtred>***가리키기만 함***</txtreD>

<txtred>***Danglinig Reference***</txtred> 발생 가능함

---

# File System Mounting
- `File System` 은 `Disk` 에 저장되어 있었음
- `OS` 단에서 시스템에 이를 <txtylw>연결</txtylw> 해야 사용할 수 있음(available)
- `UNIX` 는 서로 다른 파일시스템이 하나의 `TREE`에 ***Mount*** 됨
  - `UNIX` 는 따라서, <txtylw>***Root Directory***</txtylw> 를 가짐

- 기존의 `Directory` 를 <txtylw>Mount Point</txtylw> 로 둠

---

# Consistency Semantics
- 여러가지 `Semantics` 이 존재함
- TRIVIAL
  - shared 상태에서는 immutable 이라든지...

---

