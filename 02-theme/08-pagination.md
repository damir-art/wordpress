# Пагинация
Пагинация - постраничная навигация.

Цикл с постраничной навигацией:

    <?php if(have_posts()): ?>
      <?php while(have_posts()): the_post(); ?>
        <h2><?php the_title(); ?></h2>
        <?php the_excerpt(); ?>
      <?php endwhile; ?>
      <?php the_posts_pagination(); ?>
    <?php else: ?>
      <p>Записей нет.</p>
    <?php endif; ?>

Пагинация бывает двух видов:
- предыдущая, следующая
  - на архивных страницах `the_posts_navigation()`
  - на одиночных записях `the_post_navigation()`
- по номерам
  - только на архивных страницах `the_posts_pagination()`
  - с помощью параметров `paginate_links()` можно изменить HTML-код и стили постраничной навигации:
    - the_posts_pagination('type' => 'list' или 'array')

Может располагаться:
- на архивных страницах
- на одиночных записях

## Архивные страницы
**the_posts_`navigation()`** - выводит ссылки на следующую и предыдущую страницы постов. Используется на страницах архивов (index.php, рубрики, метки и т.п).

**the_posts_`pagination()`** - используется на страницах архивов (index.php, рубрики, метки и т.п), 1, 2, 3 и т.д.

Постраничную навигацию, размещают после цикла `endwhile` и перед `else`:

    <?php the_posts_pagination() ?>
    <?php the_posts_pagination(array('show_all' => true)) ?> // показать все ссылки на пагинацию

    <?php the_posts_navigation() ?>

- posts_nav_link() - вывод ссылок на следующую/предыдущую страницу с постами, после цикла

## Одиночные страницы
**the_`post`_navigation()** - выводит две ссылки на следующую и предыдущую посты. Объединяет в себе две функции next_post_link() и previous_post_link().

`get_the_post_navigation()` - получить HTML-код для обработки.

Наиболее удобный для кастомизации способ (не обрамляет весь текст ссылкой):
`next_post_link()` - вывод ссылки на следующую запись  
`previous_post_link()` - вывод ссылки на предыдущий пост

    <?php next_post_link( 'Следующая: %link' ); ?><br />
    <?php previous_post_link( 'Предыдущая: %link' ); ?>

Верстка:

    <div style="display: flex;justify-content: space-between;">
      <?php previous_post_link('Предыдущая: %link'); ?>
      <?php next_post_link('Следующая: %link'); ?>
    </div>

## Код постраничной навигации
Для index.php:

CSS:

    /* Loop */
    .loop {
      display: flex;
      flex-direction: column;
      gap: 24px;
    }
    .loop__item {
      display: flex;
      gap: 16px;
      background-color: rgba(255, 255, 255, 0.6);
      padding: 16px;
    }
    .loop__img {
      display: flex;
    }
    .loop__img img {
      border-radius: 8px;
    }
    .loop__wrap {
      display: flex;
      flex-direction: column;
    }
    .loop__title a {
      color: #34495e;
      text-decoration: none;
    }
    .loop__title h3 {
      margin: 0;
      color: #34495e;
      font-size: 28px;
    }
    .loop__excerpt p {
      margin: 0;
      margin-top: 8px;
    }
    .loop__category {
      margin-top: auto;
    }
    .loop__category a {
      color: #3498db;
      font-size: 14px;
    }

    /* Pagination */
    .pagination .nav-links {
      margin-top: 16px;
      display: flex;
      gap: 8px;
    }
    .pagination .page-numbers {
      color: #34495e;
    }

    .section--single .loop__item {
      flex-direction: column;
      gap: 16px;
    }

    /* Previous Next */
    .previous-next {
      display: flex;
      justify-content: space-between;
      font-size: 14px;
      margin-top: 8px;
    }

HTML + PHP:

    <?php get_header(); ?>

    <section class="section section--index">
      <div class="section__container">

    <?php if ( have_posts() ): ?>
      <div class="loop">
        <?php while ( have_posts() ) : the_post(); ?>
          <div class="loop__item">
            <div class="loop__img">
              <img src="" alt="" width="150" height="150" />
            </div> <!-- loop__img -->
            <div class="loop__wrap">
              <div class="loop__title">
                <a href="<?php the_permalink(); ?>">
                  <h3><?php the_title(); ?></h3>
                </a>
              </div> <!-- loop__title -->
              <div class="loop__excerpt">
                <?php the_excerpt(); ?>
              </div> <!-- loop__excerpt -->
              <div class="loop__category">
                <?php the_category(', '); ?>
              </div> <!-- loop__category -->
            </div> <!-- loop__wrap -->
          </div> <!-- loop__item -->
        <?php endwhile; ?>
      </div> <!-- loop -->
      <div class="pagination">
        <?php the_posts_pagination(); ?>
      </div>
    <? else : ?>
      <div class="loop">
        <div class="loop__item">
          <p>Записей нет.</p>
        </div> <!-- loop__item -->
      </div> <!-- loop -->
    <?php endif; ?>

      </div> <!-- section__container -->
    </section> <!-- section--index -->

    <?php get_footer(); ?>
