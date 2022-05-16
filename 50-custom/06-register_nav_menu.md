# Меню
Создание меню:
- создание меню в админке
- регистрация области меню в `functions.php`
- вывод меню в шаблоне

## Создание меню в админке
`Админ > Внешний вид > Меню`:
- пишем имя меню (желательно английскими буквами, можно с пробелами и начинать в верхнем регистре)
- добавляем в меню необходимые ссылки: страницы, посты и т.п.
- нажимаем создать меню
- в админке можно создать сколько угодно меню
- созданные в админке меню выводим через **области**
- код областей создается в файле `functions.php`
- код вывода областей создается в файлах шаблонах

## Регистрация области в `functions.php`

    /**
    * Функция для создания меню в WordPress
    */
    function cars_register_menus() {
        register_nav_menus(array(
            'header_menu' => 'Меню для шапки сайта', // Область для админки
            'footer_menu' => 'Меню для подвала сайта', // Область для админки
        ));
    }
    add_action( 'after_setup_theme', 'cars_register_menus' ); // Хук для подключения меню

## Вывод меню в шаблоне
Вывод меню в шаблоне:

    wp_nav_menu(
        array(
            'theme_location' => 'header_menu', // Берется из functions.php
            'menu_class' => 'header_class', // добавит CSS-класс для меню, по-умолчанию menu
            'menu_id' => 'header_id', // добавит CSS-ID для меню, по умолчанию равен `menu-` и название меню в админке
            // 'container' => '' // указываем какими тегами обрамить меню ul, по-умолчанию div
            // 'container_class' => '' // класс для контейнера
            // 'container_id' => '' // id для контейнера
        )
    );

Все параметры вывода меню: https://developer.wordpress.org/reference/functions/wp_nav_menu/

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
