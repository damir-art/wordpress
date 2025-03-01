# WordPress начало
Начинаем работу с WordPress

- Установка WordPress
- Настройка WordPress
- Установка тем
- Установка начальных плагинов
- Создание дочерней темы *(после выбора окончательной темы)*

## robots.txt
Разместить в корень сайта, а не темы.

Код по-умолчанию:

  User-agent: *
  Disallow: /wp-admin/
  Allow: /wp-admin/admin-ajax.php

  Sitemap: https://site.ru/wp-sitemap.xml

Sitemap XML уже встроен:

  Sitemap: https://site.ru/wp-sitemap.xml

## Плагины
Плагины WordPress, которые сразу ставим:
- Show Current Template
- Cyr-To-Lat
- Akismet Anti-Spam (активация)
- Contact Form 7 (капча в гугл)
- Duplicate Page (дублирование записей)
- Slim SEO (лёгкий SEO плагин, к сожалению удаляет Sitemap XML Вордпресса)
- Breadcrumb NavXT (установка http://mtekk.us/code/breadcrumb-navxt/#basic)

Разное:
- `show current template` - (рекомендуется) плагин показывающий какой шаблон сейчас используется
- `which template` - плагин показывающий какой шаблон сейчас используется
- `which template file` - плагин показывающий какой шаблон сейчас используется
