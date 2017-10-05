---
title: "Použijte ScaleR a SparkR s Azure HDInsight | Microsoft Docs"
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
ms.openlocfilehash: 29733f6f6b725dd4735219ed221431805558a5e2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="combine-scaler-and-sparkr-in-hdinsight"></a><span data-ttu-id="a2017-103">Kombinace ScaleR a SparkR v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2017-103">Combine ScaleR and SparkR in HDInsight</span></span>

<span data-ttu-id="a2017-104">Tento článek ukazuje, jak k předvídání příchodem zpoždění letů pomocí **ScaleR** logistic regresní model z dat na zpoždění letů a počasí spojena s **SparkR**.</span><span class="sxs-lookup"><span data-stu-id="a2017-104">This article shows how to predict flight arrival delays using a **ScaleR** logistic regression model from data on flight delays and weather joined with **SparkR**.</span></span> <span data-ttu-id="a2017-105">Tento scénář předvádí možnosti ScaleR pro manipulaci s daty na Spark používat s Microsoft R Server pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="a2017-105">This scenario demonstrates the capabilities of ScaleR for data manipulation on Spark used with Microsoft R Server for analytics.</span></span> <span data-ttu-id="a2017-106">Kombinace těchto technologií umožňuje použít nejnovější funkce v distribuované zpracování.</span><span class="sxs-lookup"><span data-stu-id="a2017-106">The combination of these technologies enables you to apply the latest capabilities in distributed processing.</span></span>

<span data-ttu-id="a2017-107">I když oba balíčky běží na stroji provádění Spark pro Hadoop, jsou blokovat sdílení jako každý vyžadují jejich vlastní příslušné relace Spark dat v paměti.</span><span class="sxs-lookup"><span data-stu-id="a2017-107">Although both packages run on Hadoop’s Spark execution engine, they are blocked from in-memory data sharing as they each require their own respective Spark sessions.</span></span> <span data-ttu-id="a2017-108">Dokud se tento problém řešit v budoucích verzích R Server, je alternativní řešení údržbu nepřekrývají relací Spark a vyměňovat data prostřednictvím zprostředkující soubory.</span><span class="sxs-lookup"><span data-stu-id="a2017-108">Until this issue is addressed in an upcoming version of R Server, the workaround is to maintain non-overlapping Spark sessions, and to exchange data through intermediate files.</span></span> <span data-ttu-id="a2017-109">Podle pokynů tady ukazují, že tyto požadavky jsou jednoduché k dosažení.</span><span class="sxs-lookup"><span data-stu-id="a2017-109">The instructions here show that these requirements are straightforward to achieve.</span></span>

<span data-ttu-id="a2017-110">Používáme příklad zde původně sdílí v obraťte na 2016 vrstev Mario Inchiosa a Roni Burd, která je také k dispozici prostřednictvím na webinář [vytváření škálovatelné platformy vědecké účely dat s R](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). Příklad používá SparkR připojit dobře známé airlines příchodem zpoždění datovou sadu s daty o počasí na letištích odeslání a přijetí.</span><span class="sxs-lookup"><span data-stu-id="a2017-110">We use an example here initially shared in a talk at Strata 2016 by Mario Inchiosa and Roni Burd that is also available through the webinar [Building a Scalable Data Science Platform with R](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). The example uses SparkR to join the well-known airlines arrival delay data set with weather data at departure and arrival airports.</span></span> <span data-ttu-id="a2017-111">Data připojené k se pak používá jako vstup pro ScaleR logistic regresní model pro predikci letu příchodem zpoždění.</span><span class="sxs-lookup"><span data-stu-id="a2017-111">The data joined is then used as input to a ScaleR logistic regression model for predicting flight arrival delay.</span></span>

<span data-ttu-id="a2017-112">Kód jsme návod byl původně zapsán pro R Server systémem Spark v clusteru služby HDInsight v Azure.</span><span class="sxs-lookup"><span data-stu-id="a2017-112">The code we walkthrough was originally written for R Server running on Spark in an HDInsight cluster on Azure.</span></span> <span data-ttu-id="a2017-113">Ale koncept kombinování použití SparkR a ScaleR v jednom skriptu je taky platná v kontextu místního prostředí.</span><span class="sxs-lookup"><span data-stu-id="a2017-113">But the concept of mixing the use of SparkR and ScaleR in one script is also valid in the context of on-premises environments.</span></span> <span data-ttu-id="a2017-114">V následujícím příkladu jsme předpokládá pokročilou úroveň znalosti R a jsou [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) knihovny R Server.</span><span class="sxs-lookup"><span data-stu-id="a2017-114">In the following, we presume an intermediate level of knowledge of R and R the [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) library of R Server.</span></span> <span data-ttu-id="a2017-115">Zavedeme použití [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) při procházení tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="a2017-115">We also introduce use of [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) while walking through this scenario.</span></span>

