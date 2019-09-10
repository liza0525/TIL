

# 20190910_Git branch

## git  branching

- https://learngitbranching.js.org/
  - git branch에 대해 공부할 수 있는 사이트

- rebase
  - commit branch가 복잡해지면 깔끔하게 정리해주는 명령어
  - [branch 이름]을 ~(주로 master)로 가라고 하는 그런 느낌
  - base를 새로 ~에 만든다.(rebase의 의미)

```git bash
# commit이 이루어진 후
git checkout [branch 이름]
git rebase master
# master로 기준점을 바꾼다.
```



## Forking

- 협업하는 방법 중 하나

- 계속 협업을 진행한다면, 

  - 대장의 fork를 새로 뜨는 방법

  - ```git bash
    git remote add upstream [대장의 repo 주소]
    git remote -v
    git merge upstream/master
    (conflict merge)
    ```

  - 바꾸고 내 것에 pull -> pull request



## 협업 시나리오

- Push & Pull
  - 동기적 처리를 해야하는 업무
  - 동시적 작업이 되지x
- Branching & Pull Request(PR)
  - 현실 협업 모델
- Fork & Pull
  - 오픈소스, 코드 contribution

### 리더와의 협업 시나리오

- 다른 branch로 설정 해주기
  - branch name pattern 정해주기(master)
  - require pull request reviews before merging 체크
  - 하위 리스트 중 Require review from Code Owners 체크
  - pull을 땡겨올 땐 꼭 master로 



## 기타

- 창업을 준비하다보면 많은 복잡한 것들을 해볼 수 있고, 이는 자소서에 쓸 글감이 되기 때문에 취업도 잘할 수 있음
  - 하나의 **응축된 경험**이라고 쓰는 게 매우 중요
  - 열거형? ㄴㄴ 좋지 않아
  - 그렇기 때문에 **경험**이 중요하다, SSAFY가 매우 중요한 경험이 될 수 있다. 이걸 잘 쓰도록 해보세요. 
  - 대기업 자소서는 IT 기업에 내면 떨어질 수도 있음, 많이 떨어져보고 많이 고민해봐야 하는 것임

- 프로그래머스 : github 주소에 있는 코드들을 알아서 돌리는 기능이 생길 것임, 그러므로 **프로젝트 정리를 github에 자세하고 깔끔하게 정리**해볼 것
  - 우리나라에서 혁신이 왜 안 일어나는가? 이력 중심의 채용 프로세스이기 때문에, 그래서 코드 중심의 평가를 하려고 프로그래머스에서 선도해서 도전 중임
- 어떤 서비스를 구축해봤는지 그것이 중요, 그러므로 꼭 플젝해서 정리해라 이말이양 :)ㅋㅋ

- SW 세상에서 무엇을 contribute할 것인지, 보여주고 도전해볼 것이 정말 많기 때문에 어떤 것이든 해볼 것, 도전해볼 것, 실패해볼 것(최적해를 구하려고 고민을 너무 많이 하는데 그냥 일단 해봐야 해)
- 시작의 중요한 점은 모방이므로 따라도 해보고, 그 중에서 나의 서비스를 만들어볼 것 ㅎㅎ
- **실력 중심의 산업**이 되어 가고 있음, 그러므로 장기전으로 생각하자
- **SW를 잘하는게 중요**하지, python django를 잘하는 게 중요한 건 아님, 하나의 수단일 뿐 이걸 spring이나 ruby로 바꿀 줄 아는 게 더 중요해

- https://blog.crisp.se/2016/01/25/henrikkniberg/making-sense-of-mvp

  sw는 bfs다...!