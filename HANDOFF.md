# ITCEN VisionCenter 교육 기획·운영 AI Agent — 인수인계 문서

> 다른 사람이 이 프로젝트를 이어서 작업할 수 있도록 정리한 문서입니다.
> 비개발자도 따라올 수 있게 "무엇을 / 왜 / 어디를 고치면 되는지" 중심으로 적었습니다.
> 최종 갱신: 2026-11-03

---

## 1. 한 줄 요약

**교육 기획(조사·강사 찾기)부터 운영 서류(실시·결과 품의서·입과 안내 메일·출석부·학습자 명단·교육비 정산)까지
한곳에서 처리하는 VisionCenter 업무용 웹 도구**입니다. 파일 하나(`index.html`)로 동작합니다.

- 목적: 교육 담당자가 "근거 있게" 기획하고, 반복되는 운영 서류 작업을 빠르게 끝내기 (배경은 `PRD_교육기획근거도우미.md`)
- 형태: HTML 파일 1개 (서버·빌드·설치 없음)
- AI: Google Gemini API (무료 등급, 웹 검색 그라운딩 사용)
- 배포: GitHub Pages (정적 호스팅, `visioncenter` GitHub 조직)
- 공개 URL: <https://visioncenter.github.io/>
- 저장소: <https://github.com/visioncenter/visioncenter.github.io>
- 예전 주소(`edu-research-tool`)는 폐기됨 — 공유할 땐 위 주소만 사용

---

## 2. 지금 상태

- ✅ GitHub Pages 배포 중 (위 공개 URL)
- ✅ 접속하면 먼저 **Gemini API 키 입력 화면**이 뜨고, 키를 넣어야 본 화면으로 들어감
  (키·결과·입력값은 그 사람 브라우저에만 저장)
- ✅ 탭 8개(왼쪽 메뉴 그룹): **교육 기획** 📊 교육 조사 / 🎤 강사·강의 찾기 · **교육 운영** ✉️ 입과 안내문 / 📋 출석부·명단 / 📝 실시 품의서 / 📑 결과 품의서 / 💰 교육비 정산 · (구분선) 🖼️ 공지문 메이커
  - 그룹 제목은 `.maintab-group` span(가로 탭 모드에선 숨김). 새 탭은 해당 그룹 아래에 버튼 추가
- ✅ 🖼️ 공지문 메이커 탭은 별도 파일 `notice-maker.html`을 iframe으로 불러옴(탭을 처음 열 때 로드). 공지문 메이커 수정은 그 파일만 고치면 됨. 저장 키 `vc-notice-maker-v2`
- ✅ 넓은 화면(900px 이상)에선 메인 탭이 왼쪽 세로 메뉴(sticky), 좁은 화면에선 기존 가로 탭 — CSS `@media (min-width: 900px)` 블록
- ✅ 탭마다 `📖 이 탭 사용법` 버튼 + 사용 설명서 모달(시작하기·탭별 설명)
- ⚠️ **실제 키로 처음부터 끝까지 돌려본 테스트는 아직 안 함** — AI 호출 기능(조사/강사 찾기/
  문서 생성/안내문 AI 작성/품의서 채우기)과 2026-11-03 고친 오류 처리는 가짜 응답으로만 확인한 상태.
  실제 키로 한 번 돌려보는 것을 권장.
- ℹ️ 출석부·명단 탭과 입과 안내의 엑셀 읽기는 AI를 쓰지 않음(브라우저 안에서만 처리, 전송 없음)

---

## 3. 주요 기능 (탭별)

### 📊 교육 조사
- 교육 주제 + 옵션(조사 항목 체크 13종, 출력타입/범위/분량 + 기간/규모/직급 드롭다운, 자유 입력)
- **실시간 스트리밍 출력**, 혼잡·한도 시 자동 재시도 + 보조 모델 전환
- **연관 키워드 추천**(주제 입력 후 버튼 → 칩 클릭으로 주제 교체)
- 보고서 카드 + 검색 출처 + 시간배분 막대그래프 + 커리큘럼 표
- **복사 / Markdown(.md) / Word(.doc) / 인쇄·PDF**, **결과 공유 링크**(URL 해시 `#r=`)
- **이어서 질문하기**(멀티턴)
- **AI 추가 문서**(토글 버튼): 📊 발표 슬라이드 개요 / 📋 교육 품의서 / 🗂 교육 운영 계획서 /
  📝 설문(사전·사후) / 🎤 임원 예상질문 & 방어 답변 / 🥊 레드팀 허점 검토
