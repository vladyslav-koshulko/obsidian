#auth #implementation

# 🚀 OAuth Production Checklist

## 🧠 Цель

Перед релизом убедиться, что:

- flow безопасен
- токены защищены
- нет типичных уязвимостей

---

## 🔐 1. Flow Configuration

### ✅ Authorization Code Flow

- [ ]  используется **Authorization Code**, не Implicit
- [ ]  для SPA/mobile включён [[PKCE]]
- [ ]  `response_type=code`

---

### ❌ Антипаттерны

- [ ]  НЕ используется Implicit flow
- [ ]  НЕ используется Password grant

---

## 🔑 2. Client Configuration

### ✅ Client Credentials

- [ ]  `client_secret` хранится только на backend
- [ ]  secret НЕ в:
    - frontend
    - mobile app
    - repo

---

### ✅ Redirect URI

- [ ]  strict match (exact)
- [ ]  нет wildcard (`*`)
- [ ]  whitelist разрешённых URL

---

## 🔁 3. State & CSRF

### ✅ state

- [ ]  генерируется случайно
- [ ]  хранится (session / cookie)
- [ ]  проверяется при callback

if (req.query.state !== session.state) {  
  throw new Error("CSRF detected");  
}

---

## 🔐 4. PKCE (обязательно для public clients)

- [ ]  используется `code_challenge_method=S256`
- [ ]  `code_verifier` хранится безопасно
- [ ]  проверяется при обмене token

---

## 🎟 5. Token Handling

### ✅ Access Token

- [ ]  НЕ хранится в localStorage
- [ ]  НЕ передаётся в URL
- [ ]  используется только через HTTPS

---

### ✅ Refresh Token

- [ ]  хранится только на backend
- [ ]  rotation включён
- [ ]  ограничен по времени

---

### ✅ Storage

- [ ]  HttpOnly cookies
- [ ]  Secure flag
- [ ]  SameSite=Strict/Lax

---

## 🧪 6. Token Validation

### ✅ Проверки

- [ ]  signature (JWT verify)
- [ ]  expiration (`exp`)
- [ ]  audience (`aud`)
- [ ]  issuer (`iss`)

jwt.verify(token, PUBLIC_KEY, {  
  audience: CLIENT_ID,  
  issuer: "https://accounts.google.com"  
});

---

## 🔐 7. Transport Security

- [ ]  только HTTPS
- [ ]  HSTS включён
- [ ]  нет mixed content

---

## 🚨 8. Attack Protection

### ✅ CSRF

- [ ]  используется `state`
- [ ]  SameSite cookies

---

### ✅ XSS

- [ ]  CSP включён
- [ ]  input sanitization
- [ ]  токены не доступны JS

---

### ✅ Token Leakage

- [ ]  токены не логируются
- [ ]  не попадают в URL
- [ ]  не попадают в analytics

---

### ✅ Redirect Attacks

- [ ]  redirect_uri фиксирован
- [ ]  нет open redirect

---

## 📦 9. Scope Management

- [ ]  минимально необходимые scope
- [ ]  нет “admin” без необходимости

---

## 🧱 10. Backend Security

- [ ]  exchange code → token только на backend
- [ ]  client_secret не утекает
- [ ]  rate limiting на auth endpoints

---

## 🔍 11. Logging & Monitoring

- [ ]  логируются ошибки (без токенов)
- [ ]  отслеживаются:
    - подозрительные логины
    - частые попытки
- [ ]  alerts настроены

---

## 🔄 12. Session Strategy

- [ ]  access_token не используется напрямую в UI
- [ ]  используется backend session
- [ ]  session expiration настроен

---

## 🧪 13. Testing

- [ ]  проверен CSRF
- [ ]  проверен redirect_uri
- [ ]  проверен token leakage
- [ ]  проверен PKCE

---

## 🔗 Связи

- [[Security]]
- [[Attacks]]
- [[PKCE]]
- [[JWT]]
- [[CSRF (Cross-Site Request Forgery)]]
- [[XSS (Cross-Site Scripting)]]

---

## 💡 Финальная проверка (самое важное)

Если коротко:

- есть PKCE? ✔  
- есть state? ✔  
- токены не в localStorage? ✔  
- redirect_uri фиксирован? ✔  
- token валидируется? ✔

---

## 🚨 Реальность (самый частый фейл)

90% уязвимостей:

- нет PKCE
- нет state
- токен в localStorage
- wildcard redirect_uri
---

# 🏗 OAuth Architecture Patterns (Microservices & Production)

## 🧠 Суть

OAuth — это не только flow, а **архитектура доступа**:

- где живёт токен
- кто его валидирует
- как ходят сервисы

---

## 🧩 1. BFF (Backend For Frontend)

### 📌 Идея

Frontend **не работает напрямую с OAuth**

👉 всё через backend

---

### 💡 Схема

Browser → BFF → OAuth Provider  
             ↓  
          Session  
             ↓  
         Microservices

---

### ⚙️ Как работает

1. Browser → BFF (login)
2. BFF делает OAuth flow
3. BFF хранит tokens
4. Browser получает **session cookie**
5. Все запросы идут через BFF

---

