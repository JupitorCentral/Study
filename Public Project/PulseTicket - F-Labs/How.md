	
### 하나의 repository 에서 여러개의 브랜치를 개발하는 법

-> git worktree
-> vs code 에서는 native 하게 지원, intelliJ 는 플러그인 설치 필요



### F-Labs Code Review 받는 법

pull request reviewer : f-lab-bot, `if-else-f`

1. GitHub 프로젝트 Issues 탭에서 **New Issue** 클릭하여 이슈 생성

2. 이슈 설명 작성 후 오른쪽 사이드바의 **Assignees**에서 본인 선택

3. **Create Issue** 버튼 클릭 (이슈 번호 자동 부여)

4. 이슈 상세 페이지의 Development 섹션에서 **Create a branch** 클릭

5. 브랜치명 확인 후 (브랜치명에서, 브랜치이름/이슈 번호 이렇게 해야 함 (슬래쉬 뒤가 태그가 됨)) **Create branch** 버튼 클릭 (이슈와 링크되어야함)

6. 로컬에서 `git fetch` 후 `git checkout <브랜치명>` 실행

7. 코드 작성 및 커밋 (커밋 메시지에 `#이슈번호` 포함)

8. `git push` 로 원격 저장소에 업로드

9. GitHub에서 **Pull Request** 생성

10. PR 오른쪽 사이드바의 **Assignees**에서 본인 선택

11. PR 오른쪽 사이드바의 **Reviewers**에서 `f-labs-bot` 과 `if-else-f` 추가

12. **Create pull request** 버튼 클릭

13. 멘토님의 리뷰 대기 및 리뷰 후 Merge


### git worktree, git switch, git stash

git stash 는 지금 변경사항을 완벽히 저장하지는 않는데 
(변경된 모든 파일을.)

차라리 이럴바엔 git worktree 를 써서 branch 자체를 로컬에 복사해서 쓰는게 좋아보이긴 하는데.
일단 이번 issue 완성하고 git worktree 를 써보자.

git switch 는 뭔가 맘에 안든다 intelliJ 의 shelve 기능이 맘에든다.

그리고 branch 이름 길때 짜증나는데
뭐 이걸 위해서 

git config --global alias.sw switch 
git config --global alias.cache 'switch feature/cache-stampede'

이런게 있다고 하는데 나중에 해보자



