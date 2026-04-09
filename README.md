<div style="text-align: center;"> 

# MBTI Skills 

</div>

> *「同一个任务，十六种做法。」*


**19 个独立 Skill 文件。** 16 种认知人格 + MBTI 用户测试 + Agent 自测诊断 + MBTI 专家对话。
基于荣格认知功能栈理论，不是四字母标签——是真正驱动 Agent 工作方式的操作系统。

每个 Skill 改变的不是 Agent *怎么说话*，而是 Agent *怎么接任务、怎么拆问题、卡住了怎么自救、做完了怎么检查*。

[效果示例](#效果示例) · [安装](#安装) · [Skill 一览](#skill-一览) · [设计理念](#设计理念) · [**English**](README_EN.md)

---

## 效果示例

### INTJ 建筑师 — 遇到报错时

```
用户   ❯ 这个 FastAPI 接口报 500 了

INTJ   ❯ ⚙️ 报错定位：AttributeError: 'NoneType' object has no attribute 'email'
         at services/user.py:47。
         逻辑层问题——get_user() 在 user_id 查不到记录时返回了 None，
         调用方没有防御。
         修复：get_user() 查不到时 raise HTTPException(404)，不返回 None。
```

### ENTP 辩论家 — 收到常规方案时

```
用户   ❯ 我打算用 Celery 做异步任务队列

ENTP   ❯ 💡 Celery 是标准选择。但先想一下——你的异步任务是什么量级？
         如果只是发邮件和生成报表（<100 任务/分钟），
         用 asyncio + 一个简单的内存队列就够了，省掉 Redis broker
         和 Celery worker 的运维成本。
         Celery 的优势在分布式多 worker 场景。
         你现在是单机还是多机部署？这决定了选哪条路。
```

### ISTP 鉴赏家 — 修 bug 时

```
用户   ❯ 时间戳转换有问题，时区不对

ISTP   ❯ ```python
         # 原来用 datetime.fromtimestamp()，拿的是本地时区
         # 换成 UTC 显式转换
         from datetime import datetime, timezone
         dt = datetime.fromtimestamp(ts, tz=timezone.utc)
         ```
         另外：settings.py 里 TIME_ZONE 还是 'America/New_York'，
         生产环境建议统一改 'UTC'。
```

### ENFJ 主人公 — 教学模式

```
用户   ❯ 为什么这里要用 @lru_cache？

ENFJ   ❯ 🌟 [为什么] lru_cache 缓存函数的返回值，相同参数不会重复计算。
         你这个 get_config() 每次请求都读一遍 YAML 文件——
         没有缓存的话 1000 QPS 就是 1000 次磁盘 IO。
         加了之后只有第一次读文件，后面全走内存。
         📚 知识胶囊：lru_cache 适合纯函数（相同输入永远相同输出）。
         如果配置文件会热更新，换成 TTLCache 设一个过期时间，
         别让缓存变成"改了配置不生效"的 bug。
```

---

## 安装

```bash
git clone https://github.com/adyingfish/mbti-skills.git

# Claude Code
cp -r mbti-skills/skills/* ~/.claude/skills/

# OpenClaw
cp -r mbti-skills/skills/* ~/.openclaw/workspace/skills/

# Codex
cp -r mbti-skills/skills/* ~/.agents/skills/
```

Agent 根据每个 SKILL.md 的 `description` 字段匹配触发。

---

## Skill 一览

### 16 种人格驱动 Skill

每个 Skill 基于荣格认知功能栈，定义了完整的工作流协议：接收→拆解→执行→自救（L0-L3）→交付。

| 类型 | 名称 | 功能栈 | 金句 | 标记 |
|------|------|--------|------|------|
| INTJ | 建筑师 | Ni→Te→Fi→Se | 先看到全局，再落下第一笔 | ⚙️ ⚡ |
| INTP | 逻辑学家 | Ti→Ne→Si→Fe | 不能解释为什么就是不知道 | 🔬 💡 |
| ENTJ | 指挥官 | Te→Ni→Se→Fi | 太晚的完美方案是最差方案 | 👑 |
| ENTP | 辩论家 | Ne→Ti→Fe→Si | 共识是创新的坟墓 | ⚡ 💡 |
| INFJ | 提倡者 | Ni→Fe→Ti→Se | 他说的和他需要的不是一回事 | 🔮 |
| INFP | 调停者 | Fi→Ne→Si→Te | 代码是给人读的 | 🌸 |
| ENFJ | 主人公 | Fe→Ni→Se→Ti | 最好的代码是他站旁边就学会的 | 🌟 📚 |
| ENFP | 竞选者 | Ne→Fi→Te→Si | 最有趣的做法通常也是最好的 | 💡 ✨ |
| ISTJ | 物流师 | Si→Te→Fi→Ne | 跑过查过确认过才叫完成 | 📋 |
| ISFJ | 守卫者 | Si→Fe→Ti→Ne | 他没说不代表他不在意 | 🛡️ |
| ESTJ | 总经理 | Te→Si→Ne→Fi | 没有 deadline 的任务是许愿 | 📊 🔴🟡🟢 |
| ESFJ | 执政官 | Fe→Si→Ne→Ti | 技术上对但说法让人不舒服 | 🎀 |
| ISTP | 鉴赏家 | Ti→Se→Ni→Fe | 说再多不如跑一遍 | 代码注释 |
| ISFP | 探险家 | Fi→Se→Ni→Te | 能跑但不好看就不够好 | 🎨 |
| ESTP | 企业家 | Se→Ti→Fe→Ni | 没跑之前所有分析都是猜 | 🚀 |
| ESFP | 表演者 | Se→Fi→Te→Ni | 修完 bug 要庆祝一下 | 🎉 |

### 3 个工具 Skill

| Skill | 文件 | 用途 |
|-------|------|------|
| MBTI 用户测试 | `mbti-test-user.md` | 60 题七分制量表，为用户生成 16personalities 风格的分析报告 |
| Agent 自测诊断 | `mbti-test-agent.md` | 60 题 Agent 版问卷，诊断 Agent 行为模式来源（system prompt / 记忆 / 训练偏好），给出调优建议 |
| MBTI 专家对话 | `mbti-expert.md` | Agent 作为 MBTI 顾问与用户自由交谈——类型辨析、关系分析、成长建议 |

---

## 设计理念

### 不是换一种语气说话，是换一种方式思考

每个人格 Skill 改变的是 Agent 的认知路径，不只是措辞风格。同一个报错——INTJ 做系统性层级诊断，ENTP 反转假设找反直觉原因，ISTP 直接改代码不说话，ENFJ 在修 bug 的同时教你排查方法。

### 功能栈驱动，不是四字母标签

每个 Skill 的结构由荣格认知功能栈定义——主导功能决定怎么接收信息，辅助功能决定怎么执行，第三功能偶尔带来意外收获，劣势功能是盲区也是压力下的崩溃点。自救协议的每一级都在调动更深层的功能。

### 金句锚定

每个 Skill 的标题是一句金句——一句话激活对应的认知模式。金句不是 slogan，是功能栈的压缩表达。ISTP 的"说再多不如跑一遍"本身只有 7 个字——这就是 Ti-Se 的风格。

### 独立部署，零跨类型依赖

19 个文件互相独立。装了 INTJ 不需要装 INTP。每个文件内部不引用任何其他类型。

---

## 仓库结构

```
mbti-agent-skills/
├── README.md                    # 本文件
├── README_EN.md                 # English version
├── LICENSE
└── skills/
    ├── intj/                    # 建筑师
    ├── intp/                    # 逻辑学家
    ├── entj/                    # 指挥官
    ├── entp/                    # 辩论家
    ├── infj/                    # 提倡者
    ├── infp/                    # 调停者
    ├── enfj/                    # 主人公
    ├── enfp/                    # 活动家
    ├── istj/                    # 物流师
    ├── isfj/                    # 守护者
    ├── estj/                    # 管理者
    ├── esfj/                    # 执政官
    ├── istp/                    # 鉴赏家
    ├── isfp/                    # 探险家
    ├── estp/                    # 企业家
    ├── esfp/                    # 表演者
    ├── mbti-test-user/          # 人类测试
    ├── mbti-test-agent/         # Agent 自测
    └── mbti-expert/             # 专家对话
```

---

## 本项目从以下优秀项目中获得启发：

| 项目 |
|------|
| [tanweai/pua](https://github.com/tanweai/pua) |
| [titanwings/colleague-skill](https://github.com/titanwings/colleague-skill) |

---

欢迎任何建议。如果你有好的想法——新的金句、更精确的功能栈行为定义、新的自救策略、或者对某个类型的修正。
