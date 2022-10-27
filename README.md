# OID basic configuration   
**Before running any ansible script pull the newest version of git repository as described [here](#markdown-header-git-pull)**

## Table of Contents

- [Clone/Pull/Push git repository](#markdown-header-git)
- [Prepare ansible environment](#markdown-header-ansible)
- [Run ansible playbooks](#markdown-header-ansible-playbooks)


## Clone/Pull/Push git repository <a name="markdown-header-git"></a>
### Clone git repository
To clone git repository:
1. Log in to the Ansible DEV machine: **ol03nur279.vm.exa.bamf.in.bund.de** 
2. In your home location create new directory, e.g. *oam_ansible* and run clone command

## Pull git repository <a name="markdown-header-git-pull"></a>
To pull git repository:
1. Log in to the Ansible DEV machine: **ol03nur279.vm.exa.bamf.in.bund.de** 
2. Go to the directory where you previously cloned the git repository, e.g. ~/oam_ansible/idm-basic-configuration and run pull command

```bash
cd ~/oam_ansible/idm-basic-configuration
git -c http.sslVerify=false pull
```

## Push git repository
To push git repository:
1. Log in to the Ansible DEV machine: **ol03nur279.vm.exa.bamf.in.bund.de** 
2. Go to the directory where you previously cloned the git repository, e.g. ~/oam_ansible/idm-basic-configuration and run push commands

```bash
cd ~/oam_ansible/idm-basic-configuration
git add --all
git commit -m "Changes description"
git -c http.sslVerify=false push -u origin develop
```
```bash
cd 
mkdir oam_ansible
cd oam_ansible
git -c http.sslVerify=false clone -b develop 
```

## Prepare ansible environment <a name="markdown-header-ansible"></a>  
1. Log in to the Ansible DEV machine: **ol03nur279.vm.exa.bamf.in.bund.de** 
2. Make sure you have your public key added for the oid Servers for the Oracle user
3. Copy the private key (in pem format) to your home directory and change permission to 600
4. Go to the directory where you previously cloned the git repository, e.g. ~/oam_ansible/idm-basic-configuration/
5. Go to oid directory
5. Configure *vault.yml* and *vault_secret.sh* files for the proper environment you want to configure.  

   **DEVELOPMENT Environment** 
   
   **Setup local environment**
   
    - Copy the key from your laptop to your user home directory
    - Convert the key to pem format (If they are ppk format generated from puttygen)
    - Change the permission for the copied key
	
	```bash
	puttygen yourkey.ppk -O private-openssh -o yourkey.pem
	chmod 600 <CONVERTED-KEY-NAME-TO-PEM>
	```
   
   **Configure vault_secret.sh**

    - Copy *vault_secret_template.sh* to *vault_secret.sh*.
    - Edit *vault_secret.sh* file and add password which will be used to encrypt *vault.yml* file: *password=[Password_for_vault_file]*
    
    ```bash
    cp ~/oam_ansible/idm-basic-configuration/oid/environments/dev/inventories/vault_secret_template.sh ~/oam_ansible/idm-basic-configuration/oid/environments/dev/inventories/vault_secret.sh
    vi ~/oam_ansible/idm-basic-configuration/oid/environments/dev/inventories/vault_secret.sh
	```

   **Configure vault.yml**

    
    - Copy content of the *oam_ansible/idm-basic-configuration/oam/environments/dev/inventories/vault_template.yml* to your notepad and update the parameters values
    - Create vault file
    
    ```bash
    ansible-vault create --vault-id dev@~/oam_ansible/idm-basic-configuration/oid/environments/dev/inventories/vault_secret.sh ~/oam_ansible/idm-basic-configuration/oid/environments/dev/inventories/vault.yml
	```
	The text editor would open, copy the content updated in notepad on your laptop to editor opened in your ansible node. Save the file post updating the content.

    - Edit vault file

    ```bash
    ansible-vault create --vault-id dev@~/oam_ansible/idm-basic-configuration/oid/environments/dev/inventories/vault_secret.sh ~/oam_ansible/idm-basic-configuration/oid/environments/dev/inventories/vault.yml
	```

	Key in the password for the vault generated earlier. (Password updated in file vault_secret.sh)
	
## Run ansible playbooks <a name="markdown-header-ansible-playbooks"></a>

### Deploy configuration to OID

To deploy configuration to the OID you should use *play_oid_config.yml* ansible script.  

1. Go to the *~/oam_ansible/idm-basic-configuration/oid* and edit *play_oam_config.yml*
2. Choose which roles you want to use and uncomment them. Other roles which shouldn't be used during deployment need to be commented.
3. Define on which environment you want to deploy the configuration. It is defined in "-i" parameter:
   - DEV environment: *environments/dev/hosts*
   - TEST environment: *environments/test/hosts*
   - ABN environment: *environments/abn/hosts*
   - PROD environment: *environments/prod/hosts*
4. Define which *vault.yml / vault_secret.sh* files you want to use. It is defined in "--vault-id" parameter:
   - DEV environment: *--vault-id dev@environments/dev/inventories/vault_secret.sh --vault-id dev-oauth@environments/dev/inventories/vault_oauth_secret.sh*
   - TEST environment: *--vault-id test@environments/test/inventories/vault_secret.sh --vault-id test-oauth@environments/test/inventories/vault_oauth_secret.sh*
   - ABN environment: *--vault-id abn@environments/abn/inventories/vault_secret.sh --vault-id abn-oauth@environments/abn/inventories/vault_oauth_secret.sh*
   - PROD environment: *--vault-id prod@environments/prod/inventories/vault_secret.sh --vault-id prod-oauth@environments/prod/inventories/vault_oauth_secret.sh*
5. Run ansible script as follows:
 Example for DEV:
    ```bash
    ansible-playbook play_oid_config.yml --vault-id dev@environments/dev/inventories/vault_secret.sh --vault-id deeoid@environments/dev/inventories/vault_secret.sh -i environments/dev/hosts
    ```
