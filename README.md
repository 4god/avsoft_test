# avsoft_test
Тестовое задание для AVSoft

## Основные модули
### Sender
Ожидает появление файлов в папке и обрабатывает их: при нахождении текстового файла - передает json сообщение посредством RabbitMQ в очередь: «Parsing», в сообщении указывает путь до найденного файла; для других типов файлов - передает аналогичное сообщение в очередь: «Errors».

### Parser
Обрабатывает сообщения очереди: «Parsing»: извлекает слова из текста и записывает их в таблицу MySQL с указанием количества вхождений в тексте (для каждого слова, соответственно). В качестве разделителя слов используются все не буквенные символы. Если слово уже есть в таблице, то увеличивает количество вхождений на соответствующее число.

### Reader
Обрабатывает данные таблицы в MySQL: когда слово встретилось N раз и более, удаляет запись и создает файл, содержащий 2 строки: само слово и имена файлов, содержащих данное слово. N - константа с произвольным значением.

### Error handler
Оповещает посредством электронной почты/Telegram/SIEM/SOAR, в случае получения сообщения из очереди: «Errors». В случае ошибки отправки оповещения, оповещение должно произойти ПОЗДНЕЕ.

### Generator
Ищет страницы сайта (аналогично поисковому роботу и составлению sitemap.xml) и записывает содержимое каждой страницы в отдельный файл, в папку Отправителя. Адрес сайта вводит пользователь.

## Вспомогательные модули

### Database
Содержит класс-примесь для взаимодействия с MySQL и основные запросы в базу данных

### RabbitMQ
Содержит классы издателя и подписчика для взаимодействия с RabbitMQ

### Settings
Настройки модулей