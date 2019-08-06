---
title:  "[boost course] Task and Back Stack and Flags"
categories:
  - boostcourse
tags:
  - 부스트코스
  - 안드로이드프로그래밍
  - Android
  - Task
  - Back Stack
---

이번에 Activity 생명주기 및 Flag에 대해 배우면서, activity stack에 대해 복습할 기회가 있었다. 아래 글은 개발자 페이지의 "[Understand Tasks and Back Stack](https://developer.android.com/guide/components/activities/tasks-and-back-stack)" 를 참조해 작성했다. 

## Task와 'last in, first out' object structure

Task란 Activity의 집합을 뜻한다.(하나의 앱에서 실행되는 여러 액티비로 이해해도 되지만 꼭 하나의 앱에 속한 액티비티가 아닐 수도 있다. 다른 앱에서 또다른 앱의 액티비티를 부를 수 있기 때문.) Activity는 Stack되어 정리되는데,(이를 back stack이라고도 함) 차례차례 쌓이고 가장 늦게 들어온 것이 가장 먼저 나가는 형태의 last in, first out 구조를 갖고 있다. back버튼을 누르면 가장 마지막에 쌓인 Activity가 destroy되는데. 아래 그림을 참고하자. 

![](https://developer.android.com/images/fundamentals/diagram_backstack.png)

유저가 back 버튼을 연타하면, 계속해서 아래에 있던 Activity가 드러나고, 마지막에는 홈화면이 나오게 되는 것.

## Multi-Task

이를테면 하나의 앱을 실행시켜 여러 액티비티가 스택에 쌓여있는 상황을 보자. 이때의 액티비티들은 Task A에 있다 볼 수 있다. 이 때 유저가 홈버튼을 누르는 순간, Task A는 'background'로 사라진다.(stopped) 유저가 또다른 앱을 실행시키면, 해당 앱이 실행되면서 Task B 가 수행된다. Task B 역시 종료시킨 후 언제든지 Task A로 돌아갈 수 있다. 이를 안드로이드의 멀티타스킹(Multi-tasking)이라 한다. 

![](https://developer.android.com/images/fundamentals/diagram_multitasking.png)


> 백그라운드에 너무 많은 Task가 있을 경우 시스템이 메모리 관리를 위해 destroy시킬 수도 있음. 

또한 하나의 액티비티가 여러 개의 앱에서 다른 order로 실행될 경우, 겹치는 액티비티가 발생할 수 있다. 심지어 Task가 다를 경우에도 가능하다. 

![](https://developer.android.com/images/fundamentals/diagram_multiple_instances.png)


이를 방지하기 위해 Task Manage를 알아야 한다. 


## Managing Task

일반적인 경우엔 필요하지 않은 경우들 - 이를테면 1) 앱에서 특정 액티비티가 실행 시에는 새로운 Task에 담고 싶거나, 2) 실행을 원하는 액티비티가 기존에 열려있을 경우 그것을 갖고오거나, 3) 유저가 Task를 떠날 경우 root Activity를 제외한 Activity는 clear하고 싶거나 등.. 

이를 위해서 manifest의 <activity> 태그의 attributes 또는 intent에 담을 주요 flag에 대해 알아보자. 

**Activity Attributes**
- taskAffinity
- launchMode
- allowTaskReparenting
- clearTaskOnLaunch
- alwaysRetainTaskState
- finishOnTaskLaunch

**Intent Flags**
- FLAG_ACTIVITY_NEW_TASK
- FLAG_ACTIVITY_CLEAR_TOP
- FLAG_ACTIVITY_SINGLE_TOP

## Launch mode 정의하기 

Launch mode는 해당 액티비티의 manifest에 정의하는 방법과, 해당액티비티를 시작할 intent에 flag를 통해 정의하는 방법이 있다. 

만약 Activity A 가 Activity B를 부를 때, intent에 담긴 Flag의 launch mode가 Activity B의 menifest에 있는 launch mode와 상충될 경우 **Activity A에서 보낸 요청이 우선시된다.**

### Manifesto에서 정의하기 

#### standard

디폴트 launch mode. 위에서 설명한 대로다. 하나의 액티비티가 여러 개 생길 수 있다. 

#### singleTop

호출하고자 하는 액티비티가 백스택에 쌓인 액티비티의 **가장 위**에있을 경우, 해당 activity를 새로 instance하지 않고 `onNewIntent()`를 호출한다. 

예를 들면 A-B-C-D 순으로 쌓여있는 Task에서 D를 호출할 경우, D는 맨 위에 있으므로 새로 호출되지 않는다. 하지만 만약 B를 호출할 경우, A-B-C-D-B가 될 것이다.(B가 "singleTop"임에도 불구하고 말이다.)

#### singleTask

액티비티가 다른 task에 실행되 있을 경우, 해당 액티비티의 `onNewIntent()`수행된다. 

#### singleInstance

singleTask와 동일하나, 이 액티비티는 Task의 유일한 멤버로서 활동한다. 즉 해당 액티비티가 속한 Task에는 다른 액티비티가 들어오지 못한다. 


`singleTask`같은 옵션은 앱에서 Android Browser같은 걸 열 때 쓰인다. 어떤 앱에서 Browser를 열 떄, 무조건 새로운 task로 열린다. 즉 부른 앱과는 다른 task에 놓인다.

액티비티가 새 task에서 열렸든 기존 task에서 열렸든간에 관계없이 back버튼을 누르면 밑에 쌓인 액티비티로 데려간다는 것은 이미 알고 있다. 단 어떤 액티비티가 singleTask로 열렸을 경우.. 는 호출한 액티비티가 따라오게 된다. 
아래 그림을 보고 이해해 보자 

![](https://developer.android.com/images/fundamentals/diagram_backstack_singletask_multiactivity.png)


### Intent Flag에서 정의하기 

#### FLAG_ACTIVITY_NEW_TASK

새 액티비티를 새로운 Task에서 시작한다. 만약 부르고자 하는 액티비티가 기존 Task에 실행되어 있을 경우, 기존 Task를 불러오며 `onNewIntent()`가 수행된다. manifest의 `singleTask`와 동일하다. 

#### FLAG_ACTIVITY_SINGLE_TOP

manifest의 `singleTop`와 동일하다. 

#### FLAG_ACTIVITY_CLEAR_TOP

만약 액티비티가 현재 task에 실행되어있다면, 새로 액티비티를 생성하는 것이 아니라 해당 액티비티의 **위에 있는 모든 액티비티들을 destroy** 하고 액티비티는 `onNewIntent()`로 생성한다. 이것은 manifest attribute에는 없는 옵션이다. 

`FLAG_ACTIVITY_SINGLE_TOP`과 `FLAG_ACTIVITY_CLEAR_TOP`는 거의 같이 다니는 옵션이다. 이 두개를 같이 사용함으로써, 어떤 다른 task에 수행되어있던 액티비티든지 찾아내어 불러낼 수 있게 된다. 