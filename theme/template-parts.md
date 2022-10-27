# template-parts
Помимо файл шаблонов в теме могут быть различные папки, их можно называть как угодно, но бычно есть именя папок которые используют чаще всего, например `template-parts`.

get_template_part() - ищет и подключает указанный файл темы  
echo get_post_format() - выведет формат поста (стандартный формат ничего не выводит)

**template-parts** - папка где хранят часто используемые куски кода шаблонов, например цикл
- функция используемая внутри цикла `get_template_part('template-parts/content', get_post_type())`
- файлы хранящиеся в папке `template-parts`
  - content-none.php
  - content-page.php
  - content-search.php
  - content.php
