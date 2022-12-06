# Common linux tools

- [Common linux tools](#common-linux-tools)
  - [Inotify ловим изменения файловой системы](#inotify-ловим-изменения-файловой-системы)
    - [Установка](#установка)
    - [inotifywatch](#inotifywatch)
    - [inotifywait](#inotifywait)

---

## Inotify ловим изменения файловой системы

- **inotifywait**
- **inotifywatch**

### Установка

```bash
apt-get install inotify-tools
```

### inotifywatch

```bash
# Ждём 3 секунды появления событий иначе выходим
inotifywatch -r -t 3 /root/tmp
```

### inotifywait

Мониторинг событий

```bash
#!/bin/bash

# Бэкап файлов сразу после их появления

SRC_DIR="/home"
DST_DIR="/backup"

# Функция, которая будет выполнять необходимые действия
# В нашем случае копировать в другую директорию
make_action(){
    # Получаем директорию назначения
    DIR_TO_COPY_TO=${1/${SRC_DIR}/${DST_DIR}}
    # Создаем ее, если ее еще не существует
    mkdir -p $DIR_TO_COPY_TO
    # Копируем файл
    cp $1$2 $DIR_TO_COPY_TO
}

IFS='
'
# Отслеживаем закрытие файлов после записи
# Получаем вывод в нужном нам формате
inotifywait -e close_write --format '%w %f' -m -r $SRC_DIR |\
(
while read
do
    # Получаем имя директории
    DIR=$(echo $REPLY | cut -f 1 -d' ')
    # Получаем имя файла
    FILE=$(echo $REPLY | cut -f 2 -d' ')
    # Передаем имена директории и файла в функцию
    make_action $DIR $FILE
done
)
```
