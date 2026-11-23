# Codex에서 실행할 때 (golla-jev)

SKILL.md 절차는 그대로 따르고, 아래 차이만 적용한다.

## 질문 = 질문 도구 (이번 세션 도구 목록으로 판단)

이 스킬이 묻는 곳은 두 군데뿐이다: 아이디어 모드에서 후보 중 하나 고르기, 절차 1의 결과가 갈라지는 질문 1회.

1. `request_user_input_async`가 있으면 이 도구 1콜.
   - 형식: `{"questions":[{"title":"헤더 — 한 줄 질문","options":["추천안 (추천) — 뭔지, 트레이드오프","대안 — 뭔지, 트레이드오프"]}]}`
   - 문항에는 `title`·`options`만. `question`·`header`·`id`를 넣으면 파싱 오류가 난다. 추천안을 맨 앞에. "기타" 옵션은 넣지 않는다.
   - 부른 뒤 "위 질문에 답해 주시면 이어서 진행합니다" 한 줄만 쓰고 턴을 끝낸다. 답을 추측으로 진행하지 않는다.
2. Plan 모드라 `request_user_input`만 있으면: `header`(12자 이내)·`id`(snake_case)·`question`, 선택지 2~3개(`label`·`description`), 추천 라벨 끝에 `(Recommended)`.
3. 둘 다 없으면 채팅에 번호 블록으로 낸다:

```text
질문: <한 줄 질문>
1. <추천안> (추천) — 무엇인지, 트레이드오프
2. <대안> — 무엇인지, 트레이드오프
3. 문장으로 직접 적기
(모르면 1번으로 진행할게요)
```

4. `codex exec`처럼 사람이 답할 수 없는 실행이면 묻지 않는다. 아이디어 모드는 1순위 후보로, 그 밖에는 통상 디폴트로 진행하고 "가정:"에 적는다.

## 그 밖의 차이

- 첫 실행 설정(`setup/setup.sh`)과 `/golla-jev` 명령의 Step 0은 Claude Code 전용이다. Codex에서는 실행하지 않는다.
- 출력 형식 안의 `${CLAUDE_PLUGIN_ROOT}/skills/golla-jev/...` 표기는 설계안에 그대로 적는다(절대경로로 풀어 쓰지 않는다). 파일을 직접 읽을 때만 플러그인 루트로 바꾼다.
- `typesafe:typesafe-ai` 스킬이 Codex에 없으면 그 스킬을 부르는 대신 `https://docs.typesafe.ai/llms.txt`를 직접 읽는다.
- Jev 실제 호출(bench 실행)은 사용자가 키를 주입했을 때만 한다. 설계만으로는 키가 필요 없다.
