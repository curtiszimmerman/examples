# Getting started with `fli` on Ubuntu 16.04 with Ansible

## How to

### Prerequisites

- You will need SSH access to one Ubuntu 16.04 server. You should use a non-root user account and that account should have permission to run commands as root, using sudo . You can set this up by following this [initial server setup tutorial](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04), including step 4.
- Also make sure to `sudo apt-get install python` because ansible needs python 2.7 on Ubuntu 16.04

### Install Ansible on a Control Host

#### Mac OSX control host

Install Ansible on Mac OSX control machine. 

`$ brew install ansible`

#### Other OS control host
For other distrobutions, check here: [Getting Ansible](http://docs.ansible.com/ansible/intro_installation.html#getting-ansible)

#### Configure and Run `fli`

`$ git clone https://github.com/ClusterHQ/examples`
`$ cd examples/fli-ansble`

`$ export ANSIBLE_HOST_KEY_CHECKING=False`

`$ vi ansible-hosts # change <your-servers-ip>`

> Note: you only need `--private-key` if you need a key to login as `root` to your servers.

Install (fli) on your ansible machine(s)

```
$ ansible-playbook --private-key ~/your/key -i ${PWD}/ansible-hosts  fli-playbook.yml
PLAY ***************************************************************************

TASK [setup] *******************************************************************
ok: [host1]

TASK [install docker] **********************************************************
ok: [host1]

TASK [install zfs utilities] ***************************************************
ok: [host1]

TASK [prepare zpool] ***********************************************************
changed: [host1]

TASK [create alias] ************************************************************
changed: [host1]

TASK [create alias for logged in root users] ***********************************
changed: [host1]

TASK [place fli binary] ********************************************************
changed: [host1]

PLAY RECAP *********************************************************************
host1                      : ok=7    changed=4    unreachable=0    failed=0   
```
 
Run `fli` via ansible.

```
$ ansible servers -i ${PWD}/ansible-hosts --private-key ~/your/key -a fli
host1 | SUCCESS | rc=0 >>

$ ansible servers -i ${PWD}/ansible-hosts --private-key ~/your/key -a 'fli setup -z chq'
host1 | SUCCESS | rc=0 >>

$ ansible servers -i ${PWD}/ansible-hosts --private-key ~/your/key -a 'fli config -t /root/fh.token'
host1 | SUCCESS | rc=0 >>

$ ansible servers -i ${PWD}/ansible-hosts --private-key ~/your/key -a 'fli init myVolumeset'
host1 | SUCCESS | rc=0 >>
a52cef4c-157d-4eac-b0e1-0640cf5a1c48

$ ansible servers -i ${PWD}/ansible-hosts --private-key ~/your/key -a 'fli list -a'
host1 | SUCCESS | rc=0 >>
VOLUMESET ID  CREATOR CREATED         SIZE DESCRIPTION ATTRIBUTES NAME
a52cef4c-1c48 -       Oct 24 19:54:21 0    -           -          myVolumeset
```

Or login to your server as root and you can now use fli.

```
$ ssh -i ~/your/key root@<ip-of-server>
server:$ fli setup -z chq
server:$ fli config -t /root/fh.token
server:$ fli init myVolumeset
server:$ fli list -a
VOLUMESET ID  CREATOR CREATED         SIZE DESCRIPTION ATTRIBUTES NAME
a52cef4c-1c48 -       Oct 24 19:54:21 0    -           -          myVolumeset
```


