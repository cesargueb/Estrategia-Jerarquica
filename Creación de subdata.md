# TrabajoGrupal
Debido a que la base principal es muy extensa, aquí los comandos para poder exportar los datos que nos interesan y crear, de esta manera, una subdata.

## Importamos la data:

library(rio)
data=import("Base_Enaho.csv")

## Se crea una subdata con las variables que nos interesan

subdata=subset(data,select = c("P524A1","P524E1","P538E1","ID"))
rownames(subdata)=subdata$ID

## Renombramos nuestras variables

names(subdata)= c("IngTotalPrin","IngLiquiPrin","IngLiquiSecun","ID")

## Exportamos la subdata

export(subdata, "subdata.csv") 

