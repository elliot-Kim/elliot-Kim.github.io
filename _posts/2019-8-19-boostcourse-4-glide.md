---
title:  "[boost course] Glide 라이브러리 사용법"
categories:
  - boostcourse
tags:
  - 부스트코스
  - 안드로이드프로그래밍
  - Android
  - Glide
---

이번 4번째 PJT인 네트워킹을 하면서, 이미지 로딩 라이브러리인 Glide를 사용할 기회가 있어 정리해 둔다. 

## Glide란? 

글라이드는 안드로이드의 빠르고 효과적인 오픈소스 미디어 매니지먼트 및 이미지 로딩 프레임워크로서, 미디어 디코딩/메모리 & 디스크 캐싱, 리소스 풀링 기능을 쉽고 간단한 인터페이스에 담았다. 

[공식홈 바로가기](https://github.com/bumptech/glide)
![](https://github.com/bumptech/glide/raw/master/static/glide_logo.png)

글라이드는 fetching, 디코딩, 비디오 스틸/이미지/움직이는 GIF 재생 등을 지원한다. 글라이드에 포함된 유연한 API는 개발자로 하여금 거의 어떤 네트웍 스택에도 plug 시킬 수 있게 한다. 기본적으로 글라이드는 `HttpUrlConnection` 스택을 쓰지만, Google Volley나 Square의 OkHttp 라이브러리도 쓸 수 있다. 

글라이드는 어떤 종류의 이미지라도 빠르고 부드럽게 스크롤할 수 있게 하는 것이 목적이며, 또한 붙이고, 리사이즈하고, 디스플레이하는 데도 효과적이다. 

## Gradle 추가 

```
dependencies {
  implementation 'com.github.bumptech.glide:glide:4.9.0'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.9.0'
}
```

## 사용법

### 이미지 로딩

가장 쉬운 방법은 아래 한 줄로 설명된다.

```java
// For a simple view:
@Override public void onCreate(Bundle savedInstanceState) {
  ...
  ImageView imageView = (ImageView) findViewById(R.id.my_image_view);

  Glide.with(this) // context
  .load("http://goo.gl/gEgYUd") // 이미지 url
  .into(imageView); // 붙일 imageView
}
```

### 이미지 로딩 취소

```java
Glide.with(fragment).clear(imageView);
```

### 각종 옵션

글라이드는 다양한 옵션들을 지원하는데 -변형, 변화, 캐싱옵션 등이다.

```java
//디폴트 옵션
Glide.with(fragment)
  .load(myUrl)
  .placeholder(placeholder)
  .fitCenter()
  .into(imageView);

```
옵션들은 마치 `style.xml`을 이용하든 옵션객체를 이용해 저장하고 이용할 수 있다. 

```java
RequestOptions sharedOptions = 
    new RequestOptions()
      .placeholder(placeholder)
      .fitCenter();

Glide.with(fragment)
  .load(myUrl)
  .apply(sharedOptions)
  .into(imageView1);

Glide.with(fragment)
  .load(myUrl)
  .apply(sharedOptions)
  .into(imageView2);
```

다양한 옵션 [링크](https://bumptech.github.io/glide/doc/generatedapi.html)

### ListView나 RecyclerView에서 사용하기

기본적으로 같은 방법을 사용하고, 글라이드는 View의 재사용 및 취소를 자동적으로 핸들링한다. 

```java
@Override
public void onBindViewHolder(ViewHolder holder, int position) {
    String url = urls.get(position);
    Glide.with(fragment)
        .load(url)
        .into(holder.imageView);
}
```

url의 null check을 할 필요도 없다. 

```java
@Override
public void onBindViewHolder(ViewHolder holder, int position) {
    if (isImagePosition(position)) {
        String url = urls.get(position);
        Glide.with(fragment)
            .load(url)
            .into(holder.imageView);
    } else {
        Glide.with(fragment).clear(holder.imageView);
        holder.imageView.setImageDrawable(specialDrawable);
    }
}
```

