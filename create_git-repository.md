### [*>>> к содержанию*](./readme.md)
### [*> следующая страница*](./git_add.md)
### [*предыдущая страница <*](./setting.md)

---

# **Создание git-репозитория**

#### Обычно вы получаете репозиторий Git одним из двух способов:
1. #### Вы можете взять локальный каталог, который в настоящее время не находится под версионным контролем, и превратить его в репозиторий Git, либо
2. #### Вы можете клонировать существующий репозиторий Git из любого места.

#### В обоих случаях вы получите готовый к работе Git репозиторий на вашем компьютере.

---

## **Создание репозитория в существующем каталоге**

---

#### Если у вас уже есть проект в каталоге, который не находится под версионным контролем Git, то для начала нужно перейти в него. Если вы не делали этого раньше, то для разных операционных систем это выглядит по-разному:
* #### для Linux:
<pre>
$ cd /home/user/my_project
</pre>
* #### для macOS:
<pre>
$ cd /Users/user/my_project
</pre>
* #### для Windows:
<pre>
$ cd C:/Users/user/my_project
</pre>

#### а затем выполните команду:
<pre>
$ git init
</pre>
#### Эта команда создаёт в текущем каталоге новый подкаталог с именем .git, содержащий все необходимые файлы репозитория — структуру Git репозитория. Метаданные включают подкаталоги для объектов, ссылок и файлов шаблонов. Кроме того, создается файл `HEAD`, который указывает на текущий извлеченный коммит. На этом этапе ваш проект ещё не находится под версионным контролем. Большинство остальных команд Git невозможно использовать без инициализации репозитория, поэтому данная команда обычно выполняется первой в рамках нового проекта.
#### По умолчанию команда `git init` инициализирует конфигурацию Git в подкаталоге .git по соответствующему пути. При необходимости путь подкаталога можно изменить. Если присвоить переменной среды `$GIT_DIR` пользовательское значение пути, команда `git init` инициализирует файлы конфигурации Git по этому пути. Такой же результат можно получить, если передать команде аргумент `--separate-git-dir`. Часто отдельный подкаталог .git создается из тех соображений, что системные файлы конфигурации с точкой перед именем (`.bashrc`, `.vimrc` и др.) должны размещаться в домашнем каталоге, а папка .git — в другом месте.
#### Git не требует создавать репозиторий, импортировать файлы и извлекать рабочую копию. Кроме того, для Git не требуются права администратора или доступ к серверу. Для получения полноценного репозитория Git достаточно перейти в подкаталог проекта и выполнить команду `git init`.
#### Если команда `git init` уже выполнялась по отношению к каталогу проекта и в нем есть подкаталог .git, команду `git init` можно безопасно выполнить для этого каталога повторно. Существующая конфигурация .git при этом не изменится.
#### Если вы хотите добавить под версионный контроль существующие файлы (в отличие от пустого каталога), вам стоит добавить их в индекс и осуществить первый коммит изменений. Добиться этого вы сможете запустив команду `git add` несколько раз, указав индексируемые файлы, а затем выполнив `git commit`:
<pre>
$ git add *.c
$ git add LICENSE
$ git commit -m 'Initial project version'
</pre>
#### Теперь у вас есть Git-репозиторий с отслеживаемыми файлами и начальным коммитом.

---

## **Чистые репозитории**

---

