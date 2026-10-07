# Changelog

[Sprint 2 - Ejercicio 5]
- Cálculo de infracciones con y sin evidencia visual asociada
- Conteo de imágenes sin match y ratio promedio de coincidencia
- Comparación de la tasa de match entre los grupos plates y completes
- Detección de infracciones pendientes sin evidencia visual

[Sprint 2 - Ejercicio 4]
- Extracción de matrículas por OCR sobre las 100 imágenes
- Normalización alfanumérica y cálculo del ratio de coincidencia
- Asignación de cada imagen a una infracción con 75% o más de coincidencia
- Exportación del dataset cruzado a data/processed/port_movements_image.csv

[Sprint 2 - Ejercicio 3]
- Conversión de las imágenes a escala de grises
- Ecualización de histograma para mejorar el contraste
- Suavizado con blur gaussiano de kernel 5x5
- Detección de bordes con Canny sobre las imágenes suavizadas

[Sprint 2 - Ejercicio 2]
- Inventario de las 100 imágenes con su tamaño en KB
- Separación en grupos plates y completes por relación de aspecto
- Construcción de group_images.json con los metadatos de cada imagen
- Cálculo de métricas promedio y función mostrar_muestra

[Sprint 2 - Ejercicio 1]
- Creación de la rama Sprint_2 a partir de Sprint_1
- Descarga del dataset de 100 imágenes en port_log/data/raw/imgs
- Verificación de los archivos heredados del Sprint 1
- Exclusión de las imágenes derivadas mediante .gitignore

[Ejercicio 7]
- Redacción de la conclusión del Sprint 1
- Evaluación de la calidad del dataset heredado y de los patrones de infracción
- Propuesta de mejoras para el proceso de captura de datos en el puerto

[Ejercicio 6]
- Cálculo de los porcentajes de infracciones con fecha y hora inválidas
- Identificación del tipo de carga y el origen más frecuentes entre infractores
- Cálculo de la duración promedio de estadía de los buques infractores

[Ejercicio 5]
- Generación de los seis gráficos del análisis de infracciones
- Exportación de los gráficos en formato .jpg a data/interim/plots

[Ejercicio 4]
- Definición de la clase PortAnalyzer con encapsulamiento del DataFrame limpio
- Implementación de los métodos de ranking, agrupación por turno, muelle y tipo de carga
- Cálculo del exceso de velocidad promedio con y sin tolerancia

[Ejercicio 3]
- Normalización de fechas, horas, matrículas y muelles
- Cálculo de la duración de estadía y del exceso de velocidad
- Eliminación de nulos críticos, valores fuera de rango y outliers por IQR
- Filtrado de infracciones y exportación del dataset limpio y del resumen

[Ejercicio 2]
- Descarga del dataset raw en port_log/data/raw
- Análisis exploratorio inicial: muestra, tipos de datos y valores nulos
- Definición de funciones validadoras y cálculo de completitud por columna

[Ejercicio 1]
- Inicialización del repositorio sobre la rama Sprint_1
- Creación de la estructura de directorios de port_log
- Creación de README.md y CHANGELOG.md
