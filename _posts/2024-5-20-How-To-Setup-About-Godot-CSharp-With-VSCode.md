---
layout: post
title: Godot 4.x에서 C#과 VSCode를 사용하는 방법
date: 2024-05-20 05:27:00 +0900
author: Jeongjin Oh
category: Study
tags: Godot Csharp VSCode
---

지난 1월에 설정을 실컷 해놓고 잠깐 여유있는 틈을 타서 다시 하려니까 하나도 기억이 안나서 기억 저장 겸 포스팅을 하고자 한다. 근황은 추후 각 잡고 적어보겠다.

기준은 일단 Apple Silicon으로 했는데, 윈도우도 크게 다르지 않을 것이다.

---

## 1. Godot_mono 버전과 Dotnet SDK, Visual Studio Code를 설치한다.

현재 Godot의 최신 버전은 4.2.2이다. Dotnet은 8.0이 최신이고. VSCode의 경우 플랫폼에 맞게 최신 버전을 설치하는 것이 좋겠다.

## 2. Godot_mono를 실행하여 프로젝트를 생성 후, 에디터 설정에서 VSCode를 편집 에디터로 지정해준다.

![](/images/2024-5-20-How-To-Setup-About-Godot-CSharp-With-VSCode/2024-05-20-05-09-00.png)

![](/images/2024-5-20-How-To-Setup-About-Godot-CSharp-With-VSCode/2024-05-20-05-09-25.png)

## 3. VSCode에서 `C#`, `C# Dev Kit`, `C# Tools for Godot`, `godot-tools` 등을 설치한다.

여기에서 `C# Tools for Godot`의 경우 현재 Godot 4.x에 대한 지원이 제대로 되지 않는 것 같은데, 추후 지원이 될 것 같기도 하고...

## 4. 프로젝트에서 C# 스크립트 하나를 추가해서 열어준다.

VSCode가 열려야 한다. 열리고나면...

## 5. VSCode의 Debug 탭에 가서 `create a launch.json file`을 눌러준다.

![](/images/2024-5-20-How-To-Setup-About-Godot-CSharp-With-VSCode/2024-05-20-05-16-14.png)

launch.json과 task.json이 생성되었다면 아래의 구문을 복사해서 붙여넣어준다.

```json
// launch.json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        // For these launch configurations to work, you need to setup a GODOT4
        // environment variable. On mac or linux, this can be done by adding
        // the following to your .zshrc, .bashrc, or .bash_profile file:
        // export GODOT4="/Applications/Godot.app/Contents/MacOS/Godot"
        {
            "name": "Play",
            "type": "coreclr",
            "request": "launch",
            "preLaunchTask": "build",
            "program": "${env:GODOT4}",
            "args": [],
            "cwd": "${workspaceFolder}",
            "stopAtEntry": false,
        }
    ]
}
```

```json
// task.json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "command": "dotnet",
            "type": "process",
            "args": [
                "build"
            ],
            "problemMatcher": "$msCompile",
            "presentation": {
                "echo": true,
                "reveal": "silent",
                "focus": false,
                "panel": "shared",
                "showReuseMessage": true,
                "clear": false
            }
        }
    ]
}
```

## 6. 터미널을 실행시켜서 bashrc 또는 zshrc에 다음을 추가해준다.

```export GODOT4="/Applications/Godot_mono.app/Contents/MacOS/Godot"```

적절히 본인이 설치한 경로를 넣어준다.

> 윈도우의 경우 환경 변수에 설정하면 될 것이다.

## 7. VSCode를 재실행 후, 테스트 코드를 적절히 넣어서 Debug 탭에서 `Play`를 선택하고 디버그를 실행해본다.

![](/images/2024-5-20-How-To-Setup-About-Godot-CSharp-With-VSCode/2024-05-20-05-22-22.png)

실행하면 별도의 게임 인스턴스가 실행되며, 브레이크 포인트에 잘 걸리면 끝!

> 출처1: https://docs.godot.community/tutorials/scripting/c_sharp/c_sharp_basics.html
>
> 출처2: https://github.com/godotengine/godot-csharp-vscode/issues/43#issuecomment-1258321229

---

역시 뭐든 기록을 남겨야 좋은듯. 회사 프로젝트의 프리오픈이 끝나서 월, 화 이틀 휴가를 받게 되어서 이시간까지 놀고먹고 하는 중 (...)
