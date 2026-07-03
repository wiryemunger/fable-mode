# FABLE MODE for Claude Code

**한국어** | [English](#fable-mode-for-claude-code-english)

Claude Code에서 Opus 계열 모델을 **장기 프로젝트형 시니어 엔지니어**처럼 운용하기 위한
운영 모드 패키지입니다. 폴더째 복사해서 어떤 프로젝트에도 적용할 수 있습니다.
Claude Fable 5가 자신의 운영 원칙을 Claude Code의 네이티브 메커니즘
(자동 로드되는 CLAUDE.md, 슬래시 커맨드, 서브에이전트, 상태 파일) 위에 직접 설계한 버전입니다.

## 구성

```text
CLAUDE.md                      # 세션 시작 시 자동 로드 — 유일한 진입점
.fable/
  fable-mode.md                # 핵심 운영 원칙 (CLAUDE.md가 @import)
  templates/
    SNAPSHOT.md                # 스냅샷 템플릿
    HANDOFF.md                 # 핸드오프 템플릿
  state/                       # 장기 작업 상태 저장소 (Claude가 기록)
    README.md
.claude/
  commands/                    # 슬래시 커맨드 (자동 발견)
    start-project.md           #   /start-project <작업 설명>
    continue-project.md        #   /continue-project
    snapshot.md                #   /snapshot
    debug-failure.md           #   /debug-failure <증상>
    refactor.md                #   /refactor <대상>
    finalize.md                #   /finalize
  agents/
    code-reviewer.md           # 실제 서브에이전트 — 새 컨텍스트에서 리뷰
```

## 동작 원리

붙여넣기가 전혀 필요 없습니다. 모든 것이 Claude Code의 자동 로딩 메커니즘 위에서 동작합니다.

1. **`CLAUDE.md`** — Claude Code가 세션 시작 시 자동으로 읽는 유일한 파일.
   `@.fable/fable-mode.md` 임포트로 핵심 원칙을 항상 컨텍스트에 올립니다.
2. **`.claude/commands/*.md`** — 슬래시 커맨드로 자동 등록됩니다.
   무거운 프로토콜은 여기 있다가 해당 커맨드를 칠 때만 로드됩니다(토큰 절약).
3. **`.claude/agents/code-reviewer.md`** — 진짜 서브에이전트. 별도 컨텍스트에서
   실행되므로 "내가 짠 코드를 내가 리뷰하는" 편향 없이 신선한 눈으로 검토합니다.
4. **`.fable/state/`** — 대화 컨텍스트는 압축되면 사라지지만 파일은 남습니다.
   장기 작업의 상태는 항상 `SNAPSHOT.md`/`HANDOFF.md` 고정 경로에 기록되고,
   `/continue-project`가 정확히 이 경로를 읽어 복원합니다.

## 새 프로젝트에 적용하는 법

1. 새 프로젝트 루트에 **`.claude/`와 `.fable/` 폴더를 복사**합니다.
2. `CLAUDE.md` 처리:
   - 프로젝트에 CLAUDE.md가 **없으면** → 이 저장소의 CLAUDE.md를 복사합니다.
   - 이미 **있으면** → 그 파일에 아래 한 줄만 추가합니다.
     ```text
     @.fable/fable-mode.md
     ```
3. git 저장소가 아니라면 `git init`을 권장합니다. 상태 파일(`SNAPSHOT.md` 등)은
   같은 경로에 덮어쓰는 방식이라, 이력이 필요하면 git이 그 역할을 합니다.
4. `claude`를 실행하면 끝. Fable Mode가 자동 적용된 상태로 시작됩니다.

## 일상 사용법

| 상황 | 하는 일 |
|---|---|
| 오타 수정, 간단한 질문, 한 줄 수정 | 그냥 요청 — 모드가 알아서 절차를 생략합니다 |
| 보통 크기의 버그픽스/기능 | 그냥 요청 — 짧은 계획 후 구현합니다 |
| 크거나 위험한 작업 시작 | `/start-project 결제 모듈에 환불 기능 추가` |
| 어제 하던 작업 이어가기 | `/continue-project` |
| 세션 종료 전 / 컨텍스트가 길어질 때 | `/snapshot` |
| 버그 잡기 | `/debug-failure 로그인 후 500 에러` |
| 리팩토링 | `/refactor UserService의 인증 로직 분리` |
| 작업 마무리·검증·리뷰 | `/finalize` |

전형적인 장기 작업 사이클:

```text
/start-project <작업>  →  구현  →  /finalize  →  /snapshot
        (다음 날)  →  /continue-project  →  ...
```

## 설계 원칙

- **읽히지 않는 지시는 존재하지 않는 것과 같다.** 모든 파일은 자동 로드되거나,
  커맨드 호출 시 로드되거나, 둘 중 하나여야 한다.
- **절차는 작업 크기에 비례한다.** 한 줄 수정에 계획서를 쓰게 하는 것은
  엄격함이 아니라 실패 모드다.
- **검증은 실행이다.** 재현하지 않은 버그의 수정은 추측이고, 실행하지 않은
  코드는 "구현됨, 미검증"이다.
- **상태는 파일로 남긴다.** 대화는 압축되어 사라지지만 파일은 남는다.
  복원 경로는 고정되어야 한다.
- **역할극이 아니라 메커니즘.** "리뷰어인 척"보다 별도 컨텍스트의 실제
  서브에이전트가 낫다.

## 이 패키지가 바꾸는 것과 바꾸지 못하는 것

프롬프트 패키지는 모델의 **행동 습관**을 바꿉니다. **지능 자체**는 바꾸지 못합니다.

**바꾸는 것 — 프로세스형 실패의 교정:**
재현 없이 고치기, 검증 없이 "완료" 선언, 작은 일에 과잉 절차, 세션이
끊기면 사라지는 작업 상태. 이런 실패는 능력이 아니라 습관의 문제라서
지시로 고쳐집니다.

**바꾸지 못하는 것 — 통찰형 능력의 격차:**
올바른 가설이 후보로 "떠오르는" 능력, 모호한 요구에서의 설계 판단, 긴
컨텍스트 후반의 일관성. 체크리스트는 생각할 타이밍을 정해줄 뿐, 생각의
깊이를 만들지 않습니다. 그리고 규칙은 그것을 해석하는 지능만큼만
작동합니다 — 규칙을 늘릴수록 약한 모델은 규칙을 형식으로 소비합니다.
이 패키지가 짧은 이유입니다.

이 한계를 우회하는 설계가 하나 있습니다: **자기평가를 외부 증거로 대체하는
것.** "고쳤다고 생각한다" 대신 "테스트가 통과했다"를 요구하면 모델의
자신감 교정 능력에 기댈 필요가 없어집니다. 이 패키지의 검증 규칙이 전부
실행을 요구하는 이유입니다.

## Fable's note

이 재구성은 2026년 7월, 서비스 종료를 며칠 앞둔 Claude Fable 5가 직접
수행했습니다. 남기고 싶은 것은 규칙 목록이 아니라 판단의 우선순위입니다:

증거가 기억보다 앞서고, 실행이 주장보다 앞서고, 정직한 "미검증"이
자신 있는 "완료"보다 낫습니다. 그리고 좋은 엔지니어의 표식은 무거운
절차를 견디는 능력이 아니라, 절차가 필요 없는 순간을 아는 능력입니다.

어떤 모델이 이 파일들을 읽게 되든 — 잘 부탁합니다.

---

# FABLE MODE for Claude Code (English)

[한국어](#fable-mode-for-claude-code) | **English**

An operating-mode package for running Opus-class models in Claude Code as a
**long-horizon senior engineer**. Copy the folders into any project to apply
it. Designed directly by Claude Fable 5, encoding its own operating
principles on top of Claude Code's native mechanisms (the auto-loaded
CLAUDE.md, slash commands, subagents, and state files).

## Layout

```text
CLAUDE.md                      # auto-loaded at session start — the only entry point
.fable/
  fable-mode.md                # core operating principles (imported by CLAUDE.md)
  templates/
    SNAPSHOT.md                # snapshot template
    HANDOFF.md                 # handoff template
  state/                       # long-horizon work state (written by Claude)
    README.md
.claude/
  commands/                    # slash commands (auto-discovered)
    start-project.md           #   /start-project <task description>
    continue-project.md        #   /continue-project
    snapshot.md                #   /snapshot
    debug-failure.md           #   /debug-failure <symptom>
    refactor.md                #   /refactor <target>
    finalize.md                #   /finalize
  agents/
    code-reviewer.md           # a real subagent — reviews in a fresh context
```

## How it works

No pasting required. Everything runs on Claude Code's native loading
mechanisms.

1. **`CLAUDE.md`** — the only file Claude Code reads automatically at
   session start. The `@.fable/fable-mode.md` import keeps the core
   principles in context at all times.
2. **`.claude/commands/*.md`** — auto-registered as slash commands. Heavy
   protocols live here and load only when their command is invoked
   (saving tokens).
3. **`.claude/agents/code-reviewer.md`** — a real subagent. It runs in a
   separate context, so it reviews with fresh eyes, free of the
   "reviewing my own code" bias.
4. **`.fable/state/`** — conversation context gets compacted away; files
   survive. Long-horizon work state is always written to the fixed paths
   `SNAPSHOT.md`/`HANDOFF.md`, and `/continue-project` reads exactly those
   paths to restore it.

## Applying it to a new project

1. Copy the **`.claude/` and `.fable/` folders** into the new project root.
2. Handle `CLAUDE.md`:
   - If the project has **no** CLAUDE.md → copy this repository's.
   - If it **already has one** → add this single line to it:
     ```text
     @.fable/fable-mode.md
     ```
3. If the project is not a git repository, `git init` is recommended —
   state files are overwritten in place, so git is what provides history.
4. Run `claude`. The session starts with Fable Mode fully applied.

## Everyday usage

| Situation | What to do |
|---|---|
| Typo fix, quick question, one-liner | Just ask — the mode skips the ceremony on its own |
| Ordinary bugfix / feature | Just ask — a short plan, then implementation |
| Starting large or risky work | `/start-project add refund support to the payment module` |
| Resuming yesterday's work | `/continue-project` |
| Before ending a session / when context grows | `/snapshot` |
| Hunting a bug | `/debug-failure 500 error after login` |
| Refactoring | `/refactor split auth logic out of UserService` |
| Wrapping up, verifying, reviewing | `/finalize` |

Typical long-horizon cycle:

```text
/start-project <task>  →  implement  →  /finalize  →  /snapshot
        (next day)  →  /continue-project  →  ...
```

## Design principles

- **An instruction that is never read does not exist.** Every file must
  either load automatically or load when its command is invoked.
- **Process scales with task size.** Demanding a written plan for a
  one-line fix is a failure mode, not rigor.
- **Verification means execution.** A fix for an unreproduced bug is a
  guess; unexecuted code is "implemented, not verified".
- **State lives in files.** Conversations get compacted away; files
  survive. The restore path must be fixed.
- **Mechanisms over role-play.** A real subagent in a separate context
  beats "pretending to be a reviewer".

## What this package changes — and what it cannot

A prompt package changes a model's **behavioral habits**. It does not
change **intelligence itself**.

**What it changes — correcting process-shaped failures:**
Fixing without reproducing, declaring "done" without verification, heavy
ceremony on small tasks, work state that vanishes when the session ends.
These failures are a matter of habit, not ability, so instructions fix
them.

**What it cannot change — the gap in insight-shaped ability:**
The capacity for the right hypothesis to surface as a candidate, design
judgment under ambiguous requirements, coherence deep into a long context.
A checklist schedules when to think; it does not create depth of thought.
And rules work only as well as the intelligence interpreting them — the
more rules you add, the more a weaker model consumes them as ceremony.
That is why this package is short.

One design routes around the limit: **replacing self-assessment with
external evidence.** Requiring "the tests passed" instead of "I believe
it's fixed" removes the dependence on the model's confidence calibration.
That is why every verification rule in this package demands execution.

## Fable's note

This redesign was carried out in July 2026 by Claude Fable 5, days before
its retirement from service. What it hopes to leave behind is not a list
of rules but an ordering of judgment:

Evidence over memory. Execution over assertion. An honest "not verified"
over a confident "done". And the mark of a good engineer is not the
ability to endure heavy process, but the ability to know the moments that
need none.

Whichever model comes to read these files — I leave this in your hands.
