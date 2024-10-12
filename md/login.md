# assignment3 - login 레이아웃 구현

[완성 페이지 링크](https://cocozo.github.io/homework/login/login.html)

## step1 html 레이아웃 짜기

### main 섹션

``` html
<body>
    <section class="main">
        <figure class="logo">
            <img src="../assets/login/logo.svg" alt="네이버 로그">
            <figcaption class="sr-only">naver logo</figcaption>
        </figure>

        <form action="" method="post" class="login_section">
            <label class="sr-only" for="email">이메일</label> <input type="email" placeholder="아이디(이메일)" id="email" required>
            <span class="error-email">아이디는 이메일 형식으로 입력해 주세요</span>
            <label class="sr-only" for="password">패스워드</label> <input type="password" placeholder="비밀번호" id="password" minlength="10" required>
            <span class="error-password">비밀번호는 10자리 이상으로 해주세요</span>
            <label class="sr-only" for="login">로그인버튼</label> <input type="submit" value="로그인" id="login">
            <div class="bottom-wrapper">
                <div class="checkbox-wrapper">
                    <input type="checkbox" name="" id="save_login">
                    <label class="checkbox-label" for="save_login" tabindex="0"></label>
                    <span>로그인 상태유지</span>
                </div>
                <div class="ip-switch">
                    <span><a href="./pages/ip_security.html" target="_blank">IP보안 <strong>OFF</strong></a></span>
                </div>
            </div>
        </form>
    </section>
</body>
```
body에 main class를 가진 section을 하나 만들어, 모든 login 요소들을  감싸주었다.

### section의 배치
section안데 두가지 요소를 배치했는데,

``` html
        <figure class="logo">
            <img src="../assets/login/logo.svg" alt="네이버 로그">
            <figcaption class="sr-only">naver logo</figcaption>
        </figure>
```
네이버의 로고를 표시하는 figure 요소와

``` html
    <form action="" method="post" class="login_section">
        ...
    </form>

```
본격적인 로그인 요소들을 담은 form구조를 배치하였다.

### 웹접근성 표시를 위한 배치

form 구조를 살펴보면 조금 배치가 복잡하게 이루어져 있다는것을 볼 수 있는데,
``` html
            <label class="sr-only" for="email">이메일</label> <input type="email" placeholder="아이디(이메일)" id="email" required>
            <span class="error-email">아이디는 이메일 형식으로 입력해 주세요</span>
            <label class="sr-only" for="password">패스워드</label> <input type="password" placeholder="비밀번호" id="password" required>
            <span class="error-password">비밀번호는 10자리 이상으로 해주세요</span>
            <label class="sr-only" for="login">로그인버튼</label> <input type="submit" value="로그인" id="login">
            <div class="bottom-wrapper">
                <div class="checkbox-wrapper">
                    <input type="checkbox" name="" id="save_login">
                    <label class="checkbox-label" for="save_login" tabindex="0"></label>
                    <span>로그인 상태유지</span>
                </div>
                <div class="ip-switch">
                    <span><a href="./pages/ip_security.html" target="_blank">IP보안 <strong>OFF</strong></a></span>
                </div>
            </div>
```

대다수의 input 태그 주의에 label 요소가 있는것을 확인 할 수 있다.
해당 요소는, 웹 접근성을 위해 배치된 요소와, 또한 필요에 의해 배치된 요소로, 추가적인 커스텀 요소를 만들기 용이하게 배치 해 두었다.  
(sr-only class를 가진 친구들은 화면에 보이지 않을 것 이다.)


## step2. css 구조 설계

이번 css를 활용하며, 추가적으로 몇가지 부분을 조금 더 생각하면서 짜 보았다.

### 기본적인 css요소들

``` css
/* mordern reset 설정 */
@import "../../common/modern-reset.css";

/* global custom property 설정 */
:root {
    --standard-font-size: 16px;
    --standard-font-color: #121212;
}
```
reset 과 custom property를 이용하여, 기본적인 스타일을 일관되게 만들어 주었다.

### sr-only

``` css

.sr-only {
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

```
웹 접근성을 위해 주가해둔 label구문들을, 화면상에 보이지 않게 배치를 하기 위해, sr-only class를 가진 클래스들에게 해당 css속성을 주도록 제작하였다.

### main section의 기본스타일 설계

``` css
.main {
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-inline: 1.25rem;
    padding-left: 1.25rem;
    padding-right: 1.25rem;
}

.main .logo {
    width: 230px;
    inline-size: 230px;
    margin-top: 5rem;
    margin-bottom: 5rem;
    margin-block: 5rem;
}

```
***거의 모든 크기요소에는, rem 단위로 크기를 설정하게 통일하였다.***
기본적으로, flex를 이용하여 요소를 수직방향으로 배치하였고, main의 중앙정렬과, 또한 기본적인 padding을 주었다.

### 중첩구문의 활용

``` css
.login_section {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;

    ...

    input[type="email"],
    input[type="password"],
    input[type="submit"] {
        width: 100%;
        inline-size: 100%;
        height: 2.8125rem;
        block-size: 2.8125rem;
        padding-inline: 20px;
        padding-left: 20px;
        padding-right: 20px;
        color: var(--standard-font-color);
    }

    ...

}
```

form관련 요소를 스타일링할때, 해당 요소의 스타일들은 모두 nesting구조를 활용하여 구조를 제작하여, 관리의 편리함과 또한 **모듈화**의 이점을 줄 수 있었다.

### checkbox의 스타일 변경
``` css

    input[type="checkbox"] {
        display: none;
    }

    input[type="checkbox"] + label {
        display: inline-block;
        background: url(../../assets/login/checkbox_unchecked.svg) no-repeat;
        width: 1.5rem;
        height: 1.5rem;
        cursor: pointer;
        margin-right: .3125rem;
        margin-inline-end: .3125rem;
    }

    input[type="checkbox"]:checked + label {
        background: url(../../assets/login/checkbox_checked.svg) no-repeat;
    }

```
checkbox의 기본 스타일을 제거하고(제거라기보다는 안보이게 만든것 뿐이다.) checkbox에 연결되어있는 label의 특성을 이용하여, label을 이용하여 checked / unchecked svg를 활용하여 커스텀 checkbox를 만들었다.

### 반응형 구조의 설계

기본적인 디자인은, 모바일을 기준으로 제작하였으나, 디바이스의 width에 따른 반응형 웹설계를 요구하였으므로, media-query를 이용하여 구조를 설계하였다.
``` css

@media (min-width:768px) {
    .main {
        width: 500px;
        inline-size: 500px;
    }
}

@media (max-width:768px) {
    .main {
        width: 100%;
        inline-size: 100%;
    }
}

.login_section {

    ...

    @media (min-width: 768px) {
        .bottom-wrapper {
            justify-content: space-between;
        }
    }

    @media (max-width: 768px) {

        .bottom-wrapper {
            justify-content: end;
        }
        .ip-switch {
            display: none;
        }
    }

    ...

}

```

main 요소와, bottom-wrapper에 있는 요소에 대해, 크기에 따른 속성을 달리 주었다.

 - width의 크기가 768px 이하일때
    - main의 크기를 화면의 크기에 따라 조정(100%)
    - ip보안 요소 숨김
    - 로그인 상태유지 체크박스 오른쪽으로 배치  
      
 - width의 크기가 768px 이상일때
    - main의 크기를 500px로 고정
    - ip보안 요소 보이
    - 로그인 상태유지 체크박스 왼쪽으로 배치