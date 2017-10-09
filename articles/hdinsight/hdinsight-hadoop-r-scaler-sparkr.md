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
# <a name="combine-scaler-and-sparkr-in-hdinsight"></a><span data-ttu-id="1b537-103">Kombinace ScaleR a SparkR v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1b537-103">Combine ScaleR and SparkR in HDInsight</span></span>

<span data-ttu-id="1b537-104">Tento článek ukazuje, jak toopredict letu zpoždění doručení pomocí **ScaleR** logistic regresní model z dat na zpoždění letů a počasí spojena s **SparkR**.</span><span class="sxs-lookup"><span data-stu-id="1b537-104">This article shows how toopredict flight arrival delays using a **ScaleR** logistic regression model from data on flight delays and weather joined with **SparkR**.</span></span> <span data-ttu-id="1b537-105">Tento scénář předvádí možnosti hello ScaleR pro manipulaci s daty na Spark používat s Microsoft R Server pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="1b537-105">This scenario demonstrates hello capabilities of ScaleR for data manipulation on Spark used with Microsoft R Server for analytics.</span></span> <span data-ttu-id="1b537-106">Hello kombinace těchto technologií umožňuje tooapply hello nejnovější funkce v distribuované zpracování.</span><span class="sxs-lookup"><span data-stu-id="1b537-106">hello combination of these technologies enables you tooapply hello latest capabilities in distributed processing.</span></span>

<span data-ttu-id="1b537-107">I když oba balíčky běží na stroji provádění Spark pro Hadoop, jsou blokovat sdílení jako každý vyžadují jejich vlastní příslušné relace Spark dat v paměti.</span><span class="sxs-lookup"><span data-stu-id="1b537-107">Although both packages run on Hadoop’s Spark execution engine, they are blocked from in-memory data sharing as they each require their own respective Spark sessions.</span></span> <span data-ttu-id="1b537-108">Dokud tomuto problému dochází v budoucích verzích R Server, je alternativní řešení hello toomaintain nepřekrývají Spark relace a tooexchange data prostřednictvím zprostředkující soubory.</span><span class="sxs-lookup"><span data-stu-id="1b537-108">Until this issue is addressed in an upcoming version of R Server, hello workaround is toomaintain non-overlapping Spark sessions, and tooexchange data through intermediate files.</span></span> <span data-ttu-id="1b537-109">Hello pokynů tady ukazují, že jsou tyto požadavky přehledné tooachieve.</span><span class="sxs-lookup"><span data-stu-id="1b537-109">hello instructions here show that these requirements are straightforward tooachieve.</span></span>

<span data-ttu-id="1b537-110">Používáme příklad zde původně sdílí v obraťte na 2016 vrstev Mario Inchiosa a Roni Burd, která je také k dispozici prostřednictvím hello webinář [vytváření škálovatelné platformy vědecké účely dat s R](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). Příklad hello používá SparkR toojoin hello dobře známé airlines příchodem zpoždění datové sady daty počasí na letištích odeslání a přijetí.</span><span class="sxs-lookup"><span data-stu-id="1b537-110">We use an example here initially shared in a talk at Strata 2016 by Mario Inchiosa and Roni Burd that is also available through hello webinar [Building a Scalable Data Science Platform with R](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). hello example uses SparkR toojoin hello well-known airlines arrival delay data set with weather data at departure and arrival airports.</span></span> <span data-ttu-id="1b537-111">data Hello připojený se pak používá jako vstupní tooa ScaleR logistic regresní model pro predikci letu příchodem zpoždění.</span><span class="sxs-lookup"><span data-stu-id="1b537-111">hello data joined is then used as input tooa ScaleR logistic regression model for predicting flight arrival delay.</span></span>

<span data-ttu-id="1b537-112">Hello kód jsme návod byl původně zapsán pro R Server spuštěný na Spark v clusteru služby HDInsight v Azure.</span><span class="sxs-lookup"><span data-stu-id="1b537-112">hello code we walkthrough was originally written for R Server running on Spark in an HDInsight cluster on Azure.</span></span> <span data-ttu-id="1b537-113">Ale hello koncept kombinování hello použití SparkR a ScaleR v jednom skriptu je taky platná v kontextu hello místní prostředí.</span><span class="sxs-lookup"><span data-stu-id="1b537-113">But hello concept of mixing hello use of SparkR and ScaleR in one script is also valid in hello context of on-premises environments.</span></span> <span data-ttu-id="1b537-114">V následující hello jsme předpokládá pokročilou úroveň znalosti R a R hello [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) knihovny R Server.</span><span class="sxs-lookup"><span data-stu-id="1b537-114">In hello following, we presume an intermediate level of knowledge of R and R hello [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) library of R Server.</span></span> <span data-ttu-id="1b537-115">Zavedeme použití [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) při procházení tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="1b537-115">We also introduce use of [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) while walking through this scenario.</span></span>

