---
created-dt: 2026-09-14 15:54
tags:
  - review
sr-due: 2026-09-18
sr-interval: 3
sr-ease: 250
---
`workflow:rules` - глобальные правила, которые определяют, нужно ли вообще создавать [[cicd/GitLab/pipeline|pipeline]].

Пример:
```yaml
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "push"'
```

Такой pipeline будет создаваться только при `push`.

Общая логика:
```text
Событие
  ↓
workflow:rules
  ↓
Создавать pipeline?
  ↓
да / нет
```

Если ни одно правило не подходит, pipeline не создаётся.

### Пример

```yaml
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH == "main"'
```

Pipeline будет создан:
- для Merge Request;
- для ветки `main`.

### Отличие от обычных `rules`

```text
workflow:rules
→ создавать ли весь pipeline

job:rules
→ добавлять ли конкретную job
```

Пример:
```yaml
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "push"'

test:
  script:
    - pytest

  rules:
    - if: '$CI_COMMIT_BRANCH != "main"'

deploy:
  script:
    - ./deploy.sh

  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

Логика:
```text
push
 ↓
workflow:rules
 ↓
pipeline создан
 ↓
rules каждой job
 ↓
GitLab определяет состав pipeline
```

Главная идея:
```text
workflow:rules = фильтр всего pipeline
rules = фильтр отдельной job
```