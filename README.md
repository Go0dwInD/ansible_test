## Ansible Playbook: Nginx+Docker
Данный playbook устанавливает Nginx и Docker на вашу систему под управление Ubuntu

## Требования 
Система Ubuntu 18.04, все необходимые пакеты будут установлены автоматически.

## Основне переменные 
hosts 
```yaml
[ubuntu]
host0 ansible_ssh_host=192.168.33.10  ansible_ssh_user=vagrant ansible_ssh_port=22  
# host0 Имя хоста
# ansible_ssh_host адрес хоста
# ansible_ssh_user пользователь под которым будет проходить авторизация
# ansible_ssh_port ssh порт хоста

[all:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_ssh_private_key_file=/home/go0dwind/my_vgrant_prog_1/.vagrant/machines/host0/virtualbox/private_key  
# ansible_ssh_private_key_file Путь до вашего privet ключа в моём случае это ключ от vagrant
domain_name=example.com  
# domain_name Доменное имя вашей машины 
```
## Описание возможностей
Данный playbook устанавлет Nginx в комплекте с статическим конентом file/index.html, если статический контент отсутствует
то происходит редирект на сайт google.com, таже данный playbook устанавливает Docker последней стабильной версии(19.03.12) и настраивает его демон

deamon_config.json.j2
```yaml
{
  "storage-driver": "overlay2",
  "log-opts": {
    "max-size": "512m",
    "max-file":"{{ ansible_mounts|json_query('[?mount == `/`].size_available')| first // 1024**2 // 512 // 2}}",  # (размер рутового раздела / 512) / 2
    "labels": "somelabel",
    "env": "os,customer"
  }
}
```
