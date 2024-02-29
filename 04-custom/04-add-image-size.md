# Миниатюра
Создаём миниатюры.

    add_image_size( 'theme-thumb', 164, 164, true ); // обрезает изображение
    add_image_size( 'theme-thumb', 164, 164, false ); // уменьшает изобржение по высоте или ширине, в зависимости какая сторона длиннее

Миниатюра - это специальная картинка которая прикрепляется к посту и выводится только в цикле. Настроить миниатюры можно в `Настройки > Медиафайлы` или через functions.php

thumb (thumbnail), medium, large - берутся из `Настройки > Медиафайлы`

## Подключаем миниатюру к сайту
### function.php
Чтобы иметь возможность использовать миниатюры на сайте WordPress, нужно в файл `functions.php` добавить следующий код (в админке появится блок для добавления миниатюр):

    add_theme_support('post-thumbnails');

    // только для page.php
    add_theme_support('post-thumbnails', array('page'));

### index.php
В файл-шаблон index.php в цикл, добавляем:

    if ( has_post_thumbnail() ) {
      the_post_thumbnail();
    }

    или

    <?php if(has_post_thumbnail()): ?>
      <?php the_post_thumbnail(); ?>
    <?php else: ?>
      <img src="<?php bloginfo('template_url'); ?>/img/thumb-default.jpg" />
    <?php endif; ?>

Данный код проверяет и выводит миниатюру поста. Если миниатюры нет, то выводит миниатюру по-умолчанию.

## Регистрация пользовательских размеров миниатюр
Регистрация пользовательских размеров миниатюр, изображений. Пользоватеские размеры создаются при загрузке изображения на сайт.

    add_image_size( 'myThumbName', 300, 200, true );

    имя    - пользовательского размера
    ширина - пользовательского размера
    высота - пользовательского размера
    false  - масштабирование, уменьшит по высоте, пропорции сохранятся (по-умолчанию)
    true   - кадрирование, уменьшит по высоте, обрежит ширину не сохраняя пропорции
    array( 'left', 'top') - обрезать с левого верхнего угла

Обрезаем по нужной стороне:

    add_image_size( 'homepage-thumb', 500, 9999 ); - обрезаем по высоте
    add_image_size( 'homepage-thumb', 9999, 500 ); - обрезаем по ширине

thumb, thumbnail, medium, large, post-thumbnail - данные имена запрещены.

## set_post_thumbnail_size()
Устанавливает/переопределяет дефолтный размер миниатюры поста.

Все примеры для functions.php:

    set_post_thumbnail_size( $width, $height, $crop );

## Пример

В functions.php

    if ( function_exists( 'add_theme_support' ) ) {
      add_theme_support( 'post-thumbnails' );
        set_post_thumbnail_size( 300, 300 ); // размер миниатюры поста по умолчанию
    }

    if ( function_exists( 'add_image_size' ) ) {
      add_image_size( 'category-thumb', 300, 9999 ); // 300 в ширину и без ограничения в высоту
      add_image_size( 'homepage-thumb', 220, 180, true ); // Кадрирование изображения
    }

В index.php

    if ( has_post_thumbnail() ) {
      the_post_thumbnail( 'category-thumb' );
    }