## <a name="the-airline-and-weather-datasets"></a><span data-ttu-id="a2017-116">Datové sady letecká společnost a počasí</span><span class="sxs-lookup"><span data-stu-id="a2017-116">The airline and weather datasets</span></span>

<span data-ttu-id="a2017-117">**AirOnTime08to12CSV** airlines veřejné datové sady obsahuje informace o letu příchodem a odeslání podrobnosti pro všechny obchodní lety v USA, z října 1987 prosince 2012.</span><span class="sxs-lookup"><span data-stu-id="a2017-117">The **AirOnTime08to12CSV** airlines public dataset contains information on flight arrival and departure details for all commercial flights within the USA, from October 1987 to December 2012.</span></span> <span data-ttu-id="a2017-118">Toto je velké datové sady: celkem jsou téměř 150 miliony záznamů.</span><span class="sxs-lookup"><span data-stu-id="a2017-118">This is a large dataset: there are nearly 150 million records in total.</span></span> <span data-ttu-id="a2017-119">Je právě v části vybaleno 4 GB.</span><span class="sxs-lookup"><span data-stu-id="a2017-119">It is just under 4 GB unpacked.</span></span> <span data-ttu-id="a2017-120">Je k dispozici z [US government archivy](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236).</span><span class="sxs-lookup"><span data-stu-id="a2017-120">It is available from the [U.S. government archives](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236).</span></span> <span data-ttu-id="a2017-121">Snadněji, je k dispozici jako soubor zip (AirOnTimeCSV.zip) obsahující sadu 303 samostatné měsíční CSV soubory z [úložiště Revolution Analytics datové sady](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)</span><span class="sxs-lookup"><span data-stu-id="a2017-121">More conveniently, it is available as a zip file (AirOnTimeCSV.zip) containing a set of 303 separate monthly CSV files from the [Revolution Analytics dataset repository](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)</span></span>

