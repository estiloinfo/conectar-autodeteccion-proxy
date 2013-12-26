#Versión 0.1 - Solo Debian -  


1. Configuración del servidor DHCP


	a) Back up archivo de configuración
	

	#cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcp.conf.org
	
	
	b) Copia de archivo de configuración modificado
	
	#mv dhcp.conf /etc/dhcp/dhcp.conf 
	
	
	c) Permisos y dueño dhcp.conf
	
	#chown root:root /etc/dhcp/dhcpd.conf
	#chmod 644 /etc/dhcp/dhcpd.conf
	
	
	d) Reiniciamos el servidor DHCP
	
	#service isc-dhcp-server restart


2. Archivo Wpad

	a) Copiamos wpad.dat en /var/www
	 
	#cp wpad.dat /var/www
	
	
	b) Permisos y dueño 

	#chown www-data /var/www/wpad.dat
	#chmod 600 /var/www/wpad.dat

	
	c) Crear enlaces simbolicos para otros navegadores

	#cd /var/www/
	#ln -s wpad.dat wpad.pac
	#ln -s wpad.dat proxy.pac


3. Archivo  .htaccess


	a) Copiar archivo .htaccess en /var/www

	#cp .htaccess /var/www
		

	b) Permisos y dueño
	
	#chown www-data /var/www/.htaccess
	#chmod 600 /var/www/.htaccess


4. Configuración de BIND

	a) Back up archivo de configuración

	#cp /var/lib/bind/miescuela.local.hosts  /var/lib/bind/miescuela.local.hosts.org
		

	b) Copia de archivo de configuración modificado
	
	#mv miescuela.local.hosts /var/lib/bind/miescuela.local.hosts 
	
	
	c) Permisos y dueño miescuela.local.host
	
	#chown root:root /var/lib/bind/miescuela.local.hosts
	#chmod 644 /var/lib/bind/miescuela.local.hosts
	

	d) Reiniciamos el servidor BIND 
	
	#service bind9 restart

	
	e) Probamos los cambios desde un cliente conectado a nuestra red
	
	ping wpad



5. Configuracion en navegadores de clientes
	
 	a) Activar autodetección de proxy en los navegadores de los clientes conectados a nuestra red.

