---
layout: post
title: Facebook이 설치된 Android에서 Facebook Login을 할 때 주의할 점
date: 2020-01-22 18:31:00
author: Jeongjin Oh
categories: Study
tags: Facebook Login Android
---

오늘 회사 게임의 크나큰 업데이트를 진행했다. 나는 그 중에서 *ONE store*와 관련된 전반적인 업무를 담당했다. 일단 기존에 서비스하던 Google Play/App Store의 Login System을 그대로 가져와야 해서 게임에서 제공하고 있는 Google 및 Facebook Login을 ONE store를 위해 바꾼 Package Name에 맞춰서 대응을 진행했다.

개발 과정에서는 순조로웠다. 여유가... 넘쳤지만 막상 출시를 하고나니 문제가 터졌다. 테스트 해보지 못한 곳에서 항상 문제가 터져나왔다. 여러 문제가 있었지만 내가 맡았던 업무 중 크게 걸린 것이 Facebook Login이었다.

어느 유저가 카페에 Facebook Login이 되지 않는다는 제보를 했다. 덩그러니 스크린샷만 있어서 나는 잠시 멍~ 하고 있었다. 혹시 Live APK에서는 안되는 문제였을까 싶어서 주변에 있는 기기들로 테스트를 진행했으나 정상적으로 로그인 되고 있었다. 그래서 큰 문제가 아닐 것이라고 생각했지만... 그 스크린샷에 나와있던 내용으로 검색해보니 보통 일이 아니었다. 바로 *Facebook App이 기기에 깔려있는 모든 유저들은 Login을 할 수 없었던 것*. 더 많은 피해자가 나오기 전에 빠른 수습이 필요했다. 처음엔 한글로 검색했다가 역시나 도움이 하나도 안되었다. 영어로 바꿔서 치자마자 바로 [솔루션](https://forum.unity.com/threads/solved-problem-with-login-facebook-sdk-android.257482/)이 등장했다. ~~*오오... 구글신 오오...*~~

```
1. Unzip .apk file and extract META-INF\CERT.RSA file
2. In the Prompt, run: keytool -printcert -file CERT.RSA
3. notice SHA1 bytes are printed like 29:37:F1:CB:06…
4. copy SHA1 bytes into HEX to BASE64 converter (http://tomeko.net/online_tools/hex_to_base64.php)
5. see your BASE64 key hash in output field
```

내가 이전에 Facebook Developer Console에서 Key Hash를 등록할 때 Unity에서 생성한 Keystore를 가지고 keytool을 이용하여 뽑았는데, 그 값이 틀렸던 것이다. ~~아니 이게 왜 틀리지;~~ 다행히 같은 Keystore로 Build한 APK의 Key Hash는 바뀌지 않는 것 같다.

> 2020/01/23 추가  
보통 Facebook의 Android Key Hash 등록과 관련하여 아래와 같은 솔루션이 많다.  
```keytool -exportcert -alias <RELEASE_KEY_ALIAS> -keystore <RELEASE_KEY_PATH> | openssl sha1 -binary | openssl base64```  
문제는 위의 솔루션이 Unity에서 생성한 Keystore로는 어째선지 적용이 되지 않는 것인데, 혹시 Android Studio에서 생성한 Keystore는 문제가 없는지 확인해봐야 하지만 테스트할 시간이 여의치는 않다. 뭐 그런데 대부분의 솔루션들에 대한 베이스는 Android Studio를 기반으로 하고 있기 때문에 문제가 없을 것 같다. ~~Unity 이노오오오옴...~~

혀튼 APK를 다시 뽑아야 하는 상황이 아닌 것에 굉장한 감사를 느끼며 이거 하나 테스트를 생각 못했던 내 멍청함과 QA과정에서 아무도 Facebook App이 깔린 상태로 테스트를 시도하지 않았다는 사실에 경악하며... 대규모 업데이트 첫 날을 무사히(?) 마무리할 수 있을 것 같다.

까먹을까봐 부랴부랴 회사에서 빠르게 공유를 해본다. ~~그러고보니 이번에도 Facebook이 문제였네~~
