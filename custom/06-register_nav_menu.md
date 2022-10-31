# Меню
`Админ > Внешний вид > Меню`  
https://wp-kama.ru/function/wp_nav_menu

Создание меню:
- регистрация области(ей) меню в `functions.php`
- создание меню в админке
- вывод меню в шаблоне `wp_nav_menu()`

Для создания вложенного меню (подходит и для Bootstrap), есть несколько способов, вот некоторые их них: 
- взять CSS-классы Bootstrap и применить их к к дефолтным классам меню WordPress которые создаются на автомате
- с помощью JS и DOM-объекта для работы с CSS-классами
- создать свой класс построения меню, на основе Walker_Nav_Menu{}, более сложный способ, где переопределяем методы:
  - start_lvl() - вложенность меню
  - start_el() - элементы меню
  - в параметре `walker`, создаём объект от нашего класса
  - всё это пишем в functions.php
- можно использовать уже готовый код для меню Bootstrap: https://github.com/AlexWebLab/bootstrap-5-wordpress-navbar-walker/blob/main/functions.php

Для меню можно задавать CSS-классы в БЭМ формате, для контейнера, списка, элементов, ссылок, активных ссылок/элементов и т.д. см: https://www.youtube.com/watch?v=US2y59bfilk

## Регистрация области в `functions.php`
В админке меню появится после её регистрации в functions.php

    /**
    * Функция для создания меню в WordPress
    */
    function legioner_register_nav_menus() {
      register_nav_menus(array(
        'header_menu' => 'Меню для шапки сайта', // Область для админки
        'footer_menu' => 'Меню для подвала сайта', // Область для админки
      ));
    }
    add_action( 'after_setup_theme', 'legioner_register_nav_menus' ); // Хук для подключения меню

## Создание меню в админке
- пишем имя меню (желательно английскими буквами, можно с пробелами и начинать в верхнем регистре)
- добавляем в меню необходимые ссылки: страницы, посты и т.п.
- нажимаем создать меню
- в админке можно создать сколько угодно меню
- созданные в админке меню выводим через **области**
- код областей создается в файле `functions.php`
- код вывода областей создается в файлах шаблонах

## Вывод меню в шаблоне
Вывод меню в шаблоне:

    wp_nav_menu(
      array(
        'theme_location' => 'header_menu', // Берется из functions.php
        'menu_class' => 'header_class', // добавит CSS-класс для меню, по-умолчанию menu
        'menu_id' => 'header_id', // добавит CSS-ID для меню, по умолчанию равен `menu-` и название меню в админке
        // 'container' => '', // указываем какими тегами обрамить меню ul, по-умолчанию div
        // 'container_class' => '', // класс для контейнера
        // 'container_id' => '', // id для контейнера
      )
    );

    /**
    * Формируем меню по БЭМ
    */
    wp_nav_menu(
      array(
        'theme_location' => 'top_menu',         // название области меню, указывается при регистрации в functions.php
        'container' => 'nav',                   // контейнер меню, указываем какими тегами обрамить меню ul, по-умолчанию div, false если не нужно
        'container_class' => 'nav header__nav', // класс для контейнера меню, по умолчанию menu-{menu slug}-container (menu-top-menu-container)
        'items_wrap' => '<ul class="%2$s">%3$s</ul>', // удаляем id из ul, не прописываем его
        'menu_class' => 'nav__lists',           // ul, добавит CSS-класс для меню, по-умолчанию menu
      )
    );

Все параметры вывода меню: https://developer.wordpress.org/reference/functions/wp_nav_menu/