### ✅ Плюсы

- токены не попадают в frontend
- защита от XSS
- проще контролировать доступ

---

### ❌ Минусы

- дополнительный слой
- нагрузка на BFF

---

### 🧠 Когда использовать

- SPA / Web app
- high-security системы

---

## 🔐 2. Token Relay Pattern

### 📌 Идея

Frontend получает token → передаёт его дальше

---

### 💡 Схема

Browser → API Gateway → Service A → Service B  
           (Bearer token передаётся)

---

### ⚙️ Как работает

- frontend получает `access_token`
- отправляет его в каждый request
- сервисы валидируют token

---

### ✅ Плюсы

- простота
- нет session state

---

### ❌ Минусы

- токен в frontend
- риск XSS
- сложнее revoke

---

### 🧠 Когда использовать

- публичные API
- low-security / internal tools

---

## 🧱 3. API Gateway + OAuth

### 📌 Идея

Вся авторизация через gateway

---

### 💡 Схема

Client → API Gateway → Microservices  
           ↓  
     Token validation

---

### ⚙️ Как работает

- gateway:
    - валидирует JWT
    - проверяет scope
- сервисы доверяют gateway

---

### ✅ Плюсы

- централизованная безопасность
- меньше логики в сервисах

---

### ❌ Минусы

- single point of failure
- сложность gateway

---

### 🧠 Когда использовать

- microservices
- production systems

---

## 🔑 4. Service-to-Service (Client Credentials)

### 📌 Идея

Сервисы общаются без пользователя

---

### 💡 Схема

Service A → Auth Server → access_token → Service B

---

### ⚙️ Как работает

- сервис получает token через Client Credentials
- использует его для вызова другого сервиса

---

### ✅ Плюсы

- безопасно
- нет user context

---

### ❌ Минусы

- нет user identity
- нужно управлять scope

---

### 🧠 Когда использовать

- internal APIs
- background jobs

---

## 🔄 5. Token Exchange Pattern

### 📌 Идея

Менять токен при переходе между сервисами

---

### 💡 Схема

User Token → Service A → exchange → Service Token → Service B

---

### ⚙️ Как работает

- сервис получает user token
- обменивает его на новый (с другим scope)

---

### ✅ Плюсы

- минимальные права
- изоляция сервисов

---

### ❌ Минусы

- сложность
- дополнительная latency

---

### 🧠 Когда использовать

- high-security
- zero-trust архитектуры

---

## 🧠 6. Session + OAuth Hybrid

### 📌 Идея

OAuth только для login, дальше session

---

### 💡 Схема

OAuth → Backend → Session → Microservices

---

### ⚙️ Как работает

- OAuth → login
- backend создаёт session
- токены не используются напрямую

---

### ✅ Плюсы

- проще
- безопаснее
- легко revoke

---

### ❌ Минусы

- stateful
- сложнее масштабировать

---

### 🧠 Когда использовать

- классические web apps
- enterprise системы

---

## 🔐 7. Zero Trust + JWT

### 📌 Идея

Каждый сервис валидирует токен сам

---

### 💡 Схема

Client → Service A → Service B → Service C  
        (каждый валидирует JWT)

---

### ⚙️ Как работает

- JWT подписан
- каждый сервис:
    - проверяет подпись
    - проверяет scope

---

### ✅ Плюсы

- нет доверия внутри сети
- высокая безопасность

---

### ❌ Минусы

- сложнее
- нужно правильно валидировать

---

### 🧠 Когда использовать

- distributed systems
- high-security environments

---

## ⚖️ Сравнение (быстро)

BFF → безопасно, удобно  
Token Relay → просто, но риск  
API Gateway → централизовано  
Service-to-Service → для backend  
Token Exchange → high security  
Hybrid → баланс  
Zero Trust → максимум безопасности

---

## 🔗 Связи

- [[Knowledge/Authentication & Authorization/OAuth/Overview| OAuth 2.0]]
- [[JWT]]
- [[PKCE]]
- [[Security|OAuth Security]]
- [[Microservices Architecture]]

---

## 💡 Главное понимание

OAuth — это не про login,  
а про то, как токены живут в системе

---

## 🚀 Практическое правило

Если не знаешь что выбрать:

Web app → BFF + Session  
Microservices → API Gateway + JWT  
Internal services → Client Credentials  
High security → Token Exchange + Zero Trust
# Пример реализации
## 🧠 Что делает этот код  
  
- генерирует auth URL  
- обрабатывает callback  
- меняет code → token


## 💻 Пример (минимальный)

### 1. Получение URL

```js
function getAuthUrl() {
  return "https://accounts.google.com/o/oauth2/v2/auth?...";
}
```

### 2. Callback

```js
app.get("/callback", async (req, res) => {
  const { code } = req.query;
  const token = await exchangeCode(code);
});
```

---
## 💻 Пример реализации (Node.js — базово)

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

---


# 🔗 Связи  
  
- [[Knowledge/Authentication & Authorization/OAuth/Overview]]  
- [[Security]]  
- [[Data & Credentials]]
- [[Knowledge/Authentication & Authorization/OAuth/Resources]]
- [[Use Cases]]