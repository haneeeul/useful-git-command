## 목차
* [git basic](#git-basic)
* [자주 사용하는 git command & 문제 해결 방법](#자주-사용하는-git-command-&-문제-해결-방법)

<br><br>
git basic
===================================

    git remote add [다른 이름] [repository url] // repo url에 다른 이름을 붙이는 것
    git checkout -b [branch name] // 브랜치를 만들고 그 브랜치로 이동
    git remote // 현재 romote 종류 확인 가능
    git branch // 현재 브랜치들 목록 확인
    git push origin master // 현재 브랜치인 master에서 origin으로 push 한다
    cat .git/config // 현재 레포지토리 url 확인하기 등등 깃 관리를 위한 메타데이터가 저장되어 있음


### <우리 팀의 프로젝트를 깃으로 관리하기>
1. 팀명으로 프로젝트 생성
2. fork로 각 팀원들이 프로젝트 가져오기
3. 자신의 브런치 생성 후에 커밋
4. pull request로 합치기!!

> <git과 github의 차이는?>
> 
> git은 컴퓨터 local에 설치되어 있는 소스코드 관리 프로그램(오프라인)
> 
> github는 remote 저장소가 있는 외부 서버를 지칭한다.(온라인)
> 
> <commit과 push의 차이는?>
> 
> commit은 local 작업 폴더에 history를 쌓는 것이어서 인터넷을 사용하지 않고
> 
> push는 remote 저장소에 history를 쌓는 것이어서 인터넷을 사용한다.
> 
> <fetch와 pull의 차이는?>
> 
> 두 명령 모두 remote 저장소에서 최신 commit 정보들을 가져오지만
> fetch는 가져와서 임시폴더(.git)에 저장하고
> 
> pull은 바로 현재 branch에 merge 한다
> 
> <rebase와 merge의 차이는?>
> 
> 두 명령 모두 두개의 branch의 차이점 commit을 합치지만
> rebase는 합치기 전에 되감기(rewinding)를 하고
> 
> merge는 그냥 합친다

<br><br>
자주 사용하는 git command & 문제 해결 방법
===================================

## add / commit / push 취소하기
- HEAD : 항상 작업트리의 가자 최근 커밋을 가리킨다.

#### add 취소
  git status
- 파일 상태 확인
  git reset HEAD [filename]
- 파일 이름을 명시하지 않으면 staging area에 존재하는 모든 것이 unstaged 된다


#### commit 취소
  git log // or git shortlog
- 커밋 목록 확인
  git reset [option] HEAD[^ / ~{지울 커밋 개수}]
> option 종류
> 1. --soft : commit은 취소하고 해당 파일들은 staged 상태로 working directory에 보존
> 2. --mixed : commit은 취소하고 해당 파일들은 unstaged 상태로 working directory에 보존
> 3. --hard : commit은 취소하고 해당 파일들은 unstaged 상채로 working directory에서 삭제
> 
> 예시
> 
>     git reset --mixed HEAD^ // default
>     git reset HEAD^ // 위와 동일
>     git reset HEAD~1 // 마지막 1개의 commit 취소. 위와 동일
> 
>>
>>     git reset HEAD test.c
>>     
>> 는 이제 이렇게도 사용 가능하다.
>> 
>>     git restore --staged test.c
>> 
>>
만약 현재 디렉토리 (working directory)를 원격 저장소의 마지막 commit 상태로 되돌리고 싶다면?

    git reset --hard HEAD // 마지막 commit 이후로 추가한 파일들은 사라진다.


#### commit message 변경
  git commit --amend


#### push 취소 (사실상 새로 덮어쓰기: 아주 주의할 것. 동기화 문제가 생길 가능성이 매우 높다)
    
    git log -g // commit list 확인
    git reset HEAD@{number} or git reset [commit id] // 원하는 시점으로 working directory를 되돌린다.
    git commit -m "push force" // 원하는 상태에서 commit
    git push origin [branch name] -f or git push origin +[branch name] // 강제로 덮어쓰기
    
    
#### git clean
  git이 추적 중이지 않은 파일들 지운다. 
  그러나, .gitignore 에 명시되어 있는 파일은 지우지 않는다.
  
    git clean -f // delete files excepting directory
    git clean -f -d // delete files including directory
    git clean -f -d -x // .gitignore 에 명시된 파일들도 모두 삭제
    // -n option : 가상으로 실행해보고, 어떤 파일들이 지워질지 알려주는 옵션★
    
    
    
## fork repo와 origin repo 동기화하는 방법

1. 원본 저장소 등록하기
    
       git remote -v // 등록된 저장소 확인
       git remote add upstream [원격 저장소.git] // 원격 저장소를 upstream 으로 등록
    
2. 원본 저장소 변경분 로컬로 가져오기

       git fetch upstream [파일이 저장된 원본 저장소의 브랜치 이름] // 여기서는 main 이라 하겠음
     
3. 로컬에서 원본 저장소와 포크한 저장소 병합하기

       git checkout haneul // 포크한 저장소의 로컬 브랜치로 이동
       git merge upstream/main // 현재 저장소에(origin/haneul) 원본 브랜치를(upstream/main) merge
    
4. 포크한 저장소를 원격 git에 업데이트하기

       git push origin haneul
  
  
## git rm

    git rm [file] // file을 현재 working directory 에서도, 원격 저장소에서도 지운다.
    git rm --cached [file] // file을 원격 저장소에서만 지운다.
    
    git commit -m "delete file"
    git push origin main
    
    
## git restore(git version >= 2.23)
작업 트리에서 수정한 파일을 되돌리는 명령어

    git restore test.c // git checkout --test.c 와 같다.

### git checkout
1. 브랜치 변경 시에 사용
2. __작업 트리에서 수정한 파일을 되돌릴 때 사용__
