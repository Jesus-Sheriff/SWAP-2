> ## Práctica 4. Asegurar la granja web
###  **Objetivo**
Configurar seguridad de la granja web. Para ello, llevaremos a cabo las siguientes tareas:
- Instalar un certificado SSL para configurar el acceso HTTPS a los servidores.
- Configurar las reglas del cortafuegos para proteger la granja web.

------------------------------------------------------------------------------------


**1. Instalación de un certificado SSL para configurar el acceso HTTPS a los servidores.**

Para generar un certificado SSL autofirmado tenemos que activar el módulo SSL de Apache, generar los certificados y especificarle la ruta a los certificados en la configuración.

Activamos el módulo SSL:

	a2enmod ssl
	service apache2 restart
	mkdir /etc/apache2/ssl
	openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt