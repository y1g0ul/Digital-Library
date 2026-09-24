---
created-dt: 2026-08-03 01:48
tags:
  - review
sr-due: 2026-12-10
sr-interval: 77
sr-ease: 249
---
``` yaml
on:
  workflow_dispatch:
    inputs:

      # Строка
      name:
        description: "Имя"
        required: true
        default: "Nikita"
        type: string

      # Выбор из вариантов
      environment:
        description: "Окружение"
        required: true
        type: choice
        options:
          - dev
          - staging
          - production

      # Флажок
      deploy:
        description: "Выполнить deploy?"
        required: false
        default: false
        type: boolean
```

Получить значение input:
``` yml
${{ inputs.name }}
```

Так же [[Workflow]] с [[Event]] workflow_dispatch (запуск руками через gui или api) и repository_dispatch (запуска только через api)

``` yml
name: Seventh Workflow

on:
  repository_dispatch:
	types: [test] # Можем сами задать ему название что-бы позжа обратиться к нему в api, а можем и не делать

jobs:
  display-text:
	runs-on: ubuntu-latest
	steps:
	  - name: Display some text
	    run: echo "${{ github.event.client_payload.text }}"
```