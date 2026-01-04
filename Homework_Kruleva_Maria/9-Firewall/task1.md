
# Открываем iptables

1. **Установите iptables**

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)

2. **Проверьте осталась ли возможность подключения по ssh к вашему серверу**
	Да, возможность осталась

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	
3. **Почему может пропасть такая возможность?**
	Сама по себе установка iptables не блокирует ssh, но правила по умолчанию могут это сделать.
	Причины могут быть следующие:
	1. Политика DROP для INPUT без правила для порта 223
	2. Отсутствие разрешения  ESTABLISHED соединений
	3. Нет правила `ACCEPT tcp dport 223`
4. **Откройте нужный порт на сервере чтобы восстановить подключение**
	Запускаем iptables с правами root, вставляем правило №1 в цепочку INPUT, разрешаем все TCP-соединения на порт 223

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)

5. **Это будет udp или tcp прот?**
	tcp, поскольку ssh использует tcp 

# Сохраняем

6. **Сохраняются ли записанные вами правила после перезагрузки?**
	нет, правила iptables не сохраняются после перезагрузки.
	
7. **Как их сохранить?**
	`sudo iptables-save`

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)

	Еще есть вариант установить сервис:
	
	```
	sudo apt install iptables-services
	sudo service iptables save
	sudo systemctl enable iptables
	```

При работе с firewall не рекомендую отключаться от текущей сессии ssh. Лучше подключаться из другой консольки.