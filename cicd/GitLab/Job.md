---
created-dt: 2026-09-14 14:29
tags:
  - review
sr-due: 2026-10-15
sr-interval: 20
sr-ease: 250
---
Отдельная задача внутри pipeline в GitLab.

Обычно одна job выполняет одну конкретную цель:
- запустить тесты;
- собрать приложение;
- собрать [[docker]] [[image]];
- выполнить lint;
- задеплоить приложение.

Пример:
```yaml
test:
  stage: test
  script:
    - pytest
```

Здесь:
```text
test
```

- имя job.

```yaml
stage: test
```

- stage, к которому она относится.

```yaml
script:
  - pytest
```

- команды, которые она выполняет.

Общая структура:
```text
Pipeline
└── Stage
    └── Job
        └── script
```

В отличие от GitHub Actions, где [[cicd/GitHub Actions/Job|Job]] обычно состоит из отдельных `steps`:
```text
Job
├── Step
├── Step
└── Step
```

в GitLab внутри job чаще просто выполняется список команд:
```yaml
build:
  stage: build
  script:
    - npm install
    - npm run build
    - echo "Build finished"
```

То есть:
```text
Job
└── script
    ├── command
    ├── command
    └── command
```

### Где выполняется job

Job выполняет GitLab Runner.

Схема:
```text
GitLab
  ↓
Job
  ↓
Runner
  ↓
Выполнение script
```

Runner может выполнять job, например:
- напрямую в системе через Shell;
- внутри Docker [[container]];
- в Kubernetes.


Например:
```yaml
test:
  image: python:3.13
  script:
    - pip install -r requirements.txt
    - pytest
```

[[cicd/GitLab/Runner|Runner]] запускает окружение с образом `python:3.13` и выполняет внутри него `script`.

### Условия запуска

Job может иметь `rules`, которые определяют, должна ли она попасть в pipeline:
```yaml
deploy:
  stage: deploy
  script:
    - ./deploy.sh

  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

Эта job попадёт в pipeline только при выполнении условия.

Коротко:
```text
Job = отдельная задача pipeline
```

Она:
```text
относится к stage
↓
может иметь rules
↓
передаётся Runner
↓
выполняет script
```