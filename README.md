# Ansible linux authentication roles for Active Directory or LDAP

* Supports Red Hat family (eg, RHEL, AlmaLinux, Fedora) and Debian family (eg Debian, Ubuntu) machines. It configures SSSD authentication.

* Includes roles to manage ***pam_access*** and ***sudo*** to limit access.

* Includes vagrant virtualbox configuration for testing. The vagrant configuration starts and configures two Alpine linux machines to act as Samba Active Directory Domain Controllers.

## Tested against (included in Vagrantfile)

- RedHat 7 (using CentOS 7)
- RedHat 8 (using AlmaLinux 8)
- RedHat 9 (using AlmaLinux 9)
- Ubuntu 18
- Ubuntu 20
- Ubuntu 22
- Debian 10
- Debian 11
- Debian 12

## Usage

The vagrant sample requires dc1, dc2 is optional, then whatever client OS

```
vagrant up dc1 rh9 deb12 ubu22
./runansible auth.yml
```

In the example auth.yml provided, entering an AD Join name and password will use the auth_ad role:
```
Enter AD Join Username: Administrator
Enter AD Join Password: Passw0rd
```

In the example auth.yml provided, NOT entering an AD Join name and password will use the auth_ldap role:
```
Enter AD Join Username:
Enter AD Join Password:
```

A quick test is to use 'su' to an AD account:
```
$ vagrant ssh rh9
Last login: Tue Jul 18 16:43:09 2023 from 10.0.2.2
[vagrant@rh9 ~]$ su - 1001A
Password:
Creating home directory for 1001a.
[1001a@rh9 ~]$
```

On RedHat machines, pam_access affects 'su'. The example setup restricts access to the group 'linuxusers' which does not include the account '1002B'. The default configuration of pam_access on Debian machines, pam_access is only included in sshd and login:
```
[vagrant@rh9 ~]$ su - 1002B
Password:
su: Permission denied
```


## Accounts in Samba AD (Passw0rd)

- Anonymous (LDAP only)
- Administrator
- ldappxy
- sysadm
- user1
- user2
- user3
- user4
- 1001A
- 1002B
- 20001
- 20002

## Groups in Samba AD

- sysadmins: sysadm
- linuxusers: user1, user2, 1001A, 20001
- internals: 1001A, 1002B
- externals: 20001, 20002

