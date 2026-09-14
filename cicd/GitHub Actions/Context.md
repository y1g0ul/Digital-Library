---
created-dt: 2026-08-04 09:47
tags:
  - review
sr-due: 2026-10-31
sr-interval: 53
sr-ease: 250
---
Набор информации о текущем запуске [[Workflow]], которую GitHub Actions
предоставляет во время выполнения. Используется через выражение `${{ context.property }}`

**`Основные Contexts`**
- `github` - Информация о репозитории, событии, коммите, [[Workflow]] и т.д.
- `env` - Переменные окружения
- `vars` - Переменные конфигурации
- `secrets` - Секреты
- `inputs` - Входные параметры workflow_dispatch / workflow_call
- `runner` - Информация о [[cicd/GitHub Actions/Runner]]
- `job` - Информация о текущем [[cicd/GitHub Actions/Job|Job]]
- `steps` - Информация о выполненных [[Step]]s
- `needs` - Результаты и данные [[cicd/GitHub Actions/Job|Job]], от которых зависит текущий [[cicd/GitHub Actions/Job|Job]]
- `matrix` - Значения текущей Matrix-конфигурации

``` yml
# Ветка, на которой запущен Workflow
${{ github.ref }}

# Название репозитория
${{ github.repository }}

# Имя пользователя
${{ github.actor }}

# Input
${{ inputs.environment }}

# Secret
${{ secrets.API_KEY }}

# Переменная
${{ vars.APP_NAME }}

# Результат предыдущего Step
${{ steps.build.outputs.version }}

```

