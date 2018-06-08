---
layout: post
title: MacBook Pro 13" 2017 + Razer Core with NVIDIA GeForce GTX 970 연동 후기
date: 2018-01-11 00:00:00
author: Jeongjin Oh
categories: Tech
tags: Razer Core MacBook Pro 13" 2017
---

이번에 9박 10일(2018/1/9~2018/1/18) 일정으로 한국에 오게 되었다. 쉬는 거였으면 정말로 좋았겠지만, 나름 일을 하러 온 것이다 (하지만 전혀 일하고 있지 않지...) 어차피 오게 될 거, [지난 포스팅]({{ site.baseurl }}/Considering-To-Buy-About-ThinkPad)의 원인이 된 것 중 하나인 Razer Core를 나의 MacBook과 연동을 꼭 성공시키고야 말겠다는 목표를 세웠다.

사실 이전에도 한국에 올 때마다 시도를 했던 것이 바로 Razer Core와의 연동인데, 어째선지 모르겠지만 계속 잘 되지 않아서 결국은 포기하고 돌아가는 루틴을 반복했었다. 그런데 지금까지 되지 않았던 이유가 있긴 했었다. 바로 High Sierra로 OS를 판올림한 것이다. OS의 버젼이 올라가고나서 기존의 Third Party Driver들이 아직 대응이 안된 상태였고, 나는 그 대응이 안되고 있던 녀석으로 '왜 안되는 거야 XXX!!!' 하면서 있던 것이었다.

어쨌든, 요번에 와서 새로운 패치가 적용된 녀석으로 진행했지만 여전히 되지 않았다. 왠지 지난 날에 삽질했던 흔적들이 남아있어서 그런 것이 아닐까 싶어 과감하게 macOS를 밀어버리고(!) 진행했다.

![시스템 프로파일러에서 확인한 녀석]({{ site.baseurl }}/images/2018-1-11-Razer-Core-Attached-To-MacBook-Pro/1.png)

그러니까 되는 것이 아닌가! 너무 감격하여 벤치를 돌려보려 했지만 뭔가 이상한 것을 깨달았다.

![왜 그래픽이 Intel이죠...?]({{ site.baseurl }}/images/2018-1-11-Razer-Core-Attached-To-MacBook-Pro/2.png)

그래서 뭔가 이상하다 싶어서 재시작을 한 번 더 해보았지만, 그 이후로 다시는 NVIDIA GeForce GTX 970이라는 녀석은 인식되지 않았다. 순간 큰 절망에 휩쌰였고, 모든 것이 틀렸다고 생각하여 ThinkPad X270 견적이나 맞춰서 결제해야겠다고 생각한 순간, X270 가격이 원래대로 돌아간 것을 보게 되었다. 그래서 그냥 한 번 더 시도를 해볼까 싶어서 다시 가이드 사이트로 방문하여 글을 다시 읽어보는데, 아랫쪽에 뭔가 팁이 있더라.

![혹시라도 안되면 이렇게 하세요]({{ site.baseurl }}/images/2018-1-11-Razer-Core-Attached-To-MacBook-Pro/3.png)

음?! 그런 것이 있었다니... 영어가 짧아서 못 보고 지나간 것 같다. 위의 글대로 시도해보니,

![olleh!]({{ site.baseurl }}/images/2018-1-11-Razer-Core-Attached-To-MacBook-Pro/4-1.png) ![]({{ site.baseurl }}/images/2018-1-11-Razer-Core-Attached-To-MacBook-Pro/4-2.png)

된다 되~! 나는 너무나 신나서 벤치를 돌려보았다.

![꽤 준수한 성능을 보여주었다.]({{ site.baseurl }}/images/2018-1-11-Razer-Core-Attached-To-MacBook-Pro/5.png)

그리고 그대로 Steam을 설치하여 Tomb Raider 2013을 실행해보았는데, 상당히 깔끔하게 잘 돌아가는 것을 확인했다. 다만 Razer Core에서 엄청난 소리가 들리긴 했지만 (...) 해당 게임을 QuickTime Player의 화면 녹화 기능을 이용하여 기록해보았지만, 인게임에서 보여준 부드러운 프레임의 모습을 제대로 찍어주지 못했다. 거의 스틸컷을 보는 것만 같은 영상이라서 올릴 수 없을 것 같다.

아, 가이드 사이트에 대한 링크를 걸어놓는다는 걸 깜빡했다. [이곳을 방문하여 보면 된다 (영어주의).](https://egpu.io/setup-guide-external-graphics-card-mac/) 연동이 가능한 eGPU와 이론(?), 가이드 등이 안내되어 있다. 해당 사이트는 eGPU 관련하여 심도있게 대화가 오고가는 곳인 것 같다.

일단 내가 성공(?)했던 순서로는,

1. [여기에 방문](https://egpu.io/forums/mac-setup/wip-nvidia-egpu-support-for-high-sierra/)하여 macOS 버전에 맞는 녀석들로 다운로드하여 안내하는 절차대로 설치한다.
2. [tb3-enabler](https://github.com/KhaosT/tb3-enabler)를 활성화 한다 (해당 링크에 설치법이 나와있다.)
3. [automate-eGPU.sh](https://github.com/goalque/automate-eGPU)를 실행시킨다 (역시나 해당 링크에 설치법이 나와있다.)

이렇게라도 되니까 그래도 속 시원하다. 사실 Razer Core가 그래픽 카드를 제외하더라도 비싸기 때문에, 어떤 식으로라도 써야하는 압박감(?) 같은 것이 있었다. 기존의 Razer Blade Stealth를 과감하게 팔고 MacBook Pro를 살 수 있던 것도 MacBook Pro와 Razer Core가 연동이 된다는 글을 보았고, 여러 인증 영상들도 나와있었기 때문이었다. 이렇게까지 고생하면서 연동할 줄은 꿈에도 몰랐던 것이다. 역시 인생은 타노시하구만...

혀튼 드디어 한국에서 짐만 되었던 Razer Core를 일본으로 가져갈 명분이 생겼다. 이번 캐리어도 꽤나 무겁게 가져갈 것 같다. Core를 넣고 남는 공간에 PS VR이나 넣어놔야겠다.
