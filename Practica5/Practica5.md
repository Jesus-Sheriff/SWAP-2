
> ## Práctica 5. Replicación de bases de datos MySQL
###  **Objetivos**

- Copiar archivos de copia de seguridad mediante ssh.
- Clonar manualmente BD entre máquinas.
- Configurar la estructura maestro-esclavo entre dos máquinas para realizar el
clonado automático de la información.

_______________________________________________________________________________________________



**1. Crear una BD e insertar datos.**
 - En la máquina 1 creamos una base de datos. Creamos una tabla y le insertamos datos.
 - 
  ![enter image description here](http://i.imgur.com/ORZBiWc.png)
  
  ![enter image description here](http://i.imgur.com/5KNijIx.png)
  
**2. Replicar una BD MySQL con mysqldump.**

Tenemos que tener en cuenta que los datos en la base de datos pueden estar actualizandose constantemente, por lo que debemos evitar que se acceda a la BD para modificar nada.

	mysql -u root -p 
	mysql> FLUSH TABLES WITH READ LOCK;
	mysql> quit
	
![enter image description here](http://i.imgur.com/tSbmkhF.png)

- Una vez bloqueadas las tablas podemos guardar los datos con mysqldump.
		
		mysqldump ejemplodb -u root -p > /tmp/ejemplodb.sql

- Ahora procedemos a desbloquear las tablas

			mysql -u root –p
			mysql> UNLOCK TABLES;
			mysql> quit
			
- Ya podemos ir a la máquina esclavo  (máquina 2) en la que vamos a copiar el archivo SQL con todos los datos salvados desde la máquina principal (maquina1).
![enter image description here](http://i.imgur.com/x8QTcIA.png)

- Importamos la BD en la máquina2, creamos la base de datos ejemplodb e importamos los datos.

mysql -u root –p
mysql> CREATE DATABASE ‘ejemplodb’;
mysql> quit

mysql -u root -p ejemplodb < /tmp/ejemplodb.sql

**3. Replicar una BD mediante una configuración maestro-esclavo.**

- Primero vamos a configurar la máquina que hace de maestro, para ello, tenemos que editar el archivo: /etc/mysql/mysql.conf.d/mysqld.cnf
Tenemos que hacer las siguientes modificaciones:
			
	- Comentamos el parámetro bin-address que sirve para que escuche a un servidor:
		 `	#bin-address 127.0.0.1
	- Indicamos la ruta donde almacenar el lol de errores.
			log_error = /var/log/mysql/error.log
	- Establecemos el identificador del servidor
			server-id = 1
	- Almacenamiento del registro binario 
		log_bin = /var/log/mysql/bin.log
	-  Y por último reiniciamos el servicio

		![enter image description here](http://i.imgur.com/MxngzHf.png)

- Procedemos a configurar la máquina que hará de esclavo (máquina2)
	- Para ello deberemos de hacer las mismas modificaciones descritas anteriormente para la máquina1, salvo en la línea  server-id = 2
que debemos de establecerla a 2.

		![enter image description here](http://i.imgur.com/wYXB0TD.png)


- Volvemos a la máquina1 maestro para crear un usuario y darle permisos de acceso para la replicación.


**4. Replicar una BD mediante una configuración maestro-maestro. (Opcional)**