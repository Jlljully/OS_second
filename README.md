# Домашнее задание к занятию «Операционные системы. Лекция 2»


1. На лекции вы познакомились с [node_exporter](https://github.com/prometheus/node_exporter/releases). В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой [unit-файл](https://www.freedesktop.org/software/systemd/man/systemd.service.html) для node_exporter:

    * поместите его в автозагрузку;
    * предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на `systemctl cat cron`);
    * удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

### Ответ

![Скрин](https://github.com/Jlljully/OS_second/blob/main/Screenshot_2.png "1")

![Скрин](https://github.com/Jlljully/OS_second/blob/main/Screenshot_4.png "2")

**Стоп-старт работают без проблем**

![Скрин](https://github.com/Jlljully/OS_second/blob/main/Screenshot_5.png "3")

![Скрин](https://github.com/Jlljully/OS_second/blob/main/Screenshot_6.png "4")

2. Изучите опции node_exporter и вывод `/metrics` по умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.

### Ответ

**node_scrape_collector_duration_seconds{collector="cpu"} 0.000342195  
node_scrape_collector_duration_seconds{collector="cpufreq"} 4.7937e-05  
node_scrape_collector_duration_seconds{collector="diskstats"} 0.00032759  
node_scrape_collector_duration_seconds{collector="meminfo"} 0.000134224  
node_scrape_collector_duration_seconds{collector="netstat"} 0.001132181**  

3. Установите в свою виртуальную машину [Netdata](https://github.com/netdata/netdata). Воспользуйтесь [готовыми пакетами](https://packagecloud.io/netdata/netdata/install) для установки (`sudo apt install -y netdata`). 
   
   После успешной установки:
   
    * в конфигурационном файле `/etc/netdata/netdata.conf` в секции [web] замените значение с localhost на `bind to = 0.0.0.0`;
    * добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте `vagrant reload`:

    ```bash
    config.vm.network "forwarded_port", guest: 19999, host: 19999
    ```

    После успешной перезагрузки в браузере на своём ПК (не в виртуальной машине) вы должны суметь зайти на `localhost:19999`. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata, и с комментариями, которые даны к этим метрикам.

### Ответ

![Скрин](https://github.com/Jlljully/OS_second/blob/main/Screenshot_7.png "5")

4. Можно ли по выводу `dmesg` понять, осознаёт ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?

### Ответ

**Да, можно:**

![Скрин](https://github.com/Jlljully/OS_second/blob/main/Screenshot_8.png "dmesg")

5. Как настроен sysctl `fs.nr_open` на системе по умолчанию? Определите, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (`ulimit --help`)?

### Ответ

**Это - хардлимит на количество дескрипторов (запущенных файлов). Есть еще софтлимит, посмотреть его можно uname -n**

![Скрин](https://github.com/Jlljully/OS_second/blob/main/Screenshot_10.png "fs.nr_open")

![Скрин](https://github.com/Jlljully/OS_second/blob/main/Screenshot_11.png "fs.nr_open")

![Скрин](https://github.com/Jlljully/OS_second/blob/main/Screenshot_12.png "fs.nr_open")

6. Запустите любой долгоживущий процесс (не `ls`, который отработает мгновенно, а, например, `sleep 1h`) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через `nsenter`. Для простоты работайте в этом задании под root (`sudo -i`). Под обычным пользователем требуются дополнительные опции (`--map-root-user`) и т. д.

### Ответ

![Скрин](https://github.com/Jlljully/OS_second/blob/main/Screenshot_13.png "dmesg")

7. Найдите информацию о том, что такое `:(){ :|:& };:`. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (**это важно, поведение в других ОС не проверялось**). Некоторое время всё будет плохо, после чего (спустя минуты) — ОС должна стабилизироваться. Вызов `dmesg` расскажет, какой механизм помог автоматической стабилизации.  
Как настроен этот механизм по умолчанию, и как изменить число процессов, которое можно создать в сессии?

*В качестве решения отправьте ответы на вопросы и опишите, как эти ответы были получены.*


### Ответ


