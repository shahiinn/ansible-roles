Steps playbook performs

- choose wheater to install from repository or tarball
- Adding Service Scripts if it is tarball Installation
- Starting ActiceMQ in Server

yml files Involved

apt.yml : 

- Repository Installation of activemq in server

download.yml :

- Download ActiveMQ tarball file
- Extract the downloaded Package
- Create user and group if not exist
- Changing file Permissions

service.yml :

- Coping ActiveMQ service script to server
- Enabling activemq service in default run levels

start.yml :

- To start ActiveMQ

Variables :

amq_install_mode = mode of installation(tarball or apt)

amq_user = owner user of Installation directory

amq_group = owner group of Installation directory

amq_port = running port

amq_install_dir = Installation directory

amq_home_dir = ActiveMQ home directory

amq_skip_checksum = true or false

amq_url_prefix = URL of download space

amq_version_major = Major version no

amq_version_minor = Minor version no

amq_version_patch = Patch version np

amq_version = Exact ActiveMQ version

amq_checksums = ActiveMQ checksum (List out all supported checksums here)

amq_service = If you want amq as service give 'yes'



---------------------test.yml---------------------

---

- name: Active-MQ installation

  hosts: amq
  
  remote_user: ubuntu
  
  become: yes
  
  become_method: sudo
  
  roles:
    - java-8
    - amq

--------------------------------------------------