- 💰 예산 계산기(AI 단가 추정) / 📈 ROI·기대효과 산출(예산 연동)
- 🕘 이전 조사 기록(최근 20건, JSON 내보내기/가져오기), 조건 프리셋, 📄 보고서 표지 설정

### 🎤 강사·강의 찾기
- 강의 주제 키워드 → 멀티캠퍼스·인프런 같은 강사 매칭 플랫폼·교육업체 위주로 후보 검색·비교표
- 결과는 AI가 판단한 신뢰도 순 정렬, 링크는 검색 출처 기반(최종 확인은 직접)
- **시간당 단가 범위 필터**(만원 단위, 선택)
- **기존 교재 PDF 업로드**(클릭/드래그, 15MB 이하) → 교재 내용을 분석해 맞는 강사·강의 검색
- ⭐ **저장함**: 마음에 드는 후보를 담아두기/삭제

### ✉️ 입과 안내 HTML
- 과정명·차수·교육형태 배지·날짜·시간·장소·사전 안내를 입력하면 오른쪽에 **미리보기가 바로 바뀜**
- **수강생 엑셀(.xls) 넣으면** 과정 정보 + **받는 사람 메일주소 목록** 자동 채움
  (수강취소 상태는 제외, 메일주소는 저장 안 함)
  - 엑셀 받는 곳: CEN-Class › 과목 개설 › 학습자 › 엑셀 다운로드
- **메일 제목 자동 생성**(과정명·차수·날짜 기반) + 복사
- 교육시간 자동 계산(6시간 이상이면 점심 1시간 제외, 직접 고치면 그 값 사용)
- 장소: 과천 아이티센타워 3층 C·E·N교육장 선택 또는 ✏️ 직접 입력
- **사전 안내 항목**: 항목 추가 → ✨ 칸에 대충 적고 "AI로 작성"(Ctrl+Enter) → 안내문으로 다듬음,
  URL은 링크로 자동 연결. 예시 버튼(Claude Pro 계정, 노트북 지참)
- 하단 공통 안내(수강취소·주차) 켜고 끄기
- 출력: **📋 HTML 코드 복사**(에디터 HTML 소스 모드용) / **✨ 서식째 복사**(일반 편집 화면용)
- 입력값은 브라우저에 자동 저장, ↺ 입력 초기화

### 📋 출석부·명단
- CEN-Class 수강생 엑셀(.xls/.xlsx) → 과정 정보 자동 채움 → **Word(.docx) 2종 생성**
  - **출석부**: 이름 가나다순. 하루 교육이면 이름·직급·서명 열, 여러 날이면 일차별 서명 칸(1~5일)
  - **학습자 명단**: 부서 가나다순(같은 부서는 이름순)
- 날짜·일차는 엑셀의 학습시작일·총시수로 계산(하루 7시간 기준, 주말 건너뜀) — 다르면 수정
- 학습자 체크 해제 시 문서에서 제외(수강취소는 처음부터 해제), 명단에 없는 사람 직접 추가
- Word 양식은 `index.html` 안에 base64로 내장(`ROSTER_DOCX_TPL`) → 별도 파일 없음

### 📝 실시 품의서
- 자료(PDF·이미지·Word·엑셀·텍스트, 여러 개) + 교육 내용 메모 → **✨ AI로 품의서 채우기** → 항목별 편집 → 그룹웨어 품의서 HTML
  - PDF·이미지는 Gemini에 파일째 전송, Word·엑셀·텍스트는 브라우저에서 글자만 뽑아 프롬프트에 넣음
  - 캡처 이미지는 이 탭에서 Ctrl+V로 추가. 이전 차수 품의서를 메모에 붙여넣어도 됨(HTML이면 글자만 추출)
  - AI는 자료에 없는 숫자·날짜·경력은 비워두도록 지시
