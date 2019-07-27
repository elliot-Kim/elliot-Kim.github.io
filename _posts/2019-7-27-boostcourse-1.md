---
title:  "[boost course] Compound Button"
categories:
  - Android
  - Boost Course
tags:
  - 부스트코스
  - 안드로이드프로그래밍
---

### (잡담) 부스트 코스를 시작하며..

운 좋게 네이버 커넥트재단에서 진행하는 안드로이드 부스트코스에 합격해서 공부하게 되었다. 유다시티 진도를 잠깐 중단하고 당분간 부스트코스과 자바 기초에 집중할 생각이다. 과제 진행하면서 새롭게 배운 부분들을 기록해 놓고자 한다. 

## Compound Button

영화 앱을 만들며 좋아요, 싫어요 버튼 기능을 위해 CompoundButton을 이용했다. 
[공홈링크](https://developer.android.com/reference/android/widget/CompoundButton.html)를 보면, 

> A button with two states, checked and unchecked. When the button is pressed or clicked, the state changes automatically.

즉 기본적으로 두가지 상태를 갖고 있고, 클릭시에 자동으로 변환된다는 말이다. 
이 버튼을 이용하면 굳이 onClickListener를 달지 않고도 상태변화를 이용할 수 있다.

![토글버튼](https://developer.android.com/images/ui/togglebutton.png)

4.0이상에서는 스위치도 이용할 수 있으니 참고하자. 

## 구현 

생성은 일반 버튼과 동일하다. xml파일에 생성한다. 나는 구글예시에 나온 ToggleButton(CompoundButton의 상속버튼)을 썼다. 

```
	<ToggleButton
	    android:id="@+id/tb_thumb_up"
	    android:layout_width="24dp"
	    android:layout_height="24dp"
	    android:background="@drawable/ic_thumb_up"
	    android:textOff=""
	    android:textOn="" />
```

이후 onCrete에서 찾아주면 끝. 


```java
protected void onCreate(Bundle savedInstanceState) {
	...
	thumbUp = findViewById(R.id.tb_thumb_up);
	...
}
```

## onUpCheckedChangeListener

핵심은 onUpCheckedChangeListener 구현이다. 클릭함에 따라 온오프 상태 바뀌는 거는 내장 기능이므로, 온오프에 따라 어떻게 변화시켜줘야 할지 작성해야 한다. 

참고링크:[클릭](https://developer.android.com/guide/topics/ui/controls/togglebutton)

특히 좋아요 버튼이 눌림에 따라 싫어요 버튼 상태가 바뀌도록 하는 부분이 어려웠다. 아래 리스너는 좋아요 버튼에 달 거지만, 좋아요 버튼의 상태가 변경될 시 싫어요 버튼의 상태도 체크하는 로직이 들어있다. (좋아요가 눌리면 싫어요는 안눌림 상태로 바뀌어야 하기 때문..)

```java
CompoundButton.OnCheckedChangeListener onUpCheckedChangeListener = new CompoundButton.OnCheckedChangeListener() {
        @Override
        public void onCheckedChanged(CompoundButton b, boolean isChecked) {
            // 좋아요가 눌릴 경우
            if (isChecked) {
                // 눌림상태로 아이콘 변경
                b.setBackgroundResource(R.drawable.ic_thumb_up_selected);

                // 현재 좋아요 개수에 1을 더한 후 적용
                int thumbUpNumber = Integer.parseInt(thumbUpClickedNumber.getText().toString());
                thumbUpClickedNumber.setText(String.valueOf(++thumbUpNumber));
                // 싫어요 버튼 눌려있을 경우, false로 변경
                if (thumbDown.isChecked()) {
                    // 싫어요 버튼이 false로 바뀜에 따라 onDownCheckedChangeListener 가 수행된다.
                    thumbDown.setChecked(false);
                }

                // 좋아요 버튼이 이미 눌려있는 경우
            } else {
                // 떼임상태로 아이콘 변경
                b.setBackgroundResource(R.drawable.ic_thumb_up);

                // 현재 좋아요 개수에 1을 뺀 후 적용
                int thumbUpNumber = Integer.parseInt(thumbUpClickedNumber.getText().toString());
                thumbUpClickedNumber.setText(String.valueOf(--thumbUpNumber));
            }
        }
    };
```
