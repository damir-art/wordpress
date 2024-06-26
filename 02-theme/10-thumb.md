# Миниатюра поста
Работаем с миниатюрами поста. При создании поста можно задать миниатюру поста. При сохранении изображения, WordPress создает несколько одних и тех же изображений но с разными размерами (указываются в `Админка > Настройки > Медиафайлы`).

Прежде чем работать с миниатюрами постов, включите их поддержку в файле `functions.php`:

    add_theme_support('post-thumbnails'); // обычно вешают на хук after_setup_theme

Отключение поддержки миниатюр не означает отключение их показа, это означает что в новые записи вы не сможете добавить миниатюру, но в старых записях миниатюры сохранятся хоть вы их и не сможете удалить или поменять.

Функции для работы с миниатюрами:
- `the_post_thumbnail()` - вывод миниатюры поста, используется внутри цикла
- `get_the_post_thumbnail()` - можно сохранить миниатюру в переменную
- `get_the_post_thumbnail_url()` - возвращает ссылку на полное изображение миниатюры поста
- `thumbnail`, `medium`, `large`, `full`, `post-thumbnail` - миниатюры WordPress по-умолчанию
  - `thumbnail` - 150x150px, миниатюра (thumb вроде 150 ширина, высота пропорционально)
  - `medium` - 300x300px, средний размер
  - `large` - 1024x1024px, крупный размер
  - `full` - полный размер
  - `post-thumbnail` - размер который устанавливается для темы через функцию `set_post_thumbnail_size()`, если он не установлен, то загрузится `full`
  - array(32, 32) - можно указать свою высоту и ширину или только ширину array(200)
- `add_image_size()` - в файле functions.php можно задать свои размеры для изображений

## the_post_thumbnail()
Пример, выведем миниатюру с минимальными размерами:

    <?php
      the_post_thumbnail('thumbnail');
    ?>

Пример с параметрами, выведем миниатюру среднюю и зададим ей класс:

    the_post_thumbnail( 'medium', array('class' => 'img-fluid') )

## add_image_size()
Создаём собственные размеры миниатюр.

    add_image_size('my_thumb', 300, 9999); // 300 по ширине, без ограничений по высоте

crop:
- `false` - по-умолчанию, не обрезается масштабируется по одной из указанных сторон,
- `true` - обрезается точно по указанным размерам,
- `array('right', 'top')` - кадрировать с правого верхнего угла.

## Разное
- миниатюра прикрепляется к записи, а не помещается в её контент,
- все изображения загружаеммые в WordPress, сохраняются в `wp-content/uploads`,
- загрузив одно изображение, WordPress из него делает 5, одно исходное и 4 еще размера: 150, 300, 768, 1024,
- если эти размеры вам не нужны, то поставьте `0` в `Админка > Настройки > Медиафайлы`,
- обычно еще проверяют, есть ли у поста миниатюра и если её нет то ставят миниатюру заглушку.

## Основные функции
- the_post_thumbnail()     - выводит HTML код картинки-миниатюры текущего поста,
- get_post_thumbnail_id()  - получает ID миниатюры записи,
- has_post_thumbnail()     - имеет ли запись картинку-миниатюру (условный тег),
- the_post_thumbnail_url() - выводит URL миниатюры записи,
- the_attachment_link()    - выводит ссылку (тег a) вложения или страницы вложения,
- get_attachment_link()    - получает URL на страницу вложения (на фронте),
- wp_get_attachment_link() - получает ссылку (тег a) вложения или страницы вложения.

Пример, если миниатюры нет, то выводим дефолтное изображение:

    if(has_post_thumbnail()) {
      the_post_thumbnail( 'thumbnail' );
    } else {
      echo '<img src="' . get_template_directory_uri() . '/img/thumb-150x150.png" width="150" height="150" />';
    }

Пример, если код выше нужно разместить в нескольких файлах шаблонах, то его можно поместить в функцию, чтобы не повторяться (пишем в functions.php):

    /**
    * Вывод миниатюр, используем в файл-шаблонах архивов
    */
    function legioner_thumb( $id, $size = 'medium', $class = 'img-fluid' ) {
      $html = '<div class="thumb-post mb-2">';
        if(has_post_thumbnail()) {
          $html .= get_the_post_thumbnail( $id, $size, array( 'class' => $class ) );
        } else {
          $html .= '<img class="img-fluid" src="https://picsum.photos/id/0/600/400" width="600" height="400" />';
        }
      $html .= '</div>';
      return $html;
    }

    // Размещаем в файл шаблоне
    echo legioner_thumb(get_the_ID());

## CSS стили
Стандартные стили для миниатюр:

    img {
      max-width: 100%;
      display: block;
    }
    .card-thumb img {
      object-fit: cover;
      width: 100%;
      height: 200px;
    }

## Дефолтный код

### CSS

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
    .loop__title h1 {
      margin: 0;
      color: #34495e;
      font-size: 40px;
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

### HTML

    <section class="section section--index">
      <div class="section__container">

    <?php if ( have_posts() ): ?>
      <div class="loop">
        <?php while ( have_posts() ) : the_post(); ?>
          <div class="loop__item">
            <div class="loop__img">
              <?php
                if(has_post_thumbnail()) {
                  the_post_thumbnail( 'medium' );
                } else {
                  echo '<img src="' . get_template_directory_uri() . '/img/thumb-600x450.png" width="600" height="450" />';
                }
              ?>
            </div> <!-- loop__img -->
            <div class="loop__wrap">
              <div class="loop__title">
                <h1><?php the_title(); ?></h1>
              </div> <!-- loop__title -->
              <div class="loop__content">
                <?php the_content(); ?>
              </div> <!-- loop__content -->
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
