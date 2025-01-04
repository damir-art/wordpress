# Цикл (Loop) темизация WordPress

Создаём страницу `front-page.php`, переносим туда весь код из `index.php`, в `index.php` удаляем весь код и вставляем цикл (см. внизу страницы). Создаём 5 статей, проверяем верстку и пагинацию. Ставим плагин хлебных крошек.

## Theme Unit Test
Прежде чем работать с циклом, нужно заполнить сайт тестовыми данными, для этого можно воспользоваться `Theme Unit Test`:
- переходим по адресу: https://codex.wordpress.org/Theme_Unit_Test
- в начале страницы ищем ссылку: https://raw.githubusercontent.com/WPTT/theme-unit-test/master/themeunittestdata.wordpress.xml
- кликаем по ней правой кнопкой
- сохранить объект как...
- скачается файл `themeunittestdata.wordpress.xml`

Теперь нам нужно установить плагин `WordPress Importer` и импортировать скачанный файл:
- в админке переходим в `Инструменты > Импорт`
- жмём по ссылке `WordPress > Установить`
- жмём по ссылке `Запустить импорт`
- жмём по кнопке `Обзор`
- выбираем файл `themeunittestdata.wordpress.xml`
- жмём по кнопке `Загрузить и импортировать файл`
- `Импорт автора` выбираем админ в двух местах
- ставим галочку `Скачать и импортировать файлы вложений`
- жмем по кнопке отправить

В итоге у нас появятся следующие данные:
- тестовые рубрики
- тестовые записи
- комментарии и т.д.

## Что такое цикл?
Шпаргалка по циклу WordPress: https://wp-kama.ru/handbook/cheatsheet  
Что такео цикл WordPress: https://wp-kama.ru/id_119/the-loop.html

Цикл WP организует вывод контента. Благодаря шаблонам WP понимает какой контент надо вывести.  
Рассмотрим функции цикла. Цикл находится внутри файла `index.php`, между шапкой и подвалом.  
`index.php` содержит основной цикл, который выводит на экран все существующие записи (посты) сайта.  
Количество постов, выводимых файлом `index.php`, зависит от настроек админки WordPress: Админка > Настройки > Чтение (по-стандарту это 10 постов), после которых идёт постраничная навигация.

## Схема цикла WordPress
Этот код не подходит, некуда вставлять постраничную навигацию (нормальный цикл с вёрсткой см в конце страницы):

```php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
  <!-- Цикл WordPress -->
  <p>Выводим данные записи. Здесь работают функции для цикла, например, the_title()</p>
  <h2><?php the_title() ?></h2>
<?php endwhile; else : ?>
  <p>Записей нет.</p>
<?php endif; ?>
```

Правильный цикл с пагинацией:

```php
<?php if(have_posts()): ?>

  <?php while(have_posts()): the_post(); ?>
    <p>Записи есть.</p>
  <?php endwhile; ?>

  <!-- Пагинация -->

  <?php else: ?>
  <p>Записей нет.</p>
<?php endif; ?>
```

Расположение функций постраничной навигации см на странице `08-pagination.md`

- have_posts() - возвращает true или false в зависимости от того существуют записи или нет,
- while ( have_posts() ) - выводим записи пока они есть,
- the_post() - объект текущей записи, устнавливает глобальную переменную $post, становятся доступны теги типа the_title() и т.п.

```html
<div class="row">
  <?php if(have_posts()): ?>

    <?php while(have_posts()): the_post(); ?>
      <div class="col col-md-3">
        <div class="loop">
          <!-- Посмотреть все теги анонса -->
          <?php remontkppzf_debug( $post ); ?>
          <?php remontkppzf_debug( $post->ID ); ?>
        </div>
      </div>
    <?php endwhile; ?>

  <?php else: ?>
    <div class="col">
      Записей нет.
    </div>
  <?php endif; ?>
</div> <!-- row -->
```

Полный код цикла:

```html
<div class="row flex-column gy-4 flex-md-row">
  <?php if(have_posts()): ?>
    <?php while(have_posts()): the_post(); ?>
      <div class="col d-flex justify-content-center col-md-6 col-lg-4 col-xl-3">
        <div class="card">
          <div class="card__image">
            <?php if ( has_post_thumbnail() ) : ?>
              <?php the_post_thumbnail( 'large', [ 'class' => 'img-fluid card-img-top' ] ); ?>
            <?php else : ?>
              <img class="img-fluid card-img-top" src="<?php echo get_template_directory_uri(); ?>/img/default-thumbnail.jpg" alt="Default Thumbnail">
            <?php endif; ?>
          </div> <!-- card__image -->
          <div class="card-body">
            <div class="card__title">
              <?php the_title( '<h5 class="card-title">', '</h5>' ); ?>
            </div> <!-- card__title -->
            <div class="card__excerpt">
              <?php the_excerpt(); ?>
            </div> <!-- card__excerpt -->
            <div class="card__permalink mt-2">
              <a href="<?php the_permalink(); ?>">Читать далее</a>
            </div> <!-- card__permalink -->
            <div class="card__category mt-2">
              Категории: <?php the_category(', '); ?>
            </div> <!-- card__category -->
            <div class="card__tags">
              <?php the_tags(); ?>
            </div> <!-- card__tags -->
            <div class="card__time">
              <?php the_time('Дата: d.m.Y, Время: H:i'); ?>
            </div> <!-- card__time -->
          </div> <!-- card-body -->
        </div> <!-- card -->
      </div>
    <?php endwhile; ?>
  <?php else: ?>
    <div class="col">
      Записей нет.
    </div>
  <?php endif; ?>
</div> <!-- row -->
```

