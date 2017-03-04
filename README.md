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

If the issue you want to fix is a feature, name it feature-ISSUENUMBER. If it is a bug, name it bug-ISSUENUMBER. In this example we'll fix a broken link in the docs. The issue can be [found here](https://github.com/modxcms/revolution/issues/13309). It has issue number 13309.

```
$ git checkout -b bug-13309
```

Next, we'll fix our issue and change some code. If you're confident about your changes, we want to commit it back to Github. We changed the file ```core/lexicon/en/about.inc.php```

Before doing this, we need to check if git is only trying to commit the files you had in mind. Sometimes, another file you don't know of is added to your repo.

```
$ git status
On branch bug-13309
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   core/lexicon/en/about.inc.php

no changes added to commit (use "git add" and/or "git commit -a")
```

If the files you want added to MODX are also in the status-report above, you did well. You're all set! We need to add it to our commit and push it to our online fork on Github. Use the hashtag to reference the issue and/or to tag it for stuff like the MODX Bug Hunt.

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

That's it for the command line! Now we need to head over to Github to make a Pull Request (PR).

### 4. Go to your fork on Github
You'll probably see a message resembling something like ```Your recently pushed branches: bug-13309 (2 minutes ago)```. Click the "compare & pull request" button.

Now you see two repositories being compared. On the left, there is ```modxcms/revolution```, set it to ```base: 2.5.x```. On the right you'll see ```yourname/revolution``` with ```bug-13309```.

If everything is fine, you'll notice a message stating the following: ```Able to merge. These branches can be automatically merged.```.

Please make sure you enter some explanation about your commit for our beloved integrators (the people who will actually merge your code into MODX). This will make it easier for them to test it.

Once you've did this, click the magic button **Create pull request**

Congratulations, you did it!
### 5. On to the next one!
If you want to fix another bug, we first need to be on the ```2.5.x``` branch again. To do this, we first want to make sure that our Fork's ```2.5.x``` branch is in sync with the original ```modxcms/2.5.x``` branch. Do the following to accomplish this:
```
$ git fetch upstream 2.5.x
$ git fetch origin 2.5.x
$ git checkout 2.5.x
$ git pull upstream 2.5.x
```

If in the last step, you get a text editor with a merge message. Just save and quit the editor and you are all fine. If this editor is VI, just hit Escape to exit type-mode, then type ```:wq``` and hit enter.

You now have updated your Fork. Next you can go back to step 1 in 6a. Rinse and repeat!

## 6B. Test workflow
### 1. Pick a pull request
Pick a pull request from the current [PR-list on Github](https://github.com/modxcms/revolution/pulls).
### 2. Read the PR
Read the PR and check if this is something you might be able to test. Check if the issue is still existent and you can reproduce in the current development branch:
```
$ git checkout 2.5.x
```

If you can reproduce it, comment in the PR that you are going to test it. If you can't reproduce it, mention that as-well and mention the user who made the PR. Don't forget to mention the #modxbughunt tag in your comment.
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
