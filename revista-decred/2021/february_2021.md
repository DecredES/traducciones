Lo más destacado de febrero:


- Se lanzó el software v1.6.1, agrega correcciones de errores y mejoras de UX a la versión 1.6

- Se alcanzaron los umbrales de actualización de PoW y PoS, pero no antes de que se lanzara una política de votación de tickets que obliga a los mineros a actualizar

- La participación de PoS alcanzó nuevas alturas y el porcentaje de DCR mixto aumentó del 31% al 38%

- Kohola, un front-end para dcrwallet dirigido a usuarios expertos, fue introducido y lanzado

- Las propuestas de La revista Decred y Open Source Research 2021 fueron aprobadas por votación de los stakeholders

## Versión del patch v1.6.1

La nueva [versión](https://twitter.com/decredproject/status/1364636813168693252?s=21) incluye correcciones de errores y mejoras de UX para dcrd, dcrwallet, Decrediton y dcrdex. La mayoría de los cambios han abordado problemas con la nueva participación de VSP y cómo funciona junto con la mezcla. Es importante destacar que permite a los usuarios de Decrediton establecer opciones de voto por consenso con VSP tanto heredado como nuevo.

Puede descargar y obtener las notas actualizadas [aquí](https://github.com/decred/decred-binaries/releases/tag/v1.6.1), o bien leer la descripción general comprimida de los cambios en las secciones siguientes. Como siempre, respete el ritual de [verificación de firmas](https://docs.decred.org/advanced/verifying-binaries/) para asegurarse de ejecutar exactamente lo que empaquetaron los desarrolladores.

Todavía hay algunas asperezas en el nuevo VSP + combo mixing, consulte esta [publicación](https://www.reddit.com/r/decred/comments/m3k31o/161_things_ive_learned/) de u / mowmowbeans para conocer las soluciones.

Para ponerse al día con las actualizaciones de Decrediton, consulte los tutoriales rápidos en vídeo de [mixing](https://youtu.be/QC65PBNwAK4)  y [staking](https://youtu.be/olWfTqw16OQ) de @Exitus.

## ¡Vota por un nuevo consenso!
La votación para activar la nueva tesorería descentralizada comenzó el 12 de marzo y se extenderá hasta el 9 de abril.

Para informarse, lea la [propuesta](https://proposals.decred.org/proposals/c96290a) original, la [especificación](https://github.com/decred/dcps/blob/master/dcp-0006/dcp-0006.mediawiki) técnica o puede ver la [explicación](https://youtu.be/BdTLKAassvc) de @matheusd (comienza alrededor del minuto 14).

Si aún no ha establecido sus preferencias de voto, porfavor [hágalo ahora](https://www.reddit.com/r/decred/comments/lu1af0/psa_upcoming_decentralized_treasury_consensus/) para que se emita el voto que desee cuando cualquiera de sus tickets vote. La identificación correcta para el nuevo voto es `tesorería`.

Para ver cómo se [desarrolla](https://voting.decred.org/) la votación, vea el panel de votación o la [visualizarlo](https://explorer.dcrdata.org/agenda/treasury) en dcrdata.

## Desarrollo

El trabajo que se informa a continuación tiene el estado "fusionado con el maestro" a menos que se indique lo contrario. Significa que el trabajo se completa, revisa e integra en el código fuente para que los usuarios avanzados puedan [construir y correr](https://medium.com/@artikozel/the-decred-node-back-to-the-source-part-one-27d4576e7e1c), pero aún no está disponible en binarios para usuarios regulares.

### dcrd
La versión del patch v1.6.1 incluyó los siguientes cambios:

- Se [modificó](https://github.com/decred/dcrd/pull/2597) la lógica de notificación para obligar a los mineros de prueba de PoW a actualizar para que pueda comenzar la votación sobre los nuevos cambios de consenso.
- Se solucionó un [problema](https://github.com/decred/dcrd/pull/2582) poco común en el que las conexiones podrían no restablecerse después de una interrupción de la red.

En desarrollo para la [próxima actualización](https://github.com/decred/dcrd/milestone/26), lo más destacado de este mes es la adición de un [caché UTXO](https://github.com/decred/dcrd/pull/2591) que reduce el tiempo de sincronización inicial de blockchain en ~ 33% a costa de algo de memoria adicional. También acelera significativamente el procesamiento de bloques durante el funcionamiento normal y reduce el desgaste del medio de almacenamiento al permitir que las entradas que se crean y gastan entre intervalos de descarga para evitar que se escriban en absoluto. La caché UTXO es un paso necesario hacia la descarga de bloques paralelos de [múltiples pares](https://github.com/decred/dcrd/issues/1145).

Otro punto a destacar es la [introducción](https://github.com/decred/dcrd/pull/2579)*** de los filtros Age-Partitioned Bloom. APBF es un dispositivo de búsqueda que puede saber rápidamente si contiene un elemento. Puede rastrear una gran cantidad de datos con poca memoria, a costa de una tasa controlada de falsos positivos. A diferencia de los filtros Bloom clásicos, puede manejar una cantidad ilimitada de datos envejeciendo y descartando elementos antiguos. La motivación para agregar APBF a Decred es reemplazar la caché LRU (menos utilizada recientemente) que se usa para rastrear continuamente qué datos se sabe que tienen otros pares. Un ejemplo concreto es la memoria para rastrear direcciones conocidas por 125 pares se reduce de ~ 200 a ~ 5 MiB cuando se utilizan APBF. Además, el paquete será [útil](https://matrix.to/#/!zefvTnlxYHPKvJMThI:decred.org/$ulmCeMPuDSSmt3KTB09fJpypa4fpooFILTXKtQNWUCg) para otros proyectos como wallets, DEX, wallets móviles, etc. Lea los detalles en todo su esplendor en [PR](https://github.com/decred/dcrd/pull/2579) y el archivo [README](https://github.com/decred/dcrd/tree/master/container/apbf).

Otro trabajo fusionado:

- Las operaciones primarias en el paquete `apbf` se hace dos veces [más rápido](https://github.com/decred/dcrd/pull/2584) con matemáticas inteligentes a nivel de bits (smart bit-level math).
- [seguimiento](https://github.com/decred/dcrd/pull/2580) de recientes confirmaciones en transacciones y [direcciones](https://github.com/decred/dcrd/pull/2583) conocidas por pares cambiados para usar APBF
- el seguimiento de transacciones recientemente [rechazadas](https://github.com/decred/dcrd/pull/2590) también se cambiaron a APBF, con los beneficios adicionales de reducir el uso de ancho de banda en escenarios de alto rechazo y al mismo tiempo aumentar la solidez contra pares maliciosos
- `rechazar` el mensaje en [desuso](https://github.com/decred/dcrd/pull/2586) para su eventual eliminación en futuras versiones (lea [esto](https://github.com/decred/dcrd/issues/2546) para obtener una lista de problemas de privacidad y corrección con este mensaje).

### dcrwallet

Cambios realizados para al patch v1.6.1:

- nuevo método gRPC para establecer la [opción de voto](https://github.com/decred/dcrwallet/pull/1981) por tickets en el VSP asociado a través del nuevo protocolo vspd (utilizado por Decrediton)
- cuando la cuenta solo tiene una salida, cree una transacción adicional para generar suficientes [salidas](https://github.com/decred/dcrwallet/pull/1980) para la compra de un ticket de VSP y use la salida de [tarifa](https://github.com/decred/dcrwallet/pull/2005) resultante (corrige el [error](https://www.reddit.com/r/decred/comments/lebx71/error_when_trying_to_purchase_tickets_with/) "no hay suficientes utxos")
- [omitir](https://github.com/decred/dcrwallet/pull/1985) transacciones no publicadas al seleccionar salidas y borrar el estado no publicado cuando se extrae el tx (corrige ciertos casos de doble gasto)
- considere las [tarifas de tx](https://github.com/decred/dcrwallet/pull/1992) al mezclar para evitar salidas no mezclables
- productos fijos de transacciones no publicadas que se incluyen en el saldo [gastable](https://github.com/decred/dcrwallet/pull/2002)
- se corrigió la posibilidad de pagar una [tarifa alta](https://github.com/decred/dcrwallet/pull/2002) durante la mezcla (esto fue una regresión entre la v1.6.0 y la v1.6.1, es decir, no afectó a ninguna versión)
- Se actualizaron las [dependencias](https://github.com/decred/dcrwallet/pull/1998) salsa20 y blake2b para evitar posibles daños en la memoria.
- Devuelve el [error](https://github.com/decred/dcrwallet/pull/1990) de `signrawtransaction` si la transacción no tiene entradas.
- [guarda](https://github.com/decred/dcrwallet/pull/1989) transacciones de salida mixtas y salidas vistas.

### Decrediton

Cambios realizados para el patch v1.6.1:

- configuración de la [elección](https://github.com/decred/decrediton/pull/3200) de voto por consenso implementada para tickets vspd
- [deshabilitación](https://github.com/decred/decrediton/pull/3231) de algunos botones mientras se ejecutan al mezclador, como la compra automátizada o la compra de tickets para evitar posibles problemas futuros
- deshabilitación de la [compra automatica](https://github.com/decred/decrediton/pull/3182) al cambiar entre los modos heredado y vspd para evitar compras inesperadas de tickets
- la agregación de comprobaciones de [sanidad](https://github.com/decred/decrediton/pull/3230) para la compra de tickets y así evitar que los tickets VSP heredados estén mal formados
- soporte para filtrar transacciones por múltiples [criterios](https://github.com/decred/decrediton/pull/3194)
- diseño mejorado de [estadísticas](https://github.com/decred/decrediton/pull/3205) de staking e [historial de tx](https://github.com/decred/decrediton/pull/3195) para pantallas pequeñas
- mostrar diferentes iconos para [cuentas](https://github.com/decred/decrediton/pull/3225) mixtas y no mezcladas
- mostrar un [mensaje](https://github.com/decred/decrediton/pull/3252) significativo que explique por qué a veces se compran menos tickets de los solicitados
- muestra un error de [tiempo de espera](https://github.com/decred/decrediton/pull/3199) cuando no se recibe la respuesta de estado de VSP en 5 segundos
- mostrar saldos [no confirmados](https://github.com/decred/decrediton/pull/3254) en Resumen y Cuentas
- mejoramiento en [nombres](https://github.com/decred/decrediton/pull/3219) para pestañas y etiquetas
- añadida la [traducción](https://github.com/decred/decrediton/pull/3086) al chino tradicional
- migración continua a componentes funcionales mediante ganchos y módulos CSS
- mayor cobertura de pruebas automatizadas
- 22 correcciones de errores

## Comunidad

Damos la bienvenida a los nuevos colaboradores con código fusionado en la rama master @bochinero ([dcrlnd](https://github.com/decred/dcrlnd/commits?author=bochinchero)) y @fintechtrades ([dcrdocs](https://github.com/decred/dcrdocs/pull/1150))

@arij fue entrevistada en el podcast Decred in Depth, chequelo en [Media](https://xaur.github.io/decred-news/journal/202102.html#media).


Estadísticas de la comunidad a partir del 1 de marzo Seguidores en:

- [Twitter](https://twitter.com/decredproject): 42 922 (+1 121)

- Suscriptores en [Reddit](https://www.reddit.com/r/decred/): 10,559 (+358)

- Usuarios en la sala #general de [Matrix](https://chat.decred.org/): 382 (+38)

- Usuarios en [Discord](https://discord.com/invite/GJ2GXfz): 1,444 (-473) - eliminación de usuarios inactivos 

- Usuarios en [Telegram](https://t.me/Decred): 2,518 (+90)

- Suscriptores en [YouTube](https://www.youtube.com/decredchannel): 4,420 (+100), visualizaciones: 175 000 (+6 000)

- Seguidores en [LinkedIn](https://www.linkedin.com/company/decredproject/mycompany/): 978 (+16)

- Estrellas en el repositorio [dcrd](https://github.com/decred/dcrd) en GitHub: 584 (+9), forks: 252 (+3)


Cambios inusuales detectados entre las cuentas [rastreadas](https://github.com/decredcommunity/social-media-stats):


- Twitter, Reddit y YouTube mencionados anteriormente obtuvieron el aumento más alto de los seguidores y vistas de lo habitual.

- La página [CoinMarketCap](https://coinmarketcap.com/currencies/decred/) consiguió otros + 3 000 observadores a 29 000

- Surgieron en enero Las cuentas de Twitter de [DecredKorea](https://twitter.com/DecredKorea) y [DecredNederland](https://twitter.com/DecredNederland) y comenzaron a twittear en coreano y holandés.

- [DecredDEX](https://twitter.com/DecredDEX) se ha unido a [decredexplorer](https://twitter.com/decredexplorer) y ahora tenemos dos cuentas de Twitter enfocadas en productos

- [@Checkmate](https://twitter.com/_Checkmatey_) ganó + 30% de seguidores (a 5 000) y publicó 1 200 tweets a una tasa loca de ~ 41 / día

- [ConsensusRough](https://twitter.com/ConsensusRough) obtuvo + 18% de seguidores, llegando a 438

## Gobernanza

En febrero, el [fondo de tesorería](https://explorer.dcrdata.org/address/Dcur2mcGjmENx4DhNqDctW5wJCVyT3Qeqkx) recibió 10 444 DCR y gastó 3 010 DCR (en realidad, los pagos no realizados hasta el 2 de marzo). Utilizando la tasa promedio diaria DCR/USD en febrero de $113.76, esto es $1.2 M recibido y $342 000 gastado. A la tasa diaria promedio de enero 54.25 dólares, la cifra en dólares facturada por trabajos anteriores es de $163 000. A partir del 3 de marzo el balance del fondo de tesorería es de 653 000 DCR (97.2 millones de dólares a $148.86).

Después de la participación récord de dos propuestas a principios de febrero (tratada en el [último edición](https://xaur.github.io/decred-news/journal/202101.html#governance)), el resto del mes fue relativamente tranquilo, y se aprobaron dos propuestas más.

- La [propuesta](https://proposals.decred.org/proposals/1d74b88) de Decred Journal 2021 fue aprobada con un 92,4% de votos a favor y una participación del 51%. El monto máximo de financiamiento solicitado para el año es de $ 39,000 y también cubre Politeia Digest y todo el trabajo de diseño asociado. ¡Gracias a todos los stakeholders  que votaron por la primera propuesta individual de la revista! No dude en compartir sus pensamientos sobre [Reddit](https://www.reddit.com/r/decred/search?q=decred+journal&restrict_sr=on&t=all&sort=new), [GitHub](https://github.com/xaur/decred-news/issues) o [Matrix](https://chat.decred.org/#/room/#writers:decred.org), especialmente si votó por un no (podemos sobrevivir a sus críticas).

- La [propuesta](https://proposals.decred.org/proposals/020b8b0) de Open Source Research 2021 de @ richard-red fue aprobada con un 75,4% de votos a favor y una participación del 60%. El presupuesto solicitado ha aumentado a $ 40,000, y $ 9,150 de esto se reservan para la expansión de la iniciativa de recopilación de datos abiertos de @ bee.

@ammarooni publicó una [reflexión](https://twitter.com/Ammarooni/status/1359352089714061313) sobre su [propuesta](https://proposals.decred.org/proposals/9e1d644) de libro y está buscando retroalimentación honesta.

## Red

Hashrate: el hashrate de [febrero](https://explorer.dcrdata.org/charts?chart=hashrate&zoom=kkf13di6-kltwy2po&scale=linear&bin=block&axis=time9) se abrió a ~348 Ph/s y cerró ~ 406 Ph/s, tocando fondo en 283 Ph/s alcanzando un máximo de 545 Ph/s durante todo el mes.

[Distribución](https://miningpoolstats.stream/decred) del hashrate en los pools a partir del 1 de febrero:

- Poolin 36% 
- Antpool 34% 
- F2Pool 8% 
- Luxor 2% 
- BTC.com 1.5%
- Huobipool 0.8%
- Coinmine 0.08%
- UUPool 0.03%
- unknown 18%.

Las instantáneas son demasiado volátiles, así que compárelo en cómo se han distribuido 1.000 bloques (3,5 días) extraídos antes del 1 de marzo: 
- Antpool 41%
- Poolin 29% 
- easy2mine 11%
- F2Pool 6% 
- Luxor 1,5% 
- Huobipool 0,4%
- unknown 11%.

El hashrate informado de UUPool ha caído desde el punto superior al inferior y parece que sus mineros han migrado a Antpool y F2Pool. También aparecen nuevas direcciones no identificadas.

Staking: el [precio medio del ticket](https://explorer.dcrdata.org/charts?chart=ticket-price&zoom=kkf13di6-kltwy2po&bin=window&axis=time&visibility=true-true) a 30 días fue de 181.7 DCR (+8.5). El precio varió entre  154.3 a 220.4 DCR. El [monto bloqueado](https://explorer.dcrdata.org/charts?chart=ticket-pool-value&zoom=kkf13di6-kltwy2po&scale=linear&bin=block&axis=time) por [participación](https://explorer.dcrdata.org/charts?chart=stake-participation&zoom=kkf13di6-kltwy2po&scale=linear&bin=block&axis=time) fue de 6.74 a 7.30 millones de DCR, lo que correspondió al 53.6 a 57.7% del suministro disponible en PoS.


¡Parece que estamos [batiendo](https://twitter.com/michae2xl/status/1359609723541213186) un récord cada mes en términos de precio de tickets y participación!

Nodos: a lo largo de febrero hubo un promedio de 220 nodos accesibles según dcrextdata. 

Distribución de versiones promedio de dcr.farm:
- 35% dcrd v1.6.0 
- 10% dcrd v1.6.1 
- 10% dcrd v1.5.2 
- 7% dcrd v1.5.1 
- 5% dcrd v1.6 dev y RC builds
- 4% dcrd v1.7 dev builds 
- 2% dcrd v1.5.0 
- 1.5% dcrd v1.5 dev y RC builds 
- 9% dcrwallet v1.6.0 
- 4% dcrwallet v1.6.1 
- 3% dcrwallet v1.5.1 
- ~9% otros.

[charts.dcr.farm](https://charts.dcr.farm/) se ha cerrado debido a la baja demanda y los altos costos operativos para mantener su funcionamiento de manera sostenible. Agradecemos a los operadores por todos los conocimientos de red compartidos a lo largo de los años. El legado de dcr.farm y el nuevo VSP aún están operativos.

La proporción de [monedas mixtas](https://explorer.dcrdata.org/charts?chart=coin-supply&zoom=jzk7rn6n-kmfblq6m&bin=day&axis=time&visibility=true-true-true) ha crecido del 31% al 38% solo en febrero, posiblemente gracias a la GUI que se agregó en la v1.6. [@CoinShuffle_BOT](https://twitter.com/CoinShuffle_BOT) publica estadísticas de la mezcla diariamente.

[Lightning Network](https://ln-map.jholdstock.uk/) de Decred ha visto 30 nodos (+2), 56 canales (+9) con una capacidad total de 16.8 DCR (+8.5), al 1 de marzo.

@matheusd compartió algunas [estadísticas](https://www.reddit.com/r/decred/comments/lj2her/og_ticket/gnj3zqq/) de tickets splitting: durante casi 3 años, se compraron 2.679 tickets splitting a una tasa promedio de 81 / mes, lo que significa que aproximadamente el 0,2% de los tickets se están dividiendo. Esto se discutió en el contexto de v1.6 y el nuevo replanteo vspd donde no se admite tickets splitting. Se requiere un nuevo protocolo de división / coincidencia y una reescritura tanto del servidor como del cliente para que funcione con vspd, lo que es poco probable que suceda en este momento dada la baja demanda.

## Integraciones
Tres VSP han habilitado el soporte para el nuevo staking vspd: [decredvoting.com](https://decredvoting.com/), [ibitlin.com](https://dcrpool.ibitlin.com/) y [coinmine.pl](https://vsp.coinmine.pl/). Ahora tenemos un total de 11 instancias vspd y 17 instancias VSP heredadas.

La [lista de VSP](https://decred.org/vsp/) se actualizó para mostrar los tickets de vspd junto con los anteriores. A partir del 1 de marzo, los servidores de vspd tenían 4 400 tickets en vivo, o el 11% del grupo de tickets objetivo. Los servidores VSP heredados tenían 6 500 tickets en vivo, o el 16% del objetivo.

La plataforma de intercambio [SwapSwop.io](https://swapswop.io/) vino a [saludar](https://www.reddit.com/r/decred/comments/lfgr64/buy_dcr_at_swapswopio_a_crypto_exchange_platform/) en r / decred y respondió algunas preguntas sobre fuentes de liquidez, jurisdicción y sus procesos KYC. DCR se [listo](https://twitter.com/swapswopio/status/1308107303032369152) en septiembre de 2020.

Una nueva sala de chat pública llamada [#services](https://chat.decred.org/#/room/#services:decred.org) da la bienvenida a todos para recopilar información sobre el creciente ecosistema de Decred (intercambios, procesadores de pago, VSP, cualquier cosa).

Advertencia: los autores de la Revista Decred no tienen idea de la confiabilidad de alguno de los servicios anteriores. Haga su propia investigación antes de confiar su información personal o activos.

## Alcance
Varios colaboradores de Decred respondieron todo tipo de preguntas en Decred AMA en [r / CryptoCurrency](https://www.reddit.com/r/CryptoCurrency/comments/lqlone/decred_ama_ask_us_about_privacy_daos_lightning/). El hilo obtuvo 171 comentarios y 140 votos a favor.

@elima_iii ha comenzado la segunda temporada de la serie Decred in Depth, con nuevos invitados y más participación de la comunidad. Las preguntas a los invitados se recopilan en [Twitter](https://twitter.com/elima_iii/status/1363280047059132417) (aún más para el episodio de [@matheusd](https://twitter.com/elima_iii/status/1365424755906670595)), el video tanto del anfitrión como del invitado está disponible y YouTube es la ubicación principal ahora.

@pavel compartió una [actualización](https://github.com/decredcommunity/proposals/blob/master/proposals/2bf72e/updates/20210208.md) para la [propuesta](https://proposals.decred.org/proposals/2bf72e6) withDecred:

> Debido a la acción masiva del precio en las últimas semanas, suspendí los sorteos, ya que parece que ya no es necesario generar entusiasmo en las redes sociales. En el momento de la actividad, la cuenta de Twitter [@withDecred](https://twitter.com/withdecred) tenía alrededor de 100 000 impresiones mensuales y generó algunas buenas conversaciones (y también algo de chelín). (…) Ahora estoy pensando en cómo avanzar desde aquí con las actividades de WithDecred (web + Twitter), por el momento estoy apoyando el contenido creado por la comunidad. Si tiene algunas sugerencias, no dude en enviarme un ping [aquí](https://chat.decred.org/#/room/#proposals:decred.org) o en [Twitter](https://twitter.com/paveldcr).

Cobertura de noticias por parte de Monde PR:
- creó / lanzó 3 historias para las publicaciones financieras y criptográficas
- respondió a 3 solicitudes de comentarios
- aseguró 2 entrevistas con los medios

## Eventos

- 3 de febrero - [Giveaway de 5 años de Decred](https://decredcommunity.github.io/events/index/20210203.1) - Internet. El equipo de Decred en español organizó un sorteo sofisticado para conmemorar el quinto aniversario de Decred. Se otorgó un total de $ 130 en DCR para varias tareas como retweets, responder preguntas (después de buscar algunos datos de [dcrdata](https://explorer.dcrdata.org/)), responder “¿qué te gusta de Decred?” O competir en la creación de memes. Esta actividad involucró a más de 60 personas, generó más de 57 mil impresiones en Twitter, e incorporó a más de 100 seguidores en Twitter y a más de 40 usuarios de [Telegram](https://t.me/DecredES). Detalles del [reporte](https://decredcommunity.github.io/events/index/20210203.1).
- 6 de febrero - [Blockchain, IA y Big Data](https://decredcommunity.github.io/events/index/20210206.1) - Casablanca, Marruecos. @arij y OMJD organizaron un evento para conmemorar el quinto aniversario de Decred. Hubo 3 charlas sobre aprendizaje automático, big data y blockchain. En este último, @arij habló sobre cómo se usó Decred blockchain en las elecciones municipales de Brasil. Se esperaban 20 personas, pero 40 se presentaron y ~ 500 lo vieron en vivo. Al final, hubo una pequeña celebración del aniversario (que involucró… espera, espera).
- 9 de febrero - [Introducción y características de Decred](https://decredcommunity.github.io/events/index/20210209.1) - Internet. @ michae2xl fue invitado por Monnos (intercambio brasileño que incluyó a DCR en [enero](https://xaur.github.io/decred-news/journal/202101.html#integrations)) para presentar a Decred en un en vivo en Instagram. Junto con Rodrigo Soeiro (CEO), se dirigieron a nuevas personas en el espacio, mostraron los productos de Decred y discutieron su potencial.
- 20 de febrero - [Consenso alternativo: Hablemos de PoS](https://decredcommunity.github.io/events/index/20210220.1) - Internet. @elian habló de todo lo relacionado con Decred con [Criptodemia](https://twitter.com/criptodemia) y [Kevin Negocios](https://twitter.com/KevinNegocios) de [Veinte Exchange](https://twitter.com/Veinte_ven) de Venezuela en una transmisión en vivo de más de una hora.

Próximamente:

12-14 de marzo - [Hackathon Nayarit 2021](https://www.facebook.com/EduNayarit/posts/2982126628674008) - Internet. Decred en español patrocinará el hackathon organizado por el Ministerio de Educación de Nayarit. Para preparar a los participantes, el equipo español organizó la semana de capacitación sobre Blockchain Education, que consta de 5 seminarios web.

## Media
Artículos seleccionados:

- Decred v1.6 con el cofundador y líder del proyecto, Jake Yocom-Piatt ([coinscrum.com](https://www.coinscrum.com/decred-with-jake-yocom-piatt/)): un video de enero reelaborado en un artículo, compara el nuevo código del fondo de tesorería con un "contacto inteligente cableado mínimamente complejo" y explica las características clave de la v1. 6 en términos simples
- DCRDEX fue incluido entre los mejores DEX para ver en 2021 en el [reporte](https://xangle.io/research/600a3251b7cb8c849dfa26b9) de investigación de enero de Xangle.

### Videos:

- Actualización de Decred News - 14 de febrero - ¡Lanzamiento masivo de 1.6, vote por $ 75M DAO, LN, privacidad y más! por @Exitus ([youtube](https://www.youtube.com/watch?v=f2ooNJXpR7I))
- Tutorial de privacidad de Decred: mezcla tus monedas con @Exitus ([youtube](https://www.youtube.com/watch?v=QC65PBNwAK4))
- Entrevista de Insaf Nori en Decred in Depth (en vivo) por @elima_iii ([youtube](https://www.youtube.com/watch?v=hUXk1GWhE-0))
- Ser tu propio banco por Society Decentralized ([youtube](https://www.youtube.com/watch?v=Vb-9vvU0fDU))
- Análisis de precios de Decred - 24 de febrero de 2021 por Brave New Coin ([youtube](https://www.youtube.com/watch?v=O6iTIABl2Lw))

### Audio:
- Rough Consensus 17: Reunión de Spidey. El coanfitrión perdido hace mucho tiempo @ mr.black vuelve a unirse al grupo para hablar sobre cripto y finanzas. ([libsyn](https://roughconsensus.libsyn.com/episode-17-spidey-reunion))
- Los episodios anteriores de Decred in Depth (hasta el 33) se han subido al [canal](https://www.youtube.com/decredchannel) principal de YouTube.

### Arte y diversión:
- [Clip](https://twitter.com/karamblez/status/1356921573647745024) de anuncio digital Decred v1.6 de @karamble (¡prueba detectar al "taco"!)
- [Clip](https://twitter.com/New_Copernicus/status/1357574854535487488) glitchy y ruidoso del quinto aniversario por @New_Copernicus
- la última explicación de cómo funcionan los [atomic swaps](https://twitter.com/RichardRed0x/status/1356719226724220930) por @richardred
- ["The Rewards"](https://www.reddit.com/r/decred/comments/lfxqef/the_rewards/) de @ AGNFAB1
- "I wish to be [irrisistible](https://twitter.com/Decred_ES/status/1358855083396653062) to men!!" 
- Presentando a [Decred Pączki](https://twitter.com/LolekBolek74/status/1359866192538787843) (donas).

### Traducciones:

- Actualización de Decred News 24 de enero - con subtítulos en [español](https://www.youtube.com/watch?v=Quf8u1Ksm4M) por @francov_
- Construyendo un futuro transparente con la cadena de bloques Decred, en [árabe](https://insaf01.github.io/decred-arabic/articles/building-a-transparent-future-with-decred-blockchain.html) por @arij y @ abdulrahman4
- Revista Decred de enero del 2021 se [tradujo](https://xaur.github.io/decred-news/) al árabe (@arij, @ abdulrahman4), chino (@Dominic) y español (@francov_). ¡Gracias a todos por difundir las novedades de Decred!

## Discusiones de la comunidad
Recientemente, ha habido más intentos de estafar a la gente. Recuerde que los desarrolladores y administradores reales nunca le enviarán mensajes de correo electrónico con ofertas de dinero gratis o asistencia técnica. Preste mucha atención al nombre para mostrar del usuario y al ID de usuario con el que chatea para evitar formas inteligentes de [suplantar](https://twitter.com/GrapheneOS/status/1365881076229488641) la identidad. Dile a tus amigos que también estén atentos.

Publicaciones seleccionadas de Reddit:

- una idea para cambiar el nombre de los tickets a ["OG Tickets"](https://www.reddit.com/r/decred/comments/lj2her/og_ticket/) y llamar a su décima parte un "ticket" normal
- una de las discusiones sobre [precios](https://www.reddit.com/r/decred/comments/llnzvu/dcr_breaking_through_all_time_high_price_usd/) se volvió inusualmente inteligente y tocó el tema del reclutamiento (lo seguimos diciendo: no solo se [buscan](https://www.reddit.com/r/decred/comments/llnzvu/dcr_breaking_through_all_time_high_price_usd/gnr3kpf/) desarrolladores)
- DCRDEX VS [Bisq](https://www.reddit.com/r/decred/comments/ln7co5/dcrdex_vs_bisq/)
- algunos datos interesantes sobre las [frases semillas](https://www.reddit.com/r/decred/comments/lo72lf/discussion_around_33_word_seed_vs_bip_39/) y wallets metálicas

Debates seleccionados de Twitter:
- @ jy-p recuerda el [por qué](https://twitter.com/behindtext/status/1363118198749597698) todavía estamos aquí
- otro [gran hilo](https://twitter.com/cburniske/status/1362146220123201536) de @cburniske

> Como alguien que ha seguido a @decredproject desde 2016, la fuerza de su comunidad, los fundamentos y la tracción del mercado hasta ahora en 2021 me ha sorprendido incluso a mí. ([@cburniske](https://twitter.com/cburniske/status/1362146231116460032))

## Mercados
En febrero, DCR cotizaba entre 66.95 y 169,90 USD / 0.0018–0.0030 BTC. El promedio de tarifa diaria fue de $113.76.

@Checkmate publicó un [mega](https://twitter.com/_Checkmatey_/status/1358968202898755584) hilo en on-chain con muchos (¿todos?) Indicadores en ATH. Predicción del 9 de febrero "Me sorprendería que el tren se ralentizara desde aquí, ¡simplemente no está en su carácter!" era correcto.

Se han [observado](https://twitter.com/_Checkmatey_/status/1363709306885922826) ofertas inusualmente grandes en Binance.

[DCRDEX](https://dex.decred.org/) ha negociado 390 000 DCR y 1 000 en BTC en febrero, con un promedio de 14 000 DCR y 36 BTC diarios. El precio varió entre 0.0019 y 0.0030 con un promedio de 0.0026.

## Noticias Relevantes
Yearn Finance fue el [fracaso](https://cryptobriefing.com/hacker-spends-8-3-million-fees-attack-yearn-finance/) DeFi más grande de febrero, con un ataque de préstamo flash que explotó a sus usuarios por $ 11 millones. En un desarrollo interesante para este tipo de ataque, la mayoría de los fondos explotados se pagaron en [tarifas](https://twitter.com/FrankResearcher/status/1357639434380992512) a varios pools de liquidez y servicios de staking, con solo $ 2.7 millones terminando en la wallet del atacante, y el resto presumiblemente alimentando la economía de DeFi. Las víctimas del ataque han sido [compensadas](https://twitter.com/iearnfinance/status/1359108691677614080) con el nuevo pool de fondos del la tesorería de YFI que se acuñaron recientemente: dentro de los 4 días posteriores al ataque, el equipo de Yearn pudo movilizar 9,7 millones de DAI para distribuir, y no se requirió una votación simbólica. YFI y Hacienda están actualmente bajo el control directo de los forks Multifirma, y ​​ese “empoderamiento” se [extiende](https://gov.yearn.finance/t/yip-59-temporarily-extend-multisig-empowerment/9746) hasta el 24 de mayo.

Yearn Finance también está [modificando](https://gov.yearn.finance/t/yip-56-buyback-and-build/8929) [significativamente](https://gov.yearn.finance/t/yip-56-buyback-and-build/8929) su modelo de financiación de su tesorería y la gobernanza, los cambios importantes incluyen eliminar el staking de YFI debido a su rendimiento no competitivo y, en su lugar, cambiar a un modelo de recompra y permitir que los forks de YFI que han implementado sus tokens en un pool de liquidez también los utilicen al votar.

Los desarrolladores de Bitcoin están en el [proceso](https://twitter.com/AndreCronjeTech/status/1359934584612212738) de decidir cómo se debe activar la actualización de [Taproot](https://bitcoinmagazine.com/technical/taproot-coming-what-it-and-how-it-will-benefit-bitcoin), y el punto de discusión es si la actualización de Bitcoin Core debe enviarse con una configuración que la active al final del período de señalización, independientemente de si el umbral de supermayoría es conocido (como el Soft Fork activado por el usuario) o adopte un enfoque más cauteloso y evite forzar el problema inicialmente. La señalización de mineros para el mes hasta el 2 de marzo mostró un 89% de apoyo para Taproot de los pools de acuerdo con su tasa de hash. Además, Poolin contribuyó con un esfuerzo de consenso [compilando](https://taprootactivation.com/) el sentimiento de los pools y los métodos de activación preferidos.

El Fiscal General de Nueva York llegó a un acuerdo sobre los cargos contra Tether y Bitfinex, en el que la empresa [pagó]((https://www.reuters.com/article/new-york-ifinex-settlement/bitfinex-tether-owner-pays-18-5-mln-fine-to-settle-nyag-cryptocurrency-cover-up-charges-idUSL1N2KT16E)) una multa de 18,5 millones de dólares, así evitó tener que admitir cualquier irregularidad.

El Banco Central de Nigeria se ha movido para recordar a los bancos que “está prohibido comerciar con criptomonedas o facilitar pagos para intercambios de criptomonedas” y les [ordenó](https://www.coindesk.com/nigerias-central-bank-orders-banks-to-close-accounts-of-all-crypto-users) cerrar las cuentas de todas las personas y entidades que comercien con criptomonedas.

La Reserva Federal de EE. UU. Experimentó un problema con su sistema de transferencia interbancaria que afectó el servicio durante horas. Esto sucedió poco después de que la secretaria del Tesoro de Estados Unidos, Janet Yellen, [advirtiera](https://www.cnbc.com/2021/02/22/yellen-sounds-warning-about-extremely-inefficient-bitcoin.html) que Bitcoin es "extremadamente ineficiente" y representa un riesgo para los inversores.

@notsofast pide con insistencia a todos a aprender de sus [errores](https://cryptonews.com/news/trader-s-lesson-why-you-shouldn-t-keep-large-amounts-of-cry-9302.htm) que lo han llevado a perder ~ $ 100 000 en criptomonedas en una brecha de seguridad: obtenga un buen administrador de contraseñas, use una wallet de hardware, aísle extensiones de navegador peligrosas como Metamask en perfiles o dispositivos de navegador separados.

## Sobre esta edición
Este es la edición #35 de la Revista Decred, un índice de todos los números originales y traducciones se encuentran disponibles [aquí](https://xaur.github.io/decred-news/).

La mayoría de la información de terceros se transmite directamente desde la fuente después de un control de fiabilidad mínimo. Los autores de la Revista Decred no tienen la capacidad de verificar todas las reclamaciones. Tenga cuidado con las estafas y haga su propia investigación.

Puedes enviar una historia [aquí](https://github.com/xaur/decred-news/labels/next%20release) para ser considerada para el próximo número. Los [comentarios](https://github.com/xaur/decred-news/blob/docs/contributing.md#feedback) y las [contribuciones](https://github.com/xaur/decred-news/blob/docs/contributing.md) siempre son bienvenidas.

Créditos:

Redacción y edición: bee, degeri, l1ndseymm, richardred

Revisión y comentarios: davecgh, jholdstock, peter_zen

Imagen de portada: saender.

Fondeado por: los stakeholders de Decred 





