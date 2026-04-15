#auth #flow
### Detailed Flow
#### 🔹 Шаг 1 — Redirect на Authorization Server

Client → Browser → Google

##### 📌 URL:
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

####  🔹 Шаг 2 — Пользователь логинится

User → Google login → Consent screen

#### 🔹 Шаг 3 — Redirect назад

Google → redirect_uri

##### 📌 Пример:

http://localhost:8081/callback?code=abc123&state=xyz

#### 🔹 Шаг 4 — Backend получает code

Server ← code

👉 проверяет:

state == expected_state

#### 🔹 Шаг 5 — Exchange code → token

Server → Google

##### 📌 POST:

https://oauth2.googleapis.com/token

##### 📌 Body:

{  
  "code": "abc123",  
  "client_id": "...",  
  "client_secret": "...",  
  "redirect_uri": "...",  
  "grant_type": "authorization_code"  
}

#### Шаг 6 — Получаем токены

{  
  "access_token": "...",  
  "expires_in": 3600,  
  "token_type": "Bearer",  
  "id_token": "..." // (OIDC)  
}

#### Шаг 7 — Используем access_token

GET /userinfo  
Authorization: Bearer access_token

#### 🧠 Важное понимание

OAuth НЕ про логин  
OAuth про доступ к API

---


## 🔗 Связи  
  
- [[Knowledge/Authentication & Authorization/OAuth/Overview]]  
- [[Security]]  
- [[Data & Credentials]]
- [[Implementation]]