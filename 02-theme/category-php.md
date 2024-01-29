# category.php
Функции для файл-шаблона **category.php**

- single_cat_title( $prefix, $display ) - получаем имя категории/метки, используется за пределами цикла

Код цикла:

    <?php if ( have_posts() ): ?>
      <?php while( have_posts() ): the_post(); ?>
        <h2><?php the_title() ?></h2> 
      <?php endwhile; ?>
      <?php the_posts_pagination(); ?>
    <?php 
      else:
        get_template_part( 'tmp/no-posts' );
      endif;
    ?>

## Вывод рубрик (архивной записи?)
- объект рубрик
- цикл рубрик
- вывод свойств рубрик

Код:

    <?php
      // Получаем массив объектов, где каждый объект это категория
      $cats = get_categories();

      // Проверка массива на пустоту
      if ( $cats ):
    ?>

      <!-- Если массив не пустой, то выводим HTML-теги и категории. -->
      <ul>
        <?php 
          foreach( $cats as $cat ):
            $cat_link = get_category_link( $cat->cat_ID );
        ?>
        <a href="<?php echo $cat_link; ?>"><?php echo $cat->name; ?></a>
        <?php endforeach; ?>
      </ul>

    <?php
      endif;
    ?>