- 양식: 도입 문장 → `- 아 래 -` → 1. 개설배경 / 2. 과정개요(과정명·교육일정 표·교육대상·교육장소·모듈구성 표) /
  3. 외부강사 정보(체크 해제 시 빠짐) / 4. 예상 교육비(표 + ※ 참고 문구) / 5. 비고 → 끝. (빈 섹션은 빠지고 번호 자동 재정렬)
- 자동 계산: 교육일정 표기(주말 제외, 6시간 이상 점심 1시간 제외), 품의 제목 `[교육] 과정명 실시 품의 (차수)`,
  교육비 VAT 10%·합계·계, 같은 비용구분 연속 시 칸 합치기(rowspan), 모듈 시간 합계 ↔ 교육시간 불일치 ⚠️
- 비고 칸 `//` = 줄바꿈. 도입 문장 비우면 `<과정명>을 아래와 같이 실시하고자 품의를 상신합니다.` 자동
- 출력: 품의 제목 복사 / HTML 코드 복사 / 서식째 복사. `예시 불러오기` = Claude Code 검증 워크플로우 2차수 품의서
- 입력값 자동 저장(`edu_approval_v1`). 올린 파일은 저장 안 함

### 💰 교육비 정산 (그룹공통)
- 비용 항목(항목·공급가액·비고·부가세·배분 체크) + 법인별 수료 인원 → 법인별 공급가액·부가세·합계
- 계산 규칙
  - 부가세 = 공급가액 × 0.1, **원 미만 버림** (항목별·법인별 모두)
  - 법인 공급가액 = 배분 대상 공급가액 × 법인 인원 ÷ 전체 인원, 원 미만 버림 → 끝수는 **인원 최다 법인**에 가산(화면에 ⚠️ 표시)
  - 부가세 없는 항목이 섞이면 과세/면세분을 따로 배분해서 과세분에만 부가세
- 수료자 엑셀: '법인' 열로 집계. '수료/이수' 열이 있으면 Y·O·수료·이수·완료만, 없으면 '수강상태' 취소·반려·미수료 제외
  - 법인명 앞뒤 (주)·㈜·주식회사 제거. 정렬은 ET·GLO·CLO·COR·CTS·CPF·PNS·TKMS·SON·SIT·KDA·CRD 순, 그 외는 인원순
  - **🏢 법인 담당자 목록**(`edu_corp_dir_v1`): "법인명, 약칭, 담당자, 이메일" 줄 단위로 붙여넣고 저장(쉼표·공백 섞여도 됨, 약칭=영문 대문자 토큰)
    - 엑셀 법인명 → 약칭 자동 매핑(정확히 일치 → '아이티센' 뗀 이름 일치 → 포함 관계 순). 법인 칸 드롭다운(datalist)·법인명 입력도 약칭으로 변환
    - 정렬도 목록 순서. 행에서 담당자를 고치면 목록에도 반영. 팀원 공유는 `목록 복사` → 붙여넣기
    - **실명·이메일은 코드(공개 저장소)에 넣지 않음** — 각자 브라우저에만 저장
  - 목록에 없는 법인: 직접 고친 이름 매핑(`edu_corp_alias_v1`), 담당자(`edu_corp_contacts_v1`)로 기억
- 출력: 법인 별 비용배분 표 HTML 복사, **엑셀(.xlsx) 다운로드**(비용 사용내역 + 법인별 배분, 숫자 천 단위 서식)
- 표 형식(`stView`, 저장 `edu_settle_view_v1`): 내부용(구분 열 + 약칭) / 교육업체 발송용(구분 열 없음 + 한글 법인명 — 담당자 목록의 법인명). `✨ 표 서식째 복사`는 Outlook 붙여넣기용 HTML + 탭 구분 텍스트. 결과 품의서는 항상 내부용
- `📝 실시 품의서 교육비 가져오기`: 실시 품의서 비용 중 비용구분 '예비비' 제외하고 불러옴
- 정산 유형은 지금 그룹공통만. 다른 유형은 `stType` select에 추가 예정

