#auth #data #credentials

## 🧠 Роли

- **Resource Owner** → пользователь  
- **Client** → приложение  
- **Authorization Server** → Google  
- **Resource Server** → API  

---

## 🔑 Основные данные

| Данные          | Где используются         |
| --------------- | ------------------------ |
| `client_id`     | идентификатор приложения |
| `client_secret` | секрет (только backend)  |
| `redirect_uri`  | куда вернуться           |
| `code`          | временный код            |
| `access_token`  | доступ к API             |
| `state`         | защита от CSRF           |

---
## 🎟 Типы токенов

- access_token → доступ к API
- refresh_token → обновление
- id_token → identity ([[OIDC]])
---

# 🔗 Связи  
  
- [[OAuth - Overview]]  
- [[OAuth - Security]]  
- [[OAuth - Data & Credentials]]
- [[OAuth - Attacks]]