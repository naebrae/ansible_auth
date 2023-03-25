# Ansible linux authentication roles for Active Directory or LDAP

* Includes vagrant virtualbox configuration for testing. The vagrant configuration starts and configures two Alpine linux machines to act as Samba Active Directory Domain Controllers.

## Usage

The vagrant sample requires dc1, dc2 is optional, then whatever client OS

```
vagrant up dc1 rh9 deb11 ubu22
./runansible auth.yml
```