### 📑 결과 품의서
- 양식: 도입 문장(앞부분 + `<과정명>에 대해 법인별 참가인원에 따라 비용을 정산코자 하오니 재가 바랍니다.`) → `- 아 래 -` →
  1. 과정 개요(① 과정명 ② 일 시 `2026. 9. 15 (화) 09:00 ~ 17:00 (1일 7H)` ③ 장 소) / 2. 총 비용 / 3. 비용 사용내역 표 /
  4. 법인 별 비용배분(안내 문구 + 표) → `별첨. … 끝.`
- **2~4는 💰 교육비 정산 탭 값을 실시간으로 받아 씀**(`settle-change` 이벤트). 결과 품의서 탭에는 비용 입력칸 없음
- `📝 실시 품의서에서 가져오기`: 과정명·차수·일시·장소(과천 C/E/N → `과천 아이티센타워 3층 비전센터 X교육장`)
- `✨ AI로 작성`: 도입 문장 앞부분("… 강화를 위해 실시한")을 실시 품의서의 대상·도입 문장·모듈로 작성
- 제목: `[교육] 과정명 교육 결과 보고 및 교육비 정산 품의` (+ 차수 있으면 ` (차수)`)
- 저장: `edu_result_v1`. `예시 불러오기`는 정산 탭까지 예시로 바꿈(담당자·이메일은 비워 둠 — 공개 사이트라 실명·메일 미포함)

### 공통
- **수강생 엑셀은 탭 간 공유**: 입과 안내나 출석부 탭 중 한 곳에 넣으면 다른 탭에도 반영
- 🔑 키 변경(오른쪽 위), 🌙 다크모드(토스 스타일 테마), 📖 사용 설명서

---

## 4. 기술 구성 (의도적으로 단순하게)

| 항목 | 내용 | 왜 이렇게 했나 |
|------|------|----------------|
| 파일 | `index.html` 하나 (+ 로고 png) | 비개발자가 환경설정 없이 바로 쓰고 배포 |
| 의존성 | 없음 (외부 라이브러리 X) | 깨질 부분 최소화 |
| 엑셀 읽기 | `.xls`(구형 바이너리)·`.xlsx`를 직접 파싱(`readXls`/`readXlsx`) | 라이브러리 없이 처리 |
| Word 생성 | 내장 .docx 양식을 풀어서(`zipRead`) 표를 채우고 다시 묶음(`zipWrite`) | 라이브러리 없이 처리. 최신 Chrome/Edge 필요(`DecompressionStream`) |
| AI 호출 | 브라우저에서 Gemini API 직접 fetch (스트리밍) | 서버 없이 동작 |
| 웹 검색 | Gemini `google_search` 그라운딩 | 최신 정보 + 출처 확보(환각 줄이기) |
| API 키 | 코드에 안 박음 → 시작 화면에서 사용자가 입력 → `localStorage` | 키 유출 방지. 공개 배포해도 안전 |
| 저장 | 전부 `localStorage`(브라우저 안) | 서버·DB 없음 |

> **보안 원칙**: API 키 등 비밀값은 절대 코드/저장소에 넣지 않습니다. 키는 사용자 브라우저
> `localStorage`(`gemini_api_key`)에만 저장되고 Gemini API 호출 외에는 외부로 전송되지 않습니다.
> 수강생 명단(이름·부서·메일)도 브라우저 안에서만 처리하고 저장·전송하지 않습니다.

### 사용하는 localStorage 키
| 키 | 내용 |
|----|------|
| `gemini_api_key` | Gemini API 키 |
| `edu_theme` | 라이트/다크 |
| `edu_company`, `edu_author`, `edu_logo` | 보고서 표지 설정 |
| `edu_history_v1` | 조사 기록 |
| `edu_presets_v1` | 조사 조건 프리셋 |
| `edu_seen_guide` | 사용 설명서 본 적 있음 |
| `edu_instr_saved_v1` | 강사·강의 저장함 |
| `edu_enroll_v1` | 입과 안내 입력값 |
| `edu_roster_v1` | 출석부·명단 입력값(과정 정보) |
| `edu_approval_v1` | 실시 품의서 입력값(메모 포함, 올린 파일 제외) |
| `edu_settle_v1` | 교육비 정산 입력값(비용 항목·법인 인원) |
| `edu_result_v1` | 결과 품의서 입력값 |
| `edu_corp_dir_v1` | 🏢 법인 담당자 목록(법인명·약칭·담당자·이메일) |
| `edu_corp_contacts_v1` | 목록에 없는 법인의 담당자·이메일 |
| `edu_corp_alias_v1` | 엑셀 법인명 → 표기명 매핑 |

