# Django_00_basic


​	

# Web ?

> 월드 와이드 웹(World Wide Web)이란 인터넷에 연결된 사용자들이 서로의 정보를 공유할 수 있는 공간을 의미하며, 줄여서 WWW나 W3라고도 부르며, 간단히 `웹(Web)`이라고 가장 많이 불린다.
>
> 인터넷과 같은 의미로 많이 사용되고 있지만, 정확히 말해 웹은 인터넷상의 인기 있는 `하나의 서비스`일 뿐.
>
> Web의 작동방식을 간단히 설명하자면, `요청`과 `응답`.
>
> 사용자는 서버에 어떠한 요구사항을 요청하고 서버는 이에 따라 처리결과를 응답한다.
>
> 예를들어 `사용자가 Login 버튼을 누른 행위`는 `user가 서버에 로그인 하고 싶다고 요청`한 것이며, 서버는 `로그인 창을 띄움`으로서 `user가 로그인할 수 있게 응답`한 것입니다.

​	

## web Protocal

{{< mermaid >}}
graph LR;
    A(User) -->|요청 <url로 요청을 접수>| B{Server <Model>}
    B --> C[View에서 요청을 처리]
    C --> D[Template에서 응답페이지를 꾸며]
    D --> |응답| A(User)
{{< /mermaid >}}

- 요청에 대한 응답을 처리할 서버를 만드는 것이 Django

---

​	

# Django ?

> python 기반의 web framework

​	`framework`? :  web 개발시 어려움을 줄이는데 목적이 있는 기능을 통칭

​		

## Django의 개발 방식 (MTV)

> Django의 개발 방식은  MTV(Model, Template, View) 패턴을 따른다.

**M**odel: data를 구성

**T**emplate: 사용자(User)에서 보여주는 화면을 구성 (HTML로 구현)

**V**iew: data 처리 및 전달

​		

## Django_basic

1. 장고는 하나의 프로젝트 단위로 web을 개발한다. 

2. 프로젝트 안에는 여러 app별로 기능을 나누어 구성한다.

   ​	*ex) 회원들 계정을 관리하는 app (accounts) / 게시판을 관리하는 app (articles) 등등*

3. 요청은 Url로 접수하고 (ex. 게시판으로 이동하고 싶어 게시판 버튼을 클릭)

4. 접수된 내용을 View에서 처리 (ex. 게시판버튼을 클릭하면 게시판 화면을 사용자에게 보여주게 구성)

5. 처리한 화면을 Template로 꾸며(구성해) 사용자에게 응답

   ​	

   ​	

   
