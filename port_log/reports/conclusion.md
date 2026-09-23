# Conclusión - Sprint 1

## Calidad del dataset heredado

El dataset original contenía 1500 registros de movimientos portuarios. Tras el proceso
de depuración se descartaron 255 registros por problemas de calidad, equivalentes al
17.00% del total: 166 por ausencia de datos en columnas críticas (91 sin matrícula
válida y 82 sin velocidad de ingreso, con 39 registros que compartían ambas carencias)
y 89 por valores fuera de rango (20 tonelajes negativos, 25 outliers de tonelaje y 44
de velocidad detectados por el método IQR). Los 1245 registros restantes se filtraron
luego por criterio de negocio, conservando las 473 infracciones efectivas.

Los tipos de error más frecuentes fueron, en orden de gravedad:

1. Horas de egreso inconsistentes (90.33% de completitud): convivían formatos de 12 y
   24 horas, marcas de tiempo imposibles como 39:54:00, y valores de texto como
   "sin dato" o guiones bajos que no son nulos para el sistema pero tampoco son datos.
2. Matrículas y muelles sin normalizar (93.93% y 100% de completitud recuperable):
   el mismo buque aparecía como ZIM-NORTE y como "zim norte!!", lo que duplicaba
   identidades. Tras la normalización, las 39 variantes se redujeron a 20 matrículas
   reales y los 12 valores de muelle a los 6 existentes.
3. Valores numéricos imposibles: tonelajes negativos y velocidades de hasta 199 nudos
   en muelles cuyo límite máximo es de 15.
4. Fechas en formatos mixtos (96.47% y 97.53%): junto a las fechas ISO convivían
   formatos DD/MM/YYYY y DD-MM-YYYY recuperables, y fechas imposibles como 32/13/2021.

Dentro del dataset final de infracciones persiste un 2.75% de registros con fecha
inválida y un 6.13% con hora de ingreso inválida, completados con los valores
centinela que indica la consigna.

## Patrones de infracción detectados

Las 473 infracciones registran un exceso promedio de 3.03 nudos sobre el límite del
muelle, que baja a 2.47 nudos al aplicar la tolerancia del 5%. La distribución es
asimétrica hacia la derecha: la mayoría de las infracciones son leves, con una moda
cercana a 1.5 nudos, y los casos graves llegan hasta 9.7 nudos.

Por turno, las infracciones se reparten de forma pareja: Madrugada encabeza con 134
casos (28.3%), seguida de Tarde con 131 (27.7%), Noche con 106 y Mañana con 102. Sin
embargo, este ranking debe leerse con cautela: las 29 infracciones con hora de ingreso
inválida fueron completadas con 00:00 y quedaron agrupadas en Madrugada. Descontadas
esas, el turno caería a 105 casos y pasaría al tercer lugar. El aparente predominio de
la madrugada es, en buena medida, un artefacto de la limpieza y no un patrón real.

Por muelle la distribución también es homogénea, entre 66 infracciones en el MUELLE-A
y 87 en el MUELLE-D, sin ninguna terminal que concentre el problema. Por tipo de carga
encabezan CONTENEDORES con el 15.01% y TRIGO muy cerca, lo que sugiere que el exceso
de velocidad es un comportamiento transversal y no asociado a una operación puntual.
El origen más frecuente entre los buques infractores es VALPARAISO, con 71 registros.
La duración promedio de estadía de los infractores es de 38.54 horas, aunque este
valor está afectado por 55 registros con duración negativa, producto de horas de
egreso inválidas completadas con 00:00.

## Impacto de incorporar los datos sin limpieza

Migrar el dataset heredado sin depuración habría producido consecuencias concretas.
La más grave es la duplicación de identidades: sin normalizar matrículas, un mismo
buque figuraría bajo dos denominaciones distintas y el sistema de reincidencia lo
trataría como dos embarcaciones diferentes, subestimando su historial y permitiendo
que evada sanciones acumulativas.

Las velocidades de hasta 199 nudos habrían generado alertas falsas y, de usarse para
calcular umbrales estadísticos, habrían distorsionado cualquier modelo de detección:
en el dataset original el desvío estándar de la velocidad superaba a su media, lo que
vuelve inservible cualquier criterio basado en la distribución normal.

Los tonelajes negativos habrían corrompido cualquier cálculo de carga movilizada, y
las fechas imposibles habrían roto los reportes por período, ubicando movimientos en
meses inexistentes o impidiendo directamente la conversión a tipo fecha.

## Propuesta de mejora

La medida de mayor impacto es incorporar validación en el momento de la captura, no
en el procesamiento posterior. Concretamente, el formulario de registro de movimientos
debería:

1. Reemplazar los campos de texto libre de fecha y hora por selectores de calendario y
   reloj, que emiten un único formato y hacen imposibles valores como 32/13/2021 o
   39:54:00.
2. Validar la matrícula contra un padrón cerrado de buques habilitados, mediante una
   lista desplegable en lugar de escritura manual. Esto elimina de raíz tanto las
   variantes de escritura como los valores basura.
3. Aplicar restricciones de rango en los campos numéricos: tonelaje estrictamente
   positivo y velocidad dentro de un máximo físicamente plausible para la navegación
   portuaria, rechazando el registro antes de guardarlo.
4. Impedir el guardado de un movimiento cuya fecha y hora de egreso sean anteriores a
   las de ingreso, que es la validación que habría evitado las 55 duraciones negativas.

Estas validaciones trasladan el costo de la calidad al momento de la carga, donde el
operador todavía tiene el dato correcto a mano, en lugar de diferirlo a un proceso de
depuración posterior que solo puede descartar información pero nunca recuperarla.
