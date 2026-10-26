# Port Log

## Sprint actual: Sprint 3

### Objetivo
Aplicar conocimientos de bases de datos relacionales (SQLAlchemy), versionado de
datos (DVC) y búsqueda por similitud visual (ChromaDB) para profesionalizar el
sistema portuario.

### Introducción y contexto
El sistema de registro de movimientos del Puerto Fluvial de Rosario creció en
volumen y complejidad a lo largo de los sprints anteriores. En el Sprint 1 se
depuró el dataset heredado del sistema de los años '90 y se identificaron las
infracciones por exceso de velocidad. En el Sprint 2 se incorporó la evidencia
fotográfica capturada por los radares y se cruzó con los registros mediante
reconocimiento óptico de matrículas.

El equipo de desarrollo portuario necesita ahora tres capacidades nuevas:

1. Persistencia estructurada: pasar de archivos CSV a una base de datos
   relacional gestionada con un ORM.
2. Auditoría de cambios: poder rastrear qué versión del dataset se utilizó en
   cada análisis, versionando los archivos binarios con DVC sobre un
   repositorio S3.
3. Identificación visual de buques: como el reconocimiento óptico no siempre
   funciona, el sistema debe poder identificar un buque por similitud visual de
   su imagen, sin depender del texto de la matrícula.

### Estructura del proyecto
- port_log/data/raw: datasets e imágenes en crudo
- port_log/data/interim: datasets procesados en pasos intermedios
- port_log/data/interim/plots: gráficos generados
- port_log/data/processed: datasets finales para otra aplicación
- port_log/reports: resúmenes estadísticos y conclusiones

### Sprints anteriores
- Sprint 1: limpieza y análisis exploratorio del dataset de movimientos.
- Sprint 2: procesamiento de imágenes y cruce de evidencia visual por OCR.