## <a name="hello-airline-and-weather-datasets"></a><span data-ttu-id="1b537-116">Hello letecká společnost a počasí datové sady</span><span class="sxs-lookup"><span data-stu-id="1b537-116">hello airline and weather datasets</span></span>

<span data-ttu-id="1b537-117">Hello **AirOnTime08to12CSV** airlines veřejné datová sada obsahuje informace o letu příchodem a odeslání podrobnosti o všech komerční letů v rámci hello USA, z října 1987 tooDecember 2012.</span><span class="sxs-lookup"><span data-stu-id="1b537-117">hello **AirOnTime08to12CSV** airlines public dataset contains information on flight arrival and departure details for all commercial flights within hello USA, from October 1987 tooDecember 2012.</span></span> <span data-ttu-id="1b537-118">Toto je velké datové sady: celkem jsou téměř 150 miliony záznamů.</span><span class="sxs-lookup"><span data-stu-id="1b537-118">This is a large dataset: there are nearly 150 million records in total.</span></span> <span data-ttu-id="1b537-119">Je právě v části vybaleno 4 GB.</span><span class="sxs-lookup"><span data-stu-id="1b537-119">It is just under 4 GB unpacked.</span></span> <span data-ttu-id="1b537-120">Je k dispozici z hello [US government archivy](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236).</span><span class="sxs-lookup"><span data-stu-id="1b537-120">It is available from hello [U.S. government archives](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236).</span></span> <span data-ttu-id="1b537-121">Snadněji, je k dispozici jako soubor zip (AirOnTimeCSV.zip) obsahující sadu 303 samostatné měsíční CSV soubory z hello [úložiště Revolution Analytics datové sady](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)</span><span class="sxs-lookup"><span data-stu-id="1b537-121">More conveniently, it is available as a zip file (AirOnTimeCSV.zip) containing a set of 303 separate monthly CSV files from hello [Revolution Analytics dataset repository](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)</span></span>

