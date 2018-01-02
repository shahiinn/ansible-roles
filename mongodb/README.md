Steps playbook performs

- Installing MongoDB from repository
- Creating Directories
- Configuring MongoDB
- Replication of mongoDB

yml files Involved

install.yml :

- Adding apt repository key
- Adding repository
- Install MongoDB
- Start MongoDB

pre-configure.yml :

- Creating Database Directory
- Creating Log Directory

configure.yml :

- Copying mongodb configuration file to server
- Restart MongoDB

replication.yml

- Editing MongoDB configuration file to enable replication
- Restart MongoDB
- Starting the server in replication mode and adding the replica nodes
- making primary node permanent

Variables :

KEYSERVER = keysever for adding as repository

KEY_ID = ID of the keyserver repository

MONGODB_REPO = mongoDB repository to be added to repo list

APT_PACKAGE = MongoDB apt-package name

MONGODB_USER = owner user of MongoDB data and log directory

MONGODB_GROUP = owner group of MongoDB data and log directory

MONGODB_DB_PATH = MongoDB data directory

MONGODB_LOG_PATH = MongoDB log directory

MONGODB_LOG_FILE = MongoDB log file name

REPLICA_SET_NAME = MongoDB Replica set name for replication

PORT = MongoDB running port

BIND_IP = IP address’s to be added as Bind IP

REPLICATION_MODE = flag for enabling replication(true or false)

--------
test.yml
--------

---

- name: MongoDB installation

  hosts: primary,replica
  
  remote_user: ubuntu
  
  become: yes
  
  become_method: sudo
  
  roles:
    - ansible-role-mongodb
