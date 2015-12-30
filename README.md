# Ansible deployment for JIRA

## 1. QuickStart

```
ansible-galaxy install jermon.jira
# Deploy to a remote
ansible-playbook jira.yml --extra-vars=/path/to/file_vars

```


## Table of contents

- [1. QuickStart](#1-quickstart)
- [2. Overview](#2-overview)
- [3. Requirements](#3-requirements)
- [4. Usage](#4-usage)
  - [4.1 Playbook Arguments](#41-playbook-arguments)
- [5. After Install](#5-after-install)


## 2. Overview

An Ansible playbook to automate the installation of Atlassian Jira, could posibly be used to keep it updated

The Task performed by the playbook are:

	- install or check if java-1.7.0-openjdk-devel is installed
	- create a group for jira
	- create a user that belong to the group created before
	- Download the standalone tar file given a version
	- extract the tar file
	- create a symlink to an shorter dir name for the extracted dir
	- set the jira-home dir in the jira-application.properties file
	- copy an init script or a service file depending on the EL major release 


## 3. Requirements

Ansible to be installed in the local machine.

Because I'm using RHEL the easiest way to install  is:

```
yum install ansible
```
but as you are already here (github) you can try:

```
git clone git://github.com/ansible/ansible.git --recursive
cd ./ansible
source ./hacking/env-setup
```

## 4. Usage

1. `git clone http://github.com/jermon/ansible-jira` or `ansible-galaxy install jermon.jira`
2. create a vars file in you group_vars dir or host_vars or simply add them directory on your main yml file
3. run the Ansible playbook, the fastest way is:

```
ansible-playbook jira.yml --extra-vars=/path/to/file_var
```

### 4.1 Playbook Arguments

- jira_user_group: group that jira files are going to belong to
- jira_user: user that jira files are going to belong to
- jira_user_home_dir: path to where the sources are going to be, Default: /opt
- jira_home: jira-home path, if it's the first time installation use whereever you like, otherwise use your current
- jira_version: the version check latest in atlassian site
- jira_download_link: link to download the .tar.gz file (note: it will be concatenated with the version var)
- jira_java_home: path to the jvm in to use.

Example:

```
  roles:
    - ansible-jira
    
```

**TODO** use the handler after install to run the application

### 5. After install

You can just ssh to the server(s) (more than one if you are using jira datacenter) and:

for releases version 6 or less [works with 7 as well]
```
service jira start
```

for releases version 7 
```
systemctl start jira.service
```


Enjoy
