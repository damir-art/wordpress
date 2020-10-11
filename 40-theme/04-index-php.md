# index.php - темизация WordPress
**index.php** - главный файл-шаблон темы WordPress, на основе которого создаются остальные файл-шаблоны темы.

Файл-шаблон index.php хранит в себе цикл (loop) и является главным файл-шаблоном для single.php, page.php, archive.php и других.

В index.php внедряются подключаемые файлы: header.php, footer.php между которыми располагают цикл.

Схема файл-шаблона index.php:

    <?php get_header(); ?>

        Цикл

    <?php get_footer(); ?>