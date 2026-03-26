# Схемы артефактов

Обязательные файлы для каждой папки задачи:

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

## `evidence.json`

Обязательные ключи верхнего уровня:

- `task_id`
- `overall_status`
- `acceptance_criteria`
- `changed_files`
- `commands_for_fresh_verifier`
- `known_gaps`

Допустимые значения статуса:

- `PASS`
- `FAIL`
- `UNKNOWN`

Рекомендуемая структура:

```json
{
  "task_id": "my-task",
  "overall_status": "UNKNOWN",
  "acceptance_criteria": [
    {
      "id": "AC1",
      "text": "Описание критерия",
      "status": "UNKNOWN",
      "proof": [
        {
          "type": "command",
          "path": ".agent/tasks/my-task/raw/test-unit.txt",
          "command": "npm test -- --runInBand",
          "exit_code": 0,
          "summary": "Точечные юнит-тесты пройдены."
        }
      ],
      "gaps": []
    }
  ],
  "changed_files": [],
  "commands_for_fresh_verifier": [],
  "known_gaps": []
}
```

## `verdict.json`

Обязательные ключи верхнего уровня:

- `task_id`
- `overall_verdict`
- `criteria`
- `commands_run`
- `artifacts_used`

Допустимые значения статуса:

- `PASS`
- `FAIL`
- `UNKNOWN`

Рекомендуемая структура:

```json
{
  "task_id": "my-task",
  "overall_verdict": "UNKNOWN",
  "criteria": [
    {
      "id": "AC1",
      "status": "UNKNOWN",
      "reason": "Ещё не верифицировано."
    }
  ],
  "commands_run": [],
  "artifacts_used": []
}
```

## `problems.md`

Обязательные разделы для каждого критерия со статусом не `PASS`:

- идентификатор и текст критерия
- статус
- почему не доказан
- минимальные шаги воспроизведения
- ожидаемое vs фактическое
- затронутые файлы
- минимально безопасное исправление
- корректирующая подсказка в 1-3 предложениях

## Скрипт валидации

Запуск:

```bash
scripts/task_loop.py validate --task-id <TASK_ID>
```

Проверяет:

- наличие обязательных файлов
- парсируемость JSON
- наличие ключей верхнего уровня
- допустимые значения статуса
- согласованность task id
