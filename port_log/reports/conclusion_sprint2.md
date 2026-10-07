# Conclusión - Sprint 2

## Cuántas infracciones pudimos validar

De las 473 infracciones del Sprint 1, solo 93 tienen una imagen que las respalde.
Eso es el 19.66%. Las otras 380 quedaron sin foto.

El OCR funcionó bien: de las 100 imágenes, 93 se cruzaron con el dataset y solo 7
quedaron sin match, con un 94.45% de coincidencia promedio. El problema no es el
reconocimiento sino que hay muchas más infracciones que fotos. De esas 380 sin
imagen, 81 están en estado PENDIENTE, así que son casos abiertos que dependen solo
de lo que midió el radar.

## Qué grupo funcionó mejor

Los recortes de matrícula (`plates`) tuvieron 95% de match, 57 de 60. Las fotos
completas (`completes`) llegaron al 90%, 36 de 40.

Las `completes` son archivos más grandes, pero eso no ayuda. Medimos la nitidez de
cada imagen y las `plates` que funcionaron dieron 628 contra 195 de las `completes`.
Lo que importa no es el tamaño de la foto sino cuántos píxeles le tocan a cada letra.
En un recorte la matrícula ocupa toda la imagen; en una foto completa es una parte
chica, rodeada de casco y agua.

## Qué afectó más al matching

Lo que más influyó fue el desenfoque. Las `completes` que fallaron tienen una nitidez
de 14.83 contra 195.40 de las que funcionaron: trece veces menos. Con ese nivel de
borrosidad las letras se mezclan con el fondo, y se nota en lo que leyó el OCR, que
devolvió cosas como `LLU` o `Fa Fng` en vez de la matrícula.

La oscuridad no fue el problema. Pensábamos que las fotos nocturnas iban a fallar
más, pero las `completes` que no matchearon son un 20% más claras que las que sí.
Igual son solo 7 casos, así que lo tomamos como un indicio.

La distancia afecta de forma indirecta: cuanto más lejos está el buque, menos píxeles
tiene cada letra, y eso es lo que termina complicando la lectura.

Aparte, dos de las siete fallas no fueron por la imagen. En `ONE0PARANA` y
`M4ERSK3PAMPAS` el OCR leyó el guion como si fuera un número. Ese número se queda en
la cadena y corre todo un lugar, así que la comparación da 22% y 33% aunque las
letras estén bien leídas.

## Qué mejoraríamos

Lo primero es la captura, no el algoritmo. Si el 80% de las infracciones no tiene
foto, mejorar el OCR no cambia mucho. El radar debería sacar una foto cada vez que
detecta un exceso y guardar un código igual en los dos registros. Así el cruce sería
directo y no haría falta leer la matrícula para saber a qué infracción pertenece.

Como los recortes funcionan mejor, el sistema debería guardar siempre las dos
versiones: el recorte para que lo lea el programa y la foto completa como prueba.

También agregaríamos un control antes de guardar la imagen. Si la cámara mide la
nitidez y descarta las que están muy borrosas, puede sacar otra foto en el momento.
Con un umbral de 50 se habrían filtrado las cuatro fallas de `completes` sin perder
ninguna imagen buena.

Del algoritmo, lo que falla es comparar letra por letra en la misma posición. Un
carácter de más corre todo y arruina el resultado. Una comparación que acepte esos
corrimientos recuperaría dos de las siete fallas. No la usamos porque con ese método
el ejemplo `MAERSK-L1M` de la consigna daría 84% y contaría como match, cuando el
enunciado dice que es error.

Una opción más simple sería cambiar los números por las letras que el OCR confunde
antes de comparar: 0 por O, 1 por I, 4 por A, 6 por G y 5 por S. Las matrículas casi
no tienen números, así que no se presta a confusión y arregla el error más común.
