# cors

출처: [[얄팍한 코딩사전]웹개발 짜증유발자! CORS가 뭔가요?](https://www.youtube.com/watch?v=bW31xiNB8Nc&list=WL&index=2)

<br>

* 한 사이트에서 주소가 다른 서버로 요청을 보낼 때 cors 에러가 자주 발생
* 회사에서 준 과제를 진행할때 내가 만든 프로젝트에서 회사서버로 ajax 요청을 보낼때 특정 파일을 사용하지 않으면 cors에러가 발생했음

#### cors와 sop

* cors(Cross-Origin Resource Sharing): 교차 출처 리소스 공유, 서로 다른 출처(웹사이트와 api주소)간의 리소스(주고받는 데이터)공유를 허용하는게 해주는것
* sop(Same-origin policy): 동일 출처 정책, 같은 출처끼리만 리소스의 공유를 허용하는것. 동일한 url끼리만 api등의 데이터가 접근하도록 막음.

#### cors에러는 어디서 발생하는가?

* 포스트맨이나 스프링등에서 테스트할때는 발생하지 않다가 실제 웹사이트에서 다른 서버로 요청을 보낼때 발생
* 크롬이나 엣지, 사파리같은 **브라우저**에서 발생하는 문제

#### cors에러가 발생하는 이유

* 크롬과 같은 브라우저에서 내가 방문한 사이트들을 믿지 못하기 때문에 보안상의 문제로 cors에러를 발생시킴
* ex) 내 브라우저에서 인증정보등이 저장되어있을때 악의적인 요청으로 개인정보등을 빼내올수도 있음
* **서로 다른 출처끼리 리소스 공유를 가능하게 하려면 응답하는 서버측에서 cors를 허용해줘야함**

#### cors 동작 방식

* 브라우저는 다른 출처끼리 요청이 보내질때 요청에 origin이라는 헤더를 붙이고 이 항목에는 프로토콜, 도메인, 포트 정보 등이 담김
* 응답하는 측은 응답헤더에 지정된  access-controller-allow-origin정보를 실어서 보냄
* 브라우저는 둘을 비교해서 orgin에서 보낸 정보랑 똑같은게 있으면 안전한 요청으로 간주하고 데이터를 받아옴

#### simple request와 preflight request

* cors요청에는 simple request와 preflight request 두가지가 있음
* simple request: get, post 메서드 사용할때 사용, 요청을 보내고 통과못하면 답장을 못받아옴
* preflight request: put, delete등 서버에 영향을 줄수있는 요청들에서 사용, 요청을 보내기전에 preflight요청을 보내 본 요청이 안전한지 확인후에 요청을 보냄



