# Port Log

## Sprint 1

### Objetivo
Aplicar conocimientos de versionado, organización y análisis exploratorio de datos
con pandas sobre un dataset real de operaciones portuarias.

### Introducción y contexto
El Puerto Fluvial de Rosario es uno de los complejos portuarios más importantes de
América del Sur y el principal punto de exportación de granos y derivados de la
Argentina. Diariamente ingresan y egresan decenas de buques de distintas banderas
con cargas de diverso tipo.

El sistema de registro de movimientos portuarios fue migrado recientemente desde un
sistema heredado de los años '90, que acumuló durante décadas inconsistencias de
formato en fechas, matrículas y valores numéricos fuera de rango. Esos registros no
pueden incorporarse directamente al nuevo sistema.

En este sprint el equipo analiza y depura los datos del sistema antiguo: se evalúa la
calidad del dataset, se normalizan los campos inconsistentes, se detectan las
infracciones por exceso de velocidad y se generan los reportes y visualizaciones
correspondientes.

### Estructura del proyecto
- port_log/data/raw: datasets en crudo
- port_log/data/interim: datasets procesados en pasos intermedios
- port_log/data/interim/plots: gráficos generados
- port_log/data/processed: datasets finales para otra aplicación
- port_log/reports: resúmenes estadísticos generados

### Integrantes
- Augusto Chaves Fiore
- Fabricio Nahuel Costadoni
- Erika Martínez
- Nahuel Paniagua
- Leonardo Taquini
