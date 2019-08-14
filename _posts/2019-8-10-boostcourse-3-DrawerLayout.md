---
title:  "[boost course] DrawerLayout"
categories:
  - boostcourse
tags:
  - 부스트코스
  - 안드로이드프로그래밍
  - Android
  - DrawerLayout
---


## Drawer Layout 

이번 프로젝트에서는 DrawerLayout을 사용했다. 햄버거 버튼을 누르면 왼쪽에서 튀어나오는 그것...오늘은 공홈에 대해 번역해 정리해 두도록 하겠다. 

[공홈DrawerLayout](https://developer.android.com/reference/android/support/v4/widget/DrawerLayout)


DrawerLayout은 어느 한 쪽 (왼/오) 켠에서 "drawer" view가 끌어져 나오는 형태로서, 최상위 컨테이너로서 동작한다. 

Drawer의 위치와 layout은 자식 view의 `androud:layout_gravity` attribute 에 따라 왼쪽 또는 오른쪽 (혹은 start/end, 버전에 따라)에서 나올 지 결정된다. 어느 한 쪽에만 drawer가 나올수 있다는 점을 기억하자. 양 쪽 벽에 drawer를 설정한다면, 런타임에서 exception이 발생할 것이다. 

DrawerLayout을 사용하기 위해서는, 첫 번쨰 자식 객체인 primary content view의 width엔 height을 `match_parent`로 두고, `layout_gravity`는 설정하지 말자. Main content view이후에 drawer는 child view로서 Add하고 적절하게 `layout_gravity`를 설정하자. Drawer는 보통 height로 `match_parent`를 사용한다. 

`DrawerLayout.DrawerListener`는 drawer view의 motion 상태를 모니터하기 위해 쓰인다. 애니메이션 도중에 리소스를 많이 소모하는 동작을 피하자. 끊길 수 있다. 소모값이 큰 동작은 `STATE_IDEL` 상태에서 행하자. `DrawerLayout.DrawerListener`는 각각의 callback method에서 default 또는 no-op implementations을 권한다. 

안드로이드 디자인 가이드에 따르면, 왼쪽에 위치한 drawer는 앱을 둘러볼만한 컨텐츠를 가져야만 하고, 오른쪽에 나오는 것은 현재 위치의 정보에 대한 액션을 가져야 한다. 이러한 왼쪽-네비게이션, 오른쪽-액션 컨셉은 Action Bar와 같이 모든 곳에 적용할 수 있다. 

더 많은 설명은 [네비게이션 UI 만들기](https://developer.android.com/guide/navigation/navigation-ui#add_a_navigation_drawer)에서 볼 수 있다. 

## 예시 code 

![](https://developer.android.com/images/topic/libraries/architecture/navigation-drawer.png)

```java
<?xml version="1.0" encoding="utf-8"?>
<!-- Use DrawerLayout as root container for activity -->
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true">

    <!-- Layout to contain contents of main body of screen (drawer will slide over this) -->
    <fragment
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:id="@+id/nav_host_fragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph" />

    <!-- Container for contents of drawer - use NavigationView to make configuration easier -->
    <android.support.design.widget.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true" />

</android.support.v4.widget.DrawerLayout>
```

```java
AppBarConfiguration appBarConfiguration =
        new AppBarConfiguration.Builder(navController.getGraph())
            .setDrawerLayout(drawerLayout)
            .build();
```

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    setContentView(R.layout.activity_main);

    ...

    NavController navController = Navigation.findNavController(this, R.id.nav_host_fragment);
    NavigationView navView = findViewById(R.id.nav_view);
    NavigationUI.setupWithNavController(navView, navController);
}
```

