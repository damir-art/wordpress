# get_posts()
Десять популярных примеров использования `get_posts()` в WordPress:

## Отображение последних постов

```php
$args = array(
  'numberposts' => 5,
  'orderby'     => 'date',
  'order'       => 'DESC',
);
$posts = get_posts($args);

foreach ($posts as $post) {
  setup_postdata($post);
  the_title();
  the_content();
}
wp_reset_postdata();
```

## Фильтрация постов по категории

```php
$args = array(
  'numberposts'   => 10,
  'category_name' => 'news',
);
$posts = get_posts($args);

foreach ($posts as $post) {
  setup_postdata($post);
  the_title();
  the_content();
}
wp_reset_postdata();
```

## Получение постов определенного типа

```php
$args = array(
  'post_type'    => 'page',
  'numberposts'  => -1,
);
$pages = get_posts($args);

foreach ($pages as $page) {
  setup_postdata($page);
  the_title();
  the_content();
}
wp_reset_postdata();
```

## Отбор постов по метке

```php
$args = array(
  'numberposts' => 5,
  'tag'         => 'featured',
);
$posts = get_posts($args);

foreach ($posts as $post) {
  setup_postdata($post);
  the_title();
  the_content();
}
wp_reset_postdata();
```

## Сортировка постов по комментариям

```php
$args = array(
  'numberposts' => 10,
  'orderby'     => 'comment_count',
  'order'       => 'DESC',
);
$posts = get_posts($args);

foreach ($posts as $post) {
  setup_postdata($post);
  the_title();
  comments_number();
}
wp_reset_postdata();
```

## Получение постов с определенными метаданными

```php
$args = array(
  'numberposts' => 5,
  'meta_key'    => 'color',
  'meta_value'  => 'blue',
);
$posts = get_posts($args);

foreach ($posts as $post) {
  setup_postdata($post);
  the_title();
  the_content();
}
wp_reset_postdata();
```

## Отбор постов по диапазону дат

```php
$args = array(
  'numberposts' => 10,
  'date_query'  => array(
    array(
      'after'     => 'January 1st, 2020',
      'before'    => 'December 31st, 2020',
      'inclusive' => true,
    ),
  ),
);
$posts = get_posts($args);

foreach ($posts as $post) {
  setup_postdata($post);
  the_title();
  the_date();
}
wp_reset_postdata();
```

## Исключение определенных постов

```php
$args = array(
  'numberposts' => 10,
  'exclude'     => array(123, 456),
);
$posts = get_posts($args);

foreach ($posts as $post) {
  setup_postdata($post);
  the_title();
  the_content();
}
wp_reset_postdata();
```

## Выполнение поиска по ключевым словам

```php
$s = isset($_GET['s']) ? $_GET['s'] : '';

$args = array(
  'numberposts' => 10,
  's'           => $s,
);
$posts = get_posts($args);

foreach ($posts as $post) {
  setup_postdata($post);
  the_title();
  the_content();
}
wp_reset_postdata();
```

## Создание простого виджета для отображения последних постов

```php
class Recent_Posts_Widget extends WP_Widget {

  public function __construct() {
    parent::__construct(
      'recent_posts_widget',
      __('Recent Posts Widget', 'text_domain'),
      array('description' => __('A widget that displays recent posts.', 'text_domain'))
    );
  }

  public function widget($args, $instance) {
    $title = apply_filters('widget_title', $instance['title']);

    echo $args['before_widget'];
    if (!empty($title)) {
      echo $args['before_title'] . $title . $args['after_title'];
    }

    $args = array(
      'numberposts' => 5,
      'orderby'     => 'date',
      'order'       => 'DESC',
    );
    $posts = get_posts($args);

    if ($posts) {
      echo '<ul>';
      foreach ($posts as $post) {
        setup_postdata($post);
        echo '<li><a href="' . get_permalink($post->ID) . '">' . get_the_title($post->ID) . '</a></li>';
      }
      echo '</ul>';
    }

    echo $args['after_widget'];
  }

  public function form($instance) {
    $title = !empty($instance['title']) ? $instance['title'] : __('Recent Posts', 'text_domain');
    ?>
    <p>
      <label for="<?php echo $this->get_field_id('title'); ?>"><?php _e('Title:', 'text_domain'); ?></label>
      <input class="widefat" id="<?php echo $this->get_field_id('title'); ?>" name="<?php echo $this->get_field_name('title'); ?>" type="text" value="<?php echo esc_attr($title); ?>">
    </p>
    <?php
  }

  public function update($new_instance, $old_instance) {
    $instance = array();
    $instance['title'] = (!empty($new_instance['title'])) ? strip_tags($new_instance['title']) : '';
    return $instance;
  }
}

add_action('widgets_init', function () {
  register_widget('Recent_Posts_Widget');
});
```

Эти примеры демонстрируют разнообразные способы использования `get_posts()` для решения различных задач в WordPress. Ты можешь адаптировать их под свои потребности, изменяя параметры и добавляя новые условия.
