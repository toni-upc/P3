PAV - P3: estimación de pitch
=============================

Esta práctica se distribuye a través del repositorio GitHub [Práctica 3](https://github.com/albino-pav/P3).
Siga las instrucciones de la [Práctica 2](https://github.com/albino-pav/P2) para realizar un `fork` de la
misma y distribuir copias locales (*clones*) del mismo a los distintos integrantes del grupo de prácticas.

Recuerde realizar el *pull request* al repositorio original una vez completada la práctica.

Ejercicios básicos
------------------

- Complete el código de los ficheros necesarios para realizar la estimación de pitch usando el programa
  `get_pitch`.

   * Complete el cálculo de la autocorrelación e inserte a continuación el código correspondiente.

***Este fragmento de código implementa el cálculo de la autocorrelación de una señal representada por el vector x, almacenando el resultado en el vector r. La autocorrelación permite identificar patrones repetitivos en la señal, como su periodicidad, comparando la señal consigo misma a distintos desplazamientos.***

***Primero, se recorre cada posición del vector r. Para cada índice l, se acumula el producto de los valores de la señal con su versión desplazada l posiciones hacia adelante. Esta suma se normaliza dividiéndola entre el número total de muestras de la señal (x.size()), obteniendo así el valor de la autocorrelación para ese desplazamiento.***

***Finalmente, se asegura que el primer valor r[0] no sea exactamente cero. Esto se hace asignándole un pequeño valor (1e-10) en caso de ser necesario, para evitar errores posteriores como divisiones por cero o problemas al aplicar funciones logarítmicas.***

***En definitiva, el código proporciona una forma simple y directa de calcular la autocorrelación de una señal, preparándola para análisis posteriores, como la estimación del tono (pitch).***

![alt text](Codigo_Autocorrelacion.png)
   * Inserte una gŕafica donde, en un *subplot*, se vea con claridad la señal temporal de un segmento de
     unos 30 ms de un fonema sonoro y su periodo de pitch; y, en otro *subplot*, se vea con claridad la
	 autocorrelación de la señal y la posición del primer máximo secundario.

![alt text](Grafica1.png)
	 NOTA: es más que probable que tenga que usar Python, Octave/MATLAB u otro programa semejante para
	 hacerlo. Se valorará la utilización de la biblioteca matplotlib de Python.

   * Determine el mejor candidato para el periodo de pitch localizando el primer máximo secundario de la
     autocorrelación. Inserte a continuación el código correspondiente.

***Este código busca estimar el período del pitch localizando el primer máximo secundario en el vector de autocorrelación r, es decir, el valor de desplazamiento (lag) distinto de cero donde la autocorrelación es máxima dentro de un rango plausible (npitch_min a npitch_max). Esto se hace porque el primer valor (r[0]) siempre es el máximo (correspondiente a correlación perfecta), y no aporta información útil para detectar periodicidad.***

![alt text](Codigo_MaxSec_Autocorrelacion.png)  

   * Implemente la regla de decisión sonoro o sordo e inserte el código correspondiente.

***Esta función se encarga de clasificar una trama de audio como sonora o sorda en función de tres propiedades acústicas: el valor máximo normalizado de la autocorrelación (rmaxnorm), la relación entre el primer coeficiente de autocorrelación y el valor central (r1norm), y la tasa de cruce por cero (zcr), que mide la cantidad de oscilaciones rápidas en la señal.***

***Cada una de estas características se compara con su correspondiente umbral. Si una característica sugiere que la trama es sonora (es decir, cumple su condición), se incrementa un contador. Al finalizar, si al menos dos de los tres indicadores apuntan a que el sonido es sonoro, la función devuelve false; en caso contrario, devuelve true, indicando que se trata de una trama sorda.***

![alt text](Codigo_Regla_Decision.png) 

   * Puede serle útil seguir las instrucciones contenidas en el documento adjunto `código.pdf`.

- Una vez completados los puntos anteriores, dispondrá de una primera versión del estimador de pitch. El 
  resto del trabajo consiste, básicamente, en obtener las mejores prestaciones posibles con él.

  * Utilice el programa `wavesurfer` para analizar las condiciones apropiadas para determinar si un
    segmento es sonoro o sordo. 
	
	  - Inserte una gráfica con la estimación de pitch incorporada a `wavesurfer` y, junto a ella, los 
	    principales candidatos para determinar la sonoridad de la voz: el nivel de potencia de la señal
		(r[0]), la autocorrelación normalizada de uno (r1norm = r[1] / r[0]) y el valor de la
		autocorrelación en su máximo secundario (rmaxnorm = r[lag] / r[0]).

		Puede considerar, también, la conveniencia de usar la tasa de cruces por cero.

	    Recuerde configurar los paneles de datos para que el desplazamiento de ventana sea el adecuado, que
		en esta práctica es de 15 ms.

    ![alt text](Captura_Wavesurfer.png) 

      - Use el estimador de pitch implementado en el programa `wavesurfer` en una señal de prueba y compare
	    su resultado con el obtenido por la mejor versión de su propio sistema.  Inserte una gráfica
		ilustrativa del resultado de ambos estimadores.
     
		Aunque puede usar el propio Wavesurfer para obtener la representación, se valorará
	 	el uso de alternativas de mayor calidad (particularmente Python).
  
  * Optimice los parámetros de su sistema de estimación de pitch e inserte una tabla con las tasas de error
    y el *score* TOTAL proporcionados por `pitch_evaluate` en la evaluación de la base de datos 
	`pitch_db/train`..

      ![alt text](Tasa_de_Error.png)

Ejercicios de ampliación
------------------------

- Usando la librería `docopt_cpp`, modifique el fichero `get_pitch.cpp` para incorporar los parámetros del
  estimador a los argumentos de la línea de comandos.
  
  Esta técnica le resultará especialmente útil para optimizar los parámetros del estimador. Recuerde que
  una parte importante de la evaluación recaerá en el resultado obtenido en la estimación de pitch en la
  base de datos.

  * Inserte un *pantallazo* en el que se vea el mensaje de ayuda del programa y un ejemplo de utilización
    con los argumentos añadidos.

  ![alt text](Get_Pitch.png)
- Implemente las técnicas que considere oportunas para optimizar las prestaciones del sistema de estimación
  de pitch.

  Entre las posibles mejoras, puede escoger una o más de las siguientes:

  * Técnicas de preprocesado: filtrado paso bajo, diezmado, *center clipping*, etc.

  ![alt text](Preprocesado.png)
  * Técnicas de postprocesado: filtro de mediana, *dynamic time warping*, etc.

  ![alt text](Postprocesado.png)
  * Métodos alternativos a la autocorrelación: procesado cepstral, *average magnitude difference function*
    (AMDF), etc.
  * Optimización **demostrable** de los parámetros que gobiernan el estimador, en concreto, de los que
    gobiernan la decisión sonoro/sordo.
  * Cualquier otra técnica que se le pueda ocurrir o encuentre en la literatura.

  Encontrará más información acerca de estas técnicas en las [Transparencias del Curso](https://atenea.upc.edu/pluginfile.php/2908770/mod_resource/content/3/2b_PS%20Techniques.pdf)
  y en [Spoken Language Processing](https://discovery.upc.edu/iii/encore/record/C__Rb1233593?lang=cat).
  También encontrará más información en los anexos del enunciado de esta práctica.

  Incluya, a continuación, una explicación de las técnicas incorporadas al estimador. Se valorará la
  inclusión de gráficas, tablas, código o cualquier otra cosa que ayude a comprender el trabajo realizado.

  También se valorará la realización de un estudio de los parámetros involucrados. Por ejemplo, si se opta
  por implementar el filtro de mediana, se valorará el análisis de los resultados obtenidos en función de
  la longitud del filtro.
   

Evaluación *ciega* del estimador
-------------------------------

Antes de realizar el *pull request* debe asegurarse de que su repositorio contiene los ficheros necesarios
para compilar los programas correctamente ejecutando `make release`.

Con los ejecutables construidos de esta manera, los profesores de la asignatura procederán a evaluar el
estimador con la parte de test de la base de datos (desconocida para los alumnos). Una parte importante de
la nota de la práctica recaerá en el resultado de esta evaluación.
