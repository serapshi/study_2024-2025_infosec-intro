---
## Front matter
title: "Отчёта по лабораторной работе №4"
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

Получение практических навыков работы в консоли с расширенными
атрибутами файлов1
.


# Задание


1. От имени пользователя guest определите расширенные атрибуты файла
/home/guest/dir1/file1 командой
lsattr /home/guest/dir1/file1
2. Установите командой
chmod 600 file1
на файл file1 права, разрешающие чтение и запись для владельца файла.
3. Попробуйте установить на файл /home/guest/dir1/file1 расширенный атрибут a от имени пользователя guest:
chattr +a /home/guest/dir1/file1
В ответ вы должны получить отказ от выполнения операции.
4. Зайдите на третью консоль с правами администратора либо повысьте
свои права с помощью команды su. Попробуйте установить расширенный атрибут a на файл /home/guest/dir1/file1 от имени суперпользователя:
chattr +a /home/guest/dir1/file1
5. От пользователя guest проверьте правильность установления атрибута:
lsattr /home/guest/dir1/file1
6. Выполните дозапись в файл file1 слова «test» командой
echo "test" /home/guest/dir1/file1
После этого выполните чтение файла file1 командой
cat /home/guest/dir1/file1
Убедитесь, что слово test было успешно записано в file1.
7. Попробуйте удалить файл file1 либо стереть имеющуюся в нём информацию командой
echo "abcd" > /home/guest/dirl/file1
Попробуйте переименовать файл.
8. Попробуйте с помощью команды
chmod 000 file1
установить на файл file1 права, например, запрещающие чтение и запись для владельца файла. Удалось ли вам успешно выполнить указанные команды?
9. Снимите расширенный атрибут a с файла /home/guest/dirl/file1 от
имени суперпользователя командой
chattr -a /home/guest/dir1/file1
Повторите операции, которые вам ранее не удавалось выполнить. Ваши
наблюдения занесите в отчёт.
10. Повторите ваши действия по шагам, заменив атрибут «a» атрибутом «i».
Удалось ли вам дозаписать информацию в файл? Ваши наблюдения занесите в отчёт.
В результате выполнения работы вы повысили свои навыки использования интерфейса командой строки (CLI), познакомились на примерах с тем,
как используются основные и расширенные атрибуты при разграничении
доступа. Имели возможность связать теорию дискреционного разделения
доступа (дискреционная политика безопасности) с её реализацией на практике в ОС Linux. Составили наглядные таблицы, поясняющие какие операции возможны при тех или иных установленных правах. Опробовали действие на практике расширенных атрибутов «а» и «i».

# Выполнение лабораторной работы

Загружаюсь от пользователя guest
 (рис. [-@fig:001]).

![Запуск системы](image/1.png){#fig:001 width=70%}

Установливаю командой
chmod 600 file1
на файл file1 права, разрешающие чтение и запись для владельца файла. (рис. [-@fig:002]).

![Установка ограничения и проверка](image/2.png){#fig:002 width=70%}

Не получается установить на файл /home/guest/dir1/file1 расширенный атрибут a от имени пользователя guest:
chattr +a /home/guest/dir1/file1
 (рис. [-@fig:003]).

![Команда chattr](image/3.png){#fig:003 width=70%}

Захожу на третью консоль с правами администратора, устанавливаю расширенный атрибут a на файл /home/guest/dir1/file1 от имени суперпользователя:
chattr +a /home/guest/dir1/file1 (рис. [-@fig:004]).

![Установка атрибута a](image/4.png){#fig:004 width=70%}

Выполните дозапись в файл file1 слова «test» командой
echo "test"  /home/guest/dir1/file1 (рис. [-@fig:005]).

![Дозапись в файл](image/5.png){#fig:005 width=70%}

Пробую  стереть имеющуюся в файле информацию командой
echo "abcd" > /home/guest/dirl/file1 (рис. [-@fig:006]).

![Попытка стереть данные](image/6.png){#fig:006 width=70%}

Пробую переименовать файл (рис. [-@fig:007]).

![Попытка переименовать](image/7.png){#fig:007 width=70%}

Пытаюсь командой
chmod 000 file1
установить на файл file1 права, например, запрещающие чтение и запись для владельца файла. (рис. [-@fig:008]).

![Попытка сменить права файла](image/8.png){#fig:008 width=70%}

Убераю атрибут a (рис. [-@fig:009]).

![chattr -a](image/9.png){#fig:009 width=70%}

Успешно дозаписываю в файл (рис. [-@fig:010]).

![Дозапись в файл](image/10.png){#fig:010 width=70%}

Успешно меняю имя файла (рис. [-@fig:011]).

![Смена имени файла](image/11.png){#fig:011 width=70%}

Успешно меняю права файла (рис. [-@fig:012]).

![Смена прав файла](image/12.png){#fig:012 width=70%}

Добавляю атрибут i (рис. [-@fig:013]).

![Атрибут i](image/13.png){#fig:013 width=70%}

Безуспешно повторяю серию команд (рис. [-@fig:014]).

![Повтор команд с атрибутом i](image/14.png){#fig:014 width=70%}




# Выводы

В результате выполнения работы я повысил свои навыки использования интерфейса командой строки (CLI), познакомился на примерах с тем,
как используются основные и расширенные атрибуты при разграничении
доступа. Опробовал действие на практике расширенных атрибутов «а» и «i».




::: 
:::