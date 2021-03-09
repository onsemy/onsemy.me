---
layout: post
title: Windows 10 1903 업데이트
date: 2019-09-01 02:50:00
author: Jeongjin Oh
category: Life
tags: Windows 1903 Update
---

*Razer Blade 15 9Gen R80*을 사고나서 Windows 10의 1903 업데이트를 진행하고자 부단히 노력했다. 하지만 매번 21% 또는 48%에서 막히며 번번히 실패... 그래서 오늘 날을 잡아서 각잡고(?) 업데이트를 진행해보기로 했다.

나는 해외의 [이 사이트](https://www.wintips.org/fix-windows-10-update-1903-fails-to-install/)에서 참고하며 진행했다.

1. 이전에 업데이트를 한 적이 있다면 업데이트 폴더를 지우는 것이 좋다. 서비스에서 Windows Update를 중단하고 `C:\Windows`에 있는 **SoftwareDistribution** 폴더를 삭제했다.
2. 그리고 [Windows 10 다운로드 사이트](https://www.microsoft.com/en-us/software-download/windows10)에 가서 `Windows10Upgrade9252.exe`를 직접 내려받아 업데이트를 진행했다.
3. 업데이트 준비를 마치고 다시 시작 전에 컴퓨터에 연결된 모든 USB를 분리했다.

참고한 사이트의 방법에서 조금 섞어서 진행했더니 되었다. 아무래도 USB로 연결된 외장 HDD 또는 키보드/마우스 등에서 문제가 있었던 것 같다.

우여곡절 끝에 결국 업데이트를 했지만 AMD Ryzen CPU가 아니기 때문에 혜택은 커녕 성능 저하가 기다리고 있다. 하지만 아직은 크게 체감되진 않는다. 아마 이게 나의 마지막 1903 업데이트가 아닐까 싶...었지만 앞으로 회사 컴퓨터에서도 같은 일이 벌어질 수 있기 때문에 일단 기록 차원에서 포스팅을 해보았다.