---

## 5. 파일 목록 / 저장소 구성

**저장소(GitHub)에 올라가는 = 웹사이트 구성 파일** (`.gitignore` 화이트리스트로 이것만 추적)
- **`index.html`** — 실제 도구 (핵심)
- `01_ITCEN_Logo_Basic.png` — 상단 로고
- `notice-maker.html` — 🖼️ 공지문 메이커(단독 페이지, index.html 탭에서 iframe으로 불러옴)
- `HANDOFF.md` — (이 문서)
- `.gitignore` — 추적 대상 화이트리스트

**로컬에만 있고 저장소엔 안 올라감(내부 자료)**
- `PRD_교육기획근거도우미.md`, `00_용어설명.md`~`09_E_CI.md`, `hanbit-prd-interview.skill`, `DESIGN.md` 등

> 로컬 작업본: 바탕화면 `프로젝트폴더` = 이 GitHub 저장소의 로컬 복사본.
> 강의자료·PRD는 `.gitignore`로 가려져 GitHub에 공개되지 않습니다.

---

## 6. 코드 구조 (`index.html` 내부)

`<style>`(디자인·테마) + `<body>`(화면) + `<script>`(동작) 세 부분.
화면은 `<div id="panel-research|instructor|enroll|roster|approval|result|settle" class="tabpanel">` 7개로 나뉘고,
상단 `.maintab` 버튼의 `data-panel` 값으로 전환됩니다.

### 6-1. 디자인/테마
- 색은 전부 **CSS 변수**(`:root` 라이트, `:root[data-theme="dark"]` 다크). 색을 바꾸려면 이 두 블록만 수정.
- 인쇄(`@media print`)는 다크여도 항상 라이트로 강제. 모바일(`@media max-width:640px`) 대응.

### 6-2. 자바스크립트 핵심 (여기를 고치면 동작이 바뀜)

**공통 / AI 호출**
| 위치 | 역할 |
|------|------|
| `passGate/showGate/enterGate` | 시작 화면(API 키 입력) ↔ 본 화면 전환 |
| `const MODELS = [...]` | 쓸 Gemini 모델 목록(혼잡 시 순서대로 전환, 현재 3.8 → 3.7 → 3.5 Flash-Lite). **모델명 바뀌면 여기만 수정** |
| `callGeminiStream(...)` | 실제 스트리밍 API 호출(SSE 파싱). `useSearch=false`면 검색 끔 |
| `runGemini(apiKey, contents, onText, useSearch)` | 재시도+보조모델 전환 래퍼. 못 쓰는 모델은 건너뜀. 모든 AI 호출이 여기 통함 |
| `friendlyError(e)` | API 오류를 한국어 안내로 변환(한도·키 오류·혼잡·네트워크 차단) |
| `chunkText/chunkCites/chunkSupports` | 스트리밍 조각에서 본문/출처 추출 |
| `showGuideSection` | 사용 설명서 모달 탭 전환 |

**📊 교육 조사**
| 위치 | 역할 |
|------|------|
| `const ITEMS = [...]` | **조사 항목 목록**(체크박스 자동 생성) |
| `buildPrompt(...)` | 선택값을 모아 조사 프롬프트 조립 |
| `renderMarkdown(md)` | 마크다운 → 보고서 HTML. 표 셀의 `<br>`만 줄바꿈 허용 |
| `renderChart/renderSources/buildCoverHtml/buildReportNav` | 막대그래프 / 출처 / 인쇄 표지 / 보고서 목차 |
| `finalizeReport(...)` | 보고서 렌더+표지+출처+기록저장 (조사/기록/공유 공통) |
| `research()` | 조사하기 전체 흐름 |
| `askFollowup` / `toggleDoc` | 멀티턴 질문 / 추가 문서 버튼 열고 닫기 |
| `exportMarkdown/exportDoc/downloadFile` | .md / .doc 내보내기 |
| `loadHistory/saveHistoryRecord/renderHistory/loadHistoryItem` | 조사 기록 |
| `snapshotOptions/applyOptions/renderPresetOptions` | 프리셋 |
| `computeBudget/estimateBudget` · `computeRoi/estimateRoi/syncRoiFromBudget` | 예산 · ROI |
| `loadSharedReport` | 공유 링크 열기 |
| `recommendKeywords` | 연관 키워드 추천 |

