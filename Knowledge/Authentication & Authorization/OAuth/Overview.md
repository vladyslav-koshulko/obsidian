#auth

# General
## 🧠 Суть (TL;DR)
OAuth 2.0 — это **делегированная авторизация**.

👉 приложение получает доступ к API пользователя  
👉 без передачи логина/пароля  

---

## 📌 Когда использовать

- Login через Google / GitHub
- Доступ к API от имени пользователя
- Интеграции между сервисами
- Microservices (service-to-service)

---

## ⚙️ Основная идея

❗ OAuth НЕ про логин  
👉 OAuth про **доступ к ресурсам**

Если нужен логин → [[OIDC]]

---
## 🧠 Быстрое сравнение

- OAuth → доступ  
- OIDC → кто пользователь  
---
## 🧠 Mental Model

OAuth — это как “ключ от квартиры”:

- ты не даёшь пароль
- ты даёшь ограниченный ключ
- ключ можно отозвать

👉 access_token = временный ключ

---
## 🔍 Что реально происходит

- Browser участвует только в redirect
- Backend делает обмен code → token
- access_token никогда не должен светиться в URL
---
## 🌍 Где используется  
  
- Google Login  
- GitHub OAuth  
- API integrations  
- Microservices (Client Credentials)
---

## Сравнение
  
- OAuth → доступ  
- OIDC → идентификация

# Flow
## 🔁 Основной [[Authorization Code Flow]] (Authorization Code)

1. Redirect пользователя на Authorization Server  
2. Пользователь логинится + даёт consent  
3. Получаем `code`  
4. Backend меняет `code` → `access_token`  
5. Используем `access_token` для API  
## 💡 Упрощённая схема

User → Authorization Server → code → Backend → access_token → API

---
### 🚀 Расширения

- [[OIDC]] → добавляет identity  
- [[PKCE]] → для SPA / mobile  
- [[JWT]] → структура токена  
- Refresh Token → обновление доступа  
---
# 🔗 Связи

- [[OIDC]]
- [[PKCE]]
- [[JWT]]
- [[TLS]]
- [[Authentication vs Authorization]]
---

- [[Authorization Code Flow]]
- [[Data & Credentials]]
- [[Security]]  