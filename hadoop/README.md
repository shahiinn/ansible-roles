# HADOOP-HDFS SETUP USING ANSIBLE

## SUPPORTED PLATFORMS:

    - Ubuntu-14.04
    - Ubuntu-16.04
  
## VARIABLES DEFINED IN ROLE


### *Variables in defaults/main.yml*

> Specify the hadoop install mode as single | cluster | datanode

    > single   --> For single node HDFS setup
    > cluster  --> For HDFS cluster setup
    > datanode --> For add new datanode to existing HDFS cluster

**hadoop_install_mode:** 'cluster'

> Enter the hadoop-hdfs replication factor

**hadoop_replication_factor:** "{{ replication_factor }}"

> Enter the required block size

> You can use the following suffix (case insensitive): 
```
- k(kilo) 
- m(mega) 
- g(giga)
- t(tera)
- p(peta) 
- e(exa) -  to specify the size (such as 128k, 512m, 1g, etc.), 
- Or provide complete size in bytes (such as 134217728 for 128 MB).
```
**hadoop_block_size:** "{{ block_size }}"


> Specify the directory to download and install hadoop

**hadoop_install_dir:** "{{ install_dir }}"

> Enter the user and group name to configure hadoop

**hadoop_user:** "{{ user }}"

**hadoop_group:** "{{ group }}"

> Specify the data directory

**hadoop_store_dir:** "{{ data_dir }}"

>Enter hadoop version to download. For 2.6.0 -> major-2, minor-6, patch-0

**hadoop_version_major:** "{{ version_major }}"

**hadoop_version_minor:** "{{ version_minor }}"

**hadoop_version_patch:** "{{ version_patch }}"

> THIS IS DEFUALT VARIABLES DONT CHANGE UNNECESSARILY

> Following variable value will get automatically at runtime.

**hadoop_namenode:** "{{ hostvars[groups[namenode_group][0]]['ansible_fqdn'] }}"

> Mention keyfile name to create ssh key

**hadoop_keyfile:** id_rsa

> Enter the location in ansible server to store key

**hadoop_keylocation:** /tmp

> Haddop URL to download

**hadoop_version:** "hadoop-{{ hadoop_version_major }}.{{hadoop_version_minor }}.{{ hadoop_version_patch }}"

**hadoop_url_prefix:** "https://archive.apache.org/dist/hadoop/core"

**hadoop_url:** "{{ hadoop_url_prefix }}/{{ hadoop_version }}/{{ hadoop_version }}.tar.gz"

**hadoop_home_dir:** "{{ hadoop_install_dir }}/{{ hadoop_version }}"


## TO INSTALL HADOOP AS CLUSTER

1. Copy the role in your role directory
2. Change the install_mode variable as 'cluster' test/test.yml like follows:
   **install_mode: 'cluster'**
3. To setup a Hadoop-HDFS cluster Host file should be follows:
```
[namenode]
NN-1
    
[datanode]
DN-1
DN-2
.
.
DN-n
    
[hadoop:children]
namenode
datanode
```
4. Sample test.yml file
```
---
- name: Hadoop {{ hadoop_install_mode }} installation
  gather_facts: yes
  hosts: hadoop,newdatanode
  remote_user: ubuntu
  become: yes
  become_method: sudo
  vars:
    install_mode: 'cluster'
    replication_factor: 3
    block_size: 512m
    install_dir: /data
    user: ubuntu
    group: ubuntu
    data_dir: /data/hadoop_store
    version_major: 2
    version_minor: 7
    version_patch: 3

    namenode_group: namenode
    newdatanode_group: newdatanode
    hadoop_parent: hadoop
  roles:
    - java-8
    - hadoop-test
```
5.Execute the role
```ansible-playbook test.yml```

## TO INSTALL HADOOP IN SINGLE NODE

1. Add the server IP under namenode group in hosts file.
2. Change the variable 'install_mode in test/test.yml
   install_mode: 'single'
3. Mention the single IP under in namenode group in host file
```
[namenode]
NN-IP
```
4. Execute the roles like follows:
```ansible-playbook test.yml```
   

## TO ADD NEW DATANODE INTO EXISTING HDFS CLUSTER

>It's possible to add new datanodes into existing hdfs cluster without restarting the namenode.
1. Change the hadoop_install_mode in var/main.yml file
   hadoop_install_mode: 'datanode'
2. Add the new datanode IP's under newdatanode group in hosts file and also add under existing datanode group.

    Example: Edit the host file like follows:
    ```
    [namenode]
    NN-IP
        
    [datanode]
    DN-1 --> Existing datanode
    DN-2 --> Existing datanode
    DN-3 --> New datanode IP
        
    [newdatanode]
    DN-3 -->New datanode IP
        
    [hadoop:children]
    namenode
    datanode
    ```
    
 3. Execute the role like follows:
 ```ansible-playbook test.yml```
    

:+1::ok_hand:
