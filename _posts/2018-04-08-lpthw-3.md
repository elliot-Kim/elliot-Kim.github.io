---
title:  "[python] python3 힘들게 배우기 #3"
categories: 
  - Python3
tags:
  - lpthw
sidebar:
  nav: "doc"
---

영어로 쓰는 거 생각보다 힘들다.

## ex8. Printing 한 번 더.

_.format()_에 대해 조금 더 알아보자

```python
formatter = "{} {} {} {}"
print(formatter.format(1,2,3,4))
print(formatter.format("one", "two", "three", "four"))
print(formatter.format(True, False, True, False))
```

true 나 false는 첫글자를 대문자로 쓰는 것에 유의하라.

위를 실행시키면, 아래와 같은 결과가 나온다.

```
1 2 3 4
one two three four
True False True False
```
마치 formatter 가 4개의 인수를 받을 수 있는 string 이므로 .format()함수를 통해 4개의 인수를 집어넣어 수행한 것으로 볼 수 있다.

## ex9.print..한번 더 한번 더!

프린트만 겁나게 하는구만. 그래도 뭔가 새로운게 계속 튀어나오니 주의깊게 봐보도록 하자.

```python
numbers = "one two three"
ordinal = "First\nSecond\nThird"

print(numbers)
print(ordinal)
```

\n이라는 녀석을 새로 배운다. string중간에 사용할 시 줄바꿈 효과가 있음.

```
one two three
First
Second
Third
```

트리플 쿼테이션도 있다. 얼마든지 Enter로 줄바꿈을 할 수 있게 해준다.

```python
print("""
write
whatever
""")
```
실행하면
```
write
whatever
```


## ex10.escaping

When you try to escape in string, it is called escape sequence. This can be by using \(backslash).

Here’s some of escape sequence which is used more commonly than others.

```python
tabbing = "\tI tabbed."
line_change = "I \nchanged \nmy \nline."
quote_in_quote = "This is (\")quote in quotes."
slash = "I \\ am \\ separated."
```

should be

```
      I tabbed.
I
changed
my
line.
This is (")quote in quotes.
I \ am \ separated.
```

## ex11. questions

put the end=’ ‘ at the end of the _print line_. This means to tell the print that don’t change the line.

Let’s use _input()_ method.

```python
print("What do you like to eat?", end = ' ')
food = input()
print(f"You likes {food}.")
```

should be
```
What do you like to eat? cake
You likes cake.
```

Above, you have to type something.

## ex12. prompting

Method input() also can be used like this way.
```python
food = input("What do you like to eat?")
print(f"You likes {food}.")
```
Exactly same.
```
What do you like to eat?rice
You likes rice.
```
This means the argument of your input method will be showed to user.
