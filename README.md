# Лараторная 3. Первый контейнер

## Выполнил: Виктор Анисимов

## Группа: IA2403

## Дата: 22.02.2026

## Задание: Данная лабораторная работа знакомит с контейнерами и их использованием

### Установка докера

У меня уже был установлен Docker:

``` bash
~> docker -v
Docker version 29.1.2, build v29.1.2
```

Что бы его установить на NixOS необходимо добавить соответсвующее название пакета - `docker` или конкретную версию, например, `docker_29` в список зависимостей системы и выполнить:

``` bash
sudo nixos-rebuild switch
```

Создал файловую структуру:

``` bash
> tree
.
├── Dockerfile
├── README.md
└── site
    └── index.html

2 directories, 3 files

> cat Dockerfile 
FROM debian:latest
COPY ./site/ /var/www/html/
CMD ["sh", "-c", "echo hello from $HOSTNAME"]
```

### Запуск и тестирование

Выполнил команду создания контейнира:

``` bash
docker build -t containers03 .
```

Параметр `-t` помечает контейнер названием и опционально тегом - тег пишется после двоеточия (пример названия: "name:tag").

Команда выполнилась примерно за 10 секунд из-за установки образа debian.

После запускаю докер командой:

``` bash
> docker run --name containers03 containers03
hello from bd88fcba1773
```

Видно сообщение настроенное в конфигурации **DockerFile** - `CMD ["sh", "-c", "echo hello from $HOSTNAME"]`, означающее что контейнер успешно запущен и нет никаких ошибок.

Удалив существующий докер командой:

``` bash
docker rm containers03
```

Запускаю новый командой:

``` bash
docker run -ti --name containers03 containers03 bash
```

- Параметр `--name containers03` даёт название для будущего контейнера.
- Следующий `containers03`, после `--name`, это название образа из которого запускаем контейнер.
- `bash` - это название процессов который станет PID 1 - самым приоритентым в контейнере.
- Параметр `-t` создаёт терминал для контейнера.
- Параметр `-i` делает возможным ввод команд.

Именно сочетание `-it` с `bash` позволяет удобно управлять контейнером через терминал.

Данной командой открывается терминал сервера.

``` bash
root@d9adb7cb91eb:/# cd /var/www/html/
root@d9adb7cb91eb:/var/www/html# ls -l
total 4
-rwxr-xr-x 1 root root 219 Feb 22 19:59 index.html
root@d9adb7cb91eb:/var/www/html# 
```

Данный файл оказался именно по этому пути в разультате инструкции `COPY ./site/ /var/www/html/` внутри **DockerFile**.

## Библиография

- [What is docker run -it flag?](https://stackoverflow.com/questions/48368411/what-is-docker-run-it-flag)
