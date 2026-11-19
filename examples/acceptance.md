# Проверка по критериям задания

Выполните `start.sh`, затем `demo.sh` и `verify.sh` из README. Следующая таблица помогает найти реализацию и фактический результат.

| Критерий | Реализация | Проверка |
|---|---|---|
| SHOP читает JSON-файл | `services/cli.py`, `data/products.jsonl` | `demo.log`: delivery callbacks с partition/offset |
| CLIENT: поиск и рекомендации | `services/cli.py`, PostgreSQL | `demo.log`: поиск часов и разные рекомендации Alice/Bob |
| Запросы в БД и Kafka | `client_requests`, `outbox`, `services/outbox.py` | пустой pending outbox; события в зеркале/HDFS |
| TLS и ACL | `scripts/configure.py`, `services/bootstrap.py` | `services/acl_check.py`: магазину запрещены чужие топики и требуется сертификат |
| RF=3, min ISR=2 | `compose.yaml`, bootstrap | `topology.json`, `*-topics.txt` |
| Второй кластер | MirrorMaker2 | `mirrored-products.json` |
| Потоковая фильтрация | Kafka Streams, KTable join | p006 отсутствует; p001 удаляется и возвращается при смене запрета |
| Хранение разрешённых товаров | PostgreSQL и Kafka Connect FileStreamSink | пять записей каталога, `/debug/products.jsonl` |
| Data Lake | `services/hdfs_sink.py` | `/marketplace/products`, `/marketplace/events` в HDFS |
| Аналитика Spark | `analytics/job.py` | успешный Spark job, файл рекомендаций в HDFS |
| Топик рекомендаций | secondary: `recommendations` | проверка Kafka и `recommendations.json` |
| Метрики JMX | Java agent на каждом брокере | шесть targets UP, Kafka-specific метрики |
| Grafana | provisioning и dashboard JSON | шесть панелей `Marketplace Kafka` |
| Оповещение при отказе | Prometheus rule → Alertmanager → webhook | `alerts.jsonl`: firing и resolved для primary-3 |
| Документация и примеры | README, examples, data | запуск с чистого clone без ручного создания топиков/сертификатов |

Файлы с результатами появляются только после реального выполнения. Наличие кода или этой таблицы не заменяет успешный интеграционный прогон.
