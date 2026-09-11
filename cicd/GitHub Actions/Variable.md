---
created-dt: 2026-08-04 10:35
tags:
  - review
sr-due: 2026-09-23
sr-interval: 12
sr-ease: 231
---
Значение, используемое для хранения настроек и данных [[Workflow]].

## Типы

env
→ переменные окружения, заданные в [[Workflow]].
`${{ env.MY_VARIABLE }}`

vars
→ configuration variables, заданные на уровне:
  Repository / Organization / Environment.
`${{ vars.MY_VARIABLE }}`

github
→ встроенные переменные с информацией о Workflow и событии.
`${{ github.repository }}`

Configuration Variable:
- только буквы, цифры и `_`;
- не может начинаться с `GITHUB_`;
- не может начинаться с цифры;
- имена не чувствительны к регистру;
- максимум 48 KB на одну Variable.

Переменные `GITHUB_*` и `RUNNER_*` нельзя переопределять.

## Приоритет vars

Environment
  ↓
Repository
  ↓
Organization

Более низкий уровень имеет приоритет.

| Variable             | Что содержит                                                        |
| -------------------- | ------------------------------------------------------------------- |
| `GITHUB_ACTOR`       | Пользователь или приложение, запустившее [[Workflow]]               |
| `GITHUB_EVENT_NAME`  | Событие, которое запустило Workflow (`push`, `pull_request` и т.д.) |
| `GITHUB_REF`         | Полный ref ветки или тега, запустившего Workflow                    |
| `GITHUB_REF_NAME`    | Короткое имя ветки или тега                                         |
| `GITHUB_REF_TYPE`    | Тип ref: `branch` или `tag`                                         |
| `GITHUB_REPOSITORY`  | `owner/repository`                                                  |
| `GITHUB_SHA`         | SHA коммита, запустившего Workflow                                  |
| `GITHUB_WORKFLOW`    | Название Workflow                                                   |
| `GITHUB_RUN_ID`      | Уникальный ID запуска Workflow                                      |
| `GITHUB_RUN_NUMBER`  | Номер запуска Workflow                                              |
| `GITHUB_RUN_ATTEMPT` | Номер попытки запуска при повторном запуске                         |
| `GITHUB_JOB`         | ID текущего Job                                                     |
| `GITHUB_WORKSPACE`   | Рабочая директория репозитория на Runner                            |
| `RUNNER_OS`          | ОС Runner: `Linux`, `Windows` или `macOS`                           |
| `RUNNER_ARCH`        | Архитектура Runner: `X64`, `ARM64` и т.д.                           |
| `RUNNER_ENVIRONMENT` | Тип Runner: `github-hosted` или `self-hosted`                       |

`$GITHUB_REF` -  переменная окружения, которую GitHub Actions предоставляет процессу, выполняющему [[Step]]. Поэтому она доступна внутри `run`, но сама по себе не является GitHub Actions expression и не существует на этапе обработки YAML.
`${{ github.ref }}` - GitHub Actions expression. GitHub Actions вычисляет её до запуска shell-команды и подставляет получившееся значение в команду.

``` bash
steps:
  - name: Set variable
    run: |
      echo "NEW_CUSTOM_VAR=New Value" >> "$GITHUB_ENV"

  - name: Use variable
    run: |
      echo "$NEW_CUSTOM_VAR"
```

`$GITHUB_ENV` используется для того, чтобы передать переменную в последующие steps.

