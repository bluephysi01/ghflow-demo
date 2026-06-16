# GitHub Flow 팀 개발 연습 🚀

깃허브 협업이 처음인 소규모 팀(5인 미만)이 **브랜치 → PR → 리뷰 → merge** 흐름을
직접 손으로 익히기 위한 연습용 저장소입니다.

완벽함보다 **"끝까지 한 사이클 굴려보는 성공 경험"**이 목표입니다.
처음엔 어색해도, 아래 순서대로 한 번 돌려보면 금방 익숙해집니다.

---

## 📌 우리 팀의 약속 (먼저 읽기)

이 연습은 **fork 없는 기본 GitHub Flow**로 진행합니다.

1. **`main`은 항상 작동하는 상태로 유지한다.** 깨진 코드는 `main`에 올리지 않는다.
2. **`main`에서 직접 작업하지 않는다.** 모든 작업은 **브랜치를 만들어서** 한다.
3. **작동하는 단위로 자주, 작게 merge한다.** 한 번에 몰아서 합치지 않는다.
4. **`main`에 합칠 땐 반드시 PR을 거친다.** 직접 push 금지.
5. **리뷰도 merge도 모두가 돌아가며 해본다.** 작업자·리뷰어·merge 담당을 번갈아 맡아 전 과정을 경험한다.

> 역할 정리 (연습이 목적이니 **모두가 모든 역할을 돌아가며** 해봅니다)
> - **작업자**: 브랜치에서 개발하고 PR을 올리는 사람 (= 모두)
> - **리뷰어**: 남이 올린 PR을 읽고 Approve 하는 사람 (= 작업자끼리 번갈아)
> - **merge 담당**: 승인된 PR을 최종 확인하고 merge 버튼을 누르는 사람 (= 모두가 돌아가며)
>
> 💡 한 PR에서 **작업자·리뷰어·merge 담당은 서로 다른 흐름으로 엮입니다.**
> 본인이 올린 PR은 본인이 Approve할 수 없으니(아래 6단계 참고),
> 자연스럽게 "내가 올린 건 남이 리뷰·merge, 남이 올린 건 내가 리뷰·merge" 식으로 역할이 순환됩니다.
> 이렇게 모두가 merge 버튼까지 눌러보며 전체 그림을 익히는 것이 이 연습의 핵심입니다.

---

## 🔰 처음 한 번만 하는 준비

### 저장소 관리자가 처음 한 번 하는 것
> 저장소를 만든 사람이 관리자가 됩니다. 아래는 **세팅을 위한 역할일 뿐**,
> 개발·리뷰·merge는 모두가 동등하게 참여합니다.

- [ ] 이 저장소 생성 (README, `.gitignore` 포함)
- [ ] 팀원을 **collaborator로 초대** (Settings → Collaborators)
  - 권한은 **Write** 이상으로 부여 → 모두가 브랜치 push와 **PR merge**를 할 수 있습니다.
  - Public 저장소여도 **push하려면 초대가 필요**합니다. 초대 없으면 clone은 되지만 push가 막힙니다.
- [ ] `main` 보호 설정 (Settings → Branches → Add rule)
  - "Require a pull request before merging" 체크 → `main` 직접 push 차단 (merge는 누구나 OK, 직접 push만 막음)
  - (선택) "Require 1 approval" → 리뷰 1명 승인 필수
  - ⚠️ "Restrict who can merge" 같은 **merge 권한 제한은 걸지 마세요.** 모두가 merge를 경험하는 게 목적입니다.
- [ ] merge 후 브랜치 자동 삭제 켜기 (Settings → General → "Automatically delete head branches")

### 팀원이 하는 것 (각자)
```bash
# 저장소를 내 컴퓨터로 받아오기
git clone https://github.com/<조직 또는 팀장계정>/<레포이름>.git
cd <레포이름>

# 잘 받아졌는지 확인
git remote -v        # origin이 팀 레포를 가리키면 OK
git branch -a        # main이 보이면 OK

# (선택) 원격에서 사라진 브랜치 기록을 자동 정리하도록 설정
git config --global fetch.prune true
```

---

## 🔁 개발 1 사이클 (이걸 반복합니다)

작업 하나를 할 때마다 아래 순서를 **처음부터 끝까지** 돕니다.

### 1단계 — 작업 시작 전, `main` 최신화

> 내가 작업하는 동안 다른 팀원이 먼저 merge했을 수 있습니다.
> **최신 `main` 위에서 시작해야 충돌이 줄어듭니다.**

```bash
git switch main
git pull origin main
```

### 2단계 — 작업용 브랜치 만들기

