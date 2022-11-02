# 🐱Git

```
간단한 사용법을 위주로 정리한 내용이므로 더 자세한 내용은 책이나 구글링을 통해서 학습하길 바란다.
```

### 목차

- [🐱Git](#git)
    - [목차](#목차)
    - [Git이란?](#git이란)
    - [기본 설정](#기본-설정)
    - [파일 추가](#파일-추가)
    - [branch](#branch)
    - [branch 합치기](#branch-합치기)
    - [참고 자료](#참고-자료)
  
### Git이란?

Git은 간단하게 한마디로 버전 관리 시스템이다.

예를 들어, 며칠씩 코드를 작성했다고 가정하보자.

그러다가 갑자기 코드가 제대로 작동하지 않아 2일 전의 코드로 되돌리고 싶어졌다. 이럴 경우 어떻게 해야될까? 아마도 불가능 할 것이다.

아니면 기존에 그동안 작성한 코드를 일별로 따로 저장해서 보관하고 있었다면 가능할지도 모르지만

이럴 때 필요한게 바로 버전 관리 시스템이다. 그 중 가장 유명하고 많이 사용하는 것이 바로 Git이다.

이 Git을 통해 과거로 코드를 되돌리거나 기존에 작업 과정을 편하게 확인도 가능하다.

Git을 설치하는 방법은 아래의 사이트를 참고하길 바란다.

그리고 Git을 처음 접할 때 Github와 함께 접하는 경우가 많아 Git == Github로 생각하는 사람들이 많은데 둘은 다른 것이다.

Git이 버전 관리 시스템이라고 했는데 이는 로컬에서의 버전 관리를 얘기하는 것이다. 그래서 프로젝트를 열심히 해서 Git으로 정리를 해도 프로젝트가 존재하는 디렉토리를 삭제하거나 컴퓨터롤 초기화했다면 그 프로젝트와는 영원한 이별을 맞이할 것이다.

그래서 존재하는 것이 바로 Github 이다. Github는 git 저장소를 관리하는 클라우드 기반 호스팅 서비스로 버전 관리, 소스 코드 공유, 분산 버전 제어 등등이 가능한 원격 저장소라고 생각하면 된다. 

**운영 체제별 Git 설치**

[Windows](https://taewow.tistory.com/13)

[Mac](https://velog.io/@wijoonwu/Mac-OS-%EC%97%90%EC%84%9C-Git-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)

[Ubuntu](https://coding-factory.tistory.com/502)

### 기본 설정

init은 새로운 Git 저장소를 만드는 명령어로 현재 위치한 디렉토리를 기준으로 이제부터 Git으로 버전 관리를 할 수 있게 된다.

![스크린샷 2022-11-03 오전 1 21 20](https://user-images.githubusercontent.com/78605779/199544645-da989971-59f7-48fd-8c27-ff8a2e0bff68.png)

위와 같이 실습할 빈 폴더를 만든 후, git을 설치하면 자동으로 함께 설치되는 gitBash나 vscode같은 에디터로 폴더를 열어 터미널에 `git status`를 작성해서 실행해보자.

```git
git status
```

![스크린샷 2022-11-03 오전 1 26 20](https://user-images.githubusercontent.com/78605779/199545690-08689e74-5799-4bb1-a804-a3ddd13159d0.png)

그러면 위와 같이 에러가 발생할 것이다. 그 이유는 `git status`는 git의 상태를 확인하는 명령어로 파일의 변경 등을 확인할 수 있는데 현재 디렉토리가 git repository로 설정되어있지 않기 때문이다.

그래서 아래 명령어로 현재 위치한 디렉토리를 git repository로 만들어보자.
```git
git init
```

![스크린샷 2022-11-03 오전 1 29 32](https://user-images.githubusercontent.com/78605779/199546527-41789220-6a8e-4660-bc7b-da0e67ecdbbc.png)

그러면 위 그림의 수행 결과를 통해서 현재 디렉터리 아래에 .git 디렉터리를 만들고 여기에 Git 저장소를 초기화한다는 것을 알 수 있다. 실제로 해당 폴더에 가보면 .git 폴더가 생성됐을 것이다. 만약 보이지 않는다면 숨김 폴더로 되어있기때문일 수 있으니 이를 설정하면 확인할 수 있을 것이다.

![스크린샷 2022-11-03 오전 1 33 41](https://user-images.githubusercontent.com/78605779/199547514-db1cd083-88b4-4d94-a1eb-7aafd348d49b.png)


### 파일 추가

이제부터 본격적으로 git을 통해서 버전 관리라는 것을 해보자.

우선 우리가 설정한 git 디렉토리에서 아무 파일이나 생성해보자.

![스크린샷 2022-11-03 오전 1 36 36](https://user-images.githubusercontent.com/78605779/199548234-1ba43566-78b4-4a4d-99a0-c3e39975e086.png)

`1.txt`라는 파일을 생성해 내용을 작성하였다.

현재 이 상태를 기록 또는 백업을 하고 싶을 때 사용하는 것이 add와 commit 명령어이다.

우선 add 명령어부터 사용해보자

```bash
git add <fileName>
```

add 명령어를 통해 원하는 파일을 지정해주면 된다.

쉽게 비유하면 총에 총알을 장전했다고 생각하면 된다. 총에 총알을 장전했으면 이제 뭘 해야하는가? 이제 방아쇠를 당겨 총을 쏠 차례다.

방아쇠를 당기는 행위가 바로 commit 명령어다.

```bash
git commit -m "메모"
```

![스크린샷 2022-11-03 오전 1 47 13](https://user-images.githubusercontent.com/78605779/199550684-14729256-969a-4b84-a6fd-5f8f3c5f7333.png)

위 그림은 `1.txt`라는 총알을 장전해 격발했다. 이로써 우리는 `1.txt`라는 파일을 생성해 작성한 내용을 기록했다.

이제 기록이 잘 됐는지 확인해보자

```
git log --all --oneline
```

log 명령어를 통해서 우리가 기록한 commit 내역을 확인할 수 있다. 명령어에 따른 여러 옵션은 직접 검색해서 찾아보길 바란다.

![스크린샷 2022-11-03 오전 1 59 59](https://user-images.githubusercontent.com/78605779/199553722-becb9245-ee53-40b0-b7c1-aaf1ab4a20fe.png)

위 그림처럼 우리가 기록한 commit을 확인할 수 있으며, 다시 빠져나오고 싶다면 `q`를 입력하면 된다.

위 과정의 자세한 내용은 아래 그림을 참고하기 바란다.

![image](https://user-images.githubusercontent.com/78605779/199551823-206cd27a-1794-436d-9f0b-f5f002ed8182.png)
[출처)코딩애플](https://codingapple.com/course/git-and-github/)

만약 파일 여러 개를 한 번에 add 하고 싶다면 공백을 기준으로 파일명을 나열하면 된다.

```
git add 1.txt 2.txt
```

혹은 변경된 모든 파일을 add 하고 싶다면 add 뒤에 `.`을 넣어주면 된다.

```
git add .
```

만약 스테이징된 파일을 취소하고 싶다. 즉, 장전된 총알을 다시 빼고 싶다면 `restore` 명령어에 `--staged` 옵션을 사용하면 된다.

```
git restore --staged <fileName>
```

> 이렇게 직접 터미널에 명령어를 작성하는 것이 귀찮다면 IDE에서 제공하는 git 기능을 사용하거나 별로에 도구를 사용해도 된다.

### branch

branch는 독립적으로 작업을 진행하기 위한 개념으로 이를 통해 프로젝트를 여러 갈래로 나눠 여러 개발자가 동시에 다양한 작업을 할 수 있도록 해준다.

![image](https://user-images.githubusercontent.com/78605779/199557195-5311afea-e89a-4558-9734-39dd36c5f409.png)

위 그림을 보면 쉽게 이해가 될 것이다.

이제 실제로 실습을 하면서 살펴보자.

![스크린샷 2022-11-03 오전 2 19 08](https://user-images.githubusercontent.com/78605779/199557725-5352f388-bdd0-41dc-92cf-08fbd4b48f2d.png)

위 그림에 프로젝트를 이제 쇼핑몰 프로젝트라고 가정해보자.

그런데 우리는 새로운 쿠폰 기능을 개발해야된다. 그러면 어떻게 해야될까?

물론 바로 현재 프로젝트에 이어서 개발해도 된다. 하지만 현재 안정적으로 동작하는 프로젝트는 가만히 냅두면서 새로운 기능을 개발하고 여러가지 테스트를 위해서 branch를 사용할 수 있다.

이제 branch를 만들기 위해서는 간단하기 branch 명령어를 사용하면 된다.

```
git branch <branchName>
```

![스크린샷 2022-11-03 오전 2 26 31](https://user-images.githubusercontent.com/78605779/199559456-a347da4f-13ae-4d20-9893-a6b799689891.png)

feature/coupon이라는 이름으로 branch를 생성하였다. 현재 branch를 생성만 했을 뿐 이제 해당 branch로 이동하기 위해서서는 swich 명령어를 사용해야된다.

```
git switch <branchName>
```

![스크린샷 2022-11-03 오전 2 29 41](https://user-images.githubusercontent.com/78605779/199560186-969cb204-141e-48c5-bfd4-09f2b5ddebdb.png)

switch 명령어를 통해서 branch를 이동한 것을 확인할 수 있을 것이다. 만약 윈도우에서 vscode 등을 이용해 실습해 이동한 것이 확인이 안된다면 status 명령어를 통해서 현재 위치한 branch를 확인할 수 있다.

![스크린샷 2022-11-03 오전 2 31 48](https://user-images.githubusercontent.com/78605779/199560611-b20b1728-2b54-4bf2-a0f6-cebbb27b28c3.png)

이제 새롭게 생성한 branch에서 파일을 생성하고 커밋을 하면 아래와 같은 그림으로 표현할 수 있다.

![image](https://user-images.githubusercontent.com/78605779/199561415-3fd7fea1-ca22-4ac2-b17b-4ce776e10d09.png)

feature/coupon branch에서 작업한 후 다시 main branch로 돌아가보면 그동안 작업한 내용물이 사라지게 된다.

**feature/coupon**

![스크린샷 2022-11-03 오전 2 38 52](https://user-images.githubusercontent.com/78605779/199562043-8872ed3e-ae61-4b5d-9286-804ae3e49354.png)

**main**

![스크린샷 2022-11-03 오전 2 39 44](https://user-images.githubusercontent.com/78605779/199562173-ccbf1bd8-81c7-44bd-961d-69ff094e7714.png)

이 처럼 작업을 독립적으로 개발이 가능하기 때문에 프로젝트를 안정적으로 관리할 수 있다.

그러면 다음으로 각 브랜치에서 파일을 추가하거나 수정하는 등을 통해 아래의 그림처럼 만들어보자

![image](https://user-images.githubusercontent.com/78605779/199563003-be890298-dd7d-4659-8e4a-2769b94c8f9b.png)

그리고 log 명령어에 graph 옵션을 추가해서 사용하면 아래와 같이 commit 기록을 확인할 수 있을 것이다.

```
git log --oneline --all --graph
```

![스크린샷 2022-11-03 오전 2 46 02](https://user-images.githubusercontent.com/78605779/199563417-e810b113-889b-4a37-bad4-9c55279659a2.png)

이제 쿠폰 기능 개발이 잘 끝나 기존에 프로젝트에 합칠 시간이다. 이렇게 branch를 합치는 것을 merge라고 한다.

merge를 하기 위해서는 우선 기준이 될 branch로 이동해야된다. 우리는 기존 프로젝트에 쿠폰 기능을 추가할 것이므로 main brach로 이동을 하자.

```
git switch main
```

### branch 합치기

merge 명령어로 합칠 branch를 지정해주면 된다.

```
git merge <branchName>
```

![스크린샷 2022-11-03 오전 2 53 08](https://user-images.githubusercontent.com/78605779/199564796-9ec3eca2-7a91-4c22-acb3-49e7b89c5eb1.png)

성공적으로 merge가 되면 현재 main branch에 위치해있어도 기존에 feature/coupon branch에서 작성한 것들을 확인할 수 있을 것이다.

![image](https://user-images.githubusercontent.com/78605779/199565076-cfc1a505-7723-47e0-a30b-98c2248afaed.png)

<h2>그러나</h2>

이렇게 항상 성공할 수 있는 것이 아니다.

아래 그림처럼 만약 main과 feature/coupon 둘 다 동일한 파일에 동일한 곳을 수정했다면 어떻게 될까?

![image](https://user-images.githubusercontent.com/78605779/199565494-580d179b-c50a-44f0-b500-8597f1fb69db.png)

위 그림의 상황을 재연하기 위해 각각의 branch에서 동일한 부분을 수정하고 merge를 진행해보자

![스크린샷 2022-11-03 오전 3 01 00](https://user-images.githubusercontent.com/78605779/199566689-25b2cf2a-91a1-4bdb-a63f-956c9bafa3dd.png)

그럼 위 그림과 같이 에러가 발생하면서 충돌이 발생한 부분을 우리가 직접 수정해줘야된다.

이를 해결하기 위해서는 충돌이 발생한 부분은 원하는 코드만 남긴 후 저장한 다음 다시 add와 commit을 수행해주면 된다.

![스크린샷 2022-11-03 오전 3 03 46](https://user-images.githubusercontent.com/78605779/199567134-9a4a7db3-54d2-41eb-84f8-df7abf14eeef.png)

![image](https://user-images.githubusercontent.com/78605779/199566015-528e9afe-bd5a-4b45-a6ad-cda663aa30de.png)

merge를 완료한 후 다시 log를 찍어보면 잘 branch가 잘 합쳐진 모습을 확인할 수 있다.

![스크린샷 2022-11-03 오전 3 06 34](https://user-images.githubusercontent.com/78605779/199567682-a6a75f14-041e-409e-94e6-b0e579e7d894.png)


### 참고 자료
[코딩 애플](https://codingapple.com/course/git-and-github/)

[git - 간편 안내서](https://rogerdudler.github.io/git-guide/index.ko.html)

[우아한테크코스 - 프로코스 제출 가이드](https://github.com/woowacourse/woowacourse-docs/tree/main/precourse)