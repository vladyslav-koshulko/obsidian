# 🚨 Authorization Code Reuse

## 📌 Суть

Повторное использование `code`

---

## 💥 Атака

1. attacker перехватил code  
2. использует его повторно

---

## ✅ Защита

- code одноразовый
- короткое время жизни
- PKCE

# Связи
- [[PKCE]]
- [[OAuth - Overview|OAuth - Overview]]
- [[OAuth - Data & Credentials]]
- [[OAuth - Attacks]]
- [[OAuth - Security]]