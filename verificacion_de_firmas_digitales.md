# Verificación de firmas digitales

> No instale ni utilice software sin antes verificar su firma digital.

## 1. Introducción
La verificación de firmas digitales asegura que un mensaje no haya sido alterado y que el propietario del par de claves sea el remitente quien "firmó" el mensaje. 

Las firmas digitales utilizan criptografía asimétrica: el proceso de firma utiliza la clave privada y el proceso de verificación utiliza la clave pública del mismo par de claves.
Si un mensaje (o un archivo en cualquier otro formato) se ha modificado en su origen o mientras está en tránsito, los hashes serán diferentes y, en este caso (si el hacker publicó nuevos hashes para el mensaje), los hashes no tendrían una firma digital válida.

> Lea [ Una breve noción de criptografía](https://stakey.club/en/a-brief-notion-of-cryptography/) para comprender los conceptos básicos del proceso.

En otras palabras: si el hacker modifica el ejecutable, su hash será diferente al hash en el manifiesto; si son iguales (en caso de que el hacker también haya modificado el manifiesto, porque ambos están almacenados en Github), la firma digital del manifiesto no será válida porque el manifiesto se firmó con la clave privada de los desarrolladores Decred, inaccesible para el hacker. El hacker tendría que cambiar la clave pública en los servidores del MIT, donde se almacena la clave pública de los desarrolladores Decred, y también la página web de Github con las instrucciones para importar la clave pública correcta (porque la ID cambiará si el hacker decide usar su propia llave). Incluso pasando por todos estos problemas, funcionaría solo para aquellos que importan la clave pública después de que el pirata informático la reemplace con la suya.

[imagen]

## 2. ¿Cómo funciona la firma digital? 
### 2.1. Hash
El hash es la salida de una función matemática que siempre devuelve una secuencia de longitud fija independientemente del tamaño de la entrada.

[imagen]

Cualquier modificación en la entrada - el texto, la longitud o el orden de los caracteres - producirá una salida totalmente diferente a la esperada.

### 2.2. Proceso de la firma
El proceso de la firma digital puede ocurrir con o sin un archivo de manifiesto. El manifiesto es un archivo que almacena los hashes de otros archivos, aquellos para los que el proceso está tratando de asegurar la integridad y autenticidad. Con el manifiesto no es necesario "firmar" todos los archivos porque los hash de los archivos están en dentro del el, por lo que este es el único archivo que debe firmarse digitalmente. La firma digital se almacenará en otro archivo, uno que generalmente tiene la extensión de archivo .asc o .sign.
Ejemplo de manifest.txt:

```
f19efd3bc998ad847e206e3eaf393182d2edc458a7329fafb3cd8d9b2cd7ab56  package-linux-386-v1.0.tar.gz
36375985df1ba9a45bc11b4f6cdaed4f14ff6e5e9c46e17ef6e4f70a3349aba2  package-linux-amd64-v1.0.tar.gz
683b80286f50bb2fb77d4d10ac43c736435264958f02f8ae3cbf1fdae31279ba  package-windows-386-v1.0.tar.gz
964db2509dd9bb570f6e7b5e99dc81a7680fc98e1a9fcda8c17b63147648ca88  package-windows-amd64-v1.0.tar.gz
```

Ejemplo de manifest.txt.asc:

```
-----BEGIN PGP SIGNATURE-----

iQIcBAABAgAGBQJcWcxoAAoJEG2Jft9RigMd7MQP/Atp002t+5iwtDVtC0rcO7Qc
elgFzpl/Jkjj/I0RTXDrubUEbFuyyvKXWoGYwqF2LBXqJimBjVPXqpm2p2cOMcWr
wVPtgGLt6cMOD8hP1FHGdj9jSFbqR1fWUR/tQPHZhMuBWh6ic6gndJVsWOkWf2rY
yYv2SbE3JMOT98aNBJKI8m+mYBzPtUednJG0MEt0MeWO33rIMyhjhhQHc5v8N2YL
4WsLM5MwjDiGrEtSrD2raVvcKh0VHDsMKJSaOLV+GESpZHewx91qCJe+VlRBLnyj
SJ5MusUJyEKjjVv9vre1g8ESMjnRV6sV8cQqjWKenpso69ZYSoewuEiiRgos9oFn
xtVGCEqDx/nZPwa4XKaJQWE7aVLeK+bklvFfmAcNkV3/Gs9+6jAvJUtX1IvmFBpH
jLIAAysrhECylyXBURPblL+IZkg2E3eM5wnWUhkNKw+D34ICBN90Lo7g/nEpJnHZ
bBNcIzxYeCJB2bLKXmSTrgUaNrEK+YSBSEg+t0NyuFlKsnKgY/1ZEdxFe2cF/uGs
yI0zhvSuSfhuzz4MJLV9f+qjlCOhiWgknN1C9gDixaKJHScspP4D/6LjNlIjw1zF
YYOtxBM5TK3N5wu+5OJjcxAEnlskxMlQnqhB+8RnLW25NIwVimgWXEqvqojVaOht
2Rlp+5ToMS7wAT4PE3/w
=P8Iv
-----END PGP SIGNATURE-----
```

### 2.2.1. Con manifiesto (indirecto)
a) Se genera el hash (resumen) del archivo (GZIP, en el esquema)
b) El hash se almacena en un archivo de texto `manifest.txt`, que contiene el nombre de cada archivo y su correspondiente hash
c) El manifiesto `manifest.txt` está firmado (encriptado) con la clave privada del editor. Significa que el proceso generará un hash para el archivo de manifiesto y este hash se cifrará con la clave privada del editor. Este hash cifrado se almacenará en otro archivo, `manifest.txt.asc`. Al final del proceso tendremos los archivos `package.tar.gz`, el manifiesto `manifest.txt` y el manifiesto firmado digitalmente `manifest.txt.asc`. 

