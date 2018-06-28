### Организация кода стилей:

  > `normalize.scss`
  > > `style.scss`
  
```
global/
  variables.scss
  mixins.scss
  fonts.scss
```

### Стили для каждого блока в отдельном файле!

```
blocks/
  main-nav.scss
  news.scss
  page-footer.scss
  page-header.scss
  page-main.scss
  reviews.scss

style.scss
  @import "normalize.scss";
  @import "variables.scss";
  @import "mixins.scss";
  @import "global/fonts.scss";
  @import "blocks/page-header.scss";
  @import "blocks/main-nav.scss";
  @import "blocks/page-footer.scss";  
```

### порядок следования CSS свойств

  * > позиционирования - `position`, `top`, `bottom`, `left`, `right`
  * > блочной модели - `float`, `display`, `width`, `height`
  * > типографики
  * > оформления
  * > анимации

### Вложенные правила SCSS - Не создавайте большую вложенность!


```css
  nav {
    ul {
      margin: 0;
      padding: 0;
      list-style: none;
    }

    li {
      display: inline-block;
      a {
        display: block;
      }
    }
  }
```

#### CSS

```css
  nav ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  nav li {
    display: inline-block;
  }

  nav li a {
    display: block;
  }
```

# Вложенные родительские элементы SCSS

```scss
  .super-button {
      &-red { color: red; }
      &-blue { color: blue; }
  }

  .btn {
    &-primary{
        background-color: white;
          &-hover, &:hover{
            background-color: blue;
        }
    }
  }
```

#### CSS

```css
  .super-button-red {
    color: red;
  }
  .super-button-blue {
    color: blue;
  }

  .btn-primary {
    background-color: white;
  }
  .btn-primary-hover,
  .btn-primary:hover {
    background-color: blue;
  }
```

### Переменные SASS

```scss
  $font-stack: Helvetica, sans-serif;
  $primary-color: #333;

  body

    font: 100% $font-stack;
    color: $primary-color;
```

#### CSS

```css
  body {
    font: 100% Helvetica, sans-serif;
    color: #333;
  }
```

### Строки SCSS

```scss
  $image_dir: 'images/web/';
  $page: 12;

  .some {
    background-image: url( $image_dir + 'pen.gif' );

    &:before {
      content: "Страница #{ $page }!";
    }
  }
```

#### CSS

```css
  .some { background-image: url("images/web/pen.gif"); }
  .some:before { content: "Страница 12!"; }
```

### Примеси SCSS

```scss
  @mixin cards() {
    display: inline-block;
    vertical-align: top;
  }

  .news__item {
    @include cards;
    width: 200px;
  }

  .reviews__item {
    @include cards;
    width: 320px;
  }
```

#### CSS

```css
  .news__item {
    display: inline-block;
    vertical-align: top;
    width: 200px;
  }

  .reviews__item {
    display: inline-block;
    vertical-align: top;
    width: 320px;
  }
```

### Примесь с параметром SCSS

```scss
  @mixin size($width: 50px, $height: 50px) {
    width: $width;
    height: $height;
  }

  .square {
    @include size(100px, 100px);
  }

  .landscape {
    @include size(100px);
  }

  $width: 100px;
  $color-green: green;

  @mixin color ($color) {
    color: $color;
    background-color: $color;
  }

  .block {
    @include color($color-green);
  }

  @mixin size($width, $height: $width) {
    width: $width;
    height: $height;
  }

  .block {
    @include size($width, $height: $width);
  }

  @mixin offset($padding: $margin) {
    padding: $padding;
    margin: $margin;
  }

  .block {
    @include offset(5px; 10px);
  }
```

#### CSS

```css
  .square {
    width: 100px;
    height: 100px;
  }

  .landscape {
    width: 100px;
    height: 50px;
  }

  .block {
    color: green;
    background-color: green;
  }

  .block {
    width: 100px;
    height: 100px;
  }

  .block {
      padding: 5px;
      margin: 10px;
  }
```

### Примесь с параметром по умолчанию SCSS

```scss
  @mixin big($size: 100500px) {
    width: $size;
  }

  .block {
    @include big(10px);
  }

  .block {
    @include big();
  }
```

#### CSS

```css
  .block {
    width: 10px;
  }

  .block {
    width: 100500px;
  }
```

### цвета SASS

```scss
  $blue: #3bbfce; /* цвет */
  $margin: 16px; /* отступ */
  $fontSize: 14px; /* размер текста */

  .content {
    border: 1px solid $blue; /* синий бордюр */
    color: darken($blue, 20%); /* затемнение цвета на 20% */
  }

  .border {
    padding: $margin / 2;
    margin: $margin / 2;
    border-color: $blue;
    
  $blue: #3bbfce
  $margin: 16px
  $fontSize: 14px

  .content
    border: 1px solid $blue
    color: darken($blue, 20%)

  .border
    padding: $margin / 2
    margin: $margin / 2
    border-color: $blue
```

#### CSS

```css
  .content {
    border: 1px solid #3bbfce;
    color: #217882; 
  }

  .border {
    padding: 8px;
    margin: 8px;
    border-color: #3bbfce; 
  }
```

### Математические операции


В вычислениях допустимо использовать:

  > `+ сложение`
  `- вычитание`
  `/ деление`
  `* умножение`

При вычислении учитывайте, что разные единицы CSS несовместимы друг с другом!
Нельзя сложить 10% и 500px, можно только 10px и 500px,

В противном случае будет ошибка и компиляция CSS-файла не пройдёт.

```scss
  4em + 2 = 6em
  16px + 2cm = 91.59055px
  (16px + 2cm) / 2 = 45.79528px

  ceil((16px + 2cm) / 2) = 46px
  floor((16px + 2cm) / 2) = 45px
  round((16px + 2cm) / 2) = 46px
  
  4em / 2 = 4em / 2
  12px * 2 = 24px
```