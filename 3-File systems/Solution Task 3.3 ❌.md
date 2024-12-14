## **1. Выведите содержимое fstab. Что хранится в fstab?**

```bash
cat /etc/fstab
```

Вывод:

```bash
# /etc/fstab: static file system information.  
#  
# Use 'blkid' to print the universally unique identifier for a device; this may  
# be used with UUID= as a more robust way to name devices that works even if  
# disks are added and removed. See fstab(5).  
#  
# <file system>             <mount point>  <type>  <options>  <dump>  <pass>  
UUID=4D0E-7223                            /boot/efi      vfat    defaults   0 2  
UUID=7996e106-9f03-461c-b959-8638ad1a8422 /              ext4    defaults   0 1  
/swapfile                                 swap           swap    defaults   0 0
```

## **2. Добавьте в виртуальную машину ещё один диск.**

Готово. Результат увидим в следующем пункте.

## **3. Узнайте как система видит ваш диск - выведите информацию о блочных устройствах.**

```bash
lsblk
```

Вывод:

```bash

```

## **4. С помощью полученной информации создайте на диске таблицу разделов и файловую систему ext4.**

## **5. Примонитируйте диск в каталог /mnt.**
## **6. Зайдите в каталог и создайте там файлы.**

## **7. Отмонтируйте диск и проверьте, остались ли файлы.**

## **8. Сделайте так, что бы диск автоматически подключался при загрузке систем ( добавьте информацию о нём с fstab).**

## **9. Проверьте корректность записанных в fstab данных перед перезагрузкой.**

## **10. Перезагрузите систему и убедитесь что диск был подключён к системе.**