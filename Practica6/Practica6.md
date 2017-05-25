> ## Práctica 6. Discos en RAID.
###  **Objetivos**


Los objetivos concretos de esta práctica son:
• Configurar dos discos en RAID 1. Los discos se añadirán a un sistema ya instalado y funcionando, de forma que en total tendremos tres discos (el sistema operativo estará ya instalado en /dev/sda y añadiremos dos discos
más, que serán el /dev/sdb y el /dev/sdc, para configurar el dispositivo de almacenamiento RAID en estos dos discos nuevos de igual tamaño).

• Hacer pruebas de retirar y añadir un disco “en caliente”, y comprobar que el RAID sigue funcionando correctamente.


-------------------------------------------------------------------------------------
 **1. Configuración del RAID por sofware**

 - Añadimos dos discos a la máquina del mismo tipo y capacidad.

 ![enter image description here](http://i.imgur.com/7KE1Gxm.png)

 - Configuramos el RAID
- Instalación de software necesario para la configuración del RAID.
			sudo apt-get install mdadm

	- Buscamos la identificación asignada por Linux de ambos discos:
			
			sudo fdisk -l
		![enter image description here](http://i.imgur.com/p3BhqnT.png)
- Creamos el RAID 1, usando el dispositivo /dev/md0, indicando
 el número  de dispositivos que vamos a utilizar  su ubicación.

			sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc

- Le damos formato
		
			sudo mkfs /dev/md0

- Creamos el directorio en el que se montará la unidad del RAID.
			
			sudo mkdir /dat
			sudo mount /dev/md0 /dat

