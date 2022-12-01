## AWSTAT

AWStats es un programa opensource creado para la generación de informes de estadística, principalmente sobre el tráfico de una web, que se basa en los archivos logs del servidor. AWStats analiza datos como el FTP, el email, el servidor web o el streaming, soporta diferentes formatos de logs de servidores web como IIS, WebStar o Apache y además se pueden personalizar. Los datos se extraen en gráficos de barra e informes de tablas, para saber quién, desde dónde y cómo visitan la web, datos imprescindibles para establecer una estrategia de Marketing Online. Además se pueden crear informes estáticos a través de una interfaz de línea de comando y generar informes específicos mediante un navegador web con la herramienta CGI.

![que-es-awstats-removebg-preview](https://user-images.githubusercontent.com/114391559/204782029-0b77f74a-3fb8-4191-a093-4c5a61c08ff0.png)

## Instalación y configuración AWSTAT

Comando de instalación:
```
sudo apt-get install awstats
```

Tenemos que habilitar el módulo CGI para que nos cree los informes estáticos:

```
sudo a2enmod cgi
```
Reiniciamos Apache:
```
systemctl restart apache2
```
![image](https://user-images.githubusercontent.com/114391559/204782950-63af0bed-e73a-4655-b069-32f2c8390727.png)

Terminada la instalación, procedemos a configurar, para ello copiaremos lo que esté en la ruta `/etc/awstats/awstats.conf` a la ruta `/etc/awstats/awstats.centro.intranet.conf`.
```
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
```
Abrimos el fichero `/etc/awstats/awstats.centro.intranet.conf`:
```
sudo nano /etc/awstats/awstats.centro.intranet.conf
```

Vamos a modificar los siguientes datos:

```
  SiteDomain=
  HotAliases=
  AllowToUpdateStatsFromBrowser=

```
Tenemos que dejarlo así:

```
  SiteDomain='centro.intranet'
  HotAliases='centro.intranet localhost 127.0.0.1'
  AllowToUpdateStatsFromBrowser=1
```
Ponemos en 1 el parámetro `AllowToUpdateStatsFromBrowser`, para que nos permita actualizar las estadísticas mediante un botón desde el navegador web.

Guardamos y cerramos.

Generamos las estadísticas iniciales:

```
sudo /usr/lib/cgi-bin/awstats.pl -config=centro.intranet -update
```
![image](https://user-images.githubusercontent.com/114391559/204784603-ee80017e-abf7-4b18-ad51-9fef5c80ec37.png)

Abrimos el navegador y vamos a la siguiente url `http://centro.intranet/cgi-bin/awstats.pl?config=centro.intranet`.

![image](https://user-images.githubusercontent.com/114391559/204784940-8c61fd1b-5c24-46cb-94ff-73f396810c8f.png)






