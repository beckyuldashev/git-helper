# Шпаргалка по GIT

## Сделать папку репозиторием — git init

Чтобы Git начал отслеживать изменения в проекте, папку с файлами этого проекта нужно сделать **Git-репозиторием** (от англ. repository — «хранилище»). Для этого следует переместиться в неё и ввести команду `git init` (от англ. initialize — «инициализировать»).

```
git init
```

## «Разгитить» папку, если что-то пошло не так, — rm -rf .git

Если вы случайно сделали Git-репозиторием не ту папку, её можно «разгитить». Для этого нужно удалить скрытую подпапку .git.

```
rm -rf .git
```

## Подготовить файлы к сохранению — git add

```
git add test.txt
git add --all
git add .
```

- Команда `git add` позволяет подготовить файл к сохранению.
- Команда `git add --all` подготовит к сохранению сразу все файлы.
- С помощью `git add .` можно добавить в репозиторий текущую папку со всеми файлами.

## Выполнить коммит — git commit

Сделать коммит можно командой `git commit` c ключом -m (от англ. message — «сообщение»), который присваивает коммиту сообщение.

```
git commit -m "Message"
```

## Просмотреть историю коммитов — git log

```
git log
```

`git log` выводит коммиты в обратном хронологическом порядке — последние коммиты оказываются первыми сверху

## Привязать удалённый репозиторий к локальному — git remote add

```
git remote add origin git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ_ПРОЕКТА%.git 
```

Команде необходимо передать два параметра: имя удалённого репозитория и его URL. В качестве имени используйте слово **origin**. А URL можно скопировать со страницы удалённого репозитория.

### Убедиться, что репозитории связаны, — git remote -v

```
git remote -v
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (fetch)
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (push) 
```

## Отправить изменения на удалённый репозиторий — git push

В первый раз эту команду нужно вызвать с флагом **-u** и параметрами **origin** (имя удалённого репозитория) и **main** или **master** (название текущей ветки). Флаг **-u** свяжет локальную ветку с одноимённой удалённой.

```
git push -u origin main    #в первый раз
git push                   #при следующих вызовах
```

## Навигация по коммитам. Статусы файлов
### Хеш — идентификатор коммита

- Git преобразует информацию о коммитах с помощью алгоритма SHA-1 и для каждого из них рассчитывает уникальный идентификатор — хеш.
- Хеш — основной идентификатор коммита и позволяет узнать его автора, дату и содержимое закоммиченных файлов.
- Все хеши, а также таблицу соответствий хеш → информация о коммите Git хранит в папке .git.

### Исследуем лог

После вызова `git log` появляется список коммитов.
Разберём элементы, из которых состоит описание:
- строка из цифр и латинских букв после слова commit — это хеш коммита;
- Author — имя автора и его электронная почта;
- Date — дата и время создания коммита;
- в конце находится сообщение коммита.

### HEAD — всему голова

Файл HEAD (англ. «голова», «головной») — один из служебных файлов папки .git. Он указывает на коммит, который сделан последним (то есть на самый новый).

Внутри HEAD — ссылка на служебный файл: refs/heads/master (или refs/heads/main в зависимости от названия ветки). Если заглянуть в этот файл, можно увидеть хеш последнего коммита.

### Статусы файлов в Git

До появления Git системы контроля версий выделяли только два статуса у файлов: «уже закоммичен» и «ещё не закоммичен»

#### Статусы untracked/tracked, staged и modified

Одна из ключевых задач Git — отслеживать изменения файлов в репозитории. Для этого каждый файл помечается каким-либо статусом. Рассмотрим основные.

- **untracked** (англ. «неотслеживаемый»)
Новые файлы в Git-репозитории помечаются как untracked, то есть неотслеживаемые. Git «видит», что такой файл существует, но не следит за изменениями в нём. У untracked-файла нет предыдущих версий, зафиксированных в коммитах или через команду git add.

- **staged** (англ. «подготовленный»)
После выполнения команды git add файл попадает в staging area, то есть в список файлов, которые войдут в коммит. В этот момент файл находится в состоянии staged.

- **tracked** (англ. «отслеживаемый»)  
Состояние tracked — это противоположность untracked. Оно довольно широкое по смыслу: в него попадают файлы, которые уже были зафиксированы с помощью git commit, а также файлы, которые были добавлены в staging area командой git add. То есть все файлы, в которых Git так или иначе отслеживает изменения.

- **modified** (англ. «изменённый»)  
Состояние modified означает, что Git сравнил содержимое файла с последней сохранённой версией и нашёл отличия. Например, файл был закоммичен и после этого изменён.

> 💡 Staging area, index и cache
Staging area также называют index (англ. «каталог») или cache (англ. «кеш»), а состояние файла staged иногда называют indexed или cached.

#### Про staged и modified

