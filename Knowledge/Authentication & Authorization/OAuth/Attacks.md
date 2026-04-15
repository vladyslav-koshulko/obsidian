#auth #security #attacks

# 💡 Главное понимание

Authentication attacks → пытаются стать пользователем  
OAuth attacks → пытаются украсть доступ

## 🧠 Суть

OAuth ломают не алгоритмы, а:

- неправильная реализация
- утечки токенов
- отсутствие проверок

# 🔐 OAuth Attacks

## 🧠 Суть

Атаки направлены на:

- кражу `access_token`
- перехват `authorization code`
- обход flow

---

## 🎯 Authorization Code Interception

### 📌 Что это

Перехват `code`

### ⚙️ Как работает

- attacker перехватывает redirect
- использует code раньше backend

## 🔁 CSRF (в OAuth)

### 📌 Что это

Подмена authorization запроса

### ⚙️ Как работает

- attacker инициирует flow
- пользователь логинится
- code уходит атакующему

## 🔓 Token Leakage

### 📌 Что это

Утечка access_token

### ⚙️ Как работает

- token в URL
- token в логах
- token в localStorage



## 🔀 Open Redirect

### 📌 Что это

Подмена redirect_uri

### ⚙️ Как работает

- attacker указывает свой redirect
- получает code/token

## 🧪 PKCE Bypass (если нет PKCE)

### 📌 Что это

Атака на public client

### ⚙️ Как работает

- нет client_secret
- attacker перехватывает code


## 🧱 Misconfiguration

### 📌 Что это

Неправильная настройка OAuth

### ⚙️ Примеры

- wildcard redirect_uri
- отсутствие state
- слабые scope



# 🛡 Защита (OAuth)

## 🔐 PKCE (обязательно для SPA/mobile)

- предотвращает перехват code
- связывает request ↔ token

---

## 🎯 state (обязательно)

- защита от CSRF
- проверка:

state == expected_state

---

## 🔒 redirect_uri (строго)

- whitelist
- exact match

---

## 🔑 Token Storage

❌ нельзя:

- localStorage

✅ лучше:

- HttpOnly cookies
- backend session

---

## 🔐 HTTPS (всегда)

- защита от MITM

---

## 🧠 Scope минимизация

- только нужные права

---

## 🔁 Refresh Token защита

- rotation
- ограничение времени жизни

---

## 🧪 Token validation

- JWT signature check
- expiration check
- audience / issuer


# 🔗 Связи  
  
- [[Knowledge/Authentication & Authorization/OAuth/Overview]]  
- [[Security]]  
- [[Data & Credentials]]
---
- [[Authorization Code Interception]]
- [[CSRF (Cross-Site Request Forgery)]]
- [[Token Leakage]]
- [[Open Redirect]]
- [[PKCE Bypass]]
- [[Misconfiguration]]
- [[XSS (Cross-Site Scripting)]]
- [[Authorization Code Reuse]]
- [[Missing Token Validation]]
---
- [[PKCE]]
- [[JWT]]
