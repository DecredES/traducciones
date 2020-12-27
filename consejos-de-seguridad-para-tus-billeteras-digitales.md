# Consejos de seguridad para tus billeteras digitales

## 1. Introducción
En este artículo se analiza los controles de seguridad que reducen principalmente el riesgo de acceso no autorizado o uso indebido, fuga de información y acceso físico no autorizado. Estos controles pueden aplicarse en cualquier situación, pero nos centraremos en los controles que reducen el riesgo de que un atacante acceda a la clave privada o semilla de una billetera digital.

> Todos quieren que haya respuestas simples en seguridad, pero a veces no hay respuestas simples.
>
> -Tavis Ormandy (@taviso) [15 de diciembre del 2017](https://twitter.com/taviso/status/941724872169766912?ref_src=twsrc%5Etfw)

### 1.1. Estrategia
Para un uso más fácil y una mejor gestión de riesgos, las billeteras se pueden segregar. Idealmente, la billetera principal (la que contiene la mayoría de sus recursos) debe estar aislada del dispositivo que se uso diario como si fuera una caja fuerte. Su computadora puede tener otra billetera para pagos diarios y en su celular otra para pagos pequeños cuando está fuera de casa. Hay varias opciones para aumentar la "seguridad":

- a) Una billetera de hardware, como lo es [Ledger](https://www.ledger.com/) o [Trezor](https://trezor.io/) (si la moneda es compatible) 
- b) Un hardware dedicado, como una computadora portátil 
- c) Arranque dual en una unidad USB o disco duro externo (necesita evaluar el espacio libre en el disco si se va a alojar varias blockchains) 
- d) Un sistema virtual (dedicado) 
- e) Una billetera de papel (no para mineros PoS)
- f) Dispositivo compartido con otras actividades (este siendo el menos seguro)

El usuario que elija una de las estrategias de la (a) a la (e), ya resolvió la mayoría de sus problemas y puede leer las recomendaciones de la sección 3 para obtener más información sobre la seguridad de la información. Los usuarios que compartieron un dispositivo en donde se aloja una billetera digital con otras actividades, deben leer la sección 3 con mucha atención.

### 1.2. Enfoque
En este artículo, los controles de seguridad sugeridos utilizan un enfoque en lista blanca (whitelist)

### 1.2.1. Blacklist
Un enfoque de lista negra (blacklist) define todo lo que está prohibido, haciendo que todo lo demás esté permitido.

### 1.2.2. Whitelist
Un enfoque en lista blanca (whitelist) define todo lo que está permitido. Todo lo demás no, explícitamente, estará prohibido. Este es un enfoque más práctico y seguro. Muy seguro porque no es posible imaginar todo tipo de ataques que se puedan ejecutar, todos los sitios web maliciosos, todo el software infectado. En algún momento, uno de ellos pasará por alto la lista negra (blacklist). Es más práctico porque es más rápido y fácil permitir solo un pequeño conjunto de conexiones, sitios web, extensiones de navegador, etc., que tratar de bloquear todas las amenazas que surgen todos los días.

## 2. Modelos de amenazas 
Algunas de las amenazas consideradas para la recomendación de controles de seguridad en la sección 3 son:

### 2.1. Acceso lógico no autorizado 
Dependiendo de la vulnerabilidad, es posible que el atacante acceda al archivo de la billetera, capture las teclas usadas y los movimientos del mouse, tome capturas de pantalla y lea el contenido de la memoria para obtener toda o parte de la clave privada o semilla.

### 2.2. Fugas de información
Incluso si el atacante no puede acceder a la clave privada o semilla, puede intentar obtener la mayor cantidad de información disponible para intentar iniciar sesión en plataformas de Exchanges (casas de cambio) y transferir recursos, mediante la búsqueda de correos electrónicos, contraseñas y otra información sobre el usuario que pueda ayudarlo a responder preguntas de seguridad en caso de pérdida de contraseña.

### 2.3. Acceso físico no autorizado
Si el dispositivo incluye opciones de arranque dual, es posible arrancar un sistema operativo Linux en una unidad USB y copiar los archivos de la billetera desde el disco duro. Ver recomendación 3.18.

