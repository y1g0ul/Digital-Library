---
created-dt: 2026-09-14 15:55
tags:
  - review
sr-due: 2026-09-22
sr-interval: 5
sr-ease: 248
---
YAML - текстовый формат описания структурированных данных.

Часто используется для конфигурационных файлов:
- [[GitLab]] CI/CD;
- GitHub Actions;
- Docker Compose;
- Kubernetes;
- Ansible.

Главное правило - структура задаётся отступами.

Используются пробелы, а не табуляция.

### Ключ и значение

```yaml
name: app
port: 8080
enabled: true
```

Это пары:
```text
ключ: значение
```

---

### Вложенная структура

```yaml
server:
  host: localhost
  port: 8080
```

`host` и `port` находятся внутри `server`.

Можно представить так:
```text
server
├── host
└── port
```

---

### Список

Список обозначается через `-`:
```yaml
packages:
  - nginx
  - curl
  - git
```

---

### Список объектов

```yaml
servers:
  - name: web
    port: 80

  - name: database
    port: 5432
```

---

### Строки

Обычно кавычки не обязательны:
```yaml
name: production
```

Но их можно использовать:
```yaml
name: "production"
```

Кавычки полезны, если строка содержит специальные символы или может быть неправильно интерпретирована.

---

### Комментарии

```yaml
# Это комментарий

port: 8080
```

---

### Многострочный текст

```yaml
message: |
  First line
  Second line
  Third line
```

`|` сохраняет переносы строк.

---

### Пример обычного YAML

```yaml
application:
  name: my-app
  version: 1.0

  server:
    host: localhost
    port: 8080

  packages:
    - nginx
    - curl
    - git
```

Структура:

```text
application
├── name
├── version
├── server
│   ├── host
│   └── port
└── packages
    ├── nginx
    ├── curl
    └── git
```

### Пример `.gitlab-ci.yml`

```yaml
stages:
  - test
  - build
  - deploy

variables:
  APP_ENV: production

test:
  stage: test
  image: python:3.13

  script:
    - pip install -r requirements.txt
    - pytest

  rules:
    - if: '$CI_PIPELINE_SOURCE == "push"'

build:
  stage: build

  script:
    - docker build -t my-app .

  artifacts:
    paths:
      - dist/

deploy:
  stage: deploy

  script:
    - ./deploy.sh

  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: manual
```

Здесь YAML описывает структуру GitLab CI/CD:
```text
stages
├── test
├── build
└── deploy

variables
└── APP_ENV

test job
├── stage
├── image
├── script
└── rules

build job
├── stage
├── script
└── artifacts

deploy job
├── stage
├── script
└── rules
```

YAML сам по себе ничего не выполняет.

Он только описывает данные.

GitLab читает `.gitlab-ci.yml` и интерпретирует эти данные как конфигурацию CI/CD.