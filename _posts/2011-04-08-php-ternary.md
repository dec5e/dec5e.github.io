---
id: 325
title: Сокращенный тернарный оператор в PHP
date: 2011-04-08T14:30:56+00:00
author: dec5e
layout: post
guid: http://www.dec5e.ru/?p=325
permalink: /2011/04/php-ternary/
categories:
  - Reference
  - Tips
tags:
  - php
---
Тернарный оператор — вещь известная и скучная:

<pre><code class="php">
$a = $expr1 ? $expr2 : $expr3;
</code></pre>

Если `$expr1` истинно, результатом `$a` станет `$expr2`, иначе `$expr3`.

А теперь об интересном! Начиная с версии PHP 5.3 можно опустить `$expr2`.

<pre><code class="php">
$a = $expr1 ?: $expr3;
</code></pre>

Это равносильно записи:

<pre><code class="php">
$a = $expr1 ? $expr1 : $expr3;
</code></pre>

Примеры:

<pre><code class="php">
$a = true ?: false; // true

$a = false ?: true; // true

$a = 1 ?: 2; // 1

$a = 0 ?: 2; // 2
</code></pre>

В [мануале](http://www.php.net/manual/en/language.operators.comparison.php#language.operators.comparison.ternary) об этом упоминается очень сухо и коротко. Ну а что еще добавить?