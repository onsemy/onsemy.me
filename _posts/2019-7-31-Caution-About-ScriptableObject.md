---
layout: post
title: ScriptableObject 사용시 주의해야할 것
date: 2019-07-31 15:21:00
author: Jeongjin Oh
categories: Study
tags: Unity ScriptableObject
---

어느덧 이 회사에 출근한지 3일이 되었다. 언제나 그렇지만 출근하는 첫 주는 굉장히 빨리 지나간다. 그리고 굉장히 피곤하다 (...) 회사 프로젝트의 코드를 파악하면서 여러 문제가 될 만한 것들을 발견했는데, 그 중 하나를 정리해볼까 한다.

이제까지 이 회사를 제외하고 4개의 회사를 다녔고, 그 중 3개의 회사에서 Unity를 사용했지만 이 회사에서 처음으로 ScriptableObject를 실전에서 꽤나 적극적으로 쓰고 있는 걸 보았다. [지난 2017년에 있었던 Unite Seoul에서의 한 세션](https://www.youtube.com/watch?v=VtuSKmfrFDU&list=LLfmz6FoERzWnkMg0-t1Vx_A&index=2&t=0s)을 들으면서 ScriptableObject가 좋다고 꼭 써보라고 했을 때만 하더라도 나와는 먼 이야기 같았고, 실제로 내가 겪었던 회사 프로젝트에서는 전혀 쓰지 않았기 때문에 제대로 마주할 기회가 없었다. 그러다가 이 회사에서 부딪히게 되었다.

회사 프로젝트에서는 순수 데이터로만 쓰는 것들은 DB(SQLite)로, Unity Asset과 연관된 데이터들은 ScriptableObject로 관리하고 있었다. 이는 내가 이 회사에 오기 전에 있었던 다른 프로그래머가 했던 코드인데, 이런 식의 쓰임새는 처음이었기에 꽤나 흥미로웠다. 게다가 이 ScriptableObject를 처음 커밋한 로그에는 무려 *로딩 개선*이라는 문구가 있었다. 실제로 그런 효과가 있었는지는 잘 모르겠지만, 아마 ScriptableObject를 쓰면 게임이 실행됨과 동시에 메모리에 올려버리기 때문에 처음에만 불러오는 데 시간을 쓰고 추후에는 로딩이 없으니 효과가 있었을 지도 모른다. 나도 첫 회사 프로젝트에서 그런식으로 했었으니까. 문제는 게임의 규모가 커지면 항상 들고 있어야 하는 메모리의 양이 장난이 아니기 때문에 가면 갈수록 상황이 나빠진다. 그래서 나도 첫 회사 때 분산으로 로딩하도록 바꾼 기억이 있다. 우리 게임의 런타임 메모리 용량이 꽤나 큰 것으로 알고 있어서 ScriptableObject에서 불러오는 것 중에서 바로 메모리에 올리지 않아도 될만한 것들을 추릴 수만 있다면 꽤나 큰 개선이 되지 않을까 싶었다.

혀튼 ScriptableObject로 관리되는 데이터들을 훑어보았다. 눈에 띄는게 하나 있었다. `audiotable`... 오디오를 ScriptableObject로 관리한다고? 그럴수도 있다...라고 잠시 생각했지만 압축을 푼 오디오, 특히 배경음들은 굉장한 몸집을 자랑한다. 아직은 회사 저장소에 커밋할 용기(...)가 나지 않아서 새 프로젝트를 만든 후, 게임에서 쓰는 배경음 몇 개를 불러다가 ScriptableObject에 넣고 불러오는 코드를 만들어서 프로파일링을 해보니...

![42MB의 기적](/images/2019-7-31-Caution-About-ScriptableObject/1.png)

*42MB*라는 아름다운 숫자가 등장했다. 허허... 재생하고 있지 않아도, 일부만 테스트했는데도 저정도면 대충 어떨지 상상이 된다. 이 시스템을 걷어내고 싶어도 라이브 중인지라 사이드 이펙트가 굉장히 신경쓰였다. 그렇다고 냅둘수도 없는 노릇... 그래서 혹시나 배경음으로 쓰이는 AudioClip들이 Streaming으로 되어있는지 살펴보았는데, 아니나 다를까 `Decompress On Load`로 설정되어 있었다. 이를 Streaming으로 변경하고 하니까

![299B](/images/2019-7-31-Caution-About-ScriptableObject/2.png)

재생하지 않으니 299B씩만 차지하고 있다. 42MB를 1.4KB 수준으로 낮췄으니 엄청난 성과가 아닐까? 실제 Streaming 상태에서는 얼마가 나오는지는 테스트를 해보아야 겠지만 그리 크지는 않을 것이다. 문제는 Streaming 시 저사양 기기에서 제대로 동작하는지, 다른 문제는 없는지 고려되야 할 것 같다. [AudioClip 설정에 대해서 다른 사이트에서 더 자세히 설명](https://blog.theknightsofunity.com/wrong-import-settings-killing-unity-game-part-2/) 되어 있으니 한번쯤 정독하는 것을 추천한다. 이것 외에는 대부분 Prefab들을 설정해두고 있었다. Prefab들은 그렇게 큰 공간을 차지하지 않고 있었다.

회사를 옮기면서 가장 힘든 것은 적응이다. 하지만 다른 사람이 작업했던 코드를 보는 것도 재미있다. 길지 않은 경력에 여러 회사를 다니게 되었지만 가는 곳마다 코드가 제각각이라 참 재미있다. 물론 그걸 다 파악해야 하는 건 굉장히 힘들고 고된 일이다 (...) 이렇게 새 회사에 와서 새로운 것을 하나하나 경험하고 배워가는 것도 나쁘지 않은 것 같다. 너무 잦으면 피곤하지만 말이다; 일단 정리가 대충 끝났으니 다시 코드의 바다로...ㅠㅠ