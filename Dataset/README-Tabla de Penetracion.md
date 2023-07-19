
# Tabla de penetracion en cada 100 hogares
en esta tabla haremos todas las query necesarias para poder obtener unos datos limpios y para obtener distintos analisis que nos permitiran realizar buenos calculos para lo que requiere la empresa.


primero inportamos las las librerias de Matplotlib.pyplot y  Pandas.

Decidimos leer la tabla en donde se encontraba 

df2 = pd.read_csv("Internet_Penetracion.csv") 

Reemplazamos las comas por puntos en la columna "Accesos por cada 100 hogares":

df2['Accesos por cada 100 hogares'] = df2['Accesos por cada 100 hogares'].str.replace(',', '.')

Convertimos los valores a tipo numérico:

hacemos la conversion a tipo numerico teniendo en cuenta que estos datos tienen coma(,) entre sus divisiones por lo tanto necesitamos que sean numericos, al no realizar esta conversion tendremos problemas para nustros calculos.
df2['Accesos por cada 100 hogares'] = pd.to_numeric(df2['Accesos por cada 100 hogares'])

Filtraamos los datos para el año 2022
aqui filtramos los datos del año 2022 en donde precisamente vamos a mirar como ha sido ese acceso este ultimo año.

df_2022 = df2[df2['Año'] == 2022]

Calcular la media del acceso por cada 100 hogares por provincia en el año 2022. el promedio del acceso que han tenido en este año teniendo en cuenta distintas provincias de argentina.

media_por_provincia = df_2022.groupby('Provincia')['Accesos por cada 100 hogares'].mean().round(2)

Imprimimimos  los resultados
print(media_por_provincia)

#calculamos otro analisis descriptivo como la desviacion (desviacion_estandar_por_provincia) 

desviacion_estandar_por_provincia = df_2022.groupby('Provincia')['Accesos por cada 100 hogares'].std()
print("\nDesviación Estándar:")
print(desviacion_estandar_por_provincia)


Filtramos  los datos para el año 2022
df_2022 = df2[df2['Año'] == 2022]

#grafico de barras

En este punto realizamos un grafico en barras que nos va permitir visualizar mucho mas las medias por provincias en cuanto al acceso de  cada 100 hogares.

df_promedio = df2.groupby('Provincia')['Accesos por cada 100 hogares'].mean()


procedemos a  crear la figura y los ejes teniendo en cuenta que tendremos un eje X y un eje Y para poder realizar las barras.

fig, ax = plt.subplots()

estos serian los elementos que estaran en los dos ejes tanto el eje X e Y.
provincias = df_promedio.index
promedios = df_promedio.values

creamos el grafico de barras 
ax.bar(provincias, promedios)

Configuramos las etiquetas y el título del gráfico
ax.set_xlabel('Provincia')
ax.set_ylabel('Promedio por cada 100 hogares')
ax.set_title('Promedio de accesos por cada 100 hogares por provincia en 2022')

Rotamos las etiquetas del eje x para una mejor legibilidad y comprension
plt.xticks(rotation=90)

Mostramos los valores de promedio sobre las barras
for i, v in enumerate(promedios):
    ax.text(i, v, str(round(v, 2)), ha='center', va='bottom')

#luego procedemos a realizar los analiis vibariados en donde utilizaremos el diagrama de dispersion

Obtenemos los datos del DataFrame para los distintos ejes
x = df2['Accesos por cada 100 hogares']  # Datos para el eje x
y = df2['Provincia']  # Datos para el eje y

Creamos  el gráfico de dispersión utilizando los 2 ejes anterioermente seleccionados

plt.scatter(x, y)

Configuracion de etiquetas y título
configuramos las etiquetas tanto para los ejes como el titolo del grafico.

plt.xlabel('Accesos por cada 100 hogares')
plt.ylabel('Provincia')
plt.title('Gráfico de Dispersión')

Mostramos el gráfico
plt.show()

#OUTLIERS

para los outliers debemos importar las libreria de numpy asi: 

import numpy as np

df2 = pd.read_csv("Internet_Penetracion (1).csv")

Restamos del código para detectar outliers

Q1 = np.percentile(df2['Accesos por cada 100 hogares'], 25)
Q3 = np.percentile(df2['Accesos por cada 100 hogares'], 75)
IQR = Q3 - Q1

Definimos  los límites para detectar outliers por encima y por debajo
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

Encontramos los  outliers
outliers = df2[(df2['Accesos por cada 100 hogares'] < lower_bound) | (df2['Accesos por cada 100 hogares'] > upper_bound)]

Imprimimos todos los outliers
print(outliers)






