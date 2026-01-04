# Открываем firewald

1. **Удалите iptables и установите firewalld**
	удаляем: `sudo apt-get remove iptables`
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)

	устанавливаем  firewalld

	```
	[student@S-vm-223 ~]$ sudo apt-get install firewalld
	```

2. **Попробуйте также проверить возможность подключения по ssh**
	я подключилась, никаких проблем не возникло

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)

3. **Если её нет то откройте порт**
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)

4. **Выведите список открытых портов с помощью firewall-cmd**

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)

5. **Можно ли там добавить порты по названию сервиса?**
	Да, можно и даже нужно добавлять порты по названи. сервисов. Это намного удобнее, чем потом указывать номера портов вручную.
	
	Можно посмотреть полный список встроенных сервисов:

	```
	firewall-cmd --get-services
	```

	вместо порта можно добавить сервис в зону

	```
	firewall-cmd --permanent --add-service=ssh 
	firewall-cmd --permanent --add-service=http
	firewall-cmd --reload
	```
	
6. **На вашей Локальной виртуальной машине попробуйте подключиться к серверу samba из предыдущих заданий**

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)

7. **Если не получилось то откройте нужные порты**

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)

8. **Сделайте так чтобы изменения были постоянными**

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	


