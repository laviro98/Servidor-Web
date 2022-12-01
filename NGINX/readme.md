## NGINX
NGINX, pronunciado en inglés como «engine-ex», es un famoso software de servidor web de código abierto. En su versión inicial, funcionaba en servidores web HTTP. Sin embargo, hoy en día también sirve como proxy inverso, balanceador de carga HTTP y proxy de correo electrónico para IMAP, POP3 y SMTP.

![1_KWR1wJTWCp2fuWyHTYiqDg](https://user-images.githubusercontent.com/114391559/205002037-a237fe0c-e3a9-4020-86d1-2a3ebaa5c466.png)

Este software fue lanzado oficialmente en octubre del 2004. El creador del software, Igor Sysoev, comenzó su proyecto en el 2002 como un intento de solucionar el problema C10k. C10k es el reto de gestionar diez mil conexiones al mismo tiempo. Hoy en día, los servidores web tienen que manejar un número aún mas grande de conexiones. Por esa razón, NGINX ofrece una arquitectura asíncrona y controlada por eventos, característica que hace de NGINX uno de los servidores más confiables para la velocidad y la escalabilidad.


## _NGINX vs Apache_

![image](https://user-images.githubusercontent.com/114391559/205001266-289ff4d7-bc88-4731-b029-9f9f3798ea83.png)

Entre los servidores web populares, Apache es uno de los principales rivales de NGINX. Ha existido desde los años 90 y cuenta con una gran comunidad de usuarios. 
A continuación, una comparación entre NGINX y Apache:

- Compatibilidad del sistema operativo: tanto NGINX como Apache pueden ejecutarse en muchos sistemas operativos que soportan el sistema Unix. Desafortunadamente, el rendimiento de NGINX en Windows no es tan bueno como en otras plataformas.
- Soporte al usuario: Los usuarios, que van desde novatos hasta profesionales, siempre necesitan una buena comunidad que les pueda ayudar cuando enfrenten problemas. Si bien NGINX y Apache tienen soporte por correo y un foro de Stack Overflow, Apache carece de soporte por parte de su compañía, la Apache Foundation.
- Rendimiento: NGINX puede ejecutar simultáneamente 1000 conexiones de contenido estático dos veces más rápido que Apache y usa un poco menos de memoria. Sin embargo, cuando se comparan por su rendimiento en la ejecución de contenido dinámico, ambos tienen la misma velocidad. NGINX es una mejor opción para aquellos que tienen un sitio web más estático.

## Instalación NGINX

Para instalar NFINX ejecutamos el siguiente comando:
```
sudo apt install nginx
```
Necesitamos instalar los paquetes correspondientes para usar MYSQL y PHP con NGINX:

```
sudo apt install -y php-fpm php-mysql php-json php-mbstring php-xml
```
Necesitaremos tener instalado phpMyAdmin para la configuración. 
> phpMyAdmin es una herramienta escrita en PHP con la intención de manejar la administración de MySQL a través de páginas web, utilizando un navegador web.

Descargamos y extraemos phpMyAdmin:

```
sudo wget https://files.phpmyadmin.net/phpMyAdmin/5.2.0/phpMyAdmin-5.2.0-all-languages.tar.gz
sudo tar -zxvf phpMyAdmin-5.2.0-all-languages.tar.gz

```
Lo movemos hasta la carpeta contenedor:
 ```
 sudo mv phpMyAdmin-5.2.0-all-languages /var/www/phpmyadmin
 ```

Movemos nuestra plantilla de configuración cambiándole el nombre a `config.inc.php`.
```
sudo mv /var/www/phpmyadmin/config.sample.inc.php /var/www/phpmyadmin/config.inc.php
```

## Segundo servidor (servidor2.centro.intranet)

Procedemos con la configuración, para ello debemos tener una clave Blowfish(clave secreta), que se genera a través de un comando llamado `pwgen`.

> Pwgen es una herramienta que nos permitirá crear contraseñas fuertes de forma fácil y rápida. Podremos elegir la complejidad de la contraseña (solo letras, números, mayúsculas y minúsculas y símbolos) así como su longitud.

Instalamos el comando:
```
sudo apt-get install pwgen
```

La sintaxis del comando `pwgen` es `pwgen [opcion] [tamaño de la contraseña] [número de contraseñas]`. 

```
sudo pwgen -s 32 1
```
![image](https://user-images.githubusercontent.com/114391559/205006920-fa32afaf-def5-4b44-bfc3-4d0b7b6c13ff.png)


Guardamos la información que nos devuelve para el archivo de configuración y lo editamos:

```
sudo nano /var/www/phpmyadmin/config.inc.php
```

Modificamos los siguientes datos:

> $cfg['blowfish_secret'] = 'CONTRASEÑA GENERADA AQUÍ';

> $cfg['Servers'][$i]['controlhost'] = 'localhost';

> $cfg['Servers'][$i]['controlport'] = '';

> $cfg['Servers'][$i]['controluser'] = 'NOMBRE AQUÍ';

> $cfg['Servers'][$i]['controlpass'] = 'password';


![image](https://user-images.githubusercontent.com/114391559/205006372-284ae970-1d9f-45bc-9dd4-499c3a14a47b.png)

Ahora hay que relacionar MySQL con phpMyAdmin:

```
sudo mysql < /var/www/phpmyadmin/sql/create_tables.sql -u root -p
```

Debemos configurar el archivo `phpmyadmin.conf` para el nuevo servidor:

```
sudo nano /etc/nginx/sites-available/phpmyadmin.conf
```

Insertamos:

```
server {
   listen 8080;
   server_name servidor2.centro.intranet;
   root /var/www/phpmyadmin;
   location / {
      index index.php;
   }
## Images and static content is treated different
   location ~* ^.+.(jpg|jpeg|gif|css|png|js|ico|xml)$ {
      access_log off;
      expires 30d;
   }
   location ~ /.ht {
      deny all;
   }
   location ~ /(libraries|setup/frames|setup/libs) {
      deny all;
      return 404;
   }
   location ~ .php$ {
      include /etc/nginx/fastcgi_params;
      fastcgi_pass unix:/run/php/php8.1-fpm.sock;
      fastcgi_index index.php;
      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
   }
}

```

Creamos una carpeta temporal para phpMyAdmin y cambiamos los permisos:

```
sudo mkdir /var/www/phpmyadmin/tmp
sudo chmod 777 /var/www/phpmyadmin/tmp
```
Establecemos la propiedad del directorio:

```
sudo chown -R www-data:www-data /var/www/phpmyadmin
```

Debemos agregar el dominio al archivo `/etc/hosts`.

```
sudo nano /etc/hosts
```
Agregamos: 
> 127.0.0.1 servidor2.centro.intranet

![image](https://user-images.githubusercontent.com/114391559/205008200-c4946b29-900d-48af-a96d-a6e2312ea1e3.png)

Guardamos y cerramos el nano.

Requiere un reinicio de NGINX y PHP:
```
sudo systemctl restart nginx php8.1-fpm
```

Y, por último, debemos abrir el navegador y entrar en la url `http://servidor2.centro.intranet:8080`. Si nos aparece así, ya tendríamos nuestro segundo servidor configurado:

![image](https://user-images.githubusercontent.com/114391559/205008673-811d6168-3118-4c7f-b485-6a9e0d1c46dd.png)

