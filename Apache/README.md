## APACHE

La funcionalidad principal de este servicio web es servir a los usuarios todos los ficheros necesarios para visualizar la web. Las solicitudes de los usuarios se hacen normalmente mediante un navegador (Chrome, Firefox, Safari, etc.).

Apache tiene una estructura basada en módulos, que permite activar y desactivar funcionalidades adicionales, por ejemplo, módulos de seguridad como mod_security, módulos de caché como Varnish, o de personalización de cabeceras como mod_headers. También permite ajustar los parámetros de PHP de tu hosting de forma personalizada mediante el fichero .htaccess.

## Ventajas y desventajas de Apache
## _Ventajas_
- De código abierto y gratuito, con una gran comunidad de usuarios.
- Parches de seguridad regulares y actualizados con frecuencia.
- Estructura basada en módulos.
- Multiplataforma. Está disponible en servidores Windows y Linux.
- Personalización mediante .htaccess independiente en cada hosting.
- Compatible con los principales CMS y tiendas online y plataformas e-learning

## _Desventajas_
- Presenta problemas de estabilidad por encima de las 10000 conexiones
- Un uso abusivo de módulos pueden generar brechas de seguridad.

## Instalación Apache

```
sudo apt update
sudo apt install apache2
```
