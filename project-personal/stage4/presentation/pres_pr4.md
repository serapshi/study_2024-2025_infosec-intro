---
## Front matter
lang: ru-RU
title: Индивидуальный проект этап №4
subtitle: Основы информационной безопасности
author:
  - Павлюченков С.В.
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 07 сентября 25

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---


## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Павлюченков Сергей Витальевич
  * Студент ФФМиЕН
  * Российский университет дружбы народов
  * [1132237372@pfur.ru](mailto:1132237372@pfur.ru)
  * <https://serapshi.github.io/svpavliuchenkov.github.io/>

:::
::: {.column width="30%"}

![](./image/my_photo.jpg)

:::
::::::::::::::


## Цель работы

Использование nikto
nikto — базовый сканер безопасности веб-сервера. Он сканирует и обнаруживает уязвимости в веб-приложениях, обычно вызванные неправильной конфигурацией на самом сервере, файлами, установленными по умолчанию, и небезопасными файлами, а также устаревшими серверными приложениями.


# Выполнение лабораторной работы


## Проверка факта установки nikto 

![Параметры nikto](image/1.png){#fig:001 width=70%}

## Изменение настроек безопасности DVWA на низкие

![Меняю защиту на слабую](image/2.png){#fig:002 width=70%}

##  Запуск nikto

![Поиск уязвимостей DVWA](image/3.png){#fig:003 width=70%}


## Результат работы 

![Список уязвимостей ](image/4.png){#fig:004 width=70%}

## Изменение настроек безопасности DVWA на высокие

![Подключение ](image/7.png){#fig:005 width=70%}

## Результат работы 

![Список уязвимостей ](image/8.png){#fig:006 width=70%}


## Выводы

В этом этапе я научился пользоваться nikto вместе с dvwa для нахождения уязвимостей с примерами из dvwa.