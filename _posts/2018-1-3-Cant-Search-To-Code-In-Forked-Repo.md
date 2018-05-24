---
layout: post
title: GitHub Page 저장소 이동
date: 2018-01-03 00:00:00
author: Jeongjin Oh
categories: Life
tags: Github Search
---

블로그에서 예전에 썼던 글에 대해서 검색은 관리자 페이지 혹은 블로그의 검색 기능을 이용하여 쉽게 찾을 수 있다. 하지만 어째선지 GitHub의 검색 기능은 Fork로 가져온 저장소의 경우 동작하지 않았고, 기존 검색 또한 해당 단어가 정확히 일치하지 않으면 검색 결과에 나타나지 않는 걸 알게 되었다. 예를 들어 '새해는 복 많이 받으세요!'를 '새해'로 검색하면 나오지 않고, '새해는'라고 검색해야 나온다. 내가 GitHub 검색 기능을 제대로 쓰지 못하는 거일지도 모르겠으나, 이대로 쓰다간 여간 불편할 것만 같았다.

![Fork된 저장소에서 '새해'를 검색해본 결과]({{ site.baseurl }}/images/2018-1-3-Cant-Search-To-Code-In-Forked-Repo/1.png)

![복사한 저장소에서 '새해'를 검색해본 결과]({{ site.baseurl }}/images/2018-1-3-Cant-Search-To-Code-In-Forked-Repo/2.png)

새해를 검색했으나 하이라이트 되는 부분에 '새해는'은 포함되지 않았다.

![복사한 저장소에서 '새해는'을 검색해본 결과]({{ site.baseurl }}/images/2018-1-3-Cant-Search-To-Code-In-Forked-Repo/3.png)

정확히 키워드를 입력해야 검색이 되는 것을 볼 수 있다.

그래서 일단 기존에 Fork했던 페이지를 복사하여 새로운 저장소에서 관리하기로 하고 이를 실행에 옮겼다. Fork된 저장소를 Duplicate 하는 방법은 [이곳](https://help.github.com/articles/duplicating-a-repository)을 참조하면 된다.

다른 사람이 만들어 놓은 곳에서 아직 기생하고 있지만, GitHub Page의 정확한 사용 방법에 대해서 잘 모르기 때문에 당분간은 이런식으로 기생(?)하면서 지낼 것 같다.

# 덧붙여서

그러고보니 지금 기존에 운영하던 [Tistory Blog](https://blog.onsemy.me)가 접속이 안되는 것을 확인했는데, 왜 그런고 하니 예전에 설정에 애먹던 CNAME 연동 관련인 것 같다. 블로그에 들어가서 재설정을 하려고 하니,

![저장 버튼이 활성화되지 않는다.]({{ site.baseurl }}/images/2018-1-3-Cant-Search-To-Code-In-Forked-Repo/4.png)

여간 짜증나는 일이 아닐 수 없다. 일단 [1차 주소로 접속이 가능](http://onsemy.tistory.com)하나, 방문자 수가 많이 떨어질 듯 하다. 블로그를 방문하는 사람이 얼마나 있을까 싶지만, 아쉬운 건 어쩔 수 없는듯... 이를 계기로 이쪽을 메인으로 돌리게 될 수도 있겠다. 이전 블로그는 흑역사(?) 저장소로...