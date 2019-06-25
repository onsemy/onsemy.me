---
layout: post
title: Cocos2d-x 구버전을 다시 개발하기 (feat. 디기디기)
date: 2019-06-25 21:27:00
author: Jeongjin Oh
categories: Project
tags: cocos2dx DigiDigi
---

토요일에 [게임 포켓 활동](https://github.com/Game-Pocket)을 하면서 멤버 중에서 나에게 자신의 회사에 이력서를 넣어보는게 어떠냐고 하여 요구하는 스펙을 보니 *Cocos2d-x*를 쓰는 회사였다. Cocos2d-x라... 처음 접했던 건 2012년이었고, 마지막으로 사용했던 건 2014년이었으니 쓰지 않은 지 5년이 넘었다 (...)

이전에 했던 프로젝트 중에서 가장 규모가 큰(?) 2개의 프로젝트가 있었는데, 하나는 2012년에 공모전 출전을 위해 만들었던 *DigiDigi*, 다른 하나는 2013년에 멤버십 활동 시절에 1년 동안 진행했던 *Parasite*이다. 둘다 *Cocos2d-x 2.x*를 기반으로 제작했다. *DigiDigi*의 경우엔 나름 Google Play Store에도 출시했다. 광고도 일절 없는 무료버전이었긴 하지만. 게임 볼륨면에서는 *Parasite*가 훨씬 컸지만, 프로젝트를 마무리 짓고 출시까지 했던 프로젝트는 회사 이외에서는 *DigiDigi*가 유일하다. 개인적으로는 특별했던 프로젝트 중 하나다.

그런 계기로 이전에 작업했던 Cocos2d-x 프로젝트들을 부활 시켜보려고 했다. 과거의 내가 얼마나 잘 관리를 했을지 궁금하기도 했다. 마치 타임캡슐을 열어보는 기분이랄까? 그런데 내가 이 소스코드들을 어디에다 저장해뒀는지 전혀 기억이 나지 않았는데, 다행히도 과거의 내가 내 서버에 SVN으로 관리하도록 해두었다. SVN으로 관리하던 것을 git(github)로 옮기고, 현재 환경에서 구동 가능하도록 여러 가지 시도해본 것을 여기에 적어볼까 한다.

---

일단 *DigiDigi*의 경우 Cocos2d-x 2.2.2 버전으로 제작했었다. 현재 공식 홈페이지에서 2.2.6을 받을 수 있었다. 아직 구 버전을 받을 수 있는 것은 다행이었지만 지금은 2019년이다. Visual Studio 2019가 나오는 마당에 아무런 설정을 하지 않고 잘 돌아갈리 만무하다. 혹시나 해서 `cocos2d-win32.vc2012.sln`를 열어서 빌드를 했으나 되지 않는다.

Visual Studio 2012 버전(v110)이 아니면 빌드가 되지 않는 것 같다. 그래서 *Visual Studio Express 2012*를 설치했다. Visual Studio 버전을 v110으로 리타게팅하고 Start Project를 TestCpp로 지정하고 빌드했다.

![된다!](/images/2019-6-25-Revive-DigiDigi-With-Cocos2d-x/2.png)

된다! 이제 *DigiDigi*를 솔루션 파일에 추가하여 실행해보는 일만 남았다. 빌드 타겟을 똑같이 맞추고 실행해보니 컴파일은 되지만 실행 시 오류가 발생한다. Resources 폴더 내의 에셋을 찾지 못하고 있다. 디버깅을 해보니 경로가 엉뚱한 곳으로 지정이 되어 있었다. 다른 프로젝트는 Resources 폴더를 잘 찾아서 가져오는데... Text로 검색이 가능한 모든 파일에 대해서 "Resources"를 쓰는 곳이 있는지 검색해보니 Visual Studio의 user 파일에서 따로 지정을 했던 것으로 드러났다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">
    <LocalDebuggerWorkingDirectory>$(ProjectDir)..\Resources</LocalDebuggerWorkingDirectory>
    <DebuggerFlavor>WindowsLocalDebugger</DebuggerFlavor>
  </PropertyGroup>
  <PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">
    <LocalDebuggerWorkingDirectory>$(ProjectDir)..\Resources</LocalDebuggerWorkingDirectory>
    <DebuggerFlavor>WindowsLocalDebugger</DebuggerFlavor>
  </PropertyGroup>
</Project>
```

위와 같이 `..\Resources`라고 지정해주어야 한다. 위와 같이 변경 후 VS를 재시작하여 다시 실행하면,

![짜잔](/images/2019-6-25-Revive-DigiDigi-With-Cocos2d-x/1.jpg)

잘 실행 된다. 정말 오랜만에 보는 것 같다. 좀 더 정비해서 다시 스토어에 올릴 수 있도록 해야겠다.

---

*Parasite*도 나중에 같은 방법으로 부활(?)을 시도해보아야겠다. 그런데 *Parasite*의 경우 Cocos2d-x 3.x로 개발했던 기억이 나는데 왜 README에는 2.2.2로 되어있었을까...? 잘 모르겠다 ㅎㅎ;

딴짓도 열심히 끝냈으니, 다시 이력서를 다듬어보아야겠다 (...)
