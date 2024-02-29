# single.php - файл-шаблон 
Создание файл-шаблона single.php, темизация WordPress.

Чтобы создать файл-шаблон `single.php`, нужно скопировать туда весь код из `index.php` и удалить/изменить в цикле:

- меняем цитату на the_content()
- удаляем the_permalink()
- удаляем the_posts_pagination()
- добавляем ссылки на соседние записи
- удаляем всё не нужное, по своему усмотрению, например `else`

Подробнее:

    the_content(параметры) - вывод контента текущей записи
    previous_post_link(параметры) - ссылка на предыдущий пост
    next_post_link(параметры) - ссылка на следующий пост

Вёрстка пагинации:

    /* Previous Next */
    .previous-next {
      display: flex;
      justify-content: space-between;
      font-size: 14px;
      font-weight: 500;
      margin-top: 8px;
    }

    <div class="previous-next">
      <div>
        <?php previous_post_link('Предыдущая: %link'); ?>
      </div>
      <div>
        <?php next_post_link('Следующая: %link'); ?>
      </div>
    </div>

## Разное
По идее (хотя и не рекомендуется), в постах (single.php) и страницах (page.php) проверку if и цикл while можно убрать и оставить:

    <?php the_post(); ?>
    <?php the_post_thumbnail('large') ?>
    <h1><?php the_title() ?></h1>
    <?php the_content() ?>

## Вёрстка

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
      background-color: rgba(189, 195, 199, 0.5);
      border-radius: 8px;
      padding: 16px;
    }
    .loop__img {
      display: flex;
    }
    .loop__img img {
      border-radius: 8px;
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
    .loop__title h1 {
      margin: 0;
      color: #34495e;
      font-size: 40px;
    }
    .loop__excerpt p {
      margin: 0;
      margin-top: 8px;
    }

    /* Pagination */
    .pagination .nav-links {
      margin-top: 16px;
      display: flex;
      gap: 4px;
    }
    .pagination .page-numbers {
      color: #34495e;
    }

    /* Previous Next */
    .previous-next {
      display: flex;
      justify-content: space-between;
      font-size: 14px;
      font-weight: 500;
      margin-top: 8px;
    }

HTML:

    <?php get_header(); ?>

    <section class="section section--index">
      <div class="section__container">

      <?php if ( have_posts() ): ?>
        <div class="loop">
          <?php while ( have_posts() ) : the_post(); ?>
            <div class="loop__item">
              <div class="loop__wrap">
                <div class="loop__title">
                  <h1><?php the_title(); ?></h1>
                </div>
                <div class="loop__excerpt">
                  <?php the_content(); ?>
                </div>
              </div>
            </div> <!-- loop__item -->
          <?php endwhile; ?>
        </div> <!-- loop -->
        <div class="previous-next">
          <div>
            <?php previous_post_link('Предыдущая: %link'); ?>
          </div>
          <div>
            <?php next_post_link('Следующая: %link'); ?>
          </div>
        </div>
      <?php endif; ?>

      </div> <!-- section__container -->
    </section> <!-- section_index -->

    <?php get_footer(); ?>
