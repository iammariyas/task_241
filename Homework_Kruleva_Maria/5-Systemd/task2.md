# **Пишем юниты**

1. **Создайте скрипт который создаёт папку заполняет её файлами ( имена 1-4 ) и записывает в них информацию о текущей дате, версии ядра, имени компьютера и списе всех файлов в домашнем каталоге пользователя от которого выполняется скрипт( не забудьте сдлеать проверку на существование файлов и папок)**
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104163136.png)
	
	Теперь подробное описание скрипта:
	создаю переменную DIR с путем ~/myinfo. В моем случае, поскольку я работаю под root, то путь будет /root/myinfo
	
	Далее создаю каталог:
	проверяю не существует ли эта директория и создаю ее, если ее нет.
	```
	if [ ! -d "$DIR" ]; then
    mkdir -p "$DIR"
	fi
	```
	
	Далее прописала функцию для записи в файл
	Сама функция принимает 2 аргумента: $1 - это путь к файлу, $2 - содержимое файла, `local` делает переменными локальными для функции.
	```
	echo "$content" > "$file"
	```
	Эта строка моего скрипта записывает содержимое в файл или перезаписывает в случае, если он существует
	
	Ну а затем делаю запись системной информации, которая требуется в условии:
	в первый файл будет записан файл с текущей датой
	во второй файл будет записана версия ядра линукса
	в третьем имя моего компьютера
	а в четвертом будет список всех файлов домашней директории. теперь покажу все выводы:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104164115.png)
	
2. **Создайте юнит который будет вызывать этот скрипт при запуске. Проверьте**
	Создадим следующий файл: 
	`nano /etc/systemd/system/myinfo.service` и пропишем в нем скрипт:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104181525.png)
	
	Запустим:
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104181436.png)
3. **Создайте таймер который будет вызывать выполнение одноимённого systemd юнита каждые 5 минут.**
	Создадим следующий файл: 
	`nano /etc/systemd/system/myinfo.timer` и пропишем следующий скрипт:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104182500.png)
	Активируем и проверим статус таймера и список всех таймеров:
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104182004.png)
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104182104.png)
	
4. **От какого пользователя вызыаются юниты поумолчанию?**
	по умолчанию systemd юниты вызываются от пользователя root.
5. **Создайте пользователя от имени которого будет выполняться ваш скрипт.**
	Создаем нового пользователя scriptuser
	```
	useradd -m -s /bin/bash scriptuser
	```
	Устанавливаем пароль 
	```
	passwd scriptuser
	```
	Копируем скрипт в домашний каталог:
	```
	[root@host-15 ~]# cp /root/script.sh /home/scriptuser/script.sh 
	[root@host-15 ~]# chown scriptuser:scriptuser /home/scriptuser/script.sh 
	[root@host-15 ~]# chmod +x /home/scriptuser/script.sh
	```
	Затем нужно изменить unit-файл для нового пользователя (заменить имя пользователя. Можно было сделать через $(whoami), но у меня почему-то не заработало)
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104184123.png)э
	Активируем и смотрим статус:
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104184302.png)
	
	
1. **Дополните юнит информацией о пользователе от которого должен выполняться скрипт.**
	Добавляем новые параметры для пользователя:
	- **`User=scriptuser`** - **от какого пользователя** запускается скрипт
	- **`Group=scriptuser`** - группа выполнения
	- **`WorkingDirectory=/home/scriptuser`** - рабочая директория скрипта
	
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104184556.png)
	Применяем изменения:
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104184758.png)
1. **Дополните ваш скрипт так, что бы он независимо от местоположения всегда выполнялся в домашней папке того кто его вызывает.**
	![alt text](https://github.com/iammariyas/task_241/blob/labs/Homework_Kruleva_Maria/image/Pasted%20image%20260104185117.png)
	`cd "$HOME"` будет всегда переходить в домашнюю директорию текущего пользователя
	из изменений: сделала компактную проверку существования файла. С условием if проверка мне кажется громоздкой. Так красивее на мой взгляд.
