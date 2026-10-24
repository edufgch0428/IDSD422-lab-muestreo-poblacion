# Informe — Clasificación de variables y población vs. muestra

**Para:** Gerencia de RetailAndina S.A. · **De:** Equipo de Business Intelligence
**Fecha:** 23/10/2026 · **Asunto:** Base de fidelización: tipos de variables y estimación por muestreo

---

## Resumen ejecutivo

En el desarrollo de este análisis, mediante el departamenteo de BI de la empresa, clasifiqué las distintas variables que se tenían en los archivos que alojaban los datos, de esta manera, mediante código se las interpretó para su respectiva clasificación, esto fue útil para realizar cálculos como el ingreso mensual promedio de los clientes mediante los modelos de muestreo como aleatorio simple, por conveniencia, y estratificado, entendiendo que cada una presenta sus sesgos respectivos. Todo esto hice para la toma de futuras decisiones. 

## Parte A — Clasificación de las variables

*Completa la tabla usando el **criterio** (¿tiene sentido la mitad del valor? ¿hay orden entre las categorías?), no el ejemplo. En «verificación» anota qué mostró el código: dtype, número de valores únicos y, si declaraste el orden, cómo lo comprobaste.*

| Variable | Tipo | Justificación (criterio) | Verificación en código |
|---|---|---|---|
| `cliente_id` | Categórica nominal |Es una etiqueta que se le da al cliente. | str, 12 únicos.|
| `edad` |Numérica discreta | En este caso, se manejan datos enteros para reflejar la edad de cada cliente, pues, tener una edad en fracción no resulta coherente.| int64, 12 únicos. |
| `numero_hijos` | Numérica discreta| El número de hijos es reflejado a través de números enteros para que tenga concordancia, pues, tener un número de hijos en fracción no presenta una coherencia.| int64, 5 únicos en una escala del 0 al 4. |
| `genero` | Categórica nominal| Se da una etiqueta a cada género de cada cliente.| str, 3 únicos.|
| `nivel_educacion` | Categórica ordinal  | En este caso, el orden de las etiquetas si importa porque existe un orden natural. | str, 4 únicos. Fue comprobado mediante las funciones min() y max(). |
| `ciudad` | Categórica nominal. | Solamente se dan etiquetas a las diversas ciudades sin importar el orden. | str, 3 únicos. |
| `ingresos_mensuales` | Numérica continua| Es continua porque los ingresos de cada cliente se encuentran entre un gran rango de números y no solamente se maneja enteros, ya que se puede manejar en fracciones. | int64, 12 únicos.|
| `satisfaccion` | Categórica ordinal |Se debe porque se trabaja con etiquetas que reflejan la satisfacción de cada cliente que llegan como entero.  | int64, 4 únicos. Fue comprobado mediante las funciones min() y max(). |

**P1.** ¿Qué te dijo el dtype de `satisfaccion` y por qué no basta para clasificarla? ¿Qué pasa si una ordinal queda como texto sin declarar el orden?

El dtype de satisfacción, con ayuda del script que ejecuté, indica que es un int64, dicho de esta manera, solamente el número no basta para clasificarla porque no se le da un significado o no se le categoriza de acuerdo con la Figura 1. 
En este caso, se ordenará de manera alfabética al quedar como texto sin declarar el orden tal y como en la Figura 2.

## Parte B — Población vs. muestra

### La población y su parámetro

| Concepto | Valor |
|---|---|
| Población (N) | 400 |
| Variable de interés | Ingresio mediode los clientes |
| Parámetro poblacional (media) | 1330.72|

*¿Por qué se muestrea si aquí el parámetro existe y se calculó?*

Porque es difícil procesar todos los datos exactos de todos los clientes a la vez. Dicho de esta manera, realicé los cálculos con los diferentes métodos específicos de muestreo para reflejar que en efecto el uso de estos es confiable. 

### Los tres diseños

*Corre tu script y registra las salidas. La composición por ciudad es la que delata el sesgo: anótala tal como salió.*