<span data-ttu-id="a2017-122">Důsledky počasí na zpoždění letů najdete také třeba údaje o počasí na všech letištích.</span><span class="sxs-lookup"><span data-stu-id="a2017-122">To see the effects of weather on flight delays, we also need the weather data at each of the airports.</span></span> <span data-ttu-id="a2017-123">Tato data lze stáhnout jako soubory zip v základním formátu po měsících, z [National Oceánský a důsledky správy úložiště](http://www.ncdc.noaa.gov/orders/qclcd/).</span><span class="sxs-lookup"><span data-stu-id="a2017-123">This data can be downloaded as zip files in raw form, by month, from the [National Oceanic and Atmospheric Administration repository](http://www.ncdc.noaa.gov/orders/qclcd/).</span></span> <span data-ttu-id="a2017-124">Pro účely tohoto příkladu jsme vyžádá počasí data z května 2007 – prosinec 2012 a použít v každém z 68 měsíční zips hodinové datové soubory.</span><span class="sxs-lookup"><span data-stu-id="a2017-124">For the purposes of this example, we pull weather data from May 2007 – December 2012 and used the hourly data files within each of the 68 monthly zips.</span></span> <span data-ttu-id="a2017-125">Měsíční soubory zip také obsahují mapování (YYYYMMstation.txt) mezi stanice počasí ID (WBAN), letiště, že je přidružen (volacím) a časové pásmo letišti posun od času UTC (časové pásmo).</span><span class="sxs-lookup"><span data-stu-id="a2017-125">The monthly zip files also contain a mapping (YYYYMMstation.txt) between the weather station ID (WBAN), the airport that it is associated with (CallSign), and the airport’s time zone offset from UTC (TimeZone).</span></span> <span data-ttu-id="a2017-126">Všechny tyto informace je třeba při propojení s daty zpoždění a počasí letecká společnost.</span><span class="sxs-lookup"><span data-stu-id="a2017-126">All of this information is needed when joining with the airline delay and weather data.</span></span>

## <a name="setting-up-the-spark-environment"></a><span data-ttu-id="a2017-127">Nastavení prostředí Spark</span><span class="sxs-lookup"><span data-stu-id="a2017-127">Setting up the Spark environment</span></span>

<span data-ttu-id="a2017-128">Prvním krokem je nastavení prostředí Spark.</span><span class="sxs-lookup"><span data-stu-id="a2017-128">The first step is to set up the Spark environment.</span></span> <span data-ttu-id="a2017-129">Začneme odkazující na adresář, který obsahuje naše adresáře vstupních dat, vytváření kontextu výpočtů Spark a vytvoření funkce protokolování pro informační protokolování do konzoly:</span><span class="sxs-lookup"><span data-stu-id="a2017-129">We begin by pointing to the directory that contains our input data directories, creating a Spark compute context, and creating a logging function for informational logging to the console:</span></span>

```
workDir        <- '~'  
myNameNode     <- 'default' 
myPort         <- 0
inputDataDir   <- 'wasb://hdfs@myAzureAcccount.blob.core.windows.net'
hdfsFS         <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

# create a persistent Spark session to reduce startup times 
#   (remember to stop it later!)
 
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

<span data-ttu-id="a2017-130">Další přidáme "Spark_Home" pro cestu pro balíčky R hledání tak, že jsme použít SparkR a inicializaci SparkR relace:</span><span class="sxs-lookup"><span data-stu-id="a2017-130">Next we add “Spark_Home” to the search path for R packages so that we can use SparkR, and initialize a SparkR session:</span></span>

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

## <a name="preparing-the-weather-data"></a><span data-ttu-id="a2017-131">Příprava dat počasí</span><span class="sxs-lookup"><span data-stu-id="a2017-131">Preparing the weather data</span></span>

<span data-ttu-id="a2017-132">Příprava dat počasí, jsme podmnožina ho na sloupce potřebné pro modelování:</span><span class="sxs-lookup"><span data-stu-id="a2017-132">To prepare the weather data, we subset it to the columns needed for modeling:</span></span> 

- <span data-ttu-id="a2017-133">"Viditelnosti"</span><span class="sxs-lookup"><span data-stu-id="a2017-133">"Visibility"</span></span>
- <span data-ttu-id="a2017-134">"DryBulbCelsius"</span><span class="sxs-lookup"><span data-stu-id="a2017-134">"DryBulbCelsius"</span></span>
- <span data-ttu-id="a2017-135">"DewPointCelsius"</span><span class="sxs-lookup"><span data-stu-id="a2017-135">"DewPointCelsius"</span></span>
- <span data-ttu-id="a2017-136">"RelativeHumidity"</span><span class="sxs-lookup"><span data-stu-id="a2017-136">"RelativeHumidity"</span></span>
- <span data-ttu-id="a2017-137">"Větru"</span><span class="sxs-lookup"><span data-stu-id="a2017-137">"WindSpeed"</span></span>
- <span data-ttu-id="a2017-138">"Výškoměru"</span><span class="sxs-lookup"><span data-stu-id="a2017-138">"Altimeter"</span></span>

<span data-ttu-id="a2017-139">Pak přidejte letiště kód spojený s stanice počasí a převést měření z místního času na čas UTC.</span><span class="sxs-lookup"><span data-stu-id="a2017-139">Then we add an airport code associated with the weather station and convert the measurements from local time to UTC.</span></span>

<span data-ttu-id="a2017-140">Začneme vytvořením souboru mapovat informace o počasí stanice (WBAN) na letišti kód.</span><span class="sxs-lookup"><span data-stu-id="a2017-140">We begin by creating a file to map the weather station (WBAN) info to an airport code.</span></span> <span data-ttu-id="a2017-141">Tato korelace jsme může získat ze souboru mapování součástí počasí data.</span><span class="sxs-lookup"><span data-stu-id="a2017-141">We could get this correlation from the mapping file included with the weather data.</span></span> <span data-ttu-id="a2017-142">Pomocí mapování *volacím* (například LAX) pole v datovém souboru počasí na *původu* v datech letecká společnost.</span><span class="sxs-lookup"><span data-stu-id="a2017-142">By mapping the *CallSign* (for example, LAX) field in the weather data file to *Origin* in the airline data.</span></span> <span data-ttu-id="a2017-143">Ale jsme právě došlo mít jiné mapování na straně mapující *WBAN* k *AirportID* (například 12892 pro LAX) a zahrnuje *časové pásmo* které byly uloženy do souboru CSV s názvem "wban na letiště id-tz. CSV", které můžeme použít.</span><span class="sxs-lookup"><span data-stu-id="a2017-143">However, we just happened to have another mapping on hand that maps *WBAN* to *AirportID* (for example, 12892 for LAX) and includes *TimeZone* that has been saved to a CSV file called “wban-to-airport-id-tz.CSV” that we can use.</span></span> <span data-ttu-id="a2017-144">Například:</span><span class="sxs-lookup"><span data-stu-id="a2017-144">For example:</span></span>

| <span data-ttu-id="a2017-145">AirportID</span><span class="sxs-lookup"><span data-stu-id="a2017-145">AirportID</span></span> | <span data-ttu-id="a2017-146">WBAN</span><span class="sxs-lookup"><span data-stu-id="a2017-146">WBAN</span></span> | <span data-ttu-id="a2017-147">Časové pásmo</span><span class="sxs-lookup"><span data-stu-id="a2017-147">TimeZone</span></span>
|-----------|------|---------
| <span data-ttu-id="a2017-148">10685</span><span class="sxs-lookup"><span data-stu-id="a2017-148">10685</span></span> | <span data-ttu-id="a2017-149">54831</span><span class="sxs-lookup"><span data-stu-id="a2017-149">54831</span></span> | <span data-ttu-id="a2017-150">-6</span><span class="sxs-lookup"><span data-stu-id="a2017-150">-6</span></span>
| <span data-ttu-id="a2017-151">14871</span><span class="sxs-lookup"><span data-stu-id="a2017-151">14871</span></span> | <span data-ttu-id="a2017-152">24232</span><span class="sxs-lookup"><span data-stu-id="a2017-152">24232</span></span> | <span data-ttu-id="a2017-153">-8</span><span class="sxs-lookup"><span data-stu-id="a2017-153">-8</span></span>
| <span data-ttu-id="a2017-154">..</span><span class="sxs-lookup"><span data-stu-id="a2017-154">..</span></span> | <span data-ttu-id="a2017-155">..</span><span class="sxs-lookup"><span data-stu-id="a2017-155">..</span></span> | <span data-ttu-id="a2017-156">..</span><span class="sxs-lookup"><span data-stu-id="a2017-156">..</span></span>

<span data-ttu-id="a2017-157">Následující kód načte všechny hodinové datové soubory nezpracované počasí, podmnožin ke sloupcům budeme potřebovat, sloučí soubor mapování počasí stanice, upraví časy data měření na čas UTC a zapíše na novou verzi souboru:</span><span class="sxs-lookup"><span data-stu-id="a2017-157">The following code reads each of the hourly raw weather data files, subsets to the columns we need, merges the weather station mapping file, adjusts the date times of measurements to UTC, and then writes out a new version of the file:</span></span>

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

## <a name="importing-the-airline-and-weather-data-to-spark-dataframes"></a><span data-ttu-id="a2017-158">Import dat letecká společnost a počasí do Spark DataFrames</span><span class="sxs-lookup"><span data-stu-id="a2017-158">Importing the airline and weather data to Spark DataFrames</span></span>

<span data-ttu-id="a2017-159">Teď používáme SparkR [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) funkce importování dat počasí a letecká společnost do Spark DataFrames.</span><span class="sxs-lookup"><span data-stu-id="a2017-159">Now we use the SparkR [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) function to import the weather and airline data to Spark DataFrames.</span></span> <span data-ttu-id="a2017-160">Tato funkce, jako je mnoho dalších metod Spark jsou spouštěny líné, což znamená, že jsou zařazené do fronty pro provádění ale nebyl proveden až požadovaných.</span><span class="sxs-lookup"><span data-stu-id="a2017-160">This function, like many other Spark methods, are executed lazily, meaning that they are queued for execution but not executed until required.</span></span>

```
airPath     <- file.path(inputDataDir, "AirOnTime08to12CSV")
weatherPath <- file.path(inputDataDir, "Weather") # pre-processed weather data
rxHadoopListFiles(airPath) 
rxHadoopListFiles(weatherPath) 

# create a SparkR DataFrame for the airline data

logmsg('create a SparkR DataFrame for the airline data') 
# use inferSchema = "false" for more robust parsing
airDF <- read.df(sqlContext, airPath, source = "com.databricks.spark.csv", 
                 header = "true", inferSchema = "false")

# Create a SparkR DataFrame for the weather data

logmsg('create a SparkR DataFrame for the weather data') 
weatherDF <- read.df(sqlContext, weatherPath, source = "com.databricks.spark.csv", 
                     header = "true", inferSchema = "true")
```

## <a name="data-cleansing-and-transformation"></a><span data-ttu-id="a2017-161">Vyčištění dat a transformace</span><span class="sxs-lookup"><span data-stu-id="a2017-161">Data cleansing and transformation</span></span>

<span data-ttu-id="a2017-162">Další provedeme některé vyčištění dat letecká společnost jsme jste naimportovali přejmenování sloupce.</span><span class="sxs-lookup"><span data-stu-id="a2017-162">Next we do some cleanup on the airline data we’ve imported to rename columns.</span></span> <span data-ttu-id="a2017-163">Jsme pouze zachovat proměnné potřeby a zaokrouhlit odeslání Naplánované časy dolů nejbližší hodinu povolit sloučení s nejnovější data počasí na odeslání:</span><span class="sxs-lookup"><span data-stu-id="a2017-163">We only keep the variables needed, and round scheduled departure times down to the nearest hour to enable merging with the latest weather data at departure:</span></span>

```
logmsg('clean the airline data') 
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

# Select desired columns from the flight data. 
varsToKeep <- c("ArrDel15", "Year", "Month", "DayofMonth", "DayOfWeek", "Carrier", "OriginAirportID", "DestAirportID", "CRSDepTime", "CRSArrTime")
airDF <- select(airDF, varsToKeep)

# Apply schema
coltypes(airDF) <- c("character", "integer", "integer", "integer", "integer", "character", "integer", "integer", "integer", "integer")

# Round down scheduled departure time to full hour.
airDF$CRSDepTime <- floor(airDF$CRSDepTime / 100)
```

<span data-ttu-id="a2017-164">Teď nemůžeme provést podobnými operacemi na údaje o počasí:</span><span class="sxs-lookup"><span data-stu-id="a2017-164">Now we perform similar operations on the weather data:</span></span>

```
# Average weather readings by hour
logmsg('clean the weather data') 
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

## <a name="joining-the-weather-and-airline-data"></a><span data-ttu-id="a2017-165">Spojování dat počasí a letecká společnost</span><span class="sxs-lookup"><span data-stu-id="a2017-165">Joining the weather and airline data</span></span>

<span data-ttu-id="a2017-166">Teď používáme SparkR [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) funkce udělat levé vnější spojení letecká společnost a počasí dat odeslání AirportID a data a času.</span><span class="sxs-lookup"><span data-stu-id="a2017-166">We now use the SparkR [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) function to do a left outer join of the airline and weather data by departure AirportID and datetime.</span></span> <span data-ttu-id="a2017-167">Vnější spojení umožňuje zachovat všechny záznamy letecká společnost i v případě, že neexistuje žádná odpovídající počasí data.</span><span class="sxs-lookup"><span data-stu-id="a2017-167">The outer join allows us to retain all the airline data records even if there is no matching weather data.</span></span> <span data-ttu-id="a2017-168">Následující spojení jsme odebrat některé redundantní sloupce a přejmenujte ponecháno sloupce, které chcete odebrat předponu příchozí DataFrame zaváděné spojení.</span><span class="sxs-lookup"><span data-stu-id="a2017-168">Following the join, we remove some redundant columns, and rename the kept columns to remove the incoming DataFrame prefix introduced by the join.</span></span>

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

<span data-ttu-id="a2017-169">Podobným způsobem jsme spojení počasí a letecká společnost dat na základě příchodem AirportID a data a času:</span><span class="sxs-lookup"><span data-stu-id="a2017-169">In a similar fashion, we join the weather and airline data based on arrival AirportID and datetime:</span></span>

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

## <a name="save-results-to-csv-for-exchange-with-scaler"></a><span data-ttu-id="a2017-170">Uložte výsledky do sdíleného svazku clusteru pro server exchange s ScaleR</span><span class="sxs-lookup"><span data-stu-id="a2017-170">Save results to CSV for exchange with ScaleR</span></span>

<span data-ttu-id="a2017-171">Tím končí spojení, které je potřeba udělat s SparkR.</span><span class="sxs-lookup"><span data-stu-id="a2017-171">That completes the joins we need to do with SparkR.</span></span> <span data-ttu-id="a2017-172">Nemůžeme uložit data z poslední DataFrame Spark "joinedDF5" do sdíleného svazku clusteru pro vstup na ScaleR a pak zavřete out SparkR relace.</span><span class="sxs-lookup"><span data-stu-id="a2017-172">We save the data from the final Spark DataFrame “joinedDF5” to a CSV for input to ScaleR and then close out the SparkR session.</span></span> <span data-ttu-id="a2017-173">Nemůžeme říct explicitně SparkR uložení výsledné sdíleného svazku clusteru v 80 samostatné oddíly povolit dostatečná paralelismus se při zpracování ScaleR:</span><span class="sxs-lookup"><span data-stu-id="a2017-173">We explicitly tell SparkR to save the resultant CSV in 80 separate partitions to enable sufficient parallelism in ScaleR processing:</span></span>

```
logmsg('output the joined data from Spark to CSV') 
joinedDF5 <- repartition(joinedDF5, 80) # write.df below will produce this many CSVs

# write result to directory of CSVs
write.df(joinedDF5, file.path(dataDir, "joined5Csv"), "com.databricks.spark.csv", "overwrite", header = "true")

# We can shut down the SparkR Spark context now
sparkR.stop()

# remove non-data files
rxHadoopRemove(file.path(dataDir, "joined5Csv/_SUCCESS"))
```

## <a name="import-to-xdf-for-use-by-scaler"></a><span data-ttu-id="a2017-174">Importovat do XDF pro použití ScaleR</span><span class="sxs-lookup"><span data-stu-id="a2017-174">Import to XDF for use by ScaleR</span></span>

<span data-ttu-id="a2017-175">Můžeme použít soubor CSV připojené k letecká společnost a počasí dat jako-je pro modelování prostřednictvím zdroj dat ScaleR text.</span><span class="sxs-lookup"><span data-stu-id="a2017-175">We could use the CSV file of joined airline and weather data as-is for modeling via a ScaleR text data source.</span></span> <span data-ttu-id="a2017-176">Ale jsme ho importovat do XDF nejprve, protože je efektivnější při spuštění více operací na datovou sadu:</span><span class="sxs-lookup"><span data-stu-id="a2017-176">But we import it to XDF first, since it is more efficient when running multiple operations on the dataset:</span></span>

```
logmsg('Import the CSV to compressed, binary XDF format') 

# set the Spark compute context for R Server 
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

## <a name="splitting-data-for-training-and-test"></a><span data-ttu-id="a2017-177">Rozdělení dat pro trénování a testování</span><span class="sxs-lookup"><span data-stu-id="a2017-177">Splitting data for training and test</span></span>

<span data-ttu-id="a2017-178">Můžeme použít rxDataStep zachovat rest pro školení a rozdělení 2012 dat pro testování:</span><span class="sxs-lookup"><span data-stu-id="a2017-178">We use rxDataStep to split out the 2012 data for testing and keep the rest for training:</span></span>

```
# split out the training data

logmsg('split out training data as all data except year 2012')
trainDS <- RxXdfData( file.path(dataDir, "finalDataTrain" ),fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = trainDS,
            rowSelection = ( Year != 2012 ), overwrite = T )

# split out the testing data

logmsg('split out the test data for year 2012') 
testDS <- RxXdfData( file.path(dataDir, "finalDataTest" ), fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = testDS,
            rowSelection = ( Year == 2012 ), overwrite = T )

rxGetInfo(trainDS)
rxGetInfo(testDS)
```

## <a name="train-and-test-a-logistic-regression-model"></a><span data-ttu-id="a2017-179">Trénování a testování logistic regresní model</span><span class="sxs-lookup"><span data-stu-id="a2017-179">Train and test a logistic regression model</span></span>

<span data-ttu-id="a2017-180">Nyní jsme připraveni k sestavení modelu.</span><span class="sxs-lookup"><span data-stu-id="a2017-180">Now we are ready to build a model.</span></span> <span data-ttu-id="a2017-181">Vliv data o počasí na zpoždění v čas doručení najdete používáme na ScaleR logistic regression rutiny.</span><span class="sxs-lookup"><span data-stu-id="a2017-181">To see the influence of weather data on delay in the arrival time, we use ScaleR’s logistic regression routine.</span></span> <span data-ttu-id="a2017-182">Můžeme použít pro modelování zda prodlevu příchodem delší než 15 minut je ovlivněno počasí na letištích odeslání a přijetí:</span><span class="sxs-lookup"><span data-stu-id="a2017-182">We use it to model whether an arrival delay of greater than 15 minutes is influenced by the weather at the departure and arrival airports:</span></span>

```
logmsg('train a logistic regression model for Arrival Delay > 15 minutes') 
formula <- as.formula(ArrDel15 ~ Year + Month + DayofMonth + DayOfWeek + Carrier +
                     OriginAirportID + DestAirportID + CRSDepTime + CRSArrTime + 
                     RelativeHumidityOrigin + AltimeterOrigin + DryBulbCelsiusOrigin +
                     WindSpeedOrigin + VisibilityOrigin + DewPointCelsiusOrigin + 
                     RelativeHumidityDest + AltimeterDest + DryBulbCelsiusDest +
                     WindSpeedDest + VisibilityDest + DewPointCelsiusDest
                   )

# Use the scalable rxLogit() function but set max iterations to 3 for the purposes of 
# this exercise 

logitModel <- rxLogit(formula, data = trainDS, maxIterations = 3)

base::summary(logitModel)
```

<span data-ttu-id="a2017-183">Nyní si ukážeme, jak to dělá na testovací data tím, že některé předpovědi a prohlížení ROC a AUC.</span><span class="sxs-lookup"><span data-stu-id="a2017-183">Now let’s see how it does on the test data by making some predictions and looking at ROC and AUC.</span></span>

```
# Predict over test data (Logistic Regression).

logmsg('predict over the test data') 
logitPredict <- RxXdfData(file.path(dataDir, "logitPredict"), fileSystem = hdfsFS)

# Use the scalable rxPredict() function

rxPredict(logitModel, data = testDS, outData = logitPredict,
          extraVarsToWrite = c("ArrDel15"), 
          type = 'response', overwrite = TRUE)

# Calculate ROC and Area Under the Curve (AUC).

logmsg('calculate the roc and auc') 
logitRoc <- rxRoc("ArrDel15", "ArrDel15_Pred", logitPredict)
logitAuc <- rxAuc(logitRoc)
head(logitAuc)
logitAuc

plot(logitRoc)
```

## <a name="scoring-elsewhere"></a><span data-ttu-id="a2017-184">Vyhodnocování jinde</span><span class="sxs-lookup"><span data-stu-id="a2017-184">Scoring elsewhere</span></span>

<span data-ttu-id="a2017-185">Také můžeme použít model pro vyhodnocování dat na jiné platformě.</span><span class="sxs-lookup"><span data-stu-id="a2017-185">We can also use the model for scoring data on another platform.</span></span> <span data-ttu-id="a2017-186">Ukládání do souboru vzdálené plochy a přenosu a importu této vzdálené plochy do cílového vyhodnocování prostředí, jako jsou R služby serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a2017-186">By saving it to an RDS file and then transferring and importing that RDS into a destination scoring environment such as SQL Server R Services.</span></span> <span data-ttu-id="a2017-187">Je důležité zajistit, že úrovně data, která se má vypočítat skóre shodují s těmi, na kterých byl vytvořený modelu.</span><span class="sxs-lookup"><span data-stu-id="a2017-187">It is important to ensure that the factor levels of the data to be scored match those on which the model was built.</span></span> <span data-ttu-id="a2017-188">Které odpovídají lze dosáhnout extrahování a ukládá informace o sloupci spojeného s daty modelování prostřednictvím na ScaleR `rxCreateColInfo()` funkce a potom se použijí tyto sloupce informace ke zdroji vstupní data pro předpověď.</span><span class="sxs-lookup"><span data-stu-id="a2017-188">That match can be achieved by extracting and saving the column infomation associated with the modeling data via ScaleR’s `rxCreateColInfo()` function and then applying that column information to the input data source for prediction.</span></span> <span data-ttu-id="a2017-189">V následující jsme uložit několik řádků testovací datové sady a extrahovat a použít informace o sloupci od této ukázky ve skriptu předpovědi:</span><span class="sxs-lookup"><span data-stu-id="a2017-189">In the following we save a few rows of the test dataset and extract and use the column information from this sample in the prediction script:</span></span>

```
# save the model and a sample of the test dataset 

logmsg('save serialized version of the model and a sample of the test data')
rxSetComputeContext('localpar') 
saveRDS(logitModel, file = "logitModel.rds")
testDF <- head(testDS, 1000)  
saveRDS(testDF    , file = "testDF.rds"    )
list.files()

rxHadoopListFiles(file.path(inputDataDir,''))
rxHadoopListFiles(dataDir)

# stop the spark engine 
rxStopEngine(sparkCC) 

logmsg('Done.')
elapsed <- (proc.time() - t0)[3]
logmsg(paste('Elapsed time=',sprintf('%6.2f',elapsed),'(sec)\n\n'))
```

## <a name="summary"></a><span data-ttu-id="a2017-190">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a2017-190">Summary</span></span>

<span data-ttu-id="a2017-191">V tomto článku jsme jste ukazuje, jak je možné kombinovat používání SparkR pro manipulaci s daty s ScaleR pro vývoj modelu v Hadoop, Spark.</span><span class="sxs-lookup"><span data-stu-id="a2017-191">In this article, we’ve shown how it’s possible to combine use of SparkR for data manipulation with ScaleR for model development in Hadoop Spark.</span></span> <span data-ttu-id="a2017-192">Tento scénář vyžaduje udržovat samostatných relací Spark pouze jednu relaci spuštěný v čase a vyměňovat data prostřednictvím souborů CSV.</span><span class="sxs-lookup"><span data-stu-id="a2017-192">This scenario requires that you maintain separate Spark sessions, only running one session at a time, and exchange data via CSV files.</span></span> <span data-ttu-id="a2017-193">I když je jasné, tento proces by měl být snazší i v budoucích vydání R Server, SparkR a ScaleR můžete sdílet relaci Spark a sdílet tak Spark DataFrames.</span><span class="sxs-lookup"><span data-stu-id="a2017-193">Although straightforward, this process should be even easier in an upcoming R Server release, when SparkR and ScaleR can share a Spark session and so share Spark DataFrames.</span></span>

## <a name="next-steps-and-more-information"></a><span data-ttu-id="a2017-194">Další informace a další kroky</span><span class="sxs-lookup"><span data-stu-id="a2017-194">Next steps and more information</span></span>

- <span data-ttu-id="a2017-195">Další informace o použití serveru R na Spark, najdete v článku [Průvodce Začínáme na webu MSDN](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)</span><span class="sxs-lookup"><span data-stu-id="a2017-195">For more information on use of R Server on Spark, see the [Getting started guide on MSDN](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)</span></span>

- <span data-ttu-id="a2017-196">Obecné informace o R Server, najdete v článku [začít pracovat s R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) článku.</span><span class="sxs-lookup"><span data-stu-id="a2017-196">For general information on R Server, see the [Get started with R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) article.</span></span>

- <span data-ttu-id="a2017-197">Informace o R serverem v HDInsight, naleznete v části [R serverem v Azure HDInsight přehled](hdinsight-hadoop-r-server-overview.md) a [R serverem v Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a2017-197">For information on R Server on HDInsight, see [R Server on Azure HDInsight overview](hdinsight-hadoop-r-server-overview.md) and [R Server on Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).</span></span>

<span data-ttu-id="a2017-198">Další informace o použití SparkR najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="a2017-198">For more information on use of SparkR, see:</span></span>

- [<span data-ttu-id="a2017-199">Apache SparkR dokumentu</span><span class="sxs-lookup"><span data-stu-id="a2017-199">Apache SparkR document</span></span>](https://spark.apache.org/docs/2.1.0/sparkr.html)

- <span data-ttu-id="a2017-200">[Přehled SparkR](https://docs.databricks.com/spark/latest/sparkr/overview.html) z Databricks</span><span class="sxs-lookup"><span data-stu-id="a2017-200">[SparkR Overview](https://docs.databricks.com/spark/latest/sparkr/overview.html) from Databricks</span></span>
