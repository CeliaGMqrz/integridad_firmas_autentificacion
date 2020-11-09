# Integridad de ficheros

Vamos a descargarnos la ISO de debian, y posteriormente vamos a comprobar su integridad.

Puedes encontrar la ISO en la [direccion](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/)


### 1. Para validar el contenido de la imagen CD, solo asegúrese de usar la herramienta apropiada para sumas de verificación. Para cada versión publicada existen archivos de suma de comprobación con algoritmos fuertes (SHA256 y SHA512); debería usar las herramientas sha256sum o sha512sum para trabajar con ellos.

Vamos a comprobar la integridad de la imagen con las herramientas sha256sum y sha512sum

```sh
celiagm@debian:~/Descargas$ sha256sum debian-10.6.0-amd64-netinst.iso 
2af8f43d4a7ab852151a7f630ba596572213e17d3579400b5648eba4cc974ed0  debian-10.6.0-amd64-netinst.iso
```

Lo comparamos con el que viene en la página:

![sha256sum.png](https://github.com/CeliaGMqrz/integridad_firmas_autentificacion/blob/main/capturas/sha256sum.png)

Y es correcto.

```sh
celiagm@debian:~/Descargas$ sha512sum debian-10.6.0-amd64-netinst.iso 
cb74dcb7f3816da4967c727839bdaa5efb2f912cab224279f4a31f0c9e35f79621b32afe390195d5e142d66cedc03d42f48874eba76eae23d1fac22d618cb669  debian-10.6.0-amd64-netinst.iso
```
Lo comparamos con el que viene en la página:

![sha512.png](https://github.com/CeliaGMqrz/integridad_firmas_autentificacion/blob/main/capturas/sha512.png)

Y es correcto.

Esto lo podemos hacer también bajandonos los ficheros hash y comparandolos con la herramienta

```sh
celiagm@debian:~/Descargas$ sha256sum -c SHA256SUMS 2> /dev/null | grep debian-10.6.0-amd64-netinst.iso
debian-10.6.0-amd64-netinst.iso: La suma coincide


celiagm@debian:~/Descargas$ sha512sum -c SHA512SUMS 2> /dev/null | grep debian-10.6.0-amd64-netinst.iso
debian-10.6.0-amd64-netinst.iso: La suma coincide

```

### 2. Verifica que el contenido del hash que has utilizado no ha sido manipulado, usando la firma digital que encontrarás en el repositorio. Puedes encontrar una guía para realizarlo en este artículo: [How to verify an authenticity of downloaded Debian ISO images](https://linuxconfig.org/how-to-verify-an-authenticity-of-downloaded-debian-iso-images)


Para verificar el contenido del hash utilizamos la firma digital que está en el repositorio, nos las bajamos e intentamos verificarlas. Nos dirá que no se puede comprobar la firma ya que no disponemos de la clave pública.

```sh
celiagm@debian:~/Descargas/hash$ ls
SHA256SUMS  SHA256SUMS.sign  SHA512SUMS  SHA512SUMS.sign

celiagm@debian:~/Descargas/hash$ gpg --verify SHA512SUMS.sign 
gpg: asumiendo que los datos firmados están en 'SHA512SUMS'
gpg: Firmado el dom 27 sep 2020 02:24:23 CEST
gpg:                usando RSA clave DF9B9C49EAA9298432589D76DA87E80D6294BE9B
gpg: Imposible comprobar la firma: No public key

celiagm@debian:~/Descargas/hash$ gpg --verify SHA256SUMS.sign 
gpg: asumiendo que los datos firmados están en 'SHA256SUMS'
gpg: Firmado el dom 27 sep 2020 02:24:23 CEST
gpg:                usando RSA clave DF9B9C49EAA9298432589D76DA87E80D6294BE9B
gpg: Imposible comprobar la firma: No public key

```
Ahora nos descargamos la clave pública que necesitamos y nos ha salido anteriormente

```sh
celiagm@debian:~/Descargas/hash$ gpg --keyserver keyring.debian.org --recv 6294BE9B
gpg: clave DA87E80D6294BE9B: clave pública "Debian CD signing key <debian-cd@lists.debian.org>" importada
gpg: Cantidad total procesada: 1
gpg:               importadas: 1

```
Ya si podemos verificar la firma porque disponemos de la clave pública

```sh
celiagm@debian:~/Descargas/hash$ gpg --verify SHA512SUMS.sign 
gpg: asumiendo que los datos firmados están en 'SHA512SUMS'
gpg: Firmado el dom 27 sep 2020 02:24:23 CEST
gpg:                usando RSA clave DF9B9C49EAA9298432589D76DA87E80D6294BE9B
gpg: Firma correcta de "Debian CD signing key <debian-cd@lists.debian.org>" [desconocido]
gpg: ATENCIÓN: ¡Esta clave no está certificada por una firma de confianza!
gpg:          No hay indicios de que la firma pertenezca al propietario.
Huellas dactilares de la clave primaria: DF9B 9C49 EAA9 2984 3258  9D76 DA87 E80D 6294 BE9B

celiagm@debian:~/Descargas/hash$ gpg --verify SHA256SUMS.sign 
gpg: asumiendo que los datos firmados están en 'SHA256SUMS'
gpg: Firmado el dom 27 sep 2020 02:24:23 CEST
gpg:                usando RSA clave DF9B9C49EAA9298432589D76DA87E80D6294BE9B
gpg: Firma correcta de "Debian CD signing key <debian-cd@lists.debian.org>" [desconocido]
gpg: ATENCIÓN: ¡Esta clave no está certificada por una firma de confianza!
gpg:          No hay indicios de que la firma pertenezca al propietario.
Huellas dactilares de la clave primaria: DF9B 9C49 EAA9 2984 3258  9D76 DA87 E80D 6294 BE9B

```

* Tarea 4: [Integridad y autenticidad (apt secure)](https://github.com/CeliaGMqrz/integridad_firmas_autentificacion/blob/main/t4_aptsecure.md)
