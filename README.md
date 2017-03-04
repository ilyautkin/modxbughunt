# Как стать контрибьютором MODX (руководство по отлову багов)
Это руководство написано для участников MODX Bug Hunt. Мероприятие уже завершено, если вы его пропустили, информацию можно найти [на официальном сайте modxbughunt.com](http://www.modxbughunt.com).

## 1. Инструменты
- Интерфейс с командной строкой (Терминал, iTerm и пр.)
- Git, установленный на вашей системе
- PHP Storm или другой клиент. Пожалуйста, убедитесь, что вы используете 4 пробела для отступов, а не табы.
- Любой веб-сервер. Желательно, чтобы на борту был Apache или NGINX. Мы используем Vagrant. Или можно взять на вооружение [MAMP](https://www.mamp.info/en/) или [XAMPP](https://www.apachefriends.org/index.html).

## 2. Github
- Вам понадобится аккаунт на [Github.com](https://github.com/)
- После регистрации сделайте форк MODX Revolution. Для этого нажмите кнопку «Fork» на странице [https://github.com/modxcms/revolution](https://github.com/modxcms/revolution) в правом верхнем углу.

## 3. Примите условия лицензионного соглашения
Чтобы ваш код, мог стать частью MODX, нужно принять условия Contributors License Agreement. Если вы этого еще не сделали, [прочитайте его и заполните анкету](https://develop.modx.com/contribute/cla/). 

## 4. Получение файлов MODX из репозитория
Сначала склонируйте ваш форк к себе на локальную машину (в корень сайта).

**Примечание:** в примерах ниже используются SSH-адреса репозиториев (такие — git@github.com:modxcms/revolution.git). Но Github поддерживает ещё и HTTPS-ссылки, которые легче в понимании для новичков (это такие ссылки — https://github.com/modxcms/revolution.git). Вы можете просто заменить их в примерах.
```
$ git clone git@github.com:modxcms/revolution.git
$ cd revolution
```
Далее, свяжите ваш локальный репозиторий с оригинальным modxcms/revolution. Зачем это надо, увидим позже, сейчас просто сделайте это.
```
$ git remote add upstream git@github.com:modxcms/revolution.git
```
Теперь нужно переключится (читай «скачать») текущую ветку, для разработки. На текущий момет это ```2.5.x```.
```
$ git checkout 2.5.x
```
Убедитесь, что ваш репозиторий всё ещё чист (что вы не вносили в него никаких изменений).
```
$ git status
On branch 2.5.x
Your branch is up-to-date with 'origin/2.5.x'.
nothing to commit, working tree clean
```
Если вы видете такое сообщение, значит, всё сделано правильно. Если вы видете какие-то изменения, это прям плохо-плохо! Попробуйте сделать всё с нуля и не изменять никаких файлов до этого этапа. Иначе ваш коммит испортит репозиторий MODX.

Дальше мы должны сделать кое-что странное. Git-версия MODX отличается от дистрибутива, который доступен для скачивания с сайта MODX. Нам нужно собрать его вручную.

Перейдите в папку ```_build``` и убедитесь, что вы действительно в ней. Например, при помощи команды _pwd_, которая показывает путь к текущей папке.
```
$ cd _build
$ pwd
/your-absolute-path-here/revolution/_build
```
Дальше копируем (НЕ переименовываем, а именно копируем) 2 файла:
```
$ cp build.config.sample.php build.config.php
$ cp build.properties.sample.php build.properties.php
```
В эти файлы не нужно будет вносить изменений, они просто должны существовать.

Следующим шагом нужно убедиться, что в вашей папке доступен PHP. Сделать это можно следующей командой:
```
$ php -v
PHP 7.0.15 (cli) (built: Jan 22 2017 08:51:45) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2017 Zend Technologies
```

**Если ваша система выдала ответ, непохожий на такое сообщение, установите и настройте PHP. Google вам в помощь.**

Чтобы собрать дистрибутив, запустите transport.core.php в папке ```_build```:
```
$ php transport.core.php
[2017-02-24 11:52:17] (INFO @ transport.core.php) Beginning build script processes...
[2017-02-24 11:52:17] (INFO @ transport.core.php) Removed pre-existing core/ and core.transport.zip.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Core transport package created.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Core Namespace packaged.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Default workspace packaged.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged modx.com transport provider.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 2 modMenus.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged all default modContentTypes.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged all default modClassMap objects.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 181 default events.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 225 default system settings.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 2 default context settings.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 1 default user groups.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 1 default dashboards.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 1 default media sources.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 5 default dashboard widgets.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 2 default roles Member and SuperUser.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 6 default Access Policy Template Groups.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 7 default Access Policy Templates.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 12 default Access Policies.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in web context.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in mgr context.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in connectors.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Beginning to zip up transport package...
[2017-02-24 11:52:18] (INFO @ transport.core.php) Transport zip created. Build script finished.

Execution time: 1.5657 s
```
Если будут какие-то ошибки, связанные с timezone, ничего страшного — это неважно. Главное здесь — сообщение "Transport zip created. Build script finished.". Если его нет, придётся повозиться, разобраться. Спрашивайте у старших товарищей.

## 5. Выполняем установку MODX
Теперь, когда все файлы на месте, нам осталось запустить установку MODX. Не забудьте создать базу данных и обязательно используйте везде кодировку _utf8_unicode_ci_.

Запустите установку, но **НЕ УДАЛЯЙТЕ папку setup**. В админке будет сообщение с советом удалить её — просто игнорируйте это сообщение. Если эту папку удалить, ваш коммит сломает репозиторий MODX.

После установки поменяйте следующие системные настройки:
```
friendly_urls     => Yes
use_alias_path    => Yes
publish_default   => Yes
cache_disabled    => Yes
```
Вы можете так же удалить расширение .html у типа содержимого HTML. Настройка типов содержимого доступна в верхнем меню админки (Содержимое → Типы содержимого) Это необязательно, но мы предпочитаем, чтобы у страниц не было расширения .html.

Очистите кеш сайта.

**Наполните сайт каким-нибудь контентом***

## 6. Рабочий процесс
Вклад можно осуществить двуми способами: фиксить баги (собственно, сам процесс разработки) и тестировать (если вы ещё не готовы писать код, но хотите помочь).

## 6A. Как фиксить баги
### 1. Выбирайте проблему из [баг-трекера MODX](https://github.com/modxcms/revolution/issues)
### 2. Чтобы вы не делали одну работу с кем-то ещё, оставьте комментарий к проблеме.
Можно написать что-то вроде этого: ```I'm going to try and fix this today``` _(Я попробую пофиксить это сегодня)_
### 3. После этого создавайте новую ветку из вашей текущей ветки разработки (2.5.x), чтобы работать уже в своём собственном окружении.

Если проблема, которую вы собираетесь решить, это фича, называйте ветку feature-ISSUENUMBER. Если это баг, то, соответственно, bug-ISSUENUMBER. Например, мы хотим починить битую ссылку в документах. Описание этой проблемы находится [здесь](https://github.com/modxcms/revolution/issues/13309). Её номер — 13309.

```
$ git checkout -b bug-13309
```
Теперь исправяем этот баг и меняем некоторый код. Если вы уверены в сделанных изменениях, нужно будет отправить их обратно на Github. В нашем примере мы редактировали файл ```core/lexicon/en/about.inc.php```

Перед отправкой проверьте, что git видит изменения только тех файлов, которые вы действительно редактировали.
```
$ git status
On branch bug-13309
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   core/lexicon/en/about.inc.php

no changes added to commit (use "git add" and/or "git commit -a")
```
Если файлы, которые нужно отправить, появились в этом списке, значит, вы всё сделали правильно. Теперь мы готовы добавить эти файлы в коммит и отправить его в наш онлайн репозиторий на Github. Указывайте номер решённой проблемы с помощью хештега. Так же можно указать другие теги, например, в рамках MODX Bug Hunt будет использоваться хештег ```#modxbughunt```.

```
$ git add .
$ git commit -m "Fixed issue #13309 #modxbughunt"
$ git push origin
git push --set-upstream origin bug-13309
Counting objects: 4, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 1.69 KiB | 0 bytes/s, done.
Total 4 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local objects.
To github.com:gpsietzema/revolution.git
 * [new branch]      bug-13309 -> bug-13309
Branch bug-13309 set up to track remote branch bug-13309 from origin.
```
Вот и всё, для чего нам нужна командная строка. Теперь заходим на сайт Github, чтобы создать пулл-реквест (PR).

### 4. Зайдите на страницу вашего форка на Github
Скорее всего, вы увидете уведомление ```Your recently pushed branches: bug-13309 (2 minutes ago)``` (Вы недавно отправили ветки: bug-13309). Нажмите «compare & pull request».

Теперь вы видете сравнение двух репозиториев. Слева ```modxcms/revolution```, ветка ```base: 2.5.x```. Справа ```yourname/revolution``` ветка ```bug-13309```.

Если всё нормально, будет показано уведомление ```Able to merge. These branches can be automatically merged.``` (Доступно слияние. Эти ветки могут быть автоматически объеденены).

Пожалуйтса, оставьте описание своего коммита для наших любимых интеграторов (это люди, которые будут вносить изменения, которые вы предложили, в исходный код MODX). Так им будет проще протестировать ваш коммит.

После того, как всё это сделано, нажимаем на волшебную кнопку **Create pull request**

Поздравляем! Вы сделали это!

### 5. К новым вершинам!
Если вы хотите пофиксить ещё один баг, нужно переключиться обратно на ветку ```2.5.x```. Чтобы сделать это, нужно проверить, что ветка ```2.5.x``` нашего форка синхронизирована с оригинальной веткой ```modxcms/2.5.x```. Выполните следующие команды в командной строке:
```
$ git fetch upstream 2.5.x
$ git fetch origin 2.5.x
$ git checkout 2.5.x
$ git pull upstream 2.5.x
```
Если после выполнения последней команды открылся редактор с сообщением о слиянии (merge), просто сохраните и выйдите из редактора. Если у вас VI, нажмите Escape, чтобы выйти из текстового режима, после чего введите ```:wq``` и нажмите Enter.

Теперь ваш форк обновлён и вы можете вернуться к шагу № 1 в пункте 6a. Повторять до достижения удовлетворения!

## 6B. Как тестировать
### 1. Выберите пулл-реквест
Выберите пулл-реквест из актуального [списка на Github](https://github.com/modxcms/revolution/pulls).
### 2. Прочтите PR
Прочтите PR и решите, сможете ли вы его протестировать. Проверьте, что проблема действительно до сих пор существует и вы можете воспроизвести её в ветке, которая сейчас в разработке:
```
$ git checkout 2.5.x
```
Если у вас получилось воспроизвести проблему, отметьте что вы собираетесь протестировать пулл-реквест в комментарии к нему. Если проблему воспроизвести не получилось, отметьте это в комментарии, упомянув пользователя, который отправил этот пулл-реквест. Не забудьте добавить хештег #modxbughunt в комментарий (если работа проводится в рамках этого мероприятия).
### 3. Get the PR locally
To pull this PR, you need to add the fork of the PR-owner to your remotes. In this example I'm using a random PR. In this case, one by goldsky. In hte example below, you'll see the 'git remote add goldsky' part. 'goldsky' is the name of remote. This can be anything, but we recommend to use the Git-username to make it easy to remember. The 'goldsky:patch-ellipsis' part is the Github-URL of Goldsky's modxcms-fork.

After adding the remote, fetch it and checkout the PR-branch. In this case ```patch-ellipsis```.

```
$ git remote add goldsky git@github.com:goldsky/revolution.git
$ git fetch goldsky
$ git checkout patch-ellipsis
Branch patch-ellipsis set up to track remote branch patch-ellipsis from goldsky.
Switched to a new branch 'patch-ellipsis'
```

### 4. Clear both your MODX and browser cache
### 5. Test whether the bug is really fixed or not
### 6. Is it fixed or not?
Is it fixed? Let the integrators and fixer know by mentioning them in your comment. 
Not fixed? Let the fixer know by mentioning him in a comment.

Don't forget to mention the #modxbughunt tag in your comment.

## Problems, issues, help needed?
Just ask in the [MODX Community Slack #development](https://modx.org/) or on the [MODX Community Forums](https://forums.modx.com/).
