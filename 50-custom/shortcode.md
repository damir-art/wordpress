# Шорткод
Шорткод - это тот хук (экшн).<br />
Шорткод - короткий код.<br />
Шорткоды, позволяют вставлять PHP-код посредством слов, заключённых в квадратные скобки, пример: `[слово]`.

Схема создания шорткода:

    add_shortcode('имя шорткода', 'имя функции');

`add_shortcode()` - функция WP, которая свзывает шорткод и пользовательскую функцию

- в файл-шаблоне functions.php, записываем код
- данный код вставляем в пост, посредством `[шорткода]`
- шорткод - это псевдоним кода, расположенного в файле functions.php

## Создаём шорткод
В файл-шаблон functions.php запишем:

    function myPodpis() {
        return '<p>Привет!</p>';
    }
    add_shortcode('podpis', 'myPodpis');

В посте под текстом вставляем `[podpis]`

Данный код создаёт подпись к статье.
