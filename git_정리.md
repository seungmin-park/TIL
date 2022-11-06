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
    - [다양한 merge](#다양한-merge)
      - [3-way merge](#3-way-merge)
      - [fast-forward merge](#fast-forward-merge)
    - [실수했을 때 대처 방법](#실수했을-때-대처-방법)
    - [Github](#github)
      - [원격 저장소에 올리기](#원격-저장소에-올리기)
      - [원격 저장소를 가져오기](#원격-저장소를-가져오기)
      - [branch로 협업하기](#branch로-협업하기)
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
[출처)코딩애플](https://codingapple.com/course/git-and-github/)

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
[출처)코딩애플](https://codingapple.com/course/git-and-github/)

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
[출처)코딩애플](https://codingapple.com/course/git-and-github/)

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
[출처)코딩애플](https://codingapple.com/course/git-and-github/)

<h2>그러나</h2>

이렇게 항상 성공할 수 있는 것이 아니다.

아래 그림처럼 만약 main과 feature/coupon 둘 다 동일한 파일에 동일한 곳을 수정했다면 어떻게 될까?

![image](https://user-images.githubusercontent.com/78605779/199565494-580d179b-c50a-44f0-b500-8597f1fb69db.png)
[출처)코딩애플](https://codingapple.com/course/git-and-github/)

위 그림의 상황을 재연하기 위해 각각의 branch에서 동일한 부분을 수정하고 merge를 진행해보자

![스크린샷 2022-11-03 오전 3 01 00](https://user-images.githubusercontent.com/78605779/199566689-25b2cf2a-91a1-4bdb-a63f-956c9bafa3dd.png)

그럼 위 그림과 같이 에러가 발생하면서 충돌이 발생한 부분을 우리가 직접 수정해줘야된다.

이를 해결하기 위해서는 충돌이 발생한 부분은 원하는 코드만 남긴 후 저장한 다음 다시 add와 commit을 수행해주면 된다.

![스크린샷 2022-11-03 오전 3 03 46](https://user-images.githubusercontent.com/78605779/199567134-9a4a7db3-54d2-41eb-84f8-df7abf14eeef.png)

![image](https://user-images.githubusercontent.com/78605779/199566015-528e9afe-bd5a-4b45-a6ad-cda663aa30de.png)
[출처)코딩애플](https://codingapple.com/course/git-and-github/)

merge를 완료한 후 다시 log를 찍어보면 잘 branch가 잘 합쳐진 모습을 확인할 수 있다.

![스크린샷 2022-11-03 오전 3 06 34](https://user-images.githubusercontent.com/78605779/199567682-a6a75f14-041e-409e-94e6-b0e579e7d894.png)

### 다양한 merge

merge 명령어들 통해서 branch를 합쳐봤는데 다양한 방법으로 branch를 합치는 것이 가능하다.

#### 3-way merge

첫 번째로 3-way merge는 우리가 저번에 사용한 merge 명령어로 브랜치를 합쳤을 때 일어난 merge 방식이 바로 3-way merge 방식이다.

![image](https://user-images.githubusercontent.com/78605779/200175113-1b66c9e1-f255-4d7d-81dc-e4926093c656.png)
[출처)코딩애플](https://codingapple.com/course/git-and-github/)

합치고 싶은 두 branch에 각각 새로운 commit이 존재할 때 새로운 commit을 하나 생성하면서 두 branch를 합쳐준다.

#### fast-forward merge

merge명령어를 사용했을 때 위 방식과 또 다르게 동작하는 경우가 있는데 그 경우가 기존 branch가 아무련 변동사항이 없을 경우이다.

예를 들어 일정 시점에 main branch에서 feature/abc라는 branch를 생성해서 여러 작업을 한 후 다시 main brach에 합칠려고 한다. 그런데 feature/abc branch를 생성한 시점부터 main branch에 아무런 변화가 없다면, 즉 새로운 commit이 하나도 없다면 merge 명령어를 통해 합칠 경우 fast-forward merge가 일어난다.

![image](https://user-images.githubusercontent.com/78605779/200175736-5da4c22b-f514-462a-a6c1-688f26fa0ee9.png)
[출처)코딩애플](https://codingapple.com/course/git-and-github/)

### 실수했을 때 대처 방법

여태껏 우리는 git을 통해서 작업 내용을 기록하면서 히스토리를 남겼다. 이제 우리가 열심히 쌓은 기록을 통해서 중간에 문제가 생겼을 경우 과거도 시간을 되돌려보자.

우선 타임머신을 타기 전에 스테이징된 파일을 취소하는 것부터 확인해보자. 우리가 사격장에서 총을 쏠려고 총알을 장전했다. 그러다 다행히도 쏘기 직전에 불발탄이라는 것을 확인했다. 얼른 장전된 총알을 제거해보자.

```
git restore --staged <fileName>
```

`git restore --staged a.txt`를 통해서 `a.txt` 총알을 무사히 제거했다.

다음으로 간단하게 하나의 파일을 작업하다가 뭔가 제대로 동작하지 않을 경우에 직전에 기록해둔 작업물로 되돌아가보자.

이 때 retore 명령어를 통해 파일명을 지정해주면된다.

```
git retore <fileName>
```

바로 직전이 아닌 더 과거도 돌아가는 것도 물론 가능하다. 이 때는 되돌리고 싶은 특정 시점을 가리키면 된다. 우리가 과거로 되돌아가고 싶을 때 언제 어느 시대로 시간을 되돌리고 싶다고 얘기하는 것과 동일하다.
git에서의 그 시점은 커밋 아이디가 되는 것이다.

![스크린샷 2022-11-04 오후 11 20 43](https://user-images.githubusercontent.com/78605779/199997961-1d469691-134c-4d6e-a3b2-7f2042d9cb6e.png)

`git log --oneline`을 통해서 우리의 기록을 살펴보면 커밋 메세지 옆에 이상하게 숫자와 영어가 조합된 문자를 확인할 수 있을 것이다. 이게 바로 우리가 돌아가고 싶은 날짜다.

이제 이 날짜를 다음과 같이 지정하면 된다.

```
git resore --source <commitId> <fileName>
```

위 상황들은 총을 쏘기 전에, 즉 commit 되기 전에 파일들을 수정했는데 commit을 되돌릴 수는 없을까? 물론 가능하다. 사실 완전히 commit을 삭제하는 것은 불가능하지만 해당 commit에서 수행한 작업들을 삭제하는 commit을 생성하는 것이 가능하다.

이러한 경우에 revert 명령어로 작업을 취소할 커밋 아이디를 지정해주면 된다.

```
git revert <commitId>
```

해당 명령어를 수행하면 에디터같은 곳에서 무언가 뜰 것이다.

![스크린샷 2022-11-04 오후 11 34 20](https://user-images.githubusercontent.com/78605779/200001028-fb8cc370-eff6-4698-a2e5-c1b499fd03fc.png)

각자 상황에 맞게 비슷한 화면이 뜰 텐데 맨 위가 바로 커밋 메세지가 된다. 위 메세지를 원하는 대로 바꾼후 창을 종료하면 정상적으로 해당 커밋의 작업이 취소되는 것을 확인할 수 있을 것이다.

![스크린샷 2022-11-04 오후 11 36 22](https://user-images.githubusercontent.com/78605779/200001455-7dd7aec9-9fed-4f89-aa27-276fe41042ae.png)

![스크린샷 2022-11-04 오후 11 37 12](https://user-images.githubusercontent.com/78605779/200001682-7a79a765-a8c8-430b-832f-59c55c91b131.png)

여러 커밋을 취소하고 싶다면 여러 커밋 아이디를 같이 적어주면 된다. 또는 가장 최근 커밋을 취소하고 싶다면 커밋 아이디 대신 HEAD를 적어줘도 된다.

이렇게 특정 작업을 취소하는 방법을 알아봤는데 사실 진짜 타임머신을 타는 것이 가능하다.

<span style='color:red; font-size:100px'>하지만</span> 그동안에 작업한 결과를 완전히 잃어버릴 수 있으니 주의하자.

![스크린샷 2022-11-04 오후 11 46 05](https://user-images.githubusercontent.com/78605779/200003706-38b96d00-0e51-4800-a770-2c28acb075e7.png)

위 그림에서 `0d908db`는 최초의 커밋이다. 그동안 작업물이 완전 망해서 처음부터 다시 쌓아 올리고 싶어졌다.

reset 명령어를 통해서 타임머신을 타고 과거로 돌아가자.

```
git reset --hard <commitId>
```

![스크린샷 2022-11-04 오후 11 48 07](https://user-images.githubusercontent.com/78605779/200004181-de7aea49-8e85-4d57-b1d9-177d34cb19b4.png)

정말 모든 것이 사라졌다. 그동안 작업한 파일들과 커밋이

커밋 HEAD도 처음을 가리키고 있다.

reset명령어에는 `--hard`말고 soft나 mixed 옵션이 존재하는데 이 두 옵션을 그동안에 기록을 완전히 지우지 않아 직접 찾아보고 사용해보길 바란다.

### Github

초반부에서 얘기했듯이 git과 github는 다른 것이다. 우리는 위 실습을 통해서 git의 기본적인 사용법을 익혔다. 그래서 새로운 프로젝트를 진행하면서 git 사용해 버전 관리를 열심히해서  굉장히 뿌듯한 상태가 되었다. 그런데 갑자기 컴퓨터가 고장나버렸다면? 컴퓨터를 초기화했다면? 이제 그 프로젝트와는 영원히 이별해야할 시간이다.

그래서 우리의 사랑스러운 프로젝트와 이별하지 않기 위해서 github와 같은 시스템을 사용해야되는 것이다.

이제부터 중간에 컴퓨터가 고장나더라도 프로젝트를 안전하게 관리하기 위해 온라인 저장소를 만들어보자.

![image](https://user-images.githubusercontent.com/78605779/199651878-f7a6455a-654a-4b02-b052-ac59a475d796.png)

위 그림처럼 github 사이트에 접속해 새로운 저장소를 만들어보자

![image](https://user-images.githubusercontent.com/78605779/199652027-7afc2f5a-0120-449b-bc39-aea725e21ab3.png)

그러면 다음과 같은 화면이 보일 것이다. 만약 보이지 않는다면 저장소를 만들 때 README 파일을 추가했거나 .gitignore파일이 생성되서 그럴 수 있다.

그러면 이제부터 이 원격 저장소와 우리 pc에 존재하는 local 저장소를 연결해보자

![스크린샷 2022-11-03 오후 2 20 12](https://user-images.githubusercontent.com/78605779/199652477-be7760ba-f64c-451e-9b6b-6a87a06e7658.png)

위 그림은 기존에 우리가 작업했던 로컬 저장소이다. 현재 branch를 확인해보면 main으로 되어있다.

만약 git 저장소를 생성했을 때 기본 branch가 main이 아닌 master일 수 있다. 그러면 다음과 같은 명령어로 branch 이름을 main으로 변경할 수 있다.

```
git branch -M main
```

[Master branch 이름을 main으로 바꾸는 이유](https://velog.io/@gwsyl22/git-Github-branch-%EC%9D%B4%EB%A6%84-main%EC%9D%98-%EC%A0%95%EC%B2%B4%EB%8A%94)

#### 원격 저장소에 올리기

원격 저장소도 만들었겠다. 이제부터 우리가 작업한 프로젝트를 원격 저장소에 안전하게 보관해보자.

원격 저장소에 올리는 명령어는 바로 `push` 이다. 아래와 같이 사용할 수 있다.

```
git push -u <원격 저장소 주소> <브랜치이름>
```

우선 branch 이름은 main으로 할 것이다. 그 동안 작업한 내용을 main에 merge 했기 때문에 가장 최신 버전에 내용을 한번 올려보자.

![image](https://user-images.githubusercontent.com/78605779/199653426-eb829c52-9bff-4e95-8c7f-33e6647a093d.png)

원격 저장소 주소는 위 그림에 나와있는 것을 직접 복사해서 쓰면 되는데 기본 규칙은 `https://github.com/${userName}/${repoName}.get`으로 위 url을 복사헤 마지막에 `.git` 을 붙여줘도 된다. 만약 처음에 생성했을 때 아래 그림과 비슷하게 생겼다면 우측에 code라고 적힌 초록색 버튼을 누르면 확인이 가능하다.

![image](https://user-images.githubusercontent.com/78605779/199653833-749f14f2-faf0-47cf-b06c-82e26b812980.png)

그러면 다음과 같이 우리가 작업한 내용이 잘 올라간 것을 확인할 수 있을 것이다.

![스크린샷 2022-11-03 오후 2 30 37](https://user-images.githubusercontent.com/78605779/199653405-d865208c-4825-45c3-97cb-b2076672bb23.png)

그런데 github에 push를 할 때마다 이렇게 긴 원격 저장소 주소를 작성하는 것이 꽤 번거롭다.

그래서 이 주소를 변수로 만들어서 저장해둘 수 있는데 이 때 사용하는 명령어가 `remote`이다.

```
git remote add <변수명> <주소>
```

이제 origin이라는 변수를 만들어 새롭게 push해보자.

우선 첫 번째로 우리의 원격 저장소 주소를 변수로 등록한다.

```
git remote add origin <주소>
```

그리고 여지껏 해왔던 것 처럼 파일을 수정하거나 추가한 후 add, commit 을 해준다.

마지막으로 우리가 만든 origin이라는 변수를 이용해 push를 하면된다.

```
git push origin main
```

![스크린샷 2022-11-03 오후 2 48 46](https://user-images.githubusercontent.com/78605779/199655190-fca3e6bc-253e-4ebe-81f3-bf94c493ea4a.png)

gitbub에 있는 원격 저장소에 들어가 확인하면 변경된 내용이 잘 반영되었을 것이다.

그런데 이번에는 push를 할 때 `-u`를 생략한 것을 확인할 수 있다. `-u`는 `--set-upstream`의 약어로 뒤에오는 내용들과 우리가 현재 위치한 로컬은 연결한 것이다.

![image](https://user-images.githubusercontent.com/78605779/199656719-2afdfc8f-2f4e-4d4c-a1b5-eb1f3030f552.png)

그래서 처음 push를 할 때 `-u`을 통해서 원격 저장소와 연결을 했을 경우 아래와 같이 간단하게 push도 가능하다.

```
git push
```

![스크린샷 2022-11-03 오후 3 07 28](https://user-images.githubusercontent.com/78605779/199657024-025abe9e-7efa-435b-9cd5-6f23c1ef8033.png)

#### 원격 저장소를 가져오기

원격 저장소에 올리는 방법을 알아봤으니 이제는 가져오는 방법을 알아보자.

간단하게 clone 명령어를 사용하면 되는데 다음과 같다.

```
git clone <원격 저장소 주소>
```

`git clone https://github.com/seungmin-park/git.git`를 통해서 원격 저장소를 그대로 가져올 수 있다. 이를 통해서 하나의 원격 저장소에서 함께 코드를 짜는 것이 가능해졌다.

그러나 내가 이 원격 저장소에 주인이 아니라면 push가 불가능 할 것이다. 이를 해결하기 위해서는 원격 저장소의 주인이 settings > collaborators > Add people로 사용자를 추가해줘야된다.

그러면 이제 함께 코딩을 하는 상황을 가정해보자.

![스크린샷 2022-11-03 오후 3 30 17](https://user-images.githubusercontent.com/78605779/199659480-37e8e326-a187-4a0e-9070-a62c976a21cb.png)

위 그림 처럼 열심히 작업을 했으니 이제 push를 해볼까?

![스크린샷 2022-11-03 오후 3 31 24](https://user-images.githubusercontent.com/78605779/199659604-4d0ba9e8-fe06-43ae-ab8c-3210e831ebe1.png)

push했더니 에러가 발생했다. 에러가 발생한 이유는 다른 사람이 먼저 원격 저장소에 push를 하여 변화가 생겼기 때문이다. github에 들어가 확인을 해보면 user2가 새로운 파일을 생성한 것을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/78605779/199659737-50d6ba46-d2e8-4a04-8936-9a2b9429f900.png)

이 문제를 해결하기 위해 에러 메세지에 밑에 부분을 보면 hint가 적혀있다. `git pull` 이라고

pull은 원격 저장소에 있는 내용을 로컬 저장소에 합치는 일을 해준다.

```
git pull <원격 저장소 주소> <브랜치 이름>
```

기존에 `-u` 옵션을 통해 upstream을 잘 설정해 두었으면 간단하게 push만 작성해도 된다.

그러면 이제 git pull 명령어를 통해서 로컬 저장소의 내용을 원격 저장소에 있는 내용과 합쳐보자.

![스크린샷 2022-11-03 오후 3 42 53](https://user-images.githubusercontent.com/78605779/199661036-84d49b95-21fb-4bbf-ab53-7afe2f30798d.png)

push 명령어를 통해서 user2가 작성한 파일을 local로 가져온 모습이다. 이제 로컬 저장소를 원격 저장소와 합쳐졌으니 다시 push를 하면 잘 작동될 것이다.

![image](https://user-images.githubusercontent.com/78605779/199661152-35a86664-70df-4bd3-93db-052695ed933f.png)

push 명령어는 fetch + merge를 수행해주는 것인데 fetch는 현재 로컬 저장소와 원격 저장소에 변경사항을 확인하는 것이다. 그래서 원격 저장소와 로컬 저장소에 차이점이 존재하는지 확인하고 이를 합치는 과정을 수행해 준다. 만약 여러 사용자가 동일한 부분을 수정했다면 merge할 때 충돌이 발생할 수 있는데 이는 기존에 merge에서 해결하는 것과 동일하게 해결하면 된다.

#### branch로 협업하기

github에 저장소를 올리고 내려받고 다른사람과 함께 코드를 작성하는 방법을 알아봤다.  이제는 원본 프로젝트를 안전하게 관리하며 [branch](#branch)를 통해서 협업을 해보자. github페이지에서 branch를 만들어도 되고 로컬에서 만들고 push해도 된다.

![스크린샷 2022-11-03 오후 4 11 49](https://user-images.githubusercontent.com/78605779/199664667-5dad280c-756e-4102-b1dd-fe7a3b145f4d.png)

feature/basket이라는 branch에서 새로운 기능을 추가하고 push까지 마쳤다.

github에 접속해서 확인해보니 제대로 생성된 것이 확인된다.

![image](https://user-images.githubusercontent.com/78605779/199664869-6883b175-2a21-45fc-b2df-b6cb4b71a633.png)

이렇게 새롭게 branch를 생성해서 작업한 장바구니가 잘 작동해 기존에 main branch에 합치고 싶어졌다면 local에서 merge를 해도 된다. 하지만 혼자 개발하는 것이 아닌 다른 사람과 함께 일한다면 코드를 먼저 검증해야한다. 그래서 보통 github 사이트에 접속해서 merge하는 경우가 많다. github사이트에서 원격 저장소에 접근해 Pull requests를 통해서 진행하면 된다.

`Pull Requests > new pull request > choice branch > crete new pull request`

![image](https://user-images.githubusercontent.com/78605779/199665846-ec575282-a7e1-41c5-aa43-4b1a996ad543.png)

![image](https://user-images.githubusercontent.com/78605779/199665868-a63e3efd-e3a9-47f4-8839-a88bfeb0e458.png)

![image](https://user-images.githubusercontent.com/78605779/199665914-65776620-6edd-41b0-b776-7a7c20cfcaba.png)

![image](https://user-images.githubusercontent.com/78605779/199666028-5a985132-e053-4169-ae96-facde70220de.png)

이렇게 새로운 branch를 merge할려고 할 때 두 가지 상황이 있을 수 있다.

- 첫 번째로 동일한 부분의 수정이 발생해 충돌이 발생했을 경우

    ![image](https://user-images.githubusercontent.com/78605779/199666591-7c279dd5-8b5a-4b35-b71f-0fe1469b40a6.png)

    우선 위 처럼 충돌이 발생한 경우 `Merge pull request` 버튼이 비활성화 돼있으며 `Resolve conflicts`이 존재하는 것을 확인할 수 있다. `Resolve conflicts` 버튼을 누르면 다음과 같은 창이 나오는데 기존에 충돌을 해결하듯이 해결하면 된다.

    ![image](https://user-images.githubusercontent.com/78605779/199667227-a8f928e9-44b0-4c4e-b491-eba1cc4ab049.png)

- 문제 없이 바로 merge가 가능한 경우

    ![image](https://user-images.githubusercontent.com/78605779/199666526-ad7a0536-0c9d-44a1-8c3c-d603352a9764.png)

    충돌이 발생해 충돌을 해결하거나 아무런 충돌이 발생하지 않았다면 위와 같은 화면을 볼 수 있을 것이다. 여기서 코드 리뷰를 하거나 코멘트들 달 수도 있다.

    ![image](https://user-images.githubusercontent.com/78605779/199667581-277ae650-d0b3-4254-83b9-57775b84a18a.png)

    그리고 `Merge pull request`을 통해서 branch를 merge할 수 있다.

    ![image](https://user-images.githubusercontent.com/78605779/199667667-7cb7f1ce-e57b-4094-821c-5a2f1a5aa741.png)

    그러면 아래 그림과 같이 merge가 성공한 것을 확인할 수 있다.

    ![image](https://user-images.githubusercontent.com/78605779/199667975-a071b51c-58dc-46fa-9d4a-5b38484b6aec.png)

### 참고 자료
[코딩 애플](https://codingapple.com/course/git-and-github/)

[git - 간편 안내서](https://rogerdudler.github.io/git-guide/index.ko.html)

[우아한테크코스 - 프로코스 제출 가이드](https://github.com/woowacourse/woowacourse-docs/tree/main/precourse)