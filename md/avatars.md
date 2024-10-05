# assignment2 - avatars 레이아웃 구현

[과제 페이지](https://wholesale-snipe-50a.notion.site/12-HTML-CSS-114c3e449f1d80019dcbcde99f9090db)

기본적으로 해당 두 태그를 이용해서  
body를 전체 화면을 감싼다.
~~~ css
    html {
        height: 100%;
    }

    body {
        width: 100%;
        min-height: 100%;
    }
~~~

## step 1. avatar 원형 프로필 만들기

아바타의 프로파일을 만들때, 원형 아이콘과 좌하단의 원형 상태 표시기를 구현하기위해, 다음과 같이 구성하였다.
~~~ html
<figure class="profile">
    <img class="circle-image" src="../assets/avatars/face1.jpg" alt="">
    <div class="circle-status online"></div>
    <figcaption>profile status online</figcaption>
</figure>
~~~
여기서 img는 인물 이미지를, div태그는 원형 상태 표시기로 활용하였다.

여기서 상태표시기는, 이미지에 곂쳐서 표현되어야 하기 때문에,
내부 div태그에 position 속성을 이용하여, 위치지정을 하였다.

~~~ css
.profile {
    position: relative;
}

.profile .circle-image {
    width: 64px;
    height: 64px;
    object-fit: cover;
    border-radius: 32px;
}

.profile .circle-statue {
    position: absolute;
    bottom: 0%;
    right: 0%;
    width: 16px;
    height: 16px;
    background-color: #dbdbdb;
    border-radius: 10px;
    border: #fff solid 1px;
}

.profile .circle-status.online {
    background-color: #4cfe88;
}

figcaption {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border-width: 0;
}
~~~
  

## step 2. 프로필 레이아웃 생성
step 1에서 만든 프로필을 이용해서 과제의 화면과 같은 레이아웃을 만든다.

~~~ html
<section class="avatar-list top">
    <figure class="profile">
        <img class="circle-image" src="../assets/avatars/face1.jpg" alt="">
        <div class="circle-status online"></div>
        <figcaption>profile status online</figcaption>
    </figure>
    <figure class="profile">
        ...
    </figure>
    <figure class="profile">
        ...
    </figure>
    <figure class="profile">
        ...
    </figure>
</section>
<section class="avatar-list bottom">
    ...
</section>
~~~

section 태그를 이용해 각각 4개씩, 8개의 프로필을 감싸는 레이아웃을 만든다.

여기서 4개씩 묶을 경우, 마지막 요소에 쓸데없는 margin이 들어가므로
~~~ css
.profile:last-child {
    margin: 0px;
}
~~~
해당 구문을 주어 마지막 요소에는 margin이 들어가지 않도록 한다

## step3. 브라우저의 지원 요소에 따른 레이아웃 배치방식 변경

과제에서 주어진 요구사항은

> float을 사용하여 레이아웃을 구현해 본다.
단, flex를 지원하는 환경에서는 flex를 이용하여 레이아웃을 구현해 본다.

를 요구사항으로 주었다. 
즉 따라서 브라우져의 flex 요소의 지원여부에 따라 style요소를 따로 적용해 주어야 한다.

### 구성된 html 요소
~~~ html
    <section class="main">
        <section class="avatar-list top">
            ...
        </section>
        <section class="avatar-list bottom">
            ...
        </section>
    </section>
~~~

생략 된 요소는 위의 코드와 동일 하다.

### 공통된 CSS 요소
~~~ css
.main {
    position: absolute;
    display:block;
    width: fit-content;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
~~~
main 섹션을 화면 정중앙에 배치한다.
### #1 기본 float를 이용한 레이아웃 구성

~~~ css
.avatar-list {
    position: relative;
    overflow: hidden;
    margin: 10px 10px;
}

.profile {
    float: inline-start;
    position: relative;
    width: 64px;
    height: 64px;
    margin: 0px 20px 0px 0px;
}

~~~

profile 요소에 float요소를 주어 inline으로 표현이 될 수 있도록 제작한다. 단, 해당 요소를 적용할 경우, 부모요소에서 해당 요소의 크기를 측정하는데 있어 이상이 생기므로,
부모요소에, overflow 라는 요소를 주어 float 속성이 추가된 요소를 인식할 수 있도록 적용한다.

관련 문서 -> [Block formatting context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_display/Block_formatting_context)


### #2 flex를 이용한 레이아웃 구성

~~~ css
.main{
    display: flex;
    flex-direction: column-reverse;
}
.avatar-list {
    position: relative;
    overflow: hidden;
    display: flex;
    margin: 10px 10px;
}

.profile {
    position: relative;
    width: 64px;
    height: 64px;
    margin: 0px 20px 0px 0px;
}
~~~
main 과 avatar-list 클래스를 가진 요소에 flex를 적용한다.
avatar-list 하위에는 proflie 들이 있으므로, 해당 요소들이 avatar-list의 flex 속성에 의해 정렬되고,
main 하위에 avatar-list가 있으므로 해당 요소들 또한 main의 flex요소에 의해 정렬이 될 것이다.

### #3 두 구현방식 합치기

~~~ css
@supports (display:flex) {
    .main{
        display: flex;
        flex-direction: column-reverse;
    }
    .avatar-list {
        position: relative;
        overflow: hidden;
        display: flex;
        margin: 10px 10px;
    }

    .profile {
        position: relative;
        width: 64px;
        height: 64px;
        margin: 0px 20px 0px 0px;
    }
}

@supports not (display:flex){
    .avatar-list {
        position: relative;
        overflow: hidden;
        margin: 10px 10px;
    }

    .profile {
        float: inline-start;
        position: relative;
        width: 64px;
        height: 64px;
        margin: 0px 20px 0px 0px;
    }
}
~~~
어노테이션 @supports를 이용해 flex속성의 지원여부에 따라 각각 다른 스타일이 적용될 수 있게 적용한다.

## 추가 구성요소 제작

main 요소에 border요소를 추가하고, 상단 왼쪽 'avatar'라고 적혀있는 제목을 추가한다.

~~~html
<section class="main">
    <section class="caption">
        <p>avatars</p>
    </section>
    ...
~~~

상단의 avatar 라고 적혀있는 제목을 달기 위해 main 섹션안에 caption 클래스 요소를 가진 section을 만들어 둔다.

메인의 스타일을 수정하고 caption스타일을 추가한다.
~~~ css

.main {
    position: absolute;
    display:block;
    width: fit-content;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    
    border: #dbdbdb solid 1px;
    border-radius: 5px;
    padding: 100px;
}

.caption {
    position: absolute;
    left: 0%;
    top: -10%;
    border: #dbdbdb solid 1px;
    border-radius: 5px;
    padding: 5px;
    color: #8f8f8f;
    font-family: 'Courier New', Courier, monospace;
    margin: 0;
}
~~~

main에 padding 요소를 이용하여서 충분한 내부 여백을 제공해 주고, border요소를 추가해 준다.

caption은 border의 외부에 배치되어 있어야하므로,
position을 absolute로 변경해 주고, 상단 죄측에 위치하도록 포지션을 배치해주고, 스타일을 지정해 준다.
(폰트에 대한 지정설명이 없어 임의의 스타일로 설정해 주었다.)


## 완성본
https://cocozo.github.io/homework/avatars/avatars.html

깃허브로 페이지 배포완료.