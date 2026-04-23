<div align="center">
<h1><a id="intro">Лабораторная работа №2</a><br></h1>
<a href="https://docs.github.com/en"><img src="https://img.shields.io/static/v1?logo=github&logoColor=fff&label=&message=Docs&color=36393f&style=flat" alt="GitHub Docs"></a>
<a href="https://daringfireball.net/projects/markdown"><img src="https://img.shields.io/static/v1?logo=markdown&logoColor=fff&label=&message=Markdown&color=36393f&style=flat" alt="Markdown"></a>
<a href="https://shields.io"><img src="https://img.shields.io/static/v1?logo=shieldsdotio&logoColor=fff&label=&message=Shields&color=36393f&style=flat" alt="Shields"></a>
<img src="https://img.shields.io/badge/Course-AppSec-D51A1A?style=flat" alt="Course: AppSec">
<img src="https://img.shields.io/badge/Linux-%23FCC624.svg?style=flat&logo=linux&logoColor=black" alt="Linux">
<img src="https://img.shields.io/badge/GitHub_CLI-181717?style=flat&logo=github&logoColor=white" alt="GitHub CLI">
<img src="https://img.shields.io/badge/Contributor-Кузнецов_О._А.-8b9aff?style=flat" alt="Contributor"></div>

***

Салют :wave:,<br>
Данная лабораторная работа посвящена освоению основ управления правами доступа в Linux. Работа позволит ознакомиться с базовыми навыками необходимыми для работы с `chmod`, `chown`, `chgrp`, пользователями, группами, специальными битами (SUID, SGID, Sticky bit) и POSIX ACL.

Для сдачи данной работы также будет требоваться ответить на дополнительные вопросы по описанным темам.

***

## Структура репозитория лабораторной работы

```bash
lab02
├── GIST_lab02.md
├── OUTPUT.txt
├── pygamesteel_fixed.py
├── screen
└── README.md
```

***

## Материал

Linux — многопользовательская операционная система с развитой системой прав доступа. Ключевые концепции:

- **Права доступа** — `r` (read), `w` (write), `x` (execute) для owner/group/other
- **chmod** — изменение прав доступа: `chmod [-R] [option] [rules]`
- **umask** — маска прав доступа для новых файлов (по умолчанию 666 для файлов, 777 для директорий)
- **chown** — изменение владельца: `chown [-R] user[:group] file`
- **chgrp** — изменение группы: `chgrp [-R] group file`

> Поток: `useradd` → `passwd` → `usermod` → `chmod` — базовый цикл настройки пользователей и прав

- **Специальные биты**:
  - **SUID (Set User ID)**: `chmod u+s` — процесс получает права владельца файла
  - **SGID (Set Group ID)**: `chmod g+s` — наследование группы для новых файлов
  - **Sticky bit**: `chmod +t` — только владелец может удалять файлы (пример: `/tmp`)
- **POSIX ACL** — расширенные права: `getfacl`, `setfacl` для детального контроля

### pygamesteel_fixed.py

Файл `pygamesteel_fixed.py` — исправленный Python-скрипт с использованием библиотеки `pygame` для создания графического окна. В задании исправлена критическая ошибка с переменной `screen`.

***

## Задание

- [x] 1. Анализ системы и файловой структуры
- [x] 2. Поиск файлов: which, locate, find
- [x] 3. Исправление баг в pygame-скрипте
- [x] 4. Git операции
- [x] 5. Управление пользователями и группами
- [x] 6. Управление правами доступа и ACL

***

## Tutorial

