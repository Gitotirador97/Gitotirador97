![Descripción de la imagen](PortadaBizness.png)
## INTRODUCCION
Walktrough de la Maquina Bizness, pasos que segui para lograr vulnerarla con éxito.<br/>
<br />

## RECONOCIMIENTO
Se hace un escaneado de puertos y se investiga sobre la web con el puerto 80
nmap -p- -sCV --min-rate 5000 -n bizness.htb -Pn
![Descripción de la imagen](Imagen1Bizness.png)
## ANALISIS WEB
Indagaremos en la web por encima en busca de alguna versión o usuarios que nos puedan ser utiles
![Descripción de la imagen](Imagen2Bizness.png)

Al buscar posibles subdirectorios con el comando dirsearch obtenemos una dirección de un login "/control/login"
![Descripción de la imagen](Imagen3Bizness.png)<br/>
Nos llevará a un panel de login el cual si nos fijamos en el margen superior derecho visualizamos que esta usando Apache OFBiz:
![Descripción de la imagen](Imagen4Bizness.png)
![Descripción de la imagen](Imagen5Bizness.png)

## Authentication-Bypass
Al buscar vulnerabilidades contra Apache OFBiz visualizamos lo siguiente **CVE-2023-51467** y **CVE-2023-49070**
Por lo que vemos es vulnerable a una Authentication-Bypass (https://github.com/jakabakos/Apache-OFBiz-Authentication-Bypass)
utilizaremos esta dirección anterior de github para explotar la vulnerabilidad e intentar ganar acceso:
![Descripción de la imagen](Imagen6Bizness.png)<br/>
Nos pondremos a la escucha
![Descripción de la imagen](Imagen7Bizness.png)<br/>
Y lanzaremos el comando
![Descripción de la imagen](Imagen8Bizness.png)
Ganamos acceso!
![Descripción de la imagen](Imagen11Bizness.png)

## Escalada Privilegios
Encontramos en la siguiente ruta un xml donde pone "AdminUserLoginData"
![Descripción de la imagen](Imagen13Bizness.png)<br/>
Tras abrir con cat dicho archivo vemos un hash que parece estar cifrado en SHA
![Descripción de la imagen](Imagen14Bizness.png)
Pero el hash se encuentra en la siguiente ruta"/opt/ofbiz/runtime/data/derby/ofbiz/seg0"cogeremos el hash y lo descifraremos con el script.py 
![Descripción de la imagen](Imagen16Bizness.png)<br/>
(lo he generado con chatgpt para ver si era capaz de lograrlo). Le daremos permisos de ejecución al script
![Descripción de la imagen](Imagen15Bizness.png)
![Descripción de la imagen](Imagen17Bizness.png)<br/>
Parece que funciona correctamente!
![Descripción de la imagen](rootbizness.png)

