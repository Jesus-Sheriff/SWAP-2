
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

**4. Replicar una BD mediante una configuración maestro-maestro. (Opcional)**