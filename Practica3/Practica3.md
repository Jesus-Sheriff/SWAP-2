> ## Práctica 3. Balanceo de carga.
###  **Objetivo**
En esta práctica el objetivo es configurar las máquinas virtuales de forma que dos hagan de servidores web finales mientras que la tercera haga de balanceador de carga por software.
En esta práctica se llevarán a cabo, como mínimo, las siguientes tareas:

- Configurar una máquina e instalarle el nginx como balanceador de carga.
- Configurar una máquina e instalarle el haproxy como balanceador de carga.
- Someter a la granja web a una alta carga, teniendo primero nginx y después haproxy.

------------------------------------------------------------------------------------


**1.Instalar una nueva maquina para el balanceador.**

Instalamos otra máquina con Ubuntu Server que hará de balanceador de carga.
	
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

	![enter image description here](http://imageshack.com/a/img924/5598/d22pgH.png)


Si sabemos si una máquina es más potente que otra podemos modificar la carga que se le da a cada una, a través del modificador llamado "weight"


**2.Instalacion haProxy**

	apt-get install haproxy

- Una vez instalado procedemos a modificar el archivo de configuración haproxy.cfg ubicado en /etc/haproxy/

	![enter image description here](http://imageshack.com/a/img924/9613/pl2ymK.png)

- Paramos el servicio de nginx y reiniciamos haproxy 
![enter image description here](http://imageshack.com/a/img923/1566/DaFmNE.png)
- Comprobamos que funciona correctamente
![enter image description here](http://imageshack.com/a/img922/2929/xjMa4Z.png)


**3. Someter a una alta carga el servidor balanceado**

Para comprobar el rendimiento de nuestra granja web utilizaremos Apache Benchmark. En una máquina diferente a la formada por la de nuestra granja web ejecutamos Apache Benchmark, para ver el rendimiento real y que no consuman recursos de la misma máquina.

		ab -n 1000 -c 10 http://192.168.56.103/index.html
	
- Con HaProxy como balanceador

	![enter image description here](http://imageshack.com/a/img923/9071/EdONyC.png)

- Con nginx como balanceador 

	![enter image description here](http://imageshack.com/a/img924/2667/FYI3Vk.png)


**4. Instalación Pound, otro balanceador de carga. (Opcional)**

 - Instalamos Pound mediante el comando: 	
			
				apt-get install Pound

 -  Configuración Pound


En el primer Address añadimos la dirección de nuestra máquina balanceadora y cambiamos el puerto 80, y más abajo podemos observar que hemos añadido las direcciones de máquina1 y la máquina2.
![enter image description here](http://i.imgur.com/N9MR22x.png)

- Reiniciamos el servicio

			service pound restart


- Nos aseguramos que esté activo solo Pound

![enter image description here](http://i.imgur.com/W1ylSDl.png)

- Comprobamos su funcionamiento mediante curl

![enter image description here](http://i.imgur.com/XZBDpRy.png)