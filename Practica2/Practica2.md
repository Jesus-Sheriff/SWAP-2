> ## Práctica 2. Clonar la información de un sitio web

### **Objetivos**
Los objetivos concretos de esta segunda práctica son:

-  aprender a copiar archivos mediante ssh
-  clonar contenido entre máquinas
-  configurar el ssh para acceder a máquinas remotas sin contraseña 
-  establecer tareas en cron

------------------------------------------------------------------------------------
**Configuración de las máquinas en VirtualBox**
En primer lugar hay que configurar la red en VirtualBox añadiendo en cada una un adaptador conectado a NAT y otro a sólo anfitrión.  
Además de modificar el archivo /etc/network/interfaces para asignarle una ip estática a cada máquina.

![enter image description here](http://imageshack.com/a/img924/6528/R7PZcC.png)
![enter image description here](http://imageshack.com/a/img923/7458/csQj3b.png)

------------------------------------------------------------------------------------
**1 Instalación rsync y comprobación de su funcionamiento **

		sudo apt-get install rsync

 A continuación comprobamos el funcionamiento de rsync  clonando una carpeta. Se clona la carpeta idénticamente y su contenido, en este caso he añadido el archivo prueba y comprobamos que realmente se ha añadido de la máquina1 a la máquina2.

![enter image description here](http://imageshack.com/a/img924/2763/UJh99Q.png)
![enter image description here](http://imageshack.com/a/img923/5977/j0ppwH.png)

**2 Acceso sin contraseña para ssh**

- Generamos la clave mediante ssh-keygen con la opción -t para el tipo de clave.
![enter image description here](http://imageshack.com/a/img923/9095/uuMkxJ.png)

-  Copiamos la clave mediante  ssh-copy-id

	![enter image description here](http://imageshack.com/a/img922/585/m8SZcZ.jpg)

- A continuación ya podremos conectarnos a dicho equipo sin contraseña.

	![enter image description here](http://imageshack.com/a/img922/4695/olurPv.png)


**3.Programación de tareas con crontab**
 
- Accedemos a /etc/crontab, en el que le indicaremos como se muestra en la captura en la última línea que se haga una sincronización cada dos minutos.
	![enter image description here](http://imageshack.com/a/img924/4314/nq6Mr5.png)



	
