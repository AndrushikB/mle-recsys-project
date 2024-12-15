# Инструкция по запуску микросервиса

Имя персонального бакета: s3-student-mle-20240827-f58c68b5ea

## Склонируйте репозиторий

Склонируйте репозиторий проекта:

```
git clone https://github.com/AndrushikB/mle-recsys-project.git
```

## Активируйте виртуальное окружение

Используйте то же самое виртуальное окружение, что и созданное для работы с уроками. Если его не существует, то его следует создать.

Создать новое виртуальное окружение можно командой:

```
python3.10 -m venv .venv_recsys-project
```

После его инициализации следующей командой

```
source .venv_recsys-project/bin/activate
```

установите в него необходимые Python-пакеты следующей командой

```
pip install -r requirements.txt
```

### Скачайте файлы с данными

Для работы и тестирования сервиса понадобится четыре файла с данными:
- [recommendations.parquet](https://storage.yandexcloud.net/s3-student-mle-20240827-f58c68b5ea/recsys/recommendations/recommendations.parquet)
- [top_popular.parquet](https://storage.yandexcloud.net/s3-student-mle-20240827-f58c68b5ea/recsys/recommendations/top_popular.parquet)
- [similar.parquet](https://storage.yandexcloud.net/s3-student-mle-20240827-f58c68b5ea/recsys/recommendations/similar.parquet)
- [items.parquet](https://storage.yandexcloud.net/s3-student-mle-20240827-f58c68b5ea/recsys/data/items.parquet)

Скачайте их в директорию локального репозитория. Для удобства вы можете воспользоваться командой wget:

```
wget https://storage.yandexcloud.net/s3-student-mle-20240827-f58c68b5ea/recsys/recommendations/recommendations.parquet

wget https://storage.yandexcloud.net/s3-student-mle-20240827-f58c68b5ea/recsys/recommendations/top_popular.parquet

wget https://storage.yandexcloud.net/s3-student-mle-20240827-f58c68b5ea/recsys/recommendations/similar.parquet

wget https://storage.yandexcloud.net/s3-student-mle-20240827-f58c68b5ea/recsys/data/items.parquet
```

# Расчёт рекомендаций

Код первой части проекта находится в файле `recommendations.ipynb`.

# Сервис рекомендаций

Код сервиса рекомендаций находится по пути `rec_service/recommendations_service.py`.

Перейдите в директорию 'rec_service'

``` cd rec_service ```

Для работы сервиса рекомендаций необходимы Feature store и Event store (запуск производить из двух разных терминалов, на двух разных портах)

```
uvicorn features_service:app --port 8010
uvicorn events_service:app --port 8020
```

Код для запуска сервиса рекомендаций в новом терминале:

```
uvicorn recommendation_service:app
```

# Инструкции для тестирования сервиса

Код для тестирования сервиса находится по пути `rec_service/test_service.py`.

Находясь в директории 'rec_service' запустите тест:

```
python3 test_service.py
```

Повторный запуск теста изменит рекомендации (в ходе теста к одному и тому же пользователю добавляется онлайн история)
