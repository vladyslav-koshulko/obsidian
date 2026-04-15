# Полный список технологий, протоколов и концепций аутентификации и авторизации:

---

## 1. Протоколы и стандарты

| Технология                               | Описание                                    | Когда использовать                      |
| ---------------------------------------- | ------------------------------------------- | --------------------------------------- |
| **[[Knowledge/Authentication & Authorization/OAuth/Overview]]**                        | Стандарт авторизации, делегирование доступа | Вход через Google/GitHub, API доступ    |
| **[[OIDC]] (OpenID Connect)**            | Надстройка OAuth 2.0 для аутентификации     | SSO, получение профиля пользователя     |
| **SAML 2.0**                             | XML-based SSO для enterprise                | Корпоративные интеграции, ADFS          |
| **LDAP**                                 | Доступ к директориям (Active Directory)     | Внутренние корпоративные системы        |
| **Kerberos**                             | Сетевой протокол аутентификации             | Windows домены, внутренние сети         |
| **CAS (Central Authentication Service)** | Протокол SSO для веба                       | Университеты, старые enterprise системы |
| **FIDO2 / WebAuthn**                     | Стандарт безпарольной аутентификации        | Passkeys, аппаратные ключи (YubiKey)    |
| **U2F (Universal 2nd Factor)**           | Двухфакторка через USB/NFC                  | Физические ключи безопасности           |

---

## 2. Токены и форматы

| Технология                                         | Описание                                    | Особенности                            |
| -------------------------------------------------- | ------------------------------------------- | -------------------------------------- |
| **[[JWT]] (JSON Web Token)**                       | Подписанные JSON-claims                     | Без state, RS256/HS256/ES256           |
| **PASETO**                                         | Альтернатива JWT без алгоритмических ошибок | Безопаснее JWT по умолчанию            |
| **JWE (JSON Web Encryption)**                      | Шифрованные JWT                             | Конфиденциальность данных в токене     |
| **JWS (JSON Web Signature)**                       | Подписанные JWT                             | Целостность данных                     |
| **[[JWKS]] (JSON Web Key Set)** | Набор публичных ключей для валидации        | Ротация ключей без обновления клиентов |
| **MAC (Message Authentication Code)**              | HMAC, CMAC для целостности                  | Симметричная проверка                  |
| **Opaque Tokens**                                  | Случайные строки, проверяются в БД          | Отзыв мгновенный, требует state        |
| **DPoP (Demonstrating Proof-of-Possession)**       | Привязка токена к ключу клиента             | Защита от stolen token                 |

---

## 3. Хеширование и криптография

|Технология|Описание|Рекомендации|
|---|---|---|
|**Argon2id**|Современное хеширование паролей|**Рекомендуется OWASP 2023**|
|**bcrypt**|Адаптивное хеширование|Устаревает, но всё ещё безопасен|
|**scrypt**|Память-зависимое хеширование|Для криптовалют, embedded|
|**PBKDF2**|Ключевой derivation|FIPS compliant, но медленнее Argon2|
|**SHA-3 / Keccak**|Хеш-функция последнего поколения|Для checksum, не для паролей|
|**SHA-256/512**|Хеш-функция|HMAC, подписи, не для паролей|
|**AES-256-GCM**|Симметричное шифрование|Шифрование сессий, данных|
|**ChaCha20-Poly1305**|Потоковое шифрование|Быстрее AES на mobile/без AES-NI|
|**RSA-2048/4096**|Асимметричная криптография|Подписи, legacy системы|
|**ECDSA / Ed25519**|Эллиптические кривые|Компактные подписи, современно|
|**X25519**|ECDH ключевой обмен|Forward secrecy|

---

## 4. Методы аутентификации (Factors)

|Тип|Технологии|Примеры|
|---|---|---|
|**Knowledge** (знает)|Пароли, PIN, секретные вопросы|Традиционный вход|
|**Possession** (имеет)|OTP, SMS, TOTP, hardware keys|Google Authenticator, YubiKey|
|**Inherence** (является)|Биометрия, Face ID, Touch ID|Face recognition, fingerprint|
|**Location** (где)|IP-geo, device fingerprinting|Проверка локации|
|**Behavior** (как ведёт)|Keystroke dynamics, mouse patterns|Behavioral biometrics|

---

## 5. MFA / 2FA Технологии

|Технология|Как работает|Безопасность|
|---|---|---|
|**TOTP (Time-based OTP)**|RFC 6238, 6-цифр код каждые 30 сек|Хорошо, но фишинг возможен|
|**HOTP (HMAC-based OTP)**|Счётчик-based, RFC 4226|Устаревает, проблемы синхронизации|
|**WebAuthn / FIDO2**|Асимметричная криптография|**Фишинг-резистант**|
|**SMS OTP**|Код в SMS|**Не рекомендуется** (SIM swap)|
|**Push Notification**|Подтверждение на телефоне|Удобно, но MFA fatigue атаки|
|**Backup Codes**|Одиноразовые коды|Recovery access|

