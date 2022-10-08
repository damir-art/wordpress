# get_posts()

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
