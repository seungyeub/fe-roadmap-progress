# 🧭 FE Roadmap – Progress Overview

> 이 레포지토리는 **프론트엔드 로드맵 학습 과정의 실제 진행 상황**을 기록하고 관리하기 위한 공간.
> 계획(Plan)은 [`fe-roadmap-blueprint`](https://github.com/seungyeub/fe-roadmap-blueprint), 실험(Lab)은 [`fe-roadmap-lab`](https://github.com/seungyeub/fe-roadmap-lab),
> **실제 공부 기록과 진도 관리(Progress)는 이 레포지토리**에서 담당.

---

## 1. 레포지토리 목적

- 프론트엔드 로드맵 학습 과정을 **날짜/주차 단위로 기록**한다.
- 어떤 주제를 언제, 어느 정도까지 공부했는지 **한눈에 파악**한다.
- 취업 준비 기간 동안의 학습 과정을 **포트폴리오처럼 정리**해 둔다.
- 주간/월간 회고를 통해 **학습 방법을 계속 개선**한다.

---

## 2. 관련 레포지토리

- 📘 **fe-roadmap-blueprint**
  - 이론, 개념, 정리 문서가 들어 있는 **설계용 레포**
  - “무엇을 공부할지(What)”를 정의
- 🧪 **fe-roadmap-lab**
  - 작은 실험, 미니 프로젝트, 코드 연습이 들어 있는 **실험용 레포**
  - “어떻게 구현할지(How)”를 연습
- 📈 **fe-roadmap-progress (현재 레포)**
  - 일일/주간 학습 기록, 진행률, 회고가 들어 있는 **진행 관리 레포**
  - “얼마나 진행됐는지(How far)”를 추적

---

## 3. 폴더 구조 요약

```bash
00_Guides/
  ├── roadmap_overview.md      # 이 문서 (전체 사용 설명서)
  ├── study_schedule.md        # 장·단기 학습 일정/계획
  ├── progress_table.md        # 섹션별 진행 현황 요약 표
  ├── _templates/
  │   ├── daily_log_template.md    # 일일 학습 로그 템플릿
  │   └── weekly_summary_template.md # 주간 회고 템플릿
  └── study_log/
      ├── YYYY-MM-DD.md        # 날짜별 학습 로그 (Day1, Day2, ...)
```

- study_schedule.md
  - “이번 주/다음 주에 무엇을 할지” 같은 계획 중심 문서
- progress_table.md
  - CS, JS, TS, React, 프로젝트 등 큰 단위 섹션별 진행 상태를 한 번에 보는 대시보드
- study_log/
  - 실제로 그날 공부한 내용, 느낀 점, 회고를 적는 파일 모음

---

## 4. 학습 진행 방식 (추천 워크플로우)

### 1. 주 단위 계획 세우기
- 00_Guides/study_schedule.md에서
    - 이번 주에 집중할 섹션(예: JS 기본, TS 기본, React Hooks)을 정하고
    - 주차별 목표를 업데이트한다.

### 2. 하루 공부 시작 전
- 00_Guides/_templates/daily_log_template.md 내용을 복사해  00_Guides/study_log/YYYY-MM-DD.md 파일을 새로 만든다.
- 오늘의 **학습 시간 목표, 주요 할 일(To-do)**을 먼저 적는다.

### 3. 공부하면서
- study_log/오늘날짜.md에
  - 배운 내용 요약
  - 코드/예제 링크
  - 막힌 부분, 추가로 복습할 부분을 계속 적어 나간다.

### 4. 하루 마무리
- study_log/오늘날짜.md 하단에
  - 오늘의 만족도
  - 내일 할 일
  - 느낀 점/회고를 기록한다.
- 해당 날짜에 진도가 나간 섹션들을 progress_table.md에서 진행률/상태를 업데이트한다.

### 5. 주간 회고

- 한 주가 끝나면 weekly_summary_template.md를 복사해
study_log/weekly_YYYY-WW.md 같은 파일로 만들어
  - 이번 주에 무엇을 달성했는지
  - 어떤 부분이 어려웠는지
  - 다음 주 계획 수정 사항을 정리한다.
- study_schedule.md의 다음 주 계획을 현실에 맞게 조정한다.

---

## 5. Day 번호 규칙
- Day1 기준 날짜: 2025-10-30
- 파일명은 날짜 기준으로 관리하고, Day 번호는 문서 안에서만 사용한다.
  - 예: 2025-10-30.md 문서 상단에 # Day 1 – 2025-10-30처럼 표기
- 날짜와 Day 번호가 헷갈리면, 날짜를 기준으로 관리하고 Day는 부제처럼 생각한다.

---

## 6. 상태(Status) 표기 규칙 (progress_table.md에서 사용)

`TODO` : 아직 시작하지 않음
`DOING` : 현재 진행 중
`DONE` : 1회 학습/정리 완료
`REVIEW` : 한 번 했지만 복습 필요 상태

가능하면 한 섹션이 `DONE`이 되면
- 관련 정리 문서는 [`fe-roadmap-blueprint`](https://github.com/seungyeub/fe-roadmap-blueprint)에,
- 관련 실습 코드는 [`fe-roadmap-lab`](https://github.com/seungyeub/fe-roadmap-lab)에 업데이트하는 것을 목표로 함.

---

## 7. 앞으로 유지/관리 방법

- 새로운 주제를 공부하기 시작할 때
  - progress_table.md에 섹션 행 추가
- 일일 공부를 할 때마다
  - study_log/ 아래에 새 파일 추가
- 주간 계획이 바뀔 때마다
  - study_schedule.md 업데이트 (너무 자주가 아니라 주 1회 정도를 추천)

**이 문서는 필요할 때마다 학습 방식이 바뀌면 함께 업데이트 필요.**