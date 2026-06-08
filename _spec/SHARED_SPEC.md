# 공유 스펙 — '학교 행정업무 혁신' 코스 저장소 (모든 작성 에이전트 필독)

> 이 스펙은 병렬 작성 에이전트(W-A 사람용 실라버스 / W-B M1 콘텐츠 / W-C 에이전트 인터페이스)의 단일 기준이다. 여기 정의된 메타데이터·스키마·수치와 다르게 쓰지 마라.

## 1. 코스 메타데이터 (고정)

| 키 | 값 |
|---|---|
| course_id | `SAI-501` |
| 강의명 | 학교 행정업무 혁신 (School Administrative Work Innovation with AI) |
| 대상 | 교육대학원 석사과정 — **현직교사** |
| 형태 | 프로젝트 기반 학습(PBL), 15주, 주 1회 3시간 |
| 최종 산출 | 인공지능 활용 행정업무 혁신 계획서 (소속 학교 맥락 적용) |
| 버전 | course_package v0.1.0 (2026-06-08) |

## 2. 모듈 구조 (고정 — 6모듈 × 15주 매핑)

| 모듈 ID | 제목 | 주차 | 핵심 질문 |
|---|---|---|---|
| M1 | 행정업무의 실태 — 무엇이 발생하는가 | 1–3주 | 학교에서 어떤 행정업무가 발생하는지 데이터로 분석 |
| M2 | 발생 원인 — 왜 학교에서 발생하는가 | 4–5주 | L0–L4 조건 레이어로 원인을 데이터 기반 분석 |
| M3 | 담당 구조 — 왜 그가 담당하는가 | 6–7주 | 업무 배분·검토경계를 데이터 기반 분석 |
| M4 | 데이터 파이프라인 설계 | 8–10주 | 행정업무 혁신을 위한 수집→전처리→분석 파이프라인 설계 |
| M5 | AI 활용 전략 수립 | 11–12주 | 업무별 AI 적용 가능성 평가·전략 수립 (시뮬레이터 실습) |
| M6 | 혁신 계획 수립 | 13–15주 | AI 활용 행정업무 혁신 계획서 작성·동료평가·발표 |

## 3. 지식베이스 근거 수치 (이 수치만 인용 — 임의 수치 금지)

- 공개 행정문서 1차 수집 **20,755건** → 분석 가능 파일 **15,851건**
- 대표학교 연간 업무사건 후보 **714건** (2025.3–2026.2, 정보공개포털 161 + 학교 홈페이지 553)
- 업무 도메인 **10개**: 급식보건안전(195) · 평가교육과정(141) · 개인정보민원보고(116) · 방과후늘봄(69) · 학교운영위원회(43) · 행정재정(16) · 학교폭력생활지도(6) · 특수복지(3) · 미분류(124) · 학교구조변수(1)
- 일반모델 조건 단위 **320개** (L0–L4, v6) · 특수모델 업무 이벤트 **857건**
- 보수 해석률(v6) **19.49%** · 광의 해석률 **46.79%** — "일반모델이 개별 학교 업무를 어디까지 설명하는가"
- 현장체험학습 관련 이벤트 **2,629건** (병목 검증 카드 TVC-001)
- L0–L4 정의: L0 법령·교육부 지침·국가시스템 / L1 전국 공통 핵심조건 / L2 시도교육청 조건 / L3 학교급·규모·유형·지역·프로그램 / L4 개별학교 공개 흔적·고유 사건

## 4. 문체·표기 규칙

- 한국어. 수업 문서 문체(학생에게 말 걸기 가능). 과장 금지, 모든 사실 주장은 §3 수치나 resource_id 인용.
- **비식별**: 특정 학교명·인명·지역명을 본문에 쓰지 않는다. "대표학교"로 지칭.
- 자원 참조는 로컬 경로가 아니라 **resource_id**(§6)로만. (공개 저장소에 로컬 경로 노출 금지 — PROVENANCE.md만 예외)
- 이모지 금지. 표는 자유롭게 사용.

## 5. 파일 경로 분할 (disjoint — 자기 경로 밖에 쓰지 마라)

| 에이전트 | 쓰기 허용 경로 |
|---|---|
| W-A | `syllabus/SYLLABUS_HUMAN.md` (단일 파일) |
| W-B | `contents/M1/**` + `contents/M2..M6/README.md` (구조 스텁) |
| W-C | `syllabus/syllabus_agent.json` + `agent_resources/**` |
| 오케스트레이터 | `README.md`, `PROVENANCE.md`, `architecture/**`, `_spec/**` |

## 6. resource_id 체계 (고정 — W-C가 manifest 작성, W-A·W-B는 참조만)

`kb://<묶음>/<항목>` 형식. 핵심 ID:

