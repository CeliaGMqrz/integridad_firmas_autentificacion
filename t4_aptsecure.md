# Integridad y autenticidad (apt secure)

Cuando nos instalamos un paquete en nuestra distribución linux tenemos que asegurarnos que ese paquete es legítimo. Para conseguir este objetivo se utiliza criptografía asimétrica, y en el caso de Debian a este sistema se llama apt secure. Esto lo debemos tener en cuenta al utilizar los repositorios oficiales. Cuando añadamos nuevos repositorios tendremos que añadir las firmas necesarias para confiar en que los paquetes son legítimos y no han sido modificados.

**1. ¿Qué software utiliza apt secure para realizar la criptografía asimétrica?**

apt utiliza unos fuertes mecanismos de seguridad basado en el conocido software de cifrado GPG para verificar que los paquetes distribuidos por los servidores réplica de Debian son los mismos que los que subieron los desarrolladores de Debian.


**2. ¿Para que sirve el comando apt-key? ¿Qué muestra el comando apt-key list?**

Sirve para gestionar las claves de APT que usa pra autenticar paquetes. Los paquetes autenticados mediante estas claves se consideran de confianza.

El comando apt-key list lista todas las claves públicas de confianza que tenemos.

```sh
celiagm@debian:~/github/integridad_firmas_autentificacion$ apt-key list
/etc/apt/trusted.gpg
--------------------
pub   rsa4096 2016-04-12 [SC]
      EB4C 1BFD 4F04 2F6D DDCC  EC91 7721 F63B D38B 4796
uid        [desconocida] Google Inc. (Linux Packages Signing Authority) <linux-packages-keymaster@google.com>
sub   rsa4096 2019-07-22 [S] [caduca: 2022-07-21]

pub   rsa4096 2016-04-22 [SC]
      B9F8 D658 297A F3EF C18D  5CDF A2F6 83C5 2980 AECF
uid        [desconocida] Oracle Corporation (VirtualBox archive signing key) <info@virtualbox.org>
sub   rsa4096 2016-04-22 [E]

pub   rsa2048 2015-10-28 [SC]
      BC52 8686 B50D 79E3 39D3  721C EB3E 94AD BE12 29CF
uid        [desconocida] Microsoft (Release signing) <gpgsecurity@microsoft.com>

/etc/apt/trusted.gpg.d/debian-archive-buster-automatic.gpg
----------------------------------------------------------
pub   rsa4096 2019-04-14 [SC] [caduca: 2027-04-12]
      80D1 5823 B7FD 1561 F9F7  BCDD DC30 D7C2 3CBB ABEE
uid        [desconocida] Debian Archive Automatic Signing Key (10/buster) <ftpmaster@debian.org>
sub   rsa4096 2019-04-14 [S] [caduca: 2027-04-12]

/etc/apt/trusted.gpg.d/debian-archive-buster-security-automatic.gpg
-------------------------------------------------------------------
pub   rsa4096 2019-04-14 [SC] [caduca: 2027-04-12]
      5E61 B217 265D A980 7A23  C5FF 4DFA B270 CAA9 6DFA
uid        [desconocida] Debian Security Archive Automatic Signing Key (10/buster) <ftpmaster@debian.org>
sub   rsa4096 2019-04-14 [S] [caduca: 2027-04-12]

/etc/apt/trusted.gpg.d/debian-archive-buster-stable.gpg
-------------------------------------------------------
pub   rsa4096 2019-02-05 [SC] [caduca: 2027-02-03]
      6D33 866E DD8F FA41 C014  3AED DCC9 EFBF 77E1 1517
uid        [desconocida] Debian Stable Release Key (10/buster) <debian-release@lists.debian.org>

/etc/apt/trusted.gpg.d/debian-archive-jessie-automatic.gpg
----------------------------------------------------------
pub   rsa4096 2014-11-21 [SC] [caduca: 2022-11-19]
      126C 0D24 BD8A 2942 CC7D  F8AC 7638 D044 2B90 D010
uid        [desconocida] Debian Archive Automatic Signing Key (8/jessie) <ftpmaster@debian.org>

/etc/apt/trusted.gpg.d/debian-archive-jessie-security-automatic.gpg
-------------------------------------------------------------------
pub   rsa4096 2014-11-21 [SC] [caduca: 2022-11-19]
      D211 6914 1CEC D440 F2EB  8DDA 9D6D 8F6B C857 C906
uid        [desconocida] Debian Security Archive Automatic Signing Key (8/jessie) <ftpmaster@debian.org>

/etc/apt/trusted.gpg.d/debian-archive-jessie-stable.gpg
-------------------------------------------------------
pub   rsa4096 2013-08-17 [SC] [caduca: 2021-08-15]
      75DD C3C4 A499 F1A1 8CB5  F3C8 CBF8 D6FD 518E 17E1
uid        [desconocida] Jessie Stable Release Key <debian-release@lists.debian.org>

/etc/apt/trusted.gpg.d/debian-archive-stretch-automatic.gpg
-----------------------------------------------------------
pub   rsa4096 2017-05-22 [SC] [caduca: 2025-05-20]
      E1CF 20DD FFE4 B89E 8026  58F1 E0B1 1894 F66A EC98
uid        [desconocida] Debian Archive Automatic Signing Key (9/stretch) <ftpmaster@debian.org>
sub   rsa4096 2017-05-22 [S] [caduca: 2025-05-20]

/etc/apt/trusted.gpg.d/debian-archive-stretch-security-automatic.gpg
--------------------------------------------------------------------
pub   rsa4096 2017-05-22 [SC] [caduca: 2025-05-20]
      6ED6 F5CB 5FA6 FB2F 460A  E88E EDA0 D238 8AE2 2BA9
uid        [desconocida] Debian Security Archive Automatic Signing Key (9/stretch) <ftpmaster@debian.org>
sub   rsa4096 2017-05-22 [S] [caduca: 2025-05-20]

/etc/apt/trusted.gpg.d/debian-archive-stretch-stable.gpg
--------------------------------------------------------
pub   rsa4096 2017-05-20 [SC] [caduca: 2025-05-18]
      067E 3C45 6BAE 240A CEE8  8F6F EF0F 382A 1A7B 6500
uid        [desconocida] Debian Stable Release Key (9/stretch) <debian-release@lists.debian.org>

/etc/apt/trusted.gpg.d/microsoft.gpg
------------------------------------
pub   rsa2048 2015-10-28 [SC]
      BC52 8686 B50D 79E3 39D3  721C EB3E 94AD BE12 29CF
uid        [desconocida] Microsoft (Release signing) <gpgsecurity@microsoft.com>

```


