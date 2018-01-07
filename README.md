# OCP installation on AWS


This script install a single master, 2 node ocp instance on aws

- create instances master and nodes, currently 1 master 2 nodes, tags them with clusterID, cluster ID is hardcoded, haven't found a way to make tag key dynamic

- install prerequisites (based on 3.7)

- installs OCP

 This setup looks for a vault file under vars/secrets.yml (not in this repo for obvious reasons). The values required  (RHN ID and password) are outlined in the comments in vars/vars.yml
 - The password for the vault is maintained in another file which you have to provide
 - To run the playbook

	```
	export ANSIBLE_HOST_KEY_CHECKING=False
	export AWS_SECRET_ACCESS_KEY=<accesskey>
	export AWS_ACCESS_KEY_ID=<secret key id>
	export JUMP_HOST=<ip of jump host>


	ansible-playbook -i hosts site.yml -e "jump_server_ip=$JUMP_HOST"  --vault-password-file ./pw.txt -v 

	```



 - To retry the playbook, need to unregister the system first or you can skip the registration task

 	```
	ansible-playbook -i hosts site.yml --tags "unsub" -e "jump_server_ip=$JUMP_HOST"  --vault-password-file ./pw.txt -v 

	ansible-playbook -i hosts site.yml --skip-tags "rhn-sub" -e "jump_server_ip=$JUMP_HOST"  --vault-password-file ./pw.txt -v 

 	```
