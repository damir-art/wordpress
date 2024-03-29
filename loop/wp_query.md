# wp_query()
- получение записей из БД
- вывод записей в цикле
- категории
- метки
- таксономии: работаем с таксономиями: https://wp-kama.ru/function/wp_tax_query

Глобальная переменная `$wp_query`:

    // получаем данные о текущей странице
    global $wp_query;
    echo '<pre>';
    print_r( $wp_query );
    echo '</pre>';

При создании запроса, учитывайте что он имеет предустановленные параметры (например в админке может быть прописано выводить по 5 записей в запросе): post_type, posts_per_page, orderby, order, post_status, offset.

`WP_Query` - это класс, на основе которого создается объект (запрос к БД), в свойстве объекта `$query->posts` находится массив объектов (выбранных из БД записей). `WP_Query` в БД работает с **таблицей wp_posts**.

`WP_Query` выбирает записи из базы данных по указанным критериям. На основе `WP_Query` работают функции `get_posts()` и `query_posts()` и все остальные запросы связанные с выбором записей из таблицы wp_posts.

Выбирать записи можно по разным критериям:
- по категории, метке, времени, автору
- свежие посты, популярные, рандомные
- по произвольным полям и многое другое

## Получение записей из БД
Получаем массив постов `$query->posts` принадлежащих категории с именем `Категория 1`

    $query = new WP_Query( [ 'category_name' => 'Категория 1' ] );
    echo '<pre>';
    print_r($query->posts); // Выводим посты
    echo '</pre>';

Переменная `$query` содержит объект с результатами запроса.

Получаем массив посто принадлежащих типу поста `films_films`:

    $query = new WP_Query(
      array(
        'post_type' => array('films_films'),
      )
    );
    films_debug($query->posts);

## Выводим посты в цикле
`have_posts()` проверяет есть ли посты, `the_post()` позволяет использовать стандартные функции цикла WordPress.

    while ( $query->have_posts() ) {
      $query->the_post();
      the_title(); // выводим заголовки постов
    }

Можно использовать стандартный цикл WordPress или через `foreach`:

    foreach($query->posts as $post) {
      // $query->the_post();
      // the_title();
      echo $post->post_title;
    }

Альтернативный синтаксис:

    <?php foreach ($query->posts as $post): ?>
    <?php echo $post->post_title; ?>
    <?php endforeach; ?>

Альтернативный синтаксис `while` (рекомендуется):

    <?php while ($query->have_posts()): $query->the_post(); ?>
    <?php the_title(); ?>
    <?php endwhile; ?>

Вывод таксономий пользовательского поста:

    <?php
      $cur_terms = get_the_terms( $post->ID, 'films_genre' );
      if( is_array( $cur_terms ) ) {
        foreach( $cur_terms as $cur_term ) {
          echo '<a href="'. get_term_link( $cur_term->term_id, $cur_term->taxonomy ) .'">'. $cur_term->name .'</a>'. '/';
        }
      }
    ?>

Вывод таксономий пользовательского поста, со слешем между таксономиями, при этом после последней таксономии слеша нет:

    <?php
      $cur_terms = get_the_terms( $post->ID, 'films_genre' );
      if( is_array( $cur_terms ) ) {
        $count = count($cur_terms);
        $i = 0;
        foreach( $cur_terms as $cur_term ) {
          $i++;
          $slash = '/';
          if($i == $count) $slash = '';
          echo '<a href="'. get_term_link( $cur_term->term_id, $cur_term->taxonomy ) .'">'. $cur_term->name .'</a>'. $slash;
        }
      }
    ?>

## Категории (рубрики)
- `cat` (число, строка, массив) - ID категории, можно указать несколько через запятую (в строке или массиве), если перед id поставить минус то она будет исключена из выборки вместе с дочерними категориями, чтобы дочерние не исключать используйте category__not_in
- `category_name` (строка) - имя категории, также можно использовать слаг (ярлык), можно записать несколько категорий через запятую
- `category__and` (массив) - получить посты состоящие в нескольких категориях
- `category__in` (массив) - получить посты входящие в одну из указанных категорий, дочерние категории не учитываются
- `category__not_in` (массив) - получить посты которые не входят в указанную категорию, можно указать несколько категорий, дочерние категории не учитываются

Пример:

   $query = new WP_Query(
      array (
        'cat' => array(41, 43), // id категории
        // 'category_name' => 'bez-rubriki', // имя категории или его слаг
      )
    );

## Метки
- `tag` (строка) - слаг метки, один или несколько, через запятую
  - 'tag' => 'metka-3, metka-5' - посты имеющие хотябы одну метку, `tag_in` быстрее
  - 'tag' => 'metka-3+metka-5' - посты имеющие сразу несколько меток, `tag_end` быстрее
- `tag_id` (число) - ID метки
- `tag_end` (массив) - посты имеющие несколько меток (ID)
- `tag_in` (массив) - посты имеющие хотябы одну метку (ID)
- `tag__not_in` (массив) - посты не относящиеся к указанным меткам (ID)
- `tag_slug__and` (массив) - тоже что и tag__and (slug)
- `tag_slug__in` (массив) - тоже что и tag__in (slug)

Пример:

    array (
      'tag_id' => 45, // ID метки
    )

## Таксономии
Вывод записей связанных с таксономией.
- `tax` (строка) - имя или ярлык (устарел)
- `tax_query` (массив) - массив, элементами которого являются другие массивы, в каждом из которых указывается таксономия

Примеры:

Получаем все элементы из таксономии `am_product_brand` и его термина `BMW`:

    $query = new WP_Query(
      array (
        'am_product_brand' => 'BMW',
      )
    );

Получаем все элементы из таксономии `am_product_brand` и его термина `BMW` и таксономии `am_product_category` и его термина `coupe`:

    $query = new WP_Query(
      array (
        'tax_query' => array (
          'relation' => 'AND',
          array (
            'taxonomy' => 'am_product_brand',
            'field'    => 'slug',
            'terms'    => array('BMW')
          ),
          array (
            'taxonomy' => 'am_product_category',
            'field'    => 'slug',
            'terms'    => array('coupe')
          )
        )
      )
    );

## Схема создания запроса

    // Задаем нужные нам критерии выборки данных из БД
    $args = array(
      'posts_per_page' => 5,
      'orderby' => 'comment_count'
    );

    $query = new WP_Query( $args );

    // Цикл
    if ( $query->have_posts() ) {
      while ( $query->have_posts() ) {
        $query->the_post();
        ?>
        <li><?php the_title() ?></li>
        <?php
      }
    }
    else {
      // Постов не найдено
    }

    // Возвращаем оригинальные данные поста. Сбрасываем $post.
    wp_reset_postdata();

Альтернативна схема:

    <?php
    // запрос
    $query = new WP_Query( $args ); ?>

    <?php if ( $query->have_posts() ) : ?>

      <!-- пагинация -->

      <!-- цикл -->
      <?php while ( $query->have_posts() ) : $query->the_post(); ?>
        <h2><?php the_title(); ?></h2>
      <?php endwhile; ?>
      <!-- конец цикла -->

      <!-- пагинация -->

      <?php wp_reset_postdata(); ?>

    <?php else : ?>
      <p><?php esc_html_e( 'Нет постов по вашим критериям.' ); ?></p>
    <?php endif; ?>

## Дополнительно
- на любой странице WordPress, всегда присутствует глобальная переменная `$wp_query`, хранящая в себе текущий запрос
- если в цикле используется `the_post()`, то обязательно после цикла нужно вызвать функцию `wp_reset_postdata()`
