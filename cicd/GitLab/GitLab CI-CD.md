---
created-dt: 2026-09-11 13:47
tags:
  - review
---
Pipeline - конкретный запуск CI/CD-процесса в [[GitLab]].

Он создаётся после определённого события:
- [[push]];
- создание или обновление [[merge|Merge]] Request;
- запуск по расписанию;
- ручной запуск;
- API-вызов;
- запуск другим [[cicd/GitLab/pipeline|pipeline]].

Pipeline описывается через `.gitlab-ci.yml` и состоит из [[cicd/GitLab/job|job]]s, которые обычно объединяются в [[stage]]s.

Пример:
```yaml
stages:
  - test
  - deploy

test:
  stage: test
  script:
    - pytest

deploy:
  stage: deploy
  script:
    - ./deploy.sh
```

Получится:
```text
Pipeline
│
├── test
│   └── test
│
└── deploy
    └── deploy
```

Сначала выполняется stage `test`, затем `deploy`.

Jobs одного stage могут выполняться параллельно, а следующий stage по умолчанию начинается после успешного завершения предыдущего.

### Как создаётся pipeline

При возникновении события GitLab читает `.gitlab-ci.yml`.

Общая логика:
```text
Событие
   ↓
.gitlab-ci.yml
   ↓
workflow:rules
   ↓
Создавать pipeline?
   ↓
rules jobs
   ↓
Pipeline из подходящих jobs
```

`workflow:rules` определяет, нужно ли создавать **весь pipeline**.

`rules` внутри job определяет, должна ли **конкретная job** попасть в него.

Например:
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

При push в `dev`:
```text
Pipeline
└── test
```

При push в `main`:
```text
Pipeline
└── deploy
```

Если ни одно `workflow:rule` не подходит, pipeline вообще не создаётся.

### GitHub Actions и GitLab

```text
GitHub Actions:

Event
 ↓
Workflow
 ↓
Jobs
 ↓
Steps
```

```text
GitLab CI/CD:

Event
 ↓
Pipeline
 ↓
Stages
 ↓
Jobs
 ↓
script
```

Главная идея: в GitLab `.gitlab-ci.yml` описывает возможную конфигурацию CI/CD, а при конкретном событии GitLab формирует из неё подходящий pipeline.