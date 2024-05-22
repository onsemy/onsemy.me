---
layout: post
title: Godot 4.x에서 Cysharp R3를 설치하는 방법
date: 2024-05-23 03:00:00 +0900
author: Jeongjin Oh
category: Study
tags: Godot Csharp Cysharp R3
---

> 참고: Godot 4.2.2에서 R3가 정상적으로 동작하지 않을 수 있다.
> - https://github.com/godotengine/godot/issues/78513
> - https://github.com/Cysharp/R3/issues/125

오늘은 분명 출근해야 하는데 이 시간까지 잠을 못자고 있다. 이유는 1시쯤 잠들었다가 갑자기 너무 배고파서 깨는 바람에 라면 하나 부셔먹고 소화가 어느 정도 될 때까지 기다리고 있기 때문이다 (...) 어쨌든, 깨있는 김에 지난 번 지나가다가 본 Cysharp의 R3을 Godot에서 사용할 수 있다는 것을 보고 사용법에 대해 정리해보고자 한다.

R3이라면 잘 모르는 사람도 있을 수 있으나, UniRx는 들어봤을지도 모른다. 그 UniRx를 좀 더 여러 플랫폼에서 사용 가능하도록 Cysharp에서 만들고 있는 프레임워크다. UniRx에서 제공하던 기능이 온전히 있어서 유니티에서 Godot으로 온 개발자에게 있어 단비 같은 존재가 아닐까 싶다.

---

## 1. R3를 설치한다.

터미널을 열어서 작업 중인 Godot 프로젝트 폴더까지 이동한다. 이후 아래의 명령어를 실행한다.

```$ dotnet add packet R3 --version 1.1.11```

버전의 경우 특정 버전을 넣으려면 저렇게 쓴다.

## 2. Cysharp/R3 깃허브 저장소에 방문하여 Clone 받는다.

[여기](https://github.com/Cysharp/R3)로 가면 된다.

## 3. 작업 중인 Godot 프로젝트 폴더 내에 `addons` 라는 폴더를 추가한다.

![](/images/2024-5-23-How-To-Use-R3-In-Godot-4/2024-05-23-02-39-05.png)

## 4. `addons` 폴더 안에 `R3.Godot` 폴더를 복사한다.

![](/images/2024-5-23-How-To-Use-R3-In-Godot-4/2024-05-23-02-43-04.png)

R3.Godot 폴더는 클론 받은 저장소 내에 `src/R3.Godot/addons/R3.Godot`에 있다.

## 5. `GodotR3Plugin.cs`를 열어서 `_ExitTree()` 함수 내에 있는 내용을 복사하여 `_EnterTree()` 함수 내 상단에 붙여넣는다.

```csharp
public override void _EnterTree()
{
    // Remove before adding to avoid problems
    if (observableTrackerDebugger != null)
    {
        RemoveDebuggerPlugin(observableTrackerDebugger);
        observableTrackerDebugger = null;
    }

    RemoveAutoloadSingleton(nameof(FrameProviderDispatcher));
    RemoveAutoloadSingleton(nameof(ObservableTrackerRuntimeHook));

    observableTrackerDebugger ??= new ObservableTrackerDebuggerPlugin();
    AddDebuggerPlugin(observableTrackerDebugger);

    // Automatically install autoloads here for ease of use.
    AddAutoloadSingleton(nameof(FrameProviderDispatcher), "res://addons/R3.Godot/FrameProviderDispatcher.cs");
    AddAutoloadSingleton(nameof(ObservableTrackerRuntimeHook), "res://addons/R3.Godot/ObservableTrackerRuntimeHook.cs");
}
```

## 6. 작업 중인 프로젝트 루트에 있는 `.csproj` 파일을 열어서 `<LangVersion>` 태그를 `<PropertyGroup>` 내에 추가한다.

```xml
<PropertyGroup>
    <LangVersion>12.0</LangVersion>
</PropertyGroup>
```

## 7. Godot 에디터로 돌아가서 빌드를 한 번 해준 후, 프로젝트 설정에서 플러그인 탭으로 이동하여 활성화를 눌러준다.

![](/images/2024-5-23-How-To-Use-R3-In-Godot-4/2024-05-23-02-47-54.png)

---

역시 뭐든 기록을 남겨야 좋은듯22. 토이 프로젝트를 진행 중인데, 쓰면 쓸수록 뭔가 장난감을 가지고 노는 기분이라 재미있다.
