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
Переименовать volume group  
vgrename VolGroup00 OtusRoot
https://github.com/MaxOOOOON/boot_system/blob/main/1.png
Заменить старое название на новое в следующих файлах:  
/etc/fstab, /etc/default/grub, /boot/grub2/grub.cfg  

выполнить для корректной загрузки
grub2-mkconfig -o /boot/grub2/grub.cfg  
Пересоздать initrd image
mkinitrd -f -v /boot/initramfs-$(uname -r).img $(uname -r)

3:
