# WP_Query()
- `WP_Query()` - Выбирает записи из базы данных по указанным критериям.

Выбираем тип записи `product`:

    <?php
    $query = new WP_Query(array(
        'post_type' => 'am_product',
        'post_per_page' => 2,
    ));

    while($query->have_posts()) : $query->the_post(); ?>

    <?php the_field('vendor-code'); ?>

    <div class="catalog__cards">
        <div class="catalog__card">
            <p class="catalog__code">
                Код: <span><?php the_field('vendor-code'); ?></span>
            </p>
            <?php $image = get_field('image'); ?>
            <img src="<?php echo $image['sizes']['thumb-amelia-164']; ?>" />
            <p class="catalog__card-title">Federici</p>
            <p class="catalog__card-subtitle">
                <?php the_title(); ?>
            </p>
            <p class="catalog__card-text">
                <?php the_content(); ?>
            </p>
        </div>
    </div>

    <?php endwhile; ?>

    <?php
    echo '<pre>';
    print_r($query);
    echo '</pre>';
    ?>

## get_posts()

    <?php
        // $brands = get_terms('am_product_brand');
        // var_dump($brands);
        
        $posts_federici = get_posts(
            array(
                'post_type' => 'am_product', // Тип записи откуда получаем записи, array('post','page')
                'numberposts' => 8,
                'tax_query' => array(
                    array(
                        'taxonomy' => 'am_product_brand',
                        'field'    => 'slug',
                        'terms'    => 'federici',
                    ),
                ),
            )
        );

        global $post;

        foreach( $posts_federici as $post ) {
            echo '<div class="catalog__card">';
            echo '<p class="catalog__code">Код: <span>' . the_field('vendor-code') . '</span></p>';
            $image = get_field('image');
            echo '<img src=' . $image["sizes"]["thumb-amelia-164"] . ' />';
            echo '<p class="catalog__card-title">Federici</p>';
            echo '<p class="catalog__card-subtitle">' . $post->post_title . '</p>';
            echo '<p class="catalog__card-text">' . $post->post_content . '</p>';
            echo '</div>';
        }

        // echo '<pre>';
        // print_r($posts_federici);
        // echo '</pre>';

        wp_reset_postdata(); // Сброс
    ?>