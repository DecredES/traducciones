# Gobernanza en Blockchain: Parte dos

## Parte 2: La tecnología detrás del dinero digital
En las últimas décadas, los investigadores han intentado replicar digitalmente el uso y los valores de cambio `1` del dinero, así como sus propiedades monetarias: fungibilidad, escasez, divisibilidad, transferibilidad y durabilidad (Menger, 2009).

Pero el dinero en sí debe entenderse antes de la introducción del dinero digital y las cadenas de bloques. Menger (2009) define el dinero como la mercancía porque “ciertas mercancías llegaron a ser dinero de forma bastante natural, como resultado de relaciones económicas que eran independientes del poder del estado. (…) El dinero no es producto de un acuerdo por parte de los hombres economizadores ni producto de actos legislativos. Nadie lo inventó. A medida que los individuos economizadores en situaciones sociales se volvieron cada vez más conscientes de su interés económico, en todas partes adquirieron el simple conocimiento de que entregar productos menos vendibles por otros de mayor venta los acerca sustancialmente al logro de sus propósitos económicos específicos ”(Menger, 2007, p. 262). Sin dinero, “la especialización de funciones y la división del trabajo no podrían llegar muy lejos si tuviéramos que seguir confiando en el trueque de producto por producto. En consecuencia, el dinero se ha introducido como un medio para facilitar el intercambio y para permitir que los actos de compra y venta se separen en dos partes ”(Friedman, 2002, p. 14).

En lo que respecta a la moneda, el medio de cambio, Hayek señala que “la autenticidad del dinero metálico solo se puede determinar mediante un difícil proceso de evaluación, para el cual la persona común no tenía la habilidad ni el equipo, se podría argumentar con fuerza por garantizar la fineza de las monedas mediante el sello de alguna autoridad generalmente reconocida que, sólo podía ser el gobierno ”. Luego Hayek llega al problema del desequilibrio de poder cuando argumenta que “es evidente que, a medida que se difundió la moneda, los gobiernos de todas partes pronto descubrieron que el derecho exclusivo de acuñación era un instrumento de poder más importante, así como una atractiva fuente de ganancia (…) al menos mientras la gente no tuviera más alternativa que utilizar el dinero que proporcionaba. Durante la Edad Media, sin embargo, surgió la superstición de que era el acto de gobierno el que confería el valor al dinero ”(Hayek, 1990, págs. 29, 30).

En 1976, Hayek previó la creación del dinero privado: “surge de inmediato la pregunta de si no sería igualmente deseable eliminar por completo el monopolio del gobierno que suministra dinero y permitir que la empresa privada suministre al público otros medios de intercambio". (Hayek, 1990, pág. 26).

## La cadena de bloques

Blockchain es un libro mayor distribuido que se utiliza para almacenar transacciones de forma segura en una red de nodos. Las reglas de consenso validan las transacciones antes de que se agreguen a los bloques, que están asegurados por un servidor de marcas de tiempo distribuido que prueba su existencia y refuerza las anteriores (Nakamoto, 2008).

Una transacción es una cadena de firmas digitales `2`. Las firmas digitales dependen de un par de claves para cada propietario. El propietario transfiere la moneda a la siguiente firmando digitalmente un hash de la transacción anterior y la clave pública del próximo propietario. Las nuevas transacciones se transmiten a todos los nodos y luego se recopilan en bloques. Para evitar intentos de doble gasto, la primera transacción es la que cuenta, de lo contrario sería necesaria una autoridad central de confianza (Nakamoto, 2008).

## Blockchain y la seguridad de proof-of-work

Con Bitcoin, todos pueden tener el control de su propio dinero, no del tercero de confianza. Para mantener el control, el usuario debe mantener las monedas en su "cartera". La 'cartera' es una referencia al par de claves introducidas anteriormente, específicamente la clave privada, la que firma las transacciones y debe mantenerse en secreto. La clave privada generalmente se conoce como la "semilla" `3`, un conjunto de palabras mnemotécnicas donde cada una representa parte de la clave. Si el usuario deja sus monedas en una plataforma de intercambio, un corredor, puede quebrar o simplemente desaparecer con el dinero del usuario (Narayanan, Bonneau, Felten, Miller y Goldfeder, 2016).

La prueba de trabajo mantiene la seguridad de la cadena de bloques creando un hash `2` del bloque anterior y colocándolo en el siguiente. Para que un hash sea válido, debe tener un número de ceros binarios iniciales predeterminados por el algoritmo. Si el hash no es válido, el minero aumenta el "nonce", "número usado solo una vez" y vuelve a intentarlo. Cuantos más ceros a la izquierda requiera el algoritmo, más difícil será. La Figura 1 muestra cómo se conectan los bloques creando una cadena de bloques.

