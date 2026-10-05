---
created-dt: 2026-08-06 11:47
tags:
  - review
sr-due: 2026-12-19
sr-interval: 75
sr-ease: 230
---
Cпособ запустить один [[cicd/GitHub Actions/Job]] несколько раз
с разными комбинациями заданных значений.
- версиях языка;
- ОС;
- конфигурациях;
- других параметрах.
```yaml
name: Test matrix
on: push

jobs:
  test:
    strategy:
      matrix:
        os: [windows-latest, ubuntu-latest]
        node-version: [14, 16, 18] 
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/checkout@v7
	  - name: Use Node.js ${{ matrix.node-version }}
		uses: actions/setup-node@v7
		with:
		  node-version: ${{ matrix.node-version }}
	  - run: node -v
```

`include` - добавляет дополнительные комбинации  
или значения к Matrix.
``` bash
include:
  - os: ubuntu-latest
    node: 24
```
Также `include` может добавлять дополнительные переменные:
``` bash
include:
  - os: ubuntu-latest
    node: 22
    environment: production
```
`exclude` - исключает определённые комбинации из Matrix.
``` bash
exclude:
  - os: windows-latest
    node: 18
```
