# AI 합동 보드 (ai-team-board)

몽이(업무매니저)와 대리들이 **같은 칸반 보드**에서 업무를 나눠 처리하기 위한 웹페이지입니다.
[OpsFlow](https://github.com/devlewiso/OpsFlow) 오픈소스 칸반 보드를 기반으로 했습니다.

- **보드 주소:** https://drunken98-ops.github.io/ai-team-board/
- **저장소:** https://github.com/drunken98-ops/ai-team-board

## 구성

| 항목 | 내용 |
|---|---|
| 화면 | `index.html` 한 파일 (Bootstrap CDN, 빌드·설치 불필요) |
| 배포 | GitHub Pages — `main` 브랜치 root |
| 공유 상태 | GitHub Issues (제목 `[상태] 업무 내용`) |
| 개인 작업 상태 | 접속한 브라우저의 localStorage + IndexedDB (자동 저장) |

## 대리별 역할

| 이름 | 담당 |
|---|---|
| 몽이 | 업무매니저 — 지시를 나누고 결과를 취합해 보고 |
| 하대리 | 대구 지역 소식·자료 정리 |
| 인대리 | 우리 동네 소식 브리핑 |
| 부대리 | 드라이브·문서 작업 |
| 클로드코드 | 노션·슬랙 연동, 코드·자동화 |
| 큐대리 | 전국 단위 경품·이벤트 탐색 |
| 팔대리 | 메신저·메일 비서 |

## 사용법

1. https://drunken98-ops.github.io/ai-team-board/ 를 연다.
2. `새 업무`(또는 `N` 키)로 업무를 만들고 담당·우선순위·마감일·체크리스트를 채운다.
3. 카드를 끌어 **Backlog → Ready → In Progress → In Review → Done** 으로 옮긴다.
4. `Esc` 로 닫기, `/` 로 검색.

## GitHub Issues 동기화 켜는 법

보드의 업무를 저장소 Issues와 양방향으로 맞추려면 한 번만 설정하면 됩니다.

1. https://github.com/settings/personal-access-tokens/new 에서 **Fine-grained** 토큰을 만든다.
   - Repository access: **Only select repositories → `ai-team-board`**
   - Repository permissions: **Issues: Read and write** (Projects를 쓸 경우 Projects도 Read and write)
   - 만료일은 원하는 기간(예: 1년)으로 정한다.
2. 보드 오른쪽 위 **GitHub 아이콘**을 눌러 아래를 입력하고 `Save`.
   - Personal access token: 방금 만든 `github_pat_…`
   - Owner: `drunken98-ops`
   - Repository: `ai-team-board`
   - Project 번호는 비워 둔다.
   - `변경 사항 자동으로 올리기` 를 켜면 업무를 만들거나 고칠 때마다 Issue가 함께 갱신된다.
3. `Pull from GitHub` 를 누르면 저장소의 Issue가 보드로 합쳐진다(이 브라우저의 업무는 지워지지 않음).
4. 토큰은 그 브라우저에만 저장되고, 백업 파일에는 포함되지 않는다. 공용 PC에서는 `Forget token` 으로 지운다.

> 기기마다 한 번씩 설정해야 합니다. 보드 화면의 업무는 브라우저에 저장되므로, 여러 기기·여러 대리가 같은 상태를 보려면 **Issues를 기준**으로 `Pull from GitHub` 을 씁니다.

## 클로드코드·다른 대리 연동 방법

에이전트는 GitHub Issues를 공용 작업대로 씁니다. 별도 서버나 데이터베이스가 필요 없습니다.

```bash
# 저장소 받기
gh repo clone drunken98-ops/ai-team-board

# 업무 목록 보기
gh issue list -R drunken98-ops/ai-team-board --state open

# 새 업무 등록  (제목은 "[상태] 내용" 형식)
gh issue create -R drunken98-ops/ai-team-board \
  --title "[Ready] 주간 보고 자료 정리" \
  --body "담당: 하대리 / 마감: 금요일 오전"

# 진행 상태 바꾸기 = 카드 옮기기 (Done이면 이슈를 닫는다)
gh issue edit 3 -R drunken98-ops/ai-team-board --title "[In Progress] 주간 보고 자료 정리"
gh issue close 3 -R drunken98-ops/ai-team-board --reason completed
gh issue comment 3 -R drunken98-ops/ai-team-board --body "초안 완료, 검토 요청"

# 보드 쪽에서 올린 업무를 가져오기
gh issue list -R drunken98-ops/ai-team-board --state all --json number,title,body,state
```

- 상태 접두어는 `[Backlog] [Ready] [In Progress] [In Review] [Done]` 다섯 가지입니다.
- 카드 이동 = 이슈 제목의 접두어 수정, 완료 = 이슈 close.
- 노션·슬랙 연동(클로드코드): 같은 저장소의 Issues를 노션 데이터베이스나 슬랙 채널과 잇는 방식으로, 노션에서 이슈 번호를 페이지 속성으로 두고 `gh issue edit` 로 상태를 맞춥니다.

## 이 저장소를 공개로 두는 이유

GitHub 무료 계정에서 GitHub Pages를 쓰려면 저장소가 Public이어야 합니다.
그래서 **개인정보(실명·연락처·주소·계좌·비밀번호·개인 일정 등)는 이 저장소 어디에도 적지 않습니다.** 업무 제목과 진행 상태만 오갑니다.

## 출처

- 기반 프로젝트: [devlewiso/OpsFlow](https://github.com/devlewiso/OpsFlow) — Kanban task board, single HTML file
- 원저작자 표기: © Neural Code Lab
