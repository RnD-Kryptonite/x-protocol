Реализация X-протокола на языке python 3.

# 0. Пререквизиты для запуска/тестирования

Модули pygost, datetime, pickle

# 1. Запуск тестов

Для запуска тестов необходимо в командной строке выполнить следующую команду:

`python -m unittest discover`

# 2. Утилиты командной строки 
## 2.1 Команды Сервиса
### 2.1.1 Формирование Request

Для данной операции необходимо в командной строке выполнить:

`python -m cmd.src --form_request --uid <user-id:int> --scope <data-scope:str> --due <date>`

Опциональными являются дополнительные аргументы:
1. `--output <path-to-file>` -- путь до файла (с именем файла включительно), куда
сохранить сформированный request, по умолчанию путь = `data/request`.
2. `--service <path-to-service>` -- путь до файла с ключами и иной информации о
   сервисе, по умолчанию путь = `data/src`, можно изменить в файле default.py
   или передать в явном виде.
3. `--auth <path-to-auth>` -- путь до файла с базой данных центра аутентификации
   (задает отображение scope -> inspector и т.д.)

Например:

`python -m cmd.src --form_request --uid 123 --scope "паспортные данные" --due 2099-01-01`

Дата пишется в формате YYYY-MM-DD.

### 2.1.2 Проверка подписи блоба (пришедшего от пользователя)

Для данной операции необходимо в командной строке выполнить:

`python -m cmd.src --check_blob --blob <path-to-blob>`

Опциональными являются дополнительные аргументы:

1. `--service <path-to-service>` -- путь до файла с ключами и иной информации о
   сервисе, по умолчанию путь = `data/src`, можно изменить в файле default.py
   или передать в явном виде.
2. `--auth <path-to-auth>` -- путь до файла с базой данных центра аутентификации
   (задает отображение scope -> inspector и т.д.)


### 2.1.3 Проверка ответа инспектора 

Для данной операции необходимо в командной строке выполнить:

`python -m cmd.src --check_response --response <path-to-response>`

Опциональными являются дополнительные аргументы:

1. `--service <path-to-service>` -- путь до файла с ключами и иной информации о
   сервисе, по умолчанию путь = `data/src`, можно изменить в файле default.py
   или передать в явном виде.
2. `--auth <path-to-auth>` -- путь до файла с базой данных центра аутентификации
   (задает отображение scope -> inspector и т.д.)

Команда проверяет подпись инспектора, а также ответ-подтверждение (являются ли
предоставленные в блобе зашифрованные персональные данные корректными).

## 2.2 Команды пользователя

### 2.2.1 Формирование Blob 

Для данной операции необходимо в командной строке выполнить:

`python -m cmd.usr --form_blob --request <path-to-request>`

Опциональными являются дополнительные аргументы:
1. `--output <path-to-file>` -- путь до файла (с именем файла включительно), куда
сохранить сформированный blob, по умолчанию путь = `data/blob`.
2. `--secdata <personal data>` -- персональные данные пользователя,
   соответствующие запрошенному request. Если персональные данные не переданы
   при вызове скрипта, то они будут запрошены (вместе с выводом информации об ID
   Сервиса, типе запрашиваемых персональных данных и датой, до которой требуются
   персональные данные.
2. `--user <path-to-user>` -- путь до файла с ключами и иной информации о
   пользователе, по умолчанию путь = `data/usr`, можно изменить в файле default.py
   или передать в явном виде.
3. `--auth <path-to-auth>` -- путь до файла с базой данных центра аутентификации
   (задает отображение scope -> inspector и т.д.)

Например:

`python -m cmd.usr --form_blob --request data/request --secdata "Иванов Иван Иванович"` 

### 2.2.2 Проверка Request (подпись)

Для данной операции необходимо в командной строке выполнить:

`python -m cmd.usr --check_request --request <path-to-request>`

Опциональными являются дополнительные аргументы:

1. `--user <path-to-user>` -- путь до файла с ключами и иной информации о
   пользователе, по умолчанию путь = `data/src`, можно изменить в файле default.py
   или передать в явном виде.
2. `--auth <path-to-auth>` -- путь до файла с базой данных центра аутентификации
   (задает отображение scope -> inspector и т.д.)

## 2.3 Команды Инспектора

### 2.3.1 Верификация Blob 

Для данной операции необходимо в командной строке выполнить:

`python -m cmd.insp --verify_blob --request <path-to-request> --blob <path-to-blob>`

Опциональными являются дополнительные аргументы:
1. `--output <path-to-file>` -- путь до файла (с именем файла включительно), куда
сохранить сформированный response, по умолчанию путь = `data/response`.
2. `--inspector <path-to-inspector>` -- путь до файла с ключами и иной информации о
   инспекторе, по умолчанию путь = `data/insp`, можно изменить в файле default.py
   или передать в явном виде.
3. `--auth <path-to-auth>` -- путь до файла с базой данных центра аутентификации
   (задает отображение scope -> inspector и т.д.)

Например:

`python -m cmd.insp --verify_blob --blob data/blob --request data/request` 

### 2.3.2 Добавление персональных данных пользователя 

Для данной операции необходимо в командной строке выполнить:

`python -m cmd.insp --add_user --uid <user-id:int> --secdata <data : str>`

Опциональными являются дополнительные аргументы:

1. `--inspector <path-to-inspector>` -- путь до файла с ключами и иной информации о
   инспекторе, по умолчанию путь = `data/insp`, можно изменить в файле default.py
   или передать в явном виде.
2. `--auth <path-to-auth>` -- путь до файла с базой данных центра аутентификации
   (задает отображение scope -> inspector и т.д.)
