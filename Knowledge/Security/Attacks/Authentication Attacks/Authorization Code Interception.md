# 🚨 Authorization Code Interception

## 📌 Суть

Атакующий перехватывает `authorization code` и обменивает его на `access_token`

---

## ⚙️ Уязвимый сценарий

- mobile / SPA без [[PKCE]]
- code передаётся через небезопасный канал

---

## 💥 Как выглядит атака

1. User → Authorization Server  
2. Server → redirect with code  
3. Attacker перехватывает code  
4. Attacker → token endpoint → получает access_token

---

## 💻 Уязвимый код

```
// ❌ нет PKCE  
const params = {  
  client_id: CLIENT_ID,  
  redirect_uri: CALLBACK_URL,  
  response_type: "code",  
  scope: "email"  
};
```

---

## ✅ Защита (PKCE)

```
import crypto from "crypto";  
  
// generate code_verifier  
const codeVerifier = crypto.randomBytes(32).toString("hex");  
  
// create code_challenge  
const codeChallenge = crypto  
  .createHash("sha256")  
  .update(codeVerifier)  
  .digest("base64url");  
  
const params = {  
  client_id: CLIENT_ID,  
  redirect_uri: CALLBACK_URL,  
  response_type: "code",  
  code_challenge: codeChallenge,  
  code_challenge_method: "S256"  
};
```

---

## 🧠 Почему это работает

- attacker не знает `code_verifier`
- без него нельзя получить token


# Связи
- [[Attacks]]
- [[PKCE]]
- [[Data & Credentials]]
- [[Knowledge/Authentication & Authorization/OAuth/Overview|Overview]]
- [[Security]]