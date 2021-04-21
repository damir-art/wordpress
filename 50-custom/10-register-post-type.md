# Пользовательские типы записей
Создание пользовательских типов записей.

По-умолчанию в WordPress существует два типа записей (типа контента) это посты и страницы, но можно создавать свои собственные.

Пользовательские записи при постраничной навигации выводятся в шаблонах с именем `archive-Name.php`.

- `register_post_type()` создает новый тип записи, содержит два параметра `имя записи` и массив
- `init` хук к оторому прикрепляется пользовательская функция

Код:

    // Регистрация пользовательских типов записей
    add_action( 'init', 'po_register_types' );

    function po_register_types() {
        register_post_type( 'news' ,
            [
                'labels'                 => array(
                    'name'               => 'Новости',                       // Основное название типа записи (заголовок списка), также попадает в title
                    'singular_name'      => 'Новость',                       // Название для одной записи этого типа
                    'add_new'            => 'Добавить новость',              // Для добавления новой записи (в меню)
                    'add_new_item'       => 'Добавить новость',              // Для заголовка у вновь создаваемой записи в админ-панели
                    'edit_item'          => 'Редактировать новость',         // Для редактирования типа записи
                    'new_item'           => 'Новая новость',                 // Текст новой записи
                    'view_item'          => 'Посмотреть новость',            // Для просмотра записи этого типа
                    'search_items'       => 'Найти новость',                 // Для поиска по этим типам записей
                    'not_found'          => 'Новостей не найдено',           // Если в результате поиска ничего не было найдено
                    'not_found_in_trash' => 'В корзине новостей не найдено', // Если не было найдено в корзине
                    'parent_item_colon'  => '',                              // Для родителей в древовидных типах
                    'menu_name'          => 'Новости'                   // Название в меню
                ),
                'public'             => true,
                'has_archive'        => true,                               // true - регистарция URL под этот тип записей
                'menu_position'      => 20,
                'menu_icon'          => 'dashicons-welcome-write-blog',     // _po_dist_path( 'img/news.png' )
                'supports'           => [ 'title', 'editor', 'thumbnail' ], // title (должен быть всегда), 'editor', 'author', 'thumbnail', 'excerpt', 'comments'
                // 'publicly_queryable' => true,
                // 'show_ui'            => true,
                // 'show_in_menu'       => true,
                // 'query_var'          => true,
                // 'rewrite'            => true,
                // 'capability_type'    => 'post',
                // 'hierarchical'       => false, // Вложенность записей, будет ли запись типа `страница`
                'show_in_rest'        => true, // Включить Гутенберг для пользовательских типов записей
            ]
        );
    }

- Ярлык `news` должен быть уникальным
- В меню появится ссылка на новый тип записи, выбираем вкладку `все`
- Файл в теме должен называться `archive-news.php`
- Внутри функции `po_register_types` может быть несколько `register_post_type` в зависимости от количества типов записей на сайте
- Если `'menu_position' => 20,` у нескольких записей совпадают то в админке они будут идти друг за другом
- `'has_archive' => true,` выставляем в `false` если нам не нужно выводить записи отдельно на своих страницах, а нужно выводить их в виде цикла на главной или других страницах не принадлежащих данному типу записи

**Важно:** после создания нового типа записи, обязательно нужно зайти в: `Админка > Настройки > Постоянные ссылки`. Это необходимо для того, чтобы правила ЧПУ работали и для нового типа записи.

1. Создаем пользовательскую запись (car)
2. Создаём цикл для вывода пользовательской записи (car)
3. Создаём пользовательскую таксономию для (car)

Все записи в WordPress, хранятся в базе данных wp_posts.

    function create_post_type_car() {
        register_post_type('car',
            array(
                'labels' => array(
                    'name' => __( 'Cars' ),
                    'singular_name' => __( 'Car' ),
                    'add_new' => __( 'Добавить авто' ),
                ),
                'menu_position' => 5,
                'supports' => array('title','editor','thumbnail'),
                'rewrite' => 'avtomobili',
                'public' => true,
                'has_archive' => true
            )
        );
    }
    add_action( 'init', 'create_post_type_car');

- `init` - хук к которому мы прикрепляем пользовательскую функцию create_post_type_car()
- `register_post_type()` - функция регистрации пользовательского поста
- `car` - название типа поста (post, page и т.д.)
- `labels` - массив настроек, текст в админке

Файл-шаблоны пользовательских записей:

    archive-car.php   - шаблон вывода пользовательских записей
    single-car.php    - шаблон вывода пользовательской записи
    taxonomy-year.php - шаблона вывода пользовательской таксономии

## Цикл вывода пользовательской записи
Выводим автомобили в цикле

    <?php
        $args = array( 'post_type' => 'car', 'posts_per_page' => 10 );
        $loop = new WP_Query( $args );
        while ( $loop->have_posts() ) : $loop->the_post();
    ?>
            <a href="<?php the_permalink(); ?>"> <?php the_title(); ?> </a>
    <?php
            echo '<div class="entry-content">';
            the_content();
            echo '</div>';
        endwhile;
    ?>

