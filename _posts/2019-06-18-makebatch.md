---
layout:     post
title:      make simple batch file
date:       2019-06-18 20:21:29
summary: make simple batch file for windows 10
categories: windows batch shell script 
---

# 윈도우에서 배치파일 만들기

처음 배치파일을 만들어야 겠다고 생각했던 계기는 회사에서 사용하는 프로그램이 특정 위치에 가서 관리자 권한으로 실행하지 않으면 실행이 되지 않았다. 그래서 배치파일을 이용해서 해당 디렉토리로 이동과 실행을 한번에 할려고 했다.
<img src="https://user-images.githubusercontent.com/16702158/59677806-b3c47800-9205-11e9-83a5-cb1ecafa2e1f.png" width="40%">

## 배치 파일이란

- MS-DOS, 윈도우에서 쓰이는 배치 파일은 명령 인터프리터에 의해 실행되게끔 고안된 명령어들이 
나열되어 있는 텍스트 파일이고 배치 파일이 실행될 때, COMMAND.COM 또는 cmd.exe와 
같은 셸 프로그램이 파일을 읽어 명령어를 줄 단위로 실행

1. 메모장에 사용할 명령어를 메모장에 작성
![메모장 작성](https://user-images.githubusercontent.com/16702158/59677861-e3738000-9205-11e9-8f9c-b9eb8f121d6f.png)
2. 저장할때 파일형식을 모든 파일로 하고 파일명 뒤에 .bat로 작성
![다른이름으로저장](https://user-images.githubusercontent.com/16702158/59677865-e9696100-9205-11e9-914c-fd56147a44de.PNG)

### ※ 관리자 권한으로 배치파일 실행
배치파일을 만들고 관리자 권한으로 실행해야할 때가 있으면 이렇게 작성하시면 된다.

```
@echo off

>nul 2>&1 "%SYSTEMROOT%\system32\cacls.exe" "%SYSTEMROOT%\system32\config\system"

if '%errorlevel%' NEQ '0' (

    echo 관리 권한을 요청 ...

    goto UACPrompt

) else ( goto gotAdmin )

:UACPrompt

    echo Set UAC = CreateObject^("Shell.Application"^) > "%temp%\getadmin.vbs"

    set params = %*:"=""

    echo UAC.ShellExecute "cmd.exe", "/c %~s0 %params%", "", "runas", 1 >> "%temp%\getadmin.vbs"



    "%temp%\getadmin.vbs"

    rem del "%temp%\getadmin.vbs"

    exit /B



:gotAdmin

pushd "%CD%"

    CD /D "%~dp0"
```

 위의 코드가 관리자 권한이 없으면 요청해서 실행을 한다라는 뜻입니다. 그리고 바로 밑에 코드를 밑에 작성하면 됩니다.
 그리고 실행을 해보면 이렇게 뜨고 확인을 누르시면 관리자권한으로 실행을 합니다.
 ![관리자권한요청](https://user-images.githubusercontent.com/16702158/59677872-ef5f4200-9205-11e9-8c28-70b1b0c2b6e5.PNG)
 끝~
 