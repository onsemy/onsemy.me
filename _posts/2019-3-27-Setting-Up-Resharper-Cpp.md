---
layout: post
title: Resharper C++ 설정
date: 2019-03-27 13:42:00
author: Jeongjin Oh
categories: Study
tags: resharper cpp visualstudio
---

이전에 [Visual Assist X와 Resharper C++을 비교하면서 깠던 적](/study/Visual-Assist-X-vs-Resharper-Cpp-in-UE4)이 있는데, 그럼에도 불구하고 다시 한 번 도전해보기로 했다.

Code Assistance의 경우 재설치를 하니까 잘 되었는데, 문제는 다른쪽에서 났다. 이전에 실행했을 땐 타이핑과 동시에 이거다 싶은 애를 선택해서 Assist를 해줬는데 재설치 이후에는 그게 동작을 하지 않는 것이었다. 왜일까 봤더니 아래의 설정이 바뀌어 있었다.

![](/images/2019-3-27-Setting-Up-Resharper-Cpp/1.png)

아니면 애초에 이렇게 되어 있었는데 모종의 이유(?)로 내가 의도한 대로 잘 되고 있었을지도 모른다. 위의 이미지는 내가 바꿔놓은 것이고 열었을 당시에는 `Display but do not preselect`가 선택되어 있었다. 바로 선택되길 바라는 항목에 `Display and preselect`를 선택하고 저장하면 된다. 어쨌든, 위처럼 설정하면 *Visual Assist X*처럼 타이핑과 동시에 타이핑 한 것과 유사한 항목을 선택해준다. 참고로 위의 이미지는 `2018.3.4` 버전이다.

그리고 만약에 당신이 이 글을 보고 Resharper C++을 Unreal Project에 적용하고 싶다면 조금 더 기다렸다가 해보기를 권한다. `2018.3.x`의 Indexing 속도가 굉장히 느리기 때문이다. 나의 프로젝트는 그래도 3~5분내로 읽었지만 Unreal Engine의 소스를 Indexing 할 때 굉장히 오래(약 30분)걸렸다. `2019.1 EAP`를 썼을 땐 5분 안팎의 시간을 보였다. 하지만 저 버전은 좀 불안정한 것 같다. Code Assistance도 금방 깨져버리고 2018.3의 메뉴와 조금은 달라져있기 때문에 설정에 불편을 초래할 것이다.

---

일단 생각나는 대로 이렇게 적어보았다. 앞으로도 이렇게 짧더라도 정리해두어야 겠다.