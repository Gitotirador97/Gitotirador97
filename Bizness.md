Se hace un escaneado de puertos y se investiga sobre la web con el puerto 80
![](Pasted%20image%2020240411154939.png)
![](Pasted%20image%2020240411155111.png)
Al buscar posibles subdirectorios con el comando dirsearch obtenemos una dirección de un login "/control/login"
![](Pasted%20image%2020240411155214.png)
![](Pasted%20image%2020240411155502.png)
En el margen superior derecho visualizamos que esta usando Apache OFBiz
![](Pasted%20image%2020240411155544.png)
Al buscar vulnerabilidades contra Apache OFBiz visualizamos lo siguiente **CVE-2023-51467** y **CVE-2023-49070**

Por lo que vemos es vulnerable a una Authentication-Bypass (https://github.com/jakabakos/Apache-OFBiz-Authentication-Bypass)
Por lo que utilizaremos esta dirección anterior de github para explotar la vulnerabilidad e intentar ganar acceso
![](Pasted%20image%2020240411155833.png)
Nos pondremos a la escucha
![](Pasted%20image%2020240411155908.png)
Y lanzaremos el comando
![](Pasted%20image%2020240411160540.png)
Ganamos acceso!
![](Pasted%20image%2020240411160608.png)
Flag User!
![](Pasted%20image%2020240411160839.png)
**Escalada Privilegios**
Encontramos en la siguiente ruta un xml donde pone "AdminUserLoginData"
![](Pasted%20image%2020240411161032.png)
Tras abrir con cat dicho archivo vemos un hash que parece estar cifrado en SHA
![](Pasted%20image%2020240411161235.png)
Pero el hash se encuentra en la siguiente ruta"/opt/ofbiz/runtime/data/derby/ofbiz/seg0"cogeremos el hash
![](Pasted%20image%2020240411162912.png)
y lo descifraremos con el script.py cogido de internet (Documentado con el nombre "Script descifrar Hash SHA-1")
Le daremos permisos de ejecución al script
![](Pasted%20image%2020240411162119.png)
![](Pasted%20image%2020240411163053.png)
Ya tenemos la contraseña de administrador vamos a probarla, Tenemos flag de root!
![](Pasted%20image%2020240411163857.png)
