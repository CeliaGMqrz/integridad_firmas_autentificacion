# Firmas electrónicas 

En este primer apartado vamos a trabajar con las firmas electŕonicas.

Enlaces de ayuda:

* [Intercambiar claves](https://www.gnupg.org/gph/es/manual/x75.html)
* [Validar otras claves en nuestro anillo de claves públicas](https://www.gnupg.org/gph/es/manual/x354.html)
* [Firmado de claves (Debian)](https://www.debian.org/events/keysigning.es.html)

## GPG

## 1. Manda un documento y la firma electrónica del mismo a un compañero. Verifica la firma que tu has recibido.

* Creamos el par de claves para cifrar el documento pdf

```sh
gpg --gen-key
```
* Ciframos el documento, para cifrarlo nos pide la clave creada

```sh
gpg --sign nat.pdf 

```

```sh
celiagm@debian:~/prueba$ ls
nat.pdf  nat.pdf.gpg

```
* Se la mandamos al compañero y recibimos su documento cifrado y su clave pública

```sh
celiagm@debian:~/prueba$ ls
celiagarcia.asc  documento.odt.gpg  nat.pdf  nat.pdf.gpg  public_jonathan.asc
```
* Importamos la clave pública. 

```sh
celiagm@debian:~/prueba$ gpg --import public_jonathan.asc 
gpg: clave DBFA0443B9C444DB: clave pública "Jonathan Marquez <jonathanmarquezj@gmail.com>" importada
gpg: Cantidad total procesada: 1
gpg:               importadas: 1

```

* Vemos que está importada correctamente

```sh
celiagm@debian:~/prueba$ gpg -k
/home/celiagm/.gnupg/pubring.kbx
--------------------------------
pub   rsa3072 2020-11-04 [SC] [caduca: 2022-11-04]
      3A9AF138559F9BC6A146EEBE7A01A1F808950F41
uid        [  absoluta ] celia garcia
sub   rsa3072 2020-11-04 [E] [caduca: 2022-11-04]

pub   rsa3072 2020-11-04 [SC] [caduca: 2022-11-04]
      43BFFA7AD936E1A7BBEFA8B3DBFA0443B9C444DB
uid        [desconocida] Jonathan Marquez <jonathanmarquezj@gmail.com>
sub   rsa3072 2020-11-04 [E] [caduca: 2022-11-04]

```

* Verificamos la firma y comprobamos que la firma está correcta.

```sh
celiagm@debian:~/prueba$ gpg --verify documento.odt.gpg 
gpg: Firmado el mié 04 nov 2020 13:41:31 CET
gpg:                usando RSA clave 43BFFA7AD936E1A7BBEFA8B3DBFA0443B9C444DB
gpg:                emisor "jonathanmarquezj@gmail.com"
gpg: Firma correcta de "Jonathan Marquez <jonathanmarquezj@gmail.com>" [desconocido]
gpg: ATENCIÓN: ¡Esta clave no está certificada por una firma de confianza!
gpg:          No hay indicios de que la firma pertenezca al propietario.
Huellas dactilares de la clave primaria: 43BF FA7A D936 E1A7 BBEF  A8B3 DBFA 0443 B9C4 44DB

```


## 2. ¿Qué significa el mensaje que aparece en el momento de verificar la firma?

Que la clave pública de nuestro compañero es correcta y pertenece a dicha persona y que se ha importado a nuestro anillo de claves públicas. Además nos indica que la firma no está certificada por una firma de confianza. En este caso no hay problemas de confiaza porque conocemos a esa persona. Nos indica también la fecha y hora y el tipo de cifrado.

## 3. Vamos a crear un anillo de confianza entre los miembros de nuestra clase, para ello.

* Tu clave pública debe estar en un servidor de claves

```sh
gpg --keyserver pgp.rediris.es --send-key 3A9AF138559F9BC6A146EEBE7A01A1F808950F41
gpg: enviando clave 7A01A1F808950F41 a hkp://pgp.rediris.es

```
* Escribe tu fingerprint en un papel y dárselo a tu compañero, para que puede descargarse tu clave pública.

* Te debes bajar al menos tres claves públicas de compañeros. Firma estas claves.

Descargamos las firmas de fran, sergio y javi

```sh
celiagm@debian:~/prueba$ gpg --keyserver pgp.rediris.es --recv-keys DC2F7A96
gpg: clave BEAECEA4DC2F7A96: clave pública "Francisco Javier Martín Núñez <franjaviermn17100@gmail.com>" importada
gpg: Cantidad total procesada: 1
gpg:               importadas: 1

celiagm@debian:~/prueba$ gpg --keyserver pgp.rediris.es --recv-keys 0D5A52C5
gpg: clave CFCF1D130D5A52C5: clave pública "sergio ibañez <sergio_hd_sony@hotmail.com>" importada
gpg: Cantidad total procesada: 1
gpg:               importadas: 1

```
* Si nos la han enviado como archivo por correo las importamos a nuestro anillo de confianza de esta forma

```sh
celiagm@debian:~/prueba$ ls
ce.asc  celia.asc  clave-celia.key  fran.asc  javi.asc  sergio.asc

```
```sh
$ gpg --import nombre.asc
$ gpg --import nombre.key

```

* Firmamos la clave pública con **gpg --sing-key ** 

```sh
celiagm@debian:~/prueba$ gpg --sign-key C67662D3

pub  rsa3072/6F7A456BC67662D3
     creado: 2020-10-21  caduca: 2022-10-21  uso: SC  
     confianza: desconocido   validez: no definido
sub  rsa3072/59B55B2BCFAAE2E6
     creado: 2020-10-21  caduca: 2022-10-21  uso: E   
[no definida] (1). Javier Pérez Hidalgo <javierperezhidalgo01@gmail.com>


pub  rsa3072/6F7A456BC67662D3
     creado: 2020-10-21  caduca: 2022-10-21  uso: SC  
     confianza: desconocido   validez: no definido
 Huella clave primaria: 7627 0D5E 766E 0F22 D704  66BE 6F7A 456B C676 62D3

     Javier Pérez Hidalgo <javierperezhidalgo01@gmail.com>

Esta clave expirará el 2022-10-21.
¿Está realmente seguro de querer firmar esta clave
con su clave: "celia garcia <cg.marquez95@gmail.com>" (7A01A1F808950F41)?

¿Firmar de verdad? (s/N) s


```
* Hacemos el mismo procedimiento con las claves de los otros compañeros y vemos que cambia el estado de 'desconocida' a 'total'

```sh
celiagm@debian:~/prueba$ gpg -k
/home/celiagm/.gnupg/pubring.kbx
--------------------------------
pub   rsa3072 2020-11-04 [SC] [caduca: 2022-11-04]
      3A9AF138559F9BC6A146EEBE7A01A1F808950F41
uid        [  absoluta ] celia garcia <cg.marquez95@gmail.com>
sub   rsa3072 2020-11-04 [E] [caduca: 2022-11-04]

pub   rsa3072 2020-11-04 [SC] [caduca: 2022-11-04]
      43BFFA7AD936E1A7BBEFA8B3DBFA0443B9C444DB
uid        [   total   ] Jonathan Marquez <jonathanmarquezj@gmail.com>
sub   rsa3072 2020-11-04 [E] [caduca: 2022-11-04]

pub   rsa3072 2020-10-07 [SC] [caduca: 2022-10-07]
      D6BE9BF1C7D6A435BF1B9D38BEAECEA4DC2F7A96
uid        [   total   ] Francisco Javier Martín Núñez <franjaviermn17100@gmail.com>
sub   rsa3072 2020-10-07 [E] [caduca: 2022-10-07]

pub   rsa3072 2020-10-06 [SC] [caduca: 2022-10-06]
      547D6FBDF49CD2340F1D5DB6CFCF1D130D5A52C5
uid        [   total   ] sergio ibañez <sergio_hd_sony@hotmail.com>
sub   rsa3072 2020-10-06 [E] [caduca: 2022-10-06]

pub   rsa3072 2020-10-21 [SC] [caduca: 2022-10-21]
      76270D5E766E0F22D70466BE6F7A456BC67662D3
uid        [   total   ] Javier Pérez Hidalgo <javierperezhidalgo01@gmail.com>
sub   rsa3072 2020-10-21 [E] [caduca: 2022-10-21]


```

* Tu te debes asegurar que tu clave pública es firmada por al menos tres compañeros de la clase.

Le pido a los compañeros que me firmen la clave y me la devuelvan la importamos y vemos que está firmada por los compañeros

```sh
celiagm@debian:~/prueba$ gpg --list-sign
/home/celiagm/.gnupg/pubring.kbx
--------------------------------
pub   rsa3072 2020-11-04 [SC] [caduca: 2022-11-04]
      3A9AF138559F9BC6A146EEBE7A01A1F808950F41
uid        [  absoluta ] celia garcia <cg.marquez95@gmail.com>
sig 3        7A01A1F808950F41 2020-11-04  celia garcia <cg.marquez95@gmail.com>
sig          CFCF1D130D5A52C5 2020-11-04  sergio ibañez <sergio_hd_sony@hotmail.com>
sig          BEAECEA4DC2F7A96 2020-11-04  Francisco Javier Martín Núñez <franjaviermn17100@gmail.com>
sig          6F7A456BC67662D3 2020-11-04  Javier Pérez Hidalgo <javierperezhidalgo01@gmail.com>
sub   rsa3072 2020-11-04 [E] [caduca: 2022-11-04]
sig          7A01A1F808950F41 2020-11-04  celia garcia <cg.marquez95@gmail.com>

```


* Una vez que firmes una clave se la tendrás que devolver a su dueño, para que otra persona se la firme.

Le paso su clave a mis compañeros ya firmada

* Cuando tengas las firmas sube la clave al servidor de claves y rellena tus datos en la tabla Claves públicas PGP 2020-2021

Subimos la clave de nuevo a rediris y rellenamos los datos en la tabla.


```sh
celiagm@debian:~/prueba$ gpg --keyserver pgp.rediris.es --send-key 3A9AF138559F9BC6A146EEBE7A01A1F808950F41
gpg: enviando clave 7A01A1F808950F41 a hkp://pgp.rediris.es

```

* Asegurate que te vuelves a bajar las claves públicas de tus compañeros que tengan las tres firmas.

Si me bajo por ejemplo la de javi vemos que está firmado por mas compañeros y además hay otras firmas que aparecen como desconocidas ya que no tengo las claves públicas de esos otros compañeros

```sh
pub   rsa3072 2020-10-21 [SC] [caduca: 2022-10-21]
      76270D5E766E0F22D70466BE6F7A456BC67662D3
uid        [   total   ] Javier Pérez Hidalgo <javierperezhidalgo01@gmail.com>
sig 3        6F7A456BC67662D3 2020-10-21  Javier Pérez Hidalgo <javierperezhidalgo01@gmail.com>
sig          15E1B16E8352B9BB 2020-10-27  [ID de usuario no encontrado]
sig          3E0DA17912B9A4F8 2020-11-03  [ID de usuario no encontrado]
sig          BEAECEA4DC2F7A96 2020-11-03  Francisco Javier Martín Núñez <franjaviermn17100@gmail.com>
sig          7A01A1F808950F41 2020-11-05  celia garcia <cg.marquez95@gmail.com>
sub   rsa3072 2020-10-21 [E] [caduca: 2022-10-21]
sig          6F7A456BC67662D3 2020-10-21  Javier Pérez Hidalgo <javierperezhidalgo01@gmail.com>

```

### 4. Muestra las firmas que tiene tu clave pública

```sh
gpg --list-sign
```
```sh
pub   rsa3072 2020-11-04 [SC] [caduca: 2022-11-04]
      3A9AF138559F9BC6A146EEBE7A01A1F808950F41
uid        [  absoluta ] celia garcia <cg.marquez95@gmail.com>
sig 3        7A01A1F808950F41 2020-11-04  celia garcia <cg.marquez95@gmail.com>
sig          CFCF1D130D5A52C5 2020-11-04  sergio ibañez <sergio_hd_sony@hotmail.com>
sig          BEAECEA4DC2F7A96 2020-11-04  Francisco Javier Martín Núñez <franjaviermn17100@gmail.com>
sig          6F7A456BC67662D3 2020-11-04  Javier Pérez Hidalgo <javierperezhidalgo01@gmail.com>
sub   rsa3072 2020-11-04 [E] [caduca: 2022-11-04]
sig          7A01A1F808950F41 2020-11-04  celia garcia <cg.marquez95@gmail.com>

```

### 5. Comprueba que ya puedes verificar "sin problemas" una firma recibida

Verificamos correctamente ya con una firma conocida 'total'

```sh
celiagm@debian:~/prueba$ gpg --verify documento.odt.gpg 
gpg: Firmado el mié 04 nov 2020 13:41:31 CET
gpg:                usando RSA clave 43BFFA7AD936E1A7BBEFA8B3DBFA0443B9C444DB
gpg:                emisor "jonathanmarquezj@gmail.com"
gpg: Firma correcta de "Jonathan Marquez <jonathanmarquezj@gmail.com>" [total]

``` 


#### 6. Comprueba que puedes verificar con confianza una firma de una persona en las que no confías, pero sin embargo si confía otra persona en la que tu tienes confianza total.

Le pido a Álvaro que me envíe un documento cifrado con su firma y su clave pública.

```sh
celiagm@debian:~/prueba$ gpg --import alvaro.asc 
gpg: key 3E0DA17912B9A4F8: 2 firmas no comprobadas por falta de claves
gpg: clave 3E0DA17912B9A4F8: clave pública "Álvaro Vaca Ferreras <avacaferreras@gmail.com>" importada
gpg: Cantidad total procesada: 1
gpg:               importadas: 1
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: nivel: 0  validez:   2  firmada:   4  confianza: 0-, 0q, 0n, 0m, 0f, 2u
gpg: nivel: 1  validez:   4  firmada:   0  confianza: 4-, 0q, 0n, 0m, 0f, 0u
gpg: siguiente comprobación de base de datos de confianza el: 2022-10-06

```

* Y vemos que podemos verificar la firma sin problemas aunque no tengamos confianza en él

```sh
celiagm@debian:~/prueba$ gpg --verify compilacion1.txt.gpg 
gpg: Firmado el jue 05 nov 2020 12:23:10 CET
gpg:                usando RSA clave B02B578465B0756DFD271C733E0DA17912B9A4F8
gpg: Firma correcta de "Álvaro Vaca Ferreras <avacaferreras@gmail.com>" [total]

```

