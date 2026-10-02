# katrushenkov.github.io

Личный блог на [Hugo](https://gohugo.io) с темой
[hello-friend-ng](https://github.com/rhazdon/hugo-theme-hello-friend-ng)
(подключена git-сабмодулем в `themes/hello-friend-ng`).

Сайт: <https://katrushenkov.github.io/>

## Начало работы

```bash
git clone --recurse-submodules git@github.com:katrushenkov/katrushenkov.github.io.git
cd katrushenkov.github.io
```

Если репозиторий уже склонирован без `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

Нужен **Hugo extended** той же версии, что в CI — см. `HUGO_VERSION`
в `.github/workflows/deploy.yml`.

Стили темы компилируются **Dart Sass** — исполняемый файл `sass` должен быть
в `PATH` (версия — `DART_SASS_VERSION` в том же workflow). Например, на Arch:
`pacman -S dart-sass`, или скачать архив с
[релизов](https://github.com/sass/dart-sass/releases) и добавить в `PATH`.

## Локальный просмотр

```bash
hugo server        # http://localhost:1313, черновики скрыты
hugo server -D     # вместе с черновиками (draft: true)
```

## Новый пост

```bash
hugo new content posts/YYYYMMDD-slug.md   # раздел Posts
hugo new content blog/YYYYMMDD-slug.md    # раздел Blog
```

Шаблоны front matter лежат в `archetypes/`. URL поста повторяет путь файла:
`content/posts/linux/20210320-post.md` → `/posts/linux/20210320-post/`.

## Деплой

Пуш в `main` запускает GitHub Actions (`.github/workflows/deploy.yml`):
сборка `hugo --minify` и публикация в GitHub Pages. Ручной перезапуск —
вкладка Actions → «Deploy Hugo site to Pages» → Run workflow.

`public/` и `resources/_gen/` — результат сборки, в git не хранятся.

## Обновление темы

```bash
git submodule update --remote themes/hello-friend-ng
hugo server                       # проверить, что всё выглядит как надо
git add themes/hello-friend-ng && git commit -m "Update theme"
```

`layouts/partials/head.html` — копия партиала темы, отличающаяся одной строкой
(`"transpiler" "dartsass"` вместо `"libsass"`). При обновлении темы сверьте его
с оригиналом:

```bash
diff themes/hello-friend-ng/layouts/partials/head.html layouts/partials/head.html
```

## Шрифты

IBM Plex (Serif — текст статей, Sans — заголовки, даты и меню, Mono — код),
лицензия SIL OFL. Файлы лежат в `static/fonts/ibm-plex/` (только кириллица и
латиница), правила — в `static/css/fonts.css`, подключённом через
`params.customCSS` в `config.toml`. Он загружается после CSS темы и
перекрывает её шрифты, поэтому тему править не нужно.
