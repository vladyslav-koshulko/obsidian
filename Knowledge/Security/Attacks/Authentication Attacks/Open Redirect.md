#security #attacks #open-redirect #redirect
# 🚨 Open Redirect Attack

## 📌 Суть

Подмена `redirect_uri`

---

## 💥 Атака

```
attacker задаёт:  
redirect_uri=https://evil.com/callback  
  
→ получает code/token
```

---

## 💻 Уязвимый код

```
// ❌ принимаем redirect_uri от клиента  
const redirectUri = req.query.redirect_uri;
```

---

## ✅ Защита

```
// ✔ whitelist  
const allowedRedirects = [  
  "https://myapp.com/callback"  
];  
  
if (!allowedRedirects.includes(redirectUri)) {  
  throw new Error("Invalid redirect_uri");  
}
```
---

## 🧠 Ключевая идея

redirect_uri должен быть фиксирован


# Связи
- [[OAuth - Attacks]]
- [[OAuth - Overview|OAuth - Overview]]
- [[OAuth - Data & Credentials]]
- [[OAuth - Security]]