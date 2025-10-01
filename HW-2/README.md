### Задание 1. Kernel and Module Inspection

#### 1) Версия ядра

![alt text](img/task_1_1.png)

#### 2) Все загруженные модули ядра

![alt text](img/task_1_2.png)

#### 3) Отключить автозагрузку модуля cdrom

> Модуль cdrom отсутствует

![alt text](img/task_1_3.png)

#### 4) Найти и описать конфигурацию ядра (CONFIG_XFS_FS)

![alt text](img/task_1_4.png)

`CONFIG_XFS_FS=m` - поддержка XFS как модуль (`xfs.ko`), загрузится при монтировании XFS.

### Задание 2. Наблюдение за VFS

![alt text](img/task_2_1.png)

- Открытия:

  - `openat(..., "/etc/ld.so.cache", O_RDONLY)` - кэш динамического линковщика
  - `openat(..., "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY)` - загрузка `libc`
  - `openat(..., "/usr/lib/locale/locale-archive", O_RDONLY)` - данные локали
  - **Главное:** `openat(..., "/etc/os-release", O_RDONLY) = 3` - целевой файл, обычный текст

- Чтение/запись:

  - `read(3, "PRETTY_NAME=...", 131072) = 400` - прочитано 400 байт из `/etc/os-release`
  - `write(1, "PRETTY_NAME=...", 400) = 400` - запись в `fd 1` (stdout), который перенаправлён на `/dev/null`
  - Далее `read(...)=0` - EOF, затем `close(3)`

- Почему не видно открытия `/dev/null`:

  - Перенаправление `> /dev/null` делает bash до запуска `cat`: он открывает `/dev/null` и подменяет stdout. Процесс `cat` лишь наследует готовый fd 1, поэтому в `strace` на `cat` нет `open("/dev/null")`

### Задание 3. LVM Management

#### 1. Разметка диска под LVM (GPT + один раздел)

![alt text](img/task_3_1.png)

#### 2. PV и VG

![alt text](img/task_3_2.png)

#### 3. LVs: 1200MiB и всё остальное

![alt text](img/task_3_3.png)

#### 4. Файловые системы

![alt text](img/task_3_4.png)

#### 5. Точки монтирования и монтирование

![alt text](img/task_3_5.png)

#### 6. Автомонтирование

![alt text](img/task_3_6.png)

### Задание 4. Использование pseudo filesystem

#### 1) Модель CPU и объём памяти (KiB)

![alt text](img/task_4_1.png)

#### 2) PPid через `/proc/$$/status` и что значит `$$`

![alt text](img/task_4_2.png)

`$$` - это PID текущего shell (переменная оболочки). Значит, `/proc/$$/status` - статус именно текущего shell-процесса.

#### 3) I/O-планировщик для `/dev/sda`

![alt text](img/task_4_3.png)

#### 4) MTU основного сетевого интерфейса

![alt text](img/task_4_4.png)
