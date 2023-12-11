### [*>>> к содержанию*](./readme.md)
### [*> следующая страница*](./working_with_remotes.md)
### [*предыдущая страница <*](./gitignore.md)

---

## **Операции отмены**

---

#### В любой момент вам может потребоваться что-либо отменить. Будьте осторожны, не все операции отмены в свою очередь можно отменить! Это одна из редких областей Git, где неверными действиями можно необратимо удалить результаты своей работы.
#### Отмена может потребоваться, если вы сделали коммит слишком рано, например, забыв добавить какие-то файлы или комментарий к коммиту. Если вы хотите переделать коммит — внесите необходимые изменения, добавьте их в индекс и сделайте коммит ещё раз, указав параметр `--amend`:
<pre>
$ git commit --amend
</pre>
#### Эта команда использует область подготовки (*индекс*) для внесения правок в коммит. Если вы ничего не меняли с момента последнего коммита (например, команда запущена сразу после предыдущего коммита), то снимок состояния останется в точности таким же, а всё что вы сможете изменить — это ваше сообщение к коммиту.
#### Запустится тот же редактор, только он уже будет содержать сообщение предыдущего коммита. Вы можете редактировать сообщение как обычно, однако, оно заменит сообщение предыдущего коммита.
#### Например, если вы сделали коммит и поняли, что забыли проиндексировать изменения в файле, который хотели добавить в коммит, то можно сделать следующее:
<pre>
$ git commit -m 'Initial commit'
$ git add forgotten_file
$ git commit --amend
</pre>
#### В итоге получится единый коммит — второй коммит заменит результаты первого.

<table>
    <tr>
        <td> <h4> <strong> Примечание </strong> </h4> </td>
        <td> <h4> Очень важно понимать, что когда вы вносите правки в последний коммит, вы не столько исправляете его, сколько заменяете новым, который полностью его перезаписывает. В результате всё выглядит так, будто первоначальный коммит никогда не существовал, а так же он больше не появится в истории вашего репозитория. </h4> </td>
    </tr>
</table>

---

## **Отмена индексации файла. Команда git reset.**

---

### Например, вы изменили два файла и хотите добавить их в разные коммиты, но случайно выполнили команду `git add *` и добавили в индекс оба. Как исключить из индекса один из них? Команда `git status` напомнит вам:
<pre>
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD &ltfile>..." to unstage)

    renamed:    README.md -> README
    modified:   CONTRIBUTING.md
</pre>
### Прямо под текстом «Changes to be committed» говорится: используйте `git reset HEAD <file>…​` для исключения из индекса. Если последовать этому совету и отменть индексирование файла `CONTRIBUTING.md`, то получим следующие:
<pre>
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M	CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD &ltfile>..." to unstage)

    renamed:    README.md -> README

Changes not staged for commit:
  (use "git add &ltfile>..." to update what will be committed)
  (use "git checkout -- &ltfile>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
</pre>
#### Файл CONTRIBUTING.md изменен, но больше не добавлен в индекс.

<table>
    <tr>
        <td> <h4> <strong> Примечание </strong> </h4> </td>
        <td> <h4> Команда <code>git reset</code> может быть опасной если вызвать её с параметром <code>--hard</code>. В приведенном примере файл не был затронут, следовательно команда относительно безопасна. </h4> </td>
    </tr>
</table>

---

## **Отмена изменений в файле. Команда git checkout.**

---

#### Что делать, если вы поняли, что не хотите сохранять свои изменения файла `CONTRIBUTING.md`? Как можно просто отменить изменения в нём — вернуть к тому состоянию, которое было в последнем коммите (или к начальному после клонирования, или еще как-то полученному)? Нам повезло, что `git status` подсказывает и это тоже.
#### В выводе команды из последнего примера список изменений выглядит примерно так:
<pre>
Changes not staged for commit:
  (use "git add &ltfile>..." to update what will be committed)
  (use "git checkout -- &ltfile>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
</pre>
#### Здесь явно сказано как отменить существующие изменения. Давайте так и сделаем:
<pre>
$ git checkout -- CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD &ltfile>..." to unstage)

    renamed:    README.md -> README
</pre>
#### Откат изменений выполнен.

<table>
    <tr>
        <td> <h4> <strong> Важно </strong> </h4> </td>
        <td> <h4> Важно понимать, что <code>git checkout -- &ltfile&gt </code> — опасная команда. Все локальные изменения в файле пропадут — Git просто заменит его версией из последнего коммита. Ни в коем случае не используйте эту команду, если вы не уверены, что изменения в файле вам не нужны. </h4> </td>
    </tr>
</table>

#### Помните, все что попало в коммит почти всегда Git может восстановить. Можно восстановить даже коммиты из веток, которые были удалены, или коммиты, перезаписанные параметром `--amend`. Но всё, что не было включено в коммит и потеряно — скорее всего, потеряно навсегда.

---

## **Отмена индексации файла с помощью git restore.**

---

#### Git версии 2.23.0 представил новую команду: `git restore`. По сути, это альтернатива `git reset`, которую мы только что рассмотрели. Начиная с версии 2.23.0, Git будет использовать `git restore` вместо `git reset` для многих операций отмены.
#### Например, предположим, что вы изменили два файла и хотите зафиксировать их как два отдельных изменения, но случайно набираете `git add *` и индексируете их оба. Как вы можете убрать из индекса один из двух? Команда `git status` напоминает вам:
<pre>
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged &ltfile>..." to unstage)
	modified:   CONTRIBUTING.md
	renamed:    README.md -> README
</pre>
#### Прямо под текстом «Changes to be committed», написано использовать `git restore --staged <file> …`​ для отмены индексации файла. Итак, давайте воспользуемся этим советом, чтобы убрать из индекса файл `CONTRIBUTING.md`:
<pre>
$ git restore --staged CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged &ltfile>..." to unstage)
	renamed:    README.md -> README

Changes not staged for commit:
  (use "git add &ltfile>..." to update what will be committed)
  (use "git restore &ltfile>..." to discard changes in working directory)
	modified:   CONTRIBUTING.md
</pre>
#### Файл `CONTRIBUTING.md` изменен, но снова не индексирован.

---

## **Откат измененного файла с помощью git restore.**

---

#### Если вы поймете, что не хотите сохранять изменения в файле `CONTRIBUTING.md`, его можно откатить — вернуть обратно к тому, как он выглядел при последнем коммите (или изначально клонирован, или каким-либо образом помещён в рабочий каталог). Команда git status тоже говорит, как это сделать. В выводе последнего примера, неиндексированная область выглядит следующим образом:
<pre>
Changes not staged for commit:
  (use "git add &ltfile>..." to update what will be committed)
  (use "git restore &ltfile>..." to discard changes in working directory)
	modified:   CONTRIBUTING.md
</pre>
#### Применим `git restore`:
<pre>
$ git restore CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged &ltfile>..." to unstage)
	renamed:    README.md -> README
</pre>

<table>
    <tr>
        <td> <h4> <strong> Важно </strong> </h4> </td>
        <td> <h4> Важно понимать, что <code>git restore &ltfile&gt </code> — опасная команда. ВЛюбые локальные изменения, внесенные в этот файл, исчезнут — Git просто заменит файл последней зафиксированной версией. Никогда не используйте эту команду, если точно не знаете, нужны ли вам эти несохраненные локальные изменения. </h4> </td>
    </tr>
</table>

---

### [*>>> к содержанию*](./readme.md)
### [*> следующая страница*](./working_with_remotes.md)
### [*предыдущая страница <*](./gitignore.md)