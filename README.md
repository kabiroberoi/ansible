# OID basic configuration   
**Before running any ansible script pull the newest version of git repository as described [here](#markdown-header-git-pull)**

## Table of Contents

- [Clone/Pull/Push git repository](#markdown-header-git)



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
