---
created-dt: 2026-09-14 15:34
tags:
  - review
---
Условия, которые определяют, должна ли конкретная job попасть в pipeline GitLab.

Пример:
```yaml
deploy:
  script:
    - ./deploy.sh

  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

Job `deploy` попадёт в pipeline только если текущая ветка - `main`.

Общая логика:
```text
Событие
  ↓
Pipeline
  ↓
GitLab проверяет rules каждой job
  ↓
Подходящие jobs добавляются в pipeline
```

### `if`

Позволяет проверить условие:
```yaml
rules:
  - if: '$CI_COMMIT_BRANCH == "main"'
```

Можно использовать встроенные CI/CD variables:
```text
CI_COMMIT_BRANCH
CI_PIPELINE_SOURCE
CI_COMMIT_TAG
```

Например запускать job только для Merge Request:
```yaml
rules:
  - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

---

### Несколько правил

`rules` проверяются сверху вниз.

```yaml
rules:
  - if: '$CI_COMMIT_BRANCH == "main"'
  - if: '$CI_COMMIT_BRANCH == "develop"'
```

GitLab использует первое совпавшее правило.

---

### `changes`

Job можно запускать только при изменении определённых файлов:
```yaml
rules:
  - changes:
      - backend/**
```

Например backend-тесты не будут запускаться, если изменился только frontend.

---

### `exists`

Можно проверить наличие файла:
```yaml
rules:
  - exists:
      - Dockerfile
```

Job попадёт в pipeline только если файл существует.

---

### `when`

Через `when` можно определить поведение job:
```yaml
rules:
  - if: '$CI_COMMIT_BRANCH == "main"'
    when: manual
```

Теперь job появится в pipeline, но запустится только вручную.

Частые значения:
```text
on_success
manual
always
never
```

Например:
```yaml
rules:
  - if: '$CI_COMMIT_BRANCH == "main"'
    when: on_success

  - when: never
```

---

Главная идея:
```text
rules = фильтр для job
```

То есть:
```text
pipeline создан
  ↓
rules проверяются
  ↓
job подходит?
  ↓
да → добавить в pipeline
нет → не добавлять
```

Не путать с:
```text
workflow:rules
```

`workflow:rules` определяет, создавать ли весь pipeline.

Обычные `rules` определяют, добавлять ли конкретную job.