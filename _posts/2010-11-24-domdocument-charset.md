---
id: 250
title: Кодировки при работе с DOMDocument
date: 2010-11-24T00:20:46+00:00
author: dec5e
layout: post
guid: http://www.dec5e.ru/?p=250
permalink: /2010/11/domdocument-charset/
categories:
  - Reference
tags:
  - charset
  - dom
  - php
  - xml
---
[DomDocument](http://www.php.net/manual/en/class.domdocument.php) — мощный инструмент работы с XML и HTML с использованием методов <acronym title="Document Object Model">DOM</acronym>. Главное при его использовании не запутаться в кодировках.

Конструктор DOMDocument принимает два параметра — версию и кодировку документа

<pre><code class="php">$dom = new DOMDocument([ string $version [, string $encoding ]]);</code></pre>

А чтобы начать работать с существующим HTML-документом его надо загрузить в объект DOMDocument:

<pre><code class="php">$dom->loadHTML( string $source );</code></pre>

Проблема в том, что документ загружается совсем не в той кодировке, что указана в конструкторе. Вообще, **параметры конструктора никак не влияют на загруженный документ**. А используются только при методах `save()`:

<pre><code class="php">$htmlString = $dom->saveHTML ();</code></pre>

Так что параметры конструктора можно совсем опустить, если вы собираетесь только работать с существующим деревом и не планируете его изменять и выгружать.

При загрузке же кодировка определяется только исходя из содержимого, загружаемого документа. При этом достаточно часто объект не может определить кодировку верно. Поэтому желательно указывать ее вручную. Возможны [два способа](http://hinikato.blogspot.com/2008/10/php-xml.html). Добавить к импортируемому документу строку кодировки

<pre><code class="php">
$dom->loadHTML('&lt;meta http-equiv="Content-Type" content="text/html; charset=utf-8"&gt;' . $source);
</code></pre>

Либо сконвертировать все символы документа в HTML-entities перед импортом

<pre><code class="php">
$source = mb_convert_encoding($source, 'HTML-ENTITIES', 'utf-8');
$dom->loadHTML($source);
</code></pre>

Первый вариант кажется предпочтительнее в плане производительности. В плане результатов работы различий замечено не было.

Если работать с DOM планируется много, то удобно оформить эти конструкции в отдельный класс наследника DOMDocument, добавить в методе загрузки дерева второй параметр кодировки и определить метод, например, так:

<pre><code class="php">
class MyDOMDocument extends DOMDocument {
    public function loadHTML($source, $encoding) {
        $source = '&lt;meta http-equiv="Content-Type" content="text/html; charset='.
            $encoding.'"&gt;' . $source
        $dom->loadHTML('' . $source);
    }
}
</code></pre>