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

## 🧱 Grant Types

- Authorization Code (основной)
- Client Credentials (service-to-service)
- Implicit ❌ (deprecated)
- Password ❌ (deprecated)
---
## 🎟 Типы токенов

- access_token → доступ к API
- refresh_token → обновление
- id_token → identity ([[OIDC]])
---