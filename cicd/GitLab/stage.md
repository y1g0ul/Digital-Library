---
created-dt: 2026-09-11 14:48
tags:
  - review
---
Этап [[cicd/GitLab/pipeline|pipeline]] в [[GitLab]].

[[cicd/GitLab/job|job]]s можно объединять в stages, чтобы задать общий порядок выполнения.

Пример:
```yaml
stages:
  - test
  - build
  - deploy
```

Pipeline будет идти так:
```text
test
 ↓
build
 ↓
deploy
```

Job указывает свой stage:
```yaml
unit_tests:
  stage: test
  script:
    - pytest

build_image:
  stage: build
  script:
    - docker build -t app .

deploy:
  stage: deploy
  script:
    - ./deploy.sh
```

Получается:
```text
Pipeline
│
├── test
│   └── unit_tests
│
├── build
│   └── build_image
│
└── deploy
    └── deploy
```

Если в одном stage несколько jobs, они могут выполняться параллельно:
```yaml
stages:
  - test

unit_tests:
  stage: test
  script:
    - pytest tests/unit

lint:
  stage: test
  script:
    - ruff check .
```

```text
test
├── unit_tests
└── lint
```

Следующий stage по умолчанию начинается только после успешного завершения предыдущего.

То есть:
```text
Stage = группа jobs одного этапа
```

Например:
```text
test
build
deploy
```

Stage задаёт общий порядок pipeline, а конкретную работу выполняют уже jobs.