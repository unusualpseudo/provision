# Setting up an External Drive in Ubuntu for NFS with ZFS

RAID0 using an external 2.5T HDD drive :

```sh
fdisk -l
# below cmd also formats the disks with the ZFS file system + change block size to 4k
# get disk sector size fdisk -l /dev/sda | grep "Sector size"
zpool create -o ashift=12 -f nas /dev/sda
zfs set atime=off nas
zfs set compression=lz4 nas
zfs create nas/Apps
zfs create nas/Apps/MinioIO
zfs create nas/Apps/Longhorn
zfs create nas/Apps/Media
zfs set sharenfs="no_subtree_check,all_squash,anonuid=1000,anongid=1000,rw=@192.168.1.0/24" \
nas/Apps/MinioIO
zfs set sharenfs="no_subtree_check,all_squash,anonuid=1000,anongid=1000,rw=@192.168.1.0/24" \
nas/Apps/Longhorn
zfs set sharenfs="no_subtree_check,all_squash,anonuid=1000,anongid=1000,rw=@192.168.1.0/24" \
nas/Apps/Media
chown 1000:1000 -R /nas/Apps/Longhorn
chmod 777 -R /nas/Apps/Longhorn
chown 1000:1000 -R /nas/Apps/MinioIO
chmod 777 -R /nas/Apps/MinioIO
chmod 777 -R /nas/Apps/Media
chown 1000:1000 -R /nas/Apps/Media
chmod 777 -R /nas/Apps/Media
# check ZFS pool status
zpool status
```

Great tutorial on ZFS [Link](https://arstechnica.com/information-technology/2020/05/zfs-101-understanding-zfs-storage-and-performance/)

[Reference](https://github.com/onedr0p/home-ops/blob/main/docs/src/notes/nas.md)
