---
## Front matter
title: "Отчёта по лабораторной работе №6"
subtitle: "Основы информационной безопасности"
author: "Павлюченков Сергей Витальевич"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
  - spelling=modern
  - babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Развить навыки администрирования ОС Linux. Получить первое практическое знакомство с технологией SELinux1
.
Проверить работу SELinx на практике совместно с веб-сервером
Apache.

# Задание

1. Войдите в систему с полученными учётными данными и убедитесь, что
SELinux работает в режиме enforcing политики targeted с помощью команд getenforce и sestatus.
2. Обратитесь с помощью браузера к веб-серверу, запущенному на вашем
компьютере, и убедитесь, что последний работает:
service httpd status
или
/etc/rc.d/init.d/httpd status
Если не работает, запустите его так же, но с параметром start.
3. Найдите веб-сервер Apache в списке процессов, определите его контекст
безопасности и занесите эту информацию в отчёт. Например, можно использовать команду
ps auxZ | grep httpd
или
ps -eZ | grep httpd
4. Посмотрите текущее состояние переключателей SELinux для Apache с
помощью команды
sestatus -bigrep httpd
Обратите внимание, что многие из них находятся в положении «off»
5. Посмотрите статистику по политике с помощью команды seinfo, также
определите множество пользователей, ролей, типов.
6. Определите тип файлов и поддиректорий, находящихся в директории
/var/www, с помощью команды
ls -lZ /var/www
7. Определите тип файлов, находящихся в директории /var/www/html:
ls -lZ /var/www/html
8. Определите круг пользователей, которым разрешено создание файлов в
директории /var/www/html.
9. Создайте от имени суперпользователя (так как в дистрибутиве после установки только ему разрешена запись в директорию) html-файл
/var/www/html/test.html следующего содержания:
<html>
<body>test</body>
</html>
10. Проверьте контекст созданного вами файла. Занесите в отчёт контекст,
присваиваемый по умолчанию вновь созданным файлам в директории
/var/www/html.
11. Обратитесь к файлу через веб-сервер, введя в браузере адрес
http://127.0.0.1/test.html. Убедитесь, что файл был успешно отображён.
12. Изучите справку man httpd_selinux и выясните, какие контексты файлов определены для httpd. Сопоставьте их с типом файла
test.html. Проверить контекст файла можно командой ls -Z.
ls -Z /var/www/html/test.html
Рассмотрим полученный контекст детально. Обратите внимание, что так
как по умолчанию пользователи CentOS являются свободными от типа
(unconfined в переводе с англ. означает свободный), созданному нами
файлу test.html был сопоставлен SELinux, пользователь unconfined_u.
Это первая часть контекста.
Далее политика ролевого разделения доступа RBAC используется процессами, но не файлами, поэтому роли не имеют никакого значения для
файлов. Роль object_r используется по умолчанию для файлов на «постоянных» носителях и на сетевых файловых системах. (В директории
/ргос файлы, относящиеся к процессам, могут иметь роль system_r.
Если активна политика MLS, то могут использоваться и другие роли,
например, secadm_r. Данный случай мы рассматривать не будем, как и
предназначение :s0).
Тип httpd_sys_content_t позволяет процессу httpd получить доступ к файлу. Благодаря наличию последнего типа мы получили доступ к файлу
при обращении к нему через браузер.
13. Измените контекст файла /var/www/html/test.html с
httpd_sys_content_t на любой другой, к которому процесс httpd не
должен иметь доступа, например, на samba_share_t:
chcon -t samba_share_t /var/www/html/test.html
ls -Z /var/www/html/test.html
4. Попробуйте ещё раз получить доступ к файлу через веб-сервер, введя в
браузере адрес http://127.0.0.1/test.html. Вы должны получить
сообщение об ошибке:
Forbidden
You don't have permission to access /test.html on this server.
15. Проанализируйте ситуацию. Почему файл не был отображён, если права
доступа позволяют читать этот файл любому пользователю?
ls -l /var/www/html/test.html
Просмотрите log-файлы веб-сервера Apache. Также просмотрите системный лог-файл:
tail /var/log/messages
Если в системе окажутся запущенными процессы setroubleshootd и
audtd, то вы также сможете увидеть ошибки, аналогичные указанным
выше, в файле /var/log/audit/audit.log. Проверьте это утверждение самостоятельно.
16. Попробуйте запустить веб-сервер Apache на прослушивание ТСР-порта
81 (а не 80, как рекомендует IANA и прописано в /etc/services). Для
этого в файле /etc/httpd/httpd.conf найдите строчку Listen 80 и
замените её на Listen 81.
17. Выполните перезапуск веб-сервера Apache. Произошёл сбой? Поясните
почему?
18. Проанализируйте лог-файлы:
tail -nl /var/log/messages
Просмотрите файлы /var/log/http/error_log,
/var/log/http/access_log и /var/log/audit/audit.log и
выясните, в каких файлах появились записи.
19. Выполните команду
semanage port -a -t http_port_t -р tcp 81
После этого проверьте список портов командой
semanage port -l | grep http_port_t
Убедитесь, что порт 81 появился в списке.
20. Попробуйте запустить веб-сервер Apache ещё раз. Поняли ли вы, почему
он сейчас запустился, а в предыдущем случае не смог?
21. Верните контекст httpd_sys_cоntent__t к файлу /var/www/html/ test.html:
chcon -t httpd_sys_content_t /var/www/html/test.html
После этого попробуйте получить доступ к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1:81/test.html.
Вы должны увидеть содержимое файла — слово «test».
22. Исправьте обратно конфигурационный файл apache, вернув Listen 80.
23. Удалите привязку http_port_t к 81 порту:
semanage port -d -t http_port_t -p tcp 81
и проверьте, что порт 81 удалён.
24. Удалите файл /var/www/html/test.html:
rm /var/www/html/test.html

# Выполнение лабораторной работы

Вхожу в систему с полученными учётными данными и убеждаюсь, что
SELinux работает в режиме enforcing политики targeted с помощью команд sestatus.
 (рис. [-@fig:001]).

![sestatus](image/1.png){#fig:001 width=70%}

Обращаюсь с помощью браузера к веб-серверу, запущенному на вашем
компьютере, и проверяю, что последний работает:
service httpd status (рис. [-@fig:002]).

![Проверка httpd](image/2.png){#fig:002 width=70%}

Нахожу веб-сервер Apache в списке процессов, определяю его контекст
безопасности - httpd_t. Например, можно использовать команду
ps auxZ | grep httpd (рис. [-@fig:003]).

![Контекст безопасности httpd](image/3.png){#fig:003 width=70%}

Смотрю текущее состояние переключателей SELinux для Apache с
помощью команды
sestatus -bigrep httpd (рис. [-@fig:004]).

![Состояние apache](image/4.png){#fig:004 width=70%}

Смотрю статистику по политике с помощью команды seinfo, также
определите множество пользователей, ролей, типов. (рис. [-@fig:005]).

![Статистика httpd](image/5.png){#fig:005 width=70%}

Определите тип файлов и поддиректорий, находящихся в директории
/var/www, с помощью команды
ls -lZ /var/www
 (рис. [-@fig:006]).

![Содержимое поддиректории www](image/6.png){#fig:006 width=70%}

Создаю от имени суперпользователя (так как в дистрибутиве после установки только ему разрешена запись в директорию) html-файл
/var/www/html/test.html следующего содержания:
<html>
<body>test</body>
</html> (рис. [-@fig:007]).

![Создание html-файла](image/7.png){#fig:007 width=70%}

Обращаюсь к файлу через веб-сервер, введя в браузере адрес
http://127.0.0.1/test.html.  (рис. [-@fig:008]).

![Обращение к файлу](image/8.png){#fig:008 width=70%}

Убеждаюсь, что файл был успешно отображён.
 (рис. [-@fig:009]).

![Отображение файла](image/9.png){#fig:009 width=70%}

Проверяю контекст файла можно командой ls -Z.
ls -Z /var/www/html/test.html
 (рис. [-@fig:010]).

![Проверка контекста](image/10.png){#fig:010 width=70%}

Изменяю контекст файла /var/www/html/test.html с
httpd_sys_content_t на любой другой, к которому процесс httpd не
должен иметь доступа, например, на samba_share_t:
chcon -t samba_share_t /var/www/html/test.html
ls -Z /var/www/html/test.html (рис. [-@fig:011]).

![Смена контекста файла](image/11.png){#fig:011 width=70%}

Пробую ещё раз получить доступ к файлу через веб-сервер, введя в
браузере адрес http://127.0.0.1/test.html. Получаю
сообщение об ошибке:
 (рис. [-@fig:012]).

![Сообщение об ошибке](image/12.png){#fig:012 width=70%}

Просмотриваю log-файлы веб-сервера Apache. SELinux нпе позволяет получить атрибут для чтения (рис. [-@fig:013]).

![Лог файл](image/13.png){#fig:013 width=70%}

Меняю конфиг и перезагружаю httpd (рис. [-@fig:014]).

![Обновление конфига](image/14.png){#fig:014 width=70%}

В файле /etc/httpd/httpd.conf нахожу строчку Listen 80 и
заменяю её на Listen 81.
 (рис. [-@fig:015]).

![Замена порта](image/15.png){#fig:015 width=70%}

Просмотриваю системный лог-файл:
tail /var/log/messages
 (рис. [-@fig:016]).

![Лог-файл](image/16.png){#fig:016 width=70%}

Выполните команду
semanage port -a -t http_port_t -р tcp 81 (рис. [-@fig:018]).

![Добавляю новый порт](image/18.png){#fig:018 width=70%}

Запускаю успешно файл (рис. [-@fig:019]).

![Запуск файла в браузере](image/19.png){#fig:019 width=70%}

Возвращаю 80 порт, уберая 81 (рис. [-@fig:020]).

![Смена порта на 80](image/20.png){#fig:020 width=70%}

Удаляю привязку http_port_t к 81 порту:
semanage port -d -t http_port_t -p tcp 81 (рис. [-@fig:021]).

![Смена порта на 80](image/21.png){#fig:021 width=70%}

Проверяю, что 81 порт удален (рис. [-@fig:022]).

![Проверка порта](image/22.png){#fig:022 width=70%}

Удаляю файл test.html (рис. [-@fig:023]).

![Проверка порта](image/23.png){#fig:023 width=70%}

# Выводы

В этой работе я улучшил навыки администрирования Linux, разобрался в SELinux, и научился пользоваться httpd и Apache.


::: 
:::