---
layout: article
title: "[git rebase] 과거 커밋했던 내용 수정하기"
subtitle: "+rebase로 인한 committer date 변경하기"
date: 2020-05-01 23:20:00 +0900
lastmod: 2020-11-24 17:00:00 +0900
tags: 
    - git
    - git rebase
---

<br>

아래와 같은 방법을 이용하게 되면 과거에 잘못 커밋했던 중요한 과거 개인정보들을 수정할수 있게된다.

<br>

---

# 1. 커밋 해시 확인

수정하고 싶은 커밋의 바로 직전의 커밋 해시를 복사한다.

<br>

---

# 2. `git rebase --interactive`

`git rebase --interactive 커밋 해시`를 입력한다.

> 예시

```
git rebase --interactive 896fd2887bda7eb9533b3bf5144b321aceea5e5c
```

<br>

만약 첫 커밋을 수정하고 싶다면 `--root`를 입력해준다.

```
git rebase --interactive --root
```

<br>

# 3. `pick` => `edit` 수정

이후 vi 에디터가 실행되게 되는데, `i`를 눌러 입력모드로 전환하고, 가장 최상단줄의 pick을 edit로 바꾸고 `esc` -> `:wq`를 입력해서 저장하고 빠져나온다.

![image](https://user-images.githubusercontent.com/59393359/81479254-234bb780-925d-11ea-84ca-7bb64850e1f3.png){:.border.rounded}

<br>

---

# 4. 변경하고 싶은 파일 수정

작업 디렉토리에 들어가서 잘못 커밋한 내용들을 바로잡는다.

<br>

---

# 5. 다시 커밋하기

`git status`로 변경사항을 한번 확인해주고, `git add`를 사용하여 변경사항들을 추가해준다.

```
git status
git add .
```

<br>

이후 `git rebase --continue`를 입력하면 커밋메세지를 수정할 수 있는 에디터가 뜨는데, `esc` -> `:wq`를 입력해서 빠져나오면 수정한 커밋부터 현재의 커밋까지 수정된 내용이 적용된다. *(커밋해시값 전부 바뀜)*

```
git rebase --continue
```

<br>

하지만 리베이스 도중에 충돌이 나게되면 에러가 뜨면서 멈추게 되는데, 이때 상태를 파악하면서 `git add` 또는 `git rm` 을 이용하여 충돌을 해결한뒤 다시 리베이스를 진행하면 된다.

```
git status
git add 파일1.txt
git rm 파일2.txt
git rebase --continue
```

<br>

---

# 6. `git push`

이제 리베이스를 성공했다면, 깃허브에 푸시하게 되는데, 이때 원격 저장소와 다른 내용이 들어가 있게 되어 충돌이 일어나게된다.

이럴때는 `+` 를 덧붙이게 되면 강제로 로컬에 있는 자료로 덮어 씌우게 된다.

```
git push origin +master
```

<br>

---

# 7. `GIT_COMMITER_DATE` 수정하기

`rebase`를 끝마치고 깃허브에서 커밋 히스토리를 조회하게 되면, 아래 사진과 같이 커밋 날짜별로 분류가 되는게 아니라 당일 날짜로 분류가 되게 된다. *(뿐만 아니라 과거 시점에 커밋을 하는 것도 마찬가지로 COMMITER_DATE는 현재가 된다.)*

![image](https://user-images.githubusercontent.com/59393359/81479134-825cfc80-925c-11ea-9bd1-2a5b73ef88ee.png){:.border.rounded}

<br>

이때 아래 코드를 git bash에 입력하고 다시 푸시하면, `GIT_AUTHOR_DATE` 기준으로 `GIT_COMMITER_DATE`가 수정되게 된다. *(주의: 커밋해시가 전부 변경되게됨)*

```
git filter-branch -f --env-filter \
    'if [ "$GIT_AUTHOR_DATE" != "$GIT_COMMITTER_DATE" ]
     then
         export GIT_COMMITTER_DATE="$GIT_AUTHOR_DATE"
     fi'
```

![image](https://user-images.githubusercontent.com/59393359/81481274-c9052380-9269-11ea-828c-c15f4951dfa6.png){:.border.rounded}

<br><br><br><br>