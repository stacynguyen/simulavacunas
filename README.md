# simulavacunas
Simulación de campaña de vacunación

Primer esbozo de distribución hipotética de vacunas, usando datos de SS (OWID).

index.html aquí se despliega en https://stacynguyen.github.io/simulavacunas/

<!--- Originally in Documents\Tasa de Infecciones.odt --->

El proposito de esta nota es simular la campaña de vacunación mexicana en 2021. El gobierno ha ofrecido
solamente datos del nivel general del número de vacunas aplicadas y su distribución geográfica. No tenemos
detalles de cuáles vacunas se han aplicado, dónde (p. ej. a nivel de municipio) ni qué grupos de edad han
sido beneficiados. La campaña ha sido sumamente desordenada y es imposible deducir estos datos de principios
generales. Los detalles se encuentran en medios locales de los 33 estados y las secretarías de Salud / Bienestar 
locales. Quizá se pueda reconstruir de ahí pero sería un esfuerzo titánico.

Hay por lo menos dos motivos para tener cifras aunque sean aproximadas: sirve para hacer cálculos más precisos
de las tasas de inmunidad por contagio vs vacunación, lo que a su vez se requiere para obtener mejores tasas de 
mortalidad por COVID. Otro propósito es contrafactual. La campaña de vacunación empezó tarde, ha sido lenta, caótica,
y ha favorecido grotescamente a CDMX por encima del resto del país. Evaluarla con propiedad es difícil, pero
tener por lo menos una indicación de su efecto permite responder pregunta tipo: si la campaña hubiera sido de
tal o cual modo en vez de como sucedió, ¿cuántas muertes más/menos se hubieran obtenido?


*Series de vacunación por tipo de vacuna*

Nuestro punto de arranque son las series de vacunas aplicadas de Our World in Data (OWID), que provienen de la Secretaría de Salud mexicana). Dichas series contienen 3 datos diarios (con algunos agujeros):

columna H: total de vacunas aplicadas
columna I: total de personas vacunadas
columna J: total de personas vacunadas con dosis completas

Denotando con "1/1" las vacunas de una aplicación, "1/2" la primera dosis de las vacunas de 2 aplicaciones y con
"2/2" la segunda dosis de este tipo, vemos que se complen las relaciones _acumulativas_ siguientes:


	H (vacunas aplicadas) = 1/1 + 1/2 + 2/2
	I (personas vacunadas) = 1/1 + 1/2
	J (personas con dosis completa) = 1/1 + 2/2	
	
Despejando, obtenemos las ecuaciones para encontrar el valor acumulado a cada fecha:

	2/2 = H - I
	1/1 = J - 2/2
	1/2 = I - 1/1

Estos totales son acumulativos. Obtenemos las nuevas cifras diarias para cada categoría calculando la diferencia
entre el total de un fecha menos el total de la fecha anterior con un valor. Este procedimiento nos da tres tablas 
nacionales (no. aplicaciones 1a dosis, 2a, unidosis) por grupo de edad. Debe de corresponder exactamente a los valores
reales pero no lo es. Un primer motivo es que la Secretaría de Salud ha hecho varios ajustes importantes en algunas
fechas. Patentemente, el registro de vacunas se retrasa respecto a su aplicación, se acumulan, y las descargan de
un golpe. Por ejemplo, el 30 de octubre de 2021 hicieron un ajuste de 7.2M de vacunas. 

Un segundo problema es que el procedimiento indicado deja de funcionar el 28 de septiembre de 2021, cuando la cifra
_acumulada_ de unidosis _comienza_ a caer hasta el 7 de octubre, cuando asciende de nuevo hasta el 15 de diciembre,
para caer nuevamente. 

