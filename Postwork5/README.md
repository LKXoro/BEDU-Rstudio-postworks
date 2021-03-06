
# Postwork Sesión 5.


#### Objetivos

1. A partir del conjunto de datos de soccer de la liga española de las temporadas 2017/2018, 2018/2019 y 2019/2020, crea el data frame `SmallData`, que contenga las columnas `date`, `home.team`, `home.score`, `away.team` y `away.score`; esto lo puede hacer con ayuda de la función `select` del paquete `dplyr`. Luego establece un directorio de trabajo y con ayuda de la función `write.csv` guarda el data frame como un archivo csv con nombre *soccer.csv*. Puedes colocar como argumento `row.names = FALSE` en `write.csv`. 

2. Con la función `create.fbRanks.dataframes` del paquete `fbRanks` importe el archivo *soccer.csv* a `R` y al mismo tiempo asignelo a una variable llamada `listasoccer`. Se creará una lista con los elementos `scores` y `teams` que son data frames listos para la función `rank.teams`. Asigna estos data frames a variables llamadas `anotaciones` y `equipos`.

3. Con ayuda de la función `unique` crea un vector de fechas (`fecha`) que no se repitan y que correspondan a las fechas en las que se jugaron partidos. Crea una variable llamada `n` que contenga el número de fechas diferentes. Posteriormente, con la función `rank.teams` y usando como argumentos los data frames `anotaciones` y `equipos`, crea un ranking de equipos usando unicamente datos desde la fecha inicial y hasta la penúltima fecha en la que se jugaron partidos, estas fechas las deberá especificar en `max.date` y `min.date`. Guarda los resultados con el nombre `ranking`.

4. Finalmente estima las probabilidades de los eventos, el equipo de casa gana, el equipo visitante gana o el resultado es un empate para los partidos que se jugaron en la última fecha del vector de fechas `fecha`. Esto lo puedes hacer con ayuda de la función `predict` y usando como argumentos `ranking` y `fecha[n]` que deberá especificar en `date`.

### Desarrollo

En este postwork se trabajó con la función `create.fbRanks.dataframes` y `select`, por lo que es necesario realizar la instalación de los paquetes `dplyr` y `fbRanks`para continuar con el desarrollo, por lo que al comenzar el código, el primer paso a realizar es llamar las bibliotecas de los paquetes instalados:

```R
library(dplyr)
library(fbRanks)
```
Después de llamar las bibliotecas se procede a establecer el directorio de trabajo para poder crear el data frame requerido y establecer donde será guardado el data frame creado que será utilizado para la función `create.fbRanks.dataframes`:

```R
setwd("C:/...") 
```

Se continua creando la variable *smalldata* importando el dataframe obtenido en el portwork 2:

```R
smalldata <- read.csv("resultado.csv")
smalldata <- select(smalldata,Date:FTAG)
smalldata <- rename(smalldata, date = Date, home.team = HomeTeam, away.team = AwayTeam, home.score = FTHG, away.score = FTAG) 
```

Se reescribe la variable *smalldata* seleccionando únicamente las columnas requeridas para el uso de la función `create.fbRanks.dataframes`, posteriormente se realiza un cambio de nombre a las columnas para que puedan ser detectadas y utilizadas por la función previamente mencionada, reescribiendo nuevamente la variable, para finalmente generar el archivo *soccer.csv*

```R
write.csv(smalldata,row.names = F,"soccer.csv") 
```

A continuación se carga el archivo *soccer.csv* en la variable *listasoccer* con el uso de la función `create.fbRanks.dataframes`, la cual entrega como resultado una lista con 4 elementos, de los cuales serán utilizados los elementos `scores` y `teams`, se asignan a las variables *anotaciones* y *equipos* respectivamente.

<p align="center">
<img src="../Imágenes/Postwork5.1.PNG" align="center" height="262" width="695">
</p>


```R
listasoccer <- create.fbRanks.dataframes("soccer.csv")
anotaciones <- listasoccer$scores
equipos <- listasoccer$teams 
```
Ahora se crea la variable *fecha* haciendo uso de la función `unique`para obtener las fechas sin repetir que se encuentran en la variable *anotaciones*, se crea la variable *n* la cual contendrá el número de fechas distintas, en esta ocasión se hace uso de la función `lenght` para obtener el numero de valores contenidos en *fecha*.

```R
fecha <- unique(anotaciones$date)
n <- length(fecha) 
```

El siguiente paso es generar un ranking de los equipos utilizando la función `rank.teams`, en esta función específicamos como fecha minima la fecha inicial de la base de datos y cómo máxima la fecha del penúltimo partido, la cual podemos obtener al observar la variable *fecha*. Esta función requiere los campos `score` y `teams` a los cuales serán asignados las variables *anotaciones* y *equipos* respectivamente.

```R
ranking <- rank.teams(scores = anotaciones,teams = equipos,max.date = "2020-07-16",min.date = "2017-08-18") 
```

Entregando el siguiente ranking:

<p align="center">
<img src="../Imágenes/Postwork5.2.PNG" align="center" height="395" width="459">
</p>


Por último se estiman las probabilidad de los siguientes eventos: el equipo de casa gana, el equipo visitante gana o el resultado es un empate para los partidos que se jugaron en la última fecha de la variable *fecha*, utilizando la función `predict` y los elementos `ranking` y `fecha[n]` como argumentos para la misma.

```R
predict(ranking, date=fecha[n])
```

Entregando como resultado lo siguiente:

<p align="center">
<img src="../Imágenes/Postwork5.3.PNG" align="center" height="147" width="682">
</p>

<br/>

[`Anterior`](../Postwork4) | [`Siguiente`](../Postwork6)      

</div>
