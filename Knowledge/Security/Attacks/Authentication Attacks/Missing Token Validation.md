# 🚨 Missing Token Validation

## 📌 Суть

Backend принимает любой token

---

## 💻 Уязвимый код

```
// ❌ просто принимаем токен  
const user = decode(token);
```

---

## ✅ Защита

```
import jwt from "jsonwebtoken";  
  
jwt.verify(token, PUBLIC_KEY, {  
  audience: "your-client-id",  
  issuer: "https://accounts.google.com"  
});
```

---

## 🧠 Проверки

- signature
- exp
- aud
- iss


# Связи
- [[OAuth - Overview|OAuth - Overview]]
- [[OAuth - Data & Credentials]]
- [[OAuth - Attacks]]
- [[OAuth - Security]]
- [[JWT]]