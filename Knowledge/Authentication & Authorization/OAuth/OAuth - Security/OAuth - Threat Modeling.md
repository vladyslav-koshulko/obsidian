#auth #oauth
# 🧠 OAuth Threat Modeling

## 📌 Цель

Понять:

- где границы доверия
- где можно атаковать
- что защищать в первую очередь

---

# 🧱 1. Assets (что защищаем)

## 🔑 Критичные активы

- `access_token`
- `refresh_token`
- `authorization code`
- `client_secret`
- session (cookie)

---

## 🧠 Важно

если атакующий получил любой из этих элементов → доступ скомпрометирован

---

# 🌍 2. Actors (кто участвует)

- User (browser)
- Client (frontend)
- Backend (BFF)
- OAuth Provider
- Microservices
- Attacker ❗

---

# 🧭 3. Trust Boundaries (самое важное)

## 📌 Границы доверия

[Browser] ←❗→ [Backend]  
[Backend] ←→ [OAuth Provider]  
[Backend] ←→ [Microservices]  
[Internet] ←❗→ [System]

---

## 🧠 Ключевая идея

👉 всё, что **слева от backend — недоверенная зона**

---

# 🔁 4. Data Flow (упрощённо)

1. Browser → Backend (login)  
2. Backend → OAuth Provider  
3. Provider → Browser (code)  
4. Browser → Backend (code)  
5. Backend → Provider (token)  
6. Backend → Session  
7. Browser → API (session)

---

# 🚨 5. Threats по шагам

---

## 🔹 Шаг 1–3 (Redirect)

### 💥 Угрозы

- [[CSRF (Cross-Site Request Forgery)]]
- [[Open Redirect]]

---

### 🔐 Защита

- `state`
- strict `redirect_uri`

---

## 🔹 Шаг 3–4 (code возвращается)

### 💥 Угрозы

- [[Authorization Code Interception]]

---

### 🔐 Защита

- [[PKCE]]
- short-lived code

---

## 🔹 Шаг 4–5 (exchange)

### 💥 Угрозы

- code reuse
- MITM (если нет HTTPS)

---

### 🔐 Защита

- HTTPS
- одноразовый code

---

## 🔹 Шаг 6 (token хранение)

### 💥 Угрозы

- [[Token Leakage]]
- [[XSS (Cross-Site Scripting)]]

---

### 🔐 Защита

- HttpOnly cookies
- backend storage

---

## 🔹 Шаг 7 (API calls)

### 💥 Угрозы

- stolen session
- privilege escalation

---

### 🔐 Защита

- session validation
- scope control

---

# 🔐 6. STRIDE модель (упрощённо)

## S — Spoofing (подмена)

- украли token
- украли session

---

## T — Tampering (подмена данных)

- изменение redirect_uri
- подмена параметров

---

## R — Repudiation (отказ от действий)

- нет логов → нельзя доказать

---

## I — Information Disclosure

- утечка token
- утечка user data

---

## D — Denial of Service

- brute force login
- flood OAuth endpoints

---

## E — Elevation of Privilege

- scope escalation
- неправильная авторизация

---

# 🧠 7. Attack Surface (где атакуют)

## 📌 Основные точки

- redirect_uri  
- callback endpoint  
- token storage  
- frontend (XSS)  
- API gateway

---

# 🛡 8. Defense Strategy (приоритеты)

## 🔥 Critical (must-have)

- PKCE
- state
- HTTPS
- HttpOnly cookies
- strict redirect_uri

---

## ⚠️ Important

- token validation
- scope minimization
- refresh rotation

---

## 🧠 Advanced

- anomaly detection
- device fingerprint
- rate limiting

---

# ⚡ 9. Quick Threat Checklist

- может ли attacker получить code? ❓  
- может ли attacker получить token? ❓  
- может ли attacker подменить redirect? ❓  
- может ли attacker украсть session? ❓

---

# 💡 10. Главное понимание

OAuth безопасность = контроль потоков данных через границы доверия

---

# 🚀 Практическое применение

Когда проектируешь систему:

1. Найди границы доверия  
2. Найди где идут токены  
3. Представь attacker на каждом шаге  
4. Закрой каждую точку

---

# 🔗 Связи

- [[OAuth - Overview]]
- [[OAuth - Attacks]]
- [[OAuth - Security Overview]]
- [[PKCE]]
- [[CSRF (Cross-Site Request Forgery)]]
- [[XSS (Cross-Site Scripting)]]
- [[Microservices Architecture]]