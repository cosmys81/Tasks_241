## **1. Raid массивы, что такое какие бывают.**

**RAID** (Redundant Array of Independent Disks) — это технология объединения нескольких физических дисков в один логический массив для повышения производительности, отказоустойчивости или ёмкости. Основные уровни RAID:

- **RAID 0** (Striping): данные распределяются по всем дискам без избыточности. Увеличивает скорость и ёмкость, но данные теряются при выходе из строя одного диска.
- **RAID 1** (Mirroring): данные дублируются на все диски. Обеспечивает высокую отказоустойчивость, но использует половину доступной ёмкости.
- **RAID 5**: распределение данных с использованием контрольных сумм (parity). Требует минимум 3 дисков. Обеспечивает баланс между производительностью, ёмкостью и отказоустойчивостью.
- **RAID 6**: то же, что и RAID 5, но с дополнительной избыточностью (два блока контрольных сумм). Требует минимум 4 дисков.
- **RAID 10** (или 1+0): комбинация RAID 1 и RAID 0. Использует зеркалирование и распределение данных. Требует минимум 4 дисков.

## **2. Добавьте в виртуальную машину 2 диска, отформатируйте их в ext4.**

```bash
dd if=/dev/zero of=/var/tmp/disk1.img bs=1M count=1024
dd if=/dev/zero of=/var/tmp/disk2.img bs=1M count=1024
```

```bash
sudo losetup /dev/loop1 /var/tmp/disk1.img sudo losetup /dev/loop2 /var/tmp/disk2.img
```

```bash
lsblk
```

Вывод:

```bash
NAME    MAJ:MIN    RM     SIZE    RO    TYPE    MOUNTPOINTS
loop1   7:1        0      1G      0     loop
loop2   7:2        0      1G      0     loop
sda     8:0        0      30G     0     disk
|-sda1  8:1        0      127M    0     part    [SWAP]
|-sda2  8:2        0      29,9Gb  0     part    /home
                                                /
sdb     8:16       0      5G      0     disk
sr0     11:0       1      1024M   0     rom
```

```bash
mkfs.ext4 /dev/loop1
mkfs.ext4 /dev/loop2
```

Вывод:

```bash
mk2fs 1.46.2 (28-Feb-2021)
Found a dos partition table in /dev/loop1
Proceed anyway? (y,N) y
Discarding device blocks: done
Creating filesystem with 262144 4k blocks and 65536 inodes
Filesystem UUID: 6fcdbb13-8a6e-43bb-8e22-8d123ae4d161
Superblock backups stored on blocks:
			32768, 98304, 163840, 229376

Allocating group tables: done
Writing inode tables: done
Create journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done
```

## **3. Создайте из них raid 0 массив.**

```bash
mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/loop1 /dev/loop2
```

Вывод:

```bash
mdadm: /dev/loop1 appears to contain an ex2fs file system
	   size=1048576K  mtime=Thu Jan  1 04:00:00 1970
Continue creating array? (y/n) y
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
```

---

```bash
mkfs.ext4 /dev/md0
```

Вывод:

```bash
mk2fs 1.46.2 (28-Feb-2021)
Discarding device blocks: done
Creating filesystem with 523264 4k blocks and 130816 inodes
Filesystem UUID: 93421c83-9f8f-4e37-8b59-22a3783f2111
Superblock backups stored on blocks:
			32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Create journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done
```

---

```bash
mkdir /mnt/raid0
mount /dev/md0 /mnt/raid0
```

## **4. Проверьте, всё ли работает.**

```bash
df -h
cat /proc/mdstat
```

Вывод:

```bash
Файловая система Размер Использовано Дост Использовано% Смонтировано в
udevfs           5,0М            96К 5,0М            2% /dev
runfs            2,0G           1,1М 2,0G            1% /run
/dev/sda2         30G            24G 5,5G           82% /
tmpfs            2,0G              0 2,0G            0% /dev/shm
/dev/sda2         30G            24G 5,5G           82% /home
tmpfs            392М            64К 392М            1% /run/user/500
/dev/md0         2,0G            24К 1,9М            1% /mnt/raid0

Personalities : [raid0]
md0 : active raid0 loop2[1] loop1[0]
      2093056 blocks super 1.2 512K chunks

unused devices: <none>
```

## **5. Удалите raid0 и создайте raid1**.

```bash
umount /mnt/raid0
mdadm --stop /dev/md0
```

```bash
mdadm --remove /dev/md0
```

```bash
wipefs --all /dev/loop1
wipefs --all /dev/loop2
```

```bash
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/loop1 /dev/loop2
```

```bash
cat /proc/mdstat
mdadm --detail /dev/md0
lsblk
```

Вывод:

```bash
Personalities : [raid0] [raid1]
md0 : active raid1 loop2[1] loop1[0]
      1046528 blocks super 1.2 [2/2] [UU]

unused devices: <none>

/dev/md0:
			Version : 1.2
	  Creation Time : Fri Dec 20 05:30:39 2024
		 Raid Level : raid1
		 Array Size : 1046528 (1022.00 MiB 1071.64 MB)
	  Used Dev Size : 1046528 (1022.00 MiB 1071.64 MB)
	   Raid Devices : 2
      Total Devices : 2
	    Persistence : Superblock is persistence

		Update Time : Fri Dec 20 05:30:45 2024
		      State : clean
	 Active Devices : 2
	Working Devices : 2
	 Failed Devices : 0
	  Spare Devices : 0

Consistency Policy: resync

			 Name : vbox:0  (local to host vbox)
			 UUID : 50901431:2f08e6d1:6f2d0566:fdf51e8b
		   Events : 17

	Number    Major    Minor    RaidDevice    State    
	   0        7        1          0         active sync    /dev/loop1
	   1        7        2          1         active sync    /dev/loop2

NAME    MAJ:MIN    RM     SIZE    RO    TYPE    MOUNTPOINTS
loop1   7:1        0      1G      0     loop
|-md0   9:0        0      1022M   0     raid1
loop2   7:2        0      1G      0     loop
|-md0   9:0        0      1022M   0     raid1
sda     8:0        0      30G     0     disk
|-sda1  8:1        0      127M    0     part    [SWAP]
|-sda2  8:2        0      29,9Gb  0     part    /home
                                                /
sdb     8:16       0      5G      0     disk
sr0     11:0       1      1024M   0     rom
```

## **6. В чём между ними разница?**

- **RAID 0**:
    
    - Плюсы: высокая скорость записи и чтения, увеличение доступного объёма.
    - Минусы: при выходе из строя одного диска данные теряются.
- **RAID 1**:
    
    - Плюсы: высокая отказоустойчивость (данные сохраняются, если один диск выйдет из строя).
    - Минусы: используется только половина общего объёма дисков.

## **7. Есть ли файловые системы, которые поддерживают raid массивы без стороннего ПО.**

Некоторые файловые системы имеют встроенную поддержку RAID:

- **ZFS**: поддерживает зеркалирование, RAID-Z (аналог RAID 5/6).
- **Btrfs**: поддерживает RAID 0, 1, 10 на уровне файловой системы.

## **8. Можно ли создать raid массив во время установки системы?**

Да, многие дистрибутивы Linux (например, Ubuntu Server, Debian) позволяют создавать RAID массивы во время установки:

1. На этапе разметки дисков выберите опцию "RAID" или "Software RAID".
2. Настройте RAID уровень и добавьте устройства.
3. Продолжите установку с использованием созданного массива.
