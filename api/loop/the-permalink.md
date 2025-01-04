# the_permalink()
Функция `the_permalink()` в WordPress используется для вывода URL-адреса текущего поста или страницы. Она обычно применяется внутри цикла (`The Loop`), чтобы получить ссылку на пост, который сейчас обрабатывается. Эта функция выводит результат сразу на экран, а не возвращает значение, поэтому она часто используется при генерации ссылок на посты в шаблонах тем.

**Описание:** Выводит постоянную ссылку (permalink) к текущему посту или странице.

**Использование:**
```php
<?php the_permalink(); ?>
```

## Примеры использования:

Простое использование в цикле:
```php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
  <a href="<?php the_permalink(); ?>"><?php the_title(); ?></a>
<?php endwhile; endif; ?>
```
Здесь ссылка будет вести на страницу поста, название которого выводится через функцию `the_title()`.

Использование с кастомным текстом ссылки:
```php
<a href="<?php the_permalink(); ?>" title="Читать дальше">Читать дальше</a>
```
В этом примере ссылка ведет на пост, но текст ссылки заменен на «Читать дальше».

Вывод ссылки на главную страницу:
```php
<a href="<?php echo get_home_url(); ?>">Главная страница</a>
```
Этот пример показывает, как использовать другую функцию `get_home_url()` для получения ссылки на главную страницу сайта.

Ссылка на архив категории:
```php
<a href="<?php echo get_category_link(get_the_category()[0]->term_id); ?>">
  <?php single_cat_title(); ?>
</a>
```
Здесь мы получаем ссылку на категорию текущего поста и используем её в качестве архива.

Создание списка категорий:
```html
<ul>
  <?php foreach (get_categories() as $category) { ?>
    <li>
      <a href="<?php echo get_category_link($category->term_id); ?>">
        <?php echo $category->name; ?>
      </a>
    </li>
  <?php } ?>
</ul>
```
Пример создания списка всех категорий блога с ссылками на их архивы.

Использование в виджете:
```php
class My_Widget extends WP_Widget {
  public function widget( $args, $instance ) {
    // Выводим заголовок виджета
    echo $args['before_widget'];
    if ( ! empty( $instance['title'] ) ) {
      echo $args['before_title'] . apply_filters( 'widget_title', $instance['title'] ). $args['after_title'];
    }
    // Получаем список последних записей
    $recent_posts = wp_get_recent_posts(array('numberposts' => 5));
    foreach ($recent_posts as $post) {
      echo '<a href="' . get_permalink($post["ID"]) . '">' . $post["post_title"] . '</a>';
    }
    echo $args['after_widget'];
  }
}
```
В данном примере мы создаем виджет, который отображает последние записи с использованием функции `wp_get_recent_posts` и `get_permalink`, которая является аналогом `the_permalink`, но возвращает значение вместо того, чтобы выводить его прямо на экран.

Перенаправление на страницу 404:
```php
if ( is_404() ) {
  header("Location: ".home_url()."/page-not-found");
  exit();
}
```
Этот код перенаправляет пользователей на специальную страницу 404, если они пытаются открыть несуществующую страницу.

Добавление кнопки «Поделиться»:
```php
<a href="https://twitter.com/share?url=<?php the_permalink(); ?>&amp;text=<?php the_title(); ?>" target="_blank" rel="noopener noreferrer">Поделиться в Twitter</a>
```
Создает кнопку для публикации текущей статьи в Twitter.

Создание ссылки на следующий пост:
```php
$next_post = get_next_post();
if (!empty($next_post)) {
  echo '<a href="' . get_permalink($next_post->ID) . '">Следующий пост</a>';
}
```
Использует функцию `get_next_post` для получения следующего поста и создания ссылки на него.

Отображение количества комментариев:
```html
<a href="<?php the_permalink(); ?>#comments">
  Комментарии (<?php comments_number('0', '1', '%'); ?>)
</a>
```
Отображает количество комментариев к посту и создает ссылку на них.

---

Эти примеры показывают различные способы применения функций работы со ссылками в WordPress, включая `the_permalink()`.
