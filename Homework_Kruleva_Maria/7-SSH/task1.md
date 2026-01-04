# **Настриваем**

1. **Какой по умолчанию используется порт для поключения?**
	Для SSH подключения по умолчанию использует порт 22. для http обычно 80, для https - 443, но это уже про веб-сервисы
2. **Можно ли его изменить? если да то как?**
	да, можно. для этого необходимо отредактировать конфигурационный файл ssh сервера `/etc/ssh/sshd_config`. затем найти строку \#Port 22, раскомментировать и заменить 22 на нужный порт, например, на 223 - порт, по которому мне нужно будет подключиться.
3. **Какая служба отвечает за обработку запросов на подключения по ssh?**
	за это отвечает служба sshd
4. **Какой файл конфигурации отвечает за его настройку?**
	`/etc/ssh/sshd_config`
	в альт линуксе `/etc/openssh/sshd_config`
5. **Попробуйте подключиться по ssh к предоставленному вам серверу**
	юху подключилась
	`ssh -p 223 student@ternar.io`

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104195729.png)

6. **Отредактируйте файл настроек на сервере так, чтобы была возможность подключиться к серверу используя пользователя root**
	Открываем файл настроек на сервере с помощью `nano /etc/openssh/sshd_config` и изменяем строку `#PermitRootLogin prohibit-password` на `PermitRootLogin yes`
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104195729.png)
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104195421.png)
7. **Измените количество ошибок ввода пароля перед сборосом соединения, покажите эти измененения**
	Там же, где мы выполняли пункт 6, меняем строку `MaxAuthTries`
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104200145.png)
	
8. **Создайте пользователя ssh-user и попробуйте им подключиться к серверу**
	создаю пользователя `ssh-user` с домашней папкой и задаю пароль
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104200328.png)
	проверим, что точно создали, а то вдруг нет
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104200512.png)
	теперь подключимся к серверу:
	`ssh -p 223 ssh-user@ternar.io`
	и проверим, что вход успешный 
	`whoami` - выводится `ssh-user`
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104200621.png)
	все супер, мы подключились. ура!!!
9. **Ограничьте ему возможность подключения к серверу**
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104200916.png)
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104201041.png)
10. **Как вы это сделали?**
	Отредактировала `/etc/openssh/sshd_config`. В конец файла добавила 
	`DenyUsers ssh-user`
	и перезапустила ssh
	```
	sudo systemctl restart sshd
	```
	(все представлено на скринах в п.9)
	при подключение к ssh-user получаю ошибку `Permission denied (publickey,password).`
11. **Что хранится в файле known_hosts?**
	этот файл содержит публичные ключи ssh-серверов, к которым ты когда-либо подключался.
	