## 3. Recomendaciones
> Descargo de responsabilidad: Las siguientes recomendaciones se basan únicamente en estudios, experiencia profesional y preferencias personales. Siempre **haga su propia investigación, evalúe sus riesgos y realice copias de seguridad** antes de implementar cualquier control de seguridad.

Muchas recomendaciones aquí son muy difíciles de implementar para el usuario promedio. En un mundo ideal, la seguridad de la información sería un tema muy sencillo, como marcar una casilla de verificación. Desafortunadamente, la seguridad no siempre es trivial. El usuario promedio debe, al menos, seguir las recomendaciones de las secciones 3.1, 3.2, 3.4, 3.5, 3.6, 3.9, 3.11, 3.12, 3.13, 3.14, 3.15, 3.17, 3.19. Idóneamente, casi toda la lista.

### 3.1. No utilice software pirata
Cualquier experto dirá que casi todas, si no todas, las copias pirateadas de Windows están infectadas con algún código malicioso. Hace unos años el objetivo era robar las credenciales de la banca por internet. Incluso en este tema los bancos están perdiendo espacio. Es mucho más fácil para el hacker robar monedas digitales. No dejan rastros, no dependen de otros controles de seguridad como los bancos y han sido ampliamente adoptados en el mercado. Estos códigos maliciosos generalmente evitan que el antivirus detecte su presencia, así que nunca confíe en que el antivirus se ejecuta en una copia pirateada en Windows.
La misma recomendación se aplica a otros programas pirateados (crackeados): no los use, pueden estar infectados. Algunos de ellos requieren credenciales administrativas, aunque solo sea durante la instalación. Otros intentarán aprovechar las vulnerabilidades del sistema operativo o leer claves y contraseñas de la memoria. Incluso hay quienes intentarán ubicar en la carpeta del usuario archivos de contraseña y billeteras para transferir recursos al atacante.

### 3.2. No utilice credenciales administrativas con regularidad
Cualquier tarea realizada con privilegios administrativos tiene mucho más alcance dentro del sistema operativo que con los privilegios de usuario normales. Los usuarios habituales tienen acceso a su propio entorno, mientras que los usuarios administrativos pueden hacer casi cualquier cosa dependiendo del sistema operativo. Mantenga las credenciales administrativas reservadas para tareas administrativas.

## Linux
En Linux, cree un usuario normal. Instale sudo y agregue el usuario al grupo sudo (grupo de usuarios que pueden ejecutar el comando sudo). A partir de ese momento, use sudo para ejecutar comandos que requieran privilegios de root.

```
$ apt-get install sudo
$ usermod -aG sudo [username]
```

Para verificar si el usuario es miembro del grupo sudo:

```
$ groups [username]
```

## Mac y Windows
En Mac y Windows, debe iniciar sesión con un usuario habitual y proporcionar las credenciales administrativas cuando sea necesario.

### 3.3. Configurar un firewall
El firewall es otro control de seguridad y no resolverá todos los problemas. El propósito del firewall es filtrar paquetes y conexiones no deseados. Como regla general, no se debe permitir ninguna conexión si no hay ninguna razón para hacerlo.

## Linux
Los usuarios de Linux ya tienen un firewall nativo dentro del kernel: iptables. Como recomendación general, se aplican las siguientes reglas (las listas a continuación no son exhaustivas):

```
$ sudo iptables -A INPUT -i lo -j ACCEPT
$ sudo iptables -A INPUT -m state --state INVALID -j DROP
$ sudo iptables -A INPUT -m state --state ESTABLISHED -j ACCEPT
$ sudo iptables -A INPUT -j LOG
$ sudo iptables -P INPUT DROP
$ sudo iptables -P FORWARD DROP
```

