#auth #oauth #architecrute #patterns
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

---
## 🔗 Связи

- [[OAuth - Overview| OAuth 2.0]]
- [[JWT]]
- [[PKCE]]
- [[OAuth - Security|OAuth Security]]
- [[Microservices Architecture]]