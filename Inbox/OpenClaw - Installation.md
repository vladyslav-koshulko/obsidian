# Встановлення

## Рекомендовано: скрипт встановлення

Найшвидший спосіб встановлення. Він визначає вашу ОС, встановлює Node за потреби, встановлює OpenClaw і запускає onboarding.

- macOS / Linux / WSL2   

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

Щоб встановити без запуску onboarding:

- macOS / Linux / WSL2
```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

Усі прапорці та параметри для CI/automation див. у [Внутрішня будова інсталятора](https://docs.openclaw.ai/install/installer).

## Системні вимоги

- **Node 24** (рекомендовано) або Node 22.14+ — скрипт встановлення обробляє це автоматично
- **macOS, Linux або Windows** — підтримуються як нативний Windows, так і WSL2; WSL2 стабільніший. Див. [Windows](https://docs.openclaw.ai/platforms/windows).
- `pnpm` потрібен лише якщо ви збираєте зі source

## Альтернативні способи встановлення

### Інсталятор з локальним префіксом (`install-cli.sh`)

Використовуйте це, якщо хочете зберігати OpenClaw і Node під локальним префіксом, таким як `~/.openclaw`, не покладаючись на загальносистемне встановлення Node:

```
curl -fsSL https://openclaw.ai/install-cli.sh | bash
```

Типово він підтримує встановлення через npm, а також встановлення з git checkout у межах того самого процесу з локальним префіксом. Повний довідник: [Внутрішня будова інсталятора](https://docs.openclaw.ai/install/installer#install-clish).

### npm, pnpm або bun

Якщо ви вже самі керуєте Node:

- npm

```
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

Troubleshooting: sharp build errors (npm)

### Зі source

Для контриб’юторів або всіх, хто хоче запускати з локального checkout:

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install && pnpm ui:build && pnpm build
pnpm link --global
openclaw onboard --install-daemon
```

Або пропустіть link і використовуйте `pnpm openclaw ...` зсередини репозиторію. Повні dev-процеси див. у [Налаштування](https://docs.openclaw.ai/start/setup).

### Встановлення з GitHub main

```
npm install -g github:openclaw/openclaw#main
```

## Перевірка встановлення

```
openclaw --version      # confirm the CLI is available
openclaw doctor         # check for config issues
openclaw gateway status # verify the Gateway is running
```

Якщо ви хочете керований запуск після встановлення:

- macOS: LaunchAgent через `openclaw onboard --install-daemon` або `openclaw gateway install`
- Linux/WSL2: systemd user service через ті самі команди
- Нативний Windows: спочатку Scheduled Task, із резервним варіантом per-user login item у теці Startup, якщо створення завдання відхилено

## Усунення несправностей: `openclaw` не знайдено

Якщо встановлення пройшло успішно, але `openclaw` не знаходиться у вашому терміналі:

```
node -v           # Node installed?
npm prefix -g     # Where are global packages?
echo "$PATH"      # Is the global bin dir in PATH?
```

Якщо `$(npm prefix -g)/bin` немає у вашому `$PATH`, додайте його до файлу запуску shell (`~/.zshrc` або `~/.bashrc`):

```
export PATH="$(npm prefix -g)/bin:$PATH"
```

Потім відкрийте новий термінал. Докладніше див. у [Налаштування Node](https://docs.openclaw.ai/install/node).


# Resources
- [Встановлення](https://docs.openclaw.ai/uk/install#%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%BD%D1%96-%D0%B2%D0%B8%D0%BC%D0%BE%D0%B3%D0%B8)

# Связи
[[OpenClaw - Overview]]