## Функции-шаблона используемые внутри цикла
Функции (теги) используемые внутри цикла:
- `the_title()` - вывод заголовка поста,
- `the_content()` - вывод контента поста,
- `the_excerpt()` - вывод отрывка поста,
- `the_permalink()` - ссылка на текущий пост,
- `the_post_thumbnail()` - вывод миниатюры поста,
- `the_category(', ')` - вывод категории поста,
- `the_tags()` - вывод меток (тегов) поста,
- `the_date()` - вывод даты, работает 1 раз внутри шаблона, чтобы обойти нажно записачть через `get`,
- `the_time('j M Y')` - вывод даты и времени создания поста the_time('j F Y'),
- `edit_post_link()` - ссылка на редактирование, выведет ссылку "Редактировать",
- `get_edit_post_link()` - получаем ссылку на редактирование поста из публичной части.

У каждой функции-шаблона есть функция начинающая с `get_`, например `get_the_permalink()`, она не выводит а возвращает данные.

## the_content()
Выводит весь контент, можно ограничить через `<!-- more -->`.  
Можно в конце изменить ссылку на `Читать далее...`, в параметрах или убрать её `''`.

## the_excerpt()
the_excerpt() - отрывок поста, используется в файл-шаблоноах типа archive. Берет первые 55 слов текста поста, в конце устанавливает `[...]`. В админке можно самому заполнить блок цитаты. Можно использовать `echo get_the_excerpt()`.

С помощью хуков, можно:
- добавить ссылку `Далее` в конце цитаты
- изменить длину цитаты
- убрать или изменить `[...]` на `...`

https://wp-kama.ru/function/the_excerpt

## Подробнее о функциях цикла
Проверка на наличие постов:

```php
<?php if(have_posts()): ?>
  /* Проверяет найдены ли посты на текущей странице */
<?php endif; ?>
```

Цикл выводящий все имеющиеся посты блога:

```php
<?php while(have_posts()): the_post(); ?>
  /* Вывод всех записей */
<?php endwhile; ?>
```

## Работаем с цитатами в цикле
Работаем с цитатами (анонсами) в цикле WordPress
Цитата позволяет выводить отрывок поста, в цикле вывода записей. В конце цитаты появляется: `[...]`.

Чтобы иметь возможность выводить цитаты в цикле WordPress, нужно сделать следующее:

- Поставить галочку, возле слова **Цитата**, на странице создания записи
- Разместь в файле `index.php` функцию the_excerpt(), вместо the_content()

Чтобы манипулировать различными параметрами, например удалении знака `[...]`, количеством слов (при автоматическом выводе цитат) или размещении слов "читать далее" (в конце цитаты), нужно записать различные функции в файл functions.php

При создании поста, автор должен самостоятельно, вписывать цитату в специальное поле. Если же цитата не вписана, то она создастся автоматически и будет вмещать в себя первые 55 слов поста, при этом изображения и html-теги обрезаются.

Eсли вы не хотите использовать цитаты и вам нужно просто обрезать текст без сохранения html-тегов и прочего, то можно воспользоваться следующей строкой внутри цикла WordPress:

```html
<p>
  <?php echo mb_substr(strip_tags( get_the_content()), 0, 152 ); ?>...
</p>
```

где 152 - это количество вставляемых символов

## Разное
- https://wp-kama.ru/id_119/chto-takoe-tsikl-the-loop-v-wordpress.html - цикл WordPress
- https://wp-kama.ru/id_767/3-sposoba-postroeniya-tsiklov-v-wordpress.html - три способа создания цикла в WordPress

## Нормальный цикл с вёрсткой
Нормальный цикл с вёрсткой, файл `index.php`:

CSS:

```css
/* Loop */
.loop {
  display: flex;
  flex-direction: column;
  gap: 24px;
}
.loop__item {
  display: flex;
  gap: 16px;
  background-color: #bdc3c7;
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
.loop__excerpt p {
  margin: 0;
  margin-top: 8px;
}
```

HTML + PHP:

```html
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
    <? else : ?>
      <div class="loop">
        <div class="loop__item">
          <p>Записей нет.</p>
        </div> <!-- loop__item -->
      </div> <!-- loop -->
    <?php endif; ?>

  </div> <!-- section__container -->
</section> <!-- section_index -->

<?php get_footer(); ?>
```