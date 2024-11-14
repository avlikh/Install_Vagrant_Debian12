# Мини-инструкция как установить Vagrant на Debian 12 с отключенной графикой

### Установка VirtualBox

1. Заходим в BIOS и отключаем secure boot
<details>
<summary>
2. Добавляем стандартные репозитории Debian12
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
Если видим ошибки то проверяем выключили ли мы secure boot в Bios компьютера, если не забыли, то можно попробовать переустановить Virtual Box
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
   - Дальнейшая инструкция предполагает что зарубежный VPN/Proxy у вас есть и включен на данной виртуальной машине/роутере

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
   - если у вас нет зарубежного VPN на данной машине, то скачайте и дисрибутив вручную и положите его на данную машину,   
затем установите командой: `dpkg -i`
</details>




---


