---
created-dt: 2026-08-07 12:15
tags:
  - review
sr-due: 2026-10-12
sr-interval: 24
sr-ease: 230
---
[[Workflow]], который можно вызывать из другого Workflow
и использовать повторно. Позволяет вынести общую CI/CD-логику в отдельный Workflow.

Cделать Workflow переиспользуемым
```yaml
on:
  workflow_call:
```

В вызывающем Workflow:
``` bash
jobs:
  call:
    uses: ./.github/workflows/reusable.yml
```
Для Workflow из другого репозитория:
``` yml
jobs:
  call:
    uses: owner/repository/.github/workflows/reusable.yml@main
```

**`Передача`** [[Inputs]]
В reusable Workflow:
``` yml
on:
  workflow_call:
    inputs:
      title:
        type: string
        required: false
        default: "Default title"
```
При вызове:
``` yml
jobs:
  call:
    uses: ./.github/workflows/reusable.yml
    with:
      title: ${{ inputs.title }}
```
Внутри reusable Workflow:
``` yml
${{ inputs.title }}
```

**`Передача`** [[Secrets]]
В reusable Workflow:
``` yml
on:
  workflow_call:
    secrets:
      PASSWORD:
        required: true
```
При вызове:
``` yml
jobs:
  call:
    uses: owner/repository/.github/workflows/reusable.yml@main
    secrets:
      PASSWORD: ${{ secrets.PASSWORD }}
```
Внутри:
``` yml
${{ secrets.PASSWORD }}
```

**`Outputs`**
Reusable Workflow может вернуть результат вызывающему Workflow.
1. Step создаёт output
``` yml
steps:
  - id: generate
    run: echo "date=$(date)" >> $GITHUB_OUTPUT
```

2. Job передаёт output
``` yml
jobs:
  generate:
    outputs:
      date: ${{ steps.generate.outputs.date }}
```

3. Workflow объявляет output
``` yml
on:
  workflow_call:
    outputs:
      date:
        value: ${{ jobs.generate.outputs.date }}
```

4. Вызывающий Workflow получает output
``` yml
${{ needs.call.outputs.date }}
```

[[cicd/GitHub Actions/Job]] должен иметь:
``` yml
needs: call
```