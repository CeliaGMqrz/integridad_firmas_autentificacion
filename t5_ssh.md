# Autentificación: ejemplo SSH

Vamos a estudiar como la criptografía nos ayuda a cifrar las comunicaciones que hacemos utilizando el protocolo ssh, y cómo nos puede servir también para conseguir que un cliente se autentifique contra el servidor. Responde las siguientes cuestiones:

**1. Explica los pasos que se producen entre el cliente y el servidor para que el protocolo cifre la información que se transmite? ¿Para qué se utiliza la criptografía simétrica? ¿Y la asimétrica?**

Existe la criptografía **simétrica** en la que, el cliente y el servidor comparten la misma clave y antes de poder comunicarse deben ponerse de acuerdo en el valor de la clave. Podemos cifrar con claves simétricas con *openssl* y *gpg*.

Las claves simétricas se utilizan para cifrar toda la comunicación durante una sesión SSH. Tanto el cliente como el servidor derivan la clave secreta utilizando un método acordado, y la clave resultante nunca se revela a terceros. El proceso de creación de una clave simétrica se lleva a cabo mediante un algoritmo de intercambio de claves.

Sin embargo, la criptografía **asimétrica**, el cliente y el servidor no comparten clave secreta. La clave de encriptación es pública y es conocida por ambos. La clave para desencriptar es secreta y privada, sólo la conoce el receptor. En otras palabras, el cliente y el servidor tienen dos claves cada uno, la pública se usa para encriptar y la privada se usa para desencriptar.


**2. Explica los dos métodos principales de autentificación: por contraseña y utilizando un par de claves públicas y privadas.**

El método de autentificación por **contraseña** es un sistema tradicional, en el que el usuario entra al sistema poniendo su nombre de usuario y su contraseña. La contraseña se aloja en el fichero /etc/passwd en el caso de Debian. Para cifrar las claves de acceso de los usuarios el sistema emplea un criptosistema irreversible que utiliza la función estándar de C crypt, basada en el algoritmo DES. Cuando el usuario intenta entrar con su contraseña el sistema compara la clave con la que está alojada en el fichero passwd, si coinciden el usuario puede entrar en el sistema.

El método utilizando un **par de claves** privada y pública consiste en que, el usuario tiene un par de claves. La pública se la pasa al servidor y la incluye en el fichero de authorized keys para que cuando haga una conexión ssh al servidor con su clave privada, haga de puzle con la pública y pueda entrar en el sistema. Igualmente en el caso de que mandemos un mensaje cifrado con la clave privada , el receptor de ese mensaje debe tener nuestra clave pública para descifrarlo. En otras palabras la clave pública es la llave que necesitan nuestros receptores para descrifrar nuestros mensajes.

**3. En el cliente para que sirve el contenido que se guarda en el fichero ~/.ssh/know_hosts?**

En ese fichero se encuentran las claves de host DSA de los servidore SSH a los que se accede mediante el usuario. Si la relación entre la IP de la máquina y su nombre cambian ssh nos avisa. Si confiamos en el cambio podemos borrar la entrada en el fichero. De otro modo ssh se negará a efectuar la conexión.


**4. ¿Qué significa este mensaje que aparece la primera vez que nos conectamos a un servidor?**

```sh
 $ ssh debian@172.22.200.74
 The authenticity of host '172.22.200.74 (172.22.200.74)' can't be established.
 ECDSA key fingerprint is SHA256:7ZoNZPCbQTnDso1meVSNoKszn38ZwUI4i6saebbfL4M.
 Are you sure you want to continue connecting (yes/no)? 
```

Nos está diciendo que ese host al que nos queremos conectar no esta establecido o configurado en el sistema. Nos dice cual es la clave y si estamos seguros de que queremos conectarnos a ese host. Si le decimos que sí automaticamente guarda la clave en el fichero antes mencionado y no lo volverá a mostrar a menos que lo eliminemos del mismo.


**5. En ocasiones cuando estamos trabajando en el cloud, y reutilizamos una ip flotante nos aparece este mensaje:**

```sh
 $ ssh debian@172.22.200.74
 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
 @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
 IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
 Someone could be eavesdropping on you right now (man-in-the-middle attack)!
 It is also possible that a host key has just been changed.
 The fingerprint for the ECDSA key sent by the remote host is
 SHA256:W05RrybmcnJxD3fbwJOgSNNWATkVftsQl7EzfeKJgNc.
 Please contact your system administrator.
 Add correct host key in /home/jose/.ssh/known_hosts to get rid of this message.
 Offending ECDSA key in /home/jose/.ssh/known_hosts:103
   remove with:
   ssh-keygen -f "/home/jose/.ssh/known_hosts" -R "172.22.200.74"
 ECDSA host key for 172.22.200.74 has changed and you have requested strict checking.
```
Nos advierte que la identificación ha cambiado, ha habido algún cambio en el fichero known_hosts poque hemos podido acceder desde otro sitio. O bien que alguien ha intentado acceder desde otro lugar y no ha podido.


**6. ¿Qué guardamos y para qué sirve el fichero en el servidor ~/.ssh/authorized_keys?**

Como hemos comentado anteriormente, en el fichero **authorized_keys** guardamos las claves públicas de los usuarios que van a poder acceder a nuestro sistema. Ellos acceden con su clave privada ya que la clave pública se encuetnra en ese fichero.


