> ## Práctica 4. Asegurar la granja web
###  **Objetivo**
Configurar seguridad de la granja web. Para ello, llevaremos a cabo las siguientes tareas:
	- Instalar un certificado SSL para configurar el acceso HTTPS a los servidores.
	-  Configurar las reglas del cortafuegos para proteger la granja web.

------------------------------------------------------------------------------------


**1. Instalación de un certificado SSL para configurar el acceso HTTPS a los servidores.**

Para generar un certificado SSL autofirmado tenemos que activar el módulo SSL de Apache, generar los certificados y especificarle la ruta a los certificados en la configuración.

- Activación del módulo SSL:

	- Activamos el módulo

			a2enmod ssl
		
	- Reiniciamos apache
	
			service apache2 restart
	- Creamos la carpeta ssl 
	
			mkdir /etc/apache2/ssl
	- Ruta para los certificados
	
			openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt

	![enter image description here](http://oi67.tinypic.com/qx8cd2.jpg)

	Rellenamos una serie de datos para configurar el dominio así como el correo, la provincia...

	 ![enter image description here](http://i.imgur.com/h6pauBO.png)
  		
  	![enter image description here](http://i.imgur.com/nWJOZMy.png)


- Editamos el archivo de configuración del sitio default-ssl:

		nano /etc/apache2/sites-available/default-ssl.conf

	En el que tenemos que añadir las siguientes líneas: 

		SSLCertificateFile /etc/apache2/ssl/apache.crt
		SSLCertificateKeyFile /etc/apache2/ssl/apache.key