> Перед началом выполните подготовительные инструкции:
>
> - [Подготовка рабочего окружения](https://course.geminishkv.tech/labs/intro/vmbox_tutorial/) — VirtualBox, установка Linux
> - [Настройка Git, GPG и GitHub CLI](https://course.geminishkv.tech/labs/intro/git_setup/) — git config, SSH, GnuPG, gh

- [x] 1. Анализ системы и файловой структуры: Проведён анализ текущего пользователя и системы с помощью базовых команд `who | wc -l`, `id`, `whoami`, `hostnamectl`, `tree`, `ls -a`, `ls -l`.

```bash
who | wc -l
id
whoami
hostnamectl
tree
ls -a
ls -l
```

- [x] 2. Поиск файлов: which, locate, find: Изучена разница между утилитами поиска файлов `which`, `locate`, `find`, `updatedb`.

```bash
which vi
locate hello.py
sudo updatedb
touch screen
find ~ -name screen
locate screen
```

- [x] 3. Исправление баг в pygame-скрипте: Найдена и исправлена критическая ошибка: переменная `screen` не была присвоена. Создан файл `pygamesteel_fixed.py`.

```python
screen = pygame.display.set_mode(window_size)  # FIX: присваиваем результат

pygame.draw.rect(screen, bg_color, [0, 0, screen_width, screen_height], 1)

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            quit()
    pygame.display.flip()  # FIX: перенесено внутрь while
```

- [x] 4. Git операции: Инициализирован локальный репозиторий и выполнена первая публикация в GitHub.

```bash
cd lab02
git init
git add .
git commit -S -m "First commit with files screen and pygamesteel_fixed.py"
gh repo create lab02 --public --source=. --remote=origin --push
```

- [x] 5. Управление пользователями и группами: Создан новый пользователь `smallman` и группа `readgroup`, настроены их свойства.

```bash
groups
sudo useradd smallman
sudo passwd smallman
sudo usermod smallman -c 'Hach Hachov Hacherovich,239,45-67,499-239-45-33'
sudo id smallman
sudo groupadd -g 1500 readgroup
sudo usermod -aG readgroup smallman
```

- [x] 6. Управление правами доступа и ACL: Установлены права на файл `screen` и использованы POSIX ACL для детального контроля доступа.

```bash
sudo chmod 666 screen
sudo chgrp readgroup screen
sudo chmod 640 screen
touch nmapres.txt
setfacl -m u:smallman:rw nmapres.txt
setfacl -m g:readgroup:r nmapres.txt
getfacl nmapres.txt
```

***

## Результаты

Полностью отработаны основы управления правами доступа в Linux: анализ системы, поиск файлов, исправление багов в Python, Git-операции, управление пользователями/группами и POSIX ACL.

Были использованы инструменты: базовые команды Linux (`who`, `id`, `chmod`, `useradd`), утилиты поиска (`which`, `locate`, `find`), Python с pygame, Git CLI с GPG-подписями, POSIX ACL.

Критическая ошибка в pygame-скрипте (отсутствие присвоения `screen`) исправлена. Создан пользователь `smallman` с группой `readgroup`. Файлы `screen` и `nmapres.txt` используют разные механизмы контроля доступа.

## Выводы

В ходе работы освоены фундаментальные концепции безопасности в Linux:

1. **Управление правами доступа** — традиционные права (chmod) подходят для простых сценариев, но ограничены owner/group/other моделью.

2. **POSIX ACL** — гибкий механизм, позволяющий назначать права конкретным пользователям и группам без изменения основных прав файла.

3. **Управление пользователями** — правильное создание пользователей и групп критично для разделения привилегий и предотвращения несанкционированного доступа.

4. **Баг-фиксинг в Python** — типичная ошибка (забыть присвоить результат функции переменной) приводит к runtime-ошибкам. Необходимо тестировать код перед публикацией.

5. **Git и криптографические подписи** — использование `git commit -S` обеспечивает аутентичность коммитов и является лучшей практикой для AppSec-инженеров.

Полученные навыки соответствуют требованиям безопасного администрирования Unix-систем и необходимы для разработки защищённых приложений.

## История коммитов

```
445365c (HEAD -> master, origin/master, origin/HEAD) First commit with files screen and pygamesteel_feexed.py
```

**Репозиторий**: https://github.com/OlegKuz86a1/lab02</content>
<parameter name="filePath">/home/oem/sec_course_labs/course_labs/meFolder/lab02/README.md