---

## 6. Модели авторизации

|Модель|Описание|Применение|
|---|---|---|
|**RBAC (Role-Based)**|Роли → Права|Простые системы, админ/юзер|
|**ABAC (Attribute-Based)**|Политики на основе атрибутов|Сложные enterprise, XACML|
|**PBAC (Policy-Based)**|Внешние policy engines|OPA, Cedar|
|**ReBAC (Relationship-Based)**|Отношения между сущностями|Google Zanzibar, соцсети|
|**ACL (Access Control List)**|Список разрешений на объект|Файловые системы|
|**DAC / MAC**|Discretionary / Mandatory|ОС уровень, SELinux|
|**Scope-based**|OAuth scopes|API доступ (read:profile)|

---

## 7. Безопасность сессий и cookies

|Технология|Назначение|
|---|---|
|**HttpOnly cookies**|Недоступно для JavaScript|
|**Secure cookies**|Только HTTPS|
|**SameSite=Strict/Lax**|CSRF защита|
|**__Host- prefix**|Привязка к хосту|
|**__Secure- prefix**|Требует Secure флаг|
|**Session fixation protection**|Регенерация ID после login|
|**Token binding**|Привязка к TLS channel|

---

## 8. Защита от атак

|Атака|Защитные технологии|
|---|---|
|**CSRF**|SameSite cookies, CSRF tokens, Double-submit|
|**XSS**|CSP, HttpOnly cookies, input sanitization|
|**SQL Injection**|Prepared statements, ORM|
|**Timing attacks**|Constant-time comparison|
|**Replay attacks**|Nonce, timestamps, sequence numbers|
|**Brute force**|Rate limiting, account lockout, CAPTCHA|
|**Session hijacking**|Fingerprinting, IP validation, short sessions|
|**Clickjacking**|X-Frame-Options, CSP frame-ancestors|
|**MITM**|TLS 1.3, certificate pinning (deprecated)|

---

## 9. Инфраструктура и инструменты

|Категория|Инструменты|
|---|---|
|**Identity Providers**|Auth0, Okta, Keycloak, FusionAuth, AWS Cognito, Azure AD|
|**API Gateways**|Kong, Ambassador, Istio (auth at edge)|
|**Policy Engines**|Open Policy Agent (OPA), AWS Cedar, Google Zanzibar|
|**Secret Management**|HashiCorp Vault, AWS Secrets Manager, 1Password Secrets|
|**PKI**|Let's Encrypt, CFSSL, AWS ACM|
|**Monitoring**|Falco (runtime), Audit logs, SIEM|

---

## 10. Современные подходы (Passwordless)

|Подход|Технология|Статус|
|---|---|---|
|**Passkeys**|FIDO2/WebAuthn|Apple, Google, Microsoft внедряют|
|**Magic Links**|Email-based, JWT в URL|Удобно, но email dependency|
|**QR Code login**|Device flow|Discord, WhatsApp Web|
|**Biometric login**|WebAuthn platform authenticator|Touch ID, Face ID, Windows Hello|
|**Decentralized ID (DID)**|Self-sovereign identity|W3C стандарт, Web3|

---

## 11. Связанные концепции

|Концепция|Связь с AuthN/AuthZ|
|---|---|
|**CORS (Cross-Origin)**|Разрешение запросов между доменами|
|**CSP (Content Security Policy)**|Защита от XSS, ограничение script sources|
|**HSTS**|Принудительный HTTPS|
|**Certificate Transparency**|Мониторинг TLS сертификатов|
|**mTLS (Mutual TLS)**|Клиентская аутентификация по сертификату|
|**SPIFFE/SPIRE**|Service-to-service identity в микросервисах|
|**Zero Trust**|Модель "никогда не доверяй, всегда проверяй"|

---

**Рекомендации для выбора стека:**

- **Новый проект**: OAuth2 + OIDC + JWT + Argon2id + WebAuthn (passkeys)
    
- **Enterprise**: SAML 2.0 + LDAP + RBAC + OPA
    
- **Микросервисы**: SPIFFE + mTLS + short-lived JWT
    
- **API-first**: OAuth 2.0 + DPoP + scope-based auth

---
# Заметки
OAuth ≠ login  
OIDC = login  
JWT = источник истины о пользователе  
JWKS = доверие к подписи  
Session = удобство использования

---
# Structure
- [[JWKS]]
- [[JWT]]
- [[Knowledge/Authentication & Authorization/OAuth/Overview]]
- [[OIDC]]
- [[PKCE]]
- SSO
- [[Production Authentication]]
- [[JWKS]]
- SCRF
- CORS
