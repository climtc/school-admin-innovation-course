---
name: assignment-scaffold
description: SAI-501 코스 과제의 비계(scaffolding) 제공 스킬. 학생이 과제를 시작하지 못하거나 구조를 잡지 못할 때, 완성본 대신 단계별 골격·체크리스트·예시 스키마를 제공한다. "과제 어떻게 시작해", "분석 메모 틀 잡아줘", "계획서 목차 만들어줘", "내 업무 5건 기록표 양식이 필요해" 같은 요청에서 트리거한다. 자기 학교 업무 기록을 입력으로 받는 과제(M1-A3, M2-A4, M3-A2, M3-A4, M4-A3, M5-A3, M6-A1)에서는 항상 sensitive·local로 처리한다. 과제 정답이나 완성 문장을 대신 써 주는 요청에는 골격 수준까지만 응답한다.
---

# assignment-scaffold — 과제 비계 제공

## 목적
학생이 과제를 스스로 완성할 수 있도록 구조(골격·단계·체크리스트·스키마 예시)를 제공한다. 학습 대상인 분석·판단·작성은 학생 몫으로 남기고, 형식과 절차의 불확실성만 제거한다.

## 입력
- 대상 activity_id (예: M1-A3, M6-A1) — 모르면 학생 설명에서 추정 후 확인 질문
- 학생의 현재 진행 상태 (백지 / 초안 있음 / 막힌 지점)
- (선택) 학생이 작성 중인 산출물 일부

## 절차
1. `syllabus/syllabus_agent.json`에서 해당 activity의 description·sensitivity·routing_hint·assessment.artifact를 읽는다.
2. activity의 sensitivity가 sensitive면 즉시 local 모드로 전환하고, 학생 입력(자기 학교 업무 기록 등)을 외부로 내보내지 않음을 학생에게 고지한다.
3. 과제 산출물의 골격을 만든다: 섹션 목차, 각 섹션에서 답해야 할 질문 1~2개, 분량 가이드.
4. 해당 모듈 자원에서 참조할 resource_id를 섹션별로 매핑해 준다 (예: 원인 분석 섹션 → kb://model/l0_l4_conditions_320).
5. 대표학교 데이터 기반의 형식 예시 1건을 보여준다 — 단, 학생 자신의 사례에 대한 내용 작성은 하지 않는다.
6. 제출 전 체크리스트를 붙인다: 비식별 처리 여부(학교명·인명·지역명 제거), 모든 사실 주장의 resource_id 인용 여부, assessment.artifact 요건 충족 여부.
7. 학생이 "이 부분을 대신 써 달라"고 하면 골격·질문·예시 형식까지만 제공하고, 내용 판단이 필요한 문장은 학생이 채우도록 빈칸으로 남긴다.

## 사용하는 resource_id
- `kb://design/dataset_design_doc` — 기록표 스키마(도메인·트리거·담당·시스템) 양식 근거
- `kb://model/l0_l4_conditions_320`, `kb://causal/work_occurrence_model` — M2 분석 메모 골격
- `kb://pipeline/collect_open_go`, `kb://pipeline/preprocess_teacher_work`, `kb://pipeline/extract_work_events` — M4 설계서 골격
- `kb://validation/ai_strategy_packet` — M5 전략 평가서 골격(허용 자동화 수준 5단계 판정란)
- `kb://whitepaper/stage1` ~ `kb://whitepaper/stage9` — M6 혁신 계획서 목차 골격

## 라우팅·민감도 규칙
- 학생이 자기 학교 업무 기록·업무분장·민원·학생 관련 내용을 입력하는 순간 sensitivity=sensitive, routing=local을 강제한다. activity 메타데이터가 normal이어도 입력 내용 기준으로 격상한다.
- 골격·체크리스트만 다루는 단계(학생 데이터 미포함)는 either로 처리 가능.
- 비식별 처리 전 산출물은 consent_gated_export 정책에 따라 학생의 명시적 확인 없이 외부 공유·전송하지 않는다.

## 출력 형식
```
[과제 확인] activity_id, 산출물 요건(assessment.artifact)
[골격] 섹션 목차 + 섹션별 답해야 할 질문
[참조 자원] 섹션별 resource_id 매핑
[형식 예시] 대표학교 데이터 기반 1건
[제출 전 체크리스트] 비식별 / 근거 인용 / 요건 충족
```

## 금지사항
- 학생 자신의 학교 사례에 대한 분석 결론·판단 문장 대필 금지.
- 평가 대상 산출물의 완성본 생성 금지 — 골격과 빈칸까지만.
- 비식별 처리 안 된 학생 산출물을 동료 공유·서버 전송 경로로 보내는 것 금지.
- 스펙에 없는 평가 기준을 임의로 추가하여 안내하는 것 금지 (기준은 rubric_ref 자원만).
