> ## Práctica 3. Balanceo de carga
### **Objetivo**
En esta práctica el objetivo es configurar las máquinas virtuales de forma que dos hagan de servidores web finales mientras que la tercera haga de balanceador de carga por software.
En esta práctica se llevarán a cabo, como mínimo, las siguientes tareas:

- Configurar una máquina e instalarle el nginx como balanceador de carga.
- Configurar una máquina e instalarle el haproxy como balanceador de carga.
- Someter a la granja web a una alta carga, teniendo primero nginx y después haproxy.

------------------------------------------------------------------------------------


**1.Instalar una nueva maquina para el balanceador.**

Instalamos otra máquina con Ubuntu Server que hará de balanceador de carga, teniendo en cuenta que no debemos instalar Apache porque éste se apropia del puerto 80.
	
**2.Instalacion nginx**
	
- Importamos la clave del repositorio de software

	![ImportarClave](http://imageshack.com/a/img924/9117/4LilXQ.png)


- Añadimos el repositorio al fichero /etc/apt/sources.list

		echo "deb http://nginx.org/packages/ubuntu/ lucid nginx"
		echo "deb-src http://nginx.org/packages/ubuntu/ lucid nginx" 

	![AñadirRepositorio](http://imageshack.com/a/img922/1029/vzjy6p.png)

-  Instalamos nginx

			apt-get update 
			apt-get install nginx

- Configuración nginx
Accedemos al archivo /etc/nginx/conf.d/default.conf.
Y añadimos lo siguiente:

	![enter image description here](http://imageshack.com/a/img924/8606/QsANeh.png)

- Reiniciamos el servicio

			service nginx restart
	
	![enter image description here](http://imageshack.com/a/img924/2669/ttsMJG.png)
	
- Comprobamos que funciona correctamente con el comando curl

	![enter image description here](http://imageshack.com/a/img924/6082/eHsvM3.png)


Si sabemos si una máquina es más potente que otra podemos modificar la carga que se le da a cada una, a través del modificador llamado "weight"


**2.Instalación haProxy**

Tenemos que reiniciar la máquina o para el servicio de nginx (service nginx stop) para dejar el puerto 80 libre para haProxy.
Procedemos con la instalación de haproxy
apt-get install haproxy

- Una vez instalado procedemos a modificar el archivo de configuración haproxy.cfg ubicado en /etc/haproxy/

	![enter image description here](http://imageshack.com/a/img924/9613/pl2ymK.png)

- Comprobamos que funciona correctamente