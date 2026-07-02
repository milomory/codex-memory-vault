# Снимок памяти с `codex-vds` перед закрытием (2026-07-01)

## Что проверено на хосте

- Хост: `codex-vds` (`38.54.84.49`, пользователь `anton`).
- Дата проверки: 2026-07-01 (локальное время Москвы).
- Сервис `memory-core`:
  - unit: `/home/anton/.config/systemd/user/memory-core.service`
  - status: `active (running)`
  - порт: `127.0.0.1:8765`
  - исполняемый процесс: `uvicorn memory_core.app:app`
  - режим запуска: `MEMORY_CORE_EMBEDDING_PROVIDER=hash` в unit по умолчанию + override
    из `EnvironmentFile=-/home/anton/.config/memory-core/env`.
  - реальный статус на момент проверки по `/embeddings/status`: `active: google_gemini`,
    `model: gemini-embedding-001@768`, `key_configured: true`.

## Что лежит в data / backup

- База: `/home/anton/.local/share/memory-core/memory-core.sqlite`
- Резервные копии:
  - `/home/anton/.local/share/memory-core/backups/memory-core-20260626-233448.sqlite`
  - `/home/anton/.local/share/memory-core/backups/memory-core-20260627-122830.sqlite`
- Бэкапы не копируются в `memory-vault`; хранится только путь и факт наличия.

## Структура и объём данных (снимок на 2026-07-01)

- `nodes`: 151
- `chunks`: 93
- `edges`: 200
- `events`: 1394
- `suggestions`: 1
- Логика проекта в `data_json`:
  - `project:/home/anton/Documents/OpenClaw/projects/memory-core` — 22
  - `project:openclaw-main` — 11
  - `project:/home/anton/Documents/OpenClaw/projects/main` — 9
  - `project:skystream-vpn-control` — 1
  - `project:codex-memory-vault` — 5 (в логе присутствуют ноды проекта)
  - `project:...` для `/home/anton/Documents/OpenCode/projects/server-admin` — 5

## Открытые события для продолжения контекста

- Последние события в БД включали:
  - подготовку takeover для `project:skystream-vpn-control`,
  - запись нескольких rollout-волн `project:laptop-shared-memory-rollout-*`,
  - финальную запись события `18:14 MSK` о работе через `laptop-bridge` и сетевой изоляции.
- Последнее действие `suggestions`:
  - `approved`, `project:/home/anton/Documents/OpenClaw/projects/memory-core`,
  - связь `Link memory-core to PROJECT_MEMORY_SYSTEM_SPEC.md` (все рекордные данные в локальной БД; не требуется вручную повторять).

## Что важно сохранить по архитектуре

- `memory-core` оформлялся как отдельный продукт/ядро памяти, не часть OpenClaw-кода.
- Источник истины — файловые репозитории; `memory-core` хранит индексы/ссылки/события/отношения.
- Важные инварианты:
  - `.memory-core.yml` по каждому проекту с `project_id`, `root`, `include/exclude`, `token_env`.
  - Проектные токены в отдельном файле вне git (`~/.config/memory-core/tokens/*.env`).
  - OpenClaw/Codex работают через API, не через прямой DB-write.
  - В идеале проекты подключаются через `memoryctl connect-project` / `init-project`.

## Критичный нюанс для ноутбук-бриджа

- `laptop-bridge codex-read/write` на `codex-vds` запускал Codex с `CODEX_SANDBOX_NETWORK_DISABLED=1`,
  поэтому прямой `curl http://127.0.0.1:8765` из среды sandbox-а был недоступен.
- Поэтому работал паттерн:
  - серверный OpenClaw читает память с `memory-core`,
  - прокидывает нужный контекст в prompt laptop Codex,
  - пишет краткие итоги/события обратно на сервер уже server-side.
- При запуске локальными процессами на Mac туннель `-L 127.0.0.1:8765:127.0.0.1:8765` делал endpoint доступным.

## Что уже перенесено в `memory-vault` на ноуте (до этого шага)

- Файлы/доки и решения из `OpenClaw/projects/main`:
  - `PROJECT_GRAPH_ARCHITECTURE.md`
  - `PROJECT_MEMORY_SYSTEM_SPEC.md`
  - `LAPTOP_PROJECTS_INTEGRATION.md`
  - `OPENCLAW_PROJECT_WORKFLOW.md`
  - `VPN_PROJECT_TAKEOVER.md`
  - `CODEX_PROJECT_INVENTORY_PROMPT.md`
- Обновлены:
  - `decisions/2026-07-01-memory-core-vds-offboarding.md`
  - `notes/memory-core-vds-transfer-2026-07-01.md`
  - `projects/codex-setup.md`
  - `projects/knowledge-memory-core.md`
  - `AGENTS.md`, `README.md`, `how-to-update-memory.md`, `TODO.md`
- Состояния хостов и правила: `infra/remote-codex-hosts.md`, `infra/codex-vds.md`, `infra/athena.md`.

Эта заметка нужна как компактный "портовый" архив, чтобы не потерять рабочую правду после закрытия `codex-vds`.
