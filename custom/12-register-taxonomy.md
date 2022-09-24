# Пользовательские таксономии
Создание пользовательских таксономий.
Таксономия - это то, как записи разбиваются на разные категории или метки.
Пост (post) - нативный тип записи, имеет таксономию: рубрики и метки.
У каждого типа записи должна быть своя таксономия, так можно быстрее, удобнее и правильней формиравать запросы к базе данных. 

- register_taxonomy()

Код:

    function am_register_taxonomy() {
        register_taxonomy(
            'brand',
            'product',
            array(
                'label' => 'Brand',
                // 'rewrite' => array( 'slug' => 'brand' )
            )
        );
        register_taxonomy(
            'brand_category',
            'product',
            array(
                'label' => 'Brand category',
                // 'rewrite' => array( 'slug' => 'brand-category' )
            )
        );
    }
    add_action( 'init', 'am_register_taxonomy' );

Пример создания таксономии:

    add_action( 'init', 'foo' );
    function foo() {
        register_taxonomy( 'taxonomy_name', [ 'post_name' ], [ 
            'key' => 'value',
            ...
        ]);
    }

- `taxonomy_name`   - название таксономии `id`, например `tax_news_tag`
- `[ 'post_name' ]` - массив с перечислением типов записей у которых будет эта таксономия, например `[ 'news' ]`

Таксономии бывают:
- Древовидными: категории, рубрики
- Линейными (плоскими): метки, теги

Свойства таксономии:
- Древовидная или линейная
- К какому типу записи прикрепляется
- Чпу
- Остальные свойства

Причины создания таксономий:
- Быстрота разрабоки (масштабируемость)
- Чистота кода
- Производительность сайта

Создадим две таксономии для каталога недвижимости: города, квартиры.

- регистрируем новый тип записи
- обновляем ЧПУ: админка > настройки > постоянные ссылки > "сохранить изменения"
- чтобы выводить список "пользовательских записей", нужно добавить в функцию создания типа записи, строку "has_archive => true,"
- обновляем ЧПУ: админка > настройки > постоянные ссылки > "сохранить изменения"
- создаем файл-шаблон archive-имя_типа.php
- создаем файл-шаблон single-имя_типа.php
- создаем таксономию
- присваиваем типам записей теги/категории новой таксономии
- обновляем ЧПУ: админка > настройки > постоянные ссылки > "сохранить изменения"
- смотрим получившиеся урлы
- get_the_terms($post->ID, 'tax_name') вывод таксономии в цикле

Функция создания таксономии вешается на хук init, поэтому её можно разместить внутри пользовательской функции создания типа записи.

## Пример сокращённого кода
Пример сокращённого рабочего кода

    register_taxonomy( 'tax_name', [ 'имя_типа_записи' ], [
        'labels'                => [
            'name'              => 'Города',
            'singular_name'     => 'Город',
            'search_items'      => 'Найти город',
            'all_items'         => 'Все города',
            'view_item '        => 'Посмотреть город',
            'edit_item'         => 'Редактировать город',
            'update_item'       => 'Обновить город',
            'add_new_item'      => 'Добавить новый город',
            'new_item_name'     => 'Добавить новый',
            'menu_name'         => 'Города',
        ],
        'description'           => '', // описание таксономии
        'public'                => true,
        'hierarchical'          => true, // false
    ]);

- `city/moscow`
- `city` - таксономия, аналог category
- `moscow` - имя категории/тега
- `'hierarchical' => true` даже если таксономия плоская, её делают иерархической чтобы термины было удобно заполнять в админке 

## Пример полного кода

    // Регистрация таксономий
    add_action( 'init', 'po_news_taxonomy' );
    function po_news_taxonomy(){
        register_taxonomy( 'tax_news_tag', [ 'news' ], [
            'label'                 => '', // определяется параметром $labels->name
            'labels'                => [
                'name'              => 'Метки новостей',
                'singular_name'     => 'Метка новости',
                'search_items'      => 'Найти метку новость',
                'all_items'         => 'Все метки новостей',
                'view_item '        => 'Посомтреть метку новости',
                // 'parent_item'       => 'Parent News',
                // 'parent_item_colon' => 'Parent News:',
                'edit_item'         => 'Редактировать метку новости',
                'update_item'       => 'Обновить метку',
                'add_new_item'      => 'Добавить метку',
                'new_item_name'     => 'New News Name',
                'menu_name'         => 'Метка новости',
            ],
            'description'           => 'Таксономия новостей, плоская', // описание таксономии
            'public'                => true,
            // 'publicly_queryable'    => null, // равен аргументу public
            // 'show_in_nav_menus'     => true, // равен аргументу public
            // 'show_ui'               => true, // равен аргументу public
            // 'show_in_menu'          => true, // равен аргументу show_ui
            // 'show_tagcloud'         => true, // равен аргументу show_ui
            // 'show_in_quick_edit'    => null, // равен аргументу show_ui
            'hierarchical'          => false,

            'rewrite'               => true,
            //'query_var'             => $taxonomy, // название параметра запроса
            'capabilities'          => array(),
            'meta_box_cb'           => null,  // html метабокса. callback: `post_categories_meta_box` или `post_tags_meta_box`. false — метабокс отключен.
            'show_admin_column'     => false, // авто-создание колонки таксы в таблице ассоциированного типа записи. (с версии 3.5)
            'show_in_rest'          => null,  // добавить в REST API
            'rest_base'             => null,  // $taxonomy
            // '_builtin'              => false,
            //'update_count_callback' => '_update_post_term_count',
        ] );
    }

## Ссылки
* https://wp-kama.ru/handbook/codex/taxonomies
* https://wp-kama.ru/function/register_taxonomy
* **Плагин:** https://ru.wordpress.org/plugins/custom-post-type-ui/
