# IBSheet7 Skill for Claude

[IBSheet7](https://www.ibsheet.com/) 웹 그리드 개발을 돕는 [Claude Agent Skill](https://docs.claude.com/en/docs/claude-code/skills)입니다.
Claude가 기억에 의존하지 않고 **IBSheet7 공식 매뉴얼, 10년 이상 축적된 실제 기술 문의 Q&A, 운영 가이드**를 먼저 찾아본 뒤 그 근거로 답하도록 구성되어 있습니다.

> IBSheet7 전용입니다. IBSheet8은 API가 다르므로(예: `create`가 IBSheet7은 동기, IBSheet8은 비동기) 이 스킬의 대상이 아닙니다.

## 무엇을 도와주나요?

- **코드 작성** — `createIBSheet` → `SetConfig`/`InitHeaders`/`InitColumns` → `DoSearch`/`DoSave`까지 표준 흐름에 맞는 코드
- **API·이벤트·속성 사용법** — 함수·이벤트·속성별 매뉴얼을 근거로 인자·기본값·발생 조건 안내
- **오류·증상 해결** — `License is not valid`, 엑셀 다운로드 한글 파일명 깨짐, `OnSearchEnd` 오류 코드, 성능 저하, 브라우저 호환 등
- **환경/배포/업그레이드** — 서버모듈(1.0/2.0), POI, JDK/WAS, 버전 교체 시 주의사항

다음과 같은 경우 자동으로 활성화됩니다.

- IBSheet, IBSheet7, 아이비시트, `createIBSheet`, `InitColumns`, `DoSearch`, `Down2Excel`, `OnSearchEnd` 등을 언급할 때
- "시트", "그리드"로 조회·저장·엑셀·머지·트리·정렬·필터·합계·스타일 문제를 물을 때 (프로젝트에 ibsheet 파일이 있는 경우)

## 설치

### 방법 1. Claude Code 플러그인으로 설치 (권장)

Claude Code에서 다음을 실행합니다.

```
/plugin marketplace add ibsheet/ibsheet7_skill
/plugin install ibsheet7@ibsheet7-skill
```

### 방법 2. 스킬 폴더 직접 복사

`skills/ibsheet7` 폴더를 스킬 디렉터리에 복사합니다.

- 특정 프로젝트에서만 사용: `<프로젝트>/.claude/skills/ibsheet7`
- 모든 프로젝트에서 사용: `~/.claude/skills/ibsheet7`

```bash
git clone https://github.com/ibsheet/ibsheet7_skill.git
cp -r ibsheet7_skill/skills/ibsheet7 ~/.claude/skills/
```

Windows (PowerShell):

```powershell
git clone https://github.com/ibsheet/ibsheet7_skill.git
Copy-Item -Recurse ibsheet7_skill\skills\ibsheet7 "$env:USERPROFILE\.claude\skills\"
```

설치 후 Claude Code를 다시 시작하면 스킬이 인식됩니다.

### 방법 3. Claude.ai / Claude Desktop

`skills/ibsheet7` 폴더를 zip으로 압축한 뒤 **Settings → Capabilities → Skills**에서 업로드합니다.

## 사용 예시

```
IBSheet7으로 사번, 이름, 입사일(날짜), 급여(숫자) 컬럼을 가진 시트를 만들고 조회/저장 코드를 짜줘.
```

```
Down2Excel로 받은 엑셀 파일 이름의 한글이 깨져요.
```

```
mySheet_OnSearchEnd에서 Code가 -3이 넘어오는데 무슨 의미야?
```

```
로컬에서는 되는데 운영 서버(포트 8080)에서 License is not valid가 떠요.
```

## 구성

```
skills/ibsheet7/
├── SKILL.md                 # 스킬 정의 및 작업 흐름 (문서 검색 방법, 답변 원칙)
├── references/
│   ├── manual/              # IBSheet7 공식 매뉴얼 (함수·이벤트·속성별 문서)
│   │   ├── start/           #   Quick Start, 시트 구조, 개발 실습
│   │   ├── intro/           #   제품 특징, 배포 파일 구성
│   │   ├── props/           #   ibsheet.cfg 설정, 속성 목록
│   │   ├── funcs/           #   메서드 (init, search, save, row, cell, merge, tree, export ...)
│   │   ├── event/           #   이벤트 (OnSearchEnd, OnChange, OnClick ...)
│   │   ├── appx/            #   컬럼 타입, Theme, 필터, 엑셀 머지 등 부록
│   │   └── static/          #   JSON / XML / TreeJSON 데이터 형식, ReportXML
│   ├── qna/                 # 실제 기술 문의 Q&A 지식집 (주제별 17개 파일)
│   └── guides/              # 운영 가이드 (개발 주의사항, 성능, 업그레이드, 환경별 주의점)
└── evals/                   # 스킬 품질 검증용 평가 케이스
```

| Q&A 파일 | 주제 |
|---|---|
| `01_설치_배포_라이선스` | 설치 파일, 라이선스 오류, 캐시, 버전 교체 |
| `02_시트생성_초기화` | 시트 생성·초기화, ibsheet.cfg, 안 그려질 때 |
| `03_컬럼타입_속성_1/2` | 숫자·날짜 Format, 콤보, 체크박스, 팝업, 입력 제한 |
| `04_조회_저장_데이터` | DoSearch/DoSave, 페이징, JSON/XML, 서버 연동 |
| `05_이벤트` | 이벤트 발생 조건·순서 |
| `06_행_셀_컬럼조작API` | 값 읽기/쓰기, 행 추가/삭제, 포커스, 숨김 |
| `07_셀_행_컬럼병합` | 머지 |
| `08_트리_그룹_계층` | 트리, 그룹행 |
| `09_정렬_필터_합계_계산` | 정렬, 필터, 소계/합계, 피벗 |
| `10_엑셀_PDF_CSV입출력_1/2` | 엑셀·CSV·PDF 입출력, 서버모듈, POI, DRM |
| `11_성능_메모리` | 속도·대용량 |
| `12_화면표시_스타일_사용자조작_1/2` | 스타일·CSS·너비/높이·스크롤·복사붙여넣기 |
| `13_브라우저_OS호환` | IE/Chrome/Edge, 모바일, 웹접근성 |
| `14_프레임워크_외부연동` | Spring/전자정부, jQuery, iframe, SPA |
| `15_부가컴포넌트` | IBChart, IBUpload, IBTab, IBCalendar 등 |
| `16_제품일반` | 지원 범위·정책, 보안(CSP), IBSheet8 전환 |
| `ibsheet7_server_qna_merged` | 서버모듈 전용 (JDK17/21·Jakarta, POI, DRM 등) |

## 참고

- Q&A는 문의 처리 당시 기준입니다. 답변에 "적용 버전"이 있으면 사용 중인 `ibsheet.js` 버전과 비교해 주세요.
- 스킬이 문서에서 근거를 찾지 못하면 추측하지 않고 버전·환경·재현 코드를 요청하거나 제조사 문의를 안내합니다.
- 제품 관련 문의: support@ibleaders.co.kr
