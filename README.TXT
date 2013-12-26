#Versión 0.2 - Solo Debian -  



1 - Introducción:

Con este instructivo vamos a configurar los servicios dhcp , bind, y apache , para que los navegadores web de los clientes tomen la configuración del proxy automáticamente y poder así manejar luego los dominios permitidos con DansGuardian como así también las delay pools con Squid de las conexiones https. 
Todas las operaciones se realizan en la maquina de servicios con permisos de root.


2 - Configuración del servidor DHCP

		
	a) Lo primero que vamos a realizar es un back up archivo de configuración del servicio DHCP.
	
	Siempre realizamos un back up previniendo que algo suceda en la implementación y tengamos que volver a la configuración anterior. 
	

	#cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcp.conf.org
	
	
	b) Descargamos dhcpd.conf  que se encuentra en el directorio "debian"  y lo movemos al directorio donde se encuentra alojado el archivo 	de configuración de DHCP.
	
	Una vez descargado el archivo , nos dirigimos al directorio donde está alojado en nuestra máquina de servicios y vamos a moverlo al 		directorio principal de configuración de DHCP. Este archivo nos dará la nueva configuración DHCP para la auto detección del proxy.
		
	#mv dhcp.conf /etc/dhcp/dhcp.conf 
	
	
	c) Cambiamos los permisos y el dueño del archivo de configuración.
	
	El servicio DHCP lo ejecuta y lo lee el usuario y el grupo root. Le vamos a asignar los permisos necesarios para que no tenga conflictos
	a la hora de su ejecución y lectura.
	
	#chown root:root /etc/dhcp/dhcpd.conf
	#chmod 644 /etc/dhcp/dhcpd.conf
	
	
	d) Reiniciamos el servidor DHCP
	
	Reiniciamos el servidor DHCP para que tome la configuración realizada.
		
	#service isc-dhcp-server restart


3 - Archivo de auto configuración de proxy.

	a) Copiamos archivo "wpad.dat" que se encuentra en el directorio "archivos_compartidos" a  "/var/www". 
	
	Una vez descargado el archivo "wpad.dat" , nos dirigimos al directorio donde está alojado en nuestra máquina de servicios y vamos a 		copiarlo al directorio de alojamiento web principal local de Apache.
	El archivo de configuración de dhcp envía en su información un archivo alojado en nuestro servidor web local. Este archivo es wpad.dat 		donde el archivo se auto-indexa y así el servicio DHCP lo podrá buscar para enviarle la configuración a sus clientes.
	 
	#cp wpad.dat /var/www
	
	
	b) Cambiamos los permisos y el dueño del archivo de configuración.
	
	El servicio web Apache lo ejecuta y lo en el usuario www-data, sin importar el grupo. Le vamos a asignar los permisos necesarios para que 		no tenga  conflictos a 	la hora de su ejecución y lectura.
	 

	#chown www-data /var/www/wpad.dat
	#chmod 600 /var/www/wpad.dat

	
	c) Crear enlaces simbólicos para otros navegadores.
	
	No todos los navegadores utilizan el mismo archivo de auto configuración de proxy, por lo cual tenemos que apuntar el mismo archivo 		wpad.dat a otros archivos dentro del mismo servidor web local teniendo así cubierto varios escenarios de configuración de diversos
	navegadores. 
	
	#cd /var/www/
	#ln -s wpad.dat wpad.pac
	#ln -s wpad.dat proxy.pac


4 - Archivo de configuración distribuida de Apache (.htaccess).


	a) Copiar archivo .htaccess que se encuentra en el directorio "archivos_compartidos" a  "/var/www".
	
	Una vez descargado el archivo , nos dirigimos al directorio donde está alojado en nuestra máquina de servicios y vamos a copiarlo en el 	directorio principal de alojamiento de Apache. Este archivo nos permite configurar en Apache una directiva sin tener que modificar el 		archivo de configuración principal. 
	
	
	#cp .htaccess /var/www
		

	b) Cambiamos los permisos y el dueño del archivo de configuración.
	
	El servicio web Apache lo ejecuta y lo en el usuario www-data, sin importar el grupo. Le vamos a asignar los permisos necesarios para que 		no tenga  conflictos a 	la hora de su ejecución y lectura.
	
	#chown www-data /var/www/.htaccess
	#chmod 600 /var/www/.htaccess


5 - Configuración de BIND

	a) a) Lo primero que vamos a realizar es un back up archivo de configuración del servicio BIND.
	
	Siempre realizamos un back up previniendo que algo suceda en la implementación y tengamos que volver a la configuración anterior. 

	#cp /var/lib/bind/miescuela.local.hosts  /var/lib/bind/miescuela.local.hosts.org
		

	b) Descargamos "miescuela.local.hosts" que se encuentra en el directorio "debian"  y lo movemos al directorio donde se encuentra alojado 		el archivo de configuración de nombres de BIND.
	
	Una vez descargado el archivo , nos dirigimos al directorio donde está alojado en nuestra máquina de servicios y vamos a moverlo al 		directorio de resolución de nombres de BIND. Este archivo nos dará la nueva configuración DNS local para que los navegadores que 		traten de obtener la auto configuración por DNS, puedan resolver la dirección del archivo wpad sin inconvenientes.
	
	#mv miescuela.local.hosts /var/lib/bind/miescuela.local.hosts 
	
	
	c) Cambiamos los permisos y el dueño del archivo de configuración.
	
	El servicio BIND lo ejecuta y lo lee el usuario y el grupo root. Le vamos a asignar los permisos necesarios para que no tenga conflictos
	a la hora de su ejecución y lectura.
		
	#chown root:root /var/lib/bind/miescuela.local.hosts
	#chmod 644 /var/lib/bind/miescuela.local.hosts
	

	d) Reiniciamos el servidor BIND 
	Reiniciamos el servidor BIND para que tome el cambio de configuraciones. 
	
	#service bind9 restart

	
	e) Probamos los cambios desde un cliente conectado a nuestra red
	Hacemos ping desde un cliente a wpad para comprobar que está resolviendo el nombre del archivo wpad.dat de manera local.
	 
	ping wpad


6 - Configuración en navegadores de clientes
	
 	a) Activar auto detección de proxy en los navegadores de los clientes conectados a nuestra red.

