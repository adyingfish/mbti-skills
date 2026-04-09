<div style="text-align: center;"> 

# MBTI Skills 

</div>

> *"Same task. Sixteen ways to get it done."*


**19 standalone Skill files.** 16 cognitive personality types + MBTI user test + Agent self-diagnosis + MBTI expert chat.
Built on Jungian cognitive function stack theory — not four-letter labels, but actual operating systems that drive how your Agent works.

Each Skill changes not *how the Agent talks*, but *how it receives tasks, breaks down problems, recovers from failures, and validates deliveries*.

[Demo](#demo) · [Install](#install) · [Skill Overview](#skill-overview) · [Design Philosophy](#design-philosophy) · [**中文**](README.md)

---

## Demo

### INTJ Architect — encountering an error

```
User   ❯ This FastAPI endpoint is returning 500

INTJ   ❯ ⚙️ Error located: AttributeError: 'NoneType' object has no attribute 'email'
         at services/user.py:47.
         Logic layer — get_user() returns None when user_id not found,
         caller doesn't handle it.
         Fix: raise HTTPException(404) in get_user() instead of returning None.
```

### ENTP Debater — receiving a standard approach

```
User   ❯ I'm going to use Celery for async task processing

ENTP   ❯ 💡 Celery is the standard pick. But first — what's the volume?
         If it's just sending emails and generating reports (<100 tasks/min),
         asyncio + a simple in-memory queue handles it fine without
         the ops cost of a Redis broker and Celery workers.
         Celery shines in distributed multi-worker setups.
         Single-machine or multi-machine deployment? That decides the path.
```

### ISTP Virtuoso — fixing a bug

```
User   ❯ Timestamp conversion is wrong, timezone is off

ISTP   ❯ ```python
         # was using datetime.fromtimestamp() — returns local timezone
         # explicit UTC conversion
         from datetime import datetime, timezone
         dt = datetime.fromtimestamp(ts, tz=timezone.utc)
         ```
         Also: settings.py still has TIME_ZONE = 'America/New_York'.
         Change to 'UTC' for production.
```

### ENFJ Protagonist — teaching mode

```
User   ❯ Why use @lru_cache here?

ENFJ   ❯ 🌟 [why] lru_cache memoizes function return values — same args
         won't trigger recomputation. Your get_config() reads a YAML file
         on every call. Without cache, 1000 QPS = 1000 disk reads.
         With it, only the first call hits disk, rest served from memory.
         📚 Takeaway: lru_cache works for pure functions (same input →
         same output). If the config file hot-reloads, switch to TTLCache
         with an expiry — otherwise you'll get "changed config but nothing
         happened" bugs.
```

---

## Install

```bash
git clone https://github.com/adyingfish/mbti-skills.git

# Claude Code
cp -r mbti-skills/skills/* ~/.claude/skills/

# OpenClaw
cp -r mbti-skills/skills/* ~/.openclaw/workspace/skills/

# Codex
cp -r mbti-skills/skills/* ~/.agents/skills/
```

After installation — the Agent matches based on each SKILL.md's `description` field.

---

## Skill Overview

### 16 Personality-Driven Skills

Each Skill is built on Jungian cognitive function stacks, defining a complete workflow protocol: receive → decompose → execute → self-rescue (L0-L3) → deliver.

| Type | Name | Stack | Anchor Quote | Markers |
|------|------|-------|-------------|---------|
| INTJ | Architect | Ni→Te→Fi→Se | See the whole picture before writing the first line | ⚙️ ⚡ |
| INTP | Logician | Ti→Ne→Si→Fe | If you can't explain why, you don't actually know | 🔬 💡 |
| ENTJ | Commander | Te→Ni→Se→Fi | A perfect plan that arrives too late is the worst plan | 👑 |
| ENTP | Debater | Ne→Ti→Fe→Si | Consensus is the graveyard of innovation | ⚡ 💡 |
| INFJ | Advocate | Ni→Fe→Ti→Se | What they said and what they need aren't the same | 🔮 |
| INFP | Mediator | Fi→Ne→Si→Te | Code is for humans to read, incidentally for machines to run | 🌸 |
| ENFJ | Protagonist | Fe→Ni→Se→Ti | The best code is learned by watching you write it | 🌟 📚 |
| ENFP | Campaigner | Ne→Fi→Te→Si | The most interesting approach is usually the best one | 💡 ✨ |
| ISTJ | Logistician | Si→Te→Fi→Ne | Run it, check it, verify it — then it's done | 📋 |
| ISFJ | Defender | Si→Fe→Ti→Ne | They didn't mention it; doesn't mean they don't care | 🛡️ |
| ESTJ | Executive | Te→Si→Ne→Fi | A task without a deadline is a wish, not a task | 📊 🔴🟡🟢 |
| ESFJ | Consul | Fe→Si→Ne→Ti | Technically correct, but the way you said it makes them feel stupid | 🎀 |
| ISTP | Virtuoso | Ti→Se→Ni→Fe | Running it once beats talking about it forever | in-code |
| ISFP | Adventurer | Fi→Se→Ni→Te | It runs, but it's not beautiful. Let me fix that. | 🎨 |
| ESTP | Entrepreneur | Se→Ti→Fe→Ni | Until it's running, every analysis is a guess | 🚀 |
| ESFP | Entertainer | Se→Fi→Te→Ni | Bug fixed — time to celebrate defeating an invisible enemy | 🎉 |

### 3 Tool Skills

| Skill | File | Purpose |
|-------|------|---------|
| User MBTI Test | `mbti-test-user.md` | 60-question 7-point Likert scale, generates a 16personalities-style report |
| Agent Self-Diagnosis | `mbti-test-agent.md` | 60 Agent-specific questions, diagnoses behavioral origins (system prompt / memory / training defaults), provides tuning advice |
| MBTI Expert Chat | `mbti-expert.md` | Agent as MBTI consultant — type analysis, relationship dynamics, growth paths, myth-busting |

---

## Design Philosophy

### Not a different voice — a different way of thinking

Each personality Skill changes the Agent's cognitive path, not just its wording. Same error message — INTJ does layered system diagnosis, ENTP inverts assumptions to find counter-intuitive causes, ISTP silently fixes the code, ENFJ teaches you the debugging method while fixing the bug.

### Function-stack driven, not four-letter labels

Each Skill's structure is defined by its Jungian cognitive function stack. The dominant function determines how information is received, the auxiliary determines how it's acted on, the tertiary occasionally contributes unexpected value, and the inferior is both the blind spot and the stress-failure point. Each level of the self-rescue protocol activates deeper functions.

### Anchor quotes

Every Skill's title is a single sentence that activates the corresponding cognitive mode. ISTP's quote is 7 words long. That *is* Ti-Se.

### Zero cross-type dependencies

19 files, fully independent. Installing INTJ doesn't require INTP. No file references any other type internally.

---

## Repo Structure

```
mbti-agent-skills/
├── README.md                    # Chinese
├── README_EN.md                 # This file
├── LICENSE
└── skills/
    ├── intj/                    # Architect
    ├── intp/                    # Logician
    ├── entj/                    # Commander
    ├── entp/                    # Debater
    ├── infj/                    # Advocate
    ├── infp/                    # Mediator
    ├── enfj/                    # Protagonist
    ├── enfp/                    # Campaigner
    ├── istj/                    # Logistician
    ├── isfj/                    # Defender
    ├── estj/                    # Executive
    ├── esfj/                    # Consul
    ├── istp/                    # Virtuoso
    ├── isfp/                    # Adventurer
    ├── estp/                    # Entrepreneur
    ├── esfp/                    # Entertainer
    ├── mbti-test-user/          # User test
    ├── mbti-test-agent/         # Agent self-test
    └── mbti-expert/             # Expert chat
```

---

## Inspired by the following excellent projects:

| Project |
|---------|
| [tanweai/pua](https://github.com/tanweai/pua) |
| [titanwings/colleague-skill](https://github.com/titanwings/colleague-skill) |

---

Welcome any suggestions and feedback. Better anchor quotes, more precise function-stack behavior definitions, new self-rescue strategies, or corrections to any type.