Команда `git add` добавляет в staging area только текущее содержимое файла. Если вы, например, сделаете git add file.txt, а затем измените file.txt, то новое содержимое файла не будет находиться в staging.

Git сообщит об этом с помощью статуса modified: файл изменён относительно той версии, которая уже в staging. Чтобы добавить в staging последнюю версию, нужно выполнить `git add file.txt` ещё раз.

## Стили оформления сообщений в коммитах

### Корпоративный

В корпоративном стиле в начале сообщения обычно указывают Jira-ID, а после — текст сообщения.

```
git commit -m "LGS-239: Дополнить список пасхалок новыми числами" 
```

### Conventional Commits

Он подходит для репозиториев с исходным кодом программ. Conventional Commits предлагает такой формат коммита: \<type>: <сообщение>.

```
git commit -m "feat: добавить подсчёт суммы заказов за неделю" 
```

### GitHub-стиль

GitHub можно использовать не только для хранения файлов проекта, но и для ведения списка задач (англ. issue) этого проекта. Если коммит «закрывает» или «решает» какую-то задачу, то в его сообщении удобно указывать ссылку на неё. Для этого в любом месте сообщения нужно указать #<номер задачи>. Например, вот так.

```
git commit -m "Исправить #334, добавить график температуры" 
```

## Как исправить коммит последний коммит

Иногда в только что выполненном коммите нужно что-то поменять: например, добавить ещё пару файлов или заменить сообщение на более информативное.

В таком случае можно внести правки в уже сделанный коммит с помощью опции **--amend** (от англ. amend — «исправить», «дополнить») у команды **commit**: `git commit --amend`.

> 💡 Важно: опция `--amend` работает только с последним коммитом (HEAD). Для исправления более ранних коммитов есть другие команды.

### Дополнить последний коммит новыми файлами — git commit --amend --no-edit

```
git commit --amend --no-edit
```

С опцией `--amend` команда `commit` не создаст новый коммит, а дополнит последний. При этом хеш последнего коммита изменится, потому что изменился список файлов в коммите.

Опция `--no-edit` сообщает команде commit, что сообщение коммита нужно оставить как было.

### Изменить сообщение последнего коммита — git commit --amend -m "Новое сообщение"

Может быть и так, что добавлять новые файлы в коммит не нужно, зато понадобилось изменить сообщение.

```
git commit --amend -m "Добавить главную страницу и стили"
```

> 💡 Если забыть указать у команды `git commit --amend` один из флагов (--no-edit или -m), Git предложит отредактировать сообщение коммита вручную через редактор **nano** или **Vim**


## Как откатиться назад, если «всё сломалось»

### Выполнить unstage изменений

Если вы создали или изменили какой-то файл и добавили его в список «на коммит» (staging area) с помощью `git add`, но потом передумали включать его туда. Убрать файл из staging поможет команда `git restore --staged <file>`.

```
git restore --staged <название файла>
```

### «Откатить» коммит

Иногда нужно «откатить» то, что уже было закоммичено, то есть вернуть состояние репозитория к более раннему. Для этого используют команду:

```
git reset --hard <хэш коммита куда хотим откатиться>
```

### «Откатить» изменения, которые не попали ни в staging, ни в коммит

Может быть так, что вы случайно изменили файл, который не планировали. Теперь он отображается в Changes not staged for commit (modified). Чтобы вернуть всё «как было», можно выполнить команду:

```
git restore <название файла>
```

## Как просматривать изменения в файлах

```
git diff
```

Команда `git diff` сравнит последнюю закоммиченную версию файла с той, что находится в состоянии `modified`.

Команда `git diff --staged` покажет изменения в `staged`-файлах относительно последних закоммиченных версий.

Вот что будет выведно после команды `git diff`:

```
$ git diff
diff --git a/teremok.txt b/teremok.txt
index 09348ca..e77226b 100644
--- a/teremok.txt
+++ b/teremok.txt
@@ -1,2 +1,2 @@
 Теремок стоит, и в нём:
-никого нет
\ No newline at end of file
+Мышка-норушка
\ No newline at end of file

```
Коротко разберём строки вывода команды:

- Строки --- a/teremok.txt и +++ b/teremok.txt говорят, что дальше будет выведен результат сравнения файлов a/teremok.txt и b/teremok.txt — исходной и текущей версий.

- Строка @@ -1,2 +1,2 @@ сообщает, какие строки файла попали в сравнение. Выражение 1,2 (неважно, с плюсом или с минусом) говорит, что были использованы две строки, начиная с первой. Если бы было, например, написано +15,7, это значило бы, что в сравнении участвуют 77 строк, начиная с 1515-й.

- Выражение со знаком минус (-1,2) относится к «оригинальной» версии файла (a/teremok.txt), а со знаком плюс (+1,2) — к «изменённой» (b/teremok.txt).