> 브랜치 이름은 **사람 이름이 아니라 "작업(기능) 이름"**으로 짓습니다.

```bash
git switch -c feature/기능이름
# 예) feature/login,  feature/perception,  fix/camera-bug
```

브랜치 이름 규칙 (권장):

| 접두어 | 용도 | 예시 |
|--------|------|------|
| `feature/` | 기능 개발 | `feature/login` |
| `fix/` | 버그 수정 | `fix/range-error` |
| `docs/` | 문서 작업 | `docs/readme` |

### 3단계 — 개발하고 커밋하기

> 커밋 메시지는 **prefix + 짧은 설명**으로. ("~함" 명사형으로 끝맺기)

```bash
# 작업...
git add .
git commit -m "feat: 로그인 기능 추가"
```

자주 쓰는 커밋 prefix:

| prefix | 의미 |
|--------|------|
| `feat:` | 기능 추가 |
| `fix:` | 버그 수정 |
| `docs:` | 문서 |
| `style:` | 포매팅 |
| `chore:` | 설정·잡일 |

### 4단계 — 내 브랜치를 원격에 push

```bash
git push -u origin feature/기능이름
# 두 번째 push부터는 git push 만 해도 됩니다
```

### 5단계 — GitHub에서 PR(Pull Request) 올리기

1. 저장소 페이지에 뜨는 **"Compare & pull request"** 클릭
2. 제목·내용 작성 (무엇을·왜 했는지)
3. 관련 이슈가 있으면 본문에 `close #이슈번호` 작성 → merge 시 이슈 자동 닫힘
4. 오른쪽 **Reviewers**에 리뷰해 줄 팀원 1명 지정
5. **"Create pull request"**

### 6단계 — 리뷰 (리뷰어가 하는 일)

1. PR의 **"Files changed"** 탭에서 코드 확인
2. GitHub이 자동으로 표시해 주는 충돌 여부 확인
   - ✅ "no conflicts" → 진행 가능
   - ❌ "Can't automatically merge" → 작업자가 충돌 해결해야 함 (아래 참고)
3. 문제 없으면 **Approve**, 고칠 게 있으면 **Request changes**로 의견 남기기

> ⚠️ 본인이 올린 PR은 본인이 Approve할 수 없습니다. 반드시 다른 팀원이 리뷰합니다.

### 7단계 — merge (리뷰 통과 후, merge 담당이 하는 일)

> 이 연습에서는 **모두가 돌아가며 merge를 해봅니다.** 보통 리뷰를 한 사람이나
> 작업자 외 다른 팀원이 맡습니다. 자기가 올린 PR이라도 리뷰만 통과하면 직접 merge해도 됩니다.

1. 리뷰 승인 + 충돌 없음(초록불) 확인
2. **"Merge pull request"** → 방식은 **Squash and merge** 권장 (커밋이 깔끔하게 1개로 정리됨)
3. merge 후 **"Delete branch"**로 원격 브랜치 정리 (자동 삭제 설정 시 생략)

> 💡 처음엔 merge 버튼 누르는 게 부담스러울 수 있는데, 그게 이 연습의 포인트입니다.
> "초록불(승인+충돌없음)을 확인하고 누른다"는 흐름을 모두가 직접 경험해 보세요.

### 8단계 — 작업 마무리 & 다음 준비

> ⚠️ **원격 브랜치는 자동으로 지워져도, 내 로컬 브랜치는 내가 직접 정리해야 합니다.**
> 준비 단계에서 켠 "Automatically delete head branches" 설정은 **깃허브(원격)의 브랜치만** 삭제합니다.
> 각자 컴퓨터(로컬)에 남은 브랜치까지는 건드리지 못하니, **PR을 올렸던 본인이 정리**합니다.

| 대상 | 누가 정리 | 방법 |
|------|----------|------|
| 원격 브랜치 | 깃허브 자동 | "auto delete" 설정 |
| 유령 기록 (`origin/...`) | 자동화 가능 | `git fetch --prune` |
| **로컬 브랜치** | **본인** | `git branch -d` |

```bash
git switch main
git pull origin main                 # merge된 최신 내용 받아오기
git branch -d feature/기능이름        # 다 쓴 로컬 브랜치 삭제 (← 본인이 직접)
git fetch --prune                    # 원격에서 사라진 브랜치의 유령 기록 정리
```

> 💡 준비 단계에서 `git config --global fetch.prune true`를 설정해 뒀다면,
> `pull`할 때 유령 기록이 자동 정리되어 손으로 할 일은 `git branch -d` 한 줄뿐입니다.

