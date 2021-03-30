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