Las reglas con el destino DROP rechazan todos los paquetes sin respuesta; la segunda regla permite solo el tráfico localhost (interno) y la cuarta regla solo permite el tráfico relacionado con las conexiones iniciadas por el dispositivo. Podría aplicarse una regla para restringir la salida de paquetes para evitar que el código malicioso se comunique con el atacante. Esta regla no tendría prácticamente ningún efecto ya que muchos de estos códigos se comunican a través de HTTPS (TCP / 443) y en canales ofuscados. Sería necesario bloquear todo el tráfico saliente a internet y evitar incluso las actualizaciones a través de HTTPS.

```
$ sudo iptables -A OUTPUT -o lo -j ACCEPT
$ sudo iptables -A OUTPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
$ sudo iptables -A OUTPUT -m multiport -p tcp --dports 80,443 -j ACCEPT
$ sudo iptables -A OUTPUT -m multiport -p tcp --dports 9108,9109 -m comment --comment "dcrd p2p, RPC" -j ACCEPT
$ sudo iptables -A OUTPUT -m multiport -p tcp --dports 19108,19109 -m comment --comment "testnet dcrd p2p, RPC" -j ACCEPT
$ sudo iptables -A OUTPUT -p udp --dport 53 -m comment --comment "DNS Queries" -j ACCEPT
$ sudo iptables -A OUTPUT -p tcp --dport 53 -m comment --comment "DNS Extended Queries" -j ACCEPT
$ sudo iptables -A OUTPUT -p tcp --dport 11371 -m comment --comment "GPG HKP" -j ACCEPT
$ sudo iptables -A OUTPUT -j LOG
$ sudo iptables -P OUTPUT DROP
```

Para permitir el acceso externo a dcrd (Decred) es necesario abrir otros puertos en el firewall (ENTRADA de cadena) y configurar el NAT en el enrutador. 

Más información: 

https://github.com/decred/dcrd/blob/master/docs/default_ports.md

El paquete iptables-persistent se puede instalar para guardar las reglas y volver a cargarlas automáticamente después de cada reinicio:

```
$ sudo apt-get install iptables-persistent
```

Para guardar las nuevas reglas, use:

```
$ sudo iptables-save > /etc/iptables/rules.v4
```

