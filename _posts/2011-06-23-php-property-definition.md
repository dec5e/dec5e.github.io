---
id: 373
title: Объявление полей классов в PHP
date: 2011-06-23T12:34:45+00:00
author: dec5e
layout: post
guid: http://www.dec5e.ru/?p=373
permalink: /2011/06/php-property-definition/
categories:
  - Tips
tags:
  - php
---
_Давно хотел разобраться, что будет если поле не объявлено явно в классе, но используется для чтения/записи. Периодически натыкался на такие конструкции в чужом коде, считал неверным, а потому сам никогда  не использовал и ответа на вопрос «Что же все-таки будет?» не знал. Кстати, в официальном мануале ничего об этом не нашел. Буду рад, если подскажете, где просмотрел._

Итак. Поля классов могут задаваться явно с помощью ключевых слов `private`, `protected` и `public`, определяющих видимость поля:

<pre><code class="php">
class A {

    private $foo;

    protected $bar;

    public $baz;
}
</code></pre>

Есть еще конструкция `var`, но это просто устаревший аналог `public`.

А могут задаваться неявно, просто обращением к этому полю:

<pre><code class="php">
class A {

    public function getFoo() {
        return $this-&gt;foo;
    }

    public function setFoo($value) {
        $this-&gt;foo = $value;
    }
}
</code></pre>

Второй способ никуда не годится:

  * Неявно определенное поле получает уровень доступ `public`. При явном определении — мы сразу указываем уровень доступа.
  * Если вызвать метод, читающий значение поля до его инициализации, то PHP сгенерирует `Notice` (при первом способе его нет). А поле будет инициализировано значением `null`.
  * То, что это поле есть в объекте, не получится узнать через `Reflection`, `property_exists` и `get_class_vars`. Только через `isset` и `get_object_vars`, и то только, если полю было присвоено значение.
  * С такими полями неудобно работать в редакторе — IDE их не подсказывает и вообще подсвечивает, как ошибочные. Можно описать эти поля с помощью PHPDoc тегами `property`, но непонятно, почему бы тогда не описать их явно как поля класса :).

В общем, **не пользуйтесь никогда вторым способом**. Он усложняет и запутывает код и является источником ошибок. Единственно, когда он имеет право на существование — это динамическое создание полей объекта. И даже тогда можно справиться геттерами и сеттерами (правда с потерями в производительности, при малом количестве вызовов потери незначительны).