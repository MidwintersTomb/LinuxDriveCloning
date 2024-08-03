# Cloning drives with Linux

#### This was originally created to a.) help me remember, as it's been years since I've had to do this regularly, but also, b.) provided a reference guide for my brother.

## Cloning a Windows system

<details>

<summary>Backing up the entire drive</summary>

&nbsp;&nbsp;&nbsp; (For these purposes we will be calling the drive being backed up */dev/sda* and the new drive */dev/sdb*)

1. Boot from Linux live USB

2. Escalate to root

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```sudo su```

3. Identify your drives

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```fdisk -l```

5. Clone the drive

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  5a. To another drive

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```dd if=/dev/sda of=/dev/sdb```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  5b. To a file (**NOTE:** *This requires you to have free space equal to the entire size of the drive*)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  5b1. Browse to where you want to save the file

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  5b2. Clone the drive to file

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```dd if=/dev/sda of=image.img```

</details>

<details>

<summary>Backing up the drive without backing up empty space</summary>

&nbsp;&nbsp;&nbsp; (For these purposes we will be calling the drive being backed up */dev/sda*)

1. Boot from Linux live USB

2. Escalate to root

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```sudo su```

3. Identify your drives

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```fdisk -l```

4. Browse to where you want to save the file

5. Backup the boot sector

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```dd if=/dev/sda of=boot.sector bs=446 count=1```

6. Backup the partition table

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```sfdisk -d /dev/sda > drive.table```

&nbsp;&nbsp;&nbsp; **OR**

&nbsp;&nbsp;&nbsp; 5/6. Backup the boot sector and partition table together

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```dd if=/dev/sda of=boot.table bs=512 count=1```

&nbsp;&nbsp;&nbsp; 7. Backup the partitions

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```ntfsclone --save-image --output - /dev/sda1 | gzip > sda1-backup.img.gz``` (*repeat this for each partition*)

</details>

<details>

<summary>Restoring the entire drive from file</summary>

&nbsp;&nbsp;&nbsp; (For these purposes we will be calling the new drive being restored to */dev/sda*)

1. Boot from Linux live USB

2. Escalate to root

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```sudo su```

3. Identify your drives

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```fdisk -l```

4. Browse to where you saved the file

5. Restore the image

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```dd if=image.img of=/dev/sda```

</details>

<details>

<summary>Restoring the drive from boot sector, partition table, and partition files</summary>

&nbsp;&nbsp;&nbsp; (For these purposes we will be calling the new drive being restored to */dev/sda*)

1. Boot from Linux live USB

2. Escalate to root

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```sudo su```

3. Identify your drives

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```fdisk -l```

4. Browse to where you saved the file

5. Restore the boot sector

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```dd if=boot.sector of=/dev/sda bs=446 count=1```

6. Restore the partition table

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```sfdisk /dev/sda < drive.table```

&nbsp;&nbsp;&nbsp; **OR**

&nbsp;&nbsp;&nbsp; 5/6. Restore the boot sector and partition table together

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```dd if=boot.table of=/dev/sda bs=512 count=1```

&nbsp;&nbsp; 7. Restore the partitions

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```gunzip -c backup.img.gz | ntfsclone --restore-image --overwrite /dev/sda1 -``` (*repeat this for each partition* ***and don't miss the - at the end***)

</details>

---

## Cloning a Linux system

#### Who are we kidding?  If you're using Linux regularly, you probably don't need this.  But, just in case:

<details>

<summary>Backing up the entire drive</summary>

&nbsp;&nbsp;&nbsp; (For these purposes we will be calling the drive being backed up */dev/sda* and the new drive */dev/sdb*)

1. Boot from Linux live USB

2. Escalate to root

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```sudo su```

3. Identify your drives

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```fdisk -l```

5. Clone the drive

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  5a. To another drive

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```dd if=/dev/sda of=/dev/sdb```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  5b. To a file (**NOTE:** *This requires you to have free space equal to the entire size of the drive*)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  5b1. Browse to where you want to save the file

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  5b2. Clone the drive to file

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```dd if=/dev/sda of=image.img```

</details>

<details>

<summary>Restoring the drive from file</summary>

&nbsp;&nbsp;&nbsp; (For these purposes we will be calling the new drive being restored to */dev/sda*)

1. Boot from Linux live USB

2. Escalate to root

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```sudo su```

3. Identify your drives

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```fdisk -l```

4. Browse to where you saved the file

5. Restore the image

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```dd if=image.img of=/dev/sda```

</details>
