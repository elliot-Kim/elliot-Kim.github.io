---
title:  "[boost course] SQLiteOpenHelper 알아보기"
categories:
  - boostcourse
tags:
  - 부스트코스
  - 안드로이드프로그래밍
  - Android
  - SQLiteOpenHelper
---

SQLiteDataBase를 쉽게 다룰 수 있게 하는 SQLiteOpenHelper 클래스에 대해 알아보자. 공식 홈페이지를 해석하며 분석해 보았다. [공홈](https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper#summary)

## SQLiteOpenhelper 

Database 생성과 버전 컨트롤을 쉽게 할 수 있게 도와주는 helper class이다. 

`onCreate(SQLiteDatabase)`, `onUpgrade(SQLiteDatabase, int, int)`를 반드시 Override해야 하고, `onOpen(SQLiteDatabase)`은 선택적으로 사용 가능하다. 이 클래스는 DB가 있는 경우에는 해당 DB를 열고, 존재하지 않는 경우 새로 생성하고, 필요한 경우에는 업그레이드 한다. DB가 접근 가능한 상태여야만 한다. 

이 클래스는 `ContentProvider` 가 쉽게 db를 열고 업그레이드 하게 해주며, DB업그레이드에 드는 긴 러닝시간을 막아준다. 

> ` AutoCloseable ` 인터페이스가 ` Build.VERSION_CODES.Q` 버전에서 처음으로 릴리즈 되었다. 


## Public constructors

* SQLiteOpenHelper(Context context, String name, SQLiteDatabase.CursorFactory factory, int version)
* SQLiteOpenHelper(Context context, String name, SQLiteDatabase.CursorFactory factory, int version, DatabaseErrorHandler errorHandler)
* SQLiteOpenHelper(Context context, String name, int version, SQLiteDatabase.OpenParams openParams)

: helper object를 생성하여 db를 생성, 오픈, 매니징한다. 
이 method는 언제나 빠르게 return되는데, DB가 `getWritableDatabase()` 또는 `getReadableDatabase()`가 호출될 때까지는 실제로 생성되거나 열리지 않기 때문이다. 

| Parameter | 의미 |
|---|:---:|---:|
| context | db 로의 경로 배치를 위함. `null` 가능 |
| name | db file 의 이름 또는 in-memory database일 경우 null |
| factory | `SQLiteDatabase.CursorFactory`: cursor 객체 생성을 위함. default는 null|
| version | DB의 번호(1에서 시작). DB가 오래된 것이라면 `onUpgrade()`가, DB가 (너무)새로운 것이라면 `onDowngrade()`가 호출됨|


## Public methods

주요 메쏘드에 대해 알아보자. 

* close() : DB객체를 닫는다. 
* getDatabaseName() : 주어진 생성자를 사용해 열린 DB 이름을 반환한다. 
* getReadableDatabase(): 새로운 DB를 생성 또는 열람.
* getWritableDatabase(): 읽고 쓰기가 가능한 새로운 DB를 생성 또는 열람. 최초 실행될 때 `onCreate(SQLiteDatabase)`, `onUpgrade(SQLiteDatabase, int, int)`가 수행되며 `onOpen(SQLiteDatabase)`이 수행될 수도 있다. 
한번 열린 DB는 캐싱되기 때문에 언제든 DB에 쓰기작업을 할 때마다 이 메소드를 사용해도 된다. (더이상 사용치 않을 때는 `close()`를 호출하는 것을 잊지 말자.) bad permission 또는 full-disk로 인해 에러를 뱉을 수 있지만 이후 시도에서는 성공할 수 있다. 

> DB업그레이드는 긴 시간이 걸리기 때문에 메인 쓰레드에서는 호출하지 말자.

* onCreate(SQLiteDatabase db): DB가 새로 생성될 때 호출. Table 세팅 등의 작업을 배치한다.


* onDowngrade(SQLiteDatabase db, int oldVersion, int newVersion): DB가 다운그레이드 필요할 떄 호출 

* onOpen(SQLiteDatabase db): DB가 열릴 떄 호출 
* onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion): DB가 업그레이드 필요할 때 호출. 여기서는 Drop table 또는 add table 등의 새 스키마 세팅을 위한 작업들을 해야 한다. 
SQLite 의 ALTER TABLE 명령어를 [수행하는 방법](https://sqlite.org/lang_altertable.html). 만약 live data에 새로운 column을 추가하고 싶다면 ALTER TABLE을 사용할 수 있다. 또한 column명을 바꾸거나 제거하려면 ALTER TABLE을 이용해 새 테이블을 만든 다음 예전 자료를 붙여넣으면 된다. (순서: 새 테이블 만들기 > 데이터 카피하기 > 헌 테이블 드랍하기 > 새 테이블의 이름을 헌 테이블의 이름으로 교체하기)
작업은 transaction에서 이뤄지므로 수행 중 exception이 발생 시 모든 변경사항은 자동 롤백된다. 


