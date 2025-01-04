# Contact Form 7
Конструктор форм для сайта.

```html
<!-- 
Создаём форму
Регистрируем сайдбар в functions.php
Вставляем туда виджет формы
Выводим сайдбар в шаблоне сайта
Вставляем в конфиг: define( 'WPCF7_AUTOP', false );
Или функтионс add_filter('wpcf7_autop_or_not', '__return_false');
-->

<div class="row g-4">
  <div class="col-12">
    <!-- <input type="text" class="form-control border-0 py-2" id="name" placeholder="Ваше имя"> -->
    [text* your-name id:name class:form-control class:border-0 class:py-2 placeholder "Ваше имя"]
  </div>
  <div class="col-12 col-xl-6">
    <!-- <input type="email" class="form-control border-0 py-2" id="email" placeholder="Email"> -->
    [text your-email id:email class:form-control class:border-0 class:py-2 placeholder "Email"]
  </div>
  <div class="col-12 col-xl-6">
    <!-- <input type="phone" class="form-control border-0 py-2" id="phone" placeholder="Телефон"> -->
    [tel* your-phone id:phone class:form-control class:border-0 class:py-2 placeholder "Телефон"]
  </div>
  <div class="col-12">
    <!-- <select class="form-select border-0 py-2" aria-label="Default select example">
      <option selected>Выберите услугу</option>
      <option value="1">Окна алюминиевые</option>
      <option value="2">Двери алюминиевые</option>
      <option value="3">Остекление домов \ коттеджей</option>
      <option value="4">Остекление террас \ Веранд</option>
      <option value="5">Остекление беседок</option>
    </select> -->
    [select your-select class:form-select class:border-0 class:py-2 "Окна алюминиевые" "Двери алюминиевые" "Остекление домов \ коттеджей" "Остекление террас \ Веранд" "Остекление беседок"]
  </div>
  <div class="col-12">
    <!-- <input class="form-control border-0 py-2" type="date"> -->
    [date your-date class:form-control class:border-0 class:py-2 placeholder "Выберите дату"]
  </div>
  <div class="col-12">
    <!-- <textarea class="form-control border-0 py-2" id="textarea" rows="3" placeholder="Комментарий"></textarea> -->
    [textarea your-message x3 id:textarea class:form-control class:border-0 class:py-2 placeholder "Комментарий"]
  </div>
  <div class="col-12">
    <!-- <button type="button" class="btn btn-primary w-100 py-2 px-5">Отправить заявку</button> -->
    [submit class:btn class:btn-primary class:w-100 class:py-2 class:px-5 "Отправить заявку"]
  </div>
</div>
```