## Таксономия
Создание таксономии для пользовательской записи `car`.

    function tax_car() {
        register_taxonomy(
            'year',
            'car',
            array(
                'label' => __( 'Year' ),
                'rewrite' => array( 'slug' => 'years' ),
                'capabilities' => array(
                    'assign_terms' => 'edit_guides',
                    'edit_terms' => 'publish_guides'
                )
            )
        );
    }
    add_action( 'init', 'tax_car' );

Выводим список терминов таксономии, код размещаем в archive-car.php над циклом:

    $terms = get_terms(
        array(
            'taxonomy' => 'year',
            'hide_empty' => false,
        )
    );

    $output = '';
    foreach($terms as $term){
        $output .= '<input type="checkbox" name="terms" value="' . $term->name . '" /> ' . $term->name . '<br />';
    }

    // echo $output;

## Миша Рудрастых

    add_action( 'init', 'true_register_products' ); 

    function true_register_products() {
        $labels = array(
            'name' => 'Товары',
            'singular_name' => 'Товар', // админ панель Добавить->Функцию
            'add_new' => 'Добавить товар',
            'add_new_item' => 'Добавить новый товар', // заголовок тега
            'edit_item' => 'Редактировать товар',
            'new_item' => 'Новый товар',
            'all_items' => 'Все товары',
            'view_item' => 'Просмотр товаров на сайте',
            'search_items' => 'Искать товары',
            'not_found' =>  'Товаров не найдено.',
            'not_found_in_trash' => 'В корзине нет товаров.',
            'menu_name' => 'Товары' // ссылка в меню в админке
        );
        $args = array(
            'labels' => $labels,
            'public' => true, // благодаря этому некоторые параметры можно пропустить
            'menu_icon' => 'dashicons-cart', // иконка корзины
            'menu_position' => 5,
            'has_archive' => true,
            'supports' => array( 'title', 'editor', 'excerpt', 'thumbnail', 'comments'),
            'taxonomies' => array('post_tag')
        );
        register_post_type('product', $args);
    }

## Создаём новый тип записи
При создании своего типа записи мы имеем дело с функцией register_post_type(). Прикреплять эту функцию нужно к хуку init.

add_action('init', 'reviews_post_types');

    function reviews_post_types(){
        register_post_type('reviews', [
            'labels' => [
                'name'               => 'Отзывы', // основное название для типа записи
                'singular_name'      => 'Отзыв', // название для одной записи этого типа
                'add_new'            => 'Добавить новый', // для добавления новой записи
                'add_new_item'       => 'Добавление отзыва', // заголовка у вновь создаваемой записи в админ-панели.
                'edit_item'          => 'Редактирование отзыва', // для редактирования типа записи
                'new_item'           => 'Новый отзыв', // текст новой записи
                'view_item'          => 'Смотреть отзыв', // для просмотра записи этого типа.
                'search_items'       => 'Искать отзывы', // для поиска по этим типам записи
                'not_found'          => 'Не найдено', // если в результате поиска ничего не было найдено
                'not_found_in_trash' => 'Не найдено в корзине', // если не было найдено в корзине
                'parent_item_colon'  => '', // для родителей (у древовидных типов)
                'menu_name'          => 'Отзывы', // название меню
            ],
            'public'              => true,
            'menu_position'       => 25,
            'menu_icon'           => 'dashicons-format-quote',
            'hierarchical'        => false,
            'supports'            => array('title', 'editor', 'thumbnail'), // 'title', 'editor', 'author', 'thumbnail', 'excerpt', 'trackbacks', 'custom-fields', 'comments', 'revisions', 'page-attributes', 'post-formats'
        ]);
    }

    function test_show_reviews(){
        $args = array(
            'orderby'     => 'date',
            'order'       => 'DESC',
            'post_type'   => 'reviews'
        );

        return get_posts($args);
    }

## Создаём слайдер
В файле functions.php, записываем:

    function slider_front() {
        $labels = array(
            'name' => 'Слайдер на главной',
            'singular_name' => 'Фото главного слайдера',
            'add_new' => 'Добавить новое',
            'add_new_item' => 'Добавить новое фото',
            'edit_item' => 'Редактировать фото',
            'new_item' => 'Новое фото',
            'view_item' => 'Посмотреть фото',
            'search_items' => 'Найти фото',
            'not_found' =>  'Фото не найдено',
            'not_found_in_trash' => 'В корзине фото не найдено',
            'parent_item_colon' => '',
            'menu_name' => 'Фронт слайдер'
        );
        $args = array(
            'labels' => $labels,
            'public' => true,
            'publicly_queryable' => true,
            'show_ui' => true,
            'show_in_menu' => true,
            'query_var' => true,
            'rewrite' => true,
            'capability_type' => 'post',
            'has_archive' => true,
            'hierarchical' => false,
            'menu_position' => null,
            'supports' => array('title','editor','thumbnail')
        );
        register_post_type('myTheme_slider_front', $args);
    }
    add_action('init', 'slider_front');

## Разное
* https://wp-kama.ru/id_5851/zapisi-v-wordpress.html
* https://wp-kama.ru/function/register_post_type
* https://wp-kama.ru/id_8218/taksonomii-v-wordpress.html
* https://wp-kama.ru/id_8902/metadannye-v-wordpress.html
* **Плагин:** https://ru.wordpress.org/plugins/custom-post-type-ui/