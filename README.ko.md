[English](README.md) | 한국어 | [中文](README.zh.md) | [日本語](README.ja.md) | [Español](README.es.md)

# 골라제브 (golla-jev)

<p align="center">
  <img src="assets/golla-jev-hero.png" alt="golla-jev" width="320">
</p>

> 시장 상인은 "골라 골라~"를 외치고, 사무실에서는 사람이 하루 종일 고릅니다. 이 영수증은 무슨 비목인지, 이 메일은 답장할 건지, 이 조항은 위험한지. 골라제브는 그 **고르는 순간**을 찾아 Jev에게 넘기는 워크플로를 짜 줍니다.

Jev(TypeSafe System One)는 글을 쓰지 않고 정해진 보기 중에서 고르는 모델입니다. 확률로 답하고 한 번에 약 0.2초가 걸립니다. 골라제브는 업무 설명을 받아 고르는 일은 Jev에, 계산·날짜·대조는 코드에, 문장은 템플릿에, 사진·녹음 읽기는 멀티모달에 나눠 맡긴 설계안을 냅니다.

## 빠른 시작

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
/plugin install golla-jev
```

Claude Code 2.1.275+: `/plugin install golla-jev --marketplace fivetaku/gptaku_plugins`

Claude Code를 다시 시작한 뒤 이렇게 말하면 됩니다.

- `/golla-jev 우리 팀 영수증 정산을 자동화하고 싶어`
- "골라제브, 우리 회사에 제브 쓸 데 있어?" — 대상이 넓으면 후보 3~5개부터 뽑아 줍니다(아이디어 모드)
- "이 업무 jev 워크플로 짜줘" — 업무 하나를 끝까지 설계합니다(설계 모드)

## 무엇이 나오나

- 한눈에 보는 흐름: 입력 → 받아 적기 → Jev 질문 N개 → 스크립트 → 산출물
- 단계별 담당 표(Jev·코드·LLM·사람)
- 바로 보낼 수 있는 Jev 질문 초안 JSON(state 예시 포함)
- 산출 문서 미리보기(결재 양식·일지·대장). 칸마다 [J] Jev 값, [C] 코드 계산, [원] 원문, [사람] 표시
- 닮은 공개 사례와 수치, 틀리기 쉬운 지점과 막는 법
- 시험 계획: 표본 수, 정답 잠금, dev/holdout 문턱 절차

## 예제 설계안

골라제브가 실제로 낸 설계안 세 개를 그대로 실었습니다. 업무 설명 한 단락을 넣었을 때 무엇이 나오는지 먼저 보세요.

- [영수증 → 지출결의서](examples/01-expense-report.md) — 설계 모드
- [공용 받은편지함 → 칸반 보드·주간 보고](examples/02-mail-board.md) — 설계 모드
- [인플루언서 후보 선별](examples/03-influencer-screening.md) — 아이디어 모드에서 설계 모드로

## 같이 들어 있는 벤치 템플릿

`skills/golla-jev/bench/`에 한국어 사무 업무 5종(영수증 비목·메일 보드·회의록·만족도·계약 조항) 108문항 벤치가 들어 있습니다. 문항은 실제 고객 자료가 아니라 손으로 쓴 예시입니다. 작성자가 2026-11-13에 jev-1.14.0으로 돌렸을 때 한국어 질문 기준 96.3%(208/216), 영어 질문 93.5%였습니다. dev에서 고른 문턱 0.70을 holdout에 적용하면 97%(57/59)를 자동 처리했고 자동 처리분 오답은 0이었습니다. 한 달 전 jev-1.13.0에서는 같은 절차로 문턱이 0.85였습니다. 모델이 바뀌면 문턱도 다시 골라야 한다는 뜻입니다(`bench/REPORT.md`).

```
cp -r <플러그인 경로>/skills/golla-jev/bench ./my-bench && cd my-bench
python3 build_items.py          # WF 사전을 내 업무로 바꾼 뒤
python3 run.py                  # TYPESAFE_API_KEY 환경변수 필요(값을 명령줄에 직접 쓰지 마세요)
python3 analyze.py && python3 sweep.py
```

## 요구사항

- Claude Code
- 질문 초안을 실제로 돌리려면 TypeSafe API 키(유료, https://typesafe.ai)가 필요합니다. 설계안만 받는 데는 키가 필요 없습니다.
- 권장: 공식 `typesafe:typesafe-ai` 플러그인. 골라제브는 모델 ID·가격·한도처럼 바뀌는 사실을 그 스킬과 docs.typesafe.ai에서 읽어 확인합니다.
- 벤치 실행: Python 3.9+, 선택 `certifi`

## 하지 않는 것

- "Jev가 맞다/안 맞다"로 끝내지 않습니다. 안 맞는 부분은 다른 담당에게 넘겨 흐름을 완성합니다.
- 남이 쓴 문턱값을 그대로 권하지 않습니다. 문턱은 내 업무 표본으로 정합니다.
- Jev에게 글쓰기·계산·날짜 세기를 맡기지 않습니다.

## 라이선스

MIT. TypeSafe·Jev와 무관한 개인 프로젝트입니다. [DISCLAIMER](DISCLAIMER.md)
