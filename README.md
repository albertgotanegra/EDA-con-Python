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
Se comprueba la existencia o no de duplicados mediante df.duplicated().sum() ; no existen duplicados en ningún data frame. 

LIMPIEZA Y ESTANDARIZACIÓN

Pasamos todos los nombres de las comlumnas a minúsculas y sin espacio utilizando: df.columns = df.columns.str.lower().str.strip().str.replace(' ', '_')
A continuación, como no hay duplicados, pasamos los nulos a 0. para no perder el resto de datos. Utilizamos la siguiente formula: df_csv = df_csv.fillna(0)
Como da error ya que estoy poniendo 0 en columnas str, cambiamos la formula a una que primero identifique las columnas con numero y convierta los nulos en 0 y luego lo mismo con las que contienen str para reellenarlos con 'desconocido'.

INTEGRACIÓN DE TABLAS

Como se tienen 3 tablas con la misma información correspondiente a 3 años distintos estas tablas se apilaran verticalmente con pd.concat.
Una vez tengamos la unión de estas 3, se uniran a la tabla relacionada con la información adicional mediante la columna de ID con la función pd.merge.
Nos encontramos que debemos cambiar el nombre de la columna de ID_ a ID, por ello utilizamos la formula df_csv = df_csv.rename(columns={'ID_': 'ID'}).

ANÁLISIS

Instalamos matplotlib y seaborn para generar visualizaciones claras.
Se usa la funcion .describe() para obtener promedios, mínimos, máximos y cuartiles de todas las columnas numéricas.
