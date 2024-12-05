# Мини-инструкция как установить Vagrant на Debian 12 с отключенной графикой

### Установка VirtualBox

1. Заходим в BIOS и отключаем secure boot
<details>
<summary>
2. Добавляем стандартные репозитории Debian12 (если вы их не добавили ранее)
</summary>
  
```
cat > /etc/apt/sources.list <<EOF 

#deb cdrom:[Debian GNU/Linux 12.5.0 _Bookworm_ - Official amd64 DVD Binary-1 with firmware 20240210-11:28]/ bookworm contrib main non-free-firmware
#
# From https://wiki.debian.org/SourcesList
#
deb http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware
deb-src http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware
#
deb http://deb.debian.org/debian-security/ bookworm-security main contrib non-free non-free-firmware
deb-src http://deb.debian.org/debian-security/ bookworm-security main contrib non-free non-free-firmware
#
deb http://deb.debian.org/debian bookworm-updates main contrib non-free non-free-firmware
deb-src http://deb.debian.org/debian bookworm-updates main contrib non-free non-free-firmware
#
EOF
```

</details>

<details>
<summary>
3. Устанавливаем VirtualBox
</summary>
  
```
apt-get update && apt install -y gpg wget
```

```
cd /tmp && wget -O- https://www.virtualbox.org/download/oracle_vbox_2016.asc | gpg --yes --output /usr/share/keyrings/oracle-virtualbox-2016.gpg --dearmor
```

```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/oracle-virtualbox-2016.gpg] https://download.virtualbox.org/virtualbox/debian bookworm contrib" > /etc/apt/sources.list.d/oracle-virtualbox.list
```

```
apt-get update -y && sudo apt upgrade -y && apt full-upgrade -y
```

```
apt install -y build-essential dkms rsync virtualbox-7.1 
```

</details>

<details>
<summary>
4. Проверяем верию/ошибки VirtualBox:
</summary>

```
VBoxManage --version 
```

</details>

<details>
<summary>
Если видим ошибки то проверяем выключили ли мы secure boot в Bios компьютера, если выключили, то можно попробовать переустановить Virtual Box / установить другую версию
</summary>
    
Удаляем VirtualBox:
  
```
apt-get purge virtualbox-\* 
```

Cмотрим доступные текущие версии VirtualBox:

```
apt install virtualbox 
```

Как пример, ставим версию virtualbox-6.1: 

```
apt install -y virtualbox-6.1 
```

</summary>
</details>

<details>

<summary>
5. Ставим Guest Additionals:
</summary>

Качаем Guest Additionals в папку /tmp:

```
wget -O /tmp/Oracle_VirtualBox_Extension_Pack-7.1.4.vbox-extpack 'https://download.virtualbox.org/virtualbox/7.1.4/Oracle_VirtualBox_Extension_Pack-7.1.4.vbox-extpack' 
```

Если нужна другая версия, то ищем ее тут: `https://www.virtualbox.org/wiki/Downloads`

Заходим на наш Linux через программу **Mobaxterm** и запускаем Virtualbox в графическом окне, набрав команду: `/usr/bin/virtualbox`  
Либо переводим Linux в графический режим набрав команду: `init 5` затем набираем `/usr/bin/virtualbox`  
   
В окне настроек VirtualBox выбираем: **Extension** => в открывшемся окне выбираем скачаный пакет  
(в нашем примере /tmp/Oracle_VirtualBox_Extension_Pack-7.1.4.vbox-extpack) и устанавливаем его.
</details>
   
На этом, установка VirtualBox закончена

---

### Установка Vagrant
   - Важно! Vagrant заблокирован на территории РФ - вам потребуется использовать зарубежный VPN/Proxy либо качать все вручную
   - Дальнейшая инструкция предполагает что зарубежный VPN/Proxy у вас есть и включен на данной виртуальной машине, либо на роутере 

<details>

<summary>
1. Скачаем и установим Vagrant:
</summary>

```
wget -O /tmp/vagrant_2.4.3-1_amd64.deb 'https://releases.hashicorp.com/vagrant/2.4.3/vagrant_2.4.3-1_amd64.deb'
```

```
dpkg -i vagrant_2.4.3-1_amd64.deb && rm vagrant_2.4.3-1_amd64.deb 
```

   - примечание: Данная версия Vagrant актуальна на 13.11.2024. Вы може найти актуальную версию по ссылке: `https://releases.hashicorp.com/vagrant`
   - если у вас нет зарубежного VPN на данной машине, то скачайте и дистрибутив вручную и положите его на данную машину,   
затем установите командой: `dpkg -i`
</details>
   
На этом, установка Vagrant закончена

---
### Кратко о том как работать с Vagrant:

   - Vagrant хранит скачанные box-файлы (образы ОС) в домашнюю папку пользователя по пути: **~/.vagrant.d/boxes/**
   - Vagrant будет хранить настройки созданных виртуальных машин/доп. диски, в папке, откуда он будет запущен
   - Обратите внимание на то, что бы в папке было достаточно свободного места
   - Vagrrant не будет скачивать образы из РФ (необходим иностранный VPN)

### Основные команды для работы с Vagrant:

`vagrant init` 	                          Создает новый Vagrantfile в текущем каталоге. Этот файл содержит настройки для виртуальной машины   
`vagrant up`	                            Запускает и настраивает виртуальную машину согласно настройкам в Vagrantfile. Если машина еще не создана, она будет создана автоматически   
`vagrant up --debug`                      То же самое, но в режиме отладки   
`vagrant halt`	                          Останавливает виртуальную машину, но не удаляет ее   
`vagrant destroy`	                        Удаляет виртуальную машину и освобождает используемое ею пространство на диске   
`vagrant destroy -f` флаг `-f`            для принудительного удаления без подтверждения   
`vagrant reload`	                        Перезагружает виртуальную машину, применяя любые изменения, которые были сделаны в Vagrantfile   
`vagrant ssh`	                            Подключается к виртуальной машине через SSH   
`vagrant status`	                        Показывает текущее состояние виртуальной машины (например, запущена она или остановлена)   
`vagrant box add`	                        Добавляет новую "коробку" (образ) в локальный список доступных базовых образов Vagrant   
`vagrant box list`                        Показывает список всех установленных в локальной системе "коробок"   
`vagrant box remove`                      Удаляет "коробку" из локального списка доступных образов (пример: vagrant box remove hashicorp/bionic64)   
`vagrant provision`                       Запускает скрипты конфигурации (например, shell-скрипты или Ansible) для настройки виртуальной машины, если она уже запущена   
`vagrant snapshot save my_snapshot`       Управляет моментальными снимками состояния виртуальной машины, позволяя сохранить и восстановить состояние системы   
`vagrant snapshot restore my_snapshot` 	  Восстановить из снапшота   
`vagrant plugin install vagrant-vbguest`	Управляет плагинами Vagrant. Можно устанавливать, обновлять и удалять плагины   
`vagrant plugin list`			                Список установленных плагинов   
`vagrant global-status`			              Показывает информацию о всех виртуальных машинах, управляемых Vagrant, на вашем компьютере

