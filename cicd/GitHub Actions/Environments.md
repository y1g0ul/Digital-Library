---
created-dt: 2026-08-04 14:58
tags:
  - review
sr-due: 2026-12-09
sr-interval: 65
sr-ease: 230
---
Отдельное окружение [[Workflow]]. У GitHub фактически есть несколько уровней хранения:
``` 
Repository
├── Variables
├── Secrets
│
└── Environments
    ├── Variables
    └── Secrets
```

Они позволяют разделять настройки **по окружениям**, например:
- development
- staging
- production
У каждого Environment могут быть свои Secrets и [[Variable]]s. `vars`- это GitHub Actions context для [[Variable]]s, созданных в настройках GitHub.

```
${{ vars.APP_NAME }}
```

Если Variable `APP_NAME` создан на уровне Repository, он доступен workflow из этого репозитория.

Если Variable создан внутри Environment, его значение становится доступно [[cicd/GitHub Actions/Job]], которая использует этот Environment.
``` yml
jobs:
  deploy:
    environment: production
    runs-on: ubuntu-latest

    steps:
      - run: echo "${{ vars.APP_URL }}"
```
[[cicd/GitHub Actions/Job]] получает настройки именно из `production`.

