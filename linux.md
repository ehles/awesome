# Common linux tools

- [Common linux tools](#common-linux-tools)
  - [Inotify catch file system events](#inotify-catch-file-system-events)
    - [Installation](#installation)
    - [inotifywatch](#inotifywatch)
    - [inotifywait](#inotifywait)
  - [Тестирование IOPS дисков в Linux](#тестирование-iops-дисков-в-linux)
  - [Other](#other)

---

## Inotify catch file system events

- **inotifywait**
- **inotifywatch**

### Installation

```bash
apt-get install inotify-tools
```

### inotifywatch

```bash
# Ждём 3 секунды появления событий иначе выходим
inotifywatch -r -t 3 /root/tmp
```

### inotifywait

Event monitoring

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

## Тестирование IOPS дисков в Linux

```bash
# Тест случайных операций на чтение/запись
fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=fiotest --filename=testfio --bs=4k --iodepth=64 --size=8G --readwrite=randrw --rwmixread=75

# Тест случайных операций на чтение
fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=fiotest --filename=testfio --bs=4k --iodepth=64 --size=8G --readwrite=randread

# Тест случайных операций на запись
fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=fiotest --filename=fiotest --bs=4k --iodepth=64 --size=8G --readwrite=randwrite

# Проверка latency диска с помощью ioping
ioping -c 20 /tmp/
```

## Other

- **script** - command in Linux is used to make typescript or record all the terminal activities.
