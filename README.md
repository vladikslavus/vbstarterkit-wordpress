## My web development environment for Wordpress CMS (any version)
This is my desktop starter kit for Wordpress front-end developing.
I use Webpack with Babel inside Gulp via webpack-stream plugin here.
Open `package.json` to read packages I use.

## Wordpress
All your Wordpress project code is in the `public` folder.

## Environment requirements
The following tools must be installed to create the environment:
- [Node.js](https://nodejs.org/en/)
- [Git](https://git-scm.com/)
- [Gulp-cli](https://gulpjs.com/docs/en/getting-started/quick-start/) (need to globally install only gulp-cli)
- [Open Server Panel 6.0](https://ospanel.io/)

If you do not have these tools, you need to install them.

## Project dependencies installation
To install the project dependencies, enter the command at the command line:
- `npm install`

Akso need:
- nvm use 14.18.1
- OpenServer (any) to start browser-sync with proxy: 'russian-box-ltd-lo' and the same directory

## How to use the environment
Normal mode: enter `gulp` at the command line.
Selective build: enter the task you need at the command line. For example, enter `gulp css_build` to build CSS or `gulp js_build` to build JS. Other available tasks can be found in gulpfile.js.

--------------------------------------------------

It's Time to Learn Russian ;)

Чеклист по созданию нового проекта на Wordpress

Создаём папку по имени домена, клонируем и инсталлим сборку, в ней в папке public/ распаковываем WP, настраиваем .osp/project.ini и инсталлим WP, предварительно создав в phpMyAdmin базу и пользователя (такие же как на удалённом сервере).

Чистим админку и приводим всё в порядок:
1) Страницы -> выделяем все и удаляем
2) Плагины -> выделяем все и удаляем
3) Заходим на сайт https://underscores.me/ и создаём пустую тему project-name.zip
4) Внешний вид -> Темы -> Добавить -> Загрузить тему (project-name.zip) -> Установить сейчас -> Активировать
5) Заходим в папку с темами C:\OSPanel\home\grain-fiber\public\wp-content\themes\ и удаляем все лишние темы. Не трогаем файл index.php (// Silence is golden.).
6) Создаём в админке главную страницу для того, чтобы она не бралась по умолчанию из файла index.php
   a) Страницы -> создаём новую "Главная страница"
   б) Настройки -> Чтение -> Статическую страницу (выбераем эту созданную Главную страницу)
   в) Создаём к корне нашей темы файл для нашей Главной страницы, назовём его page-main.php
   г) Открываем файл page-main.php и прописываем на самом верху в комментариях название шаблона:
      <?php
      /*
      Template Name: Шаблон "Главная страница"
      */
      ?>
   д) Цепляем этот файл-шаблон в админке к странице "Главная страница"
7) Прописываем в файле page-main.php <?php get_header(); ?> и <?php get_footer(); ?>
8) Редактируем header.php
   - Заменяем теги нашей вёрстки на системные wordpress-овские, например <html <?php language_attributes(); ?>> и так прочие (1:03)
   - Помещаем перед закрывающим тегом </head> функцию <?php wp_head(); ?>, а также прописать <body <?php body_class(); ?>><?php wp_body_open(); ?> (1:05)
   - Удаляем польностью тег <title>. WP сам сгенерит его.
9) Редактируем footer.php (1:06:30). Не забываем поместить функцию <<?php wp_footer(); ?> перед закрывающим тегом </body>
10) Чистим файл style.css в корне нашей темы, чтобы старые стили не затирали стили нашей темв (см. след. пункт), когда мы запускаем функцию <?php wp_head(); ?>
10) Переносим папки файлы нашей вёрстки css, fonts, img и js в корень темы (1:10:40)
11) Прописываем:
   - в header.php <link rel="stylesheet" href="<?=get_template_directory_uri();?>/css/main.min.css">, а также к тегам <img /> (1:14)
   - в footer.php <script src="<?=get_template_directory_uri();?>/js/main.min.js"></script> (1:14)
     (этот фрагмент кода с main.min.js не подключается в админке, поэтому если на сайте используется jquery, то импортируем его на этапе сборки из node_modules)
   - путь к fonts/ прописан в CSS в _common.scss


P.S. Если не запускается Gulp, то https://www.youtube.com/watch?v=vObwhyh5h5I
Если при выполнении команды в Windows PowerShell у вас возникла ошибка с таким текстом, это значит что в вашей системе закрыт доступ к выполнению сценариев PowerShell политикой безопасности. Чтобы открыть доступ необходимо выполнить следующую команду: Set-ExecutionPolicy Unrestricted -Scope CurrentUser