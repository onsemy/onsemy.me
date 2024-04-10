---
layout: post
title: Godot에서의 GetNode 함수에 대해
date: 2024-01-22 22:40:00 +0900
author: Jeongjin Oh
category: Study
tags: Godot Csharp Performance
---

최근에 Godot 엔진을 공부해보고 있다. GDScript가 메인이고 주류이지만 언젠가 C#이 메인이 될 것 같아서 처음부터 C#으로 파고 있다. GDScript가 파이썬과 비슷한 문법이면서 게임 개발에 군더더기 없이 간결한 점을 어필하고 있는데 나는 오히려 GDScript가 파이썬 기반이었으면 하는 쪽이어서 아쉬워하는 중.

어쨌든, 공식 문서나 유튜브 등을 통해서 튜토리얼과 샘플 코드 등을 살펴보는 와중에 유니티로 치면 `GetComponent`(또는 `GameObject.Find`)와 같은 기능을 하는 `GetNode`라는 함수가 유니티로 치면 `Update` 함수(Godot에서는 `_Process`) 내에서 계속 호출하는 모습을 볼 수 있었다. GDScript에서는 퍼포먼스가 어떨진 모르겠으나 C# 에서 보기에는 그다지 좋은 선택은 아닐 것만 같아서 다음과 같은 샘플 코드를 통해서 성능을 검증해보고자 한다.

> 아직 내가 Godot 프로파일링에 익숙하지 않아서 메모리나 그런 것은 나중에 추가 포스팅을 하거나 이 포스트에 보충으로 넣어보도록 하겠다. 이번 성능 검증은 Stopwatch를 통해 소요 시간만으로 판단하기로 한다.

```csharp
// NOTE(JJO): 아래 코드는 같은 Sprite2D를 반복문 내에서 GetNode로 호출하는 경우이다.
using Godot;

public partial class Test1 : Node
{
	// Called when the node enters the scene tree for the first time.
	public override void _Ready()
	{
		System.Diagnostics.Stopwatch sw = new();
		sw.Start();

		for (int i = 0; i < 100000; i++)
		{
			var sprite = GetNode<Sprite2D>("Sprite2D");
			sprite.Position = new();
		}

		sw.Stop();
		GD.Print($"Time: {sw.ElapsedMilliseconds.ToString()}");
	}
}

```

```csharp
// NOTE(JJO): 아래 코드는 Export(유니티로 치면 SerializeField)로 외부에서 미리 할당하고 사용하도록 한다.
using Godot;

public partial class Test2 : Node
{
	[Export]
	private Sprite2D _sprite;

	// Called when the node enters the scene tree for the first time.
	public override void _Ready()
	{
		System.Diagnostics.Stopwatch sw = new();
		sw.Start();

		for (int i = 0; i < 100000; i++)
		{
			_sprite.Position = new();
		}

		sw.Stop();
		GD.Print($"Time: {sw.ElapsedMilliseconds.ToString()}");
	}
}

```

테스트 환경은 다음과 같다.

- CPU: i7-13700KF
- RAM: 삼성 DDR5 64GB
- SSD: SK 하이닉스 P31 2TB
- GPU: NVIDIA GeForce RTX 4070 Ti

두 코드를 10번 반복하여 실행해본 결과는 다음과 같다.

| 테스트 내용 | 최소 값(ms) | 최대 값(ms) | 평균 값(ms) |
|:------------:|:--------:|:---------:|:--------:|
| Test1 | 87 | 93 | 90.3 |
| Test2 | 9 | 10 | 9.1 |

평균값이 거의 10배 차이가 나는 것으로 확인되었다. 10만번 반복이긴 하지만 게임을 만들다보면 무시하지 못할 결과긴 하다. 왠지 원인은 유니티와 흡사하다고 생각이 들긴 하지만 유니티에서 Godot으로 넘어오신 분들이라면 평소 습관(?)대로 노드를 적절히 멤버 변수에 캐싱해서 쓰는 것이 좋을 것 같다.

아직 오랜 시간 써본건 아니지만 꽤나 매력적인 엔진인 것 같다. 이런식으로 종종 유니티와 비교하며 공부하는 것도 재미있을 것 같고 혹시나 Godot에 관심있는 분들에게 도움이 되지 않을까 싶다.
