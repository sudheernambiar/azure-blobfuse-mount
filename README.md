# azure-blobfuse-mount
## Prerequisites 
- A Linux machine(CentOS 7.9/RHEL7) with internet access for mounting the blob. A blob account, container with access key

## Procedure Steps
### Install blobfuse 
rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm
yum clean all
yum install blobfuse -y

### Create a caching location.
```
mkdir /mnt/tmp
```
### make authentication settings.
```
vi /root/fuse_connection.cfg
````
#### Information needed to be add in will be as follows.
#accountName myaccount
#accountKey storageaccesskey
#containerName mycontainer
```
accountName myblob_direct
accountKey UBgNyWj33erpOOJlgdNgfw3eA4aB1e3qaELWIEEqNma0rd3jAmBUFHl/+CXbxzjjloofo+fBvbxjfNsOTK144==
containerName myblob
```
#### Change permission for the file, 
```
chmod 600 ~/fuse_connection.cfg
```
### Create a local directory to mount in.
```
mkdir /blob_loc
```
### Mount format.
Mount temporary, till reboot -
> blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120

### Mount command in this case, (temp mount)
```
blobfuse /blob_loc --tmp-path=/mnt/tmp  --config-file=/root/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```
### Permanent mount
```
blobfuse /blob_loc       fuse      defaults,_netdev,--tmp-path=/mnt/tmp,--config-file=/root/fuse_connection.cfg,--log-level=LOG_DEBUG,allow_other 0 0
mount -a
```
### Result
Now blob will be mounted in as /blob_loc
