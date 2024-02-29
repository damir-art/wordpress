# index.php - темизация WordPress
**index.php** - главный файл-шаблон темы WordPress, на основе которого создаются остальные файл-шаблоны темы.

Файл-шаблон index.php хранит в себе цикл (loop) и является главным файл-шаблоном для single.php, page.php, archive.php и других.

В index.php внедряются подключаемые файлы: header.php, footer.php между которыми располагают цикл.

Схема файл-шаблона index.php:

    <?php get_header(); ?>
      Цикл
    <?php get_footer(); ?>

## Подключаем изображения

    <img src="<?php echo get_template_directory_uri() ?>/img/header.png" width="600" height="400" alt="" />

Можно создать функцию и разместить её в `function.php`:

    function _po_dist_path( $path ) {
      return get_template_directory_uri() . '/dist/' . $path;
    }

Код для шаблона:

    <img src="<?php echo _po_dist_path( 'img/header.png' ) ?>" width="600" height="400" alt="" />
