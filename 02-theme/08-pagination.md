# Пагинация
Пагинация - постраничная навигация.

Цикл с постраничной навигацией.

archive.php:

```php
<?php if(have_posts()): ?>
  <?php while(have_posts()): the_post(); ?>
    <h2><?php the_title(); ?></h2>
    <?php the_excerpt(); ?>
  <?php endwhile; ?>
  <?php the_posts_pagination(); ?>
<?php else: ?>
  <p>Записей нет.</p>
<?php endif; ?>
```

single.php:

```html
<div class="previous-next">
  <div class="previous-next__item">
    <?php previous_post_link('Предыдущая: %link'); ?>
  </div> <!-- previous-next__item -->
  <div class="previous-next__item">
    <?php next_post_link('Следующая: %link'); ?>
  </div> <!-- previous-next__item -->
</div> <!-- previous-next -->
```

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

## Настройка the_posts_pagination()
Функция `the_posts_pagination()` предоставляет возможность добавить CSS-классы и другие настройки. Эта функция является частью тем WordPress начиная с версии Twenty Seventeen и используется для создания стандартной пагинации.

Вот пример использования `the_posts_pagination()` с добавлением CSS-класса и другими настройками:

```php
<?php
$args = [
  'screen_reader_text' => __( 'Posts navigation' ), // Текст для экранных читателей
  'prev_text'          => __( 'Previous page' ),    // Текст для предыдущей страницы
  'next_text'          => __( 'Next page' ),        // Текст для следующей страницы
  'before_page_number' => '<span class="meta-nav screen-reader-text">' . __( 'Page' ) . ' </span>', // Текст перед номером страницы
  'class'              => 'custom-pagination-class', // Ваш собственный CSS-класс
];
?>

<div class="navigation">
  <?php the_posts_pagination( $args ); ?>
</div>
```

Параметры, которые вы можете передать в массив `$args`:

- `screen_reader_text` - текст для экранных читателей, который будет скрыт от обычных пользователей.
- `prev_text` - текст для ссылки на предыдущую страницу.
- `next_text` - текст для ссылки на следующую страницу.
- `before_page_number` - текст, который будет добавлен перед номером страницы.
- `class` - класс, который будет применен к контейнеру пагинации.

Вы можете добавить дополнительные классы или изменить существующие, а также настроить другие параметры, такие как тексты ссылок и т.п., в зависимости от ваших потребностей.

## Все настройки
Все настройки `the_posts_pagination()`. Давайте рассмотрим полный набор параметров, которые могут быть переданы в эту функцию:

```php
$args = [
  'screen_reader_text'     => __( 'Posts navigation' ),  // Текст для экранных читателей
  'prev_text'              => __( 'Previous page' ),     // Текст для предыдущей страницы
  'next_text'              => __( 'Next page' ),         // Текст для следующей страницы
  'before_page_number'     => '<span class="meta-nav screen-reader-text">' . __( 'Page' ) . ' </span>', // Текст перед номером страницы
  'aria_current'           => 'page',                    // Атрибут aria-current для текущей страницы
  'mid_size'               => 1,                         // Количество страниц, показываемых слева и справа от текущей
  'end_size'               => 0,                         // Количество страниц, показываемых в начале и конце
  'total'                  => $GLOBALS['wp_query']->max_num_pages, // Общее количество страниц
  'type'                   => 'array',                   // Формат вывода (array, plain, list)
  'class'                  => 'pagination',              // Класс контейнера пагинации
  'next_class'             => 'next',                    // Класс для ссылки на следующую страницу
  'prev_class'             => 'prev',                    // Класс для ссылки на предыдущую страницу
  'dots_class'             => 'dots',                    // Класс для точек между страницами
  'current_class'          => 'current',                 // Класс для текущей страницы
  'disabled_class'         => 'disabled',                // Класс для неактивной ссылки
  'before_output'          => null,                      // Код, который будет выведен перед пагинацией
  'after_output'           => null,                      // Код, который будет выведен после пагинации
  'item_spacing'           => 'preserve',                // Пробелы между элементами (preserve, discard)
  'aria_label'             => null,                      // Атрибут aria-label для ссылок
  'mode'                   => 'array',                   // Режим работы (array, links)
  'extra'                  => [],                        // Дополнительные аргументы
];
```

Разберем каждый параметр:

- `screen_reader_text` - текст, который будет показан пользователям с ограниченными возможностями при использовании программ чтения экрана. Обычно этот текст скрыт от обычного просмотра.
- `prev_text` - текст, который будет использоваться для ссылки на предыдущую страницу.
- `next_text` - текст, который будет использоваться для ссылки на следующую страницу.
- `before_page_number` - текст, который будет добавлен перед номером каждой страницы.
- `aria_current` - значение атрибута `aria-current` для активной страницы. Обычно устанавливается в `"page"` для обозначения текущей страницы.
- `mid_size` - количество страниц, которое будет показано слева и справа от текущей страницы.
- `end_size` - количество страниц, которое будет показано в начале и конце списка страниц.
- `total` - общее количество страниц, доступное для навигации.
- `type` - тип вывода. Может принимать значения `"array"`, `"plain"` или `"list"`.
- `class` - основной класс контейнера пагинации.
- `next_class`, `prev_class`, `dots_class`, `current_class`, `disabled_class` - классы для различных элементов пагинации: следующая страница, предыдущая страница, точки между страницами, активная страница и неактивная ссылка соответственно.
- `before_output`, `after_output` - HTML-код, который будет вставлен перед и после блока пагинации.
- `item_spacing` - определяет, нужно ли сохранять пробелы между элементами (`"preserve"`) или нет (`"discard"`).
- `aria_label` - атрибут `aria-label` для ссылок.
- `mode` - режим работы функции. Может принимать значения `"array"` или `"links"`.
- `extra` - массив дополнительных аргументов, которые будут переданы в функцию.

Этот расширенный список параметров дает вам еще больше контроля над внешним видом и поведением вашей пагинации. Вы можете комбинировать их в соответствии с вашими потребностями, чтобы создать идеальную навигационную систему для вашего сайта.

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
