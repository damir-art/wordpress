# Авторизация
В REST API есть публичные и приватные конечные точки. Аутентификация в REST API выполняется во время запроса.

## Аутентификация по cookies
Авторизация по кукам - это стандартный метод аутентификации в WordPress.

## Встроенный Javascript API
Рекомендуемый способ. Не требует nonce-кода. https://wp-kama.ru/handbook/rest/basic/wp-api-js

## AJAX запросы
wp_create_nonce( 'wp_rest' )

## Пароли приложений
https://wp-kama.ru/handbook/rest/basic/authentication/application-passwords  
