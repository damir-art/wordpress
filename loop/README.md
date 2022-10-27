# Что такое цикл в WordPress (the loop)
Цикл WordPress - это перебор записей (постов) и вывод информации о посте в цикле.  
Цикл получает массив записей, каждая запись это объект. В WordPress есть специальные функции используемые внутри цикла, для получения той или иной информации о посте.

Цикл используется в таких файл-шаблонах как: index.php, category.php, tag.php

## Как выглядит цикл
Код стандартного цикла WordPress:

    <!-- Цикл WordPress -->
    <?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
      <h3><a href="<?php the_permalink() ?>"><?php the_title() ?></a></h3>
      <?php the_content() ?>
    <?php endwhile; else : ?>
      <p>Записей нет.</p>
    <?php endif; ?>

Другие виды записей циклов смотрите ниже.  
Вся информация о записи (посте), хранится в глобальной переменной `$post`.  
Переменная `$post` создаётся внутри цикла для каждой записи.  
До цикла, функция `have_posts()` обращается к массиву записей, глобальной переменной `$wp_query`, данные в этой переменной в зависимости от страницы (одиночная, архив) будут разные.  
Создать массив записей $wp_query и получить из него данные можно также через пользовательские циклы.  
В зависимости от количества записей в цикле, переменная `$post` меняется столько же раз.  
Функции цикла WordPress, типа `the_title()`, берут информацию из `$post`.

Глобальная переменная `$wp_query` - хранит массив записей, результат запроса к БД. Массив записей затем обрабатывается циклом. `$wp_query` используется в стандартном цикле и цикле `query_posts()`.

Глобальная переменной `$query_string`, содержит базовый запрос для функции query_posts.

## Что содержится внутри $wp_query и $post
Что содержится внутри глобальных переменных `$wp_query` и `$post` можно узнать через print_r():

    <?php
      echo '<pre>';
      print_r($wp_query);
      echo '</pre>';
    ?>

    <!-- Цикл WordPress -->
    <?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
      <?php
        echo '<pre>';
        print_r($post);
        echo '</pre>';
      ?>
      <h3><a href="<?php the_permalink() ?>"><?php the_title() ?></a></h3>
      <?php the_content() ?>
    <?php endwhile; else : ?>
      <p>Записей нет.</p>
    <?php endif; ?>

## Функции цикла
Функции цикла, работают только внутри цикла (их еще называют тегами шаблонов):

- the_title()     - выводит заголовок записи
- the_excerpt()   - выводит отрывок контента записи
- the_content()   - выводит контент записи
- the_permalink() - выводит ссылку на запись
- the_date()      - выводит дату создания записи

Чтобы тег шаблона сработал, нужно определить переменную `$post` которая не известна за пределами цикла или может быть не корректной (содержать данные последнего обработанного в цикле поста).

Полный список тегов шаблона: https://wp-kama.ru/functions/template-tags

## Как еще можно записать цикл?

    <?php if ( have_posts() ) { while ( have_posts() ) { the_post(); ?>
      <!-- Цикл WordPress -->
      <!-- Вывод постов: the_title() и т.д. -->
    <?php } } else { ?>
      <p>Записей нет.</p>
    <?php } ?>

Сначала while():

    <?php while ( have_posts() ){ the_post(); ?>
      <!-- Цикл WordPress -->
      <!-- Вывод постов: the_title() и т.д. -->
    <?php } ?>
    <?php if ( ! have_posts() ){ ?>
      <p>Записей нет.</p>
    <?php } ?>

Полный пример цикла, взятый с `wp-kama.ru`:

    <!-- Проверка наличия записей в цикле -->
    <?php if ( have_posts() ) : ?>
      <!-- Начало цикла -->
      <?php while ( have_posts() ) : the_post(); ?>
        <!-- Цикл WordPress -->
        <!-- Здесь уже определилась переменная $post, -->
        <!-- на основе которой будет строится дальнейший код. -->
        <!-- $post будет меняться для каждого поста while( have_posts() ). -->
        <!-- $post нужна, чтобы работали теги шаблона: in_category('3'), the_permalink() и т.д. -->

        <!-- Проверка находится ли этот пост в категории 3. -->
        <!-- Если да, то для div задаем CSS класс class="post-cat-three". -->
        <!-- Если нет, то класс будет post class="post". -->
        <?php if ( in_category('3') ) { ?>
          <div class="post-cat-three">
        <?php } else { ?>
          <div class="post">
        <?php } ?>

          <!-- Выводим заголовок поста, как ссылку на сам пост. -->
          <h2><a href="<?php the_permalink() ?>" title="Ссылка на: <?php the_title_attribute(); ?>"><?php the_title(); ?></a></h2>

          <!-- Выводим дату поста и ссылку на другие записи автора. -->
          <small><?php the_time('F jS, Y') ?> Автор: <?php the_author_posts_link() ?></small>

          <!-- Выводим текст поста в теге div. -->
          <div class="entry">
            <?php the_content(); ?>
          </div>

          <!-- Выводим категории поста, через запятую. -->
          <p class="postmetadata">Расположено в <?php the_category(', '); ?></p>
        </div> <!-- закрываем основной тег div -->

        <!-- Отсюда цикл начинает повторятся, если есть еще посты -->
        <!-- Останавливаем цикл (endwhile), -->
      <?php endwhile; ?>
      <!-- Полное окончание цикла. -->

    <!-- Если записей в цикле нет - цикл не сработал (else) -->
    <?php else: ?>
      <p>Нет постов в цикле.</p>
    <?php endif; ?>

## Что находится внутри переменной $post

    WP_Post Object
    (
        [ID] => 8
        [post_author] => 1
        [post_date] => 2022-10-06 12:06:08
        [post_date_gmt] => 2022-10-06 09:06:08
        [post_content] => Текст записи 1
        [post_title] => Запись 1
        [post_excerpt] => 
        [post_status] => publish
        [comment_status] => open
        [ping_status] => open
        [post_password] => 
        [post_name] => zapis-1
        [to_ping] => 
        [pinged] => 
        [post_modified] => 2022-10-06 12:06:08
        [post_modified_gmt] => 2022-10-06 09:06:08
        [post_content_filtered] => 
        [post_parent] => 0
        [guid] => http://wpboo.loc/?p=8
        [menu_order] => 0
        [post_type] => post
        [post_mime_type] => 
        [comment_count] => 0
        [filter] => raw
    )

Прописывать ли `global $post;` перед циклом?

Нет не обязательно. Глобальная переменная будет использована методами, её обязательно нужно сбросить после цикла! Если она тебе нужна где-то в цикле, например из нее нужны какие-то данные, то можно перед циклом указать global $post; и использовать эту переменную внутри цикла. Данные в ней будут меняться по ходу цикла.