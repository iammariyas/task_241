# Юниты

1. **Что такое systemd юнит?**
	Systemd - это init процесс, который запускается при загрузке ядра Линукса. Systemd запускает и контролирует все системные службы. В ее основе лежит понятие **units**, которые связаны между собой и имеют определенное имя и тип с соответствующими конфигурационными файлами, то есть это файлы конфигурации хранящие информацию о сервисе, сокете, устройстве и т.д.
	
2. **Проверьте статус любого systemd юнита, какую информацию выводит эта команда?**
	Это делается с помощью команды 
	```
	systemctl status <имя_юнита>
	```
	`systemctl status` - если прописать эту команду выведется вообще все юниты, которые есть
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104160206.png)
	
3. **Попробуйте остановить сервис.**
	`systemctl stop <сервис>`
	Чтобы проверить статус, что сервис остановлен, нужно ввести команду `systemctl is-active NetworkManager.service`, должно вывести `inactive`
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104160531.png)
4. **Перезапустите его.**
	Чтобы перезапустить сервис, нужно ввести команду 
	```
	systemctl restart <сервис>
	```
	В нашем случае будет:
	```
	[root@host-15 ~]# systemctl restart NetworkManager.service
	```
	Затем проверяем, что он активен:
	```
	[root@host-15 ~]# systemctl is-active NetworkManager.service
	```
	Выведется `active`
5. **УДалите из автозагрузки**
	Чтобы удалить из автозагрузки, нужно ввести команду:
	`systemctl disable <сервис>`
	Чтобы проверить, что сервис удален из автозагрузки, можно ввести команду `systemctl is-enabled <сервис>` и должно вывестись `disabled`
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104161351.png)
6. **Верните обратно**
	Здесь, в целом, аналогично, только вместо `disable` используется `enable`. Вот так выглядит следующая команда:
	
	`systemctl enable <сервис>`
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104161533.png)
7. **Что такое таймеры?**
	timer - это механизм активации других единиц по таймеру, предоставляющий функциональность, аналогичную cron: запуск процессов или задач в заданные промежутки времени, то есть по таймеру.