## Mac
Los usuarios de Mac pueden intentar usar el [firewall de red nativo pfsense](https://pleiades.ucsc.edu/hyades/PF_on_Mac_OS_X) y habilitar el [firewall de la aplicación nativa](https://support.apple.com/en-us/HT201642), o evaluar las soluciones más amigables como [Little Snitch](https://www.obdev.at/products/littlesnitch/index.html) y [Hands Off](http://www.oneperiodic.com/products/handsoff/).

## Windows
Los usuarios de Windows pueden usar el Firewall de Windows (que es mejor que nada) o buscar una solución más sólida.

### 3.4. Mantenga el sistema operativo y las aplicaciones actualizados
Todos los sistemas operativos y aplicaciones, en todas las plataformas, vienen con vulnerabilidades que se descubren, parchean y actualizan con frecuencia. Mantenga todos sus dispositivos actualizados. Habilite las actualizaciones automáticas o use la herramienta del sistema para buscar actualizaciones todos los días.

## Mac
Abra la App Store y haga clic en Actualizaciones.

## Debian Linux
```
$ sudo apt-get update && apt-get upgrade
```
## Redhat Linux

```
$ sudo yum update
```
## Windows
Ejecute Windows Update (ubicado en algún lugar dentro del Panel de control)

### 3.5. Elija contraseñas difíciles de adivinar
Una contraseña difícil de adivinar hace que sea casi imposible para el atacante obtenerla mediante la fuerza bruta durante el tiempo necesario para probar todas (o casi todas) las combinaciones posibles. Dichas contraseñas tendrían más de 14 caracteres, letras mayúsculas [AZ], letras minúsculas [az], números [0-9] y símbolos [! @ # $% ^ & * ();: - = +], para nombrar unos pocos. Nunca use palabras de ningún diccionario, en ningún idioma. Nunca utilice datos que el atacante pueda obtener en bases de datos de proveedores de servicios como nombres, correos electrónicos, fechas de nacimiento, etc.

[MasterPassword](https://masterpassword.app/) es una aplicación que ayuda a quienes tienen dificultades para crear nuevas contraseñas. Genera nuevas contraseñas a través de un algoritmo que combina una contraseña maestra (que debe ser elegida y memorizada por el usuario) y el nombre de los sitios a los que se accederá. No realiza ningún tipo de conexión externa y no almacena las contraseñas de forma local. Todo el proceso es determinista, nada es aleatorio, por lo que se puede reproducir en otros dispositivos sin necesidad de sincronización.

## Frase de contraseña para la billetera
Cree siempre su billetera con una contraseña difícil de adivinar. La billetera es un archivo que reside en el dispositivo y este contiene la clave privada que le permite transferir los recursos. Si el atacante tiene acceso y esta protegida con contraseña, tendrá que descifrarla antes de transferir los recursos.

### 3.6. Utilice 2FA en todas partes
El segundo factor de autenticación (2FA) aumenta en gran medida la protección de las credenciales de inicio de sesión. Se llama segundo factor porque el primero suele ser lo que conoce (contraseña). Otros factores son lo que tienes (un token OTP como ese) y quién eres (autenticación biométrica). OTP significa One-Time Password y tiene este nombre porque se genera un nuevo número cada 30 segundos. Conocer el número anterior no aumenta las posibilidades de predecir el siguiente. El uso de OTP obliga al atacante a pensar la contraseña y este número en una ventana de 30 segundos, lo que hace que el inicio de sesión no autorizado sea prácticamente imposible. Nunca revele la semilla, que generalmente tiene 16 caracteres (o código QR) y se usa para configurar la aplicación en su teléfono, pero haga una copia de seguridad si necesita reinstalarla en otro dispositivo.
Nunca confíes en la seguridad de un sitio web. Algunos almacenan contraseñas en texto plano (sin cifrado, lo que permite a los atacantes leerlas en caso de invasión), otros existen únicamente para recopilar usuarios y contraseñas e intentar robar información del usuario. No use la misma contraseña, incluso con 2FA habilitado.
Las aplicaciones 2FA más utilizadas son [Authy](https://authy.com/) y Google Authenticator, que hace que el código fuente esté disponible en [Github](https://github.com/google/google-authenticator).

### 3.7. Deshabilitar JavaScript
Muchos ataques de navegador dependen de JavaScript. La desactivación completa de Javascript hará que casi todos los sitios web dejen de funcionar. Una solución intermedia es la instalación de la extensión ScriptSafe, que está disponible para Firefox y Chrome. En esta extensión es posible bloquear y permitir (lista negra y lista blanca) sitios web y etiquetas que pueden ejecutar JavaScript. Lleva un tiempo desbloquear solo los necesarios que hacen que los sitios web funcionen, además de aprender los JavaScripts relacionados con los trackers, pero luego puedes exportar la lista e importarla en otros dispositivos.

### 3.8. Habilitar bloqueadores de anuncios
Los anuncios son fragmentos de HTML con JavaScript que provienen de fuentes que no son de confianza. A menudo se utilizan para violar la privacidad de los usuarios (rastreadores) y pueden cargar en el navegador Javascript desconocido. Una extensión que se puede utilizar para bloquear anuncios es [uBlock](https://github.com/gorhill/uBlock).

### 3.9. No hagas clic en todos los enlaces que tienes delante de ti
Siempre que acceda a un sitio web, verifique la dirección en el navegador para asegurarse de que no está accediendo a un sitio de phishing. También debes tener cuidado con los anuncios que aparecen en el motor de búsqueda, como Google, por ejemplo. A menudo, el atacante paga un anuncio para que el sitio de phishing aparezca como primer resultado. En el que suele tener una dirección muy similar junto con una copia fiel a su estructura al original. El objetivo es capturar el nombre de usuario junto con la contraseña que se utilizarán en el sitio original, como los intercambios, por ejemplo. Consulte 2FA en la sección 3.6.

Importante: no existe tal cosa como "Solo voy a navegar, no hago clic en nada". Simplemente abra cualquier página de un sitio web y su código, incluido Javascript, se ejecutará en su navegador.

## URL cortas 
Antes de hacer clic en una URL "acortada", utilice un servicio como [Unshorten.it](https://www.unshorten.it/) para averiguar qué URL se ha acortado y adónde lo llevarán. Este es uno de los sitios que dependen de Javascript para funcionar.

### 3.10. Utilice HTTPS en todas partes
Cuando envíe información confidencial como lo es el nombre de usuario y contraseñas, asegúrese de que el candado (https) esté presente en la barra de direcciones. [HTTPS Everywhere](https://www.eff.org/https-everywhere) es una extensión para Firefox, Chrome y Opera que fuerza las conexiones encriptadas a sitios web conocidos, evitando que algunos objetos se carguen fuera de la conexión encriptada, que incluso puede revelar la dirección IP real del usuario.

### 3.11. Deshabilitar las extensiones del navegador
Las extensiones del navegador pueden leer y cambiar la información ingresada por el usuario, como las contraseñas. Desactive todas las extensiones y conserve solo las necesarias y fiables.

### 3.12. Instale solo las aplicaciones y servicios necesarios
Las nuevas aplicaciones y servicios aumentan la superficie de ataque de un dispositivo. Traen nuevas vulnerabilidades, nuevos puertos abiertos, nuevas configuraciones (que deben fortalecerse) y nuevas dependencias de otras bibliotecas. Aumentan la probabilidad de que un ataque tenga éxito y también el esfuerzo administrativo para mantener el entorno actualizado y seguro.

### 3.13. Use solo software confiable, obtenido de fuentes confiables
En general, la recomendación es descargar aplicaciones solo desde el sitio web del fabricante o desarrollador (o recomendado por ellos). En el caso de las billeteras digitales, el software debe ser seguro en primer lugar y funcional en segundo lugar. Pruebe la billetera proporcionada por los desarrolladores, vea si es de código abierto y se adapta a sus necesidades. Probablemente será la mejor opción.

### 3.14. Verificar firmas digitales
Antes de instalar o utilizar cualquier software, [verifique su firma digital](https://stakey.club/en/verifying-digital-signatures/).

### 3.15. Reinicie el proceso ejecutable del navegador
Inicie un nuevo navegador, sin pestañas abiertas, antes de hacer cualquier cosa relacionada con las criptomonedas. Esto no es lo mismo que abrir una nueva ventana. El proceso (ejecutable) debe cerrarse y volverse abrir. El objetivo es evitar que el código malicioso descargado a través de cualquier sitio web y vinculado al proceso del navegador pueda estar ejecutándose en el momento del acceso a los sitios de intercambio (exchanges). Cuando termine, cierre el navegador (proceso) e inicie otro para otras actividades.

### 3.16. Habilite el aislamiento de procesos y los contextos de seguridad
Si usa Google Chrome, puede habilitar el aislamiento de procesos. Cada sitio abierto se cargará en su propio ejecutable en la memoria, aislado de los datos de otros sitios cargados por otros procesos. Leer más [aquí](https://support.google.com/chrome/answer/7623121?hl=en).
Si usa Firefox no necesita hacer nada. Las versiones más nuevas ya tienen aislamiento de procesos de forma predeterminada. Puede verificar esto siguiendo las instrucciones en [Electrolysis](https://wiki.mozilla.org/Electrolysis) y puede obtener más información sobre [Sandbox](https://wiki.mozilla.org/Security/Sandbox).

## Contextos de seguridad
Los usuarios de Firefox pueden habilitar [contextos de seguridad](https://wiki.mozilla.org/Security/Contextual_Identity_Project/Containers) de espacio aislado, un concepto que suena familiar para aquellos que ya conocen el proceso de virtualización de Qubes OS. Este sandboxing en diferentes contextos de seguridad le da al usuario más control sobre los datos a los que pueden acceder los sitios web (por ejemplo: cookies, localStorage, indexedDB, etc.).

### 3.17. Instalar un antivirus
En algún momento, un usuario normal se comportará como un usuario normal. Por eso es mejor tener instalado algún antivirus. Si este usuario no ejecuta ejecutables aleatorios que no sean de confianza, no es necesario.

> El contexto aquí es tener guías de "cómo no ser pirateado". Si no está ejecutando programas aleatorios que no sean de confianza, el antivirus es una responsabilidad. Si estás ejecutando ejecutables que no son de confianza, te piratearán.
>
> - Tavis Ormandy (@taviso) [16 de noviembre del 2017](https://twitter.com/taviso/status/931010786717155328?ref_src=twsrc%5Etfw)

### 3.18. Cifre todo el disco
Cifrar todo el disco reduce la amenaza de acceso físico no autorizado. Si el dispositivo no será transportado y no se está considerando el acceso físico no autorizado a su entorno, esto no debería ser la principal preocupación. Sin embargo, debe tenerse en cuenta que mientras en Mac es posible habilitar el cifrado en cualquier momento, en Linux es necesario particionar el disco con este soporte ANTES de instalar el sistema o copiar los datos. Esto significa, incluir la seguridad en el diseño, ser proactivo.

- Mac, Filevault2: https://support.apple.com/en-us/HT204837
- Debian, LUKS: https://xo.tc/setting-up-full-disk-encryption-on-debian-jessie.html
- Microsoft, Bitlocker: https://docs.microsoft.com/en-us/windows/device-security/bitlocker/bitlocker-overview

El disco de respaldo se puede cifrar con una solución como [Veracrypt](https://www.veracrypt.fr/en/Home.html).

### 3.19. Realizar copias de seguridad
Las copias de seguridad deben realizarse con regularidad, como recomendación general de seguridad de la información. El problema es que la restauración no es preventiva, es correctiva. Esto significa que una vez que el atacante accede a su clave privada o semilla, no se puede hacer nada. Por lo tanto, aunque los archivos de respaldo pueden resolver muchos problemas, la seguridad de la billetera debe ser completamente preventiva. Recuerde: siempre haga una copia de seguridad de la semilla de la billetera en una hoja de papel, guardada en un lugar seguro.
Hacer una copia de seguridad no significa mover todos los archivos a una memoria USB o disco duro externo. Significa mantener una copia de los archivos en otra ubicación, desconectada de internet. El archivo debe existir en al menos dos lugares al mismo tiempo.
Las copias de seguridad deben realizarse preferiblemente con el dispositivo desconectado de internet, para reducir el riesgo de que un ransomware cifre ambos discos. Un ransomware es un código malicioso que cifra todos los archivos del dispositivo y requiere el pago de un rescate para que el usuario reciba la clave para recuperar sus archivos. Por lo general, este rescate debe pagarse en Bitcoin u otra criptomoneda y no hay garantía de que el usuario pueda volver a acceder a su contenido.

### 3.20. Utilice un sistema operativo (razonablemente) seguro
Los más paranoicos deberían experimentar [Qubes OS](https://www.qubes-os.org/) y [Subgraph](https://subgraph.com/), dos sistemas operativos Linux orientados a la seguridad.

## 4. Restauración
Si sospecha que hay algún problema con su sistema, la única recomendación segura es: vuelva a instalar desde cero (desde el medio de instalación, no desde una copia de seguridad que ya se haya visto comprometida). No existe ningún software, técnica o antivirus capaz de garantizar la eliminación de cualquier código malicioso que pudiera haber sido instalado. La razón es que existe una gran cantidad de códigos maliciosos en el mundo y las empresas de seguridad simplemente no los conocen todos. Además, nadie sabe con certeza cómo funcionan en su totalidad junto con todos los cambios que podrían haber implementado en su sistema.

Durante esta reinstalación, intente no cometer los mismos errores que antes para no reproducir el mismo estado de inseguridad. Sí lo hace, obtendrá los mismos resultados.

Además de la reinstalación, en caso de sospecha de acceso no autorizado, el usuario también debe crear una nueva billetera y transferir todos los recursos de la anterior lo antes posible. De esta forma el usuario evita el riesgo de que un atacante acceda (con o sin contraseña) y transfiera los recursos.

Articulo original: https://stakey.club/en/security-hardening-for-digital-wallets/
