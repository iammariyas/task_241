# **Шарим**
1. **Установите пакет samba**
	Samba - стандартный набор программ для взаимодействия с Windows для Linux и Unix.
	
	Samba - важный компонент для беспрепятственной интеграции Linux/Unix-серверов и настольных компьютеров в среду Active Directory. Она может функционировать как в качестве контроллера домена, так и в качестве обычного члена домена.
	
	устанавливаем samba:
	```
	[maria@maria ~]# sudo apt-get install samba
	```
	
2. **ЧТо такое побщая папка, зачем оно может быть нужно?**
	общая папка - это сетевой ресурс для совместного доступа к файлам и каталогам между пользователями или устройствами в локальной сети. Она обеспечивает удобный обмен файлами без копирования
	
3. **Создайте общую папку без пароля с правами только на чтение файлов**
	создаем папку и дописываем в `smb.conf`, что общая папка только для чтения и есть гостевой режим:
	
	```
	[maria@maria ~]$ sudo mkdir -p /tmp/readonly
	[maria@maria ~]$ sudo nano /etc/samba/smb.conf
	```
	открывается текстовый редактор nano и дописываем в `smb.conf`:
	
	```
	[readonly]
        comment = Read Only Files
        path = /tmp/readonly
        read only = yes
        guest ok = yes
        browsable = yes
	```
	клиент видит шару:
	

	запишем че-нибудь в файл, чтобы потом проверить на другом устройстве, что мы можем подключиться:
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108200619.png)
	
	Подключаемся с другого устройства, находящемся в той же сети, что подключились, можем прочитать, что написано в файле, но записать в него ниче не можем:

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108203025.png)

4. **Создайте общую папку с паролем с правами на чтение и запись**
	 создаю новую папку:
	`[maria@maria ~]$ mkdir -p /tmp/rw`
	затем создаю нового пользователя и задаю ему пароль: 
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108202445.png)
	
	пишу в `smb.conf` следующее: 
	```
	[rwshare]
        comment = Read/Write Share
        path = /tmp/rw
        valid users = smbuser
        read only = no
        writable = yes
        guest ok = no
        browsable = yes
        create mask = 0664
        directory mask = 0775
	```
	
	видим, что самба видит общую папку:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108203954.png)
	
	передаю права на папку пользователю `smbuser`:  
	```
	[maria@maria ~]$ sudo chown smbuser:smbuser /tmp/rw/
	```
	теперь подключаемся через другое устройство и смотрим, что все успешно:

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108213918.png)
	
	здесь видим, что smbuser может создавать папочки, загружать или записывать локальные файлы на samba-сервер, может смотреть, что находится в этом файле.
	
5. **Создайте общую папку с доступом для какой-то группы с полными правами**
	
	создаем группу и пользователей в ней:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108205127.png)
	
	далее редактирую `smb.conf`, добавляю в него шару для новой группы:
	
	```
	[groupdev]
        comment = Developers Group Share
        path = /tmp/groupdev
        valid users = @developers
        read only = no
        writable = yes
        guest ok = no
        brownsable = yes
        force group = developers
	```
	все ок, общая папка видна:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108205539.png)
	
	теперь нужно проверить работоспособность при подключении к другому устройству. будем проверять и пользователя dev1 и dev2, что они работают и с их помощью можно создавать файлы и читать их:
	
	**dev1:**

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108214315.png)
	
	все работает. dev1 может загружать и записывать файлы локального файла на самба сервер, может создавать папки, может читать, что находится в файле.
	
	**dev2:**

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108214332.png)

	здесь все то же самое, что и с dev1
	
6. **Создайте общую папку в которой у одной группы будет полный доступ, а у другой только доступ на чтение. Третья группа не должна иметь к ней доступа**
	
	создаем группы и пользователей:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108211008.png)
	
	далее устанавливаем пароли для samba:
	
	```
	[maria@maria ~]$ sudo smbpasswd -a user1
	New SMB password:
	Retype new SMB password:
	Added user user1.
	[maria@maria ~]$ sudo smbpasswd -a user2
	New SMB password:
	Retype new SMB password:
	Added user user2.
	[maria@maria ~]$ sudo smbpasswd -a user3
	New SMB password:
	Retype new SMB password:
	Added user user3.
	```
	
	создаем папку:
	
	`[maria@maria ~]$ sudo mkdir -p /tmp/secure`

	далее пишу в `/etc/samba/smb.conf`:
	```
	[secure]
        comment = Secure Group Share
        path = /tmp/secure
        valid users = @group1, @group2
        read list = @group2
        write list = @group1
        admin users = @group1
        read only = no
        guest ok = no
        browsable = yes
	```
	проверяем, что создалось и все работает:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108212014.png)
	
	теперь подключаемся с другого устройства и проверяем работоспособность:
	**user1:**

	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108214544.png)
	
	**user2**:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108214623.png)
	
	**user3:**
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108214641.png)
	
	**что мы видим**: у user1, который относится к группе, где все права есть, он может добавлять в эту папку файлы и записывать в них, user2, который относится к группе только для чтения может читать, что написано в файле, но не может создавать папки, файлы, не может ничего записывать, а user3 даже не может зайти, что в целом-то логично, потому что у него нет никаких прав.
___

как я подключалась к другому устройству? в VirtualBox сделала тип подключения "Сетевой мост", через `ifconfig` получила ip виртуалки, далее потом подключаюсь с другого устройства по этому ip (на скринах видно).

![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%2020260108210257.png)
___
P.S. на всех лабах, кроме этой и по сетям, отличается имя компьютера. Везде был host-15 (я очень прошу меня за такое простить), но она у меня слетела в какой-то момент и я пересоздала новую и дала имя компьютера maria. я надеюсь на понимание и что у вас не возникнет подозрений, что делала лабы не я. на этой лабе раньше тоже был host-15, но переделывала эту лабу с проверкой, что у меня все подключается уже после того, как создала новую виртуалку