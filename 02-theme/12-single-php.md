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