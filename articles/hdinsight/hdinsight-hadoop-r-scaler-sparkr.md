---
title: aaaUse ScaleR a SparkR s Azure HDInsight | Microsoft Docs
description: "Použít ScaleR a SparkR s R Server a HDInsight"
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: da732ff0235cf465a1452b81750c7cdd0351eed5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="combine-scaler-and-sparkr-in-hdinsight"></a>Kombinace ScaleR a SparkR v HDInsight

Tento článek ukazuje, jak toopredict letu zpoždění doručení pomocí **ScaleR** logistic regresní model z dat na zpoždění letů a počasí spojena s **SparkR**. Tento scénář předvádí možnosti hello ScaleR pro manipulaci s daty na Spark používat s Microsoft R Server pro analýzu. Hello kombinace těchto technologií umožňuje tooapply hello nejnovější funkce v distribuované zpracování.

I když oba balíčky běží na stroji provádění Spark pro Hadoop, jsou blokovat sdílení jako každý vyžadují jejich vlastní příslušné relace Spark dat v paměti. Dokud tomuto problému dochází v budoucích verzích R Server, je alternativní řešení hello toomaintain nepřekrývají Spark relace a tooexchange data prostřednictvím zprostředkující soubory. Hello pokynů tady ukazují, že jsou tyto požadavky přehledné tooachieve.

Používáme příklad zde původně sdílí v obraťte na 2016 vrstev Mario Inchiosa a Roni Burd, která je také k dispozici prostřednictvím hello webinář [vytváření škálovatelné platformy vědecké účely dat s R](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). Příklad hello používá SparkR toojoin hello dobře známé airlines příchodem zpoždění datové sady daty počasí na letištích odeslání a přijetí. data Hello připojený se pak používá jako vstupní tooa ScaleR logistic regresní model pro predikci letu příchodem zpoždění.

Hello kód jsme návod byl původně zapsán pro R Server spuštěný na Spark v clusteru služby HDInsight v Azure. Ale hello koncept kombinování hello použití SparkR a ScaleR v jednom skriptu je taky platná v kontextu hello místní prostředí. V následující hello jsme předpokládá pokročilou úroveň znalosti R a R hello [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) knihovny R Server. Zavedeme použití [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) při procházení tohoto scénáře.

## <a name="hello-airline-and-weather-datasets"></a>Hello letecká společnost a počasí datové sady

Hello **AirOnTime08to12CSV** airlines veřejné datová sada obsahuje informace o letu příchodem a odeslání podrobnosti o všech komerční letů v rámci hello USA, z října 1987 tooDecember 2012. Toto je velké datové sady: celkem jsou téměř 150 miliony záznamů. Je právě v části vybaleno 4 GB. Je k dispozici z hello [US government archivy](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236). Snadněji, je k dispozici jako soubor zip (AirOnTimeCSV.zip) obsahující sadu 303 samostatné měsíční CSV soubory z hello [úložiště Revolution Analytics datové sady](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)

