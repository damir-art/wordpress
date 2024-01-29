# Search in WordPress
 https://wp-kama.ru/function/get_search_form

- `get_search_form()` - функция выводящая форму в файл шаблоне, например в *header.php*
- `searchform.php` - подключаемый файл-шаблон хранящий пользовательскую форму, данный файл подгрузится в месте вызова функции *get_search_form()*, если этого файла нет, то подгрузится стандартная форма WordPress
- `search.php` - файл-шаблон, выводящий результат поиска
- `get_search_query()` - получает поисковый запрос, безопасная от XSS атак
- `виджет сайдбара` - поисковую форму можно создать еще с помощью виджета

## Поисковая форма `get_search_form()`
Чтобы вывести поисковую форму в файл шаблоне, например `header.php`, нужно разместить код:

    get_search_form()

Данный код выведет форму из файла `searchform.php`, если данного файла нет то, выведется стандартная форма поиска WordPress.

Код стандартной формы поиска:

    <form role="search" method="get" id="searchform" class="searchform" action="http://site.name/">
      <div>
        <label class="screen-reader-text" for="s">Найти:</label>
        <input type="text" value="" name="s" id="s">
        <input type="submit" id="searchsubmit" value="Поиск">
      </div>
    </form>

При создании своей формы нужно указать:
- в теге `form`
  - `method="get"` - метод передачи данных get
  - `id="searchform"` - id равен searchform, для сторонних плагинов
  - `action="http://site.name/"` - action равен URL сайта `<?php echo home_url('/'); ?>` со слешем на конце
- в теге текстового/поискового поля `input`
  - `type="text"` или `type="search"` (если в теме указана поддержка HTML5)
  - `name="s"` имя со значением s
  - `id="s"` - id равен s, для сторонних плагинов
- в теге кнопки `input`, `button`
  - `type="submit"`

## Дополнительно
Копируем в файл search.php всё из файла index.php, добавляем форму поиска.

В цикле под endwhile создаем строку, а под ней текст который будет выводится, при отсутствии результатов поиска. Добавить заголовок "Результаты поиска" и т.п.

Создать форму поиска, можно одним из двух способов:
- Прописать код самостоятельно
- Вывести форму поиска с помощью виджета в сайдбаре

Прописываем код самостоятельно:

    <form method="get" action="<?php echo home_url('/') ?>">
    	<input type="text" name="s" value="Поиск" />
	    <input type="submit" />
    </form>

Значение `s` в атрибуте name, тега input, говорит о том что поиск произвобидтся по базе WordPress.

    <?php endwhile; ?>
    <?php else: ?>
        <p>
            Результаты поиска отсутствуют.
        </p>
    <?php endif; ?>
