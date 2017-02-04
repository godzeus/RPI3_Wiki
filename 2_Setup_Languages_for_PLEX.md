## Setup languages for PLEX
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
    # If en_US.utf8 is not present, it must be generated
    ```
	* Edit the file *locale.gen* and uncomment the line en_US.utf8
	
      ```bash
      $ sudo nano /etc/locale.gen  # Uncomment the line_US.UTF-8 UTF-8 and press Ctrl+X, Y and Enter
		  $ sudo locale-gen # to generate the locales
      ```
	* If you get an error like *locale-gen cannot be found*, need to execute the following:
	
    ```bash
    $ echo "export PATH=$PATH:/usr/sbin" >> ~/.profile
		$ sudo locale-gen
	  $ $ sudo dpkg-reconfigure locales # reconfigure the locales for good measures
    ```
	* Then execute locale to list it:
	
    ```bash
    $ locale
    ```
	* If still some errors are showed, like:
	
    ```bash
    # locale: Cannot set LC_CTYPE to default locale: No such file or directory
		# locale: Cannot set LC_ALL to default locale: No such file or directory
    ```
	* Open the file *environments* and add the following lines:
	
    ```bash
    $ sudo nano /etc/environment
		# LC_ALL=en_US.UTF-8
		# LANG=en_US.UTF-8
    # pressCtrl+X, Y and Enter to save
    ```
	* Reboot the RPi3
	
    ```bash
    $ sudo reboot
    ```
