
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
La máquina1 hará de maestro para la máquina 2 que será la esclavo, por lo tanto al modificar algo en el maestro, se actualizará en el esclavo.

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

Entramos en mysql y ejecutamos las siguientes sentencias:

	mysql> CREATE USER esclavo IDENTIFIED BY 'esclavo';
	mysql> GRANT REPLICATION SLAVE ON  *.*  TO  'esclavo'@'%'  IDENTIFIED  BY 'esclavo'; 
	mysql> FLUSH PRIVILEGES;
	mysql> FLUSH TABLES;
	mysql> FLUSH TABLES WITH READ LOCK;

- Obtenemos los datos de la base de datos que vamos a replicar, que usaremos posteriormente en la configuración de la máquina2 esclavo.

	![enter image description here](http://i.imgur.com/13GW5rP.png)
	![enter image description here](http://i.imgur.com/0raiMhY.png)
- Volvemos a la máquina2 esclavo, entramos a mysql y le damos los datos del maestro, en estas setencias tenemos que tener en cuenta en MASTER_HOST indicar correctamente la ip de nuestra máquina maestro, MASTER_LOG_POST indicarle la posición mostrada anteriormente, además de indicar EL MASTER_LOG_FILE correctamente. 

	![enter image description here](http://i.imgur.com/hKENEBo.png)
	
- Por último, arrancamos el esclavo y ya está todo listo para que los demonios de MySQL de las dos máquinas repliquen automáticamente los datos que se introduzcan/modifiquen/borren en el servidor maestro:

		mysql> START SLAVE;

Nota: he tenido que configurar otras dos máquinas nuevas porque con las anteriores daban problemas, he vuelto a realizar todo este proceso anterior y en unas máquinas desde 0 funciona correctamente.
Para comprobar que todo funciona, insertamos nuevos datos en la máquina maestro y podemos ver que en la máquina esclavo se actualizan los mismos datos insertados.

![enter image description here](http://i.imgur.com/yZWJE38.png)

**4. Replicar una BD mediante una configuración maestro-maestro. (Opcional)**

En este caso configuraremos las máquinas para que cada una se crea que es el maestro y así al modificar datos en la base da datos de cualquier máquina se actualizará en la otra.

He hecho una clonación de las máquinas maestro-esclavo. Primero habra que ir a la segunda máquina que teníamos configurada como esclavo y crear un usuario.

![enter image description here](http://i.imgur.com/SYgGzyt.png)

![enter image description here](http://i.imgur.com/EHgf97L.png)
Ahora tendremos que irnos a la máquina configurada como maestro, para indicarle que la otra es su maestro.

![enter image description here](http://i.imgur.com/a7WkhA3.png)

Comprobamos que realmente funciona esta configuración, añadiendo datos en cada máquina y visualizando que se ha actualizado en la otra máquina.

![enter image description here](http://i.imgur.com/7IEtE8D.png)