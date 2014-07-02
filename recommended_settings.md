Cassandra Recommended Configuration Options
==========================================

The following are the settings carried out by the ansible playbooks install_cassandra.yml and install_java.yml.

Modify cassandra startup script
-------------------------------
Starting Cassandra as a service fails, because the the service script at '/etc/init.d/cassandra' attempts to change the ulimits. The method the files attempts is not possible in Ubuntu 14.04, even for root. To be able to start as a service these lines must be commented out. The recommended startup settings seem to do the same thing, setting ulimit -l to unlimited and ulimit -n to 100000, but by modifying other files. This can be accomplished differently way depending on how Cassandra was installed.


Set limits
----------
If installing from a package the changes must be made in '/etc/security/limits.d/cassandra.conf':

```
cassandra - memlock unlimited
cassandra - nofile 100000
cassandra - nproc 32768
cassandra - as unlimited
```

If installing from a tarball the changes must be made in '/etc/security/limits.conf':

```
* - memlock unlimited
* - nofile 100000
* - nproc 32768
* - as unlimited

root - memlock unlimited
root - nofile 100000
root - nproc 32768
root - as unlimited
```

Modify the max map count
------------------------
Increase the max_map_count kernel parameter to avoid running out of mapped areas. This is accomplished by adding the following to '/etc/sysctl.conf':

```
vm.max_map_count = 131072
```

To make the changes take effect, reboot the server or run the following command:

```
sudo sysctl -p
```

Decrease the heap size
----------------------
Set the Cassandra heap size in '/etc/cassandra/cassadra-env.sh':

```
MAX_HEAP_SIZE="750M"
HEAP_NEWSIZE="250M"
```

Disable swap
------------
This prevents the Java Virtual Machine (JVM) from responding poorly because it is buried in swap and ensures that the OS OutOfMemory (OOM) killer does not kill Cassandra.

```
sudo swapoff --all
```

To make this change permanent, remove all swap file entries from /etc/fstab.


Optimum blockdev --setra settings for RAID
------------------------------------------
Typically, a readhead of 128 is recommended, especially on Amazon EC2 RAID0 devices.

Check to ensure setra is not set to 65536:

```
sudo blockdev --report /dev/<device>
```

To set setra:

```
sudo blockdev --setra 128 /dev/<device>
```

Modify cassandra.yaml
---------------------

For an AWS SC2 instance, set listen_address to the private IP address.

```
listen_address: <private ip>
```

For an AWS S2 instance, set rpc_address to 0.0.0.0 address.

```
rpc_address: 0.0.0.0
```

For an AWS S2 instance, set broadcast_address to the public IP address.

```
broadcast_address: <public ip>
```

For authentication, change the authenticator to PasswordAuthenticator:

```
authenticator: PasswordAuthenticator
```

If using cqengine, the python cassandra driver, and AWS use Ec2MultiRegionSnitch rather than Ec2Snitch:

```
endpoint_snitch: Ec2MultiRegionSnitch
```

To setup backups, set incremental_backups to true to have Cassandra create a hard link to each SSTable flushed or streamed locally in a backups/ subdirectory of the keyspace data. Removing these links is the operator's responsibility.

```
incremental_backups: true
```
