`streamstats` — программа на Python для потокового анализа журналов событий в форматах CSV и JSONL. Программа читает входные файлы построчно, проверяет корректность записей, подсчитывает статистику и сохраняет результат в JSON-файл.


Сборка образа:

```bash
docker build -t streamstats .
```

Запуск приложения :

```bash
docker run --rm streamstats --help
```


Анализ CSV-файла с сохранением отчёта в текущую папку:

```bash
docker run --rm \
  -v "$PWD:/app" \
  streamstats \
  analyze /app/tests/fixtures/valid.csv \
  --format csv \
  --output /app/report-docker.json
```


Анализ JSONL-файла:

```bash
docker run --rm \
  -v "$PWD:/app" \
  -e PYTHONPATH=/app/src \
  streamstats \
  analyze /app/tests/fixtures/valid.csv \
  --format csv \
  --output /app/report-dev.json
```

- `-v "$PWD:/app"` подключает проект с компьютера;
- `-e PYTHONPATH=/app/src` указывает Python путь к исходному коду;
- изменения файлов в `src/streamstats/` будут использоваться при следующем запуске контейнера;
- пересобирать образ после изменения Python-кода не потребуется.

При изменении файлов исходного кода в папке `src/streamstats/` изменения становятся доступны внутри контейнера благодаря bind mount.

При следующем запуске контейнера Python использует обновлённый исходный код. Повторная сборка Docker-образа не требуется.

Таким образом, bind mount позволяет разрабатывать и проверять приложение непосредственно из локальной папки проекта.


Загрузка образа с Docker Hub:

```bash
docker pull yulia0642/streamstats:1.0
```

Запуск приложения:

```bash
docker run --rm yulia0642/streamstats:1.0 --help
```

Анализ файлов с использованием bind mount:

```bash
docker run --rm \
  -v "$PWD:/app" \
  yulia0642/streamstats:1.0 \
  analyze /app/tests/fixtures/valid.csv \
  --format csv \
  --output /app/report-docker.json
```

## 8. Docker Hub

Опубликованный образ доступен по адресу:

https://hub.docker.com/r/yulia0642/streamstats

Тег образа: `1.0`