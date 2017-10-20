---
id: 417
title: Относительные пути в svn:externals
date: 2011-08-09T10:38:31+00:00
author: dec5e
layout: post
guid: http://www.dec5e.ru/?p=417
permalink: /2011/08/svn-externals-relative/
categories:
  - Tips
tags:
  - svn
---
При переезде рабочего Subversion-репозитория на другой сервер «посыпались» экстерналы. Старый svn-репозиторий был не доступен, а правила для svn:externals были указаны в виде:

<pre><code class="no-highlight">foo_ext http://old-server.com/repo/foo 
bar_ext http://old-server.com/repo/bar</code></pre>

И естественно выпадали в ошибки.

Здесь и далее рассматривается условный репозиторий с такой структурой: корень находится в `repo`, а мы хотим подключить внешние папки `foo` и `bar` в папку `lib`:

<pre>repo\
   |-- bar\
   |-- foo\
   +-- lib\
       |-- bar_ext
       +-- foo_ext
</pre>

Захотелось во избежание подобных проблем в будущем указать пути относительно. Оказалось, такая возможность в Subversion доступна, [начиная с версии 1.5](http://subversion.apache.org/docs/release-notes/1.5.html#externals). При этом был изменен формат описания подключения внешних папок. Старый формат также допустим, но только при указании абсолютных путей. Предыдущий пример в новом формате должен выглядеть так:

<pre><code class="no-highlight">http://new-server.com/repo/foo foo_ext 
http://new-server.com/repo/bar bar_ext</code></pre>

Т.е. сначала путь, потом папка.

Допустимы несколько способов указания относительных путей:

`<strong>../</strong>` — относительно текущей папки:

<pre><code class="no-highlight">../../foo foo_ext 
../../bar bar_ext</code></pre>

`<strong>^/</strong>` — относительно корня репозитория:

<pre><code class="no-highlight">^/foo foo_ext 
^/bar bar_ext</code></pre>

`<strong>//</strong>` — независимо от схемы сети (http/https):

<pre><code class="no-highlight">//new-server.com/repo/foo foo_ext 
//new-server.com/repo/bar bar_ext</code></pre>

`<strong>/</strong>` —относительно корня сервера:

<pre><code class="no-highlight">/repo/foo foo_ext 
/repo/bar bar_ext</code></pre>

Что интересно, версии 1.5, описавшей новый формат указания svn:externals, уже три года. Но при этом практически все примеры и статьи в интернете используют старый формат.