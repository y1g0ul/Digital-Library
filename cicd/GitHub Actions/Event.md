---
created-dt: 2026-07-28 11:52
tags:
  - review
sr-due: 2026-11-24
sr-interval: 70
sr-ease: 241
---
Cобытие, которое запускает выполнение [[cicd/GitHub Actions/Pipeline|Pipeline]] ([[Workflow]]).

Примеры Event
- `push` - отправка изменений в репозиторий.
- `pull_request` - создание или обновление [[pull]] Request.
- `schedule` - запуск по расписанию ([[cron]]).
- `workflow_dispatch` - ручной запуск.
- `release` - создание релиза.
- `tag` - создание тега.

Разработчик выполняет:
git [[push]] origin main
↓
Срабатывает Event `push`
↓
Запускается [[Workflow]]
↓
Выполняется [[cicd/GitHub Actions/Pipeline]]

| Event                         | На что реагирует                                                                   |
| ----------------------------- | ---------------------------------------------------------------------------------- |
| `branch_protection_rule`      | Создание, изменение или удаление правила защиты ветки                              |
| `check_run`                   | Создание, изменение или завершение отдельного Check Run                            |
| `check_suite`                 | Создание или изменение Check Suite                                                 |
| `create`                      | Создание ветки или тега                                                            |
| `delete`                      | Удаление ветки или тега                                                            |
| `deployment`                  | Создание Deployment                                                                |
| `deployment_status`           | Изменение статуса Deployment                                                       |
| `discussion`                  | Создание, изменение, закрытие, открытие и другие действия с Discussion             |
| `discussion_comment`          | Создание, изменение или удаление комментария в Discussion                          |
| `fork`                        | Создание fork репозитория                                                          |
| `gollum`                      | Создание или изменение Wiki-страницы                                               |
| `image_version`               | Создание или изменение версии container image                                      |
| `issue_comment`               | Создание, изменение или удаление комментария в Issue или Pull Request              |
| `issues`                      | Создание, изменение, закрытие, открытие и другие действия с Issue                  |
| `label`                       | Создание, изменение или удаление Label                                             |
| `merge_group`                 | Добавление Pull Request в Merge Queue                                              |
| `milestone`                   | Создание, изменение, закрытие или удаление Milestone                               |
| `page_build`                  | Событие сборки GitHub Pages                                                        |
| `public`                      | Изменение репозитория с private на public                                          |
| `pull_request`                | Действия с Pull Request: открытие, изменение, закрытие, merge и др.                |
| `pull_request_review`         | Отправка, редактирование или удаление Review                                       |
| `pull_request_review_comment` | Создание, изменение или удаление комментария к конкретной строке кода в Review     |
| `pull_request_target`         | Те же действия, что `pull_request`, но Workflow запускается в контексте base-ветки |
| `push`                        | Отправка коммитов в ветку или тег                                                  |
| `registry_package`            | Публикация или изменение Package в GitHub Packages                                 |
| `release`                     | Создание, публикация, изменение или удаление Release                               |
| `repository_dispatch`         | Получение кастомного события от внешнего источника через API                       |
| `schedule`                    | Наступление времени по расписанию [[cron]]                                         |
| `status`                      | Изменение commit status                                                            |
| `watch`                       | Пользователь поставил ⭐ репозиторию                                                |
| `workflow_call`               | Вызов Workflow из другого Workflow                                                 |
| `workflow_dispatch`           | Ручной запуск Workflow                                                             |
| `workflow_run`                | Завершение или изменение статуса другого Workflow                                  |
