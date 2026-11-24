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
Se utiliza la función .groupby para combinar distintas columnas del df_definitivo y así obtener datos numerícos en función de distintas características de los clientes. 
Se utilizan los graficos de barras (análisis univariado) para ver la distribución de la cantidad de clientes con cada tipo de estudios.
Se utiliza el análisis bivariado para ver que variables numéricas suben y bajan juntas.
-Se crea una copia del df_definitivo para trabajar la parte del análisis correctamente reutilizando el Dataframe.

CONCLUSIONES

El análisis exploratorio realizado sobre el conjunto de datos `df_eda` ha permitido obtener una visión general de las características de los clientes, sus ingresos, su situación familiar, sus características sociodemográficas y los resultados de las campañas comerciales.

En primer lugar, tras integrar las diferentes fuentes de información, se ha obtenido un conjunto de datos formado por **43.170 registros y 30 variables**. La combinación de las tablas ha permitido enriquecer la información inicial y relacionar los datos de los clientes con sus características personales y económicas.

Durante la exploración se ha comprobado que no existen registros duplicados completos. Sin embargo, se han identificado algunos valores ausentes y registros con información desconocida, especialmente en variables como edad, nivel educativo, profesión y fecha.

También destaca la presencia de 5.120 registros con edad igual a cero, que representan aproximadamente el 11,86 % de los registros de la tabla original. Estos valores deben considerarse potencialmente inválidos o desconocidos, por lo que conviene tenerlos en cuenta en los análisis relacionados con la edad.

Asimismo, se han detectado 170 registros sin correspondencia en la integración de las tablas, lo que explica parte de los valores ausentes presentes en el conjunto final.

Por tanto, aunque el proceso de integración ha permitido construir una base de datos más completa, todavía existen aspectos relacionados con la calidad de la información que deben considerarse al interpretar los resultados.

En relación con los ingresos, se observa que los clientes presentan unos ingresos medios de aproximadamente 93.227, mientras que la mediana se sitúa en 93.009,50. Estos valores relativamente próximos indican que la distribución está bastante centrada alrededor de los 93.000, aunque existe una dispersión considerable entre clientes.

Respecto a la composición familiar, tanto la media de hijos (`kidhome`) como la de adolescentes (`teenhome`) se sitúa aproximadamente en un miembro por hogar. Esto permite identificar una presencia significativa de familias con hijos y adolescentes dentro de la muestra.

Por otro lado, el número medio de visitas mensuales a la página web es de aproximadamente 16,59, lo que refleja una actividad considerable de los clientes en los canales digitales.

Respecto al perfil sociodemografico, el análisis de las variables categóricas muestra que la profesión más frecuente es la categoría administrativa (`admin.`), con un 25,19 % de los registros, seguida de los trabajadores del sector industrial y oficios (`blue-collar`), con un 22,36 %, y los técnicos (`technician`), con un 16,28 %.

En cuanto al estado civil, predominan claramente los clientes casados, que representan el 60,22 % de la muestra, seguidos de los solteros, con un 28,04 %.

Respecto al nivel educativo, destacan los clientes con estudios universitarios, que representan el 29,47 %, seguidos de aquellos con estudios de secundaria (`high.school`), con un 22,99 %.

Estos resultados permiten concluir que la muestra está formada principalmente por personas casadas, con formación secundaria o universitaria y vinculadas especialmente a ocupaciones administrativas, industriales y técnicas.

Por lo que hace al análisis de los ingresos, según el nivel educativo muestra que las diferencias entre las medias de los distintos grupos son relativamente reducidas.

Los clientes con estudios de secundaria presentan los ingresos medios más elevados, con aproximadamente 94.142, seguidos de aquellos con formación profesional, con 93.669.

Por el contrario, los grupos con estudios básicos presentan medias ligeramente inferiores. No obstante, las diferencias no son suficientemente amplias como para afirmar que exista una relación claramente determinante entre el nivel educativo y los ingresos.

Además, la elevada dispersión observada dentro de cada categoría indica que existen diferencias importantes de ingresos entre personas con el mismo nivel educativo.

En cuanto al análisis de correlaciones, la matriz de correlación permite identificar algunas relaciones importantes entre las variables numéricas.

Destaca especialmente la elevada correlación positiva entre la variación de la tasa de empleo (`emp.var.rate`) y el Euríbor a tres meses (`euribor3m`), con un coeficiente de 0,97. También existe una correlación positiva elevada entre el Euríbor y el número de empleados (`nr.employed`), de aproximadamente 0,95.

Estas relaciones sugieren que determinadas variables económicas están estrechamente relacionadas entre sí.

Por otro lado, la correlación entre edad e ingresos es prácticamente inexistente, con un coeficiente de 0,0096. Esto indica que, en términos lineales, la edad no presenta una asociación significativa con los ingresos dentro de esta muestra.

También se observa una correlación negativa entre `pdays` y `previous` (-0,59), relacionada con el historial de contactos anteriores de los clientes.

A continuación, se puede observar que uno de los resultados más relevantes del análisis es la distribución de la variable `y`, que representa el resultado de la campaña.

Se observa que el 88,39 % de los registros corresponde a resultados negativos (`no`), mientras que únicamente el 11,22 % corresponde a resultados positivos (`yes`). El 0,39 % restante presenta valores ausentes.

Esta distribución evidencia que la proporción de clientes que aceptan la oferta es considerablemente inferior a la de aquellos que no lo hacen.

Al analizar la tasa de respuesta según diferentes características, aparecen diferencias interesantes:

- **Profesión:** los estudiantes presentan una tasa de respuesta del 31,34 %, seguidos de los jubilados, con un 25,20 %.
- **Estado civil:** los clientes solteros presentan una tasa de respuesta del 13,93 %, superior a la de los casados y divorciados.
- **Nivel educativo:** se observan diferencias entre categorías, aunque los resultados del grupo de personas sin alfabetización deben interpretarse con precaución debido a su reducido tamaño muestral.
- **Tipo de contacto:** el contacto mediante teléfono móvil (`cellular`) presenta una tasa de respuesta del 14,74 %, frente al 5,16 % del contacto telefónico convencional.

Estos resultados sugieren que determinadas características personales y, especialmente, el canal de comunicación pueden estar asociadas a una mayor probabilidad de respuesta positiva a las campañas.

Finalmente, en términos generales, el análisis exploratorio de `df_eda` ha permitido identificar las principales características de los clientes y detectar patrones relevantes en su comportamiento.

Los resultados muestran que las campañas comerciales presentan una tasa de aceptación relativamente baja y que existen diferencias de respuesta según la profesión, el estado civil, el nivel educativo y el canal de contacto utilizado.

Asimismo, se ha comprobado que algunas variables económicas presentan relaciones muy fuertes entre sí, mientras que otras, como la edad y los ingresos, muestran una asociación lineal prácticamente inexistente.

Por tanto, el análisis pone de manifiesto la importancia de segmentar adecuadamente a los clientes y adaptar las estrategias comerciales a sus características y comportamientos. En particular, el canal de contacto y determinados perfiles profesionales podrían constituir elementos de interés para mejorar la eficacia de futuras campañas.
