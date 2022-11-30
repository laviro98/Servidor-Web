## WSGI

WSGI son las siglas de Web Server Gateway Interface. Es una especificación que describe cómo se comunica un servidor web con una aplicación web, y cómo se pueden llegar a encadenar diferentes aplicaciones web para procesar una solicitud/petición (o request).

Un servidor WSGI solo recibe la solicitud del cliente, la pasa a la aplicación y luego envía la respuesta devuelta por la aplicación al cliente. No hace nada más. Todos los detalles explicitos deben ser suministrados por la aplicación o middleware.

![image](https://user-images.githubusercontent.com/114391559/204765176-c34d602e-771f-414a-a680-8ad3cf2e068c.png)
> Diagrama de Cliente/Servidor(WSGI)

## Instalación WSGI

Usaremos el módulo WSGI para permitir la ejecución de aplicaciones Python.

Para la instalación, ejecutamos:
```
sudo apt install libapache2-mod-wsgi-py3
```
Es muy probable que al intentar ejecutarlo nos de el siguiente error:

![image](https://user-images.githubusercontent.com/114391559/204776424-cb4d1e57-8bdf-4f19-81c4-a8e4477a054d.png)

Si es así, debemos ejecutar `sudo dpkg --configure -a`, esto nos corrige el problema.
Volvemos a ejecutar el primer comando de instalación.

Luego vamos a la ruta `cd /var/www/html` y ahí creamos un fichero python, en este caso `sudo touch test.py`.

Abrimos el archivo python creado:

```
sudo nano test.py
```

Insertamos el siguiente texto y guardamos:
```
def application(environ, start_response):
    status = '200 OK'
    output = b'Hello Howtoforge!\n'
    response_headers = [('Content-type', 'text/plain'),
                        ('Content-Length', str(len(output)))]
    start_response(status, response_headers)
    return [output]
```

Le cambiamos los permisos al fichero `test.py`.
```
sudo chown www-data:www-data /var/www/html/test.py
sudo chmod 775 /var/www/html/test.py
```

## Establecemos el dominio con el que se va a comunicar (a través de Python):

Entramos en el archivo de configuración `000-default.conf`.

```
sudo nano /etc/apache2/sites-available/000-default.conf
```
Añadimos lo siguiente, donde viene la información del dominio:
```
<VirtualHost *:80>
        ServerName departamentos.centro.intranet
        ServerAlias www.departamentos.centro.intranet
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        WSGIScriptAlias /test /var/www/html/test.py  
</VirtualHost>
```

Para que funcione, el dominio debe estar dentro del fichero `/etc/hosts`, lo que ya hemos hecho anteriormente.

