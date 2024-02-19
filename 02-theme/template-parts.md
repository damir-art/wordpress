# template-parts
get_template_part() - ищет и подключает указанный файл темы  
echo get_post_format() - выведет формат поста (стандартный формат ничего не выводит)

**template-parts** - папка где хранят часто используемые куски кода шаблонов, например цикл
- функция используемая внутри цикла `get_template_part('template-parts/content', get_post_type())`
- файлы хранящиеся в папке `template-parts`
  - content-none.php
  - content-page.php
  - content-search.php
  - content.php

Подключение файла:

    /**
    * Функции создания пунктов меню в панели администратора
    */
    require_once get_template_directory() . '/inc/admin-functions.php';

Разное:  
Помимо файл шаблонов в теме могут быть различные папки, их можно называть как угодно, но бычно есть имена папок которые используют чаще всего, например `template-parts`.

Отнести к разделу курса: Организация кода темы.
