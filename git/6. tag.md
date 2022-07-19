# tag
 
 출처: [지옥에서온 Git](https://www.inflearn.com/course/%EC%A7%80%EC%98%A5%EC%97%90%EC%84%9C-%EC%98%A8-git/dashboard)
 
 - 특정커밋을 참조하기 쉽게 이름을 부여하는것
 - 유저들에게 release할때 버전을 제공하는데 이때 버전마다 해당하는 커밋이 있고 이는 바뀌지않고 고정되어야함

## annotated tag vs lightweight tag

 - lightweight tag: 태그이름같은 단순한 정보만 남기는 방식
 - annotated tag: 주석으로 상세한 정보추가, 작성자 등 자세한 정보를 남기는 방식


## 명령어

 - git tag (태그이름) (브랜치이름or커밋아이디): 해당브랜치의 최신커밋 or 커밋아이디를 통해 tag생성
 - git tag: 생성한 태그들의 이름확인
 - git tag -v (태그이름): 태그의 상세내용 확인
 - git tag -a 태그이름 -m "메세지내용": annotated방식으로 태그를 생성하고 태그메세지 작성
 - git push --tags: 작성한 태그들을 원격저장소에 올림. --tags명령어를 사용하지않으면 원격에 올라가지않음
 - git tag -d 태그이름: 태그삭제

 
 
 
