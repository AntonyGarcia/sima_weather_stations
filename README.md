Para configurar el Arduino Yun se necesita tener la memoria SD insertada en el Arduino Yun.
También se necesita haber configurado la red wifi del Arduino Yun (192.168.240.1).

# Expansión de la memoria del Arduino Yun
Para expandir la memoria del Arduino Yun hay que subir el Archivo expand_memory.ino a la placa con el IDE de Arduino.

Luego de subir el codigo, deben ir a Monitor Serie. Les aparecerá la siguiente instrucción:
```
This sketch will format your micro SD card and use it as additional disk space for your Arduino Yun.

Please ensure you have ONLY your micro SD card plugged in: no pen drives, hard drives or whatever.

Do you wish to proceed (yes/no)?
```
Deben escribir "yes" y presionar Enter.

Luego de eso, les aparecerá la siguiente instrucción:
```
Starting Bridge...
 
Ready to install utility software. Please ensure your Arduino Yun is connected to internet.
Ready to proceed (yes/no)?
```
Deben escribir "yes" y presionar Enter.

Luego de eso iniciará la actualización de software del Yún, para lo cual utilizara la conexión a internet del Arduino Yun.

Una vez finalizada la actualización, les aparecerá la siguiente instrucción:
```
Updating software list...
Software list updated. Installing software (this will take a while)...
e2fsprogs dosfstools fdisk rsync kmod-fs-ext4 installed
 
Proceed with partitioning micro SD card (yes/no)?
```
Deben escribir "yes" y presionar Enter.

Luego de eso se debe especificar el tamaño de la partición de datos, que en este caso será de 4096 MB.

Se escribe 4096 y Enter.

Con eso se debe finalizar con la configuración de la memoria MicroSD.

# Instalación de software
Para instalar el software que necesitamos, hará falta conectarse por medio de SSH con el Arduino Yún. Esto se puede
hacer con cualquier cliente SSH, como Putty o Bitvise.

Una vez conectados por SSH, se debe escribir los siguientes comandos:
```
opkg update
opkg install nano
opkg install openssh-sftp-server
opkg install luci
opkg install uci libuci libuci-lua
opkg install php7
opkg install php7-mod-json
opkg install php7-mod-curl
opkg install php7-mod-sqlite3
opkg install php7-cli
opkg install php7-cgi
uci set uhttpd.main.interpreter=".php=/usr/bin/php-cgi"
uci set uhttpd.main.index_page="index.html index.htm default.html default.htm index.php"
uci commit uhttpd
sed -i 's,doc_root.*,doc_root = "",g' /etc/php.ini
sed -i 's,;short_open_tag = Off,short_open_tag = On,g' /etc/php.ini
/etc/init.d/uhttpd restart
opkg install libsqlite3
opkg install sqlite3-cli
opkg install python3
opkg install python3-pip
opkg install python3-sqlite3
```

Con esto se instalarán todos los paquetes necesarios para el funcionamiento del sistema.