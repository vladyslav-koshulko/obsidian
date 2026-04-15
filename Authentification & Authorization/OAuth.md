## Resources
[- Що таке OAuth 2.0 і як працює авторизація через токени](https://wezom.com.ua/ua/blog/scho-take-oauth-20-i-yak-pratsyuje-avtorizatsiya-cherez-tokeni)

## История
### OAuth 1.0 (2007)

- Первая версия стандарта
- Делали компании вроде Twitter и Google

**Как работал:**

- Сложная криптография (подписи запросов)
- Каждый HTTP-запрос подписывается

**Проблемы:**

- ❌ Сложно реализовать (подписи, nonce, timestamp)
- ❌ Легко ошибиться → дыры в безопасности
- ❌ Плохо подходит для мобильных и SPA

### OAuth 1.0a

- Фикс уязвимости (session fixation)
- Но **архитектура осталась сложной**

### OAuth 2.0 (2012)

Создавался уже с учётом:

- мобильных приложений
- JS (SPA)
- REST API

**Главная идея:**  
👉 убрать криптографию из клиента → упростить всё
получаешь access_token и просто используешь его

### Разные сценарии (flows)

OAuth 2.0 ввёл grant types:

- Authorization Code (основной)
- Client Credentials
- Implicit (устарел)
- Password (deprecated)

👉 гибкость под разные кейсы

### HTTPS вместо подписей

- Безопасность держится на TLS
- Нет необходимости в сложных алгоритмах подписи

### Расширяемость

OAuth 2.0 — это **framework**, а не строгий протокол

👉 поэтому появился:

- [[OIDC]]
- [[PKCE]]
- [[JWT]]

### Минус OAuth 2.0

👉 он **слишком гибкий**

- нет жёстких правил
- можно реализовать небезопасно

Поэтому:

- добавили [[PKCE]]
- добавили [[OIDC]]

### Что с OAuth 1.0 сейчас

- практически **мертв**
- почти никто не использует
- заменён OAuth 2.0

Исключения:
- старые API (редко)

### Почему все используют OAuth 2.0

1. Проще внедрить
2. Подходит под web + mobile + backend
3. Расширяем ([[OIDC]], [[JWT]])
4. Поддерживается всеми (Google, GitHub и т.д.)

### Итог

- OAuth 1.0 → безопасный, но сложный
- OAuth 2.0 → простой, гибкий, стал стандартом
- [[OIDC]] → добавил аутентификацию


## OAuth 2.0

### 📌 Что это

**OAuth 2.0** — это протокол **делегированной авторизации**.
Ключевая идея: Ты даёшь приложению доступ к своим данным БЕЗ передачи пароля

### 🧠 Кто участвует (роли)

| Роль                 | Кто это          |
| -------------------- | ---------------- |
| Resource Owner       | пользователь     |
| Client               | твоё приложение  |
| Authorization Server | Google           |
| Resource Server      | API (Google API) |

### 📦 Что передаётся

|Данные|Где используются|
|---|---|
|`client_id`|идентификатор приложения|
|`client_secret`|секрет (только backend)|
|`redirect_uri`|куда вернуться|
|`code`|временный код|
|`access_token`|доступ к API|
|`state`|защита от CSRF|

### 🔁 Основной Flow (Authorization Code)

---

### 🔹 Шаг 1 — Redirect на Authorization Server

Client → Browser → Google

### 📌 URL:
```
https://accounts.google.com/o/oauth2/v2/auth?  
  client_id=...  
  &redirect_uri=http://localhost:8081/callback  
  &response_type=code  
  &scope=email profile  
  &state=RANDOM_STRING
```

- `response_type=code` → говорим: “дай code”
- `scope` → какие данные нужны
- `state` → защита

### Шаг 2 — Пользователь логинится

User → Google login → Consent screen

---

### 🔹 Шаг 3 — Redirect назад

Google → redirect_uri

### 📌 Пример:

http://localhost:8081/callback?code=abc123&state=xyz

### Шаг 4 — Backend получает code

Server ← code

👉 проверяет:

state == expected_state

### Шаг 5 — Exchange code → token

Server → Google

### 📌 POST:

https://oauth2.googleapis.com/token

### 📌 Body:

{  
  "code": "abc123",  
  "client_id": "...",  
  "client_secret": "...",  
  "redirect_uri": "...",  
  "grant_type": "authorization_code"  
}

### Шаг 6 — Получаем токены

{  
  "access_token": "...",  
  "expires_in": 3600,  
  "token_type": "Bearer",  
  "id_token": "..." // (OIDC)  
}

### Шаг 7 — Используем access_token

GET /userinfo  
Authorization: Bearer access_token

### 🧠 Важное понимание

OAuth НЕ про логин  
OAuth про доступ к API

### 🔐 Безопасность

---

### 📌 state (обязательно)

защита от CSRF

---

### 📌 redirect_uri должен совпадать

иначе запрос отклоняется

---

### 📌 client_secret

никогда не отдаётся в frontend

---
### 💻 Пример реализации (Node.js — базово)

---

### 📦 Шаг 1 — генерируем URL

const querystring = require("querystring");  
  
function getAuthUrl() {  
  const params = {  
    client_id: process.env.CLIENT_ID,  
    redirect_uri: "http://localhost:3000/callback",  
    response_type: "code",  
    scope: "email profile",  
    state: "random_string"  
  };  
  
  return "https://accounts.google.com/o/oauth2/v2/auth?" + querystring.stringify(params);  
}

---

### 📦 Шаг 2 — обработка callback

app.get("/callback", async (req, res) => {  
  const { code, state } = req.query;  
  
  // validate state  
  if (state !== expectedState) {  
    return res.send("Invalid state");  
  }  
  
  const token = await exchangeCode(code);  
  
  res.send("Login success");  
});

---

### 📦 Шаг 3 — обмен code → token

const axios = require("axios");  
  
async function exchangeCode(code) {  
  const response = await axios.post(  
    "https://oauth2.googleapis.com/token",  
    {  
      code,  
      client_id: process.env.CLIENT_ID,  
      client_secret: process.env.CLIENT_SECRET,  
      redirect_uri: "http://localhost:3000/callback",  
      grant_type: "authorization_code"  
    }  
  );  
  
  return response.data;  
}

---

### 📦 Шаг 4 — использование access_token

async function getUserInfo(accessToken) {  
  const res = await axios.get(  
    "https://www.googleapis.com/oauth2/v2/userinfo",  
    {  
      headers: {  
        Authorization: `Bearer ${accessToken}`  
      }  
    }  
  );  
  
  return res.data;  
}

---

### 🧠 Итог Flow

1. Redirect → Google  
2. Login + consent  
3. Receive code  
4. Exchange code → access_token  
5. Access API


### ⚠️ Ограничения OAuth 2.0

---

### ❌ Нет identity

ты не знаешь КТО пользователь

👉 только доступ

---

### ❌ access_token может быть opaque

иногда это просто строка

---

### 🚀 Что делают дальше (улучшение)

---

### 1. ➕ OIDC (обязательно)

добавляет id_token → user identity

---

### 2. 🔄 Refresh Token

обновление access_token без логина

---

### 3. 🔐 PKCE

для mobile / SPA  
без client_secret

---

### 4. 🧱 Backend sessions

access_token → session → user

---

### 5. 🧠 Scopes management

минимизация прав

---

### 6. 🏗 Multi-provider

Google + GitHub + Apple

---

### 7. 🛡 Token validation

JWT / introspection

---

### 🧠 Главное понимание

OAuth = доступ  
OIDC = идентификация



---
User → Google → Access Granted → App
### 📌 Flow (Authorization Code)

1. Redirect user → Google  
2. User login + consent  
3. Google → redirect with code  
4. Backend → exchange code → tokens