**🎤 강사·강의 찾기**
| 위치 | 역할 |
|------|------|
| `buildInstructorPrompt` | 강사 검색 프롬프트 |
| `searchInstructors` | 검색 전체 흐름 |
| `parseFirstTable/resolveRowLink/parseHourlyRate` | 결과 표 파싱, 행별 출처 링크 연결, 시간당 단가 추출(필터용) |
| `renderInstructorResults/renderInstrSources` | 결과 카드·출처 표시 |
| `analyzeInstrPdf/buildPdfAnalysisPrompt/parsePdfAnalysis` | 교재 PDF 분석 |
| `loadInstrSaved/saveInstrCandidate/deleteInstrCandidate/renderInstrSaved` | ⭐ 저장함 |

**✉️ 입과 안내** (`setupEnroll()` 안에 모여 있음)
| 위치 | 역할 |
|------|------|
| `buildHtml` | 메일 본문 HTML 조립(**VisionCenter 참석 안내 양식은 여기**) — `boxHtml/infoRow/footerBox` 조각 |
| `subjectText` | 메일 제목 자동 생성 |
| `autoHours/dateText` | 교육시간 자동 계산 / 날짜 표기 |
| `addNotice/noticeRow/aiWriteNotice` | 사전 안내 항목 추가 / AI로 안내문 다듬기 |
| `loadEnrollFile` | 수강생 엑셀 → 과정 정보·받는 사람 목록 |
| `save/update` | 입력값 저장 / 미리보기 갱신 |

**📋 출석부·명단** (`setupRoster()` 안에 모여 있음)
| 위치 | 역할 |
|------|------|
| `loadFile/renderTable` | 엑셀 읽어 학습자 목록 표시 |
| `addBizDays/dateLine` | 주말 건너뛴 일차 계산 / 날짜 줄 |
| `ROSTER_DOCX_TPL` + `loadTemplate` | 내장 Word 양식(base64) 불러오기 |
| `buildDocx(kind)` | `attend`(출석부) / 명단 Word 생성 — 열 구성·정렬은 여기 |
| `cell/table/download` | Word 표 XML 조립 / 파일 저장 |

**📝 실시 품의서** (`setupApproval()` 안에 모여 있음)
| 위치 | 역할 |
|------|------|
| `buildHtml` (+ `P/SP/BUL/TD/TABLE/HEAD` 조각) | 품의서 본문 HTML 조립 — **양식 스타일을 바꾸려면 여기** |
| `schedInfo` | 교육일정 표기·총 시간 계산 |
| `addMod/addInstr/addCost` + `readState/setState` | 모듈·강사·비용 반복 행, 폼 ↔ 데이터 변환 |
| `addFiles/docxText/renderFiles` | 자료 파일 읽기(PDF·이미지 base64 / Word·엑셀·텍스트 → 글자) |
| `buildApPrompt/parseJson/applyAi` | AI 프롬프트(작성 규칙·문체 예시) / 응답 JSON 반영 |
| `SAMPLE` | `예시 불러오기` 데이터 |

**💰 교육비 정산 + 📑 결과 품의서** (`setupSettleAndResult()` 하나에 같이 있음 — 계산을 공유)
| 위치 | 역할 |
|------|------|
| `allocate(total, corps)` | 인원수 비율 배분 + 끝수 처리 — **배분 규칙을 바꾸려면 여기** |
| `computeSettle()` | 항목별 부가세·합계, 법인별 배분 결과 계산(정산·결과 품의서 공통) |
| `allocTableHtml/usageTableHtml` | 4. 법인 별 비용배분 표 / 3. 비용 사용내역 표 |
| `loadSettleFile` | 수료자 엑셀 → 법인별 인원 |
| `xlsxBlob(rows, sheet)` | 라이브러리 없이 .xlsx 만들기 |
| `buildRsHtml/rsSchedText` | 결과 품의서 본문 / 일시 표기 |

