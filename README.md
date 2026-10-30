# Souza_Facundo_Preentrega-opcional---Limpieza
Pre-Entrega: Limpieza y Análisis Exploratorio de un Dataset Real

# Limpieza y análisis exploratorio: ventas de una cafetería

Pre-entrega opcional - Data Science I
Autor: Facundo Souza Soler

## Descripción

Notebook de limpieza y análisis exploratorio de un dataset de ventas de una cafetería (año 2023), pensado para practicar el tratamiento de datos sucios con Pandas: tipos incorrectos, valores nulos y valores inválidos escondidos como texto.

Pregunta de investigación: ¿qué productos, métodos de pago y ubicaciones generan más ingresos, y cómo evolucionan las ventas a lo largo del año?

## Origen de los datos

- Fuente: Kaggle, "Cafe Sales - Dirty Data for Cleaning Training"
- Link: [PEGAR AQUÍ EL LINK DEL DATASET EN KAGGLE]
- Archivo: dirty_cafe_sales.csv (10.000 filas y 8 columnas)
- Columnas: Transaction ID, Item, Quantity, Price Per Unit, Total Spent, Payment Method, Location, Transaction Date

## Contenido del repositorio

- [NOMBRE DEL NOTEBOOK].ipynb: notebook con todo el proceso
- dirty_cafe_sales.csv: dataset original
- README.md: este archivo

## Diagnóstico inicial

- Las 8 columnas se leían como texto (object), incluso las numéricas y la fecha.
- Había nulos reales, sobre todo en Location (3.265) y Payment Method (2.579).
- Había valores "ERROR" y "UNKNOWN" escondidos como texto, que Pandas no cuenta como nulos.
- No había duplicados: Transaction ID es único en todas las filas.
- Los rangos numéricos eran consistentes: Quantity y Price Per Unit entre 1 y 5, Total Spent entre 1 y 25, y fechas entre el 1/1/2023 y el 31/12/2023.

## Decisiones de limpieza

1. Valores inválidos: "ERROR" y "UNKNOWN" se reemplazaron por nulos reales (NaN) para poder convertir los tipos.
2. Conversión de tipos: Quantity, Price Per Unit y Total Spent a numérico (pd.to_numeric), y Transaction Date a fecha (pd.to_datetime).
3. Recuperación de datos numéricos sin imputar con la media:
   - Total Spent = Quantity x Price Per Unit, y las relaciones inversas cuando faltaba otra de las tres columnas.
   - Cada producto tiene un único precio en el dataset, por lo que el precio faltante se recuperó a partir del Item.
4. Eliminación estratégica: se eliminaron 26 filas donde faltaban dos de las tres columnas numéricas y no se podían reconstruir (quedaron 9.974).
5. Nulos de texto: en Item, Payment Method y Location no se eliminaron filas (se habría perdido más de un tercio del dataset). Se marcaron con la categoría "Sin dato".
6. Filtros booleanos:
   - Se descartaron las filas sin producto identificado (9.011 filas).
   - Se descartaron las filas sin fecha válida (8.593 filas).
   - Se creó un subconjunto aparte, df_completo, con las filas que tienen método de pago y ubicación conocidos (3.570 filas).
7. Columna eliminada: Transaction ID, por ser un identificador único sin valor analítico.
8. Se usó reset_index después de eliminar filas.

Resultado: dataset final de 8.593 filas y 7 columnas (se perdió el 14 % de las filas originales).

## Agrupaciones (groupby)

1. Ingresos por producto: Salad es el que más factura (16.575) y Cookie el que menos (3.076). El número de transacciones es parecido entre productos, por lo que la diferencia viene del precio.
2. Ingresos por método de pago y ubicación (sobre df_completo): reparto muy parejo entre las seis combinaciones, sin un método ni una ubicación dominante.
3. Evolución mensual: ingresos estables durante 2023. Febrero es el mes más bajo (5.953,5) y octubre el más alto (6.755).

## Limitaciones

- Payment Method y Location tienen muchos faltantes, por lo que el análisis de esas variables usa solo el 41 % de las filas y debe tomarse con cautela.
- La categoría "Sin dato" y la eliminación de filas sin fecha son decisiones que podrían revisarse según el objetivo del análisis.

## Cómo ejecutarlo

1. Abrir el notebook en Google Colab o Jupyter.
2. Subir dirty_cafe_sales.csv al mismo entorno que el notebook.
3. Ejecutar todas las celdas en orden.

Librerías: pandas, numpy.
