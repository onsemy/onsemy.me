---
layout: post
title: C++에서 LevelStreaming으로 불러온 LevelSequence를 재생하는 방법
date: 2019-01-07 15:54:00
author: Jeongjin Oh
categories: Study
tags: UnrealEngine4 LevelSequence LevelStreaming
---

회사에서 언리얼로 작업을 하다보면 수 많은 벽에 부딪히곤 하는데 그럴 때마다 검색을 해보면 대부분은 질문만 있고 답이 없는 (...) 경우가 많다. 그래서 이런 나같은 한국인을 위해서, 또는 이 언어를 이해할 수 있는 사람이나 번역으로 정보를 얻고자 하는 사람들에게 도움이 되기 위해 검색으로 얻지 못했던 답을 찾았을 경우 이렇게 포스팅을 하려고 한다. Unity의 경우에도 시도했으나 아쉽게도 딱 한 번만 이런 고충을 공유했을 뿐인지라...

어쨌든, 우리 회사 프로젝트에서 기존에 `UGameStatics::OpenLevel()`로 Level을 불러오게 만들어져 있다가 최근에 LevelStreaming 방식으로 바꾸게 되면서 OpenLevel 체제에서는 잘 되던 시스템들이 여기저기서 빵빵 터지기 시작했다. 그 중 하나가 바로 LevelSequence가 담긴 Level을 Streaming 방식으로 불러왔을 때 재생이 되지 않는 문제였다. 이 문제를 거의 3일동안 매달리며 왜 안되는지 확인해보았고, 드디어 찾았다. ALevelSequenceActor에 설정되어 있던 Binding이 깨지면서 재생이 제대로 되지 않는 문제였다. 자세한 원인은 잘 모르겠지만 새로 인스턴싱 하는 과정에서 기존에 가리키고 있던 BindingID가 유효하지 않게 되버려서 재생이 되지 않는 것으로 보인다.

BindingID를 의도한대로 배치하기 위해서는 *LevelSequence에서 썼던 모든(!) Actor를 다시 Binding해야 한다*. 아래는 다시 Binding하기 위해 내가 썼던 코드이다.

```cpp
template<typename T>
void USomethingObject::GetBindingID(ALevelSequenceActor* InSequenceActor, FText* InDisplayName)
{
	AActor* TargetActor = nullptr;
	for (TActorIterator<T> ActorIter(GetWorld()); ActorIter; ++ActorIter)
	{
		if (InDisplayName == nullptr ||
			(InDisplayName != nullptr && ActorIter->GetName().Contains(InDisplayName->ToString())))
		{
			TargetActor = *ActorIter;
			break;
		}
	}

	ULevelSequence* LevelSequence = InSequenceActor->GetSequence();
	UMovieScene* MovieScene = LevelSequence->GetMovieScene();
	int32_t PossableCount = MovieScene->GetPossessableCount(); // 1
	for (int32_t Index = 0; Index < PossableCount; ++Index)
	{
		const FMovieScenePossessable& Possessable = MovieScene->GetPossessable(Index); // 2
		if (!LevelSequence->CanRebindPossessable(Possessable))
		{
			continue;
		}

		FGuid ObjectGuid = Possessable.GetGuid();
		FText DisplayName = MovieScene->GetObjectDisplayName(ObjectGuid);

		FMovieSceneObjectBindingID BindingID(ObjectGuid, MovieSceneSequenceID::Root); // 3
		if ((InDisplayName == nullptr && DisplayName.EqualToCaseIgnored(FText::FromString(TargetActor->GetActorLabel()))) ||
			(InDisplayName != nullptr && InDisplayName->EqualToCaseIgnored(DisplayName)))
		{
			InSequenceActor->SetBinding(BindingID, { TargetActor }); // 4
			return;
		}
	}

	UE_LOG(LogTemp, Log, TEXT("something has wrong! - failed rebind"));
}
```

매개변수로 받고 있는 *InSequenceActor*의 경우 밖에서 지정해서 가지고 있거나 *TActorIterator*를 통해서 검색해서 가져와야 한다. *InDisplayName*은 World에서 해당 Actor가 하나라는 확신이 없으면 특별히 지정할 수 있도록 만들어 두었다. 주요 부분에 대해서 설명을 달아보겠다.

> 1. *MovieScene*으로부터 *LevelSequence*에 등록된 모든 Possessable을 조회하기 위해 준비한다.
> 2. Rebind가 가능한지 확인하여 걸러낸다.
> 3. Rebind 가능한 Possessable의 BindingID를 Guid를 통해 얻는다.
> 4. `ALevelSequenceActor::SetBinding()`을 호출하여 *3번*에서 나온 BindingID에 해당하는 Possessable을 World에 놓인 Actor로 다시 Binding을 해준다.

---

뭔가 새해 첫 포스팅이 언리얼이 되어버렸지만 그동안 게을러서 쓰지 않았을 뿐... 새해에는 뭘 목표로 할지에 대한거라던가 그런 것들을 좀 정리해야겠다. 위의 글도 뭔가 두서없이 적어놔서 나중에 정신 좀 차리고 나면 다시 정리를 해야겠다.