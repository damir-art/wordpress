# the_post_thumbnail()
Функция `the_post_thumbnail()` в WordPress используется для вывода миниатюры текущего поста или страницы. Миниатюра поста задается автором в редакторе, и эта функция позволяет легко интегрировать ее в шаблон темы. По умолчанию, миниатюра отображается в полном размере, однако можно указать размер изображения, которое нужно вывести.

**Описание:** Выводит миниатюру текущего поста или страницы.

**Использование:**
```php
<?php the_post_thumbnail(); ?>
```

## Примеры использования

Простое использование в цикле:
```php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
  <h2><?php the_title(); ?></h2>
  <?php the_post_thumbnail(); ?>
  <?php the_content(); ?>
<?php endwhile; endif; ?>
```
Этот пример демонстрирует базовый цикл WordPress, где сначала выводится заголовок поста, затем миниатюра и, наконец, содержание поста.

Указание размера миниатюры:
```php
<?php the_post_thumbnail('medium'); ?>
```
Выводит миниатюру в среднем размере (по умолчанию 300x300 пикселей).

Вывод миниатюры с определенным классом:
```php
<?php the_post_thumbnail('large', ['class' => 'featured-image']); ?>
```
Выводит миниатюру большого размера с добавлением класса `featured-image`.

Проверка наличия миниатюры:
```php
<?php if ( has_post_thumbnail() ) : ?>
  <?php the_post_thumbnail('full'); ?>
<?php else : ?>
  <img src="<?php echo get_template_directory_uri(); ?>/images/default-thumbnail.jpg" alt="Default Thumbnail">
<?php endif; ?>
```
Проверяет наличие миниатюры у поста. Если миниатюра есть, то выводит ее в полном размере, иначе отображает изображение по умолчанию.

Использование миниатюры в виджете:
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
      echo '<a href="' . get_permalink($post["ID"]) . '">';
      echo get_the_post_thumbnail($post["ID"], 'thumbnail');
      echo $post["post_title"];
      echo '</a>';
    }
    echo $args['after_widget'];
  }
}
```
В этом примере создается виджет, который отображает последние записи вместе с их миниатюрами.

Вывод миниатюры с альтернативным текстом:
```php
<?php the_post_thumbnail('thumbnail', ['alt' => get_the_title()]); ?>
```
Выводит миниатюру в размере thumbnail с альтернативным текстом, равным названию поста.

Вывод миниатюры в модальном окне:
```html
<div id="modal-content">
  <?php the_post_thumbnail('large'); ?>
</div>
<script>
document.getElementById('open-modal').addEventListener('click', function() {
  document.getElementById('modal').style.display = 'block';
});
</script>
```
Открывает миниатюру в большом размере в модальном окне при нажатии на кнопку.

Вывод миниатюры с наложением текста:
```html
<div class="overlay">
  <?php the_post_thumbnail('medium'); ?>
  <div class="overlay-text">
    <?php the_title(); ?>
  </div>
</div>
```
Создает эффект наложенного текста поверх миниатюры.

Создание галереи миниатюр:
```php
<?php
$attachments = get_children(array(
  'post_parent' => get_the_ID(),
  'post_type' => 'attachment',
  'post_mime_type' => 'image'
));
foreach ($attachments as $attachment) {
  echo wp_get_attachment_image($attachment->ID, 'thumbnail');
}
?>
```
Получает все вложения изображений к посту и выводит их в виде галереи миниатюр.

Вывод миниатюры с эффектом наведения:
```html
<?php the_post_thumbnail('large', ['class' => 'hover-effect']); ?>
<style>
.hover-effect {
  transition: transform 0.3s ease-in-out;
}
.hover-effect:hover {
  transform: scale(1.05);
}
</style>
```
Добавляет эффект увеличения миниатюры при наведении курсора мыши.

---

Эти примеры демонстрируют разнообразные варианты использования функции `the_post_thumbnail()` в разработке тем и плагинов для WordPress.
