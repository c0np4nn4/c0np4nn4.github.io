+++
title = "우분투 23.04에 카카오톡 설치"
date = "2023-12-02"
+++

# 1. wine 설치
[Ref1](https://wine.htmlvalidator.com/install-wine-on-ubuntu-23.04.html)
[Ref2](https://linuxize.com/post/how-to-install-wine-on-ubuntu-20-04/#installing-wine-50-on-ubuntu)
- `Ref1`만 봐도 설치하는 데 무리는 없음
- 아래 명령어로 설정 창을 열 때, Mono Installer 꼭 다운받아야함
```bash
wine winecfg
```

# 2. KaKaotalk 설치
```bash
wine ~/Downloads/KakaoTalk_Setup.exe
```

# 3. Font 설정
적당한 한글 ttf 파일을 가져와서

1. Install
2. mv /path/to/<name>.ttf ~/.wine/drive_c/windows/Fonts/<name>.ttf 
3. sudo vi ~/.wine/system.reg 한 다음 `/MS Shell` 해서 나오는 곳의 `Tahoma` 대신 방금 설치한 폰트 이름 넣기
4. 카카오톡 설정 창으로 가서 Display > Font 설정 (네모로 깨진게 엄청 많으니 잘 찾아야 함)