**3. En que fichero se guarda el anillo de claves que guarda la herramienta apt-key?**

Se guarda en el fichero **/etc/apt/trusted.gpg**


**4. ¿Qué contiene el archivo Release de un repositorio de paquetes?. ¿Y el archivo Release.gpg?. Puedes ver estos archivos en el repositorio http://ftp.debian.org/debian/dists/Debian10.1/. Estos archivos se descargan cuando hacemos un apt update.**


El archivo **Release** de un repositorio de paquetes contiene la lista de paquetes con sus respectivas versiones. 

El archivo **Release.gpg** es el que tiene las firmas de los creadores de los paquetes para comprobar que son de auténticos y son de confianza.

Recordamos que:

Con **apt-key**, para poder añadir una clave nueva primero necesita descargarla (debería asegurarse de que está usando un canal de comunicación seguro cuando la consiga), se ejecuta apt-key y después se ejecuta apt-get update para que apt descargue y compruebe los ficheros InRelease o Release.gpg de los archivos de paquetes configurados.


**5. Explica el proceso por el cual el sistema nos asegura que los ficheros que estamos descargando son legítimos.**

Este proceso si nos referimos a apt, el cuál utilizamos normalmente para descargar e instalar paquetes, consta de una cadena de confianza desde el archivo apt hasta el usuario final. **apt-secure** es el último en esta cadena. Confiar en un archivo no significa que los paquetes no contengan código malicioso, pero sí se confía en el responsable del archivo, el cuál es el que asegura la integridad del mismo.

La cadena de confianza de Debian comienza cuando un **mantenedor** sube un nuevo paquete o una versión de un paquete al archivo de Debian. Para que esto sea efectivo se debe firmar con una clave de un mantenedor del registro de claves de los mantenedores de Debian (disponible en el paquete *keyring*). Las claves de los mantenedores son firmados por otros siguiendo otros procedimientos.

Una vez que el paquete enviado se ha verificado e incluido en el archivo, se elimina la firma del mantenedor y se realizan las **sumas de control** del paquete, que se incluyen en el fichero 'packages'. Después se realiza una suma de control de todos los ficheros '**Package**', y se incluyen en el fichero 'Release'. A continuación el fichero '**Release**' se fima con la clave del archivo de la distribución y se distribuye con el resto de paquetes.

El **usuario final** puede comprobar la firma del fichero '**Release**', extraer la **suma de control** de un paquete de él y compararlo con la suma de control del paquete descargado manualmente, o depender de la comprobación automática de **APT**.

**6. Añade de forma correcta el repositorio de virtualbox añadiendo la clave pública de virtualbox como se indica en la documentación.**

Puedes ver esta tarea en [este link](https://unbitdeinformacioncadadia.netlify.app/posts/2020/10/instalaci%C3%B3n-de-virtualbox-en-debian-buster/) en mi blog estático.


* Tarea 5: [Autentificación: ejemplo SSH](https://github.com/CeliaGMqrz/integridad_firmas_autentificacion/blob/main/t5_ssh.md)
