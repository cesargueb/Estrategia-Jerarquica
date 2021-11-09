#  Importamos la base de datos requerida

##  Abrimos nuestra base de datos

```{r}
biblioteca ( rio )
lkCSV = " https://raw.githubusercontent.com/cesargueb/TrabajoGrupal/main/subdata.csv "
subdata = importar ( lkCSV )
subdata = subdata [ ! subdata $ IngTotalPrin == 999999.0000 ,]
subdata = subdata [ ! subdata $ IngLiquiPrin == 999999.0000 ,]
subdata = subdata [ ! subdata $ IngLiquiSecun == 999999.0000 ,]
```

##  Trabajamos sin valores perdidos

```{r}
subdata = na.omit ( subdata )
subdata = subdata [, - c ( 4 )]
```

##  Verificamos los tipos de datos:

```{r}
subdata $ IngTotalPrin = as.numeric ( subdata $ IngTotalPrin )
subdata $ IngLiquiPrin = as.numeric ( subdata $ IngLiquiPrin )
subdata $ IngLiquiSecun = as.numeric ( subdata $ IngLiquiSecun )
str ( subdatos )
```

#  Estrategia Jerárquica

##  Calculamos las distancias entre los casos (filas):

```{r}
biblioteca ( clúster )
g.dist = daisy ( subdata , metric  =  " gower " )
```

##  Estrategia Aglomerativa

###  1. Cúmulo calcular

```{r}
semillas ( 123 )
biblioteca ( factoextra )
res.agnes <- hcut ( g.dist , k = 4 , hc_func = ' agnes ' , hc_method = " ward.D " )
subdata $ clustAGL = res.agnes $ cluster
```

###  2. Explorar resultados

```{r}
biblioteca ( plyr )
agregado (cbind ( IngTotalPrin , IngLiquiPrin , IngLiquiSecun ) ~ clustAGL , datos = subdatos , media )
```


####  Modificamos los grupos

```{r}
subdata $ clustAGL = dplyr :: recode ( subdata $ clustAGL , `1`  =  1 , ` 2` = 3 , `3` = 2 , ` 4` = 4 )
agregado (cbind ( IngTotalPrin , IngLiquiPrin , IngLiquiSecun ) ~ clustAGL , datos = subdatos , media )
```


###  3. Visualizar

```{r}
fviz_dend ( res.agnes , cex  =  0.6 , horiz  =  T )
```

##  Estrategia Divisiva

###  1. Agrupaciones calculares

```{r}
semillas ( 123 )
res.diana <- hcut ( g.dist , k = 4 , hc_func  =  ' diana ' )
subdata $ clustDIV = res.diana $ cluster
```

###  2. Exploramos resultados

```{r}
biblioteca ( plyr )
agregado (cbind ( IngTotalPrin , IngLiquiPrin , IngLiquiSecun ) ~ clustDIV , datos = subdatos , media )
```

###  3. Recodificamos

```{r}
subdata $ clustDIV = dplyr :: recode ( subdata $ clustDIV , `1`  =  1 , ` 2` = 3 , `3` = 4 , ` 4` = 2 )
agregado (cbind ( IngTotalPrin , IngLiquiPrin , IngLiquiSecun ) ~ clustDIV , datos = subdatos , media )
```


###  4. Visualizamos

```{r}
fviz_dend ( res.diana , cex  =  0.8 , horiz  =  T )
```

##  Comparamos ambas estrategias por jerarquía

```{r}
table ( subdata $ clustDIV , subdata $ clustAGL , dnn  = c ( ' División ' , ' Aglomeración ' ))
```
