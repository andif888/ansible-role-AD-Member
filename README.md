ansible-role-AD-Member
=========

Joins a linux host to a Active Directory Domain and allows AD users to logon to the linux host


Role Variables
--------------

Available variables are listed below, along with default values (see defaults/main.yml):

```yaml
NTP_SERVERS:
  - { name: '192.168.2.20'}
```
A list of IP addresses of available NTP servers to use for time synchronisation

```yaml
adauth_username: administrator
```
Active Directory username, which is used to join the AD Domain.

```yaml
adauth_password: blah blah
```
Password of the AD account used to join the AD Domain

```yaml
KRB5_DOMAIN_NAME: prianto.com
```
FQDN of the AD Domain

```yaml
KRB5_DOMAIN_SHORTNAME: prianto
```
Netbios Name of the AD Domain

```yaml
KRB5_KDC_SERVERLIST:
  - { name: 'mucdc01.prianto.com' }
  - { name: 'rzdc01.prianto.com' }
```
List of KDC server names

```yaml
SMB_PASSWORD_SERVER: '192.168.2.20'
```
IP address of the server validating passwords

```yaml
AD_GROUP_IN_SUDOERS: 'linux_sudoers'
```
Name of the AD Group which gets added to the sudoers list

```yaml
SSH_ALLOW_GROUPS: 'root linux_sudoers'
```
Space separated list of group names, which are allowed to logon using ssh

ID mapping range of AD user and groups to linux uid and gid.

```yaml
SMB_IDMAP_UID: 10000-20000
SMB_IDMAP_GID: 10000-20000
``` 


### Resolve AD logon issues after UID and GID changes

Stop the Winbind and Samba services:

```console
service winbind stop
service smbd stop
```

Clear the Samba Net cache:

```console
net cache flush
```

Delete the Winbind caches:

```console
rm -f /var/lib/samba/*.tdb
rm -f /var/lib/samba/group_mapping.ldb
```

Start the Samba and then Winbind services - Note: The order is important

```console
service smbd start
service winbind start
``` 


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```yaml
- hosts: localhost  
  remote_user: root  
  roles:
    - ansible-role-AD-Member
```
