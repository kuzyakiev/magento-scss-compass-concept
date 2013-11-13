Original: https://docs.google.com/document/d/1v9X6gCWawDk0sBbq2l-EOT7OrOm9G5JcV1KKYcp_4ew/edit#

Задача: обновить все css файлы и спрайты при работе с git.
Мы подразумеваем, что compass у вас уже установлен.

Настраиваем config.rb:
```compass
http_path = "/"
css_dir = "skin/frontend/package/theme/compiled_css"
sass_dir = "skin/frontend/package/theme/sass"
images_dir = "skin/frontend/package/theme/images"
generated_images_dir = "skin/frontend/package/theme/images/compiled_sprites"
sprite_load_path = "skin/frontend/package/theme/images/sprites"
```

Добавляем в .gitignore папки, которые автоматически генерируют css и sprites, а так же папку:
```
#SCSS & CSS
.sass-cache
skin/frontend/package/theme/compiled_css
skin/frontend/package/theme/images/compiled_sprites
```
Добавляем в .git/hooks/post-checkout и .git/hooks/post-merge :
```ruby
SASSDIR=skin/frontend/package/theme/sass
COMPILED_CSS_DIR=skin/frontend/package/theme/compiled_css
COMPILED_SPRITES_DIR=skin/frontend/package/theme/images/compiled_sprites
if [ ! -d "$COMPILED_CSS_DIR" ]; then
	mkdir $COMPILED_CSS_DIR
fi
if [ ! -d "$COMPILED_SPRITES_DIR" ]; then
	mkdir $COMPILED_SPRITES_DIR
fi
compass compile .
```
В итоге при git checkout или git merge мы обрабатываем все sass файлы и генерируем все css и спрайты в соответствующие папки.

Проверим работу, создав main.scss в соответствующей папке с таким содержанием:
```sass
$font-stack:    'PT Sans', Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
$social-sprite-layout: smart;
$social-sprite-dimensions: true;
@import "sprites/social/*.png";
@include all-social-sprites;

$icons-sprite-layout: smart;
$icons-sprite-dimensions: true;
@import "sprites/icons/*.png";
@include all-icons-sprites;
```
Запускаем и радуемся успеху :)
