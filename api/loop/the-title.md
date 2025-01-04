# the_title()
Функция `the_title()` в WordPress используется для вывода названия записи (поста, страницы и т.п.). Она автоматически получает и выводит название текущей записи, основанное на контексте, в котором вызывается.

## Описание функции the_title()
Функция `the_title()` имеет следующую базовую форму:

```php
the_title( $before = '', $after = '', $echo = true );
```

Где:
- `$before` (строка) — текст, который будет добавлен перед названием записи. По умолчанию пустая строка.
- `$after` (строка) — текст, который будет добавлен после названия записи. По умолчанию пустая строка.
- `$echo` (логическое значение) — определяет, нужно ли выводить название записи сразу (true) или вернуть его как строку (false). По умолчанию true.

## Примеры использования the_title()

Простое использование на архивной странице:

```php
if ( have_posts() ) :
  while ( have_posts() ) : the_post();
    the_title('<h2>', '</h2>');
  endwhile;
endif;
```

Добавление ссылок вокруг названия:

```php
the_title( sprintf( '<a href="%s" rel="bookmark">', esc_url( get_permalink() ) ), '</a>' );
```

Использование для вывода заголовков виджетов:

```php
echo '<h3 class="widget-title">';
the_title();
echo '</h3>';
```

Форматирование заголовка страницы:

```php
if ( is_single() || is_page() ) {
  the_title('<h1>', '</h1>');
} else {
  the_title('<h2><a href="' . get_permalink() . '">', '</a></h2>');
}
```

Возвращение названия записи как строки:

```php
$title = the_title('', '', false);
echo '<p>Название записи: ' . $title . '</p>';
```

Добавление специальных символов перед и после названия:

```php
the_title('&ldquo;', '&rdquo;');
```

Вывод названия записи с префиксом:

```php
the_title('Запись: ', '');
```

Вывод названия записи с суффиксом:

```php
the_title('', ' - Мой блог');
```

Применение фильтров к названию записи:

```php
function custom_title_format( $title ) {
  return ucfirst( $title );
}
add_filter( 'the_title', 'custom_title_format' );

the_title();
```

Использование в кастомном цикле:

```php
$args = array(
  'posts_per_page' => 5,
  'orderby'        => 'date',
  'order'          => 'DESC'
);

$recent_posts_query = new WP_Query( $args );

if ( $recent_posts_query->have_posts() ) {
  while ( $recent_posts_query->have_posts() ) {
    $recent_posts_query->the_post();
    the_title( '<h3>', '</h3>' );
  }
}
wp_reset_postdata();
```

## Заключение
Функция `the_title()` — мощный инструмент для управления и форматирования заголовков записей в WordPress. С ее помощью можно легко настраивать внешний вид заголовков, добавлять ссылки, стили и дополнительные элементы. Эти примеры демонстрируют лишь малую часть возможностей, которые предоставляет эта функция.