No tengo idea de qué haya provocado este zig-zag  ni es fácil saber cómo parchar la situación. Mi corrección tentativa
consiste en volver al acumulado de unidosis estrictamente no decreciente. Ya que esto altera los totales (y 
la relación con las otras columnas), prorrateo cada valor acumulado de unidosis para que al final coincida la
suma total de dosis con la anunciada al fin de SE-52 2021, 5.82M. Revisando la información disponible,
el uso de unidosis parece haber sido más bien bajo en esta fase de la campaña: CanSino para el magisterio 
y Johnson & Johnson para el estado de Baja California y su suma está en el rango de este total. 

De estas series se derivan inmediatamente los totales por semana epidemiológica en 2021 de vacunas aplicadas 
por cada tipo de vacuna.


	| SE      | 1/2		  		| 2/2    		| 1/1       |
	| ---:	  | ---:	  		| ---:	 		| ---:	    |
	| 1       | 415418  		| 1959    		| 1         |
	| 2       | 53291   		| 1476    		| 0         |
	| 3       | 135733  		| 21751   		| 0         |
	| 4       | 27044   		| 16657   		| 0         |
	| 5       | 6196    		| 33994   		| 0         |
	| 6       | 25803   		| 10362   		| 0         |
	| 7       | 572619  		| 366857  		| 0         |
	| 8       | 653569  		| 112368  		| 0         |
	| 9       | 355619  		| 38916   		| 0         |
	| 10      | 1484754 		| 5542    		| 0         |
	| 11      | 1165478 		| 106887  		| 0         |
	| 12      | 1271987 		| 134171  		| 0         |
	| 13      | 1756726 		| 260825  		| 0         |
	| 14      | 1401080 		| 609665  		| 216377    |
	| 15      | 1115451 		| 1392884 		| 288552    |
	| 16      | 143296  		| 1333405 		| 408023    |
	| 17      | 94899   		| 1398091 		| 296215    |
	| 18      | 1134657 		| 1278199 		| 258298    |
	| 19      | 750367  		| 590259  		| 327568    |
	| 20      | 2130386 		| 594278  		| 363788    |
	| 21      | 3209070 		| 348890  		| 159527    |
	| 22      | 2435904 		| 1701806 		| 16278     |
	| 23      | 2119757 		| 911511  		| 20561     |
	| 24      | 1073040 		| 1061010 		| 355222    |
	| 25      | 1427594 		| 1502415 		| 468915    |
	| 26      | 2408742 		| 787763  		| 54654     |
	| 27      | 2683354 		| 817625  		| 74222     |
	| 28      | 2790519 		| 880494  		| 33264     |
	| 29      | 3760328 		| 2057275 		| 123057    |
	| 30      | 4960119 		| 1514371 		| 200336    |
	| 31      | 2957930 		| 1191687 		| 93173     |
	| 32      | 3020095 		| 1666805 		| 152501    |
	| 33      | 2222911 		| 1656385 		| 134740    |
	| 34      | 1126140 		| 2532343 		| 23230     |
	| 35      | 500465  		| 1168992 		| 599468    |
	| 36      | 1259108 		| 2852629 		| 369188    |
	| 37      | 923004  		| 2221834 		| 252262    |
	| 38      | 1103065 		| 2608001 		| 126731    |
	| 39      | 1913358 		| 2529947 		| 8032      |
	| 40      | 2310157 		| 2380596 		| 0         |
	| 41      | 902886  		| 1916462 		| 0         |
	| 42      | 1896564 		| 2594419 		| 0         |
	| 43      | 4075343 		| 6519844 		| 62321     |
	| 44      | 302955  		| 1571828 		| 16690     |
	| 45      | 375934  		| 1062742 		| 78532     |
	| 46      | 273188  		| 747324  		| 92534     |
	| 47      | 659990  		| 585258  		| 54586     |
	| 48      | 1561998 		| 402749  		| 60114     |
	| 49      | 2071246 		| 679465  		| 30106     |
	| 50      | 3963333 		| 3130940 		| 5569      |
	| 51      | 955036  		| 6861587 		| 0         |
	| 52      | 0       		| 0       		| 0			|


(refuerzos)