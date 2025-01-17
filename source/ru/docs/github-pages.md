---
title: GitHub Pages
---

В этом туториале мы используем [Travis CI](https://travis-ci.com/) для деплоя в Github Pages. Travis CI бесплатен для репозиториев с открытым исходным кодом, то есть ветка `master` вашего репозитория должна быть публичной. Пожалуйста, перейдите в описание [Приватного репозитория](#Private-repository), если вы предпочитаете не открывать свой исходный код, либо откажитесь от загрузки своих файлов на GitHub.

1. Создайте репозиторий с названием <b>*username*.github.io</b>, где `username` — ваше имя пользователя GitHub. Если вы уже загрузили файлы в репозиторий с другим названием, просто переименуйте его.
2. Запушьте файлы вашей папки Hexo в этот репозиторий. Папка `public/` не должна загружаться по умолчанию, проверьте, что файл `.gitignore` содержит строку `public/`. Структура папки должна быть такой же, как в [этом репозитории](https://github.com/hexojs/hexo-starter), без файла `.gitmodules`.
3. Добавьте [Travis CI](https://github.com/marketplace/travis-ci) в свой аккаунт.
4. Зайдите на страницу [Настроек приложения](https://github.com/settings/installations), сконфигурируйте Travis CI, чтобы оно имело доступ к репозиторию.
5. Вас перенаправят на страницу Travis.
6. В новой вкладке сгенерируйте [новый токен](https://github.com/settings/tokens) с областью видимости **repo**. Запишите значение токена.
7. На странице Travis зайдите в настройки репозитория. В поле **Environment Variables**, вставьте **GH_TOKEN** в качестве имени и токен в качестве значения. Нажмите `Add` для сохранения.
8. Добавьте файл `.travis.yml` в свой репозиторий (рядом с _config.yml & package.json) со следующим контентом:
```yml
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - master # build master branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master
  local-dir: public
```
9. Как только Travis CI завершит деплой, сгенерированные страницы появятся в ветке `gh-pages` вашего репо.
10. В настройках своего репозитория GitHub перейдите в раздел "GitHub Pages" и измените `Source` на **ветку gh-pages**.
11. Проверьте страницу на *username*.github.io.

### Страница проекта

Если вы препочитаете страницу проекта на GitHub:

1. Перейдите на страницу своего репо на GitHub. Откройте таб **Settings**. Измените **Repository name**, чтобы ваш блог был доступен на <b>username.github.io/*repository*</b>, **repository** может быть любым словом, как *blog* или *hexo*.
2. Редактируйте файл **_config.yml**, изменив значение `root:` на `/<repository>/` (должно начинаться и заканчиваться косой чертой).
3. Закоммитьте и запушьте.

## Приватный репозиторий

Это инстуркция только для приватных репозиториев.

1. Создайте репозиторий с названием <b>*username*.github.io</b>, где `username` — ваше имя пользователя GitHub. Если вы уже загрузили файлы в репозиторий с другим названием, просто переименуйте его. _(Перейдите к шагу 3, если предпочитаете на загружать свои файлы на GitHub)_
2. Запушьте файлы вашей папки Hexo в этот репозиторий. Папка `public/` не должна загружаться по умолчанию, проверьте, что файл `.gitignore` содержит строку `public/`. Структура папки должна быть такой же, как в [этом репозитории](https://github.com/hexojs/hexo-starter), без файла `.gitmodules`.
3. Запустите команду `hexo generate` и скопируйте папку `public/` куда-то ещё (на своеё рабочей машине).
4. Создайте новую ветку `gh-pages` в вашей папке Hexo, Мы рекомендуем создание ветки-"сироты" (чтобы создать новую ветку без истории коммитов):
``` bash
$ git checkout --orphan gh-pages
```
5. Удалите всё, включая скрытые файлы, **кроме** папки `.git/`. Не волнуйтесь, эти файлы всё ещё находятся в ветке master.
6. Переместите _содержимое_ папки `public/` обратно.
7. Закоммитьте и запушьте ветку gh-pages.
``` bash
$ git add .
$ git commit -a -m "Initial commit"
$ git push origin gh-pages
```
8. В настройках своего репозитория GitHub перейдите в раздел "GitHub Pages" и измените `Source` на **ветку gh-pages**.
9. Проверьте страницу на *username*.github.io.
10. Перейдите обратно в папку своего исходного кода,
``` bash
$ git checkout master
```

## Полезные ссылки

- [GitHub Pages](https://help.github.com/categories/github-pages-basics/)
- [Документация Travis CI](https://docs.travis-ci.com/user/tutorial/)
