---
title: "(nepoužívané) Vytváření prognóz: Autoregressive integrované klouzavý průměr (ARIMA) - Azure | Microsoft Docs"
description: "(nepoužívané) Vytváření prognóz - Autoregressive integrované klouzavý průměr (ARIMA)"
services: machine-learning
documentationcenter: 
author: yijichen
manager: jhubbard
editor: cgronlun
ms.assetid: 1e0d525f-8a9e-4b42-87e0-c9423f059f8c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: yijichen
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 4f3af41fd8873fdf75c6426fd395351cb41db190
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-forecasting---autoregressive-integrated-moving-average-arima"></a><span data-ttu-id="62c77-103">(nepoužívané) Vytváření prognóz - Autoregressive integrované klouzavý průměr (ARIMA)</span><span class="sxs-lookup"><span data-stu-id="62c77-103">(deprecated) Forecasting - Autoregressive Integrated Moving Average (ARIMA)</span></span>

> [!NOTE]
> <span data-ttu-id="62c77-104">Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="62c77-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="62c77-105">Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="62c77-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="62c77-106">Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="62c77-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>


<span data-ttu-id="62c77-107">To [služby](https://datamarket.azure.com/dataset/aml_labs/arima) implementuje předpovědi tooproduce Autoregressive integrované přesunutí průměrná (ARIMA) podle hello historická data zadaná uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="62c77-107">This [service](https://datamarket.azure.com/dataset/aml_labs/arima) implements Autoregressive Integrated Moving Average (ARIMA) tooproduce predictions based on hello historical data provided by hello user.</span></span> <span data-ttu-id="62c77-108">Bude hello vyžádání pro konkrétní produkt zvýšení tohoto roku?</span><span class="sxs-lookup"><span data-stu-id="62c77-108">Will hello demand for a specific product increase this year?</span></span> <span data-ttu-id="62c77-109">Může I předpovědi Moje prodej produktu pro hello za Štědrý sezóny tak, aby plánuji můžete efektivně Moje inventáře?</span><span class="sxs-lookup"><span data-stu-id="62c77-109">Can I predict my product sales for hello Christmas season, so that I can effectively plan my inventory?</span></span> <span data-ttu-id="62c77-110">Modelů prognózy, které jsou výstižný tooaddress takové dotazy.</span><span class="sxs-lookup"><span data-stu-id="62c77-110">Forecasting models are apt tooaddress such questions.</span></span> <span data-ttu-id="62c77-111">Zadané hello za data, zkontrolujte tyto modely skrytá trendy a vývoji toopredict sezónnosti.</span><span class="sxs-lookup"><span data-stu-id="62c77-111">Given hello past data, these models examine hidden trends and seasonality toopredict future trends.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="62c77-112">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="62c77-112">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="62c77-113">Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="62c77-113">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="62c77-114">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="62c77-114">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="62c77-115">Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="62c77-115">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="62c77-116">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="62c77-116">Consumption of web service</span></span>
<span data-ttu-id="62c77-117">Tato služba přijímá argumenty 4 a vypočítá hello ARIMA prognózy.</span><span class="sxs-lookup"><span data-stu-id="62c77-117">This service accepts 4 arguments and calculates hello ARIMA forecasts.</span></span>
<span data-ttu-id="62c77-118">Hello vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="62c77-118">hello input arguments are:</span></span>

* <span data-ttu-id="62c77-119">Frekvence - určuje četnost hello hello nezpracovaná data (denně nebo týdně nebo měsíční nebo čtvrtletně nebo ročně).</span><span class="sxs-lookup"><span data-stu-id="62c77-119">Frequency - Indicates hello frequency of hello raw data (daily/weekly/monthly/quarterly/yearly).</span></span>
* <span data-ttu-id="62c77-120">Horizon - budoucí prognózy časového rámce.</span><span class="sxs-lookup"><span data-stu-id="62c77-120">Horizon - Future forecast time-frame.</span></span>
* <span data-ttu-id="62c77-121">Date – přidání v hello nové časové řady dat dobu.</span><span class="sxs-lookup"><span data-stu-id="62c77-121">Date - Add in hello new time series data for time.</span></span>
* <span data-ttu-id="62c77-122">Hodnota – přidání v hello nové časových řad datové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="62c77-122">Value - Add in hello new time series data values.</span></span>

<span data-ttu-id="62c77-123">výstup Hello hello služby je, že hello vypočítat prognózy hodnoty.</span><span class="sxs-lookup"><span data-stu-id="62c77-123">hello output of hello service is hello calculated forecast values.</span></span> 

<span data-ttu-id="62c77-124">Ukázka vstup může být:</span><span class="sxs-lookup"><span data-stu-id="62c77-124">Sample input could be:</span></span> 

* <span data-ttu-id="62c77-125">Frekvence - 12</span><span class="sxs-lookup"><span data-stu-id="62c77-125">Frequency - 12</span></span>
* <span data-ttu-id="62c77-126">Horizon - 12</span><span class="sxs-lookup"><span data-stu-id="62c77-126">Horizon - 12</span></span>
* <span data-ttu-id="62c77-127">Datum - 1, 15 nebo 2012, 2, 15 nebo 2012, 3, 15 nebo 2012; 4, 15 nebo 2012; 5 nebo 15/2012; 6/15/2012; 15/7/2012, 8 / 15/2012, 9, 15 nebo 2012, 10, 15 nebo 2012; 11/15/2012, 12/15/2012; 1, 15 nebo 2013, 2, 15 nebo 2013, 3, 15 nebo 2013; 4, 15 nebo 2013; 5 nebo 15/2013; 6, 15 nebo 2013; 7, 15 nebo 2013, 8 / 15/2013, 9, 15 nebo 2013, 10, 15 nebo 2013; 11, 15 nebo 2013, 12/15/2013; 1, 15 nebo 2014, 2, 15 nebo 2014, 3, 15 nebo 2014; 4/15/2014; 5 nebo 15/2014; 6/15/2014; 7, 15 nebo 2014, 8 / 15/2014; 9, 15 nebo 2014</span><span class="sxs-lookup"><span data-stu-id="62c77-127">Date - 1/15/2012;2/15/2012;3/15/2012;4/15/2012;5/15/2012;6/15/2012;7/15/2012;8/15/2012;9/15/2012;10/15/2012;11/15/2012;12/15/2012; 1/15/2013;2/15/2013;3/15/2013;4/15/2013;5/15/2013;6/15/2013;7/15/2013;8/15/2013;9/15/2013;10/15/2013;11/15/2013;12/15/2013; 1/15/2014;2/15/2014;3/15/2014;4/15/2014;5/15/2014;6/15/2014;7/15/2014;8/15/2014;9/15/2014</span></span>
* <span data-ttu-id="62c77-128">Hodnota – 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509</span><span class="sxs-lookup"><span data-stu-id="62c77-128">Value - 3.479;3.68;3.832;3.941;3.797;3.586;3.508;3.731;3.915;3.844;3.634;3.549;3.557;3.785;3.782;3.601;3.544;3.556;3.65;3.709;3.682;3.511; 3.429;3.51;3.523;3.525;3.626;3.695;3.711;3.711;3.693;3.571;3.509</span></span>

> <span data-ttu-id="62c77-129">Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="62c77-129">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="62c77-130">Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/ArimaForecasting.aspx)).</span><span class="sxs-lookup"><span data-stu-id="62c77-130">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/ArimaForecasting.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="62c77-131">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="62c77-131">Starting C# code for web service consumption:</span></span>
    public class Input
    {
        public string frequency;
        public string horizon;
        public string date;
        public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
         byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
         return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }


    void Main()
    {
          var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };
        var json = JsonConvert.SerializeObject(input);
        var acitionUri =  "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";

          var httpClient = new HttpClient();
           httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere","ChangeToAPIKey");
           httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
          var query = httpClient.PostAsync(acitionUri,new StringContent(json));
          var result = query.Result.Content;
          var scoreResult = result.ReadAsStringAsync().Result;
      }

