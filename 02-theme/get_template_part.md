# get_template_part() - многоразовый код
Если какой-нибудь код в теме WordPress используется более одного раза, то его нужно вынести в отдельный файл и подключать с помощью функции `get_template_part()`.

Подключаемый код, относится к понятию программирования **DRY** *(don't repeat yourself)* - не повторяйся. В вашем проекте, не должно быть ни одного кода, который бы повторялся более одного раза.

Чтобы подключить, ту или иную часть кода к теме, можно вместо `require()` или `include()` использовать спецально для этого созданную функцию WordPress:

    get_template_part();

С помощью данной функции, можно подключать файлы содержащие код:

- хлебных крошек
- меню
- пагинации
- цикла и т.д.

Подключаем файл breadcrumbs.php:

    get_template_part('breadcrumbs');

Подключаем файл breadcrumbs-footer.php:

    get_template_part('breadcrumbs', 'footer');

Пропишем в шаблоне page.php
    
    <?php get_template_part('parts/content', 'page'); ?>
    
Данная строка, будет ссылаться на файл `parts/content-page.php`

## require()
Преимущества по сравнению с `require()` или `include()`:
- не нужно указывать путь до темы,
- возможность использования дочерних тем

Аналог get_header():

    <?php get_header(); ?>
    <?php include(TEMPLATEPATH . '/header.php'); ?>

## Примеры

    // parts/part.php
    get_template_part("parts/part");

    // parts/part-one.php
    get_template_part("parts/part", "one");

    // третьим параметром можно передавать массив данных
    get_template_part("parts/part", "two", array());

## Разное
- https://wp-kama.ru/function/get_template_part
