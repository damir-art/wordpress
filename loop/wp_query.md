# wp_query()

    // задаем нужные нам критерии выборки данных из БД
    $query = new WP_Query([
      'posts_per_page' => 5,
      'orderby'        => 'comment_count'
    ]);

    // Цикл
    global $post;

    if ( $query->have_posts() ) {
      while ( $query->have_posts() ) {
        $query->the_post();
        <?php the_title() ?>
      }
    } else {
      // Постов не найдено
    }

    wp_reset_postdata(); // Сбрасываем $post. Возвращаем оригинальные данные