<span data-ttu-id="1b537-122">toosee hello účinky počasí na zpoždění letů, potřebujeme také hello počasí dat na všech letištích hello.</span><span class="sxs-lookup"><span data-stu-id="1b537-122">toosee hello effects of weather on flight delays, we also need hello weather data at each of hello airports.</span></span> <span data-ttu-id="1b537-123">Tato data mohou stáhnout jako soubory zip v základním formátu měsíc z hello [National Oceánský a důsledky správy úložiště](http://www.ncdc.noaa.gov/orders/qclcd/).</span><span class="sxs-lookup"><span data-stu-id="1b537-123">This data can be downloaded as zip files in raw form, by month, from hello [National Oceanic and Atmospheric Administration repository](http://www.ncdc.noaa.gov/orders/qclcd/).</span></span> <span data-ttu-id="1b537-124">Pro účely tohoto příkladu hello jsme vyžádá počasí data z května 2007 – prosinec 2012 a použít hello hodinové datové soubory v každém z 68 měsíční zips hello.</span><span class="sxs-lookup"><span data-stu-id="1b537-124">For hello purposes of this example, we pull weather data from May 2007 – December 2012 and used hello hourly data files within each of hello 68 monthly zips.</span></span> <span data-ttu-id="1b537-125">měsíční soubory zip Hello také obsahovat mapování (YYYYMMstation.txt) mezi hello počasí stanice ID (WBAN), hello letiště, že je spojen s (volacím) a hello na letišti posun časového pásma od času UTC (časové pásmo).</span><span class="sxs-lookup"><span data-stu-id="1b537-125">hello monthly zip files also contain a mapping (YYYYMMstation.txt) between hello weather station ID (WBAN), hello airport that it is associated with (CallSign), and hello airport’s time zone offset from UTC (TimeZone).</span></span> <span data-ttu-id="1b537-126">Všechny tyto informace je třeba při propojení s daty zpoždění a počasí letecká společnost hello.</span><span class="sxs-lookup"><span data-stu-id="1b537-126">All of this information is needed when joining with hello airline delay and weather data.</span></span>

## <a name="setting-up-hello-spark-environment"></a><span data-ttu-id="1b537-127">Nastavení prostředí Spark hello</span><span class="sxs-lookup"><span data-stu-id="1b537-127">Setting up hello Spark environment</span></span>

<span data-ttu-id="1b537-128">prvním krokem Hello je tooset hello Spark prostředí.</span><span class="sxs-lookup"><span data-stu-id="1b537-128">hello first step is tooset up hello Spark environment.</span></span> <span data-ttu-id="1b537-129">Začneme odkazující toohello adresář, který obsahuje naše adresáře vstupních dat, vytváření kontextu výpočtů Spark a vytvoření funkce protokolování pro konzolu toohello informační protokolování:</span><span class="sxs-lookup"><span data-stu-id="1b537-129">We begin by pointing toohello directory that contains our input data directories, creating a Spark compute context, and creating a logging function for informational logging toohello console:</span></span>

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

<span data-ttu-id="1b537-130">Další přidáme cestu pro balíčky R hledání toohello "Spark_Home" tak, že jsme použít SparkR a inicializaci SparkR relace:</span><span class="sxs-lookup"><span data-stu-id="1b537-130">Next we add “Spark_Home” toohello search path for R packages so that we can use SparkR, and initialize a SparkR session:</span></span>

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

## <a name="preparing-hello-weather-data"></a><span data-ttu-id="1b537-131">Příprava dat počasí hello</span><span class="sxs-lookup"><span data-stu-id="1b537-131">Preparing hello weather data</span></span>

<span data-ttu-id="1b537-132">data o počasí hello tooprepare, jsme podmnožina ho toohello sloupce potřebné pro modelování:</span><span class="sxs-lookup"><span data-stu-id="1b537-132">tooprepare hello weather data, we subset it toohello columns needed for modeling:</span></span> 

- <span data-ttu-id="1b537-133">"Viditelnosti"</span><span class="sxs-lookup"><span data-stu-id="1b537-133">"Visibility"</span></span>
- <span data-ttu-id="1b537-134">"DryBulbCelsius"</span><span class="sxs-lookup"><span data-stu-id="1b537-134">"DryBulbCelsius"</span></span>
- <span data-ttu-id="1b537-135">"DewPointCelsius"</span><span class="sxs-lookup"><span data-stu-id="1b537-135">"DewPointCelsius"</span></span>
- <span data-ttu-id="1b537-136">"RelativeHumidity"</span><span class="sxs-lookup"><span data-stu-id="1b537-136">"RelativeHumidity"</span></span>
- <span data-ttu-id="1b537-137">"Větru"</span><span class="sxs-lookup"><span data-stu-id="1b537-137">"WindSpeed"</span></span>
- <span data-ttu-id="1b537-138">"Výškoměru"</span><span class="sxs-lookup"><span data-stu-id="1b537-138">"Altimeter"</span></span>

<span data-ttu-id="1b537-139">Pak přidejte letiště kód spojený s hello počasí stanice a převést hello měření z místního času tooUTC.</span><span class="sxs-lookup"><span data-stu-id="1b537-139">Then we add an airport code associated with hello weather station and convert hello measurements from local time tooUTC.</span></span>

<span data-ttu-id="1b537-140">Začneme vytvořením souboru toomap hello počasí stanice (WBAN) informace o tooan letiště kód.</span><span class="sxs-lookup"><span data-stu-id="1b537-140">We begin by creating a file toomap hello weather station (WBAN) info tooan airport code.</span></span> <span data-ttu-id="1b537-141">Tato korelace nám může získat ze souboru mapování hello součástí hello počasí data.</span><span class="sxs-lookup"><span data-stu-id="1b537-141">We could get this correlation from hello mapping file included with hello weather data.</span></span> <span data-ttu-id="1b537-142">Pomocí mapování hello *volacím* (například LAX) pole v datovém souboru počasí hello příliš*původu* v datech letecká společnost hello.</span><span class="sxs-lookup"><span data-stu-id="1b537-142">By mapping hello *CallSign* (for example, LAX) field in hello weather data file too*Origin* in hello airline data.</span></span> <span data-ttu-id="1b537-143">Ale jsme právě došlo k toohave jiné mapování na straně mapující *WBAN* příliš*AirportID* (například 12892 pro LAX) a zahrnuje *časové pásmo* , byla uložena tooa Soubor CSV názvem "wban na letiště id-tz. CSV", které můžeme použít.</span><span class="sxs-lookup"><span data-stu-id="1b537-143">However, we just happened toohave another mapping on hand that maps *WBAN* too*AirportID* (for example, 12892 for LAX) and includes *TimeZone* that has been saved tooa CSV file called “wban-to-airport-id-tz.CSV” that we can use.</span></span> <span data-ttu-id="1b537-144">Například:</span><span class="sxs-lookup"><span data-stu-id="1b537-144">For example:</span></span>

| <span data-ttu-id="1b537-145">AirportID</span><span class="sxs-lookup"><span data-stu-id="1b537-145">AirportID</span></span> | <span data-ttu-id="1b537-146">WBAN</span><span class="sxs-lookup"><span data-stu-id="1b537-146">WBAN</span></span> | <span data-ttu-id="1b537-147">Časové pásmo</span><span class="sxs-lookup"><span data-stu-id="1b537-147">TimeZone</span></span>
|-----------|------|---------
| <span data-ttu-id="1b537-148">10685</span><span class="sxs-lookup"><span data-stu-id="1b537-148">10685</span></span> | <span data-ttu-id="1b537-149">54831</span><span class="sxs-lookup"><span data-stu-id="1b537-149">54831</span></span> | <span data-ttu-id="1b537-150">-6</span><span class="sxs-lookup"><span data-stu-id="1b537-150">-6</span></span>
| <span data-ttu-id="1b537-151">14871</span><span class="sxs-lookup"><span data-stu-id="1b537-151">14871</span></span> | <span data-ttu-id="1b537-152">24232</span><span class="sxs-lookup"><span data-stu-id="1b537-152">24232</span></span> | <span data-ttu-id="1b537-153">-8</span><span class="sxs-lookup"><span data-stu-id="1b537-153">-8</span></span>
| <span data-ttu-id="1b537-154">..</span><span class="sxs-lookup"><span data-stu-id="1b537-154">..</span></span> | <span data-ttu-id="1b537-155">..</span><span class="sxs-lookup"><span data-stu-id="1b537-155">..</span></span> | <span data-ttu-id="1b537-156">..</span><span class="sxs-lookup"><span data-stu-id="1b537-156">..</span></span>

<span data-ttu-id="1b537-157">Následující kód čte každý počasí nezpracovaných dat po hodinách hello Hello soubory podmnožin toohello sloupce jsme potřebovat, sloučí soubor mapování počasí stanice hello, upraví hello datum čas tooUTC měření a pak zapíše na novou verzi souboru hello:</span><span class="sxs-lookup"><span data-stu-id="1b537-157">hello following code reads each of hello hourly raw weather data files, subsets toohello columns we need, merges hello weather station mapping file, adjusts hello date times of measurements tooUTC, and then writes out a new version of hello file:</span></span>

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

## <a name="importing-hello-airline-and-weather-data-toospark-dataframes"></a><span data-ttu-id="1b537-158">Import hello letecká společnost a počasí data tooSpark DataFrames</span><span class="sxs-lookup"><span data-stu-id="1b537-158">Importing hello airline and weather data tooSpark DataFrames</span></span>

<span data-ttu-id="1b537-159">Teď používáme hello SparkR [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) funkce tooimport hello počasí a letecká společnost dat tooSpark DataFrames.</span><span class="sxs-lookup"><span data-stu-id="1b537-159">Now we use hello SparkR [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) function tooimport hello weather and airline data tooSpark DataFrames.</span></span> <span data-ttu-id="1b537-160">Tato funkce, jako je mnoho dalších metod Spark jsou spouštěny líné, což znamená, že jsou zařazené do fronty pro provádění ale nebyl proveden až požadovaných.</span><span class="sxs-lookup"><span data-stu-id="1b537-160">This function, like many other Spark methods, are executed lazily, meaning that they are queued for execution but not executed until required.</span></span>

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

## <a name="data-cleansing-and-transformation"></a><span data-ttu-id="1b537-161">Vyčištění dat a transformace</span><span class="sxs-lookup"><span data-stu-id="1b537-161">Data cleansing and transformation</span></span>

<span data-ttu-id="1b537-162">Další provedeme některé vyčištění dat letecká společnost hello jsme jste naimportovali toorename sloupce.</span><span class="sxs-lookup"><span data-stu-id="1b537-162">Next we do some cleanup on hello airline data we’ve imported toorename columns.</span></span> <span data-ttu-id="1b537-163">Jsme pouze zachovat hello proměnných potřebných a zaokrouhlit odeslání Naplánované časy dolů toohello nejbližší hodinu tooenable sloučení s hello nejnovější data o počasí na odeslání:</span><span class="sxs-lookup"><span data-stu-id="1b537-163">We only keep hello variables needed, and round scheduled departure times down toohello nearest hour tooenable merging with hello latest weather data at departure:</span></span>

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

<span data-ttu-id="1b537-164">Nyní jsme provádět podobné operace s daty počasí hello:</span><span class="sxs-lookup"><span data-stu-id="1b537-164">Now we perform similar operations on hello weather data:</span></span>

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

## <a name="joining-hello-weather-and-airline-data"></a><span data-ttu-id="1b537-165">Spojování dat počasí a letecká společnost hello</span><span class="sxs-lookup"><span data-stu-id="1b537-165">Joining hello weather and airline data</span></span>

<span data-ttu-id="1b537-166">Teď používáme hello SparkR [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) funkce toodo levé vnější spojení hello letecká společnost a počasí dat odeslání AirportID a data a času.</span><span class="sxs-lookup"><span data-stu-id="1b537-166">We now use hello SparkR [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) function toodo a left outer join of hello airline and weather data by departure AirportID and datetime.</span></span> <span data-ttu-id="1b537-167">vnější spojení Hello umožňuje tooretain všechna data letecká společnost hello zaznamenává i v případě, že neexistuje žádná odpovídající počasí data.</span><span class="sxs-lookup"><span data-stu-id="1b537-167">hello outer join allows us tooretain all hello airline data records even if there is no matching weather data.</span></span> <span data-ttu-id="1b537-168">Následující spojení hello jsme odebrat některé redundantní sloupce a přejmenujte hello zachovány sloupce tooremove hello příchozí DataFrame předponu zaváděné hello spojení.</span><span class="sxs-lookup"><span data-stu-id="1b537-168">Following hello join, we remove some redundant columns, and rename hello kept columns tooremove hello incoming DataFrame prefix introduced by hello join.</span></span>

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

<span data-ttu-id="1b537-169">Podobným způsobem jsme spojení hello počasí a letecká společnost dat na základě příchodem AirportID a data a času:</span><span class="sxs-lookup"><span data-stu-id="1b537-169">In a similar fashion, we join hello weather and airline data based on arrival AirportID and datetime:</span></span>

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

## <a name="save-results-toocsv-for-exchange-with-scaler"></a><span data-ttu-id="1b537-170">Uložte výsledky tooCSV pro exchange s ScaleR</span><span class="sxs-lookup"><span data-stu-id="1b537-170">Save results tooCSV for exchange with ScaleR</span></span>

<span data-ttu-id="1b537-171">Tím končí potřebujeme toodo s SparkR spojení hello.</span><span class="sxs-lookup"><span data-stu-id="1b537-171">That completes hello joins we need toodo with SparkR.</span></span> <span data-ttu-id="1b537-172">Nemůžeme uložit hello data z hello konečné Spark DataFrame "joinedDF5" tooa sdíleného svazku clusteru pro vstupní tooScaleR a zavřete out hello SparkR relace.</span><span class="sxs-lookup"><span data-stu-id="1b537-172">We save hello data from hello final Spark DataFrame “joinedDF5” tooa CSV for input tooScaleR and then close out hello SparkR session.</span></span> <span data-ttu-id="1b537-173">Nemůžeme říct explicitně SparkR toosave hello výsledné sdíleného svazku clusteru v 80 samostatné oddíly tooenable dostatečná paralelismus v ScaleR zpracování:</span><span class="sxs-lookup"><span data-stu-id="1b537-173">We explicitly tell SparkR toosave hello resultant CSV in 80 separate partitions tooenable sufficient parallelism in ScaleR processing:</span></span>

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

## <a name="import-tooxdf-for-use-by-scaler"></a><span data-ttu-id="1b537-174">Import tooXDF za účelem použití ScaleR</span><span class="sxs-lookup"><span data-stu-id="1b537-174">Import tooXDF for use by ScaleR</span></span>

<span data-ttu-id="1b537-175">Můžeme použít soubor CSV hello připojené k letecká společnost a počasí dat jako-je pro modelování prostřednictvím zdroj dat ScaleR text.</span><span class="sxs-lookup"><span data-stu-id="1b537-175">We could use hello CSV file of joined airline and weather data as-is for modeling via a ScaleR text data source.</span></span> <span data-ttu-id="1b537-176">Ale můžeme ho importovat tooXDF nejprve vzhledem k tomu, že je efektivnější při spuštění více operací pro datovou sadu hello:</span><span class="sxs-lookup"><span data-stu-id="1b537-176">But we import it tooXDF first, since it is more efficient when running multiple operations on hello dataset:</span></span>

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

## <a name="splitting-data-for-training-and-test"></a><span data-ttu-id="1b537-177">Rozdělení dat pro trénování a testování</span><span class="sxs-lookup"><span data-stu-id="1b537-177">Splitting data for training and test</span></span>

<span data-ttu-id="1b537-178">Můžeme použít rxDataStep toosplit hello 2012 dat pro testování a zachovat hello rest pro školení:</span><span class="sxs-lookup"><span data-stu-id="1b537-178">We use rxDataStep toosplit out hello 2012 data for testing and keep hello rest for training:</span></span>

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

## <a name="train-and-test-a-logistic-regression-model"></a><span data-ttu-id="1b537-179">Trénování a testování logistic regresní model</span><span class="sxs-lookup"><span data-stu-id="1b537-179">Train and test a logistic regression model</span></span>

<span data-ttu-id="1b537-180">Nyní jsme připravené toobuild modelu.</span><span class="sxs-lookup"><span data-stu-id="1b537-180">Now we are ready toobuild a model.</span></span> <span data-ttu-id="1b537-181">toosee hello vlivu data o počasí na zpoždění při čas doručení hello, použijeme je ScaleR logistic regression rutiny.</span><span class="sxs-lookup"><span data-stu-id="1b537-181">toosee hello influence of weather data on delay in hello arrival time, we use ScaleR’s logistic regression routine.</span></span> <span data-ttu-id="1b537-182">Můžeme použít ho toomodel zda prodlevu příchodem delší než 15 minut je ovlivněno hello počasí na letištích odeslání a přijetí hello:</span><span class="sxs-lookup"><span data-stu-id="1b537-182">We use it toomodel whether an arrival delay of greater than 15 minutes is influenced by hello weather at hello departure and arrival airports:</span></span>

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

<span data-ttu-id="1b537-183">Nyní si ukážeme, jak ho na hello testovacích dat tím, že některé předpovědi a prohlížení ROC a AUC.</span><span class="sxs-lookup"><span data-stu-id="1b537-183">Now let’s see how it does on hello test data by making some predictions and looking at ROC and AUC.</span></span>

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

## <a name="scoring-elsewhere"></a><span data-ttu-id="1b537-184">Vyhodnocování jinde</span><span class="sxs-lookup"><span data-stu-id="1b537-184">Scoring elsewhere</span></span>

<span data-ttu-id="1b537-185">Také můžeme použít hello model vyhodnocování dat na jiné platformě.</span><span class="sxs-lookup"><span data-stu-id="1b537-185">We can also use hello model for scoring data on another platform.</span></span> <span data-ttu-id="1b537-186">Uložení souboru tooan vzdálené plochy a přenosu a importu této vzdálené plochy do cílového vyhodnocování prostředí, jako jsou R služby serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1b537-186">By saving it tooan RDS file and then transferring and importing that RDS into a destination scoring environment such as SQL Server R Services.</span></span> <span data-ttu-id="1b537-187">Je důležité tooensure, který hello Multi-Factor úrovně toobe data hello skóre pro magnitudu shodují s těmi, na které hello byl vytvořený model.</span><span class="sxs-lookup"><span data-stu-id="1b537-187">It is important tooensure that hello factor levels of hello data toobe scored match those on which hello model was built.</span></span> <span data-ttu-id="1b537-188">Že shoda lze dosáhnout extrahování a ukládá informace o sloupci hello přidružené hello modelování dat přes na ScaleR `rxCreateColInfo()` funkce a potom se použijí tento sloupec informace toohello vstupní zdroj dat pro předpověď.</span><span class="sxs-lookup"><span data-stu-id="1b537-188">That match can be achieved by extracting and saving hello column infomation associated with hello modeling data via ScaleR’s `rxCreateColInfo()` function and then applying that column information toohello input data source for prediction.</span></span> <span data-ttu-id="1b537-189">V následující hello jsme uložit několik řádků hello testovací datové sady a extrahovat a použít informace o sloupci hello od této ukázky ve skriptu předpovědi hello:</span><span class="sxs-lookup"><span data-stu-id="1b537-189">In hello following we save a few rows of hello test dataset and extract and use hello column information from this sample in hello prediction script:</span></span>

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

## <a name="summary"></a><span data-ttu-id="1b537-190">Souhrn</span><span class="sxs-lookup"><span data-stu-id="1b537-190">Summary</span></span>

<span data-ttu-id="1b537-191">V tomto článku jsme jste ukazuje, jak je možné toocombine použití SparkR pro manipulaci s daty s ScaleR pro vývoj modelu v Hadoop, Spark.</span><span class="sxs-lookup"><span data-stu-id="1b537-191">In this article, we’ve shown how it’s possible toocombine use of SparkR for data manipulation with ScaleR for model development in Hadoop Spark.</span></span> <span data-ttu-id="1b537-192">Tento scénář vyžaduje udržovat samostatných relací Spark pouze jednu relaci spuštěný v čase a vyměňovat data prostřednictvím souborů CSV.</span><span class="sxs-lookup"><span data-stu-id="1b537-192">This scenario requires that you maintain separate Spark sessions, only running one session at a time, and exchange data via CSV files.</span></span> <span data-ttu-id="1b537-193">I když je jasné, tento proces by měl být snazší i v budoucích vydání R Server, SparkR a ScaleR můžete sdílet relaci Spark a sdílet tak Spark DataFrames.</span><span class="sxs-lookup"><span data-stu-id="1b537-193">Although straightforward, this process should be even easier in an upcoming R Server release, when SparkR and ScaleR can share a Spark session and so share Spark DataFrames.</span></span>

## <a name="next-steps-and-more-information"></a><span data-ttu-id="1b537-194">Další informace a další kroky</span><span class="sxs-lookup"><span data-stu-id="1b537-194">Next steps and more information</span></span>

- <span data-ttu-id="1b537-195">Další informace o použití serveru R na Spark, najdete v části hello [Průvodce Začínáme na webu MSDN](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)</span><span class="sxs-lookup"><span data-stu-id="1b537-195">For more information on use of R Server on Spark, see hello [Getting started guide on MSDN](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)</span></span>

- <span data-ttu-id="1b537-196">Obecné informace o R Server najdete v tématu hello [začít pracovat s R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) článku.</span><span class="sxs-lookup"><span data-stu-id="1b537-196">For general information on R Server, see hello [Get started with R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) article.</span></span>

- <span data-ttu-id="1b537-197">Informace o R serverem v HDInsight, naleznete v části [R serverem v Azure HDInsight přehled](hdinsight-hadoop-r-server-overview.md) a [R serverem v Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1b537-197">For information on R Server on HDInsight, see [R Server on Azure HDInsight overview](hdinsight-hadoop-r-server-overview.md) and [R Server on Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).</span></span>

<span data-ttu-id="1b537-198">Další informace o použití SparkR najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="1b537-198">For more information on use of SparkR, see:</span></span>

- [<span data-ttu-id="1b537-199">Apache SparkR dokumentu</span><span class="sxs-lookup"><span data-stu-id="1b537-199">Apache SparkR document</span></span>](https://spark.apache.org/docs/2.1.0/sparkr.html)

- <span data-ttu-id="1b537-200">[Přehled SparkR](https://docs.databricks.com/spark/latest/sparkr/overview.html) z Databricks</span><span class="sxs-lookup"><span data-stu-id="1b537-200">[SparkR Overview](https://docs.databricks.com/spark/latest/sparkr/overview.html) from Databricks</span></span>