[imagen]

### 2.2.2. Sin manifiesto (directo) 
a) El archivo `package.tar.gz`  (GZIP, en el esquema) está firmado (encriptado) con la clave privada del editor. Significa que el proceso generará un hash para el archivo que se firmará y este hash se cifrará con la clave privada del editor. Este hash cifrado se almacenará en otro archivo `package.tar.gz.asc.`
b) El package con su correspondiente firma (generalmente un archivo con el mismo nombre y una extensión de archivo más, como `package.tar.gz` y `package.tar.gz.asc` o `package.dmg` y `package.dmg.sign`) están listos para ser usado

[imagen]

## 2.3. Verificación de firma
### 2.3.1. Con manifesto (indirecto) 
a) El proceso de verificación genera un hash del manifiesto `manifest.txt`.
b) El hash firmado (hash cifrado) `manifest.txt.asc` se descifra utilizando la clave pública del remitente y se obtiene el hash original. Si ambos hash son iguales, el mensaje no se alteró y el remitente es quien creemos que es, porque esta clave pública no pudo descifrar un hash cifrado con una clave privada de otro par. En criptografía asimétrica se utilizan ambas claves del mismo par. El proceso comenzó con una tecla y terminó con la otra.
c) El usuario genera un hash del archivo `package.tar.gz` (GZIP, en el esquema) y se compara con los almacenes de hash dentro del manifiesto `manifest.txt`. Si ambos valores son iguales, el archivo no se modificó.
[imagen]

### 2.3.2. Sin manifiesto (directo) 
a) El proceso de verificación genera un hash del archivo descargado `package.tar.gz` (GZIP, en el esquema).
b) El hash firmado (hash cifrado) se descifra utilizando la clave pública del remitente y se obtiene el hash original. Si ambos hash son iguales, el mensaje no se alteró y el remitente es quien creemos que es, porque esta clave pública no pudo descifrar un hash cifrado con una clave privada de otro par. En criptografía asimétrica se utilizan ambas claves del mismo par. El proceso comenzó con una tecla y terminó con la otra.

[imagen]

## 3. ¿Cuáles son los riesgos? 
Los riesgos cambian de acuerdo con el software que pueda haber sido comprometido. En el caso de las billeteras de criptomonedas es posible que el atacante copie la semilla o la clave privada para transferir todos los recursos a otras direcciones. En otros casos, las amenazas incluyen filtración de información, acceso no autorizado a información almacenada por el usuario e incluso acceso remoto o escalada de privilegios según el código malicioso insertado en el software original.

## 4. Verificación de archivos 
> No instale ni utilice software sin antes verificar su firma digital.
Esta recomendación está relacionada con el software en general. Incluso el software más simple, como calculadoras o editores de texto, que no necesitan credenciales administrativas para su instalación o ejecución, puede contener código malicioso que explota una vulnerabilidad en el sistema operativo o kernel. La recomendación más amplia debería ser: no instale software que no sea de confianza en su dispositivo, y cuando decida instalar algo, descárguelo de fuentes oficiales y si es posibles, verifique su firma digital.
Obtenga más información sobre la verificación de firmas digitales para:
- [Decred](enlace en español)
- [Electrum](https://stakey.club/en/verifying-digital-signatures-electrum/)
- [Debian Linux](https://stakey.club/en/verifying-digital-signatures-debian-linux/)

Estos pasos se pueden utilizar para verificar cualquier software descargado en internet. El usuario deberá importar la clave pública de los desarrolladores (es de esperar que toda esta información se muestre en el sitio web). Por lo general, las claves públicas se importan una vez, luego nuevamente solo si el par de claves se revoca o reemplaza.
> Aprenda más sobre [Decred Verifier](traducir...?), [Electrum Verifier](https://stakey.club/en/electrum-verifier/)  y [Debian Verifier](https://stakey.club/en/debian-verifier/) , los scripts de shell también trasladados a Python que automatizan la firma digital y el proceso de verificación de hash.
Obtenga más información sobre esta verificación en el proceso de actualización de macOS y Linux:
- Mac: Cómo verificar la autenticidad de las actualizaciones de software de Apple descargadas manualmente [Inlglés](https://support.apple.com/en-us/HT202369)
- Linux Debian: Debian SecureApt [Inglés](https://wiki.debian.org/SecureApt)