toosee hello účinky počasí na zpoždění letů, potřebujeme také hello počasí dat na všech letištích hello. Tato data mohou stáhnout jako soubory zip v základním formátu měsíc z hello [National Oceánský a důsledky správy úložiště](http://www.ncdc.noaa.gov/orders/qclcd/). Pro účely tohoto příkladu hello jsme vyžádá počasí data z května 2007 – prosinec 2012 a použít hello hodinové datové soubory v každém z 68 měsíční zips hello. měsíční soubory zip Hello také obsahovat mapování (YYYYMMstation.txt) mezi hello počasí stanice ID (WBAN), hello letiště, že je spojen s (volacím) a hello na letišti posun časového pásma od času UTC (časové pásmo). Všechny tyto informace je třeba při propojení s daty zpoždění a počasí letecká společnost hello.

## <a name="setting-up-hello-spark-environment"></a>Nastavení prostředí Spark hello

prvním krokem Hello je tooset hello Spark prostředí. Začneme odkazující toohello adresář, který obsahuje naše adresáře vstupních dat, vytváření kontextu výpočtů Spark a vytvoření funkce protokolování pro konzolu toohello informační protokolování:

```
workDir        <- '~'  
myNameNode     <- 'default' 
myPort         <- 0
inputDataDir   <- 'wasb://hdfs@myAzureAcccount.blob.core.windows.net'
hdfsFS         <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

# create a persistent Spark session tooreduce startup times 
#   (remember toostop it later!)
 
sparkCC        <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort, persistentRun=TRUE)

# create working directories 

rxHadoopMakeDir('/user')
rxHadoopMakeDir('user/RevoShare')
rxHadoopMakeDir('user/RevoShare/remoteuser')

(dataDir <- '/share')
rxHadoopMakeDir(dataDir)
rxHadoopListFiles(dataDir) 

setwd(workDir)
getwd()

# version of rxRoc that runs in a local CC 
rxRoc <- function(...){
  rxSetComputeContext(RxLocalSeq())
  roc <- RevoScaleR::rxRoc(...)
  rxSetComputeContext(sparkCC)
  return(roc)
}

logmsg <- function(msg) { cat(format(Sys.time(), "%Y-%m-%d %H:%M:%S"),':',msg,'\n') } 
t0 <- proc.time() 

#..start 

logmsg('Start') 
(trackers <- system("mapred job -list-active-trackers", intern = TRUE))
logmsg(paste('Number of task nodes=',length(trackers)))
```

Další přidáme cestu pro balíčky R hledání toohello "Spark_Home" tak, že jsme použít SparkR a inicializaci SparkR relace:

```
#..setup for use of SparkR  

logmsg('Initialize SparkR') 

.libPaths(c(file.path(Sys.getenv("SPARK_HOME"), "R", "lib"), .libPaths()))
library(SparkR)

sparkEnvir <- list(spark.executor.instances = '10',
                   spark.yarn.executor.memoryOverhead = '8000')

sc <- sparkR.init(
  sparkEnvir = sparkEnvir,
  sparkPackages = "com.databricks:spark-csv_2.10:1.3.0"
)

sqlContext <- sparkRSQL.init(sc)
```

## <a name="preparing-hello-weather-data"></a>Příprava dat počasí hello

data o počasí hello tooprepare, jsme podmnožina ho toohello sloupce potřebné pro modelování: 

- "Viditelnosti"
- "DryBulbCelsius"
- "DewPointCelsius"
- "RelativeHumidity"
- "Větru"
- "Výškoměru"

Pak přidejte letiště kód spojený s hello počasí stanice a převést hello měření z místního času tooUTC.

Začneme vytvořením souboru toomap hello počasí stanice (WBAN) informace o tooan letiště kód. Tato korelace nám může získat ze souboru mapování hello součástí hello počasí data. Pomocí mapování hello *volacím* (například LAX) pole v datovém souboru počasí hello příliš*původu* v datech letecká společnost hello. Ale jsme právě došlo k toohave jiné mapování na straně mapující *WBAN* příliš*AirportID* (například 12892 pro LAX) a zahrnuje *časové pásmo* , byla uložena tooa Soubor CSV názvem "wban na letiště id-tz. CSV", které můžeme použít. Například:

| AirportID | WBAN | Časové pásmo
|-----------|------|---------
| 10685 | 54831 | -6
| 14871 | 24232 | -8
| .. | .. | ..

Následující kód čte každý počasí nezpracovaných dat po hodinách hello Hello soubory podmnožin toohello sloupce jsme potřebovat, sloučí soubor mapování počasí stanice hello, upraví hello datum čas tooUTC měření a pak zapíše na novou verzi souboru hello:

```
# Look up AirportID and Timezone for WBAN (weather station ID) and adjust time

adjustTime <- function(dataList)
{
  dataset0 <- as.data.frame(dataList)
  
  dataset1 <- base::merge(dataset0, wbanToAirIDAndTZDF1, by = "WBAN")

  if(nrow(dataset1) == 0) {
    dataset1 <- data.frame(
      Visibility = numeric(0),
      DryBulbCelsius = numeric(0),
      DewPointCelsius = numeric(0),
      RelativeHumidity = numeric(0),
      WindSpeed = numeric(0),
      Altimeter = numeric(0),
      AdjustedYear = numeric(0),
      AdjustedMonth = numeric(0),
      AdjustedDay = integer(0),
      AdjustedHour = integer(0),
      AirportID = integer(0)
    )
    
    return(dataset1)
  }
  
  Year <- as.integer(substr(dataset1$Date, 1, 4))
  Month <- as.integer(substr(dataset1$Date, 5, 6))
  Day <- as.integer(substr(dataset1$Date, 7, 8))
  
  Time <- dataset1$Time
  Hour <- ceiling(Time/100)
  
  Timezone <- as.integer(dataset1$TimeZone)
  
  adjustdate = as.POSIXlt(sprintf("%4d-%02d-%02d %02d:00:00", Year, Month, Day, Hour), tz = "UTC") + Timezone * 3600

  AdjustedYear = as.POSIXlt(adjustdate)$year + 1900
  AdjustedMonth = as.POSIXlt(adjustdate)$mon + 1
  AdjustedDay   = as.POSIXlt(adjustdate)$mday
  AdjustedHour  = as.POSIXlt(adjustdate)$hour
  
  AirportID = dataset1$AirportID
  Weather = dataset1[,c("Visibility", "DryBulbCelsius", "DewPointCelsius", "RelativeHumidity", "WindSpeed", "Altimeter")]
  
  data.set = data.frame(cbind(AdjustedYear, AdjustedMonth, AdjustedDay, AdjustedHour, AirportID, Weather))
  
  return(data.set)
}

wbanToAirIDAndTZDF <- read.csv("wban-to-airport-id-tz.csv")

colInfo <- list(
  WBAN = list(type="integer"),
  Date = list(type="character"),
  Time = list(type="integer"),
  Visibility = list(type="numeric"),
  DryBulbCelsius = list(type="numeric"),
  DewPointCelsius = list(type="numeric"),
  RelativeHumidity = list(type="numeric"),
  WindSpeed = list(type="numeric"),
  Altimeter = list(type="numeric")
)

weatherDF <- RxTextData(file.path(inputDataDir, "WeatherRaw"), colInfo = colInfo)

weatherDF1 <- RxTextData(file.path(inputDataDir, "Weather"), colInfo = colInfo,
                filesystem=hdfsFS)

rxSetComputeContext("localpar")
rxDataStep(weatherDF, outFile = weatherDF1, rowsPerRead = 50000, overwrite = T,
           transformFunc = adjustTime,
           transformObjects = list(wbanToAirIDAndTZDF1 = wbanToAirIDAndTZDF))
```

## <a name="importing-hello-airline-and-weather-data-toospark-dataframes"></a>Import hello letecká společnost a počasí data tooSpark DataFrames

Teď používáme hello SparkR [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) funkce tooimport hello počasí a letecká společnost dat tooSpark DataFrames. Tato funkce, jako je mnoho dalších metod Spark jsou spouštěny líné, což znamená, že jsou zařazené do fronty pro provádění ale nebyl proveden až požadovaných.

```
airPath     <- file.path(inputDataDir, "AirOnTime08to12CSV")
weatherPath <- file.path(inputDataDir, "Weather") # pre-processed weather data
rxHadoopListFiles(airPath) 
rxHadoopListFiles(weatherPath) 

# create a SparkR DataFrame for hello airline data

logmsg('create a SparkR DataFrame for hello airline data') 
# use inferSchema = "false" for more robust parsing
airDF <- read.df(sqlContext, airPath, source = "com.databricks.spark.csv", 
                 header = "true", inferSchema = "false")

# Create a SparkR DataFrame for hello weather data

logmsg('create a SparkR DataFrame for hello weather data') 
weatherDF <- read.df(sqlContext, weatherPath, source = "com.databricks.spark.csv", 
                     header = "true", inferSchema = "true")
```

## <a name="data-cleansing-and-transformation"></a>Vyčištění dat a transformace

Další provedeme některé vyčištění dat letecká společnost hello jsme jste naimportovali toorename sloupce. Jsme pouze zachovat hello proměnných potřebných a zaokrouhlit odeslání Naplánované časy dolů toohello nejbližší hodinu tooenable sloučení s hello nejnovější data o počasí na odeslání:

```
logmsg('clean hello airline data') 
airDF <- rename(airDF,
                ArrDel15 = airDF$ARR_DEL15,
                Year = airDF$YEAR,
                Month = airDF$MONTH,
                DayofMonth = airDF$DAY_OF_MONTH,
                DayOfWeek = airDF$DAY_OF_WEEK,
                Carrier = airDF$UNIQUE_CARRIER,
                OriginAirportID = airDF$ORIGIN_AIRPORT_ID,
                DestAirportID = airDF$DEST_AIRPORT_ID,
                CRSDepTime = airDF$CRS_DEP_TIME,
                CRSArrTime =  airDF$CRS_ARR_TIME
)

# Select desired columns from hello flight data. 
varsToKeep <- c("ArrDel15", "Year", "Month", "DayofMonth", "DayOfWeek", "Carrier", "OriginAirportID", "DestAirportID", "CRSDepTime", "CRSArrTime")
airDF <- select(airDF, varsToKeep)

# Apply schema
coltypes(airDF) <- c("character", "integer", "integer", "integer", "integer", "character", "integer", "integer", "integer", "integer")

# Round down scheduled departure time toofull hour.
airDF$CRSDepTime <- floor(airDF$CRSDepTime / 100)
```

Nyní jsme provádět podobné operace s daty počasí hello:

```
# Average weather readings by hour
logmsg('clean hello weather data') 
weatherDF <- agg(groupBy(weatherDF, "AdjustedYear", "AdjustedMonth", "AdjustedDay", "AdjustedHour", "AirportID"), Visibility="avg",
                  DryBulbCelsius="avg", DewPointCelsius="avg", RelativeHumidity="avg", WindSpeed="avg", Altimeter="avg"
                  )

weatherDF <- rename(weatherDF,
                    Visibility = weatherDF$'avg(Visibility)',
                    DryBulbCelsius = weatherDF$'avg(DryBulbCelsius)',
                    DewPointCelsius = weatherDF$'avg(DewPointCelsius)',
                    RelativeHumidity = weatherDF$'avg(RelativeHumidity)',
                    WindSpeed = weatherDF$'avg(WindSpeed)',
                    Altimeter = weatherDF$'avg(Altimeter)'
)
```

## <a name="joining-hello-weather-and-airline-data"></a>Spojování dat počasí a letecká společnost hello

Teď používáme hello SparkR [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) funkce toodo levé vnější spojení hello letecká společnost a počasí dat odeslání AirportID a data a času. vnější spojení Hello umožňuje tooretain všechna data letecká společnost hello zaznamenává i v případě, že neexistuje žádná odpovídající počasí data. Následující spojení hello jsme odebrat některé redundantní sloupce a přejmenujte hello zachovány sloupce tooremove hello příchozí DataFrame předponu zaváděné hello spojení.

```
logmsg('Join airline data with weather at Origin Airport')
joinedDF <- SparkR::join(
  airDF,
  weatherDF,
  airDF$OriginAirportID == weatherDF$AirportID &
    airDF$Year == weatherDF$AdjustedYear &
    airDF$Month == weatherDF$AdjustedMonth &
    airDF$DayofMonth == weatherDF$AdjustedDay &
    airDF$CRSDepTime == weatherDF$AdjustedHour,
  joinType = "left_outer"
)

# Remove redundant columns
vars <- names(joinedDF)
varsToDrop <- c('AdjustedYear', 'AdjustedMonth', 'AdjustedDay', 'AdjustedHour', 'AirportID')
varsToKeep <- vars[!(vars %in% varsToDrop)]
joinedDF1 <- select(joinedDF, varsToKeep)

joinedDF2 <- rename(joinedDF1,
                    VisibilityOrigin = joinedDF1$Visibility,
                    DryBulbCelsiusOrigin = joinedDF1$DryBulbCelsius,
                    DewPointCelsiusOrigin = joinedDF1$DewPointCelsius,
                    RelativeHumidityOrigin = joinedDF1$RelativeHumidity,
                    WindSpeedOrigin = joinedDF1$WindSpeed,
                    AltimeterOrigin = joinedDF1$Altimeter
)
```

Podobným způsobem jsme spojení hello počasí a letecká společnost dat na základě příchodem AirportID a data a času:

```
logmsg('Join airline data with weather at Destination Airport')
joinedDF3 <- SparkR::join(
  joinedDF2,
  weatherDF,
  airDF$DestAirportID == weatherDF$AirportID &
    airDF$Year == weatherDF$AdjustedYear &
    airDF$Month == weatherDF$AdjustedMonth &
    airDF$DayofMonth == weatherDF$AdjustedDay &
    airDF$CRSDepTime == weatherDF$AdjustedHour,
  joinType = "left_outer"
)

# Remove redundant columns
vars <- names(joinedDF3)
varsToDrop <- c('AdjustedYear', 'AdjustedMonth', 'AdjustedDay', 'AdjustedHour', 'AirportID')
varsToKeep <- vars[!(vars %in% varsToDrop)]
joinedDF4 <- select(joinedDF3, varsToKeep)

joinedDF5 <- rename(joinedDF4,
                    VisibilityDest = joinedDF4$Visibility,
                    DryBulbCelsiusDest = joinedDF4$DryBulbCelsius,
                    DewPointCelsiusDest = joinedDF4$DewPointCelsius,
                    RelativeHumidityDest = joinedDF4$RelativeHumidity,
                    WindSpeedDest = joinedDF4$WindSpeed,
                    AltimeterDest = joinedDF4$Altimeter
                    )
```

## <a name="save-results-toocsv-for-exchange-with-scaler"></a>Uložte výsledky tooCSV pro exchange s ScaleR

Tím končí potřebujeme toodo s SparkR spojení hello. Nemůžeme uložit hello data z hello konečné Spark DataFrame "joinedDF5" tooa sdíleného svazku clusteru pro vstupní tooScaleR a zavřete out hello SparkR relace. Nemůžeme říct explicitně SparkR toosave hello výsledné sdíleného svazku clusteru v 80 samostatné oddíly tooenable dostatečná paralelismus v ScaleR zpracování:

```
logmsg('output hello joined data from Spark tooCSV') 
joinedDF5 <- repartition(joinedDF5, 80) # write.df below will produce this many CSVs

# write result toodirectory of CSVs
write.df(joinedDF5, file.path(dataDir, "joined5Csv"), "com.databricks.spark.csv", "overwrite", header = "true")

# We can shut down hello SparkR Spark context now
sparkR.stop()

# remove non-data files
rxHadoopRemove(file.path(dataDir, "joined5Csv/_SUCCESS"))
```

## <a name="import-tooxdf-for-use-by-scaler"></a>Import tooXDF za účelem použití ScaleR

Můžeme použít soubor CSV hello připojené k letecká společnost a počasí dat jako-je pro modelování prostřednictvím zdroj dat ScaleR text. Ale můžeme ho importovat tooXDF nejprve vzhledem k tomu, že je efektivnější při spuštění více operací pro datovou sadu hello:

```
logmsg('Import hello CSV toocompressed, binary XDF format') 

# set hello Spark compute context for R Server 
rxSetComputeContext(sparkCC)
rxGetComputeContext()

colInfo <- list(
  ArrDel15 = list(type="numeric"),
  Year = list(type="factor"),
  Month = list(type="factor"),
  DayofMonth = list(type="factor"),
  DayOfWeek = list(type="factor"),
  Carrier = list(type="factor"),
  OriginAirportID = list(type="factor"),
  DestAirportID = list(type="factor"),
  RelativeHumidityOrigin = list(type="numeric"),
  AltimeterOrigin = list(type="numeric"),
  DryBulbCelsiusOrigin = list(type="numeric"),
  WindSpeedOrigin = list(type="numeric"),
  VisibilityOrigin = list(type="numeric"),
  DewPointCelsiusOrigin = list(type="numeric"),
  RelativeHumidityDest = list(type="numeric"),
  AltimeterDest = list(type="numeric"),
  DryBulbCelsiusDest = list(type="numeric"),
  WindSpeedDest = list(type="numeric"),
  VisibilityDest = list(type="numeric"),
  DewPointCelsiusDest = list(type="numeric")
)

joinedDF5Txt <- RxTextData(file.path(dataDir, "joined5Csv"),
                           colInfo = colInfo, fileSystem = hdfsFS)
rxGetInfo(joinedDF5Txt) 

destData <- RxXdfData(file.path(dataDir, "joined5XDF"), fileSystem = hdfsFS)

rxImport(inData = joinedDF5Txt, destData, overwrite = TRUE)

rxGetInfo(destData, getVarInfo = T)

# File name: /user/RevoShare/dev/delayDataLarge/joined5XDF 
# Number of composite data files: 80 
# Number of observations: 148619655 
# Number of variables: 22 
# Number of blocks: 320 
# Compression type: zlib 
# Variable information: 
#   Var 1: ArrDel15, Type: numeric, Low/High: (0.0000, 1.0000)
# Var 2: Year
# 26 factor levels: 1987 1988 1989 1990 1991 ... 2008 2009 2010 2011 2012
# Var 3: Month
# 12 factor levels: 10 11 12 1 2 ... 5 6 7 8 9
# Var 4: DayofMonth
# 31 factor levels: 1 3 4 5 7 ... 29 30 2 18 31
# Var 5: DayOfWeek
# 7 factor levels: 4 6 7 1 3 2 5
# Var 6: Carrier
# 30 factor levels: PI UA US AA DL ... HA F9 YV 9E VX
# Var 7: OriginAirportID
# 374 factor levels: 15249 12264 11042 15412 13930 ... 13341 10559 14314 11711 10558
# Var 8: DestAirportID
# 378 factor levels: 13303 14492 10721 11057 13198 ... 14802 11711 11931 12899 10559
# Var 9: CRSDepTime, Type: integer, Low/High: (0, 24)
# Var 10: CRSArrTime, Type: integer, Low/High: (0, 2400)
# Var 11: RelativeHumidityOrigin, Type: numeric, Low/High: (0.0000, 100.0000)
# Var 12: AltimeterOrigin, Type: numeric, Low/High: (28.1700, 31.1600)
# Var 13: DryBulbCelsiusOrigin, Type: numeric, Low/High: (-46.1000, 47.8000)
# Var 14: WindSpeedOrigin, Type: numeric, Low/High: (0.0000, 81.0000)
# Var 15: VisibilityOrigin, Type: numeric, Low/High: (0.0000, 90.0000)
# Var 16: DewPointCelsiusOrigin, Type: numeric, Low/High: (-41.7000, 29.0000)
# Var 17: RelativeHumidityDest, Type: numeric, Low/High: (0.0000, 100.0000)
# Var 18: AltimeterDest, Type: numeric, Low/High: (28.1700, 31.1600)
# Var 19: DryBulbCelsiusDest, Type: numeric, Low/High: (-46.1000, 53.9000)
# Var 20: WindSpeedDest, Type: numeric, Low/High: (0.0000, 136.0000)
# Var 21: VisibilityDest, Type: numeric, Low/High: (0.0000, 88.0000)
# Var 22: DewPointCelsiusDest, Type: numeric, Low/High: (-43.0000, 29.0000)

finalData <- RxXdfData(file.path(dataDir, "joined5XDF"), fileSystem = hdfsFS)

```

## <a name="splitting-data-for-training-and-test"></a>Rozdělení dat pro trénování a testování

Můžeme použít rxDataStep toosplit hello 2012 dat pro testování a zachovat hello rest pro školení:

```
# split out hello training data

logmsg('split out training data as all data except year 2012')
trainDS <- RxXdfData( file.path(dataDir, "finalDataTrain" ),fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = trainDS,
            rowSelection = ( Year != 2012 ), overwrite = T )

# split out hello testing data

logmsg('split out hello test data for year 2012') 
testDS <- RxXdfData( file.path(dataDir, "finalDataTest" ), fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = testDS,
            rowSelection = ( Year == 2012 ), overwrite = T )

rxGetInfo(trainDS)
rxGetInfo(testDS)
```

## <a name="train-and-test-a-logistic-regression-model"></a>Trénování a testování logistic regresní model

Nyní jsme připravené toobuild modelu. toosee hello vlivu data o počasí na zpoždění při čas doručení hello, použijeme je ScaleR logistic regression rutiny. Můžeme použít ho toomodel zda prodlevu příchodem delší než 15 minut je ovlivněno hello počasí na letištích odeslání a přijetí hello:

```
logmsg('train a logistic regression model for Arrival Delay > 15 minutes') 
formula <- as.formula(ArrDel15 ~ Year + Month + DayofMonth + DayOfWeek + Carrier +
                     OriginAirportID + DestAirportID + CRSDepTime + CRSArrTime + 
                     RelativeHumidityOrigin + AltimeterOrigin + DryBulbCelsiusOrigin +
                     WindSpeedOrigin + VisibilityOrigin + DewPointCelsiusOrigin + 
                     RelativeHumidityDest + AltimeterDest + DryBulbCelsiusDest +
                     WindSpeedDest + VisibilityDest + DewPointCelsiusDest
                   )

# Use hello scalable rxLogit() function but set max iterations too3 for hello purposes of 
# this exercise 

logitModel <- rxLogit(formula, data = trainDS, maxIterations = 3)

base::summary(logitModel)
```

Nyní si ukážeme, jak ho na hello testovacích dat tím, že některé předpovědi a prohlížení ROC a AUC.

```
# Predict over test data (Logistic Regression).

logmsg('predict over hello test data') 
logitPredict <- RxXdfData(file.path(dataDir, "logitPredict"), fileSystem = hdfsFS)

# Use hello scalable rxPredict() function

rxPredict(logitModel, data = testDS, outData = logitPredict,
          extraVarsToWrite = c("ArrDel15"), 
          type = 'response', overwrite = TRUE)

# Calculate ROC and Area Under hello Curve (AUC).

logmsg('calculate hello roc and auc') 
logitRoc <- rxRoc("ArrDel15", "ArrDel15_Pred", logitPredict)
logitAuc <- rxAuc(logitRoc)
head(logitAuc)
logitAuc

plot(logitRoc)
```

## <a name="scoring-elsewhere"></a>Vyhodnocování jinde

Také můžeme použít hello model vyhodnocování dat na jiné platformě. Uložení souboru tooan vzdálené plochy a přenosu a importu této vzdálené plochy do cílového vyhodnocování prostředí, jako jsou R služby serveru SQL Server. Je důležité tooensure, který hello Multi-Factor úrovně toobe data hello skóre pro magnitudu shodují s těmi, na které hello byl vytvořený model. Že shoda lze dosáhnout extrahování a ukládá informace o sloupci hello přidružené hello modelování dat přes na ScaleR `rxCreateColInfo()` funkce a potom se použijí tento sloupec informace toohello vstupní zdroj dat pro předpověď. V následující hello jsme uložit několik řádků hello testovací datové sady a extrahovat a použít informace o sloupci hello od této ukázky ve skriptu předpovědi hello:

```
# save hello model and a sample of hello test dataset 

logmsg('save serialized version of hello model and a sample of hello test data')
rxSetComputeContext('localpar') 
saveRDS(logitModel, file = "logitModel.rds")
testDF <- head(testDS, 1000)  
saveRDS(testDF    , file = "testDF.rds"    )
list.files()

rxHadoopListFiles(file.path(inputDataDir,''))
rxHadoopListFiles(dataDir)

# stop hello spark engine 
rxStopEngine(sparkCC) 

logmsg('Done.')
elapsed <- (proc.time() - t0)[3]
logmsg(paste('Elapsed time=',sprintf('%6.2f',elapsed),'(sec)\n\n'))
```

## <a name="summary"></a>Souhrn

V tomto článku jsme jste ukazuje, jak je možné toocombine použití SparkR pro manipulaci s daty s ScaleR pro vývoj modelu v Hadoop, Spark. Tento scénář vyžaduje udržovat samostatných relací Spark pouze jednu relaci spuštěný v čase a vyměňovat data prostřednictvím souborů CSV. I když je jasné, tento proces by měl být snazší i v budoucích vydání R Server, SparkR a ScaleR můžete sdílet relaci Spark a sdílet tak Spark DataFrames.

## <a name="next-steps-and-more-information"></a>Další informace a další kroky

- Další informace o použití serveru R na Spark, najdete v části hello [Průvodce Začínáme na webu MSDN](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)

- Obecné informace o R Server najdete v tématu hello [začít pracovat s R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) článku.

- Informace o R serverem v HDInsight, naleznete v části [R serverem v Azure HDInsight přehled](hdinsight-hadoop-r-server-overview.md) a [R serverem v Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).

Další informace o použití SparkR najdete v tématu:

- [Apache SparkR dokumentu](https://spark.apache.org/docs/2.1.0/sparkr.html)

- [Přehled SparkR](https://docs.databricks.com/spark/latest/sparkr/overview.html) z Databricks
