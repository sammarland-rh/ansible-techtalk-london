# Howto Guide for the Ansible TechTalk Demo

Once the hertzner setup script has been run to install kvm it is time to isntall the ansible tower machine

Create the Vault password file and stick the password in there

Once Ansible is installed install boto to make the ec2 module work later:

```
/usr/bin/python -m easy_install pip
/usr/bin/python -m pip install boto

```

Now run the provision playbook

```
ansible-playbook create-vms.yml --vault-password-file=vault_pass.txt
```


To keep an eye on the install progress watch the virsh console for the vm

```
virsh console ansible-tower
```


Once the install is done run the following command to allow password ssh to distribute the keys

```
export ANSIBLE_HOST_KEY_CHECKING=False
```


Now run the key distribution playbook

```
ansible-playbook prepare_ssl.yml  -i /root/inventory -k
```


Change the inventory file

```
vim /root/inventory
```

Run the prepare_guest.yml playbook to register the node and install the basic packages

```
ansible-playbook prepare_guests.yml -i /root/inventory --vault-password-file=vault_pass.txt
```

Now its time to install ansible tower

```
ansible-playbook prepare_ansible_tower.yml -i /root/inventory --vault-password-file=vault_pass.txt
```