# Ejercicio explorativo con datos hidrológicos #

Para la elaboración de la practica realizada el día lunes 17 de mayo, se utilizó un archivo de nombre **FDC** el cual estaba en formato **.csc** y poseía en su interior datos referentes a mediciones sobre el caudal de dos ríos de nombre **Estrella** y **Banano**. 
> inp <- read.csv("FDC.csv", na.strings="") #_Asignación de **inp** para traer al documento **FDC.csv** ademas de utilizar **na.strings** para rellenar valores faltantes en los datos_.#

Luego se utiliza la siguiente función para ver si aún hay casos incompletos:
>inp[!complete.cases(inp),] 

Para una graficación rapida del volumen de agua que recorre los caudales entre el tiempo, se utiliza la acción siguiente:
>plot(inp[,2], type = "l",
     col="blue",
     main = "Volumen del caudal atravez del tiempo en los ríos Estrella y Banano",
     xlab = "Tiempo",
     ylab = "Volumen de agua",)
     
>lines(inp[,3], col="green") 

Si queremos ver una información comparativa entre los caudales de ambos ríos se utiliza la acción **summary** de modo que:
>summary(inp[,2:3]) #_Lo cual nos despliega en la consola información del maximo y minimo así como los quartares de cada flujo de agua_.#

Si queremos visualizar los datos de manera separada utilizamos la función **hist(inp[,2])** o **hist(inp[,3])** respectivamente de los ríos Estrella y Banano:
> hist(inp[,2]) #_Para río Estrella_#

> hist(inp[,2]) #_Para río Banano_#

Lo siguiente que se realizará es cambiar los nombres de las colunmas para una mejor comprención y facilitar la escritura:
>names(inp) <-  c("fecha", "Estrella", "Banano")  

> plot(Estrella, main = "Graficación rápida del rio Estrella", xlab = "Tiempo") #_visualizacion basica del rio estrella_#

>attach(inp)

## Creación de **MAQ** y **MMQ** 

Para la creacción de **MAQ** y **MMQ** se debe configurar el formato de las fechas esto con:
> Tempdate <- strtime(inp[,1], format = "%d/%m/%Y")

Una vez hecho lo anterior se puede proseguir a realizar el **MAQ** de la siguiente manera y para los dos caudales:
> MMQ_Estrella <- tapply(Estrella, format(Tempdate, format = "%m"),FUN=sum)  
> MMQ_Banano <- tapply(Banano, format(Tempdate, format = "%m"),FUN=sum)  

> write.csv(rbind(MAQ_Estrella,MAQ_Banano), file= "MAQ.csv") #_Esta ultima acción es para crear un archivo **.csv** con el caudal anual de ambos rios atravez de los años_#

Para crear una representación grafica de estos sobre del volumen de agua entre el tiempo, se debe realizar de la siguiente manera:
> plot(MAQ_Banano, ylim =c(100,3000) ) #_los puntos negros representan los maximos de cada año para el río Banano_#  
> lines(MAQ_Estrella, col=2) #_La linea roja representa al rio Estrella_#

## Analisis de correlación ##

En este apartado final, se observará la correlación que se presentan en las dos cuencas del río Banano y Estrella.
> corinp <- cor(inp[,2:3], method = "spearman")  #_Para calcular el cooeficiente relación de las dos cuencas generando  una matrix. Se debe poner en la consola *corinp*._#  

>plot(Estrella, Banano, main= "Correlación rio Estrella y Banano") #_Generación grafico de correlación Estrella contra Banano_#  

Para finalizar, se hará un modelo de regresión lineal:
> inp.lm <- lm(inp[,2] ~ inp[,3], data= inp  
> summary(inp.lm) #_El resultado se presenta en la consola_#

