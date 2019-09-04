---
title:  "[boost course] 핀치 줌, 아웃 구현하기"
categories:
  - boostcourse
tags:
  - 부스트코스
  - 안드로이드프로그래밍
  - Android
  - pinch zoom, out
---


두 손가락을 이용해 이미지를 확대 축소하는 기능을 라이브러리 없이 구현하는 방법이다. ScaleGestureDetector를 이용한다 [공식홈링크](https://developer.android.com/reference/android/view/ScaleGestureDetector)


## 이미지 뷰 생성 

확대할 이미지뷰를 xml파일에 생성한다. 

```
<ImageView
android:id="@+id/imageView"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:src="@mipmap/ic_launcher"
android:layout_centerVertical="true"
android:layout_centerHorizontal="true" />
```


## 코드 구현 

아래와 같이 변수를 선언한다. 

```java 
private ScaleGestureDetector mScaleGestureDetector;
private float mScaleFactor = 1.0f;
private ImageView mImageView;
```

`onCreate()`바깥에 다음과 같이 `onTouchEvent()`를 오버라이드한다.

```java
public boolean onTouchEvent(MotionEvent motionEvent) {
    //변수로 선언해 놓은 ScaleGestureDetector
    mScaleGestureDetector.onTouchEvent(motionEvent);
    return true;
}
```

ScaleGestureDetector.SimpleOnScaleGestureListener를 상속받아 ScaleListener class를 정의하자.

```java
private class ScaleListener extends ScaleGestureDetector.SimpleOnScaleGestureListener {
    @Override
    public boolean onScale(ScaleGestureDetector scaleGestureDetector){
        // ScaleGestureDetector에서 factor를 받아 변수로 선언한 factor에 넣고 
        mScaleFactor *= scaleGestureDetector.getScaleFactor();
        
        // 최대 10배, 최소 10배 줌 한계 설정 
        mScaleFactor = Math.max(0.1f,
        Math.min(mScaleFactor, 10.0f));

        // 이미지뷰 스케일에 적용
        mImageView.setScaleX(mScaleFactor);
        mImageView.setScaleY(mScaleFactor);
        return true;
    }
}
```

마지막으로 `onCreate()`안에 아래 코드를 넣으면 끝난다.

```java
    // xml에 정의한 이미지뷰 찾고 
    mImageView=(ImageView)findViewById(R.id.imageView);

    // 스케일제스쳐 디텍터 인스턴스 
    mScaleGestureDetector = new ScaleGestureDetector(this, new ScaleListener());
```

### 레퍼런스 

참고 사이트: [https://medium.com/quick-code/pinch-to-zoom-with-multi-touch-gestures-in-android-d6392e4bf52d](https://medium.com/quick-code/pinch-to-zoom-with-multi-touch-gestures-in-android-d6392e4bf52d)