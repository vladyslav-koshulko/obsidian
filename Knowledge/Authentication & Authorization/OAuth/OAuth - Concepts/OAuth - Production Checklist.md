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
## 🔗 Связи

- [[OAuth - Security]]
- [[OAuth - Attacks]]
- [[PKCE]]
- [[JWT]]
- [[CSRF (Cross-Site Request Forgery)]]
- [[XSS (Cross-Site Scripting)]]
