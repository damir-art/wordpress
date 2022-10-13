# get_posts()

    global $post;
    $products = get_posts(
      array(
        'post_type' => 'am_product',         // тип записи
        'posts_per_page' => -1,              // все записи
        'am_product_brand' => '$brand->slug' // слаг терма
        // 'tax_query' => array(
        //   array(
        //     'taxonomy' => 'am_product_brand',
        //     'field'    => 'slug',
        //     'terms'    => $brand->slug,
        //   ),
        // ),
      )
    );

    foreach($products as $product){
      echo 'Продукт:' . $product->ID . '<br />';
      // $terms = get_the_terms( $product->ID, 'am_product_category' );
      // echo '<pre>';
      // echo print_r($terms);
      // echo '</pre>';
    }

Дополнительно о 'tax_query' можно почитать тут: https://wp-kama.ru/function/wp_query#taxonomies

Пример 2

    global $post;

    $myposts = get_posts( 'numberposts=5&offset=1&category=1' );

    foreach( $myposts as $post ){
      setup_postdata( $post );
      ?>
      <!--
      здесь формирование вывода постов,
      где работают теги шаблона относящиеся к the loop, например, the_title()
      -->
      <?php
    }
    wp_reset_postdata();
