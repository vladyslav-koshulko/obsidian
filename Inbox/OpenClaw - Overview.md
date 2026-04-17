
После установки OpenClaw у тебя появляется не просто CLI, а целая среда:

- **Gateway** — центральный процесс: каналы, роутинг, сессии, веб‑интерфейс.
    
- **Agent workspace** — рабочая папка агента: файлы, память, локальные инструкции.
    
- **Главный конфиг** `~/.openclaw/openclaw.json` — место, где задаётся почти всё.
    
- **Каналы** — Telegram, WhatsApp, Discord и другие.
    
- **Автономность** — heartbeat, cron, hooks.
    
- **Multi‑agent** — несколько агентов в одном Gateway.

Если совсем упростить: **OpenClaw — это шлюз между мессенджерами, моделью и твоими агентами**.

После установки твоя задача не в том, чтобы «заполнить все поля», а в том, чтобы:

1. поднять Gateway;
    
2. выбрать модель и канал;
    
3. привести в порядок `openclaw.json`;
    
4. настроить workspace;
    
5. только потом добавлять автономность и несколько агентов.

### Базовая последовательность

```
openclaw onboard
```

Это мастер первичной настройки. Он помогает:

- выбрать режим работы;
    
- настроить Gateway;
    
- задать авторизацию;
    
- выбрать модель или провайдера;
    
- подключить канал;
    
- создать стартовую конфигурацию.



После этого, если нужно что то изменить, используйте:

```
openclaw configure
```

А потом проверить текущее состояние:

```
openclaw statusopenclaw doctor
```

Если нужно понять, где лежит активный конфиг:

```
openclaw config file
```

Обычно это:

```
~/.openclaw/openclaw.json
```

#### Главный файл: ~/.openclaw/openclaw.json

> **Почти вся взрослая настройка OpenClaw живёт в** `**~/.openclaw/openclaw.json**`**.**

Это главный конфиг Gateway. Формат — **JSON5**, а не строгий JSON.

OpenClaw **строго валидирует конфиг**.

Это значит:

- опечатка в ключе;
    
- неправильный тип значения;
    
- неизвестное поле;
    
- невалидная структура,
    
- и Gateway может **отказаться стартовать**.

После ручных правок полезно делать:

```
openclaw config validate
openclaw doctor
```

### Можно ли редактировать конфиг «на лету»

Да, во многих случаях есть **hot reload**. Но часть настроек применяются сразу, а часть требует рестарта.

Практичное правило:

- каналы, агенты, heartbeat, messages, tools — чаще всего можно менять без боли;
    
- gateway‑настройки (порт, bind и т.п.) — чаще требуют рестарта.
    

Если не хочется гадать:

```
openclaw gateway restart
```


#### Какие конфиг‑файлы бывают

### 1) ~/.openclaw/openclaw.json

Это **главный системный конфиг**. Здесь настраиваются:

- Gateway
    
- каналы
    
- агенты
    
- routing / bindings
    
- heartbeat
    
- session policy
    
- tools
    
- models
    
- hooks / cron
    
- secrets и другое
    

Если нужно менять поведение системы — почти наверняка это делается здесь.

### 2) ~/.openclaw/workspace/*

Это **рабочая среда агента**. Обычно там есть:

