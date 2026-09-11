---
created-dt: 2026-09-07 13:54
tags:
  - review
sr-due: 2026-09-12
sr-interval: 2
sr-ease: 230
---
GitLab имеет встроенную систему CI/CD, которая описывается в файле `.gitlab-ci.yml`.

В отличие от GitHub Actions, где обычно создаётся несколько отдельных [[Workflow]] под разные события, в GitLab чаще описывается общий набор [[cicd/GitLab/job|job]], а при возникновении события GitLab определяет, какие из них должны попасть в конкретный [[cicd/GitLab/pipeline|pipeline]].

Общая схема:
```text
Событие
   ↓
.gitlab-ci.yml
   ↓
workflow:rules
   ↓
Создавать pipeline?
   ↓
rules отдельных job
   ↓
Подходящие jobs
   ↓
Pipeline
   ↓
Stages
   ↓
Runner выполняет jobs
```

Основные сущности:
- **Pipeline** - конкретный запуск CI/CD-процесса.
- **Stage** - этап pipeline, например `test`, `build`, `deploy`.
- **Job** - отдельная задача внутри stage.
- **script** - команды, которые выполняет job.
- **rules** - условия, определяющие, должна ли job попасть в pipeline.
- **workflow:rules** - условия создания всего pipeline.
- **Runner** - машина или процесс, который выполняет jobs.

Например:
```yaml
stages:
  - test
  - deploy

test:
  stage: test
  script:
    - pytest
  rules:
    - if: '$CI_PIPELINE_SOURCE == "push"'

deploy:
  stage: deploy
  script:
    - ./deploy.sh
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

При push GitLab создаёт pipeline и на основе `rules` определяет, какие jobs должны в него войти.

Основная модель:
```text
GitHub Actions:
Event → Workflow → Jobs → Steps

GitLab CI/CD:
Event → Pipeline → Stages → Jobs → script
```