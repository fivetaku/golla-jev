---
name: golla-jev
description: 골라제브 — 업무·아이디어를 말하면 사람이 고르는 순간을 Jev에게 넘기는 워크플로를 설계한다(질문 JSON·문서 미리보기·시험 계획).
argument-hint: "[업무나 아이디어 설명, 선택]"
allowed-tools:
  - Read
  - Bash
  - Write
---

<!-- first-run setup + star opt-in. AskUserQuestion must NOT be in frontmatter allowed-tools. -->
**Step 0 — run once at the very start, before anything else:** run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" ask`. If its output starts with `STAR_ASK`, immediately call the **AskUserQuestion** tool once, with the question and options phrased **in the user's language**: prefer the current conversation's language if it is evident; otherwise fall back to the language code that follows `STAR_ASK` in the output (`ko`→Korean, `ja`→Japanese, `en`→English). Never default to Korean blindly.
- header: a short localized "GitHub Star" label
- question: ask whether they'd like to give this plugin (and the gptaku-plugins marketplace) a GitHub ⭐ to support it — note it is optional and every feature works either way
- options: exactly two — (1) yes, star it → then run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" star yes`; (2) no thanks → then run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" star no`

If the output is empty, just continue silently. Do not narrate beyond the question itself.

# /golla-jev — 골라제브

Read `${CLAUDE_PLUGIN_ROOT}/skills/golla-jev/SKILL.md` and follow it.

사용자 요청(비어 있을 수 있음): $ARGUMENTS

- 요청이 비어 있으면 어떤 업무나 아이디어에 Jev를 붙이고 싶은지 한 줄로 묻는다.
- 대상이 넓으면 아이디어 모드, 업무 하나가 정해졌으면 설계 모드로 진행한다.