## <a name="creation-of-web-service"></a><span data-ttu-id="62c77-132">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="62c77-132">Creation of web service</span></span>
> <span data-ttu-id="62c77-133">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="62c77-133">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="62c77-134">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="62c77-134">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="62c77-135">Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="62c77-135">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="62c77-136">Z v rámci Azure Machine Learning, která vytvořila nový prázdný experiment.</span><span class="sxs-lookup"><span data-stu-id="62c77-136">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="62c77-137">Ukázková vstupní data byla nahrána s předem definovaných datových schématu.</span><span class="sxs-lookup"><span data-stu-id="62c77-137">Sample input data was uploaded with a predefined data schema.</span></span> <span data-ttu-id="62c77-138">Schéma dat propojené toohello je [spustit skript jazyka R] [ execute-r-script] modul, který generuje hello ARIMA předpovědi modelu s použitím funkce 'auto.arima' a 'Prognóza' z R.</span><span class="sxs-lookup"><span data-stu-id="62c77-138">Linked toohello data schema is an [Execute R Script][execute-r-script] module, which generates hello ARIMA forecasting model by using ‘auto.arima’ and ‘forecast’ functions from R.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="62c77-139">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="62c77-139">Experiment flow:</span></span>
![Vytvořit pracovní prostor][2]

