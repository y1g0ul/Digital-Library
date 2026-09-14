---
created-dt: 2026-08-05 12:43
tags:
  - review
sr-due: 2026-11-16
sr-interval: 63
sr-ease: 250
---
Файл или набор файлов, созданных во время выполнения [[cicd/GitHub Actions/Job]],
которые можно сохранить и использовать после завершения [[cicd/GitHub Actions/Job]].
- сохранить результат сборки;
- передать файлы между [[cicd/GitHub Actions/Job]]s;
- сохранить логи и отчёты;
- скачать результат [[Workflow]] после его завершения.

## Пример

```yaml
# Создать Artifact
- name: Upload artifact
  uses: actions/upload-artifact@v7
  with:
    name: build
    path: ./build

# Скачать Artifact  
- name: Download artifact
  uses: actions/download-artifact@v4
  with:
    name: build
```

Artifact хранится отдельно от файловой системы Runner и доступен после завершения [[cicd/GitHub Actions/Job]].