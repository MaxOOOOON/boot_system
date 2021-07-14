# boot_system
Задания:  
1. Попасть в систему без пароля несколькими способами  
2. Установить систему с LVM, после чего переименовать VG  
3. Добавить модуль в initrd  

1:
Необходимо зайти в режим редактора в меню grub.  
В конце строки, которая начинается с kernel вставить rd.break.  
Комбинация Ctrl + X  
mount -o remount,rw /sysroot/  - добавить права на запись для /sysroot    
chroot /sysroot  
passwd  - изменить пароль пользователя root  
touch /.autorelabel  - необходим для SELinux context  
exit  
logout  
Если систему перезагрузить принудительно, то изменения не сохранятся.

2:  
Переименовать volume group cl_centos8  
![](https://github.com/MaxOOOOON/boot_system/blob/main/1.png)  

vgrename VolGroup00 OtusRoot  
![](https://github.com/MaxOOOOON/boot_system/blob/main/2.png)  
Заменить старое название на новое в следующих файлах:  
/etc/fstab  
![](https://github.com/MaxOOOOON/boot_system/blob/main/3.png)  

/etc/default/grub  
![](https://github.com/MaxOOOOON/boot_system/blob/main/4.png)  

/boot/grub2/grub.cfg  
![](https://github.com/MaxOOOOON/boot_system/blob/main/5.png)  

Пересоздать initrd image
mkinitrd -f -v /boot/initramfs-$(uname -r).img $(uname -r)

выполнить для корректной загрузки
grub2-mkconfig -o /boot/grub2/grub.cfg  

3:
Создать папку в директории mkdir /usr/lib/dracut/modules.d/01test  
Поместить нужные скрипты (https://gist.github.com/lalbrekht/e51b2580b47bb5a150bd1a002f16ae85/raw/80060b7b300e193c187bbcda4d8fdf0e1c066af9/gistfile1.txt и https://gist.githubusercontent.com/lalbrekht/ac45d7a6c6856baea348e64fac43faf0/raw/69598efd5c603df310097b52019dc979e2cb342d/gistfile1.txt)  
Выполнить mkinitrd -f -v /boot/initramfs-$(uname -r).img $(uname -r) или  
dracut -f -v  
При загрузке отобразится пингвин, если в редакторе grub конфига удалить rghb и quiet в конце строки kernel
![](https://github.com/MaxOOOOON/boot_system/blob/main/6.png)  
