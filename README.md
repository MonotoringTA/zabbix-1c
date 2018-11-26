## Мониторинг соединений к базам 1С Предприятия средствами Zabbix

Для обнаружения баз используется LLD Zabbix, используемые версии Zabbix 3.4 - 4.0, ниже не получится, используются зависимые элементы данных.

Описание подготовлено для Windows.

Для получения информации используются консольные утилиты RAS (сервер) и RAC (клиент).
Для установки сервера в качестве службы используется команда:  
`sc create "1C:Enterprise RAS" binpath= "C:\Program Files\1cv8\8.Х.Х.ХХХХ\bin\ras.exe cluster --service" displayname= "1C:Enterprise RAS" start= auto` 

Для запуска - команда:  
`net start "1C:Enterprise RAS"`

В системную переменную PATH необходимо добавить путь где находится исполняемый файл rac.exe: `c:\Program Files\1cv8\8.X.XX.XXXX\bin\`  
![set path](/set_path.JPG?raw=true)
Правой кнопкой на Этот компьюьтер - Свойства - Дополнительные параметры системы (справа) - Переменные среды - раздел Системные переменные - выбрать переменную Path, изменить, откроется окно Изменение системной перемнной - в поле значение перемнной добавить в конец путь вида `c:\Program Files\1cv8\8.X.XX.XXXX\bin\`, т.е. путь где лежит используемая платформа.
При использовании 32-х разрядной платформы путь будет вида: `c:\Program Files (x86)\1cv8\8.X.XX.XXXX\bin\`

Необходимо установить Python. http://python.org, проверьте что бы путь установки соответствовал пути прописанному в файле 1s_zabbix.conf. Ожидается что интерпретатор будет находится `c:\Program Files\Python37\python.exe` 

На сервер, с установленным RAC и Zabbix agent необходимо положить файлы из каталога scripts и файл конфигурации 1s_zabbix.conf.

`ib_list.py` - скрипт возвращающий список баз  
`sess_list.py` - скрипт возвращает кол-во сессий на присланный uuid базы

Настройки каталогов:
- zabbix agent - c:\zabbix
- python - c:\Program Files\Python37\python.exe

Шаблон для Zabbix 4.0: `1c-zabbix.xml`  
Шаблон для Zabbix 3.4: `1c_ent_template_34.xml`

Снимаются данные о количестве соединений в разбивке по видам: тонкий клиент, толстый клиент и т.д., число заблокированных объектов, время блокировки и объем данных.  
Для каждой базы создается отдельный график.

При обнаружении ошибок, возникающих вопросах, необходимости добавления новых данных в мониторинг - создавайте issues.
