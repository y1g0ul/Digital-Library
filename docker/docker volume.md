---
created-dt: 2026-01-20 08:01
tags:
  - review
sr-due: 2026-02-24
sr-interval: 22
sr-ease: 250
---
Это выделенное хранилище данных в [[docker]], которое существует отдельно от [[container]].


| способ работы с данными | пример                                                                                   |
| ----------------------- | ---------------------------------------------------------------------------------------- |
| Проброс папки хоста     | `docker run -v /host/path:/container/path image`                                         |
| Именованный том         | `docker run -v mydata:/container/path image`                                             |
| Анонимный том           | `docker run -v /container/path image` или прям в [[docker]]file `VOLUME /container/path` |


|Команда|Что делает|
|---|---|
|`docker volume create myvol`|Создать новый том|
|`docker volume ls`|Показать все тома|
|`docker volume inspect myvol`|Показать детали тома|
|`docker volume rm myvol`|Удалить том|
|`docker run -v myvol:/data …`|Использовать том в контейнере|
