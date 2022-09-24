# Пользовательские циклы
Содаём пользовательские циклы, получаем записи самостоятельно.

Функции циклов:
- get_post()
- get_posts() - используется в большинстве случаев, быстро и удобно
- WP_Query()
- query_posts()

Рабочий процесс:
- отбираем необходимые записи
- выводим необходимые записи

## get_post()
Функция которая получает одну запись по её `id`.
    
Шаблон:

    $post = get_post( $id [, $output, $filter] );

Пример, код пишем в WP-шаблоне:

    // Получаем запись
    $post = get_post(31);
    var_dump($post);

    // Выводим запись
    echo $post->post_title;
    echo $post->post_content;
    echo get_post_meta( $post->ID, 'price', true ); // Пользовательские записи, через ACF: get_field( 'price', $post->ID ) 
    // Пользовательские записи, через ACF: get_field( 'price', $post->ID ) или get_fields( $post->ID )[ 'price' ]

Если контент обрабатывают плагины и возникает ошибка то, можно написать следующий пост:

    echo apply_filters( 'the_content', $post->post_content );

## get_posts()
Функция которая получает несколько записей. Возвращает массив объектов.

Шаблон, массив с набором ключей:

    $posts = get_posts(
        array(
            'numberposts' => 5,         // Количество выводимых постов
            'category'    => 0,         // ID категории(й) откуда получаем записи 2, 4, 7
            'orderby'     => 'date',    // Сортировка по указанным полям
            'order'       => 'DESC',    // Как сортировать ASC/DESC
            'meta_key'    => '',        // Получаем записи по мета ключу
            'meta_value'  =>'',         // Получаем записи по мета значению
            'post_type'   => 'post',    // Тип записи откуда получаем записи, array('post','page')
        )
    );

    foreach( $posts as $post ){
        echo $post->$post_title;
    }

    wp_reset_postdata(); // Сброс

Чтобы работать без использования объектов со свойствами `$post->$post_title` в выводе, без использования `apply_filters()`, чтобы работать с шаблонами цикла `the_content()` и другими вне цикла WP, нужно внедрить функцию `setup_postdata()`:

    foreach( $posts as $post ) {
        setup_postdata( $post );
        the_title();
    }

    wp_reset_postdata(); // Сброс

## WP_Query()
WP_Query() позволяет создавать более сложные запросы чем **get_posts()**. Все ключи из get_posts() можно использовать в WP_Query(). get_posts() является обёрткой для WP_Query().

## query_posts()
Не использовать.

    <?php
    $posts_federici = get_posts(
        array(
            'numberposts' => 5,         // Количество выводимых постов
            'tag'    => 'federici',     // ID категории(й) откуда получаем записи 2, 4, 7
            'orderby'     => 'date',    // Сортировка по указанным полям
            'order'       => 'DESC',    // Как сортировать ASC/DESC
            'post_type'   => 'product', // Тип записи откуда получаем записи, array('post','page')
        )
    );

    echo var_dump($posts_federici);

    foreach( $posts_federici as $post ) {
        echo $post->$post_title;
    }

    wp_reset_postdata(); // Сброс
    ?>