#### Командой `git init --bare` инициализируется пустой репозиторий Git без рабочего каталога. Флаг `--bare` приводит к созданию репозитория без рабочего каталога. В таком репозитории нельзя редактировать файлы и вносить изменения. Чистый репозиторий создается для выполнения команд `git push` и `git pull`, но не для прямых коммитов. Центральные репозитории всегда следует создавать чистыми, поскольку при отправке веток в обычный репозиторий существует риск перезаписи изменений. Просто представьте, что флаг `--bare` превращает репозиторий из среды разработки в хранилище. По этой причине для рабочих процессов Git центральный репозиторий практически всегда создается **чистым**, а локальные репозитории разработчиков — **обычными**.  
#### Команда `git init --bare` чаще всего используется для создания удаленного центрального репозитория:
<pre>
ssh <user>@<host> cd path/above/repo git init --bare my-project.git
</pre>
#### Сначала устанавливается SSH‑соединение с сервером, на котором будет размещаться центральный репозиторий. Затем нужно перейти в каталог, где планируется хранить проект, и с помощью флага `--bare` создать центральный репозиторий‑хранилище. Позднее разработчики смогут клонировать каталог `my-project.git` для создания локальной копии на рабочих компьютерах.

---

## **Параметры командной строки git init**

---

#### `git init` имеет еще ряд параметров командной строки. Их полный перечень приводится ниже:
1. #### `-Q`, `--QUIET` - Вывод только критических сообщений, сообщений об ошибках и предупреждений. Вывод других сообщений блокируется;
2. #### `--BARE` - Создание чистого репозитория;
3. #### `--TEMPLATE=` - Указание каталога с шаблонами для использования;
4. #### `--SEPARATE-GIT-DIR=` - Создание текстового файла, содержащего путь к каталогу. Данный файл служит ссылкой на каталог .git и может быть полезен, если каталог .git нужно хранить отдельно от рабочих файлов проекта (например, на другом диске). Параметр `--separate-git-dir` обычно используют в следующих случаях:
    * #### Необходимо хранить системные файлы конфигурации с точкой перед именем (`.bashrc`, `.vimrc` и др.) в домашнем каталоге, а папку .git — в другом месте;
    * #### История Git стала занимать слишком много места и требует переноса на диск большей емкости.
    * #### Проект Git требуется разместить в общедоступном каталоге, например `www:root`

---

## **Клонирование существующего репозитория**

---

#### Для получения копии существующего Git-репозитория, например, проекта, в который вы хотите внести свой вклад, необходимо использовать команду `git clone`. При выполнении `git clone` с сервера забирается (pulled) каждая версия каждого файла из истории проекта. Фактически, если серверный диск выйдет из строя, вы можете использовать любой из клонов на любом из клиентов, для того, чтобы вернуть сервер в то состояние, в котором он находился в момент клонирования (вы можете потерять часть серверных хуков (server-side hooks) и т. п., но все данные, помещённые под версионный контроль, будут сохранены.
#### Клонирование, как и команда `git init`, обычно выполняется один раз. Получив рабочую копию, разработчик в дальнейшем выполняет все операции контроля версий из своего локального репозитория.
#### Клонирование репозитория осуществляется командой `git clone <url>`. В качестве параметра в команду `git clone` передается URL-адрес репозитория.  Например, если вы хотите клонировать библиотеку `libgit2`, вы можете сделать это следующим образом:
<pre>
$ git clone https://github.com/libgit2/libgit2
</pre>
#### Эта команда создаёт каталог `libgit2`, инициализирует в нём подкаталог `.git`, скачивает все данные для этого репозитория и извлекает рабочую копию последней версии. Если вы перейдёте в только что созданный каталог `libgit2`, то увидите в нём файлы проекта, готовые для работы или использования. Для того, чтобы клонировать репозиторий в каталог с именем, отличающимся от `libgit2`, необходимо указать желаемое имя, как параметр командной строки
<pre>
$ git clone https://github.com/libgit2/libgit2 mylibgit
</pre>
#### Эта команда делает всё то же самое, что и предыдущая, только результирующий каталог будет назван `mylibgit`.
#### В Git реализовано несколько транспортных протоколов, которые вы можете использовать. В предыдущем примере использовался протокол `https://`, вы также можете встретить `git://` или `user@server:path/to/repo.git`, использующий протокол передачи SSH. 

---

### [*>>> к содержанию*](./readme.md)
### [*> следующая страница*](./git_add.md)
### [*предыдущая страница <*](./setting.md)