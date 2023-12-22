# TIL-2023.12.22

## Git, GitHub

얄팍한 코딩 사전 Git, GitHub 강의
https://www.youtube.com/watch?v=1I3hMwQU6GU

CLI 방식, GUI 방식 (확인 편함)

### Commit

1. git init
 해당 디렉토리를 git이 관리하도록 지정, 디렉토리 안에 .git 이라는 폴더 생성 됨

1. git status
해당 디렉토리의 git 상태 표시
- changes to be commit: 커밋 목록에 들어가있음, deleted, modified 로 해당 파일의 변경사항 표시
- untracked files: 커밋 목록에 포함 안되어있음

git add 파일명 : 해당 파일 커밋 목록에 추가 (가끔 다른 버전들에 따로따로 업데이트 해주거나 할 때 사용)
git add . : 디렉토리 안의 전체 파일 커밋 목록에 추가
git diff: 해당 디렉토리의 변경 사항 자세히 표시

1. .gitignore 파일에 해당 파일명 입력
깃에 저장하지 않을 되는 부속 파일, 보안상 중요한 파일들 제외
- .gitignore 안의 형식으로 지정 가능
    // 모든 file.c
    file.c
    
    // 최상위 폴더의 file.c
    /file.c
    
    // 모든 .c 확장자 파일
    *.c
    
    // .c 확장자지만 무시하지 않을 파일
    !not_ignore_this.c
    
    // logs란 이름의 파일 또는 폴더와 그 내용들
    logs
    
    // logs란 이름의 폴더와 그 내용들
    logs
    
    // logs 폴더 바로 안의 debug.log와 .c 파일들
    logs/debug.log
    logs/*.c
    
    // logs 폴더 바로 안, 또는 그 안의 다른 폴더 안의 debug.log
    logs/**/debug.log
    

1. git commit -m “FIRST COMMIT”
git commit → vi 에디터로 열림, 첫줄에 설명 명시하고 저장하면 그대로 커밋됨
이 과정을 한번에 해주는 명령어
git commit -am “(메세지)” : 새로 추가된 파일이 없을 경우 add와 커밋을 한번에 처리

1. git log
커밋 버전들 조회할 수 있음
사용자 정보, 커밋 내역, 일자, 해쉬 확인 가능

1. Reset vs Revert
- Reset: 해당 커밋으로 돌아가고 이후의 커밋들은 전부 삭제함
  
git reset —hard (해당 커밋의 해쉬) → 해당 커밋으로 이동하고 이후 커밋들 삭제
해당 커밋의 해쉬 입력 안하면 현재 위치한 커밋 정보로 reset

- Revert: 해당 커밋의 작업을 되돌리는 커밋을 하나 생성해서 해당 커밋만 취소하고 이후 커밋들은 유지함
git revert (해쉬) → 해당 해쉬 커밋을 되돌리는 커밋을 하나 생성해서 해당 커밋 취소
만약 해당 커밋이 현재 상태와 충돌을 일으킨다면 git add나 git rm으로 깃에서 파일 조정후 git revert —continue 로 수행
revert 도 결국 커밋을 만들어서 처리하는 것이라 똑같이 reset revert 써서 되돌리기 가능
revert시 커밋까지 하지 않고 작업만 되돌리기 → git revert —no-commit (해쉬) → 다른 수정까지 더해서 커밋하고싶을 때

** GUI (SourceTree) 로 commit, reset, revert
commit: 스테이지에 올릴 변경사항 체크, 커밋
reset: 해당 커밋에 우클릭 후 main을 이 커밋으로 초기화 선택
revert: 해당 커밋에 우클릭 후 커밋 되돌리기 선택

---

### Branch

줄기(분기) 만들기, 각기 다른 기능을 분기를 생성해서 개발하다가 메인에 병합, 실험적인 시도는 다른 브랜치를 만들어서 시도 해볼 수 있음

1. branch 생성 확인 이동
git branch (이름), git branch, git switch (이름)
git switch -c (이름): 브랜치 생성 이동 동시에
git branch -d (이름): 브랜치 삭제 (대문자 D는 강제 삭제)
git branch -m (이름) (이름): 브랜치 이름 바꾸기
git log —all —decorate —oneline —graph: git log에서 모든 브랜치 확인 가능

