# WP_Query()
Десять популярных примеров использования `WP_Query` в WordPress.

## Отображение последних постов

```php
$args = array(
  'post_type'      => 'post',
  'posts_per_page' => 5,
  'orderby'        => 'date',
  'order'          => 'DESC',
);
$query = new WP_Query($args);

if ($query->have_posts()) {
  while ($query->have_posts()) {
    $query->the_post();
    the_title();
    the_content();
  }
}
wp_reset_postdata();
```

## Фильтрация постов по категории

```php
$args = array(
  'post_type'      => 'post',
  'category_name'  => 'news',
  'posts_per_page' => 10,
);
$query = new WP_Query($args);

if ($query->have_posts()) {
  while ($query->have_posts()) {
    $query->the_post();
    the_title();
    the_content();
  }
}
wp_reset_postdata();
```

## Получение постов определенного типа

```php
$args = array(
  'post_type'      => 'page',
  'posts_per_page' => -1,
);
$query = new WP_Query($args);

if ($query->have_posts()) {
  while ($query->have_posts()) {
    $query->the_post();
    the_title();
    the_content();
  }
}
wp_reset_postdata();
```

## Отбор постов по метке

```php
$args = array(
  'post_type'      => 'post',
  'tag'            => 'featured',
  'posts_per_page' => 5,
);
$query = new WP_Query($args);

if ($query->have_posts()) {
  while ($query->have_posts()) {
    $query->the_post();
    the_title();
    the_content();
  }
}
wp_reset_postdata();
```

## Сортировка постов по комментариям

```php
$args = array(
  'post_type'      => 'post',
  'orderby'        => 'comment_count',
  'order'          => 'DESC',
  'posts_per_page' => 10,
);
$query = new WP_Query($args);

if ($query->have_posts()) {
  while ($query->have_posts()) {
    $query->the_post();
    the_title();
    comments_number();
  }
}
wp_reset_postdata();
```

## Получение постов с определенными метаданными

```php
$args = array(
  'post_type'      => 'post',
  'meta_key'       => 'color',
  'meta_value'     => 'blue',
  'posts_per_page' => 5,
);
$query = new WP_Query($args);

if ($query->have_posts()) {
  while ($query->have_posts()) {
    $query->the_post();
    the_title();
    the_content();
  }
}
wp_reset_postdata();
```

## Отбор постов по диапазону дат

```php
$args = array(
  'post_type'      => 'post',
  'date_query'     => array(
    array(
      'after'     => 'January 1st, 2020',
      'before'    => 'December 31st, 2020',
      'inclusive' => true,
    ),
  ),
  'posts_per_page' => 10,
);
$query = new WP_Query($args);

if ($query->have_posts()) {
  while ($query->have_posts()) {
    $query->the_post();
    the_title();
    the_date();
  }
}
wp_reset_postdata();
```

## Исключение определенных постов

```php
$args = array(
  'post_type'      => 'post',
  'post__not_in'   => array(123, 456),
  'posts_per_page' => 10,
);
$query = new WP_Query($args);

if ($query->have_posts()) {
  while ($query->have_posts()) {
    $query->the_post();
    the_title();
    the_content();
  }
}
wp_reset_postdata();
```

## Пагинация результатов

```php
$paged = (get_query_var('paged')) ? get_query_var('paged') : 1;

$args = array(
  'post_type'      => 'post',
  'posts_per_page' => 5,
  'paged'          => $paged,
);
$query = new WP_Query($args);

if ($query->have_posts()) {
  while ($query->have_posts()) {
    $query->the_post();
    the_title();
    the_content();
  }
  
  next_posts_link('Older Entries');
  previous_posts_link('Newer Entries');
}
wp_reset_postdata();
```

## Выполнение поиска по ключевым словам

```php
$s = isset($_GET['s']) ? $_GET['s'] : '';

$args = array(
  'post_type'      => 'post',
  's'              => $s,
  'posts_per_page' => 10,
);
$query = new WP_Query($args);

if ($query->have_posts()) {
  while ($query->have_posts()) {
    $query->the_post();
    the_title();
    the_content();
  }
}
wp_reset_postdata();
```

Эти примеры охватывают самые распространенные сценарии использования `WP_Query` в WordPress. Ты можешь адаптировать эти примеры под свои нужды, меняя параметры и добавляя новые условия.
