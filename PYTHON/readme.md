## PYTHON

Python es un lenguaje sencillo de leer y escribir debido a su alta similitud con el lenguaje humano. Además, se trata de un lenguaje multiplataforma de código abierto y, por lo tanto, gratuito, lo que permite desarrollar software sin límites. Con el paso del tiempo, Python ha ido ganando adeptos gracias a su sencillez y a sus amplias posibilidades, sobre todo en los últimos años, ya que facilita trabajar con inteligencia artificial, big data, machine learning y data science, entre muchos otros campos en auge. 

## _¿Dónde se utiliza Python?_
- Data analytics y big data.
- Data mining.
- Data science.
- Inteligencia artificial.
- Blockchain.
- Machine learning.
- Desarrollo web.
- Juegos y gráficos 3D.


## Instalación y configuración Python

Para la instalación, ejecutamos el siguiente comando:

```
sudo apt install python3-pip
```
Para comprobar la versión instalada:
```
python3 --version
```

## Dominio departamentos.centro.intranet
Para añadir el dominio, entramos en `etc/hosts`, añadimos la IP y el nombre del dominio:

```
sudo nano /etc/hosts
```
![image](https://user-images.githubusercontent.com/114391559/204754209-795faf72-0ee3-4962-b81d-b6df661b9ef4.png)

## Proteger acceso autenticación

Para poder proteger el acceso, debemos entrar en el fichero `.htpasswd`.
> .htpasswd es un modelo de tabla usado para almacenar nombres de usuarios y contraseñas para una autenticación de acceso básica en un servidor HTTP Apache.
Debemos poner el nombre de usuario que queramos al final del comando. Luego, nos pregunta por la contraseña que queremos que tenga.
```
sudo htpasswd -c /etc/apache2/.htpasswd lara
```
A continuación, accedemos al archivo de configuracion `000-default.conf`. 
Para acceder, entramos en la ruta `/etc/apache2/sites-enabled`.
```
sudo nano /etc/apache2/sites-enabled/000-default.conf
```
Añadimos el siguiente archivo de configuración:
```
  <Directory /var/www/html>
        AuthType Basic
        AuthName "Secure area - Authentication required"
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user
  </Directory>
```

Abrimos el navegador y entramos en la url `http://departamentos.centro.intranet` para comprobar si nos pide autenticación. 
Debería salir así:

![image](https://user-images.githubusercontent.com/114391559/204757792-e78f0346-5a51-454b-b172-2ea5429a10f1.png)