| Diseño | n | Media estimada | Composición por ciudad | Diferencia vs. parámetro |
|---|---|---|---|---|
| Conveniencia (`head`) | 20|1348.50 |Quito: 20 |+17.78 |
| Aleatorio simple (`sample`) | 20| 1336.00|Cuenca: 8, Quito: 8, Guayaquil: 4 |+5.28 |
| Estratificado (`groupby` + 5 %) |20 | 1287.50| Quito: 10, Guayaquil: 6, Cuenca: 4| -43.22|

*Anota también las medias de 5 sorteos distintos del aleatorio simple (semillas 1 a 5):*

| Semilla | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| Media | 1336.00|1405.00 | 1235.00| 1491.50| 1288.00|

**P2.** ¿Cuánto vale el parámetro poblacional y cuánto estimó cada diseño? ¿Cuál quedó más cerca y por qué eso **no** alcanza para declarar un ganador?

El parámetro poblacional de todos los cuatrocientos clientes dicta un total de 1330.72 y a su vez, en el método por conveniencia reflejó una media estimada 1348.50, mientras que, en el aleatorio simple reflejó una media estimada de 1336.0 y finalmente, el estratificado dictó una media de 1287.50. 
El método que quedó más cerca a diferencia de los demás fue aleatorio simple, pero esto no es suficiente porque la media estimada respectiva provino de un solo sorteo con la semilla 1. Si se hace otro sorteo al cambiar la semilla, se obtendrá resultados distintos. 


**P3.** La muestra por conveniencia también tiene 20 filas. ¿Qué defecto tiene y cómo lo delatas en la composición por ciudad? ¿Se corrige tomando más filas?

El defecto que tiene es que solamente toma los datos o muestras de clientes de Quito, cuando en los datos se tiene clientes también en Guayaquil y Cuenca de acuerdo con la Figura 2. Esto no se puede corregir al aumentar más filas porque de esta manera, se va a tomar más muestras de lo establecido a diferencia de los demás métodos que solamente ocupan veinte muestras eliminando esa igualdad.

**P4.** El sorteo aleatorio dio un número distinto en cada semilla. ¿Qué implica esa variabilidad para una empresa que va a decidir con un solo estudio?

Esa variabilidad implica que no existe una homogeneidad clara en cuanto a los datos de los ingresos mensuales de cada cliente ya que de acuerdo con los diferentes datos que obtuve al tomar una semilla de diferente valor, obtuve medias muy distintas y que no se acercaban al parámetro poblacional. Por lo tanto, tomar una decisión solamente basándose en un solo estudio, resultaría erróneo por la variabilidad descrita anteriormente. 

**P5.** ¿Cuándo usarías cada diseño en la práctica? Relaciona cada uno con un uso real (piloto, subgrupos que deben aparecer, conclusión sobre toda la población).

•	Por conveniencia: Lo usaría para realizar pruebas rápidas o pruebas piloto en algún experimento donde requiera una toma de datos para validar que el sistema de recolección implementado es el idóneo. Todo esto debido a los altos sesgos que pueden existir al momento de su implementación. 
•	Aleatorio simple: Para este caso, lo usaría para obtener alguna conclusión/conclusiones acerca de toda una población ya que se puede abarcar mucha más información debido a que, en este método, todos tienen la misma probabilidad de ser seleccionados. 
•	Estratificado: La mejor opción es usarlo para abarcar todos los subgrupos que puedan existir en una población, de esta manera, ningún elemento queda excluido del muestreo y así, obtendríamos datos más reales al aplicar algún cálculo. 


## Conclusiones

1.	En conclusión, clasifiqué las variables existentes en los archivos que usé en la práctica con ayuda de Python, dónde cliente_id, genero, nivel_educacion, ciudad son tratados como strings (str) y edad, numero_hijos, ingresos_mensuales y satisfaccion son tratados como enteros (int64).
2.	Calculé el ingreso mensual de los cuatrocientos clientes siendo 1330.72 que se la trató como el parámetro poblacional, a su vez la media estimada por conveniencia fue de 1348.50, aleatorio simple de 1336.0 y finalmente estratificado de 1287.50.
3.	Finalmente, la media estimada que más se acercó fue aleatorio simple con 1336.00. Por otro lado, es importante resaltar que el diseño es el que decide, más no el tamaño de elementos y esto fue lo que sucedió con el muestreo por conveniencia ya que en el mismo, solo se ocupó datos de clientes en Quito y esto no representaba totalmente a toda la población. 


