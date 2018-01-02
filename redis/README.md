Steps playbook performs

- Download Redis Tarball Package
- Download Dependencies
- Building Package
- Starting Redis as a service in Server

yml files Involved

download.yml :
- Download Redis tarball file
- Extract the downloaded Package
- Create user and group if not exist
- Changing file Permissions
depends.yml :
- Update Repository
- Install dependencies ( build-essentials and tcl8.5 )
service.yml :
- Copying Redis service script to server
- Copying Redis configuration file
- Enabling Redis service in default run levels
- To start Redis

Variables :

redis_url = Download url of redis
redis_install_dir = Install directory of Redis
redis_home_dir = Home directory of Redis
redis_user = owner user of Redis Installation Directory
redis_group = owner group of Redis Installation Directory
redis_port = Running port
redis_config_dir = Configuration file directory of Redis 
redis_data_dir = Data directory of Redis
daemonize = Redis to run as daemon(yes or no)
pidfile = Redis PID file location
loglevel = Setting loglevel flag (degub|verbose|notice|warninig)
logfile = Logfile location
protected_mode = To enable or disable authentication (yes or no)
bind_address = Bind IP’s For Redis running in server

——————————————————— test.yml

---
- name: Redis installation
  hosts: redis
  remote_user: ubuntu
  become: yes
  become_method: sudo 
  roles:
    - ansible-role-redis

————————————————————