Вывод по `БЭМ` (https://www.youtube.com/watch?v=US2y59bfilk) многоуровневое меню, активная ссылка:

    /**
    * Функция для создания меню в WordPress
    */
    function am_register_menus() {
      register_nav_menus(array(
        'top_menu' => 'Основное меню сверху сайта', // Область для админки
        'left_menu' => 'Основное меню в слева сайта', // Область для админки
      ));
    }
    add_action( 'after_setup_theme', 'am_register_menus' ); // Хук для подключения меню

    /*
    * Настройки wp_nav_menu через хук
    */
    function am_wp_nav_menu_args($args = 'top_menu') {
      if($args['theme_location'] === 'top_menu') {
        $args['container']       = 'nav';                        // контейнер меню, указываем какими тегами обрамить меню ul, по-умолчанию div, false если не нужно
        $args['container_class'] = 'nav header__nav';            // класс для контейнера меню, по умолчанию menu-{menu slug}-container (menu-top-menu-container)
        $args['items_wrap']      = '<ul class="%2$s">%3$s</ul>'; // удаляем id из ul, не прописываем его
        $args['menu_class']      = 'nav__lists';                 // ul, добавит CSS-класс для меню, по-умолчанию menu
      }
      return $args;
    }
    add_filter('wp_nav_menu_args', 'am_wp_nav_menu_args');

    /**
    * Удаляем id у элементов li, локация top_menu
    */
    function am_delete_id_item_menu($menu_id, $item, $args, $depth) {
      // Проверяем что изменяем меню из области top_menu
      return $args->theme_location === 'top_menu' ? '' : $menu_id;
    }
    add_filter('nav_menu_item_id', 'am_delete_id_item_menu', 10, 4);

    /**
    * Функция для добавления/удаления классов li элементам, локация top_menu
    */
    function am_add_css_class_item_menu( $classes, $item, $args, $depth ) {
      if ($args->theme_location === 'top_menu') {
        $classes = [ 'nav__list' ];
      }
      return $classes;
    }
    add_filter( 'nav_menu_css_class', 'am_add_css_class_item_menu', 10, 4 );

    /**
    * Функция для добавления/удаления классов a ссылкам, локация top_menu
    */
    function am_add_css_class_link_menu($atts, $item, $args, $depth) {
      if ($args->theme_location === 'top_menu') {
        $atts['class'] = 'nav__link locale-menu';
      }
      return $atts;
    }
    add_filter( 'nav_menu_link_attributes', 'am_add_css_class_link_menu', 10, 4 );

## Дополнительные параметры
Дополнительные параметры вывода меню в шаблоне:

    wp_nav_menu(
      array(
        'theme_location' => 'header_menu', // Берется из functions.php
        'menu_class' => ,
      )
    );

## register_nav_menu (functions.php)
Регистрируем меню в functions.php:
https://wp-kama.ru/function/register_nav_menu

    // Регистрация зон меню
    add_action('after_setup_theme', 'po_menus');

    function po_menus() {
      register_nav_menu('menu-top', 'Меню для шапки сайта');
      register_nav_menu('menu-footer', 'Меню для подвала сайта');
    }

https://wp-kama.ru/function/register_nav_menus

## Админка
В админке `Внешний вид > Меню` настраиваем меню и галочкой выбираем `Область отображения`.

## wp_nav_menu (header.php)
Внедряем меню в шаблоне, можно вывести одной строкой, с помощью `theme_location` или `menu`:

    wp_nav_menu( [
      // 'theme_location' => 'menu-top',
      'menu' => 'Верхнее меню',
    ] );

    wp_nav_menu( [
      'theme_location' => 'menu-footer',
      // 'menu' => 'Нижнее меню',
    ] );

https://wp-kama.ru/function/wp_nav_menu:

Полный список всех параметров:

    wp_nav_menu( [
      'theme_location'  => 'menu-top',
      'menu'            => '', 
      'container'       => 'div', // nav, false (оборачивает ul)
      'container_class' => '', 
      'container_id'    => '',
      'menu_class'      => '', 
      'menu_id'         => '',
      'echo'            => true,
      'fallback_cb'     => 'wp_page_menu',
      'before'          => '',
      'after'           => '',
      'link_before'     => '',
      'link_after'      => '',
      'items_wrap'      => '<ul id="%1$s" class="%2$s">%3$s</ul>',
      'depth'           => 0,
      'walker'          => '',
    ] );

## Разное
- Если страниц пока нет, то в меню можно создать произвольную ссылку и вместо URL вставить `#`

## Второй способ вывода меню
Больше контролируем структуру меню и его теги, не рекомендуемый способ. Делаем через функции WordPress.

- пункты меню получаем ввиде массива
- перебираем массив, циклом
- устанавливаем необходимые теги

Код в шаблоне:

    <?php
      // Получаем данные из меню
      $locations  = get_nav_menu_locations(); // получаем массив зон меню
      // var_dump($locations);
      $menu_id    = $locations[ 'menu-footer' ]; // получаем id меню
      $menu_items = wp_get_nav_menu_items( $menu_id, [
          'order' => 'ASC',
          'orderby' => 'menu_order',
      ]);
      // var_dump($menu_items); // вывод объектов, каждый объект это ссылка
    ?>

    <nav>
      <ul>
        <?php
          // Цикл перебора массива состоящего из элементов-объектов
          foreach( $menu_items as $item ):
        ?>
        <li>
          <a href="<?php echo $item->url ?>"><?php echo $item->title ?></a>
        </li>
        <?php
          endforeach;
        ?>
      </ul>
    </nav>

Добавляем класс для текущей страницы:

    <nav>
      <ul>
        <?php
          // Цикл перебора массива состоящего из элементов-объектов
          $http_s = 'http' . ( (isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] != 'off' ) ? 's' : '' ) . '://'; // проверяем на https
          $url = $http_s . $_SERVER['SERVER_NAME'] . $_SERVER['REQUEST_URI'];
          foreach( $menu_items as $item ):
            $class_text = '';
            if( $item->url === $url ) {
              $class_text = 'class="active"';
            }
        ?>
        <li <?php echo $class_text ?>>
          <a href="<?php echo $item->url ?>"><?php echo $item->title ?></a>
        </li>
        <?php
          endforeach;
        ?>
      </ul>
    </nav>
