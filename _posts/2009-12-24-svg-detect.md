---
id: 3
title: Как проверить поддерживает ли браузер SVG
date: 2009-12-24T10:01:07+00:00
author: dec5e
layout: post
guid: http://dec5e.ru/?p=3
permalink: /2009/12/svg-detect/
categories:
  - Tips
tags:
  - svg
---
Для проверки нативной поддержки можно использовать два метода. Первый из [презентации](http://www.slideshare.net/Dmitry.Baranovskiy/web-vector-graphics-presentation) (страница 27) [Дмитрия Барановского](http://dmitry.baranovskiy.com/), автора [Raphaël](http://raphaeljs.com/):

<pre class="brush: jscript; title: ; notranslate" title="">if (window.SVGAngle) {...}</pre>

основан на проверке доступности интерфейса [SVGAngle](http://www.w3.org/TR/SVG/types.html#InterfaceSVGAngle) и позволяет определить наличие поддержки в принципе. Второй из [обсуждения](http://stackoverflow.com/questions/654112/how-do-you-detect-support-for-vml-or-svg-in-a-browser) на Stack Overflow:

<pre class="brush: jscript; title: ; notranslate" title="">if (document.implementation.hasFeature("http://www.w3.org/TR/SVG11/feature#BasicStructure", "1.1")) {...}</pre>

проверяет наличие поддержки определенной версии спецификации и даже определенных возможностей.

Оба метода работают в браузерах Firefox 2+ Win, Opera 9.0+ Win, Safari 3+ Win, Chrome 3+ Win, IE+ChromeFrame. Версии для других платформ у меня нет возможности  проверить, но, думаю, ситуация аналогичная. Хотя в <span>Safari 3.0.4 для MacOS</span> есть [баг](http://github.com/DmitryBaranovskiy/raphael/issues/closed/#issue/14), из-за которого первый способ не работает. После выхода 4й версии в августе доля Safari 3 составляет меньше 1 процента. Но все же второй вариант проверки выглядит предпочтительным и более правильным.