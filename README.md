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
>

