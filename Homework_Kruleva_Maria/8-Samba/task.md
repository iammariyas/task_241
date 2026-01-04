# **Шарим**
1. **Установите пакет samba**
	Samba - стандартный набор программ для взаимодействия с Windows для Linux и Unix.
	
	Samba - важный компонент для беспрепятственной интеграции Linux/Unix-серверов и настольных компьютеров в среду Active Directory. Она может функционировать как в качестве контроллера домена, так и в качестве обычного члена домена.
	
	устанавливаем samba:
	```
	[maria@host-15 ~]# sudo apt-get install samba
	```
	
2. **ЧТо такое побщая папка, зачем оно может быть нужно?**
	общая папка - это сетевой ресурс для совместного доступа к файлам и каталогам между пользователями или устройствами в локальной сети. Она обеспечивает удобный обмен файлами без копирования
	
3. **Создайте общую папку без пароля с правами только на чтение файлов**
	создаем папку и устанавливаем права доступа:
	устанавливаю chmod 555 потому что он подходит под задачу: только для чтение и даже владелец не может ничего изменить
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	Затем в `etc/samba/smb.conf` дописываю следующие строчки:
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
4. **Создайте общую папку с паролем с правами на чтение и запись**
	создаю папку и нового пользователя. пользователя добавляю в samba, папке устанавливаю соответствующий режим доступа chmod 770 - владелец и группа могут читать, записывать и выполнять, а другие пользователи не имеют никаких прав.
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	задаем пароль:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	
	пишу в `/etc/samba/smb.conf` следующее: 
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
1. **Создайте общую папку с доступом для какой-то группы с полными правами**
	создаем группу и пользователей
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	
	далее создаем папки с правами группы
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	права 2770 - устанавливает права доступа к каталогу или файла. владелец и группа получают полные права, а у других нет прав. первая цифра `2` включает бит SGID (Set Group ID), чтобы все новые файлы и папки, созданные внутри, наследовали группу родительского каталога, что идеально для совместной работы в группе, например, в папке Samba.
	
	далее редактирую /etc/samba/smb.conf:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	
1. **Создайте общую папку в которой у одной группы будет полный доступ, а у другой только доступ на чтение. Третья группа не должна иметь к ней доступа**
	создаем группы и пользователей:
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	далее устанавливаем пароли для samba
	```
	pdbedit -a dev1
	pdbedit -a viewer1
	pdbedit -a outsider1
	```
	создаем папку и устанавливаем права:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	далее пишу в /etc/samba/smb.conf^
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260104232530.png)
	