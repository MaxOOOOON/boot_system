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
