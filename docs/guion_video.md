# Guion del video

Guion por bloques del video de presentación. Los números provienen de `reports/resultados.json`, de la misma ejecución que el README y los notebooks.

## Bloque 1. Introducción (1 minuto)

«¿Qué tan pronto se podría saber quién llega a los playoffs de la NFL?» Proyecto de Aprendizaje Automático, Centro Politécnico Superior Malvinas Argentinas.

«En la NFL clasifican 14 de 32 equipos a los playoffs en una temporada corta de 17 partidos. En este trabajo se busca estimar si un equipo llegaría a los playoffs a partir de los partidos ya jugados, y medir cómo cambiaría la calidad de esa estimación con 4, 6, 9 o 12 partidos. Es un problema de clasificación binaria supervisada. La predicción se haría después del partido N de cada equipo, utilizando únicamente los resultados de esos N partidos. El trabajo continúa uno anterior de la materia, en el que se había utilizado solo el corte de 9 partidos.»

## Bloque 2. Dataset (1 minuto 30 segundos)

«Se utilizó el archivo games.csv del proyecto abierto nflverse, con los resultados de todos los partidos de la liga. Se emplearon las temporadas 2002 a 2025, desde que la liga tiene 32 equipos. El repositorio de origen no declara licencia, por lo que el uso sería exclusivamente académico y con la fuente citada. Cada fila representa a un equipo en una temporada: 768 filas por corte.»

«Las variables se calcularon solo con los primeros N partidos de cada equipo: porcentaje de victorias, puntos a favor y en contra por partido, partidos jugados de local y, desde el corte de 6, el rendimiento en los últimos 4 partidos. Las variables redundantes y los identificadores fueron excluidos, y cada exclusión está explicada en el diccionario de datos. La variable objetivo valdría 1 si el equipo jugó al menos un partido de playoffs. Se verificó que no hubiera valores faltantes ni duplicados y que ninguna variable utilizara información posterior al corte.»

## Bloque 3. Modelo (2 minutos)

«Se compararon regresión logística, árbol de decisión, KNN y SVM, junto con dos referencias: un modelo que siempre responde «no llega» y una regla simple por porcentaje de victorias. La evaluación respetó el orden temporal: para cada temporada entre 2010 y 2025 se entrenó con las temporadas anteriores y se evaluó en esa temporada una sola vez. Los hiperparámetros se eligieron solo con el entrenamiento, mediante una validación interna que nunca parte una temporada, y el escalado se ajustó dentro de un Pipeline. Las temporadas 2019 a 2025 ya habían sido utilizadas como prueba en un trabajo anterior, por lo que la evaluación se considera retrospectiva.»

«La regresión logística combinaría las variables escaladas en un valor z, lo transformaría con la función sigmoide en una probabilidad y aplicaría un umbral. KNN compararía cada equipo con sus vecinos más parecidos del pasado; el escalado es necesario porque las distancias dependerían de las unidades de cada variable. El árbol seguiría una cadena de preguntas, por ejemplo si el porcentaje de victorias supera cierto valor, hasta llegar a una hoja. En el notebook se muestran estos tres mecanismos con un caso concreto.»

«Antes de ver resultados se fijó el criterio: F1 medio de al menos 0,75; una mejora de al menos 0,03 sobre la regla simple; superarla en al menos 11 de 16 temporadas; precisión y recall medios de al menos 0,70; y un desvío del F1 entre temporadas de, como máximo, 0,10.»

## Bloque 4. Resultados (1 minuto 30 segundos)

«El F1 medio de los mejores modelos aumentaría con los partidos jugados: 0,701 con 4 partidos, 0,748 con 6, 0,752 con 9 y 0,813 con 12. Sin embargo, la regla simple por porcentaje de victorias obtendría 0,687, 0,712, 0,764 y 0,811 en los mismos cortes. Los modelos no la superarían de forma clara.»

«Como ningún corte cumpliría el criterio completo, el modelo final se eligió con una regla fijada de antemano: el mayor F1 medio. Resultó la SVM con 12 partidos, con F1 medio de 0,813, precisión de 0,856 y recall de 0,781. En la matriz de confusión se observan 159 aciertos de la clase «llega», 280 de la clase «no llega», 45 falsos negativos y 28 falsas alarmas. La única mejora de al menos 0,03 sobre la regla aparecería con la logística con umbral en el corte de 6 partidos, pero con un F1 de 0,748, por debajo de lo exigido.»

## Bloque 5. Conclusión (1 minuto)

«Con los datos y la evaluación descriptos, el criterio de confiabilidad no se cumpliría en ninguno de los cuatro cortes evaluados. Con 12 partidos la calidad sería alta, pero equivalente a la de una regla simple. Como hipótesis, el récord de victorias podría resumir casi toda la información de las variables utilizadas; esto no fue verificado. Entre las limitaciones se señalan la evaluación retrospectiva con 16 temporadas, la correlación entre cortes y la ausencia de lesiones, mariscal de campo y calendario restante. Los resultados describen asociaciones, no causas. Como trabajo futuro, se incorporarían esas variables y se evaluaría con la temporada 2026 una vez finalizada.»
