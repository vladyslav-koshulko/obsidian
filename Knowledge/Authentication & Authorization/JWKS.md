#auth #jwks
**JWKS - JSON Web Key Set**

## 📌 Что это

Endpoint с публичными ключами:

https://www.googleapis.com/oauth2/v3/certs

---

## 📌 Зачем

JWT содержит:

{  
  "kid": "abc123"  
}

👉 мы:

1. берём `kid`
2. находим ключ в JWKS
3. проверяем подпись


# 🧠 Реализация JWT валидации

---

## Шаги

1. decode header  
2. extract kid  
3. fetch JWKS  
4. find matching key  
5. build RSA key  
6. validate JWT

---

## Код (логика)

```
let kid = extract_kid(id_token);  
let jwks = fetch_google_jwks();  
  
let jwk = find_key(kid);  
  
let decoding_key = DecodingKey::from_rsa_components(n, e);  
  
decode(token, decoding_key, validation);
```



# Связи
- [[JWT]]