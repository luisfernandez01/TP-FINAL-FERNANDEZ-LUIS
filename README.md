# Proyecto Final Laboratorio Kathará

## Quinto registro – Configuración del Servidor DNS
 - Cambios realizados
1. Configuración DNS centralizada
    Instalación y configuración de  un servidor DNS en el hots HA1.
    Creación de todas las zonas directas e inversas para hosts y routers.
    Configuración de Forwarder configurado hacia internet 8.8.8.8 para resolución de nombres externos.
##
2.  Convención de nombres para zonas  internas
    Cada registro de una zona directa sigue la forma:
    dig @192.168.10.1  <nombre_del_router_o_host>.empresa.izo
    Cada registro PTR de una zona inversa sigue la forma:
    dig @192.168.10.1 -x <IP_del_router_o_host>
##  
3. Verificación rápida del servicio DNS
  # Verificar estado del servicio
    systemctl status bind9

  # Verificar que el DNS escucha en el puerto 53
    ss -tulnp | grep :53

## 
4.  Resolución de nombres y consultas
    Se verificó la resolución de nombres con dig, incluyendo pruebas directas e invers>  dig @192.168.10.1 ha1.empresa.izo     # Resolución directa interna
    dig @192.168.10.1 -x 192.168.100.6    # Resolución inversa interna
    dig @192.168.10.1  www.google.com     # Resolución directa externa (Internet)
    dig @192.168.10.1 -x 8.8.8.8          # Resolución inversa externa (Internet)
##
5. Estructura de archivos BIND
    named.conf.local → define todas las zonas directas e inversas.
    named.conf.options → forwarders, acceso a la LAN y opciones generales.
    Archivos de zona directa: db.empresa.izo
    Archivos de zona inversa: db.rev.*(ip)
##
6. Optimización del laboratorio
    Toda la configuración de BIND se encuentra en /ha1/etc/bind/.
    Los archivos se copian automáticamente al iniciar el laboratorio.
    La configuración respeta la nomenclatura de los routers y hosts según la topología.
    Funciona con scripts de Kathará (lstart y lrestart) sin modificaciones.

## Carpeta de imágenes
Todas las imágenes del proyecto están en la carpeta `imagenes`.

### Topología de red
![Topología de red](imagenes/.gitkeep)


