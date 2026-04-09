## Structure
```
header.payload.signature
```

Payload example:
```
{
  "sub": "user_id",
  "email": "user@gmail.com",
  "name": "Your name",
  "iss": "https://accounts.google.com",
  "aud": "client_id",
  "exp": 1234567890
}
```
### Значения

|Поле|Значение|
|---|---|
|`sub`|уникальный id пользователя|
|`email`|email|
|`name`|имя|
|`iss`|кто выдал|
|`aud`|для кого|
|`exp`|срок жизни|
# 🔑 JWT Подпись (критично)

---

## HS256 vs RS256

### HS256 (не используется Google)

shared secret

---

### RS256 (использует Google)

private key (Google)  
public key (мы)