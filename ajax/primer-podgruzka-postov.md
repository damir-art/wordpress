# Пример: подгрузка постов
https://www.youtube.com/watch?v=cLwC91Zx024&list=PLV2Ofep-dtIxwP2G27NTkqntN6Vq6nPx4&index=5 (пересмотреть)

Кликаем по рубрике в виджете и загружаются посты из рубрики по AJAX, аналогично работает и через клик по ссылке в меню.

- открываем инспектор
- смотрим id `#block-6` виджета рубрик или его класс `.widget_block`
- устанавливаем Query Monitor
- переходим на страницу рубрики
- у плагина Query Monitor сверху страницы кликаем по `Шаблон: archive.php`
- откроем файл `archive.php`
- смотрим между какими тегами располагается цикл вывода постов рубрик
- создаём плагин `ajax-cat` состоящий из 3х файлов:
  - `custom.js` - код AJAX-запроса
  - `ajax-cat.php` - код обработки AJAX-запроса
  - `tpl-cat.php` - шаблон вывода постов

## custom.js

    // Запускаем код после того как все HTML элементы будут загружены
    jQuery(document).ready(function ($) {

      // Контейнер с контентом помещаем в переменную, будем обращаться к нему несколько раз в коде (кешируем)
      var $mainBox = $('.site-main');

      // Обработчик перехватывающий щелчок по ссылке и отсылающий запрос
      // Отправка ajax запроса при клике по ссылке на рубрику в виджете "Рубрики"
      // Получаем ссылку > Отправляем её на сервер
      // Эмуляция перехода по ссылке, страница при этом не перегружается
      $('.wp-block-categories a, .primary-menu a').click(function (e) {
        e.preventDefault();

        // Получаем данные для запроса от ссылки
        var linkCat = $(this).attr('href'); // URL ссылки
        var titleCat = $(this).text(); // Текст ссылки (можно не делать)

        document.title = titleCat; // Меняем title страницы (можно не делать)
        history.pushState({page_title: titleCat}, titleCat, linkCat); // изменяем ссылку в браузере, добавляем страницу в историю браузера (можно не делать)

        ajaxCat(linkCat); // Передаем ссылку в запрос
      });

      // Отслеживание события нажатия кнопок браузера "Вперед/Назад" (можно не делать)
      // Эмулирует переход вперед/назад
      window.addEventListener("popstate", function (event) {
        document.title = event.state.page_title;
        ajaxCat(location.href);
      }, false);

      /**
      * Ajax запрос постов из рубрики по переданной ссылке на неё
      * @param linkCat ссылка на рубрику
      * Формирование AJAX-запроса, принимает ссылку на рубрику linkCat
      */
      function ajaxCat(linkCat) {
        // Блок с контентом делается полупрозрачным
        // Вместо прозрачности можно запустить прелоадер
        $mainBox.animate({opacity: 0.5}, 300);

        jQuery.post(
          myPlugin.ajaxurl, // ищи в ajax-cat.php
          {
            action: 'get_cat',
            link: linkCat // ссылка на рубрику
          },
          function (response) { // при возвращении обработанного запроса, вставляем его между тегами .site-main, убираем полупрозрачность у блока или прекращаем анимацию прелоадера
            $mainBox
              .html(response)
              .animate({opacity: 1}, 300);
          }
        );
      }
    });

## ajax-cat.php

    <?php
    /**
    * Plugin Name: Ajax Cat
    * Description: Ajax подгрузка постов из рубрик
    */
    function ajax_show_posts_in_cat() {
      $link = ! empty( $_POST['link'] ) ? esc_attr( $_POST['link'] ) : false;
      $slug = $link ? wp_basename( $link ) : false; // извлекаем слаг рубрики из ссылки
      $cat  = get_category_by_slug( $slug ); // возвращает объект терма (основная информация о рубрике)

      if ( ! $cat ) {
        die( 'Рубрика не найдена' );
      }

      // Формирование запроса списка постов из рубрики
      // Выбираем query_posts потому что в шаблоне есть пагинация, которая работает на основе глобальных данных, которые query_posts формирует
      query_posts( array(
        'posts_per_page' => get_option( 'posts_per_page' ), // получаем количесвто постов указанных в админке
        'post_status'    => 'publish',
        'category_name'   => $cat->slug
      ) );

      require plugin_dir_path( __FILE__ ) . 'tpl-cat.php';
      wp_die();
    }

    add_action( 'wp_ajax_get_cat', 'ajax_show_posts_in_cat' );
    add_action( 'wp_ajax_nopriv_get_cat', 'ajax_show_posts_in_cat' );

    // Подключаем JavaScript-файл `ajax-cat/custom.js`
    add_action( 'wp_enqueue_scripts', 'my_assets' );
    function my_assets() {
      wp_enqueue_script( 'custom', plugins_url( 'custom.js', __FILE__ ), array( 'jquery' ) );
      
      wp_localize_script( 'custom', 'myPlugin', array(
        'ajaxurl' => admin_url( 'admin-ajax.php' )
      ) );
    }

## tpl-cat.php

    <?php
    /**
    * Шаблон вывода названия рубрики и постов из неё темы Twenty Sixteen
    */
    if ( have_posts() ) : ?>

    <header class="page-header">
      <?php
        the_archive_title( '<h1 class="page-title">', '</h1>' );
        the_archive_description( '<div class="taxonomy-description">', '</div>' );
      ?>
    </header><!-- .page-header -->

    <?php
      while ( have_posts() ) : the_post();
        get_template_part( 'template-parts/content', get_post_format() );
      endwhile;

      // В archive.php пагинация выводится сразу на экран, и ссылку для пагинации берет из admin-ajax.php
      // Поэтому вместо неё используем get_the_posts_pagination и ссылку на текущую страницу admin-ajax.php заменяем на URL категории
      $pagination = get_the_posts_pagination( array(
        'prev_text'          => __( 'Previous page', 'twentysixteen' ),
        'next_text'          => __( 'Next page', 'twentysixteen' ),
        'before_page_number' => '<span class="meta-nav screen-reader-text">' . __( 'Page', 'twentysixteen' ) . ' </span>',
      ) );
      echo str_replace( admin_url( 'admin-ajax.php/' ), get_category_link( $cat->term_id ), $pagination );

    else :
      get_template_part( 'template-parts/content', 'none' );

    endif;


## Пояснения к коду
Когда мы заходим на страницу рубрики, то WordPress анализирует URL, на основе его делает запрос к БД. Когда мы обращаемся к `admin-ajax.php` у движка нет никаких данных чтобы определить на какой мы сейчас странице для формирования контента. Поэтому AJAX запрос должен содержать в себе все нужные данные: например переддать информацию о рубрике `id` или `slug` (они уникальны). id рубрики можно узнать у CSS-класса элемента li (вырезать число через регулярное выражение). slug рубрики содержится в ссылке (самый надежный метод) 