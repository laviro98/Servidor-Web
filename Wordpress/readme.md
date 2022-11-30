## WORDPRESS

WordPress es un sistema de gestión de contenidos (CMS), un software utilizado para construir, modificar y mantener sitios web.

Se trata del CMS más popular del mercado, ya que lo utilizan el 65,2% de los sitios web cuyo CMS conocemos. Esto se traduce en el 42,4% de todos los sitios web, casi la mitad de Internet.

## _¿Por qué es tan popular?_

- Es gratuito: aunque los usuarios tendrán que conseguir un hosting y un nombre de dominio antes de empezar a utilizarlo, WordPress en sí se puede descargar y utilizar de forma totalmente gratuita.
- Es flexible y personalizable: WordPress tiene muchos usos, desde la creación de un blog y un portafolio online hasta la construcción de una tienda de comercio electrónico. WordPress también admite desarrolladores de la comunidad. Por lo tanto, hay muchas opciones de temas y plugins disponibles en la biblioteca oficial de WordPress y en los portales de terceros.
- Es una plataforma escalable: WordPress puede manejar sitios web de cualquier tamaño siempre que el plan de CMS hosting tenga los recursos necesarios. No hay necesidad de migrar a una plataforma diferente una vez que el sitio web comience a crecer.
- Es relativamente fácil de usar: en comparación con la programación manual de un sitio web desde cero, WordPress requiere muchos menos conocimientos técnicos. Aunque se puede modificar el código, es posible crear sitios web bonitos y totalmente funcionales sólo a través de la interfaz gráfica de usuario.

##Instalación Wordpress
Descargamos el paquete de instalación y lo descomprimimos en la siguiente ruta:
```
wget https://es.wordpress.org/latest-es_ES.tar.gz
sudo tar xf latest-es_ES.tar.gz -C /var/www/html/
```
Cambiamos la propiedad del directorio al usuario con el que corre el servicio web para que WordPress pueda escribir y modificar sus propios archivos sin necesidad de FTP:
```
sudo chown -R www-data:www-data /var/www/html/wordpress/
```

> Nota: FTP es un protocolo que se utiliza para transferir todo tipo de archivos entre equipos conectados a una red.

