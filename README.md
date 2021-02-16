자주 사용하는 git command & 문제 해결 방법
===================================

# add / commit / push 취소하기
### add 취소
  git status
- 파일 상태 확인
  git reset HEAD [filename]
- 파일 이름을 명시하지 않으면 staging area에 존재하는 모든 것이 unstaged 된다


### commit 취소
  git log // or git shortlog
- 커밋 목록 확인
  git reset [option] HEAD[^ / ~{지울 커밋 개수}]
> option 종류