1. Merge vs Rebase
- Merge: 브랜치의 히스토리를 남기고 합침, 합치는 것도 하나의 커밋으로 생성됨 → reset으로 취소 가능
    git switch main
    git merge add-coach
    git branch -d add-coach
    **main으로 이동해서 브랜치를 merge
    
- Rebase: 히스토리를 남기지 않고 한줄로 합침
    
    git switch new-team
    git rebase main
    main이 new-teams에 비해 뒤쳐져있기 때문에 merge로 당겨와야함
    *git switch main
    *git merge new-teams
    git branch -d new-teams
    **브랜치로 이동해서 main에 rebase
  
    협업시에는 웬만해선 merge 사용, 깃허브에서 pull하는 경우에서나 rebase 사용함
    합병한 브랜치는 그때그때 삭제 권장
    
1. Merge 충돌
합치는 두 브랜치의 같은 줄이 다를 때 깃에서 어떤걸 선택해야하는지 모르기 때문에 사용자가 지정 해줘야함 >>>>>로 검색 가능
충돌이 발생하면 충돌 해결 해주고 커밋 한번 더 해주면 됨
만약 충돌이 너무 많아 해결이 어려우면 merge 취소 하는게 나음
git merge —abort

1. Rebase 충돌
Merge는 맨 끝 커밋을 연결해서 붙여주는 것이라 충돌이 한번 발생하지만
Rebase는 브랜치 안의 커밋들을 다 옮겨 붙여주는 것이기 때문에 충돌이 커밋 수만큼 발생
충돌 해결 → git add . 로 변경 사항 스테이지에 올리고 → git rebase —continue 로 남은 rebase도 확인 → 모든 커밋 충돌 해결 할 때 까지 반복후 rebase 마무리 과정

** GUI (SourceTree)로 Merge, Rebase 똑같음

---

### GitHub

1. Push (업로드)
git remote add origin https://github.com/kimUIhyun/git-practice.git
git branch -M main
git push -u origin main

- git remote add origin (원격 저장소 주소)
로컬의 git 저장소에 원격 저장소로의 연결 추가 (경로 설정)
origin : 원격 저장소의 이름

- git branch -M main
기본 브랜치 명을 main으로 설정 (권장)

- git push -u origin main
로컬 git의 현재 브랜치의 커밋 내역들을 origin의 main브랜치로 푸쉬 (업로드)
—> 이렇게 설정이 다 되어있으면 git push 만으로 푸쉬 가능

- git remote
현재 프로젝트가 연결된 원격 저장소 확인

- git remote -v
세부 정보 링크 확인

1. Pull (깃허브 최신 커밋 불러오기)
git pull


**깃허브에서 업데이트 된 내용도 있고 내가 새로 커밋해서 푸쉬할 내용도 있는 경우**

- git pull —no-rebase : merge 방식
로컬의 메인 브랜치와 원격의 메인 브랜치를 별도로 보고 분기한 다음 merge 로 합침

- git pull —rebase : rebase 방식
로컬과 원격의 커밋을 선형적인 시간축으로 보고 원격의 커밋을 현재에 붙이고 로컬의 커밋을 그 뒤에 붙임

이중 하나의 방식으로 커밋을 맞추고 git push 해주면 됨
여기서도 충돌이 발생 할 수 있는데 앞서 merge, rebase에서 충돌 해결해주는 방식과 똑같이 해결해주면 됨

- git push —force
원격의 상태를 로컬로 강제 push, 협업시에는 특별한 경우 아니면 사용 안함

1. Clone (레포지토리 다운로드)
터미널에서 로컬 디렉토리 들어간 다음 git clone (원격 저장소 주소)
*Download ZIP은 깃 관리 내역 제외하고 파일만 다운받음

1. 원격 저장소의 branch 관리
- git push -u origin from-local
현재 브랜치가 from-local 일 때 from-local 이라는 로컬 브랜치를 origin에 푸쉬 하려고할 때 원격 저장소의 브랜치를 지정 해줘야함

- git fetch
- git switch -t origin/from-remote
origin에 있는 from-remote 브랜치를 로컬로 가져와서 생성

- git push (원격 이름) —delete (원격의 브랜치)
해당 원격의 브랜치를 삭제

- git branch —all
로컬 브랜치 뿐만 아니라 원격 브랜치 까지 확인 가능
