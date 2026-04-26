#auth #implementation #node-js
# OAuth Implementation - Node.js
## 💻 Пример реализации (Node.js — базово)

### 📦 Шаг 1 — генерируем URL

const querystring = require("querystring");  
  
function getAuthUrl() {  
  const params = {  
    client_id: process.env.CLIENT_ID,  
    redirect_uri: "http://localhost:3000/callback",  
    response_type: "code",  
    scope: "email profile",  
    state: "random_string"  
  };  
  
  return "https://accounts.google.com/o/oauth2/v2/auth?" + querystring.stringify(params);  
}

---

### 📦 Шаг 2 — обработка callback

app.get("/callback", async (req, res) => {  
  const { code, state } = req.query;  
  
  // validate state  
  if (state !== expectedState) {  
    return res.send("Invalid state");  
  }  
  
  const token = await exchangeCode(code);  
  
  res.send("Login success");  
});

---

### 📦 Шаг 3 — обмен code → token

const axios = require("axios");  
  
async function exchangeCode(code) {  
  const response = await axios.post(  
    "https://oauth2.googleapis.com/token",  
    {  
      code,  
      client_id: process.env.CLIENT_ID,  
      client_secret: process.env.CLIENT_SECRET,  
      redirect_uri: "http://localhost:3000/callback",  
      grant_type: "authorization_code"  
    }  
  );  
  
  return response.data;  
}

---

### 📦 Шаг 4 — использование access_token

async function getUserInfo(accessToken) {  
  const res = await axios.get(  
    "https://www.googleapis.com/oauth2/v2/userinfo",  
    {  
      headers: {  
        Authorization: `Bearer ${accessToken}`  
      }  
    }  
  );  
  
  return res.data;  
}

---

### 🧠 Итог Flow

1. Redirect → Google  
2. Login + consent  
3. Receive code  
4. Exchange code → access_token  
5. Access API

---

# 🔗 Связи  
  
- [[OAuth - Overview]]  
- [[OAuth - Security]]  
- [[OAuth - Data & Credentials]]
- [[OAuth - Resources]]
- [[OAuth - Use Cases]]
- [[OAuth - Authorization Code Flow]]
- [[OAuth - Attacks]]