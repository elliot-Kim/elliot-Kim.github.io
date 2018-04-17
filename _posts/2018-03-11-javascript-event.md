---
title:  "[Beginning JavaScript] Event"
categories: 
  - javascript
tags:
  - Beginning JavaScript
sidebar:
  nav: "doc"
---





## 이벤트란 무엇인가? 

Beginning JavaScript에서 chapter 하나를 온전히 바쳐서 쓸 만큼 이벤트는 중요하다. js에서 이벤트란 현실에서 전화벨과 같다. 친구로부터의 만남을 기대하고 있다면, 친구로부터 전화가 올 것이고 — 우리는 그것을 기다리고 있다. 전화벨이 울리고 나서야 그 다음 단계-친구와의 만남- 을 수행할 수 있게 된다.

js에서 어떤 일을 수행할 수 있을 때, 언제 수행할 것인지를 결정하는 것이 바로 event이다. 다른 말로 event handler라고 하며, listener라고도 불리는 이유다.

### Event의 종류를 알아보자

Mouse event: 마우스를 이용해 하는 모든 것들 . 클릭, 커서 이동, 더블클릭, 드래그 등
Keyboard Event : 키보드를 누를 때, 뗄 때
Progression Event : object가 다른 상태로 변화할때를 통칭한다.
Form event : form(?)안의 무엇인가 변화할 때
Mutation event : DOM nodes가 변화할 때 ← 3번과 뭐가 다름?
Touch Event : user가 sensor를 touch할때
Error event : error 가 발생할 때
Event를 통해 CODE를 실행시키는 방법은 여러 가지가 있다. 특히 obj자체에도 event관련 method나 property 가 있는 것들이 있으나, native js에는 존재하지 않고, 특수한 obj들 — BOM과 DOM은 갖고 있다.

3가지 event 수행 방법에 대해 하나씩 알아보도록 하자.

Assigning HTML attributes
Assigning an object’s special properties
Calling an object’s special methods

### 첫번째. HTML attribute을 이용한 Event 핸들링

```html
<a href=”somepage.html” onclick=”alert(‘You clicked?’)”>Click Me</a>
```

`<a>` 태그는 자체적으로 onclick 시에 지정된 링크로 이동하는 기능을 가진 (특수한) 태그지만, onclick attribute을 임의로 추가함으로서 alert CODE를 추가적으로 실행할 수 있게 만들었다. 라인이 하나일 경우에는 쓸 수 있으나, 복잡한 CODE를 실행시키에는 맞지 않다. 이를 위해서는 onclick에 function을 연결하면 된다.

```html
<img src=”usa.gif” onclick=”changeImg(this)” />

…

<script>
function changeme(that){}

</script>
```


이런식으로.. 여기서 this가 뜻하는 것은 <img> 태그가 되고 that은 this를 refer하기 위한 parameter명으로 많은 개발자들이 사용한다.

추가적으로, 위와 같이 function이 2개 이상의 element에 연결되어 있을 때, 어떤 element를 통해 해당 이벤트가 발생했는지 확인해볼 필요가 있을 수 있다. 이 때는 다음과 같은 방법을 이용한다.

```html
<p ondblclick=”handle(event)”>Paragraph</p>
<h1 onclick=”handle(event)”>Heading 1</h1> 
<span onmouseover=”handle(event)”>Special Text</span>
<script> 
function handle(e) { alert(e.type);} 
</script>
```

바로 event object자체를 변수로 사용하는 것인데, 위와 같이 3개의 element의 같은 handle f 가 연결되어 있더라도, event.type을 통해 어떤 이벤트가 발생되었는지 구분할 수 있게 된다. (if (e.type == “mouseover”)) 같은 조건문을 통해서 이벤트 종류에 따른 추가 CODE를 수행할 수 있다)

또한 event property중에서 target이라는 것을 사용해 위 예제의 this와 같은 효과를 낼 수 있다. event.target 은 this와 같음.즉 이벤트가 발생되게 된 지점 element.

→ this보다는 event 를 사용하는 것이 훨씬 CODE를 변화무쌍하게 만들어 준다


---

### 2번째 방법, obj prop을 이용한 event발생시키기

즉 element 를 먼저 만들어놓고, 나중에 script에서 element 에 .onclick 등의 method에 선언한 function을 연결하는 것. 

```html
<a id=”someLink” href=”somepage.html”> Click Me </a>
…
<script>
function linkClick()={};
document.getElementById(“someLink”).onclick = linkClick;
```



이 방법을 이용하면, Event1 글에서 다루었던 어떤 event인지 구분할 수 있는 방법은 없어지게 되는데.. (function을 execute하는 것이 아니라 단지 onclick 이라는 prop에 linkClick 이라는 function을 assign 한 것에 불과하다. olinkClick 뒤에 pathensis()가 있어서 변수를 전달하는 게 아님에 주목하라) 또 다 방법이 있다. 자동적으로 event object를 전달하므로!
저렇게 괄호가 없어도, linkClick function을 선언 시에 parameter e 를 사용하고 e.target과 같이 쓰면 알아서 event로 인식한다. (?) 


---

### 마지막 방법인 Standard way 

놀랍게도 앞에서 살펴본 두 가지 방법은 전부 standard한 방법이 아니었다는 것…! 그리고 이것이 바로 jquery에서 사용하는 방법이다. 

EventTarget obj를 새로 이해해야한다. 바로 이벤트가 일어나는 target, element를 뜻하며 기존에 이용한 e.target과 같지만, W3C에서 standard로 만든 obj이다. 이를 이용해서 어떤 이벤트가 일어나는지 알 수 있고 listen 방법을 지정할 수도 있다. 
두가지 중요한 method가 있는데, 

```
addEventListener()
```

event listener를 달아주는 method이다. 즉..
`getElementById` 등으로 element를 불러온 후 `.addEventListener( <이벤트명>, <function>)` 형식으로 작동한다.

예시)
```html
function linkClick() { alert(“This link is going nowhere”);
e.preventDefault();
}
link.addEventListener(“click”, linkClick);
```


당연히 기명 함수를 쓰는것이 reuse하기에 좋다. 
```
removeEventListener()
```

addEventListener()로 이미 달아놓은 event listener를 지우는 함수이다. 이것을 사용할 때의 주의점은, event뿐만 아니라 함께 달아놓은 function까지 동일해야 unregister된다는 점! 따라서 기명 함수가 유리함은 말할 것도 없다. 

이 standard방법이 object property에 assign하는 것보다 유리한 점은 무엇일까? 바로 하나의 event listener에 여러 개의 function을 달 수 있다는 점이다..!(property 는 하나밖에 못 assign하잖아) extreamly useful하다고 함
like this..

```html
elementObj.addEventListener(“click”, handlerOne);
elementObj.addEventListener(“click”, handlerTwo);
elementObj.addEventListener(“click”, handlerThree);
```


순서대로 f가 실행되겠죠//