**엑셀·압축 공통 유틸**
| 위치 | 역할 |
|------|------|
| `readXls/readXlsx/readSheetFile` | .xls/.xlsx 읽기 |
| `zipRead/zipWrite/inflateRaw/crc32` | .docx/.xlsx(zip) 풀기·묶기 |
| `shareRosterRows` + `roster-file` 이벤트 | 수강생 엑셀을 입과 안내 ↔ 출석부 탭에 공유 |
| `bindPlaceSelect` | 교육장 선택(C·E·N/직접 입력) 공통 처리 |

### 6-3. 흐름
```
[조사하기] → buildPrompt() → runGemini(스트리밍) → finalizeReport()
[추가 문서 버튼] → toggleDoc() → askFollowup()
[강사·강의 찾기] → (PDF 있으면 analyzeInstrPdf) → searchInstructors() → runGemini() → renderInstructorResults()
[수강생 엑셀 투입] → readSheetFile() → shareRosterRows() → 입과 안내·출석부 탭 둘 다 채움
[입과 안내 입력] → update() → buildHtml()/subjectText() → 미리보기·복사
[출석부/명단 다운로드] → buildDocx() → loadTemplate() → zipWrite() → .docx 저장
[품의서 자료+메모] → addFiles() → [AI로 채우기] buildApPrompt() → runGemini() → applyAi() → update() → buildHtml()
```

---

## 7. 자주 하는 수정 (레시피)

- **조사 항목 추가/삭제**: `ITEMS` 배열에 `{ id, label, text, def }` 한 줄.
- **Gemini 모델 변경**: `const MODELS = [...]` 모델명 교체. (예: 2026-11 Google이 신규 키에 `gemini-2.5-flash`를 막아서 교체함)
- **프롬프트 말투/형식**: `buildPrompt`(조사) / `buildInstructorPrompt`(강사) 템플릿 수정.
- **추가 문서 버튼 추가**: 버튼 HTML + 요소 ref + `genButtons()` 목록 + `toggleDoc("새키", 버튼, "프롬프트", "라벨")`.
- **입과 안내 메일 양식 변경**: `setupEnroll()` 안의 `buildHtml`·`footerBox` 수정. 공통 안내 문구도 여기.
- **교육장 추가**: 입과 안내·출석부 탭의 장소 `<select>` 두 곳에 `<option>` 추가.
- **품의서 작성 규칙·문체 변경**: `buildApPrompt`의 `[작성 규칙]`·`[문체 예시]` 수정.
- **교육비 배분 규칙 변경**(예: 끝수를 다른 법인에): `allocate`. 부가세 규칙은 `vatOf`.
- **결과 품의서 양식 변경**: `buildRsHtml` (표는 `usageTableHtml`·`allocTableHtml`).
- **품의서 양식 변경**: `setupApproval()` 안의 `buildHtml`. 교육장 목록은 `apPlaceSel` `<select>`.
- **출석부·명단 열 구성/정렬 변경**: `setupRoster()` 안의 `buildDocx` 수정.
- **새 탭 추가**: `.maintab` 버튼(`data-panel="이름"`) + `<div id="panel-이름" class="tabpanel hidden">` +
  사용 설명서 `guide-tab`(`data-g="이름"`)과 설명 섹션.
- **테마 색 변경**: `:root` / `:root[data-theme="dark"]` 변수값만 수정.

---

## 8. 배포 / 업데이트 방법

**`프로젝트폴더`가 GitHub 저장소에 직접 연결돼 있습니다.** 이 폴더에서 바로 푸시하면 됩니다.

1. `프로젝트폴더`의 `index.html`(또는 로고 등) 수정
2. 터미널(Git Bash/PowerShell)에서:
   ```bash
   git add index.html
   git commit -m "수정 내용 설명"
   git push
   ```
   (Claude에게 "수정하고 배포해줘"라고 해도 됨)
3. 1~2분 뒤 공개 URL을 **Ctrl+F5**(강력 새로고침)로 확인 — 팀원에게도 Ctrl+F5 안내

- 새 파일을 웹사이트에 포함시키려면 `.gitignore`의 화이트리스트(`!/파일명`)에 추가해야 함.
- 다른 곳(Claude 클라우드 등)에서 푸시했다면 로컬에서 `git pull` 먼저.

