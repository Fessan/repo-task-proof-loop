
# Справочник

Когда в примерах ниже упоминается `scripts/task_loop.py`, этот путь относительно корня навыка. Запускай его, находясь в рабочей директории внутри целевого репозитория.

Этот навык спроектирован переносимым, но локальные артефакты и файлы субагентов, которые он создаёт, должны оставаться в целевом репозитории.

## Рекомендуемые места установки

### Codex

Навык проекта:
- `.agents/skills/repo-task-proof-loop/`

Персональный навык:
- `$HOME/.agents/skills/repo-task-proof-loop/`

### Claude Code

Навык проекта:
- `.claude/skills/repo-task-proof-loop/`

Персональный навык:
- `~/.claude/skills/repo-task-proof-loop/`

Одна и та же директория навыка может использоваться в обоих продуктах. Скрипт инициализации записывает локальные файлы рабочего процесса в текущий репозиторий, а не в директорию навыка.

## Файлы репозитория, создаваемые `init`

```text
.agent/tasks/TASK_ID/
  spec.md
  evidence.md
  evidence.json
  raw/
    build.txt
    test-unit.txt
    test-integration.txt
    lint.txt
    screenshot-1.png
  verdict.json
  problems.md
```

Инициализатор также создаёт или обновляет эти интеграционные файлы на уровне проекта:

```text
.codex/agents/
  task-spec-freezer.toml
  task-builder.toml
  task-verifier.toml
  task-fixer.toml

.claude/agents/
  task-spec-freezer.md
  task-builder.md
  task-verifier.md
  task-fixer.md
```

И вставляет управляемый блок рабочего процесса в:

- `AGENTS.md`
- `CLAUDE.md`

Управляемый блок заменяется на месте при повторном запуске, поэтому пользовательский контент за пределами управляемых маркеров сохраняется.

## Команды

### Инициализация файлов рабочего процесса

```bash
scripts/task_loop.py init --task-id my-task
```

Заполнение задачи из файла:

```bash
scripts/task_loop.py init --task-id my-task --task-file docs/task.md
```

Заполнение задачи из текста:

```bash
scripts/task_loop.py init --task-id my-task --task-text "Реализовать фичу X"
```

Управление созданием или обновлением файлов руководства:

```bash
scripts/task_loop.py init --task-id my-task --guides auto
scripts/task_loop.py init --task-id my-task --guides both
scripts/task_loop.py init --task-id my-task --guides agents
scripts/task_loop.py init --task-id my-task --guides claude
scripts/task_loop.py init --task-id my-task --guides none
```

Управление установкой субагентов на уровне проекта:

```bash
scripts/task_loop.py init --task-id my-task --install-subagents both
scripts/task_loop.py init --task-id my-task --install-subagents codex
scripts/task_loop.py init --task-id my-task --install-subagents claude
scripts/task_loop.py init --task-id my-task --install-subagents none
```

### Валидация набора артефактов

```bash
scripts/task_loop.py validate --task-id my-task
```

### Сводка текущего статуса

```bash
scripts/task_loop.py status --task-id my-task
```

## Ожидаемый рабочий паттерн

1. Инициализация папки задачи
2. Заморозка спецификации
3. Реализация
4. Сбор доказательств
5. Свежая верификация
6. Исправление при необходимости
7. Повторная свежая верификация

Точные промпты для использования с дочерними агентами — см. `references/COMMANDS.md`.

## Примечания

- Инициализатор не записывает финальное содержимое `spec.md` за тебя. Он создаёт строгую файловую структуру и заполняет формулировку задачи, если она предоставлена. Собственно заморозка спецификации — это шаг агента.
- `evidence.json` и `verdict.json` создаются с валидным содержимым-заглушкой, чтобы валидация могла быть запущена сразу после `init`.
- `raw/screenshot-1.png` создаётся как маленький PNG-заглушка, чтобы необходимый путь существовал с самого начала.
