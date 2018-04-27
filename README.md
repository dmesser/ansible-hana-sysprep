# ansible-hana-sysprep

## Release History

This git repository will have the following  branches as follows:
- master:  current ongoing development, documentation may not be correct
- master, tag: v1.0.1 : fully tested release **recommended to use**
- stable:  fully tested releases incl. documentation, can do scale-up only

### latest changes

- 27-Apr-18: removed all local roles and moved to ansible galaxy (http://galaxy.ansible.com/mk-ansible-roles)
- 06-Mar-18: removed disk-partining and base-role-setup
- 06-Mar-18: created release tag v1.0.1 before restructuring the repository
- 24-Nov-17: base-host-setup role is deprecated. This setup will move to my project ansible-generic-roles and more of it to rhel-system-roles/linux-system-roles project

### Possible deployment options

#### hana scale-up
- one instance
- multi instance installation (not multi-container)
- hana scale-up system-replication (one and multiinstance)

#### hana scale-out
- one instance
- multi instance installation (not multi-container)
- hana scale-out system-replication (one and multiinstance)

This playbook can be used with Command Line (ansible-playbook), Ansible Tower and CloudForms 4.5 and later

Follow the documentation on https://www.ansible.com to setup your ansible environment or ansible-tower server.
Follow the instructions on https://access.redhat.com/documentation/en/red-hat-cloudforms/ for CloudForms

### Use with Tower
There is a playbook designed for use with tower. It calls the roles "precofigure, deployment, hsr"
In tower project setup chose "hana_tower.yml" as a  playbook

### Getting started
In order to use this playbook, you need a brand new RHEL6.x (x<=7) or RHEL 7.x (x<=2) installation.

You can download this ansible playbook by the following command:
<pre>
$ git clone https://github.com/rhmk/ansible-hana-sysprep.git
Initialized empty Git repository in /root/ansible-hana-sysprep/.git/
remote: Counting objects: 181, done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 181 (delta 2), reused 0 (delta 0), pack-reused 163
Receiving objects: 100% (181/181), 146.76 KiB, done.
Resolving deltas: 100% (64/64), done.
</pre>

### Variable Configuration

Follow the instructions in the wiki to configure your ansible variables to use this playbook.
The wiki is located here https://github.com/rhmk/ansible-hana-sysprep/wiki

## Running the Playbook from commandline

Here is a short Howto on running this playbook from commandline

1. Configure your inventory with your servers (default: `/etc/ansible/hosts`), e.g.
```
   [...]

   [hana-servers]
   10.32.111.3 ansible_user=ansible-remote

   [...]
```
  `ansible_user defines` a specific username to use for the ssh connection

2. Make sure your ansible connection to the target host is working
```
   $ ansible -m setup [-k connection_user password ] [-K root password] -l hana-servers
   10.32.111.3 | SUCCESS => {
   "ansible_facts": {
        [...]
          },
       "changed": false
   }
```
See `man ansible` for password options. You could also use ssh-keys and sudo without password

3. If your connection works you can kick-off the playbook
```
$ ansible-playbook -vvv  -b -e @examples/var.yaml -l hana-servers hana_sysprep.yml

  -vvv enables debugging output
  -b : switches execution to root
  -e @examples/var.yaml points to the file with the defined variables
  -l hana-servers limits the execution to hana-servers as defined in /etc/ansible/hosts or a particular hostname
  hana_sysprep.yml: Playbook that defines the actual role
```


### TODO ###
- act depending on HANA versions (different required libraries)
- tasks in hana-host and preconfigure are currently redundant, cleanup required
- host/DNS setup is not checked appropriately
