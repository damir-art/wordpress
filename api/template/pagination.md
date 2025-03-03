# API pagination
В WordPress для работы с постраничной навигацией (пагинацией) используются несколько функций. Вот наиболее популярные.

## paginate_links()
Эта функция используется для генерации ссылок на страницы постов или архивов. Она позволяет настроить внешний вид и поведение пагинации.

```php
$args = array(
  'base' => '%_%', // базовый URL для страниц
  'format' => '?page=%#%', // формат URL для страниц
  'total' => $wp_query->max_num_pages, // общее количество страниц
  'current' => max( 1, get_query_var('paged') ), // текущая страница
  'show_all' => false, // показывать все страницы?
  'end_size' => 1, // сколько ссылок отображать до и после текущей страницы
  'mid_size' => 2, // сколько ссылок отображать вокруг текущей страницы
  'prev_next' => true, // отображение кнопок "предыдущая" и "следующая"
  'prev_text' => __('« Предыдущая'), // текст кнопки "предыдущая"
  'next_text' => __('Следующая »'), // текст кнопки "следующая"
  'type' => 'plain', // формат вывода (plain, list, array)
);
echo paginate_links($args);
```

## get_pagenum_link()
Функция возвращает ссылку на определенную страницу поста или архива.

```php
// Получаем ссылку на вторую страницу
$link = get_pagenum_link(2);
echo '<a href="' . esc_url($link) . '">Страница 2</a>';
```

## previous_post_link() и next_post_link()
Эти функции генерируют ссылки на предыдущий и следующий посты соответственно.

```php
// Ссылка на предыдущий пост
previous_post_link('%link', 'Предыдущий пост');

// Ссылка на следующий пост
next_post_link('%link', 'Следующий пост');
```

## posts_nav_link()
Функция выводит ссылки на предыдущую и следующую страницы списка постов.

```php
// Выводим ссылки на предыдущие и следующие страницы
posts_nav_link();
```

## the_posts_pagination()
Эта функция является частью темы Twenty Seventeen и выше. Она выводит стандартную пагинацию для постов.

```php
// Выводим пагинацию
the_posts_pagination(array(
  'mid_size' => 2,
  'prev_text' => __('Previous'),
  'next_text' => __('Next'),
));
```

## wp_link_pages()
Используется для разбиения длинных постов на страницы внутри одного поста.

```php
// Разбиваем длинный пост на страницы
wp_link_pages(array(
  'before' => '<div class="page-links">' . __('Pages: '),
  'after' => '</div>',
  'link_before' => '<span>',
  'link_after' => '</span>',
));
```

Эти функции помогут вам легко реализовать различные виды пагинаций в вашем проекте на WordPress.