- [`AGENTS.md`](http://agents.md/)
    
- [`SOUL.md`](http://soul.md/)
    
- [`USER.md`](http://user.md/)
    
- [`TOOLS.md`](http://tools.md/)
    
- [`HEARTBEAT.md`](http://heartbeat.md/)
    
- [`IDENTITY.md`](http://identity.md/)
    
- `memory/`[`YYYY-MM-DD.md`](http://yyyy-mm-dd.md/)
    
- [`MEMORY.md`](http://memory.md/)
    

Это не просто заметки: эти файлы попадают в контекст агента и сильно влияют на поведение и расход токенов.

### 3) auth-profiles.json и связанные auth‑файлы

Полезно, когда доходишь до multi‑agent: **авторизация у агентов может быть разделена**.

### 4) .env и ~/.openclaw/.env

OpenClaw умеет читать переменные окружения:

- из процесса;
    
- из локального `.env`;
    
- из `~/.openclaw/.env`.
    

Это удобно для ключей, токенов и приватных данных.
### 5) Разбитые конфиги через $include

Если конфиг разрастается, его можно делить на несколько файлов:

```
{  gateway: { port: 18789 },  agents: { $include: "./agents.json5" },  channels: { $include: "./channels.json5" },}
```


#### Как мыслить об openclaw.json, чтобы не утонуть

Разбей его мысленно на 6 блоков:

1. **как работает шлюз** → `gateway`
    
2. **какие агенты** → `agents`
    
3. **откуда приходят сообщения** → `channels`
    
4. **куда маршрутизировать** → `bindings`
    
5. **как живут сессии и ответы** → `session`, `messages`
    
6. **автономность и права** → `heartbeat`, `tools`, `cron`, `hooks`
    

Если понять эти шесть частей, дальше всё становится сильно проще.




### Разбор ключевых секций openclaw.json

#### gateway: как работает OpenClaw как сервис

Простейший пример:

```
{  gateway: {    mode: "local",    port: 18789,    bind: "loopback",    auth: {      mode: "token",      token: "super-secret-token",    },  },}
```

- Что здесь важно новичку
    
    - `mode`: обычно нужен `local`.
        
    - `port`: по умолчанию часто `18789`.
        
    - `bind`:
        
        - `loopback` — безопасный вариант (доступ только локально).
            
        - `lan` — слушать на всех интерфейсах.
            
        - `tailnet` — для Tailscale.
            
        - `custom` — кастомный bind.
            
    - `auth`: чтобы не оставить Gateway без защиты, настрой токен.
        

---

#### agents: кто вообще у тебя работает

Минимальный стартовый пример:

```
{  agents: {    defaults: {      workspace: "~/.openclaw/workspace",      model: {        primary: "openai/gpt-5.4",      },    },  },}
```

- На что смотреть в `agents.defaults`
    
    - `workspace`: папка, где живёт агент.
        
    - `model`: основная модель, можно настроить fallback.
        
    - `userTimezone`: чтобы heartbeat и расписания работали адекватно.
        
    - `heartbeat`: автономность (ниже — отдельный раздел).
        

---

#### channels: откуда агент получает сообщения

Пример для Telegram:

```
{  channels: {    telegram: {      enabled: true,      botToken: "123456:ABCDEF",      dmPolicy: "pairing",      groups: {        "*": { requireMention: true },      },    },  },}
```

**Ментальная модель:** `channels` отвечает на вопрос: через какие мессенджеры можно достучаться и на каких правилах.


#### bindings: как сообщения попадают к конкретному агенту

Пример:

```
{
  bindings: [
    { agentId: "main", match: { channel: "telegram", accountId: "default" } },
    { agentId: "ops", match: { channel: "telegram", accountId: "ops-bot" } },
  ],
}
```

> `channels` описывает входные двери, а `bindings` решает, в какую комнату отправить посетителя.


#### session: как живёт память разговора

```
{  session: {    dmScope: "per-channel-peer",    reset: {      mode: "idle",      idleMinutes: 180,    },    threadBindings: {      enabled: true,      idleHours: 24,    },  },}
```


#### messages: как агент отвечает

```
{  messages: {    ackReaction: "👀",    queue: {      mode: "collect",      debounceMs: 1000,    },    inbound: {      debounceMs: 1500,    },  },}
```

#### tools: что агенту вообще разрешено

Безопасный старт:

```
{  tools: {    profile: "coding",    deny: ["browser", "canvas"],    elevated: {      enabled: false,    },  },}
```

### Heartbeat: автономность без хаоса

Большинство агентов реактивны: написал — ответил. Heartbeat делает агента проактивным. Это чуть ли не самое крутое что есть в OpenClaw.  
По таймеру Gateway отправляет агенту сигнал, и тот решает — есть ли что-то, о чём стоит сообщить, или нужно выполнить фоновую задачу. Это может быть проверка статуса деплоя, напоминание о зависшей задаче или утренний дайджест. По сути, heartbeat превращает агента из собеседника в фонового ассистента, который работает даже когда ты ему не пишешь.

Если `openclaw.json` — мозг конфигурации, то **heartbeat — сердце автономной работы**.

Heartbeat очень мощный, но легко становится источником шума и расхода токенов, если не задать понятный сценарий.

#### Как включается heartbeat

```
{  agents: {    defaults: {      heartbeat: {        every: "30m",        target: "last",        directPolicy: "allow",        lightContext: false,      },    },  },}
```

- Что значат основные настройки
    
    - `every`: интервал (`30m`, `1h`, `2h`, `0m` — выключить).
        
    - `target`: куда отправлять результат (`none`, `last`, конкретный канал).
        
    - `directPolicy`: можно ли писать в личку.
        
    - `lightContext`: облегчённый контекст, в основном с опорой на [`HEARTBEAT.md`](http://heartbeat.md/).
        

#### Зачем нужен HEARTBEAT.md

Без [`HEARTBEAT.md`](http://heartbeat.md/) heartbeat часто превращается в:

- шумного собеседника, который дёргает без дела;
    
- дорогой таймер, который регулярно сжигает токены.
    

[`HEARTBEAT.md`](http://heartbeat.md/) — это **чеклист автономного режима**.

Пример:

```
# HEARTBEAT- Проверь, есть ли что-то срочное или зависшее в текущих задачах.- Если есть блокер — коротко скажи, что именно мешает.- Если есть законченный важный результат — коротко сообщи.- Не дёргай без причины.- Если ничего важного нет, отвечай HEARTBEAT_OK.- Ночью без срочности не пиши.
```

### Telegram: как настроить бота нормально

#### Базовый конфиг Telegram‑бота

```
{  channels: {    telegram: {      enabled: true,      botToken: "123456:ABCDEF",      dmPolicy: "pairing",      groups: {        "*": { requireMention: true },      },      historyLimit: 50,      replyToMode: "first",      linkPreview: true,      streaming: "partial",    },  },}
```

- Ключевые моменты
    
    - `botToken`: токен от BotFather.
        
    - `dmPolicy`:
        
        - `pairing` — мягкий вход.
            
        - `allowlist` — самый предсказуемый доступ.
            
        - `open` — все.
            
        - `disabled` — личка выключена.
            
    - `requireMention: true` в группах — хорошая защита от «болтливого» бота.

### Multi‑agent: несколько агентов и привязка к каналам, чатам, топикам

**Правильная модель:** один Gateway держит несколько изолированных агентов. У каждого может быть свой workspace, инструкции, auth‑профили и маршрутизация входящих сообщений.

#### 1) Добавить нового агента

```
openclaw agents add research
```

Пример руками:

```
{  agents: {    list: [      {        id: "main",        default: true,        workspace: "~/.openclaw/workspace",        agentDir: "~/.openclaw/agents/main/agent",      },      {        id: "research",        workspace: "~/.openclaw/workspace-research",        agentDir: "~/.openclaw/agents/research/agent",      },    ],  },}
```

Один workspace на двух агентов — почти всегда плохая идея. Разделяй `workspace` и `agentDir`.

#### 2) Привязать агента к отдельному Telegram‑аккаунту (два бота)

```
{  channels: {    telegram: {      accounts: {        default: {          botToken: "123456:MAIN_TOKEN",          dmPolicy: "allowlist",          allowFrom: ["tg:123456789"],        },        researchbot: {          botToken: "654321:RESEARCH_TOKEN",          dmPolicy: "allowlist",          allowFrom: ["tg:123456789"],        },      },    },  },  bindings: [    { agentId: "main", match: { channel: "telegram", accountId: "default" } },    { agentId: "research", match: { channel: "telegram", accountId: "researchbot" } },  ],}
```

#### 3) Привязать агента к конкретному чату

```
{  bindings: [    {      agentId: "research",      match: {        channel: "telegram",        peer: { kind: "group", id: "-1001234567890" },      },    },  ],}
```

#### 4) Привязать агента к конкретному Telegram topic

```
{  channels: {    telegram: {      groups: {        "-1001234567890": {          requireMention: true,          topics: {            "3": { agentId: "dev", requireMention: false },            "5": { agentId: "ops", requireMention: false },          },        },      },    },  },}
```

#### 5) Отдельный heartbeat для каждого агента

```
{  agents: {    defaults: {      heartbeat: {        every: "0m",      },    },    list: [      {        id: "main",        default: true,        workspace: "~/.openclaw/workspace-main",      },      {        id: "ops",        workspace: "~/.openclaw/workspace-ops",        heartbeat: {          every: "1h",          target: "telegram",          to: "123456789",          lightContext: true,        },      },    ],  },}
```

---

### Итог

OpenClaw спроектирован так, что можно начать с одного агента в Telegram и постепенно наращивать сложность — не переписывая конфиг с нуля, а добавляя секции по мере необходимости. Главное — не пытаться включить всё сразу, действуйте шаг за шагом, смотрите к чему приводят перемены, все ли вас устраивает.

Gateway, одна модель, один канал, чистый workspace — этого достаточно, чтобы агент заработал. Heartbeat, bindings, multi‑agent — это следующий шаг, когда понятно, зачем они нужны именно тебе.

Если что‑то пошло не так — `openclaw doctor` и `openclaw status` почти всегда подскажут, где копать.



### Architecture
User --------------------------------------------> OpenClaw -------------------------------------------> LLM Provider (Ollama, OpenAI, Gemini) --------------------------------------> LLM Model
(Sends Request | Initiate Prompt)  |  (Agent / Executor | Routes & Executes) | (LLM Runtime | Loads & Manages) | (The Brain | Reasons & Decides)

LLM -> Brain
Ollama -> Body (Running Brain)
OpenClaw -> Hands (Using Brain)



### Resources:

- [Тонкая настройка OpenClaw. Как дойти от установки до multi-agent за один вечер?](https://habr.com/ru/articles/1009862/)


# Связи
- [[OpenClaw - Installation]]