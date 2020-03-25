# NFS Server Install and Configuration


ssh as root to your NFS Server host
Run (hint: put below in a script):

```
apt-get update
apt-get install nfs-kernel-server
mkdir -p /nfs/shared/volume1
mkdir -p /nfs/shared/volume2
chmod -R 777 /nfs/shared/volume1
chmod -R 777 /nfs/shared/volume2
chown nobody:nogroup /nfs/shared/volume1
chown nobody:nogroup /nfs/shared/volume2
```

`vi /etc/exports`
edit and add following entries for each client (provide worker IP addresses) and file system (volume1 and volume2)

```
/nfs/shared/volume1 worker1-ip(rw,sync,no_subtree_check,insecure,no_root_squash)
/nfs/shared/volume1 worker2-ip(rw,sync,no_subtree_check,insecure,no_root_squash)
/nfs/shared/volume1 worker3-ip(rw,sync,no_subtree_check,insecure,no_root_squash)


/nfs/shared/volume2 worker1-ip(rw,sync,no_subtree_check,insecure,no_root_squash)
/nfs/shared/volume2 worker2-ip(rw,sync,no_subtree_check,insecure,no_root_squash)
/nfs/shared/volume2 worker3-ip(rw,sync,no_subtree_check,insecure,no_root_squash)
```

Run:
`systemctl restart nfs-kernel-server`


# NFS Client Install and Configuration


ssh as root to each worker node
Run (hint: put below in a script):
*Replace nfs-server-IP with your NFS Server host ip below

```
apt-get update 
apt-get install nfs-common
mkdir -p /nfs/shared/volume1
mkdir -p /nfs/shared/volume2
chmod -R 777 /nfs/shared/volume1
chmod -R 777 /nfs/shared/volume2
mount nfs-server-IP:/nfs/shared/volume1 /nfs/shared/volume1
mount nfs-server-IP:/nfs/shared/volume2 /nfs/shared/volume2
```

`vi /etc/fstab`
add following entries and replace nfs-server-IP with your NFS Server host address.

```
nfs-server-IP:/nfs/shared/volume1 /nfs/shared/volume1 nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
nfs-server-IP:/nfs/shared/volume2 /nfs/shared/volume2 nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
```

You can verify mounts with the command 'df -h'

```
nfs-server-IP:/nfs/shared/volume1  382G   52G  330G  14% /nfs/shared/volume1
nfs-server-IP:/nfs/shared/volume2    382G   52G  330G  14% /nfs/shared/volume2
```
