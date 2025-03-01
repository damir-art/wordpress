# archive.php
Выводит список анонсов постов, архивная страница наподобие index.php  
Шаблон для показа таксономий: category, tag, пользовательских и их терминов  

```html
<div class="row">
  <div class="col">
    <div class="title">
      <h1><?php the_archive_title(); ?></h1>
      <p>
        <?php the_archive_description(); ?>
      </p>
    </div> <!-- title -->
  </div> <!-- col -->
</div> <!-- row -->
```

`the_archive_title()` - заголовок архива  
`the_archive_description()` - описание архива  
`single_cat_title()`

```html
<div class="row">
  <div class="col">
    <div class="title">
      <?php if(is_archive()): ?>
        <a id="card">
          <h2>
            <?php
              single_cat_title(); // Вывод названия категории / метки
            ?>
          </h2>
        </a>
        <p>
          <?php
            $cat_ID = get_queried_object_id(); // Получаем ID текущей категории / метки
            echo category_description($cat_ID); // Выводим описание категории / метки
          ?>
        </p>
      <?php endif; ?>
    </div> <!-- title -->
  </div> <!-- col -->
</div> <!-- row -->
```

Список функций, связанных с `archive.php`:

**is_archive()** - проверяет, находится ли пользователь на странице архива (категории, метки, даты, автора и т.п.).

```php
if ( is_archive() ) {
  // Действия на странице архива
}
```

**get_the_archive_title()** - возвращает заголовок архива (название категории, метки, автора и т.д.).

```php
echo '<h1>' . get_the_archive_title() . '</h1>';
```

**get_the_archive_description()** - возвращает описание архива, если оно установлено.

```php
if ( get_the_archive_description() ) {
  echo '<div class="archive-description">' . get_the_archive_description() . '</div>';
}
```

get_template_part() - позволяет загружать части шаблона (например, файлы с постами), что удобно при создании архивных страниц.

```php
get_template_part( 'content', 'archive' ); // Загружаем content-archive.php
```

wp_get_archives() - отображает список ссылок на архивы (по месяцам). Может использоваться для создания виджета или вставки списка архивов прямо в контент страницы.

```php
wp_get_archives( array(
  'type' => 'monthly',
  'limit' => 12,
) );
```

get_post_type_archive_title() - получение названия архива для конкретного типа записи.

```php
echo get_post_type_archive_title(); // Например, "Books Archive"
```

get_queried_object() - возвращает объект текущей запрашиваемой сущности (категория, метка, автор и т.д.). Полезен для получения дополнительной информации о текущем архиве.

```php
$queried_object = get_queried_object();
var_dump($queried_object); // Выведет массив данных о текущем объекте архива
```

single_term_title()` - выводит название термина (например, категории или метки).

```php
single_term_title(); // Название категории/метки
```

term_description() - выводит описание термина (если оно задано).

```php
term_description(); // Описание категории/метки
```

get_the_category_list() - возвращает список категорий через запятую для текущего поста.

```php
echo get_the_category_list(', '); // Список категорий поста
```

get_the_tag_list() - аналогично предыдущей функции, но возвращает список меток.

```php
echo get_the_tag_list('', ', ', ''); // Список меток поста
```

get_author_meta() - используется для получения информации об авторе (в случае архива авторов).

```php
echo get_author_meta('display_name'); // Имя автора
```

get_year_link(), get_month_link() и get_day_link() - используются для получения URL архива по году, месяцу или дню.

```php
echo get_year_link(2023); // Ссылка на архив за 2023 год
```

Этот набор функций поможет вам настраивать и адаптировать ваши архивные страницы под конкретные нужды без использования функционала постраничной навигации и цикла постов.
