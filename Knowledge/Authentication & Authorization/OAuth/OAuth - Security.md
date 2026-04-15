#auth #security 
## 🔐 Безопасность (критично)

- `state` → защита от [[CSRF (Cross-Site Request Forgery)]]  
- `redirect_uri` должен совпадать  
- `client_secret` только на backend  
- всегда HTTPS (TLS)

---
## 🚨 Уязвимости

- CSRF (если нет state)
- Authorization code interception
- Token leakage (в логах / URL)
- Open redirect

👉 решение:
- [[PKCE]]
- strict redirect_uri

## 🚨 Critical (must-have)  
  
- PKCE  
- state  
- strict redirect_uri  
- token storage (no localStorage)

---
## ⚠️ Ограничения

- ❌ нет identity (кто пользователь)
- ❌ access_token может быть opaque

👉 решение:
- [[OIDC]]
- [[JWT]]

---

## 💡 Типичные ошибки

- использовать OAuth как login ❌  
- хранить client_secret в frontend ❌  
- не проверять state ❌  
- давать слишком широкие scope ❌  
---

## 🔗 Связи  
  
- [[OAuth - Overview]]  
- [[OAuth - Data & Credentials]]
- [[OAuth - Attacks]]
- [[OAuth - Use Cases]]

- [[OIDC]]
- [[JWT]]
- [[PKCE]]
- [[CSRF (Cross-Site Request Forgery)]]