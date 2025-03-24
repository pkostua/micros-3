# Решение домашнего задания к занятию «Микросервисы: подходы»
https://github.com/netology-code/micros-homeworks/blob/main/11-microservices-03-approaches.md

## Задача 1: Обеспечить разработку
На сегодняшний день мне удалось найти единственное облачное решение в РФ с доступной документацией это https://gitverse.ru/

Судя по документации решение удовлет всем требованиям:

облачная система - есть поддержка   
система контроля версий Git - это оно и есть  
репозиторий на каждый сервис - работает из коробки  
запуск сборки по событию из системы контроля версий - поддерживается  
запуск сборки по кнопке с указанием параметров - пишут что как в github  
возможность привязать настройки к каждой сборке - пишут что как в github  
возможность создания шаблонов для различных конфигураций сборок - пишут что как в github  
возможность безопасного хранения секретных данных (пароли, ключи доступа) - есть хранилище секретов  
несколько конфигураций для сборки из одного репозитория - пишут что как в github  
кастомные шаги при сборке - описываются в манифесте сборки  
собственные докер-образы для сборки проектов - есть хранилище артефактов  
возможность развернуть агентов сборки на собственных серверах - поддерживаются ранеры, облачные и собственные  
возможность параллельного запуска нескольких сборок - пишут что как в github, там был механизм  matrix  
возможность параллельного запуска тестов - также как в github  

Не подошли сделующие решения:   
https://gitflic.ru - не удалось найти документацию  
self hosted gitlab  - не удовлетворяет  первому пункту требований

## Задача 2: Логи

Для обработки логов  существуют три стека  
1. Elasticsearch + Logstash +  Kibana  
2. Opensearch + Opensearch Output Plugin + Opensearch Dashboard
3. Grafana Loki + Vector

Так или иначе все  три удовлетворяют требованиям, но я бы останиовился на Elasticsearch + Logstash +  Kibana как более привычном  

1. Для сбора слоготв с сервисов применяем Fluent Operator (в кластере k8s) или Filebeat для сбора логов с ВМ  
2. Центральное хранилище логов  
   Elasticsearch: поисковая система, которая будет служить центральным хранилищем для логов  
   Logstash : Если требуется предварительная обработка логов   
3. Гарантированная доставка логов  
   Kafka: Для обеспечения гарантированной доставки логов можно использовать Apache Kafka как промежуточное хранилище. Fluent или Filebeat могут отправлять логи в Kafka, а затем Logstash или другой компонент может считывать из Kafka и отправлять в Elasticsearch  
4. Поиск и фильтрация  
   Kibana: Это UI для Elasticsearch  
5. Пользовательский интерфейс и доступ  
   Kibana также предоставляет пользовательский интерфейс, который используется для поиска по записям логов.  

## Задача 3: Мониторинг

На сегодняшний день самым доступным решением для мониторинга является Prometheus + Grafana  
- Метрики будут собираться экспортерами. Существет множество готовых, например node-exporter для мониторинга хоста или pg-exporter для мониторинга БД. Также можно встроить экспортер в свой сервис.  
- Prometheus будет собирать метрики из всех доступных мест в соответсвии со своей конфигурацией.  
- За отображение метрик будет отвечать Grafana.  
