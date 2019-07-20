---
layout: post
title: Unity에서 Screenshot을 공유하기
date: 2019-07-20 19:11:00
author: Jeongjin Oh
categories: Study
tags: Unity Android Screenshot NativeShare
---

요즘 백수 생활을 하다보니 개인 프로젝트에 투자할 시간이 굉장히 많아졌다. 그렇다고 매일매일 하는 것은 아니지만 (...) 어쨌든 최근에는 부쩍 늘은 건 사실이다. 퇴사하고 나서는 노느라 정신이 팔렸지만 지금은 놀 것도 없고(?) 다시 코드를 붙잡고 있는 시간이 많아졌다. 슬슬 다시 회사로 기어 들어가야 하는 시기가 온 것 같다. 안그래도 부모님께서 슬슬 일하라고 하시니까 ㅠㅠ...

어쨌든 [게임 포켓](https://gamepocket.team)에서 진행 중인 `공주의 탄생`이라는 프로젝트를 작업하고 있다. 현재 Alpha Milestone이긴 하지만 나의 작업 분량은 끝나서 먼저 Beta Feature를 작업하고 있다. 그 중 하나인 Screenshot 공유 기능이 간단해 보이고 다른 기능들과 크게 연관이 없어서 먼저 해보고 있는데 이게 생각보다... 진행이 잘 안되어서 작업(이라고 쓰고 삽질이라고 읽는다) 일지를 기록해보는 것이 좋겠다 싶어서 그동안의 삽질 절차를 써내려가보고자 한다.

---

실질적인 작업은 3일전부터 시작했다. 원래는 게임 내의 버튼을 누르면 Screenshot이 찍히고 별도의 공유하기 버튼을 통해서 SNS로 공유할 수 있는 그런 시스템이다. 그런데 생각해보면 모바일 OS들은 Native에서 공유하는 기능을 제공하고 있으니까 그 기능만 호출하면 따로 JNI를 통해서 뭔가 작업하지 않아도 되지 않을까 싶어서 그 방향으로 진행했다.

일단 iOS는 빌드조차 하지 못하고 있으니까 Android 부터 기능을 만들어가고자 했다. 다행히도 전세계에는 나와 같은 고민을 하는 사람이 많기 때문에 Android의 공유 기능을 호출하는 코드는 쉽게 구할 수 있었다. 문제는 이 코드가 최신 Android에서는 정책상의 이유로 막히게 되었다는 것. 왜 안되는지는 다른 분들의 블로그에 신나게 쓰여 있으니 여기에서는 따로 쓰진 않겠다. 그래서 우회를 해야 하는데 여기서부터는 결국 이전보다 훨씬 복잡한 과정이 펼쳐질 것만 같았다. 순간 뇌절하면서 하루 정도는 그냥 아무것도 하지 않았다 (...)

다음 날에 정신차리고 위에서도 말했던 것처럼 나와 같은 고민을 한 전세계인들의 지식(?)을 찾아보았다. 역시 Asset Store에 Android와 iOS 둘 다 지원하는 [`NativeShare`](https://github.com/yasirkula/UnityNativeShare)라는 Asset이 무료로 있었다. 받아서 사용해보니 공유 기능이 잘 된다! 하지만 문제가 있었으니...

> 두번째 공유 이후에는 화면이 멈춘다!

하... 왜일까? 일단 처음에 생각이 든 것은 약 5년 전에 친구네 회사에서 알바할 때 결재 후에 화면이 멈추는 문제가 있었던 것이 떠올랐다. 게임→결제 액티비티→게임 순으로 진행 될 때 화면이 전환될 당시의 화면으로 멈추면서 아무 것도 되지 않는 문제가 있었다. 당시에 Logcat을 통해 로그들을 세밀히 살펴보면서 발견한 것은 액티비티가 전환될 때 렌더해야할 것이 굉장히(?) 많으면 그리다가 멈춰버리는 것으로 보였다. 그래서 그 때는 NGUI로 개발중이었는데, UI Layer의 최상단에 최대값이 255를 기준으로 1로 설정한 전체화면을 덮는 흰 패널을 넣었더니 해결되었다. 지금 개발중인 게임은 uGUI로 만들고 있지만 뿌리는 같다(...)고 생각하여 한번 시도해보았으나 실패했다.

혹여나 완전히 불투명해야 하는게 아닐까 싶어서 가림막을 완전히 까맣게 하고 또 하나 의심되는 구문이 있었으니

```csharp
private IEnumerator _DoScreenshotAndShare()
{
    yield return new WaitForEndOfFrame();

    var fileName = System.DateTime.Now.ToString("yyyy-MM-dd-HHmmss");
    string filePath = Path.Combine(Application.temporaryCachePath, $"{fileName}.png");
    var ss = ScreenCapture.CaptureScreenshotAsTexture();
    var bytes = ss.EncodeToPNG();
    File.WriteAllBytes(filePath, bytes);

    yield return new WaitForSecondsRealtime(0.5f); // <- 바로 여기

    Destroy(ss);

    DialogBlank dialog = App.ui.Generate<DialogBlank>();

    new NativeShare().AddFile(filePath).Share();

    yield return new WaitForSecondsRealtime(1.0f); // <- 바로 여기

    yield return new WaitUntil(() => _isFocus);

    dialog.CloseAction();
}
```

`WaitForSecondsRealtime`은 왠지 게임 액티비티가 비활성화 상태여도 동작을 했을 것 같아서 `WaitForSeconds`로 변경했더니 되었다. 불투명한 것은 아무 상관이 없었다 (...) 제대로 헛다리 짚은 것. 그런데 왜 `WaitForSecondsRealtime`에서는 렌더링이 멈춰버렸던 것일까? 비슷한 사례는 있었지만 미묘하게 내가 겪었던 것과는 다른 문제였다. 이와 관련된 문제는 조만간 로그를 자세히 살펴보면서 알아보아야겠다. 어쩌면 그저 이 버전에서만 간헐적으로 나오던 문제일 수도 있으니...

---

일단 Screenshot에 대한 작업은 마무리했다. 저 문제의 원인은 나머지 Feature가 마무리되면 살펴봐야겠다.