---

## 9. 알려진 제약 / 주의사항

| 항목 | 내용 |
|------|------|
| **API 키 각자 발급** | Gemini 무료. 키는 `AIza`로 시작(OpenAI `sk-...`와 혼동 주의). 발급: <https://aistudio.google.com/app/apikey> |
| **무료 한도/혼잡** | 분당 요청수 제한/일시 혼잡 → 자동 재시도·보조모델 전환. 원인은 한국어로 안내됨 |
| **신규 키 모델 제한** | Google이 신규 가입자에게 일부 구형 모델을 막을 수 있음 → `MODELS` 교체 |
| **회사 네트워크** | 사내망에서 Gemini API가 막히면 호출 실패(안내 문구 표시) |
| **환각** | AI가 사실을 지어낼 수 있음. **실제 링크는 "검색 출처"에서만**. 강사 단가·경력, 회사 사례·수치는 재확인 |
| **Word 생성 브라우저** | 최신 Chrome/Edge 필요(`DecompressionStream`) |
| **공유 링크 길이** | 아주 긴 보고서는 링크화 불가 → Word/PDF/.md로 공유 |
| **기록은 브라우저별** | 기기·브라우저마다 따로. 옮길 땐 기록 JSON 내보내기 → 가져오기 |
| **로고 다크모드** | 로고 글자가 검정이라 다크모드에선 흰 박스를 깔아 처리 |

---

## 10. 작업 이력 (요약)

**v1 (2026-06)**: PRD → MVP → "키워드로 조사" 방향 전환 → Gemini 직접 연결(무료+웹검색) →
조사 항목/필터 → 보고서 카드·인쇄 → 커리큘럼표/시간그래프 → 혼잡 재시도·보조모델 →
링크 환각 차단 → GitHub Pages 배포.

**v2 (2026-06)**: 자유 입력, 폴더-저장소 직접 연결(git push 배포), 스트리밍 출력, 기록·프리셋,
멀티턴·추가 문서, 예산 계산기·ROI, 모바일, 다크모드·표지·연관 키워드.

**v3 (2026-09)**: 🎤 강사·강의 찾기 탭(교재 PDF 업로드·드래그앤드롭, 저장함), 카드 제목 깨짐 수정.

**v4 (2026-11-03)** — "기획 도구"에서 "기획·운영 도구"로 확장:
1. 강사 찾기 시간당 단가 필터
2. ✉️ 입과 안내 HTML 탭(메일 제목 자동, 엑셀로 받는 사람 목록, 사전 안내 AI 작성 + 링크 자동 연결)
3. 📋 출석부·학습자 명단 Word 생성 탭(하루 교육은 이름·직급·서명 열)
4. 수강생 엑셀 탭 간 공유, 교육장 선택(C·E·N/직접 입력)
5. 배포 주소 변경 → `visioncenter.github.io`
6. 도구 이름 변경 → ITCEN VisionCenter 교육 기획·운영 AI Agent
7. API 키 시작 화면 + 키 발급 안내, 탭별 사용 설명서, CEN-Class 엑셀 받는 경로 안내
8. Gemini 보조 모델 교체(2.5-flash → 3.7 / 3.5 Flash-Lite) + 오류 원인 한국어 안내
9. 📝 실시 품의서 탭(자료·메모 → AI 채우기 → 그룹웨어 품의서 HTML)
10. 📑 결과 품의서 + 💰 교육비 정산(그룹공통 n분의 1) 탭

---

## 11. 다음 할 일 (후보)

- [ ] 실제 Gemini 키로 7개 탭을 처음부터 끝까지 테스트
- [ ] 📈 만족도 조사 결과 정리 탭(설문 XLS → 익명화 엑셀 + 강사 메일 요약)
- [ ] 🎓 수료증 자동 생성(수강생 엑셀 재사용)
- [ ] 💰 교육비 정산(계열사별 인원 비율 배분)
- [x] 📝 실시 품의서 탭
- [x] 📑 결과 품의서 + 💰 교육비 정산(그룹공통)
- [ ] 💰 교육비 정산 — 그룹공통 외 유형
