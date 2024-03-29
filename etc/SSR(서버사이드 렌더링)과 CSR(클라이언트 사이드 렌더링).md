# SSR(서버사이드 렌더링)과 CSR(클라이언트 사이드 렌더링)


## MPA vs SPA

#### MPA

* multi page application의 약자로 여러 페이지로 구성된 웹 어플리케이션
* 탭을 이동할때마다 새로운 html을 받아와서 해당링크로 이동하여 페이지 전체를 렌더링하는 전통적인 웹페이지 구성방식

#### SPA

* Single Page Application의 약자로 하나의 페이지로 구성된 웹어플리케이션
* 브라우저에 최초에 한번 페이지 전체를 로드하고, 이후 부터 필요한 부분 Ajax를 통해 데이터만 전달받음

**- react나 vue등을 이용한 SPA방식이 현재 웹개발의 트랜드**
<br>
**- MPA는 SSR, SPA는 CSR  사용**

<br>

## SSR

**서버로부터 완전하게 만들어진 html파일을 받아와 페이지 전체를 렌더링 하는 방식**

* 클라이언트가 초기 화면을 로드하기위해 서버에 요청을 보냄
* 서버는 화면에 표시하는 필요한 데이터를 구성하고 css까지 적용
* 랜더링 준비를 마친 HTML과 JS코드를 브라우저에 응답으로 전달
* 브라우저는 전달 받은 페이지를 띄우고, 브라우저가 이어서 JS코드를 다운로드하고 HTML에 실행

#### 장점

* 빠른 초기 로딩속도 (해당페이지의 구성정보만 받아오기때문)
* 검색엔진 최적화

#### 단점

* **TTV와 TVI의 공백시간**

   유저가 페이지를 요청했을때 HTML은 바로 내려줄수 있다. 하지만 웹사이트를 동적으로 제어할 수 있는 JS파일은 아직 받아오지 않았기 때문에 사용자가 클릭해도 아무것도 처리할수   없는 상태에 놓인다

  => TTV(사용자가 사이트를 볼 수 있는 시간)과 실제 인터랙션이 가능한 시간(TTI)의 공백시간 발생

* **서버 과부하**

   새로운 요청이 생길때마다 전체 페이지를 다시 받아와야한다. 바뀌지 않아도 되는 부분도 랜더링 되므로 서버에 과부하가 걸릴 수 있음

* **Blinking Issue**

   사용자가 새로고침을 하게되면 전체 웹사이트를 다시 서버에서 받아와야 하므로 화면 깜빡거림이 발생
   
<br>

## CSR

**사용자의 요청에 따라 필요한 부분만 응답 받아 렌더링 하는 방식**

* 클라이언트에서 초기화면을 로드하기 위해 서버에 요청을 보냄
* 서버는 화면에 표시하는데 필요한 완전한 리소스를 응답
  * 이때 SSR과는 다르게 모든 js파일을 다운
  * **따라서 초기 로딩 시간이 더 오래 걸림**

#### 장점

* **변경된 부분과 관련된 데이터만 가져오므로 SSR보다 속도가 빠름**
* 변경된 부분만 요청하기때문에 **서버의 부담이 줄어든다**
* 화면이동시 깜빡거림없이 이동하기 때문에 **사용자 친화적** 

#### 단점

* 초기에 모든 js파일을 다운받아 와서  **초기 로딩 속도가 느림**
* 웹크롤러봇 입장에서 뼈대만 있는 HTML만 보기때문에 **검색엔진 최적화에 불리**




