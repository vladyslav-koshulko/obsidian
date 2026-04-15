# General
### 📌 Что это

Подмена запроса от пользователя

### ⚙️ Как работает

- пользователь авторизован
- атакующий заставляет отправить запрос


## CSRF защита

- CSRF token
- SameSite cookies

# 🚨 CSRF (OAuth Flow Hijacking)

## 📌 Суть

Атакующий подсовывает свой OAuth flow пользователю

---

## 💥 Как выглядит атака

1. Attacker начинает OAuth flow  
2. Получает authorization request  
3. Жертва логинится  
4. code уходит attacker’у

---

## 💻 Уязвимый код

// ❌ нет state  
const params = {  
  client_id: CLIENT_ID,  
  redirect_uri: CALLBACK_URL,  
  response_type: "code"  
};

---

## ✅ Защита

// generate state  
const state = crypto.randomBytes(16).toString("hex");  
storeStateInSession(state);  
  
const params = {  
  client_id: CLIENT_ID,  
  redirect_uri: CALLBACK_URL,  
  response_type: "code",  
  state  
};

---

### Проверка

app.get("/callback", (req, res) => {  
  if (req.query.state !== session.state) {  
    return res.status(403).send("CSRF detected");  
  }  
});

---

## 🧠 Ключевая идея

state связывает request ↔ response


# Связи
- [[Knowledge/Authentication & Authorization/OAuth/Overview|Overview]]
- [[Data & Credentials]]
- [[Attacks]]
- [[Security]]
- [[Authorization Code Flow]]

