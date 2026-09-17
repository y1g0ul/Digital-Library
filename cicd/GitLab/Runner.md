---
created-dt: 2026-09-14 16:04
tags:
  - review
sr-due: 2026-09-18
sr-interval: 1
sr-ease: 228
---
Runner - компонент, который получает [[cicd/GitLab/Job|Job]] от GitLab и выполняет её. [[GitLab]] сам по себе не выполняет команды из `script`.

Схема:
```text
GitLab
  ↓
Pipeline
  ↓
Job
  ↓
Runner
  ↓
Выполнение команд
```

Runner постоянно связывается с GitLab и забирает подходящие jobs.

### Как выбирается Runner

Runner может быть:
- общим для нескольких проектов;
- привязанным к конкретному проекту;
- привязанным к группе проектов.

Для выбора Runner можно использовать `tags`.

Например:
```yaml
deploy:
  tags:
    - production

  script:
    - ./deploy.sh
```

Эту job сможет забрать Runner с подходящим тегом `production`.

### Executor

Runner - это не само окружение выполнения.

Он использует **executor**, который определяет, где и как будет выполняться job.

Частые варианты:
```text
Runner
├── Shell executor
├── Docker executor
└── Kubernetes executor
```

Например при Docker executor:
```yaml
test:
  image: python:3.13

  script:
    - pytest
```

Процесс примерно такой:

```text
GitLab
  ↓
Runner получает job
  ↓
Runner запускает container python:3.13
  ↓
Выполняет script
  ↓
Возвращает результат GitLab
```

При Shell executor команды выполняются прямо в системе Runner:
```text
Runner host
  ↓
bash
  ↓
script
```

### Shared и self-hosted Runner

Runner может предоставляться GitLab или быть установлен самостоятельно.

Self-hosted Runner часто используют, когда нужен:
- доступ к внутренней сети;
- доступ к production-серверам;
- Docker daemon;
- Kubernetes cluster;
- специальное ПО или оборудование;
- собственные ресурсы.

Например:
```text
GitLab
   ↓
Self-hosted Runner
   ↓
внутренняя сеть компании
   ↓
production server
```

Главная идея:
```text
GitLab описывает и координирует CI/CD
Runner физически выполняет jobs
```

Не путать:
```text
Runner
→ исполнитель jobs

Executor
→ способ, которым Runner запускает job
```