> 🔧 `git branch -d`가 "아직 merge 안 됐다"며 거부할 때가 있습니다.
> 특히 **Squash and merge**로 합치면 깃이 인식하지 못해 그럴 수 있어요.
> merge된 게 확실하면 대문자로 강제 삭제하세요: `git branch -D feature/기능이름`
> (작업 내용은 이미 `main`에 있으니 안전합니다.)

➡️ 이제 다음 작업은 다시 **1단계**부터 반복합니다.

> 로컬 브랜치를 깜빡 안 지워도 **작동엔 문제없습니다.** 목록이 지저분해질 뿐이니
> 부담 갖지 말고, "merge 확인 → 정리"를 습관처럼 묶어두면 깔끔합니다.

---

## ⚠️ 충돌(Conflict)이 났을 때 — Don't Panic!

> 충돌은 **두 사람이 같은 파일의 같은 부분을 다르게 고쳤을 때** 생깁니다.
> 실수가 아니라 협업에서 자연스럽게 생기는 일이니 당황하지 마세요.
> GitHub이 자동으로 감지해서 빨간색으로 알려줍니다.

**충돌은 보통 PR을 올린 작업자가 해결합니다.**

```bash
# 1) 내 브랜치에서 main 최신을 받아와 합쳐보기
git switch feature/기능이름
git pull origin main          # 여기서 충돌 발생

# 2) 충돌난 파일을 열면 아래처럼 표시되어 있음
#    <<<<<<< HEAD
#    내가 쓴 내용
#    =======
#    main에 있던 내용
#    >>>>>>> main
#
#    → 표시 기호(<<<, ===, >>>)를 지우고
#      최종 내용으로 직접 정리합니다.

# 3) 해결한 내용을 커밋하고 다시 push
git add .
git commit -m "fix: 충돌 해결"
git push origin feature/기능이름

# 4) GitHub PR이 다시 초록불(no conflicts)로 바뀌면 merge 가능
```

간단한 충돌은 GitHub PR 화면의 **"Resolve conflicts"** 버튼으로 브라우저에서 바로 고칠 수도 있습니다.

### 충돌 예방 습관
- 작업 **시작 전**에 항상 `git pull origin main`
- **작게 자주** merge하기 (오래 묵힌 브랜치일수록 충돌↑)
- 가능하면 **서로 다른 파일**을 담당해서 작업

---

## ✅ 자주 쓰는 명령어 모음

```bash
git status                  # 현재 상태 확인 (가장 자주 씀)
git branch                  # 로컬 브랜치 목록 (* = 현재 위치)
git switch <브랜치>          # 브랜치 이동
git switch -c <브랜치>       # 브랜치 생성 + 이동
git switch main             # main으로 복귀
git pull origin main        # 원격 main 최신 받기
git add .                   # 변경사항 전체 스테이징
git commit -m "메시지"       # 커밋
git push -u origin <브랜치>  # 첫 push (이후엔 git push)
git branch -d <브랜치>       # 로컬 브랜치 삭제
git branch -D <브랜치>       # 로컬 브랜치 강제 삭제 (squash merge 후 등)
git fetch --prune           # 원격 유령 브랜치 기록 정리
```

---

## 🧭 헷갈릴 때 기억할 것

- **로컬과 원격은 따로 논다.** 올릴 땐 `push`, 받을 땐 `pull`로 직접 주고받아야 한다.
- **`main`은 받아오기 전용.** 작업은 항상 브랜치에서.
- **막히면 일단 `git status`.** 지금 내가 어디서 무슨 상태인지 알려준다.
- **모르면 물어보기.** 혼자 main을 강제로 되돌리거나 `push -f` 하지 말 것!

---

## 🎯 연습 미션 (이 흐름으로 해보기)

작은 과제로 위 사이클을 직접 돌려봅니다. 한 사람이 한 브랜치씩 맡아보세요.

1. `feature/hello` — 간단한 인사 출력 파일 만들기
2. `feature/calculator` — 두 수를 더하는 함수 추가
3. `docs/usage` — 사용법을 README에 추가
4. (도전) 일부러 같은 파일을 두 명이 수정해서 **충돌을 만들고 해결**해보기

각 과제마다 **브랜치 생성 → 작업 → PR → 리뷰 → merge → 정리**의 한 사이클을 완주하는 것이 목표입니다. 🎉

> 🔄 **역할을 돌려가며 해보세요.** 한 과제에서 작업자였다면, 다음 과제에서는
> 다른 사람의 PR을 리뷰하고 merge 버튼을 눌러보는 식으로요.
> 모두가 **작업·리뷰·merge를 최소 한 번씩** 경험하는 것을 목표로 하면,
> 짧은 연습으로 깃허브 협업의 전체 그림을 손에 익힐 수 있습니다.
