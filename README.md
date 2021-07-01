# shrikh-fr-dbus-service
dbus service for cash register (ShtrikhFR) 

Драйвер для Linux ККМ аппаратов серии Штрих-М, Штрих-ФР, и другие поддерживающие протокол Штрих.

Сделан в виде сервиса DBus.
Преимущество - работает в любых тулкитах биндинг на DBus (bash, javascript, c++, perl, python и.т.д), можно расшарить через сеть.

В драйвере реализован протокол Штрих 1.12 (+добавлена несколько новых команд из нового протокола 2.0.24, открыть смету и.т.д.).
Добавлены консольная утилита чтения/записи таблиц и примеры на Qt.

Установка:
- склонировать проект shtrikh-fr-dbus-service например в /opt, воспользуйтесь git clone
- установить в систему дополнительные модули для Perl - Device::SerialPort, Time::HiRes, Math::BigInt, Unix::Syslog, Net::DBus

- в файле ru.shtrih_m.fr.service исправить путь до исполняемого файла shtrih_fr.service, поправить права на запуск, также User=XXXX должен быть сервисным пользователем системы, который имеет доступ чтения и записи в порты ttyS*,ttyUSB* (обычно это группа dialout), для теста можете воспользоваться правами root

- файл ru.shtrih_m.fr.service скопировать в сервисную директорию, обычно это /usr/share/dbus-1/system-services
- файл ru.shtrih_m.fr.conf скопировать в директорию /etc/dbus-1/system.d
- в зависисмости от версии DBus он либо сам перечитает все свои конфиги либо надо будет перезапустить службу DBus

- запустить команду на получение статуса ККМ examples/get_status.sh, сервис стартанет автоматически и выдаст в консоль результат

Правило udev для usb подключения на фиксированный порт:  
```
KERNEL=="ttyACM*", ACTION=="add", ENV{ID_BUS}=="usb", ENV{ID_VENDOR_ID}=="1fc9", ENV{ID_MODEL_ID}=="0083", SYMLINK+="ttyShtrikh"
```

Пример работы консольной утилиты для работы с таблицами:  
```
$ ./examples/tables.pl
|  1 | ТИП И РЕЖИМ КАССЫ
|  2 | ПАРОЛИ КАССИРОВ И АДМИНИСТРАТОРОВ
|  3 | ТАБЛИЦА ПЕРЕВОДА ВРЕМЕНИ
|  4 | ТЕКСТ В ЧЕКЕ
|  5 | НАИМЕНОВАНИЕ ТИПОВ ОПЛАТЫ
|  6 | НАЛОГОВЫЕ СТАВКИ
|  7 | НАИМЕНОВАНИЕ ОТДЕЛОВ
|  8 | НАСТРОЙКА ШРИФТОВ
|  9 | ТАБЛИЦА ФОРМАТА ЧЕКА
| 10 | СЛУЖЕБНАЯ
| 11 | ПАРАМЕТРЫ КОДИРОВАНИЯ QR-КОДОВ
| 12 | ВЕБ-ССЫЛКА
| 13 | ПАРАМЕТРЫ ТЕРМОПЕЧАТИ
| 14 | SDCARD STATUS
| 15 | СЕРВЕР ТРАНЗАКЦИЙ
| 16 | CЕТЕВОЙ АДРЕС
| 17 | РЕГИОНАЛЬНЫЕ НАСТРОЙКИ
| 18 | FISCAL STORAGE
| 19 | ПАРАМЕТРЫ ОФД
| 20 | СТАТУС ОБМЕНА ФН
| 21 | СЕТЕВЫЕ ИНТЕРФЕЙСЫ
| 22 | CЕТЕВОЙ АДРЕС WIFI (УСТАРЕЛА)
| 23 | УДАЛЕННЫЙ МОНИТОРИНГ И АДМИНИСТРИРОВАНИЕ
| 24 | ВСТРАИВАЕМАЯ И ИНТЕРНЕТ ТЕХНИКА
| 25 | ФИСКАЛИЗАЦИЯ
prompt> help
| exit
| help
| show tables
| describe table <tid>
| select <all|fid> from table <tid>
| update table <tid> set field <fid> = value
| update table <tid> set field <fid> = value where col = <cid>
prompt>
```
