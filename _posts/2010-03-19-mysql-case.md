---
id: 69
title: Регистр в именах таблиц MySQL
date: 2010-03-19T13:46:26+00:00
author: dec5e
layout: post
guid: http://www.dec5e.ru/?p=69
permalink: /2010/03/mysql-case/
categories:
  - Tips
tags:
  - mysql
---
Каждая таблица MySQL связана с файлом в системе. Потому при переносе проекта с Windows на рабочий \*nix-хостинг нажно помнить, что под Windows имена таблиц не чувствительны к регистру, а на большинстве систем \*nix чувствительны. Хорошим решением не наткнуться на эти грабли будет везде использовать строчные буквы при именовании таблиц. Тем более, что официальный мануал именно такой вариант и [рекомендует](http://dev.mysql.com/doc/refman/5.0/en/identifier-case-sensitivity.html). Тем не менее существует возможность влиять на то, как система работает с регистром с помощью параметра `lower_case_table_names`. Этот параметр может принимать три значения:

  * 0 — значение по-умолчанию на *nix-системах. Идентификаторы хранятся в том виде, в каком были указаны при создании. Имена регистрозависимы.
  * 1 — значение по умолчанию для Windows. Идентификаторы хранятся на диске в строчных буквах, имена регистронезависимы.
  * 2 — значение по умолчанию для MacOS. Идентификаторы хранятся в том виде, в каком были указаны при создании, но при поиске MySQL конвертирует их в строчные. Поиск имен регистронезависим. Это работает только на регистронезависимых системах! Таблицы `InnoDB` хранятся в строчных, как при `lower_case_table_names=1`.

Стоит добавить, что названия столбцов и индексов во всех системах регистронезависимы.

Подробнее читайте в [мануале](http://dev.mysql.com/doc/refman/5.0/en/identifier-case-sensitivity.html).