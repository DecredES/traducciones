# Analisis de la Blockchain de Decred Pt. 2 POW wow

## Enredandose (Enredandonos) en la cadena Decred
Desde mi primer informe sobre los datos de la cadena de bloques de Decred, he descendido a sus profundidades en un esfuerzo por comprender lo que podemos aprender más sobre la red. Este trabajo ha progresado en tres direcciones que se superponen de la siguiente manera:

- Agrupar las direcciones de Decred para ver qué direcciones gastan fondos o compran tickets juntos, por lo que probablemente pertenezcan a la misma billetera (wallet).
- Observar las exchanges y cómo identificar las transacciones relacionadas con el exchange: estas son plataformas clave dentro del ecosistema, por lo que es importante poder identificarlas en los datos.
- Mirando el proceso de mezcla y los DCR que participan en él.

Mi plan para el segundo informe era terminar de agrupar y proporcionar un historial detallado de la cadena Decred, pero agrupar a todos los mineros / stakeholders / contratistas / intercambios de Decred al mismo tiempo es una labor demasiado ardua. He pasado por muchas iteraciones en este proceso, y en las últimas ha sido difícil identificar errores, pero aún así encuentro algunos datos curiosos que no tienen mucho sentido, por lo que no son 100% confiables. Aún.

Si bien todavía planeo escribir este tipo de historiales detallados, hay un largo camino por recorrer, y será bastante largo cuando esté terminado. Sin embargo, he llegado a un punto en el que la exploración es bastante interesante, por lo que escribiré reportes breves que darán una idea de los diferentes aspectos de la actividad en cadena a medida que voy avanzando.

El primero de estos informes analiza los principales mineros de 2019-2020 (firmemente la era ASIC), para demostrar algunas de las herramientas que he desarrollado y ver qué tipo de imagen podemos dibujar con los análisis disponibles. Paré el reloj para la recopilación de datos para este informe el 9 de enero de 2021.

## Conociendo a los mineros
Para empezar, tomé todas las recompensas de PoW para este período y rastreé su flujo durante 2 saltos, esto debería cubrir el salto 0 (recompensa de bloque) al salto 1 (para grupos, a menudo un pago de grupo o movimiento interno antes), en el salto 2.

Ha habido 192 direcciones que recibieron recompensas de PoW directamente de la base de monedas, y estas a su vez enviaron DCR recién acuñado a 242,226 direcciones. El arreglo típico sería que la transacción de coinbase acuñe DCR a una dirección que el grupo siempre usa (la 192), luego el grupo realiza transacciones que envían este DCR a los participantes, pero existe una variación considerable en cómo los grupos gestionan este proceso, ya que veremos.

*Tabla

## Vista de la red
Mirar tablas de direcciones y hashes de transacciones y tratar de seguir un flujo entre estos es difícil para la mente, por lo que en algunas ocasiones he buscado métodos para visualizar estas redes. Los métodos convencionales para extraer los nodos y los bordes de una red no se adaptan bien al tamaño de estos grupos. La mayoría de las veces, cuando pruebo uno de estos, golpea mi máquina durante horas antes de fallar o me rindo y lo dejo. Cuando los gráficos se dibujan, generalmente son incomprensibles.

Mientras experimentaba con muestras más pequeñas recientemente para aprender a controlar los diseños y demás, me di cuenta de que funcionan bastante bien para dar una idea de cómo el minero o el pool organiza sus direcciones y transacciones. Para estos clústeres de minería, tomar solo las primeras 2,000 - 20,000 filas de la tabla de direcciones (entradas / salidas para transacciones) parece lograr un buen equilibrio entre tener una muestra representativa que ilustre cómo fluye los DCR y poder dibujarlo. La mayor limitación sería si el pool cambiara su estructura más adelante, lo que no estaría representado en estos gráficos. Se puede hacer clic en todos los gráficos para expandirlos, y para estos gráficos de red probablemente sea necesario distinguir todo lo que esté representado.

La gran decisión al representar el flujo de los DCR como una red es usar una red de uno o dos modos. He optado por dos modos aquí, siendo Direcciones y Transacciones los dos tipos diferentes de nodo. DCR fluye de una dirección a una transacción y de una transacción a nuevas direcciones. También he experimentado con redes monomodo (donde solo existen nodos de direcciones y las transacciones se representan con bordes entre ellos), pero la masa de conexiones que esto genera es difícil de visualizar.

El tipo de red en las visualizaciones es una red de ego muy básica centrada en las direcciones del clúster, es una red de ego ligeramente poco convencional porque utilicé mi técnica de agrupamiento para definir el límite de la red, luego solo extendí un borde de los nodos dentro de este "ego".

Las visualizaciones de red utilizan las primeras 2000 - 20 000 entradas / salidas de las direcciones del clúster y dan un salto en cada dirección para ver si las entradas provienen de las direcciones del clúster o las salidas van a las direcciones del clúster. En estos casos (la mayoría de los bordes), el DCR se mueve entre direcciones controladas por la misma billetera. Cuando las entradas no provienen de una dirección controlada por un clúster, los he marcado como nodos "internos"; las transacciones de coinbase son un tipo especial de nodo interno y les he dado su propio color. Cuando las salidas de las transacciones de clúster no van a direcciones controladas por clúster, las he marcado como nodos "en adelante". Las direcciones posteriores que coinciden con las direcciones de intercambio conocidas se han resaltado con su propio color.

La visualización de redes es una adición reciente a mi conjunto de herramientas de exploración, pero ahora que tengo métodos para producir este tipo de red del ego, hay muchos otros pools para mirar y análisis basados ​​en redes que puedo usar. Cada grupo a continuación tiene un gráfico de red con los mismos parámetros de color. Varié la cantidad de filas de datos que se utilizarán para sembrar la red ego y elegí diferentes niveles para cada clúster, el número máximo legible (y el punto en el que se vuelven muy lentos para dibujar) depende de cuánto se extienden las transacciones del clúster. También estoy experimentando con diseños, y estos pueden tener un gran efecto en el aspecto de las redes, el diseño se da en el título (junto con el número de filas).

## Top 5 Coinbase
### 1 - DsiD
Este clúster se vio por primera vez en enero de 2019 y todavía estaba activo en enero de 2021, recibió 987 000  de recompensas de PoW por la extracción de 53,766 bloques. El grupo controla 1.109 direcciones, hay la principal que recibe recompensas PoW y las otras son direcciones de cambio de transacciones de pago, el cambio se repite para usarse con pagos adicionales.

*Tabla

### Equilibrio del clúster DsiD
El saldo de las direcciones de este clúster nunca es demasiado grande. Otro indicador útil de lo que está haciendo el clúster es observar la regularidad de su comportamiento. Los comportamientos muy regulares que se repiten durante meses o años son un signo de automatización.

*Tabla

### DCR fluyendo hacia / desde el cluster DsiD
La cantidad de DCR que se extrae y se mueve fuera del clúster cada día es similar, lo que sugiere que se trata de un grupo de minería, el más grande que opera actualmente en la red de DCR.

*Tabla

### Distribución de salidas DCR entre direcciones externas
También podemos ver hacia dónde fluye ese DCR, y en este caso hay dos direcciones que han recibido alrededor de 200 000 DCR del poolotras dos que han recibido 40-50K DCR. El destinatario más grande (DsgK) aparece a continuación en el número 3 en la lista de los 5 principales mineros de piscinas.
