# **Журнальчики**

1. **Посмотрите журналы ssh**
	`journalctl -u sshd` - так можно посмотреть журналы ssh
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104185520.png)
2. **Выведите журналы в реальном времени**
	 `journalctl -f` - выводит журналы в реальном времени
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104185909.png)
3. **Выведите лог в реальном времени для службы sshd**
	`journalctl -u sshd -f` показывает все действия sshd в реальном времени и все новые подключения/отключения и неудачные попытки входа
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104190320.png)
4. **Можно ли без комады journalctl прочитать логи systemd?**
	Можно. только логи systemd хранятся в бинарном формате.
	`strings /var/log/journal/*/system.journal`
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104190641.png)
5. **Сколько будет 2-2?**
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104191018.png)
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104191118.png)
	