#### <a name="module-1"></a><span data-ttu-id="62c77-141">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="62c77-141">Module 1:</span></span>
    # Add in hello CSV file with hello data in hello format shown below 
![Vytvořit pracovní prostor][3]    

#### <a name="module-2"></a><span data-ttu-id="62c77-143">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="62c77-143">Module 2:</span></span>
    # data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)

    # preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]

    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)

    # fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- auto.arima(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)

    # produce forecasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")

    # data output
    maml.mapOutputPort("data.forecast");


## <a name="limitations"></a><span data-ttu-id="62c77-144">Omezení</span><span class="sxs-lookup"><span data-stu-id="62c77-144">Limitations</span></span>
<span data-ttu-id="62c77-145">Toto je velmi jednoduchý příklad u prognózy ARIMA.</span><span class="sxs-lookup"><span data-stu-id="62c77-145">This is a very simple example for ARIMA forecasting.</span></span> <span data-ttu-id="62c77-146">Jak je vidět z výše uvedený kód příklad hello, žádné chyby zachytávání je implementována a hello služby předpokládá, že jsou všechny proměnné hello průběžné nebo kladnou hodnoty a hello frekvence musí být celé číslo větší než 1.</span><span class="sxs-lookup"><span data-stu-id="62c77-146">As can be seen from hello example code above, no error catching is implemented, and hello service assumes that all hello variables are continuous/positive values and hello frequency should be an integer greater than 1.</span></span> <span data-ttu-id="62c77-147">Délka Hello hello hodnotu data a vektory musí hello stejné.</span><span class="sxs-lookup"><span data-stu-id="62c77-147">hello length of hello date and value vectors should be hello same.</span></span> <span data-ttu-id="62c77-148">proměnná Datum Hello by měl splňovat toohello formátu, mm/dd/rrrr'.</span><span class="sxs-lookup"><span data-stu-id="62c77-148">hello date variable should adhere toohello format ‘mm/dd/yyyy’.</span></span>

## <a name="faq"></a><span data-ttu-id="62c77-149">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="62c77-149">FAQ</span></span>
<span data-ttu-id="62c77-150">Nejčastější dotazy o spotřebě hello webové služby nebo publikování toomarketplace, najdete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="62c77-150">For frequently asked questions on consumption of hello web service or publishing toomarketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-arima/arima-img1.png
[2]: ./media/machine-learning-r-csharp-arima/arima-img2.png
[3]: ./media/machine-learning-r-csharp-arima/arima-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

