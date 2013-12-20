
1. Editar el archivo de configuración del servidor DHCP

	a) Agregar como parametro general la siguiente linea

	option wpad-url code 252 = text;

	b) Agregar a la subred el archivo que se utilizara para pasarle los datos al navegador
	para que pueda autodetectar el proxy

	subnet 172.16.0.0 netmask 255.255.0.0 {
	.
	.
	.
		option wpad-url = "http://172.16.0.1/wpad.dat";
		option domain-name "miescuela.local";
	}

2. Reiniciar el servidor DHCP

	/etc/init.d/.......       restart


3. Copiar el archivo  wpad.dat en /var/www

	Cambiarle los permisos a www-data

	#chown www-data /var/www/wpad.dat

	Cambiarle los permisos
	#chmod 600 /var/www/wpad.dat

4. Crear enlaces simbolicos para otros navegadores

	#cd /var/www/
	#ln -s wpad.dat wpad.pac
	#ln -s wpad.dat proxy.pac

5. Copiar archivo .htaccess en /var/www

	Cambiarle los permisos a .htaccess

	#chown www-data /var/www/.htaccess

	Cambiarle los permisos
	#chmod 600 /var/www/.htaccess

6. Modificacion de bind
	Agregar en /var/lib/bind/miescuela.local.hosts

	wpad.miescuela.local. IN CNAME servicios.miescuela.local.

7. reiniciar el bind

	/etc/init.d/.......       restart

8. prueba

	ping wpad


9 configuracion de navegadores

	autodeteccion de proxy activada

