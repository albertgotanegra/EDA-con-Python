# EDA con Python

TRANSFORMACIÓN Y LIMPIEZA

En primer lugar, se abren los archivos de datos proporcionados con el programa Visual Studio Code.
Se instala el complemento Rainbow CSV para una mejor visualización de los datos.
Me encuentro que VSCode no puede abrir correctamente el archivo .xlsx
Antes de empezar a escribir codigo me aseguro de tener instalado Pandas y el motor necesario para leer archivos de Excel. Para ello escribo en el terminal el siguiente codigo: py -m pip install pandas openpyxl
Importamos la librería de Pandas.
Importamos archivo CSV y visualizamos las primeras filas.
Importamos la hoja de cálculo cargando cada pestaña en un dataframe independiente por si luego tenemos que analizarlas por separado o fusionarlas.
Una vez ya tenemos los archivos proporcionados, integrados en nuestro archivo .ipynb procedemos a estudiar las dimensiones y la muestra utilizando df.shape. 
    Respuesta:
        df_csv (43000, 24)
        df_pestana1 (20115, 7)
        df_pestana2 (8965, 7)
        df_pestana3 (14090, 7)
Se ejecuta df.info() para verificar el tipo de dato. Se puede observar que los datos contenidos en las 3 pestañas del archivo .xlsx son iguales formados por numeros enteros, una columna con fecha y otra con string correspondiente a la ID. Y en relación al otro archivo, esta formado por 24 columnas con mezcla de strings, integer y floats. Se puede observar que en todos los archivos figura la columna de ID por lo que se podrá usar como nexo entre las tablas.
Se comprueba cuantos datos faltan por columna en las distintas tablas mediante: df.isnull().sum() ; en el archivo .csv hay grasn cantidad de nulos en función de la columna, en cambio en las otras tres df no hay ninguno. 
Se comprueba la existencia o no de duplicados mediante df.duplicated().sum() ; 
