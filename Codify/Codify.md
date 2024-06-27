![Descripción de la imagen](PortadaCodify.png)
## INTRODUCCION
Walktrough de la Maquina Codify, pasos que segui para lograr vulnerarla con éxito.<br/>
<br />

## RECONOCIMIENTO
Empezaremos escaneando los puertos de la máquina victima y visualizamos los siguientes puertos abiertos:
![Descripción de la imagen](Imagen1Codify.png)
![Descripción de la imagen](Imagen2Codify.png)
## ANALISIS WEB
Si observamos en el puerto 3000 hay una web que esta creada con Node.js
![Descripción de la imagen](Imagen3Codify.png)

Pero antes buscaremos sobre esta web subdirectorios interesantes como logins o archivos ocultos, para ello enumeraremos con la herramienta Nikto<br>
![Descripción de la imagen](Imagen4Codify.png)<br>
![Descripción de la imagen](Imagen5Codify.png)<br>
Visualizamos una ruta de configuración que al parecer esta hecho en Wordpress pero sin éxito:
![Descripción de la imagen](Imagen6Codify.png)<br>
Vamos a utilizar otra herramienta ya conocida como dirsearch en busca de mas subdirectorios sobre el puerto 3000 y visualizamos un "About" y "Editor"
![Descripción de la imagen](Imagen7Codify.png)

## Arbitrary JS Code
Al parecer hay una vulnerabilidad con esa librería vm2 que nos permite insertar código arbitrario con JS **POC** (https://www.bleepingcomputer.com/news/security/new-sandbox-escape-poc-exploit-available-for-vm2-library-patch-now/?source=post_page-----7c9b3c0dfef5--------------------------------) y **Github** (https://gist.github.com/leesh3288/381b230b04936dd4d74aaf90cc8bb244)
Si ponemos un simple /etc/passwd visualizamos que efectivamente se puede ver los usuarios de dicha maquina
![Descripción de la imagen](Imagen9Codify.png)<br/>
Por lo que ahora probaremos hacer una revershell a nuestra maquina local
![Descripción de la imagen](Imagen13Codify.png)<br/>
Al conseguir una consola como usuario svc del servidor web vemos que en la carpeta var/www/contact hay un archivo llamado tickets.db y visualizamos un usuario llamado Joshua y un hash
![Descripción de la imagen](Imagen14Codify.png)
Meteremos el hash en un archivo llamado joshua.txt para descifrarlo con john y el diccionario de datos rockyou.txt
![Descripción de la imagen](Imagen15Codify.png)
![Descripción de la imagen](Imagen16Codify.png)
Probemos con un ssh con dicho usuario hacia la máquina objetiva:
ssh joshua@10.10.11.239 y la contraseña respectiva:
Ganamos acceso!
![Descripción de la imagen](Imagen17Codify.png)

## Escalada Privilegios
Al hacer "sudo -" visualizamos que este usuario tiene permisos en la siguiente ruta:
![Descripción de la imagen](Imagen18Codify.png)<br>
Al leer dicho archivo al parecer debemos generar un script en Python que haga fuerza bruta para conseguir la contraseña del usuario del SQL<br>
[IMPORTANTE Explicación!]<br>
Esto significa que la entrada del usuario (USER_PASS) se trata como un patrón y, si incluye caracteres globales como * o ?, potencialmente puede coincidir con cadenas no deseadas.

Por ejemplo, si la contraseña real (DB_PASS) es contraseña123 y el usuario ingresa * como contraseña (USER_PASS), la coincidencia del patrón será exitosa porque * coincide con cualquier cadena, lo que resulta en un acceso no autorizado.

Esto significa que podemos aplicar fuerza bruta a cada carácter en DB_PASS
![Descripción de la imagen](Imagen19Codify.png)
Script en python que hace esto mismo<br>
![Descripción de la imagen](Imagen20Codify.png)<br>
Ejecutamos el script<br>
![Descripción de la imagen](Imagen21Codify.png)
Al hacer ya 'su root' y poner la contraseña descifrada con el script ejecutado anteriormente que funciona con fuerza bruta conseguimos acceso y bandera de root!
![Descripción de la imagen](Imagen22Codify.png)
