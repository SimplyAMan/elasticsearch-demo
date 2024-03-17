# docker-compose.yml

- db - БД MySQL
- odfe-node - нода ElasticSearch
- kibana - Kibana - для перегляду вмісту існуючих індексів ElasticSearch 
- logstash - для переносу вмісту таблиць БД MySQL в індекс ElasticSearch

Як виконується перенос даних:
logstash має файл налаштувань logstash.conf, який в нашому випадку складається з налаштувань двох плагінів: input i output.

**Input** використовується для налаштувань джерела даних. 
В нашому випадку ми підключаємось до реляційної БД MySQL, тому використовуємо JDBC інтерфейс.

Де крім параметрів підключення до БД (драйвера, рядка підключення, імені і пароля користувача)
також вказуємо і SQL запит за допомогою якого logstash буде отримувати дані з БД, а також
зміни якого поля він має аналізувати (tracking_column).

Завантаження даних відбувається періодично, для задання періодичності використовується 
параметр schedule в форматі UNIX cron

**Output** використовується для налаштування збереження даних. В нашому випадку ми використовуємо ElasticSearch.

В параметрах вказуємо в який індекс мають зберігатись дані. Щоб не створювати дублікати даних
використовуємо параметри document_id (ідентифікатор документа в ElasticSearch), а параметри 
doc_as_upsert і action відповідають за те, що робити коли документ з таким кодом вже є в БД.