- `kb://calendar/work_event_candidates_714` — 연간 업무사건 후보 714건 CSV
- `kb://calendar/domain_summary` — 도메인별 업무 요약 (완전 비식별)
- `kb://calendar/monthly_domain_crosstab` — 월×도메인 크로스탭 (완전 비식별)
- `kb://design/dataset_design_doc` — 데이터세트 설계서 (10개 도메인 분류체계)
- `kb://model/l0_l4_conditions_320` — L0–L4 조건 단위 320개
- `kb://model/general_vs_special` — 일반/특수모델 통합 전략 문서
- `kb://causal/work_occurrence_model` — 행정업무 발생원인 모델
- `kb://pipeline/collect_open_go` — 정보공개포털 수집 스크립트
- `kb://pipeline/preprocess_teacher_work` — 파일 전처리 파이프라인
- `kb://pipeline/extract_work_events` — 업무사건 추출 스크립트
- `kb://simulator/app` — 행정업무 프로세스 시뮬레이터 (D-2 스키마, 6차원 루브릭)
- `kb://whitepaper/stage1..stage9` — 백서 9단계 카드
- `kb://validation/teacher_cards` — 교사 검증 카드팩 (TVC-001~)
- `kb://validation/ai_strategy_packet` — AI 전략 검증 패킷 (AIC-0001~0030, 허용 자동화 수준 5단계)

## 7. 스키마 (W-C용 — JSON은 모두 UTF-8, json.load 통과 필수)

### syllabus_agent.json (에이전트용 실라버스)
```json
{
  "schema_version": "0.1.0",
  "course": {"course_id": "SAI-501", "title": "...", "audience": "...", "weeks": 15, "version": "0.1.0"},
  "modules": [
    {
      "module_id": "M1", "title": "...", "weeks": [1,2,3], "core_question": "...",
      "learning_objectives": ["..."],
      "resources": ["kb://calendar/work_event_candidates_714"],
      "activities": [{"activity_id": "M1-A1", "type": "data_analysis", "description": "...", "agent_support": ["course-qa","data-analysis-helper"], "sensitivity": "normal|sensitive", "routing_hint": "local|server|either"}],
      "assessment": {"artifact": "...", "rubric_ref": "kb://validation/..."}
    }
  ],
  "agent_policies": {"raw_data_stays_local": true, "consent_gated_export": true, "sensitive_keywords_route_local": ["성적", "개인정보", "민원", "상담"]}
}
```

### agent_resources/interface.json (인터페이스: 실라버스↔자원↔스킬)
```json
{
  "schema_version": "0.1.0",
  "entrypoints": {"human": "syllabus/SYLLABUS_HUMAN.md", "agent": "syllabus/syllabus_agent.json"},
  "resource_manifest": "agent_resources/resource_manifest.json",
  "skills_dir": "agent_resources/skills/",
  "bindings": [{"module_id": "M1", "skills": ["course-qa", "data-analysis-helper"], "resources": ["kb://calendar/work_event_candidates_714", "kb://calendar/domain_summary"]}]
}
```

### resource_manifest.json — 항목: `{"resource_id", "title", "type"(dataset|document|script|app|card), "deid_status"(full|generalized|source_only), "include_in_package"(true|false), "source_uri"("local-kb://..."), "sha256"("TBD" 허용), "module_tags"(["M1"])}`

### 코스 스킬 5종 (agent_resources/skills/<name>/SKILL.md — Anthropic Agent Skills 관례: YAML frontmatter `name`/`description` + 본문)
`course-qa` · `assignment-scaffold` · `data-analysis-helper` · `rubric-self-check` · `week-planner`

## 8. 모범 예시 (형식 기준 — 이 수준의 구체성으로)

### 예시 A: SYLLABUS_HUMAN.md의 주차 항목 1개
```markdown
### 2주차 — 업무사건 데이터 첫 분석

**이번 주 질문.** 우리 학교에서 1년 동안 일어나는 행정업무는 몇 건이고, 어디에 몰려 있는가?

**수업 흐름 (3시간).**
1. (40분) 강의: 공개 행정문서는 업무의 흔적이다 — 대표학교 연간 업무사건 714건은 어떻게 추출되었나 [kb://design/dataset_design_doc]
2. (80분) 실습: 업무사건 데이터셋[kb://calendar/work_event_candidates_714]을 열어 ①도메인별 분포 ②월별 피크(3월 87건 → 2월 160건) ③내 직무 관련 업무 필터링
3. (40분) 토론: 데이터에 안 보이는 업무는 무엇인가 (가시성 결손 예고)
4. (20분) 과제 안내

**과제.** 자기 소속 학교의 최근 1개월 업무 5건을 같은 스키마(도메인·트리거·담당·시스템)로 기록해 오기. ※개인정보·학교 식별 정보 제거 필수.
**에이전트 활용.** 로컬 비서에게 "내 업무 5건을 10개 도메인으로 분류해줘" — 분류 근거를 반드시 확인할 것.
```

### 예시 B: syllabus_agent.json의 activity 1개
```json
{"activity_id": "M1-A2", "type": "data_analysis", "description": "업무사건 714건 데이터셋에서 도메인별·월별 분포를 분석하고 자기 직무 관련 업무를 필터링한다", "agent_support": ["data-analysis-helper", "course-qa"], "sensitivity": "normal", "routing_hint": "local"}
```

## 9. 검증 (오케스트레이터가 실행 — 통과 못 하면 재작성)

- 모든 `.json`: `python3 -c "import json;json.load(open(f))"` 통과
- `SYLLABUS_HUMAN.md`: 필수 섹션 `# `(제목), `## 강의 개요`, `## 주차별 계획`(15개 주차 헤더), `## 평가`, `## 에이전트(AI 비서) 활용 안내` 존재
- 모듈 ID 교차 일치: HUMAN의 모듈 구성 = agent.json의 modules[].module_id = interface.json bindings — M1~M6 전부
- resource_id는 §6 목록(또는 manifest에 정의된 것)만 사용
- 비식별: 본문에서 특정 학교명 grep 0건
