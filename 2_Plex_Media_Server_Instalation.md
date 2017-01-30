## PLEX MEDIA SERVER INSTALL & SETUP
* Language configuration
	* Proceed to configure the languages(this can be do it by *root* or *pi* user
    ```bash
		$ locale -a
    ```
	* The output should be like this
    ```bash
		 C
		 C.UTF-8
		 en_US.utf8
		 POSIX
    ```
	* IN case that you whant to use spanish subtitles, the output should be like this
    ```bash
		C
		C.UTF-8
		en_US.utf8
		es_AR.utf8
		es_US.utf8
		POSIX
    ```
	* Si en_US.utf8 no esta presente se debe generar.
	* Editar el archivo locale.gen para habilitar en_US.utf8. Descomentar la linea.
		>$ sudo nano /etc/locale.gen
		> # en_US.UTF-8 UTF-8
	* Guardar el archivo con Ctrl+X, Y y ENTER
	* Luego generar los locales
		>$ sudo locale-gen
	* Si se obtiene el error de "locale-gen cannot be found" deberia realizar lo siguiente:
		>$ echo "export PATH=$PATH:/usr/sbin" >> ~/.profile
		>$ sudo locale-gen
	* Luego reconfigurar los locales para un buen instalacion(good measures):
		>$ sudo dpkg-reconfigure locales
	* Luego ejecutar locale para listarlos:
		>$ locale
	* Si se visualizan estos errores:
		> locale: Cannot set LC_CTYPE to default locale: No such file or directory
		> locale: Cannot set LC_ALL to default locale: No such file or directory
	* Abriri el archivo environments y agregar las siguientes lineas:
		>$ sudo nano /etc/environment
		> LC_ALL=en_US.UTF-8
		> LANG=en_US.UTF-8
	* Guardar el archivo con Ctrl+X, Y y ENTER, luego reiniciar el dispositivo:
		>$ sudo reboot
2) Instalacion de PLEX MEDIA SERVER usando repositorio:
	* Habilitar https para evitar el error: "E: The method driver /usr/lib/apt/methods/https
	could not be found."
		>$ sudo apt-get update && sudo apt-get install apt-transport-https binutils -y --force-yes
	* Obtener gpg key de uglymaoo's para el repositorio:
		>$ wget -O - https://dev2day.de/pms/dev2day-pms.gpg.key | sudo apt-key add -
	* Agregar el repositorio de uglymaoo's
		echo "deb https://dev2day.de/pms/ jessie main" | sudo tee /etc/apt/sources.list.d/pms.list
	* Actualizar la lista de paquetes:
		>$ sudo apt-get update
	* Instalar PMS
		>$ sudo apt-get install plexmediaserver -y
		>$ sudo reboot
	** Para actualizar PMS desde repositorio:
	Note: The repository contains the latest non-Plex Pass version. If you want the
	Plex Pass version you will have to build it yourself using the manual method 3
	[[outlined here.|http://www.htpcguides.com/install-plex-media-server-on-raspberry-pi-2/]]
		>$ 	sudo apt-get update && sudo apt-get upgrade -y
