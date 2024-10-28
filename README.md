---
"Análisis y Predicción de Precios de Computadoras Portátiles Usando 
Factores Técnicos y de Marca"
author: "Sara Villada, Maria Buitrago, Annie Arenilla"
![imagen](https://github.com/user-attachments/assets/526ed078-fcde-4fc6-b5bb-190af26f4773)


```{r include= FALSE}
library(readr)
datos <- read_csv("datos.csv")
```

## Problemática

La medición de los precios de los computadores portátiles es crucial para las personas que utilizan estos dispositivos electrónicos, ya que el precio es un factor determinante en sus decisiones de compra. Los computadores portátiles se han convertido en herramientas esenciales para la vida cotidiana, utilizados para trabajar, estudiar y entretenimiento.

Estos dispositivos contienen una gran diversidad de características y especificaciones técnicas,lo cual puede impactar en el precio. Por esta razón es importante analizar y entender cómo distintas variables, como el almacenamiento, la memoria RAM, la garantía, y el tamaño de la pantalla, entre otros, pueden influir en el precio de un portátil.

## Introducción

A traves del análisis de los datos se pretende resolver a las siguientes preguntas:

### Pregunta principal

1.  ¿Cómo influyen las especificaciones técnicas y la marca en el precio de los portátiles?

### Preguntas auxiliares

1.  ¿Cómo afecta el almacenamiento de los portátiles en relación a el precio de estos?
2.  ¿Existe relación entre el precio de los portátiles y la tarjeta gráfica?

## Objetivos

### Objetivo general

Analizar en base a los resultado del modelo realizado con regresión lineal múltiple como las especificaciones técnicas, como lo son: La marca, el modelo, el procesador, almacenamiento, tamaño de pantalla, tarjeta gráfica, sistema operativo, peso, duración de la batería y garantía, influyen en el precio de los portátiles.

### Objetivos específicos

-   Evaluar como la capacidad de almacenamiento influye en la variación de los precios de los portátiles, identificando la relación entre diferentes tamaños de almacenamiento y su impacto en el costo final.

-   Estudiar como el tipo de unidad del procesamiento gráfico se ve relacionada con el precio de los portátiles

## Datos

El conjunto de datos que se analizaran corresponden a 3000 observaciones y 12 variables, de las cuales 7 son cualitativas y 5 son cuantitativas. La @tbl-Tabla1 presenta una descripción de la clasificación de las variables.

| Variable   | Tipo         |
|------------|--------------|
| Brand      | Cualitativa  |
| Processor  | Cualitativa  |
| RAM        | Cualitativa  |
| Storage    | Cualitativa  |
| Graphics   | Cualitativa  |
| System     | Cualitativa  |
| Warranty   | Cualitativa  |
| Model      | Cuantitativa |
| ScreenSize | Cuantitativa |
| Weight     | Cuantitativa |
| Battery    | Cuantitativa |
| Price      | Cuantitativa |

: Clasificación de variables {#tbl-Tabla1}

### Descripción de las variables

A continuación, se presentan la descripción de las variables de interés en este estudio, las cuales pueden influir en la predicción del precio de los portátiles:

1.  **Brand (Marca):** Marca fabricante del portátil.
2.  **Processor (Procesador):** Tipo de procesador que tiene cada portátil.
3.  **RAM:** Cantidad de memoria del portátil.
4.  **Storage (Almacenamiento):** Capacidad de almacenamiento del portátil.
5.  **Graphics (Tarjeta gráfica):** Tipo de unidad de procesamiento gráfico del portátil.
6.  **System (Sistema Operativo):** Sistema operativo que trae cada portátil.
7.  **Warranty (Garantía):** Duración de la garantía en años de cada portátil.
8.  **Model (Modelo):** Identificador único para cada portátil.
9.  **ScreenSize (Tamaño de Pantalla):** Tamaño físico en pulgadas de la pantalla de cada portátil.
10. **Weight (Peso):** Peso en kg de cada portátil.
11. **Battery (Duración de Batería):** Duración estimada en horas de la batería de cada portátil.
12. **Price (Precio):** Precio en dólares estadounidenses de cada portátil.

## Análisis exploratorio

Se hará un análisis de las gráficas usadas en el análisis exploratorio tanto para las variables cuantitativas como para las cualitativas. De igual manera se pretende analizar las Anovas realizadas. Se comenzará con las variables cuantitativas que previamente fueron agrupadas en un subconjunto. De dichas variables se realizo un mapa de calor y una gráfica de matrices. Para la variable respuesta **Price** se realizó un histograma en el cual se puede observar la distribución y frecuencia de los precios, posteriormente se realizaron los boxplots paras las variables cualitativas y las Anovas que pretenden analizar el impacto de cada una de estas variables en el precio de los portátiles. Este análisis se hace con el fin de averiguar que variables vale la pena investigar a mayor profundidad.

### Mapa de calor

La primera gráfica realizada fue un mapa de calor que nos muestra la correlación entre la variable respuesta (precio) y las variables regresoras (la batería y el peso), en el mapa de calor se observa esta correlación por colores, siendo azul el valor más bajo (-1.0) y el rojo el valor más alto (1.0).

```{r}
#| echo: false
#| warning: false
# Cargar librerías necesarias
library(GGally)
library(ggplot2)

VAR_CUANTI <- subset(datos, select = c("Weight", "Battery", "Price"))

# Crear la gráfica de correlación
ggcorr(VAR_CUANTI, method = c("pairwise.complete.obs", "pearson"),
       label = TRUE, label_size = 3, 
       low = "blue", high = "red", mid = "gray")

```

En este caso tanto para el precio y la batería, como para el precio y el peso, se observa un tono muy claro, dicho color indica que, hay poca o ninguna relación entre estas variables y no hay una relación lineal importante entre ellas. Lo dicho anteriormente implica que, estas características de los portátiles no influyen de forma notable en el precio de éstos.

### Gráfica de matrices

La segunda gráfica realizada fue una gráfica de matrices, en esta gráfica se pueden analizar, correlaciones, gráficos de dispersión y gráficos de densidad.

```{r}
#| echo: false
#| warning: false
#| # Cargar librerías necesarias
library(GGally)
library(ggplot2)

VAR_CUANTI <- subset(datos, select = c("Weight", "Battery", "Price"))

# Crear gráfico de matriz de pares
ggpairs(VAR_CUANTI)
```

En las correlaciones se puede observar una correspondencia con los resultados obtenidos a través del mapa de calor donde los valores entre las correlaciones son muy pequeños, a partir de esto también se puede interpretar que es muy poco probable que haya colinealidad entre las variables.

De los gráficos de dispersión se puede decir que es una distribución aleatoria por lo tanto no se pueden hacer muchas interpretaciones a partir de este ya que los gráficos no presentan patrones claros ni tendencias.

Los gráficos de densidad muestran distribuciones que varían en diferentes rangos de valores, es decir que las variables parecen tener distribuciones más complejas, con concentraciones de datos en ciertos valores.

### Histograma

El tercer gráfico realizado fue un histograma y se le realizo a la variable respuesta **Price**, este gráfico nos permite hacer observaciones sobre la distribución de los precios y la frecuencia.

```{r}
#| echo: false
#| warning: false

library(GGally)
library(ggplot2)

VAR_CUANTI <- subset(datos, select = c("Weight", "Battery", "Price"))

# Crear el histograma de la variable 'Price'
hist(VAR_CUANTI$Price, 
     xlab = "Precio", 
     ylab = "Frecuencia", 
     main = "Histograma de la Variable Price", 
     col = "pink")

```

En el histograma se observa una distribución uniforme puesto que las barras tienen alturas similares a lo largo de la mayoría de los precios

En la frecuencia podemos ver que la mayoría de los precios esta en un rango de 500 y 3000. También se observa que no hay valores atípicos ya que no hay valores extremadamente altos o bajos fuera del rango previamente mencionado.

Ya que los precios tienen una distribución bastante uniforme se puede decir que los precios están bien distribuidos por lo que no hay una fuerte concentración en un rango específico de precios.

### Boxplot

Después de realizar las gráficas para las variables cuantitativas se procedió a hacer los boxplots para las variables cualitativas.

```{r}
#| echo: false
#| warning: false

library(GGally)
library(ggplot2)
par(mfrow = c(3, 3))
boxplot(Price ~ Brand, data = datos, main = "Price vs Brand", col = "blue")
boxplot(Price ~ ScreenSize, data = datos, main = "Price vs ScreenSize", col = "orange")
boxplot(Price ~ Processor, data = datos, main = "Price vs Processor", col = "green")
boxplot(Price ~ RAM, data = datos, main = "Price vs RAM", col = "red")
boxplot(Price ~ Storage, data = datos, main = "Price vs Storage", col = "yellow")
boxplot(Price ~ Graphics, data = datos, main = "Price vs Graphics", col = "purple")
boxplot(Price ~ System, data = datos, main = "Price vs System", col = "pink")
boxplot(Price ~ Warranty, data = datos, main = "Price vs Warranty", col = "brown")
par(mfrow = c(1, 1))
```

#### Precio vs Marca

Las marcas muestran una dispersión similar en el precio. Ninguna marca parece tener una ventaja clara en términos de precios más altos o más bajos. Las medianas están bastante alineadas, lo que indica que no hay grandes diferencias en el precio según la marca, no se observan valores atípicos notables y se puede notar una distribución simétrica entre los precios.

#### Precio vs Tamaño de pantalla

Los tamaños de pantalla parecen no influir significativamente en el precio ya que las medianas son similares, hay poca variabilidad en los precios de los diferentes tamaños de pantalla lo que sugiere que el tamaño de la pantalla no es un factor clave que afecta el precio, no se observan valores atípicos y tienen una distribución simétrica.

#### Precio vs Procesador

Las medianas de los procesadores son muy parecidas, y la variabilidad de sus precios es consistente lo que indica que el procesador no es muy influyente en el precio, no se observan valores atípicos ni asimetría por lo que hay una distribución de los precios balanceada dentro de cada tipo de procesador.

#### Precio vs RAM

El precio medio de las RAM de 16 y 64 es ligeramente mayor a las de 8 y 32, pero no se sigue una tendencia clara en la media de los precios, no hay mucha variabilidad y tampoco se observan valores atípicos.

#### Precio vs Almacenamiento

Los tamaños de almacenamiento no muestran un impacto claro en el precio. Las medianas y la dispersión son muy similares, lo que sugiere que el almacenamiento no es un factor decisivo en la variación de precios, tampoco se observan valores atípicos

#### Precio vs Tarjeta gráfica

Las medianas son casi idénticas, lo que indica que las tarjetas gráficas no afectan mucho el precio central, casi no hay variabilidad y no se observan valores atípicos

#### Precio vs Sistema operativo

Los sistemas operativos muestran una dispersión similar en precios, con medianas muy cercanas. No parece haber una diferencia importante en el precio en función del sistema operativo y no se observan valores atípicos.

#### Precio vs Garantía

Las garantías muestran medianas y dispersiones muy parecidas. El número de años de garantía no parece tener un impacto significativo en el precio del producto y no se observan valores atípicos.

### Análisis Anovas

Una vez analizados los gráficos tanto para las variables cuantitativas como para las cualitativas se procedió a hacer el análisis de las Anovas del precio con cada una de las variables cualitativas.

1.  Precio con la marca.

```{r}
#| echo: false
#| warning: false
analisis1<-aov(Price~Brand, data=datos)
summary(analisis1)
```

En esta anova el valor p tuvo un valor de **0.837**, con este valor se puede concluir que como el valor p es mayor al nivel de significancia del 0.05 y el valor F **0.498** es menor a los grados de libertad (7) no se rechaza la hipótesis nula y se dice que la marca no es significativa, es decir, que no explica el cambio en los precios.

2.  Precio con el tamaño de la pantalla.

```{r}
#| echo: false
#| warning: false
analisis2<-aov(Price~ScreenSize, data=datos)
summary(analisis2)
```

En esta anova el valor p tuvo un valor de **0.458**, con este valor se puede concluir que como el valor p es mayor al nivel de significancia del 0.05 y el valor F **0.551** es menor a los grados de libertad (1) no se rechaza la hipótesis nula y se dice que el tamaño de la pantalla no es significativo, es decir, que no explica el cambio en los precios.

3.  Precio con el procesador.

```{r}
#| echo: false
#| warning: false
analisis3<-aov(Price~Processor, data=datos)
summary(analisis3)
```

En esta anova el valor p tuvo un valor de **0.456**, con este valor se puede concluir que como el valor p es mayor al nivel de significancia del 0.05 y el valor F **0.937** es menor a los grados de libertad (5) no se rechaza la hipótesis nula y se dice que el procesador no es significativo, es decir, que no explica el cambio en los precios.

4.  La cuarta anova realizada fue del precio con la RAM.

```{r}
#| echo: false
#| warning: false
analisis4<-aov(Price~RAM, data=datos)
summary(analisis4)
```

En esta anova el valor p tuvo un valor de **0.46**, con este valor se puede concluir que como el valor p es mayor al nivel de significancia del 0.05 y el valor F **0.547** es menor a los grados de libertad (1) no se rechaza la hipótesis nula y se dice que la RAM no es significativa, es decir, que no explica el cambio en los precios.

5.  Precio con el almacenamiento.

```{r}
#| echo: false
#| warning: false
analisis5<-aov(Price~Storage, data=datos)
summary(analisis5)
```

En esta anova el valor p tuvo un valor de **0.846**, con este valor se puede concluir que como el valor p es mayor al nivel de significancia del 0.05 y el valor F **0.037** es menor a los grados de libertad (1) no se rechaza la hipótesis nula y se dice que el almacenamiento no es significativo, es decir, que no explica el cambio en los precios.

6.  Precio con la tarjeta gráfica.

```{r}
#| echo: false
#| warning: false
analisis6<-aov(Price~Graphics, data=datos)
summary(analisis6)
```

En esta anova el valor p tuvo un valor de **0.636**, con este valor se puede concluir que como el valor p es mayor al nivel de significancia del 0.05 y el valor F **0.637** es menor a los grados de libertad (4) no se rechaza la hipótesis nula y se dice que la tarjeta gráfica no es significativa, es decir, que no explica el cambio en los precios.

7.  Precio con el sistema operativo.

```{r}
#| echo: false
#| warning: false
analisis7<-aov(Price~System, data=datos)
summary(analisis7)
```

En esta anova el valor p tuvo un valor de **0.148**, con este valor se puede concluir que como el valor p es mayor al nivel de significancia del 0.05 y el valor F **1.783** es menor a los grados de libertad (3) no se rechaza la hipótesis nula y se dice que el sistema operativo no es significativo, es decir, que no explica el cambio en los precios.

8.  Precio con la garantía.

```{r}
#| echo: false
#| warning: false
analisis8<-aov(Price~Warranty, data=datos)
summary(analisis8)

```

En esta anova el valor p tuvo un valor de **0.866**, con este valor se puede concluir que como el valor p es mayor al nivel de significancia del 0.05 y el valor F **0.028** es menor a los grados de libertad (1) no se rechaza la hipótesis nula y se dice que la garantía no es significativa, es decir, que no explica el cambio en los precios.

-   Del análisis exploratorio se revela que, tanto en las gráficas de las variables cuantitativas como en los boxplots de las variables cualitativas, no se encuentran relaciones significativas que expliquen variaciones en el precio de los portátiles. Los mapas de calor y gráficos de matrices indican una variación baja entre las variables cuantitativas y el precio, mientras que las Anovas realizadas para cada variable cualitativa confirman que factores como marca, tamaño de pantalla, procesador, RAM, almacenamiento, tarjeta gráfica, sistema operativo y garantía no tienen un impacto significativo en el precio.

## Multicolinealidad

Para identificar si las variables tienen multicolinealidad entre ellas se realizó la prueba del vif que nos indica la multicolinealidad de las variables cuando el valor del vif es mayor a 5.

**Para las variables cuantitativas**

```{r}
#| echo: false
#| warning: false

library(car)

# Supongamos que 'datos' es tu data frame
VAR_CUANTI <- subset(datos, select = c("Weight", "Battery", "Price"))

# Ajustar el modelo
Modelo1 <- lm(Price ~ ., data = VAR_CUANTI)

# Calcular el VIF
vif_values <- vif(Modelo1)

# Mostrar los valores de VIF
print(vif_values)
```

Como se puede observar en los valores del vif de estas variables, no existe multicolinealidad entre ellas ya que el vif nos dio un valor menor a **5**, esta conclusión es solo una confirmación de lo que ya se esperaba, pues recordemos que en el gráfico de matrices se pudo observar una correlación muy baja entre ellas lo que supone que la probabilidad de que hubiera multicolinealidad era muy pequeña, sin embargo con  el valor del vif se puede confirmar esta suposición y estar totalmente seguros de que no hay multicolinealidad.

## Modelos

Se decide empezar a analizar primero el modelo con todas las variables de la base de datos, excluyendo la variable "Model", ya que esta es una variable clasificatoria y al dejarla en el modelo podría presentar posibles inconsistencias y daba un resultado invalido.

### Modelo

```{r}
#| echo: false
#| warning: false
Modelo<-lm(Price~.-Model, data=datos)
summary(Modelo)

```

En el **Modelo** de las 32 variables solo una de ellas da significativa, en el resto de las variables su valor p es por encima del alpha y el R-squared arroja un valor negativo, se decide no validar el modelo, ya que sus valores indican que el modelo actual no tiene la capacidad para predecir las variaciones en la variable respuesta (Price).

### Correlación de variables con la variable respuesta

Para seleccionar el siguiente modelo, se realizó un análisis de correlación entre las variables predictoras y la variable de respuesta. Este enfoque permite identificar las variables con mayor relacion con la variable respuesta, facilitando así la selección de aquellas que contribuyen de manera más significativa al modelo y a la hora de interpretar se obtengan mejores resultados.

#### Redefinir variables

```{r}
datos$ScreenSize <- as.numeric(as.character(datos$ScreenSize))
datos$RAM <- as.numeric(as.character(datos$RAM))
datos$Storage <- as.numeric(as.character(datos$Storage))
datos$Weight <- as.numeric(as.character(datos$Weight))
datos$Weight <- as.numeric(as.character(datos$Weight))
datos$Warranty <- as.numeric(as.character(datos$Warranty))

```

#### Correlaciones con todas las variables

```{r}
cor(datos$Price, datos$ScreenSize)
cor(datos$Price, datos$RAM)
cor(datos$Price, datos$Storage)
cor(datos$Price, datos$Weight)
cor(datos$Price, datos$Battery)
cor(datos$Price, datos$Warranty)

```

Luego de realizar el análisis de correlación entre las variables y evaluar los resultados, se decidió seleccionar para el siguiente modelo las variables con mayor asociación con la variable de respuesta: **Warranty**, **RAM**, **Storage** y **ScreenSize**.

### Modelo 1

```{r}
#| echo: false
#| warning: false
datos$Warranty <- as.factor(datos$Warranty)
datos$RAM <- as.factor(datos$RAM)
datos$Storage <- as.factor(datos$Storage)
datos$ScreenSize <- as.factor(datos$ScreenSize)
modelo1<-lm(Price~Warranty+RAM+Storage+ScreenSize, data = datos)
summary(modelo1)
```

Para el **Modelo 1**, se decidió analizar las variables mencionadas previamente en el estudio de correlación, con el objetivo de observar si el R\^2 presentaba alguna mejora en comparación con el modelo principal. En este análisis, se consideraron las variables Warranty, RAM, Storage y ScreenSize como cualitativas, agrupándolas en tres categorías cada una: Warranty en **Warranty2** y **Warranty3**; RAM en **RAM16**, **RAM32** y **RAM64**; Storage en **Storage512**, **Storage1024** y **Storage2048**; y ScreenSize en **ScreenSize14**, **ScreenSize15.6** y **ScreenSize17**.

A pesar de la reducción en el número de variables, como se observa en el resumen del modelo, la única variable que muestra significancia es **Warranty2**, con un valor p de 0.0424. Las demás variables presentan un valor p superior al nivel de significancia de 0.05. Por otro lado, el R\^2 ajustado del modelo es extremadamente bajo (5.712e-07), lo que indica que estas variables prácticamente no explican los cambios de la variable de respuesta Price.

#### Validez

##### Normalidad

```{r}
#| echo: false
#| warning: false
library(lmtest)
library(tseries)
Residuales <-rstandard(modelo1)
shapiro.test(Residuales)
```

##### Gráfica de Residuos

```{r}
#| echo: false
#| warning: false
modelo1 <- lm(Price~Warranty+RAM+Storage+ScreenSize, data = datos)
library(car)
# Graficar los residuales
qqPlot(Residuales, 
       pch = 19, 
       col = "blue", 
       col.lines = "purple",
       main = "QQ-Plot de los Residuales")
```

Para definir si los residuales del **Modelo 1** siguen una distribución normal se utiliza la prueba estadística Shapiro-Wilk, donde el valor p es menor al nivel de significancia de 0.05, lo que indica que los residuales del **Modelo 1** no sigue una distribución normal. En cuanto al QQ-Plot, se puede observar la banda de confianza representada por la línea morada, y se nota que algunos de los puntos residuales no se ajustan a esta banda, lo que también confirma que los residuales del modelo no siguen una distribución normal.

##### Varianza constante

```{r}
#| echo: false
#| warning: false
bptest(modelo1)
```

##### Gráfica de Residuos

```{r}
#| echo: false
#| warning: false
library(ggplot2)
datos$Valores_ajustados <- modelo1$fitted.values
plot(datos$Valores_ajustados , Residuales,xlab= "Valores ajustados", ylab= "Residuales",
     main= "Valores ajustados vs residuales", pch = 20, col= "blue")
abline(h=0, col="purple", lwd=2)
```

Para definir si los residuales del **Modelo 1** tiene una varianza constante (homocedasticidad), se utiliza la prueba estadística Breusch-Pagan,en donde el valor p de la prueba es de **0.2586**, el cual es mayor al nivel de significancia de 0.05, lo que significa que los residuales tiene homocedasticidad. Además, en la gráfica de valores ajustados vs. residuales se observa que los puntos están distribuidos de manera uniforme alrededor de la línea y = 0 y no se detecta un patrón de embudo, lo que puede indicar que los residuos siguen una varianza constante.

##### Independencia

```{r}
#| echo: false
#| warning: false
dwtest(modelo1)
bgtest(modelo1)
```

Para determinar si los residuos del **Modelo 1** son independientes (es decir, si no presentan autocorrelación), se utilizaron las pruebas estadísticas de Durbin-Watson y Breusch-Godfrey,en la prueba de Durbin-Watson, el valor p fue **0.06696** y en la prueba de Breusch-Godfrey el valor p obtenido fue **0.1372**, ambos valores mayores al nivel de significancia de 0.05, lo cual confirma que no hay autocorrelación significativa y que los residuales son independientes.

#### Transformación

Se decide aplicar una transformación logarítmica natural a la variable respuesta Price en el Modelo 1, con el objetivo de mejorar el ajuste del modelo, así como verificar si se puede mejorar la normalidad de los residuos.

```{r}
#| echo: false
#| warning: false
modelo1.1 <- lm(log(Price)~ Warranty+RAM+Storage+ScreenSize, data = datos)
summary(modelo1.1)
```

##### Normalidad

```{r}
#| echo: false
#| warning: false
datos$Residuales <-rstandard(modelo1.1)
shapiro.test(datos$Residuales)
library(tseries)
library(lmtest)

```

En la transformación realizada para el **Modelo 1** no se observó mejora en el R\^2, al contrario, obtuvo valor negativo. Además, ninguna de las variables resultó ser estadísticamente significativa. En cuanto a la normalidad, no hubo ninguna mejora, lo que indica que los residuos siguen sin ajustarse a una distribución normal.

### Modelo 2

```{r}
#| echo: false
#| warning: false
datos$Warranty <- as.factor(datos$Warranty)
datos$Storage <- as.factor(datos$Storage)
modelo2<-lm(Price~Warranty+Storage, data = datos)
summary(modelo2)
```

Para el **Modelo 2** se decide dejar en base al modelo anterior las variables en las que su valor p fue mas bajo. En este análisis, se tomaron las variables Storage y Warranty como variables cualitativas, dividiéndola en tres categorías: Warranty en **Warranty2** y **Warranty2**; Storage en **Storage512**, **Storage1024** y **Storage2048**.

Como se evidencia en el summary del modelo, solo la variable **Warranty2** es estadísticamente significativa, ya que su valor p es de **0.040**, menor al nivel de significancia, las demás variables estan por encima del nivel de significancia. Además, el R\^2 ajustado del modelo es muy bajo (0.0008861), lo que indica que estas variables no explican casi nada de la variación del precio de los portátiles. Por lo tanto, se concluye que este conjunto de variables no tienen una relación significativa con el precio de los portátiles.

#### Validez

##### Normalidad

```{r}
#| echo: false
#| warning: false
library(lmtest)
library(tseries)
Residuales <-rstandard(modelo2)
shapiro.test(Residuales)
```

##### Gráfica de Residuos

```{r}
#| echo: false
#| warning: false
modelo2 <- lm(Price~Warranty+Storage, data = datos)
library(car)
# Graficar los residuales
qqPlot(Residuales, 
       pch = 19, 
       col = "blue", 
       col.lines = "purple",
       main = "QQ-Plot de los Residuales")
```

Para definir si los residuales del **Modelo 2** siguen una distribución normal se utiliza la prueba estadística Shapiro-Wilk, donde el valor p es menor al nivel de significancia de 0.05, lo que indica que los residuales del **Modelo 2** no sigue una distribución normal. En cuanto al QQ-Plot, se puede observar la banda de confianza representada por la línea morada, y se nota que algunos de los puntos residuales no se ajustan a esta banda, lo que también confirma que los residuales del modelo no siguen una distribución normal.

##### Varianza constante

```{r}
#| echo: false
#| warning: false
bptest(modelo2)
```

##### Gráfica de Residuos

```{r}
#| echo: false
#| warning: false
library(ggplot2)
datos$Valores_ajustados <- modelo2$fitted.values
plot(datos$Valores_ajustados , Residuales,xlab= "Valores ajustados", ylab= "Residuales",
     main= "Valores ajustados vs residuales", pch = 20, col= "blue")
abline(h=0, col="purple", lwd=2)
```

Para determinar si el **Modelo 2** tiene varianza constante(homocedasticidad), se aplica la prueba estadística Breusch-Pagan, donde el valor p obtenido es **0.0747** el cual es mayor, pero muy cercano al nivel de significancia de 0.05, lo que sugiere que no se puede rechazar completamente la hipótesis nula de homocedasticidad. Al observar la gráfica de valores ajustados vs. residuales se puede decir que los residuales cumplen con el supuesto de la homocedasticidad , ya que se nota que los puntos, a pesar de estar en lineas, están dispersos alrededor de la línea y = 0 y además no sé observa un patrón de embudo lo cual es una indicación de que la varianza de los residuos se mantiene constante.

##### Independencia

```{r}
#| echo: false
#| warning: false
dwtest(modelo2)
bgtest(modelo2)
```

Para determinar si los residuos del **Modelo 2** son independientes (es decir, si no presentan autocorrelación), se utilizaron las pruebas estadísticas de Durbin-Watson y Breusch-Godfrey,en la prueba de Durbin-Watson, el valor p fue **0.0563** y en la prueba de Breusch-Godfrey el valor p obtenido fue **0.1167**, ambos valores mayores al nivel de significancia de 0.05, lo cual confirma que no hay autocorrelación significativa y que los residuales son independientes.

#### Transformación

Se decide aplicar una transformación logarítmica natural a la variable respuesta Price en el Modelo 2, con el objetivo de mejorar el ajuste del modelo, así como verificar si se puede mejorar la normalidad de los residuos y lograr una varianza constante.

```{r}
#| echo: false
#| warning: false
modelo2.1 <- lm(log(Price)~ Storage + Warranty, data = datos)
summary(modelo2.1)
```

##### Normalidad

```{r}
#| echo: false
#| warning: false
datos$Residuales <-rstandard(modelo2.1)
shapiro.test(datos$Residuales)
library(tseries)
library(lmtest)

```

##### Varianza constante

```{r}
#| echo: false
#| warning: false
bptest(modelo2.1)
```

En la transformación realizada para el **Modelo 2** no se observo mejora en el R\^2. Además, ninguna de las variables resultó ser estadísticamente significativa. En cuanto a la normalidad, no hubo ninguna mejora, lo que indica que los residuos siguen sin ajustarse a una distribución normal. Por otro lado, para la varianza constante, el valor p aumento un poco lo sugiere que hay homocedasticidad en el modelo.

### Modelo 3

```{r}
#| echo: false
#| warning: false
datos$Storage <- as.factor(datos$Storage)
modelo3 <- lm(Price ~ Storage, data = datos)
summary(modelo3)
```

Para el **Modelo 3** se decide investigar cómo afecta el almacenamiento de los portátiles en relación con el precio de estos dispositivos. En este análisis, se tomó la variable Storage como una variable cualitativa, dividiéndola en tres categorías: **Storage512**, **Storage1024** y **Storage2048**.

Como se evidencia en el summary del modelo, ninguna de estas variables es estadísticamente significativa, ya que los valores p para Storage512 (0.126), Storage1024 (0.460) y Storage2048 (0.662) son mayores al nivel de significancia estándar de 0.05. Además, el R\^2 del modelo es muy bajo (0.00084) y el R\^2 ajustado es negativo (-0.00016), lo que indica que el almacenamiento no explica prácticamente nada de la variabilidad en el precio de los portátiles. Por lo tanto, se concluye que la variable Storage no tiene una relación significativa con el precio de los portátiles en este conjunto de datos.

#### Validación

##### Normalidad

```{r}
#| echo: false
#| warning: false
Residuales <-rstandard(modelo3)
shapiro.test(Residuales)
library(tseries)
library(lmtest)

```

##### Gráfica de Residuos

```{r}
#| echo: false
#| warning: false
modelo3 <- lm(Price ~ Storage, data = datos)
library(car)
qqPlot(Residuales, 
       pch = 19, 
       col = "blue", 
       col.lines = "purple",
       main = "QQ-Plot de los Residuales")
```

Para definir si los residuales del **Modelo 3** siguen una distribución normal se utiliza la prueba estadística Shapiro-Wilk, donde el valor p es menor al nivel de significancia de 0.05, lo que indica que los residuales del **Modelo 3** no sigue una distribución normal. En cuanto al QQ-Plot, se puede observar la banda de confianza representada por la línea morada, y se nota que algunos de los puntos residuales no se ajustan a esta banda, lo que también confirma que los residuales del modelo no siguen una distribución normal.

##### Varianza constante

```{r}
#| echo: false
#| warning: false
bptest(modelo3)
```

##### Gráfica de Residuos

```{r}
#| echo: false
#| warning: false
datos$Valores_ajustados <- modelo3$fitted.values
plot(datos$Valores_ajustados ,Residuales,xlab= "Valores ajustados", ylab= "Residuales",
     main= "Valores ajustados vs residuales", pch = 20, col= "blue")
abline(h=0, col="purple",lwd=2)
```

Para determinar si el **Modelo 3** tiene varianza constante(homocedasticidad), se aplica la prueba estadística Breusch-Pagan, donde el valor p obtenido es **0.05652** el cual es mayor, pero muy cercano al nivel de significancia de 0.05, lo que sugiere que no se puede rechazar completamente la hipótesis nula de homocedasticidad. Al observar la gráfica de valores ajustados vs. residuales se puede decir que los residuales cumplen con el supuesto de la homocedasticidad , ya que se nota que los puntos están dispersos alrededor de la línea y = 0 y además no sé observa un patrón de embudo lo cual es una indicación de que la varianza de los residuos se mantiene constante.

##### Independencia

```{r}
#| echo: false
#| warning: false
dwtest(modelo3)
bgtest(modelo3)
```

Para determinar si los residuos del **Modelo 3** son independientes (es decir, si no presentan autocorrelación), se utilizaron las pruebas estadísticas de Durbin-Watson y Breusch-Godfrey,en la prueba de Durbin-Watson, el valor p fue **0.05877** y en la prueba de Breusch-Godfrey el valor p obtenido fue **0.1215**, ambos valores mayores al nivel de significancia de 0.05, lo cual confirma que no hay autocorrelación significativa y que los residuales son independientes.

#### Transformación

Se decide aplicar una transformación logarítmica natural a la variable respuesta Price en el Modelo 3, con el objetivo de mejorar el ajuste del modelo, así como verificar si se puede mejorar la normalidad de los residuos y lograr una varianza constante.

```{r}
#| echo: false
#| warning: false
modelo3.1 <- lm(log(Price)~ Storage, data = datos)
summary(modelo3.1)
```

##### Normalidad

```{r}
#| echo: false
#| warning: false
Residuales <-rstandard(modelo3.1)
shapiro.test(Residuales)
library(tseries)
library(lmtest)

```

##### Varianza constante

```{r}
#| echo: false
#| warning: false
bptest(modelo3.1)
```

En la transformación realizada para el **Modelo 3** se observó una leve mejora en el R\^2, aunque no fue significativa. Además, ninguna de las variables resultó ser estadísticamente significativa. En cuanto a la normalidad, no hubo ninguna mejora, lo que indica que los residuos siguen sin ajustarse a una distribución normal. Por otro lado, para la varianza constante, el valor p aumento un poco lo sugiere que hay homocedasticidad en el modelo.

### Modelo 4

```{r}
#| echo: false
#| warning: false
datos$Warranty <- as.factor(datos$Warranty)
modelo4<-lm(Price~Warranty, data = datos)
summary(modelo4)
```

En el **Modelo 4** se busca responder si existe una relación entre el precio (Price) de los portátiles y la garantía (Warranty). Para el análisis, se convierte la variable Warranty en una variable cualitativa, dividiéndola en dos categorías: **Warranty2** y **Warranty3**. Después de analizar el summary del modelo, se observa que la variable Warranty2 es significativa, ya que su valor p **0.0423** es menor que el nivel de significancia de 0.05. Esto indica que Warranty2 tiene un efecto significativo en el precio, por otro lado, la variable Warranty3 no es significativa, con un valor p 0.9300 mucho mayor que 0.05, lo que sugiere que esta categoría no tiene un impacto relevante en el precio. El R2 del modelo es 0.001681 y el R\^2 ajustado es 0.001015, lo cual es extremadamente bajo, esto sugiere que la variable Warranty apenas explica la variabilidad del Price, indicando que esta variable no es un buen predictor del precio de los portátiles.

#### Validación

##### Normalidad

```{r}
#| echo: false
#| warning: false
Residuales <-rstandard(modelo4)
shapiro.test(Residuales)
library(tseries)
library(lmtest)
```

##### Gráfica de Residuos

```{r}
#| echo: false
#| warning: false
modelo4<-lm(Price~Warranty, data = datos)
library(car)
qqPlot(Residuales, 
       pch = 19, 
       col = "blue", 
       col.lines = "purple",
       main = "QQ-Plot de los Residuales")
```

Para determinar si los residuales del **Modelo 4** siguen una distribución normal se usa el estadístico de prueba Shapiro-Wilk,los resultados de este muestran que el valorp es demasiado pequeño,lo que indica que se rechaza la hipótesis nula de normalidad y que los residuos del modelo no siguen una distribución normal.En cuanto a la gráfica de QQ-Plot, se evidencia que no todos los puntos se encuentran alineados en la banda de confianza (en color morado), este patrón confirma visualmente que los residuos no siguen una distribución normal.

##### Varianza constante

```{r}
#| echo: false
#| warning: false
bptest(modelo4)
```

##### Gráfica de Residuos

```{r}
#| echo: false
#| warning: false
datos$Valores_ajustados <- modelo4$fitted.values
plot(datos$Valores_ajustados ,Residuales,xlab= "Valores ajustados", ylab= "Residuales",
     main= "Valores ajustados vs residuales", pch = 20, col= "blue")
abline(h=0, col="purple",lwd=2)
```

Para definir si los residuales del **Modelo 4** tiene una varianza constante (homocedasticidad), se utiliza la prueba estadística Breusch-Pagan,en donde el valor p de la prueba es de **0.3904**, el cual es mayor al nivel de significancia de 0.05, lo que significa que los residuales tiene homocedasticidad. Además, en la gráfica de valores ajustados vs. residuales se observa que los puntos están distribuidos de manera uniforme alrededor de la línea y = 0 y no se detecta un patrón de embudo, lo que puede indicar que los residuos siguen una varianza constante.

##### Independencia

```{r}
#| echo: false
#| warning: false
dwtest(modelo4)
bgtest(modelo4)
```

Para evaluar si los residuos del **Modelo 4** son independientes (es decir, si no presentan autocorrelación), se utilizaron las pruebas estadísticas de Durbin-Watson y Breusch-Godfrey. En la prueba de Durbin-Watson, el valor p obtenido fue **0.05967** y en la prueba de Breusch-Godfrey el valor p fue **0.124** los cuales son mayores que el nivel de significancia de 0.05, esto indica que los residuos no presentan autocorrelación significativa, es decir, que los residuos del Modelo 4 son independientes.

#### Transformación

Se decide aplicar una transformación logarítmica natural a la variable respuesta Price en el Modelo 4, con el propósito de mejorar el ajuste del modelo y ver si esto ayuda a que los residuos se acerquen a una distribución normal, requisito necesario para validar el modelo.

```{r}
#| echo: false
#| warning: false
modelo4.1 <- lm(log(Price)~ Warranty, data = datos)
summary(modelo4.1)
```

##### Normalidad

```{r}
#| echo: false
#| warning: false
Residuales <-rstandard(modelo4.1)
shapiro.test(Residuales)
library(tseries)
library(lmtest)

```

En la transformación logarítmica del **Modelo 4** se observa que el R\^2 ajustado no mejora en comparación con el modelo sin transformar, lo cual indica que la transformación no aporta un ajuste significativo al modelo. Por lo tanto, el modelo sin transformar sigue siendo la mejor opción en términos de capacidad explicativa. Además, al evaluar la normalidad de los residuos en el modelo transformado, se nota que no hubo ninguna mejora lo que significa que los residuales siguen sin ajustarse a una distribución normal, lo cual afecta la validez del modelo.

## Influencia

Después de analizar los datos y observar las posibles influencias en los cuatro modelos considerados, se decidió no eliminar ninguna observación. Todas las observaciones se encuentran dentro del mismo intervalo, sin valores suficientemente grandes o pequeños que justifiquen su exclusión. Además, eliminar cualquier observación implicaría también eliminar otras con valores similares, lo que resultaría en una pérdida significativa de datos, incluso de aquellos que no presentan puntos de influencia. Asimismo, eliminar datos en las variables no aportaría beneficios, ya que, aunque se transformaran los modelos, estos seguirían sin ser válidos debido a la falta de normalidad.

## Resultado

Luego de analizar cuatro modelos, se concluye que el modelo con mayor significancia fue el *Modelo 4* sin transformar, el cual estudia la relación entre la variable respuesta Price y la variable regresora Warranty. En este modelo, la categoría Warranty2 resultó significativa, con un valor p de **0.0423**, lo que sugiere que tener esta categoría de garantía tiene un efecto en el precio del portátil.

Sin embargo, el R\^2 ajustado de **0.001015** sigue siendo extremadamente bajo, lo que indica que, aunque Warranty2 tiene un efecto estadísticamente significativo, la variable Warranty en su conjunto no explica adecuadamente la variabilidad del precio. Esto sugiere que el precio de los portátiles está influenciado por otros factores no incluidos en este modelo y que Warranty por sí sola no es un buen predictor del precio.

**Con respecto a las preguntas planteadas:**

1.  ¿Cómo afecta el almacenamiento de los portátiles en relación con su precio? En el **Modelo 3** se observa que la variable Storage no tiene un impacto significativo en el precio de los portátiles, ninguna de las categorías de Storage resultó significativa en el modelo, y el R\^2 ajustado es muy bajo y negativo, lo cual indica que Storage no explica la variabilidad en el precio.

2.  ¿Existe relación entre el precio de los portátiles y la garantía? Como se mencionó en el análisis del **Modelo 4**, aunque el R\^2 ajustado es positivo y una de las categorías de Warranty (Warranty2) resultó significativa, estos valores son extremadamente bajos,lo que significa que la garantía no explica bien la variabilidad en el precio.

## Conclusiones

El análisis de regresión es una herramienta estadística valiosa para identificar y establecer relaciones entre variables. Sin embargo, en este caso, los datos proporcionados no fueron suficientes para identificar las variables que pueden predecir de manera efectiva el precio de los portátiles.

Ninguno de los modelos cumplió con el supuesto de normalidad de los residuos, lo que afecta la validez de los modelos. Aunque se intentaron transformaciones logarítmicas para mejorar el ajuste y la normalidad de los residuos, los resultados no mejoraron significativamente. Esta falta de normalidad en los residuos impidió realizar una selección de variables adecuada.

En conclusión, la base de datos utilizada no fue adecuada para predecir el precio de los portátiles. Se recomienda obtener datos adicionales o variables más relevantes que puedan tener un impacto significativo en el precio. Con una base de datos más completa y variables que influyan más directamente en el precio, se podría desarrollar un modelo de predicción más preciso y representativo.

## Referencias

[@MASS], [@graphics], [@GGally], [@plotly], [@Metrics], [@car], [@lmtest], [@ggplot2], [@readr], [@tseries].
