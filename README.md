# Инструкция по переходу на новую тему оформления

Новая тема реализована на основе фреймворка Semantic-UI.
[Статья на официальном сайте фреймворка](https://semantic-ui.com/usage/theming.html)

## Настройка импорта стилей

1. Прописать в `bower.json` пакет `ember-flexberry-themes`
2. Проверить, что в `ember-cli-build.js` прописаны пути до пакетов с темами (`semantic-ui` и `ember-flexberry-themes`)

```js
  lessOptions: {
    paths: [
      'bower_components/semantic-ui',
      'bower_components/ember-flexberry-themes',
    ]
  }
```

3. Настроить файл `theme.config` (можно скопировать из [примера](src/theme.config.example))

`@semanticUiThemesFolder` - путь до папки с темой `semantic-ui`
`@emberFlexberryThemesFolder` - путь до папки с темой `ember-flexberry-themes`
`@siteFolder` - папака с локальными стилями приложения

4. Настроить файл `app/styles/theme.less` (можно скопировать из [примера](src/theme.less))

Прописать в `app/styles/app.less` импорт стилей (помимо импорта локальных стилей должна остаться одна строка, `semantic` импортировать не нужно)

```less
  @import 'src/flexberry-imports';
```

## Настройка шрифтов

1. Скопировать шрифты в папку `vendor/fonts` из папки `assets/fonts` ([src/themes/ghost/assets/fonts](src/themes/ghost/assets/fonts))
2. Скопировать в папку `vendor` `.css` с объявлением стиилей из папки `assets` ([src/themes/ghost/assets](src/themes/ghost/assets))
3. Добавить импорт шрифтов `GOSTUI2`, `guideline-icons` в `ember-cli-build.js`
4. Добавить импорт стилей и настроек для иконок и шрифтов в `ember-cli-build.js`

```js
  app.import('vendor/fonts.css');

  // GOSTUI2
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w170-regular_g_temp.eot', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w170-regular_g_temp.ttf', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w170-regular_g_temp.woff', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w170-regular_g_temp.woff2', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w450-medium_g_temp.eot', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w450-medium_g_temp.ttf', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w450-medium_g_temp.woff', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w450-medium_g_temp.woff2', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w706-bold_g_temp.eot', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w706-bold_g_temp.ttf', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w706-bold_g_temp.woff', { destDir: 'assets/fonts' });
  app.import('vendor/fonts/GOSTUI2/GOSTUI2-w706-bold_g_temp.woff2', { destDir: 'assets/fonts' });

  // guideline-icons
  app.import('vendor/guideline-icons.css');
  app.import('vendor/fonts/guideline-icons/guideline-icons.eot', { destDir: 'assets/fonts/guideline-icons' });
  app.import('vendor/fonts/guideline-icons/guideline-icons.ttf', { destDir: 'assets/fonts/guideline-icons' });
  app.import('vendor/fonts/guideline-icons/guideline-icons.woff', { destDir: 'assets/fonts/guideline-icons' });
  app.import('vendor/fonts/guideline-icons/guideline-icons.woff2', { destDir: 'assets/fonts/guideline-icons' });
  app.import('vendor/fonts/guideline-icons/guideline-icons.svg', { destDir: 'assets/fonts/guideline-icons' });
```

## Настройка приложения


Установить пакет `autoprefixer` и добавить настройку в `ember-cli-build.js`

```js
  const autoprefixer = require('autoprefixer');
  module.exports = function(defaults) {
    let app = new EmberAddon(defaults, {
      ...
      postcssOptions: {
        compile: {
          enabled: false,
          browsers: ['last 3 versions'],
        },
        filter: {
          enabled: true,
          plugins: [
            {
              module: autoprefixer,
              options: {
                browsers: ['last 2 versions']
              }
            }
          ]
        }
      }
    });
    ...
  }
```
### Основное меню приложения

1. Чтобы отображать новое меню нужно использовать компонент `flexberry-sitemap-guideline`. [Пример на стенде](https://github.com/Flexberry/ember-flexberry/blob/feature-new-theme/tests/dummy/app/templates/application.hbs#L15).
2. Чтобы отображать элементы в подвале сайдбара, их нужно поместить в блок с классом `sitebar-footer`. [Пример на стенде](https://github.com/Flexberry/ember-flexberry/blob/feature-new-theme/tests/dummy/app/templates/application.hbs#L16)
2. Для установки иконок в пункты меню при объявлении `sitemap'а` добавить для узла параметр `icon`

```js
  sitemap: computed('i18n.locale', function() {
    let i18n = this.get('i18n');

    return {
      nodes: [{
        link: 'index',
        caption: i18n.t('forms.application.sitemap.index.caption'),
        title: i18n.t('forms.application.sitemap.index.title'),
        icon: 'home',
        children: null
      }, {
        link: null,
        caption: i18n.t('forms.application.sitemap.application.caption'),
        title: i18n.t('forms.application.sitemap.application.title'),
        icon: 'clock outline',
        ...
```

3. toggleSidebar скопировать [отсюда](https://github.com/Flexberry/ember-flexberry/blob/feature-new-theme/tests/dummy/app/controllers/application.js#L43) 

### Модальные окна в режиме `Sidepage`

В новой теме добавлен дополнительный режим открытия модального окна `sidepage`.
В данном режиме модальное окно открывается справа во всю высоту страницы а на мобильных устройствах распахивается на весь экран.
Чтобы модальное окно открылось в режиме `sidepage`, необходимо в разметку добавить класс `flexberry-sidepage`, а также использовать анимацию `transition:'slide left'`.

При использовании компоеннта `modal-dialog` достаточно указать `useSidePageMode=true`.

Для того, чтобы модальные окна `lookup`'a и настройки столбцов открывались в режиме `sidepage`, необходимо добавить следующие настройки в environment.js:

```js
components: {
  ...
  // For guideline theme
  // Settings for flexberry-objectlistview component.
  flexberryObjectlistview: {
    // Flag indicates whether to side page or usually mode.
    useSidePageMode: true,
  },

  // Settings for flexberry-lookup component.
  flexberryLookup: {
    // Flag: indicates whether to side page or usually mode.
    useSidePageMode: true,
  }
  ...
}
```
### Некоторые классы
Чтобы модальное окно распахивалось на мобильнольном устройстве на весь экран необходимо использовать класс `fullhight-mobile-modal`. 

## Доработка стилей в приложении

1. Добавить папку `site` в app/styles
2. Стили рассортированы по компонентам ember-flexberry
3. Файл `*.variables` для переменных, `*.overrides` для добавления новых стилей.
4. В первую очередь нужно использовать переменные для изменения стилей.

### Структура каталога

```
  app
  ├── ...
  ├── styles
  |   ├── site
  |   |   ├── components
  |   |   |   ├── flexberry-button.variables/.overrides
  |   |   |   ├── flexberry-checkbox.variables/.overrides
  |   |   |   ├── flexberry-colsconfig.variables/.overrides
  |   |   |   ├── flexberry-dropdown.variables/.overrides
  |   |   |   ├── flexberry-field.variables/.overrides
  |   |   |   ├── flexberry-file.variables/.overrides
  |   |   |   ├── flexberry-groupedit.variables/.overrides
  |   |   |   ├── flexberry-lookup.variables/.overrides
  |   |   |   ├── flexberry-modal.variables/.overrides
  |   |   |   ├── flexberry-objectlistview.variables/.overrides
  |   |   |   ├── flexberry-sidebar.variables/.overrides
  |   |   |   ├── flexberry-simpledatetime.variables/.overrides
  |   |   |   └── flexberry-validationsummary.variables/.overrides
  |   |   ├── globals
  |   |   |   └── site.variables/.overrides
  |   |   └── pages
  |   |   |   ├── login-form.variables/.overrides
  |   |   |   └── main.variables/.overrides
  |   └── app.less
  └──...
```

В теме используется цветовая схема, таким образом, если поменять основной цвет глобально, то он поменяется для кнопок и чекбокса и тд.
Цветовая схема задается в `globals/site.variables`

```less
  /*******************************
            COLOR SCHEME
  *******************************/

  // Main
  @defaultColor                   : #ECF2FB;
  @primaryColor                   : @cobaltBlue;
  @activeColor                    : @mayaBlue;
  @accentColor                    : #E94B3D;
  @secondaryColor                 : #7699B3;
  @disabledColor                  : @lightGrayishBlue;

  // Sidebar
  @sidebarBackgroundColor         : @textColor;

  // Page
  @simplePageBackground           : @defaultColor;
  @textColor                      : @blueZodiak;
  @iconColor                      : @lightGrayishBlue;

  // Input
  @inputBorderColor               : @defaultColor;
  @inputBackground                : @defaultColor;
  @inputHoverBorderColor          : @chambray;

  @defaultInputFocusBackground    : @white;
  @defaultFocusBorderColor        : @activeColor;
  @focusedFormBorderColor         : @activeColor;
```
### Список основных цветов

| Свойство          | Описание                                               | Дефолтное значение                                                                     |
|-------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------|
| `@defaultColor`   | Основной цвет (заливка полей, фон, кнопки по умолчанию)| ![#ECF2FB](https://placehold.it/15/ECF2FB/000000?text=+) `#ECF2FB`                     |
| `@primaryColor`   | Основной цвет (кнопки с классом `primary`, чекбоксы)   | @cobaltBlue: ![#0C49CD](https://placehold.it/15/0C49CD/000000?text=+) `#0C49CD`        |
| `@accentColor`    | Акцентный цвет (акцентные элементы управления)         | ![#E94B3D](https://placehold.it/15/E94B3D/000000?text=+) `#E94B3D`                     |
| `@secondaryColor` | Вспомогательный цвет (кнопка secondary, ссылки)        | ![#7699B3](https://placehold.it/15/7699B3/000000?text=+) `#7699B3`                     |
| `@textColor`      | Цвет текста                                            | @blueZodiak: ![#3B4256](https://placehold.it/15/3B4256/000000?text=+) `#3B4256`        |
| `@iconColor`      | Иконки в полях                                         | @lightGrayishBlue: ![#848E99](https://placehold.it/15/848E99/000000?text=+) `#848E99`  |
| `@disabledColor`  | Недоступные элементы управления                        | @lightGrayishBlue: ![#848E99](https://placehold.it/15/848E99/000000?text=+) `#848E99`  |

### Интерактивные

| Свойство          | Описание                                               | Дефолтное значение                                                                     |
|-------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------|
| `@activeColor`    | Активный элемент (focus)                               | @mayaBlue:   ![#62B0FF](https://placehold.it/15/62B0FF/000000?text=+) `#62B0FF`        |
| `@negativeColor`  | Ошибка (error)                                         | @cinnabar:   ![#E53935](https://placehold.it/15/E53935/000000?text=+) `#E53935`        |

### Список цветов полей на форме

Применяется ко всем компонентам для ввода данных
`flexberry-field`, `flexberry-dropdown`, `flexberry-lookup` и тд.

| Свойство                       | Описание                              | Дефолтное значение                                                                |
|--------------------------------|---------------------------------------|-----------------------------------------------------------------------------------|
| `@inputBorderColor`            | Цвет бордера поля                     | @defaultColor: ![#ECF2FB](https://placehold.it/15/ECF2FB/000000?text=+) `#ECF2FB` |
| `@inputHoverBorderColor`       | Цвет бордера поля при наведении       | @chambray:     ![#B3BBC3](https://placehold.it/15/B3BBC3/000000?text=+) `#B3BBC3` |
| `@inputBackground`             | Цвет заливки поля                     | @defaultColor: ![#ECF2FB](https://placehold.it/15/ECF2FB/000000?text=+) `#ECF2FB` |
| `@defaultInputFocusBackground` | Цвет заливки поля, когда оно в фокусе | @white:        ![#FFFFFF](https://placehold.it/15/FFFFFF/000000?text=+) `#FFFFFF` |
| `@focusedFormBorderColor`      | Цвет бордера поля, когда оно в фокусе | @activeColor:  ![#62B0FF](https://placehold.it/15/62B0FF/000000?text=+) `#62B0FF` |

