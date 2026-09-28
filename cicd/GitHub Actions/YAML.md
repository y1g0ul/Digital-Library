---
created-dt: 2026-07-29 12:21
tags:
  - review
sr-due: 2026-12-23
sr-interval: 86
sr-ease: 247
---
YAML Ain't Markup Language - язык сериализации данных, используемый для хранения конфигурации в удобном для чтения виде.

В DevOps YAML применяется для описания инфраструктуры, [[cicd/GitHub Actions/Pipeline|Pipeline]] и конфигурационных файлов.

Основные правила

**`Отступы`**
YAML использует пробелы, а не фигурные скобки или табуляцию.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```
> Используйте только пробелы. Табуляция (`Tab`) недопустима.

**`Ключ: значение`**
```yaml
name: Build
```

**`Списки`**
```yaml
steps:
  - name: Checkout
  - name: Build
  - name: Test
```

**`Вложенность`**
```yaml
job:
  steps:
    - name: Build
```
Вложенность определяется количеством пробелов.

**`Типы данных`**

Строка
```yaml
name: Build
```

Число
```yaml
version: 3
```

Логическое значение
```yaml
enabled: true
```

Список
```yaml
ports:
  - 80
  - 443
```

Словарь
```yaml
database:
  host: localhost
  port: 5432
```


``` yaml
# Комментарий

# Ключ: значение
name: My Project
version: 1
enabled: true
price: 15.5

# null
description: null

# Строки
short: Hello
quoted: "Hello World"
multiline: |
  Первая строка
  Вторая строка
  Третья строка

folded: >
  Эта строка
  будет объединена
  в одну.

# Список
ports:
  - 80
  - 443
  - 8080

# Список объектов
users:
  - name: Nikita
    role: admin

  - name: Alex
    role: developer

# Словарь (Map)
database:
  host: localhost
  port: 5432
  user: postgres
  password: secret

# Вложенные структуры
app:
  name: API
  replicas: 3

  resources:
    cpu: "500m"
    memory: "512Mi"

# Пустой список
volumes: []

# Пустой объект
labels: {}

# Массив внутри объекта
service:
  ports:
    - 80
    - 443

# Объект внутри массива
containers:
  - name: nginx
    image: nginx:latest

    env:
      - name: ENV
        value: production

      - name: DEBUG
        value: "false"
```

Файлик с самым важным для CI / CD

``` yaml
# Название Pipeline
name: CI-CD Pipeline


# Event / Trigger
on:
  push:
    branches:
      - main

  pull_request:

  workflow_dispatch:


# Переменные
variables:
  APP_ENV: production
  VERSION: "1.0"


# Jobs
jobs:

  # Сборка приложения
  build:

    # Где выполняется Job
    runs-on: ubuntu-latest


    # Последовательность действий
    steps:

      # Получить код
      - name: Checkout code
        uses: checkout-action


      # Выполнить команду
      - name: Install dependencies
        run: |
          npm install
          npm build


      # Переменные внутри Step
      - name: Build
        env:
          NODE_ENV: production
        run: npm run build



    # Артефакты
    artifacts:
      paths:
        - build/


    # Кэш
    cache:
      key: dependencies
      paths:
        - node_modules/


  # Тестирование
  test:

    needs:
      - build

    runs-on: ubuntu-latest

    steps:

      - name: Run tests
        run: npm test



  # Деплой
  deploy:

    needs:
      - test


    environment:
      name: production


    steps:

      - name: Login registry
        run: docker login


      - name: Build Docker image
        run: docker build -t app .


      - name: Push image
        run: docker push app


      - name: Deploy
        run: |
          ssh server
          docker compose up -d



# Secrets
secrets:

  DATABASE_PASSWORD:
    value: hidden

  SSH_KEY:
    value: hidden
```