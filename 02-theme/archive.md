# archive.php
Выводит список анонсов постов, архивная страница наподобие index.php  
Шаблон для показа таксономий: category, tag, пользовательских и их терминов  

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
