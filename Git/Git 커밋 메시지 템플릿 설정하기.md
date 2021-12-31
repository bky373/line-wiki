# 1. Git Config

* Git은 **내장된 기본 규칙을 따르지만, 설정된 것이 있으면 그에 따른다.**

## --system
* Git은 먼저 `/etc/gitconfig` 파일을 찾는다. 이 파일은 해당 **시스템**에 있는 모든 사용자와 모든 저장소에 적용되는 설정 파일이다.
* `git config` 명령에 `--system` 옵션을 주면 이 파일을 사용한다.

## --global
* 다음으로 Git은 `~/.gitconfig` 파일을 찾는다. 이 파일은 해당 **사용자**에게만 적용되는 설정 파일이다. **`--global` 옵션을 주면 Git은 이 파일을 사용한다.**

## --local
* 마지막으로 현재 작업 중인 저장소의 Git 디렉토리에 있는 `.git/config` 파일을 찾는다. 이 파일은 해당 **저장소**에만 적용된다. `git config` 명령에 `--local` 옵션을 적용한 것과 같다. (아무런 범위 옵션을 지정하지 않으면 Git은 기본적으로 `--local` 옵션을 적용한다)
* 각 설정 파일에 중복된 설정이 있으면 설명한 “순서대로” 덮어쓴다. 예를 들어 `.git/config` 와 `/etc/gitconfig` 에 같은 설정이 들어 있다면 `.git/config` 에 있는 설정을 사용한다.

# 2. 커밋 메시지 템플릿 설정
* 커밋 시 Git은 `commit.template` 옵션에 설정한 템플릿 파일을 보여준다.
* 커밋 메시지 템플릿을 지정하면 커밋 메시지를 작성할 때 **일정한 스타일**을 유지할 수 있다.

## 템플릿 파일 생성하기
* `.git`이 있는 현재 폴더 위치에서 아래 명령을 실행해 `.gitmessage.txt` 파일을 생성한다.
> touch  .gitmessage.txt
* 명령 실행 전,
  
  ![](https://images.velog.io/images/bky373/post/cebfa9b0-196a-497c-9a1f-4a6bd06e1157/image.png)
* 명령 실행 후, `.gitmessage.txt` 파일이 생성된 것을 볼 수 있다.
  
  ![](https://images.velog.io/images/bky373/post/8e260eff-dc0f-4802-b873-b37659c2e270/image.png)


## 템플릿 파일 작성하기
* 아래 두 가지 방법 중 하나를 선택해 템플릿 파일에 내용을 작성할 수 있다.

### 방법 1: txt 파일 안에 내용 직접 작성하기
* 방금 생성한 `.gitmessage.txt` 파일을 클릭한 다음, [아래 내용](#커밋-메시지-템플릿)을 복사 + 붙여넣기 한다.

### 방법 2: vim 이용하기
1. bash에서 아래 명령을 실행하여 편집기를 연다.
> vim .gitmessage.txt


2. `i` 키를 한 번 누르고, [아래 내용](#커밋-메시지-템플릿)을 복사 + 붙여넣기 한다.
3. `ESC`를 한 번 누르고 `:wq`  + `Enter`를 입력하여 편집기에서 나온다.

### 커밋 메시지 템플릿
* `#`로 시작하는 부분은 주석으로 커밋에 반영되지 않는다.
  (즉, 원격 저장소 커밋에 주석 내용이 보이지 않는다.)

```
################
# <타입> : <제목> 의 형식으로 제목을 아래 공백줄에 작성
# 제목은 50자 이내 / 변경사항이 "무엇"인지 명확히 작성 / 끝에 마침표 금지
# 예) feat : 로그인 기능 추가

# 바로 아래 공백은 지우지 마세요 (제목과 본문의 분리를 위함)

################
# 본문(구체적인 내용)을 아랫줄에 작성
# 여러 줄의 메시지를 작성할 땐 "-"로 구분 (한 줄은 72자 이내)

################
# 꼬릿말(footer)을 아랫줄에 작성 (현재 커밋과 관련된 이슈 번호 추가 등)
# 예) Close #7

################
# feat : 새로운 기능 추가
# fix : 버그 수정
# docs : 문서 수정
# test : 테스트 코드 추가
# refact : 코드 리팩토링
# style : 코드 의미에 영향을 주지 않는 변경사항
# chore : 빌드 부분 혹은 패키지 매니저 수정사항
################
```

## 템플릿 설정하기

* 템플릿 파일을 설정해놓으면, `git commit` 명령을 실행할 때 지정한 템플릿 메시지를 편집기에서 매번 사용할 수 있다.
* 템플릿 파일을 설정한다는 것은 `commit.template`에 `.gitmessage.txt` 파일을 등록한다는 의미다.
* 템플릿 파일을 설정하는 명령은 아래와 같다.
> git config --global commit.template .gitmessage.txt

## 커밋 메시지 작성하기
* 템플릿 설정을 마친 상태에서, `git add [파일명]` 명령을 실행해 변경사항이 있는 파일을 스테이지에 올린다.
* 다음으로 `git commit`을 실행해 `COMMIT_EDITMSG` 안에 템플릿 메시지가 들어있는지 확인한다.
  ![](https://images.velog.io/images/bky373/post/d9a0c9f6-324a-4407-b04b-34e3b93f98b8/image.png)
* 템플릿 메시지가 적용되었다면, `COMMIT_EDITMSG` 안에  **커밋 메시지의 제목과 본문, 꼬릿말**을 작성한다.
  (여기에서는 간단한 예시로 제목과 본문 두 줄을 작성하였다.)
  ![](https://images.velog.io/images/bky373/post/e388204e-6052-41f6-ba24-2caedeb9bc79/image.png)


* `COMMIT_EDITMSG` 창의 오른쪽 `X` 버튼을 눌러 창을 닫으면 아래 이미지처럼 커밋 메시지가 등록된다.
  (커서가 `COMMIT_EDITMSG` 창에 위치해 있다면 `Ctrl + w` 버튼으로 창을 닫아도 된다.)
  ![](https://images.velog.io/images/bky373/post/610a8246-b044-492c-bb1a-b2a9c1999cde/image.png)

* 작업이 끝났다면 `git push`를 통해 커밋을 원격 저장소에 올릴 수 있다.

## 참고 자료
* [Git 설정하기 * git-scm.com](https://git-scm.com/book/ko/v2/Git%EB%A7%9E%EC%B6%A4-Git-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)
* [좋은 커밋 메시지를 작성하기 위한 커밋 템플릿 만들어보기](https://junwoo45.github.io/2020-02-06-commit_template/)
* [[GitHub] git commit 템플릿 사용하여 commit message convention 준수하기](https://chanhuiseok.github.io/posts/git-4/)