![proof-of-work](https://stakey.club/assets/images/blockchain-pow.png)

La prueba de trabajo es esencialmente una CPU, un voto. La cadena más larga tiene el mayor esfuerzo de prueba de trabajo invertido en ella, lo que representa la decisión de la mayoría. Siempre que la mayor parte de la potencia de la CPU esté controlada por nodos honestos, la cadena honesta será la más larga, superando a otras cadenas. Cuanto más larga es la cadena, más seguros se vuelven los bloques anteriores, porque sería necesario un trabajo exponencial para recrear la cadena desde el punto en que un adversario quiere hacer un cambio arbitrario, ponerse al día y superar la cadena honesta.

Los mineros pueden reunir recursos y cooperar en un grupo de mineros y compitiendo con otros grupos para encontrar el hash que valida el bloque. Los incentivos se comparten en el pool de acuerdo con la capacidad de procesamiento de cada minero (Eyal, The Miner’s Dilemma, 2015).

El software de código abierto también contribuye en gran medida a la seguridad, transparencia, auditabilidad, replicación de código e interoperabilidad.

## Incentivos e inflación

Las criptomonedas minables crean dinero mediante la emisión de nuevas monedas. La primera entrada en el bloque se llama "coinbase" y crea y envía las monedas a una dirección elegida por el minero. Esta recompensa es el incentivo por el trabajo en el bloque, gastando electricidad en el procesamiento y desgastando las máquinas. Para compensar los aumentos en la potencia del hardware y los nuevos mineros que se unen a la competencia, la dificultad se ajusta mediante un promedio móvil que apunta a un número promedio de bloques por hora. Las máquinas pierden su utilidad más rápidamente cuando aumenta la dificultad, lo que genera una competencia en términos de costos fijos (alquiler de espacio y costo de máquinas y equipos de enfriamiento) y eficiencia de las máquinas (costo de electricidad por miles de millones de hashes por segundo y equipos de enfriamiento).

Los incentivos también se pueden financiar con tarifas de transacción porque el usuario que firma la transacción decide si el minero recibirá una tarifa por el trabajo de procesamiento. Por otro lado, el minero incluye en su bloque las transacciones con tarifas más altas porque quiere maximizar su beneficio. Las tarifas son importantes para seleccionar qué transacciones se extraerán y reducir el spam de transacciones.

La inflación en una cadena de bloques se limita a la creación de nuevas monedas y es un proceso predecible gestionado por las reglas de consenso de la red. El precio de la moneda refleja la expectativa que tienen los participantes sobre la utilidad de la red y la creación de nuevo dinero. Para Hayek, el patrón oro imperfecto “tenía tres ventajas muy importantes: creaba en efecto una moneda internacional sin someter la política monetaria nacional a las decisiones de una autoridad internacional; hizo que la política monetaria fuera en gran medida automática y, por tanto, predecible; y los cambios en la oferta de dinero básico que aseguró su mecanismo fueron en general en la dirección correcta ”(Hayek, 2009, p. 41). Entonces, el oro no tenía problemas de gobernanza, era predecible y su producción se estimulaba o desanimaba según su valor. También es cierto para las criptomonedas extraíbles.

La fuente del dinero ha sido un problema durante mucho tiempo. En 1983, Margaret Thatcher hizo una advertencia que golpeó gran parte del problema. Ella dijo: “No olvidemos nunca esta verdad fundamental: el Estado no tiene otra fuente de dinero que el dinero que la gente gana. Si el Estado desea gastar más, sólo puede hacerlo pidiendo prestados tus ahorros o gravándote más ”(Thatcher, 1983). Pero la Sra. Thatcher no mencionó que el gobierno también puede crear nuevo dinero de la nada, la "flexibilización cuantitativa", porque controla la oferta de dinero (Banco de Inglaterra, 2019). Friedman explicó la diferencia en sus proposiciones clave del monetarismo. Escribió que “el gasto público puede o no ser inflacionario. Claramente será inflacionario si se financia creando dinero, es decir, imprimiendo moneda o creando depósitos bancarios ”, no el caso del endeudamiento o la tributación (Friedman & Goodhart, 2003, p. 86). Significa que la creación de nuevo dinero devalúa la moneda, y esta es la razón de ser una de las reglas de consenso más importantes y la base de la minería de prueba de trabajo (PoW) de riesgo-recompensa.

Hayek también señaló el control de la oferta monetaria cuando escribió que “un dinero controlado deliberadamente en la oferta por una agencia cuyo interés propio lo obligó a satisfacer los deseos de los usuarios podría ser lo mejor. Un dinero regulado para satisfacer las demandas de los intereses del grupo seguramente será lo peor posible. (…) El monopolio gubernamental de la emisión de dinero ya era bastante malo mientras predominaba el dinero metálico. Pero se convirtió en una calamidad sin alivio desde que el papel moneda (u otro dinero simbólico), que puede proporcionar el mejor y el peor dinero, quedó bajo control político ”(Hayek, 1990, p. 31).

Cuando se trata de papel moneda, la moneda fiduciaria se convierte en un monopolio técnico. La competencia no proporcionará un límite efectivo porque el emisor privado quiere emitir más moneda hasta que el valor de mercado de la moneda adicional iguale el costo del papel donde se imprime. Las tareas del gobierno entonces son establecer un límite externo a la cantidad de dinero y prevenir la falsificación (Friedman, 1960).

Pero existe el problema de la agencia, que aparece en el argumento de Hayek: "No hay razón para dudar de que la empresa privada, si se hubiera permitido, habría sido capaz de proporcionar monedas tan buenas y al menos tan confiables". Pero, “dado que la función del gobierno al emitir dinero ya no es simplemente certificar el peso y la finura de una determinada pieza de metal, sino que implica una determinación deliberada de la cantidad de dinero que se emitirá, los gobiernos se han vuelto totalmente inadecuados para el tarea y, se puede decir sin calificaciones, han abusado incesantemente y en todas partes de su confianza para defraudar al pueblo ”(Hayek, 1990, pp. 25, 30).

Dicho de esta manera, para proporcionar 'monedas buenas y confiables' que satisfagan los deseos de sus usuarios, emitidas de manera predecible y de acuerdo con su valor, la empresa privada necesita un buen gobierno, que venga con buenas reglas de consenso para la emisión de dinero y el procesamiento de pagos. , tema que se conectará en las siguientes secciones.

### Notas:

`1` Menger explica que “El valor de uso, por tanto, es la importancia que adquieren los bienes para nosotros porque nos aseguran directamente la satisfacción de necesidades que no se cubrirían si no tuviéramos los bienes a nuestro alcance. El valor de cambio es la importancia que adquieren los bienes para nosotros porque su posesión asegura el mismo resultado de forma indirecta ”. (Menger, 2007)

`2` Una breve noción de criptografía: https://stakey.club/en/a-brief-notion-of-cryptography/.

`3` Lista de palabras PGP: https://en.wikipedia.org/wiki/PGP_word_list.


### Referencias:

- Eyal, I. (20 de julio de 2015). El dilema del minero. Simposio de IEEE sobre seguridad y privacidad de 2015 (págs. 89-103). San José, CA: IEEE. Obtenido de https://ieeexplore.ieee.org/abstract/document/7163020

- Friedman, M. (1960). Un programa de estabilidad monetaria. Fordham.

- Friedman, M. (2002). Capitalismo y Libertad. Prensa de la Universidad de Chicago.

- Friedman, M. y Goodhart, C. A. (2003). Dinero, inflación y posición constitucional del Banco Central. Obtenido del Institute of Economic Affairs: https://iea.org.uk/publications/research/money-inflation-and-the-constitutional-position-of-central-bank

- Hayek, F. A. (1990). Desnacionalización del dinero: el argumento refinado (3ª ed.). El Instituto de Asuntos Económicos. Obtenido del Instituto Ludwig von Mises: https://mises.org/library/denationalisation-money-argument-refined

- Hayek, F. A. (2009). Una moneda de reserva para productos básicos. En F. A. Hayek, Un tigre por la cola (págs. 41-43). El Instituto de Asuntos Económicos y el Instituto Ludwig von Mises. Obtenido de A Tiger by the Tail: https://iea.org.uk/publications/research/a-tiger-by-the-tail-the-keynesian-legacy-of-inflation

- Menger, C. (2007). Principios de Economía (5ª ed.). (J. Dingwall y B. F. Hoselitz, Trad.) Viena, Austria: Instituto Ludwig von Mises. Obtenido del Instituto Ludwig von Mises: https://mises.org/library/principles-economics

- Menger, C. (2009, 17 de noviembre). Sobre los orígenes del dinero. Instituto Ludwig von Mises. Obtenido del Instituto Ludwig von Mises: https://mises.org/library/origins-money-0

- Nakamoto, S. (2008, 31 de octubre). Bitcoin: un sistema de efectivo electrónico peer-to-peer. Obtenido del Instituto Nakamoto: https://nakamotoinstitute.org/bitcoin/

- Narayanan, A., Bonneau, J., Felten, E., Miller, A. y Goldfeder, S. (2016). Tecnologías de Bitcoin y criptomonedas: una introducción completa. Princeton, Oxford, Estados Unidos, Reino Unido: Princeton University Press.
