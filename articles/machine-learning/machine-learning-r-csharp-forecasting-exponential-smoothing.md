---
title: "(nepoužívané) Vytváření prognóz - exponenciální vyrovnání - Azure | Microsoft Docs"
description: "(nepoužívané) Webové služby: exponenciální prognózy vyhlazení"
services: machine-learning
documentationcenter: 
author: yijichen
manager: jhubbard
editor: cgronlun
ms.assetid: a4150681-6eac-4145-9eca-0cdf60781dde
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: yijichen
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 736d5d4adb8ecfd1e3372d273b64917f4a2e76ce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-forecasting---exponential-smoothing"></a><span data-ttu-id="9cc97-103">(nepoužívané) Vytváření prognóz - exponenciální vyrovnání</span><span class="sxs-lookup"><span data-stu-id="9cc97-103">(deprecated) Forecasting - Exponential Smoothing</span></span>

> [!NOTE]
> <span data-ttu-id="9cc97-104">Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="9cc97-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="9cc97-105">Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="9cc97-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="9cc97-106">Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="9cc97-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="9cc97-107">To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/ets) implementuje model exponenciální vyhlazování (ETS) k vytvoření předpovědi na základě historických dat zadané uživatelem.</span><span class="sxs-lookup"><span data-stu-id="9cc97-107">This [web service](https://datamarket.azure.com/dataset/aml_labs/ets) implements the Exponential Smoothing model (ETS) to produce predictions based on the historical data provided by the user.</span></span> <span data-ttu-id="9cc97-108">Zvýší zatížení pro určitý produkt tohoto roku</span><span class="sxs-lookup"><span data-stu-id="9cc97-108">Will the demand for a specific product increase this year?</span></span> <span data-ttu-id="9cc97-109">Může I předpovědi Moje produktu prodeje za Štědrý sezónu, tak, aby plánuji můžete efektivně Moje inventáře?</span><span class="sxs-lookup"><span data-stu-id="9cc97-109">Can I predict my product sales for the Christmas season, so that I can effectively plan my inventory?</span></span> <span data-ttu-id="9cc97-110">Prognózy lze modely jsou výstižný adres takové dotazy.</span><span class="sxs-lookup"><span data-stu-id="9cc97-110">Forecasting models are apt to address such questions.</span></span> <span data-ttu-id="9cc97-111">Zadaný posledních dat, zkontrolujte tyto modely skrytá trendy a sezónnosti předpovídat budoucí trendy.</span><span class="sxs-lookup"><span data-stu-id="9cc97-111">Given the past data, these models examine hidden trends and seasonality to predict future trends.</span></span>  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="9cc97-112">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="9cc97-112">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="9cc97-113">Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="9cc97-113">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="9cc97-114">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="9cc97-114">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="9cc97-115">Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="9cc97-115">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="9cc97-116">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="9cc97-116">Consumption of web service</span></span>
<span data-ttu-id="9cc97-117">Tato služba přijímá argumenty 4 a vypočítá ETS prognózy.</span><span class="sxs-lookup"><span data-stu-id="9cc97-117">This service accepts 4 arguments and calculates the ETS forecasts.</span></span>
<span data-ttu-id="9cc97-118">Vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="9cc97-118">The input arguments are:</span></span>

* <span data-ttu-id="9cc97-119">Frekvence - určuje četnost (denně nebo týdně nebo měsíční nebo čtvrtletně nebo ročně) nezpracovaná data.</span><span class="sxs-lookup"><span data-stu-id="9cc97-119">Frequency - Indicates the frequency of the raw data (daily/weekly/monthly/quarterly/yearly).</span></span>
* <span data-ttu-id="9cc97-120">Horizon - budoucí prognózy časového rámce.</span><span class="sxs-lookup"><span data-stu-id="9cc97-120">Horizon - Future forecast time-frame.</span></span>
* <span data-ttu-id="9cc97-121">Date – přidání v nová data řady čas dobu.</span><span class="sxs-lookup"><span data-stu-id="9cc97-121">Date - Add in the new time series data for time.</span></span>
* <span data-ttu-id="9cc97-122">Hodnota – přidejte do nové časových řad datové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9cc97-122">Value - Add in the new time series data values.</span></span>

<span data-ttu-id="9cc97-123">Výstup služby je počítané hodnoty prognózy.</span><span class="sxs-lookup"><span data-stu-id="9cc97-123">The output of the service is the calculated forecast values.</span></span>

<span data-ttu-id="9cc97-124">Ukázka vstup může být:</span><span class="sxs-lookup"><span data-stu-id="9cc97-124">Sample input could be:</span></span> 

* <span data-ttu-id="9cc97-125">Frekvence - 12</span><span class="sxs-lookup"><span data-stu-id="9cc97-125">Frequency - 12</span></span>
* <span data-ttu-id="9cc97-126">Horizon - 12</span><span class="sxs-lookup"><span data-stu-id="9cc97-126">Horizon - 12</span></span>
* <span data-ttu-id="9cc97-127">Datum - 1, 15 nebo 2012, 2, 15 nebo 2012, 3, 15 nebo 2012; 4, 15 nebo 2012; 5 nebo 15/2012; 6/15/2012; 15/7/2012, 8 / 15/2012, 9, 15 nebo 2012, 10, 15 nebo 2012; 11/15/2012, 12/15/2012; 1, 15 nebo 2013, 2, 15 nebo 2013, 3, 15 nebo 2013; 4, 15 nebo 2013; 5 nebo 15/2013; 6, 15 nebo 2013; 7, 15 nebo 2013, 8 / 15/2013, 9, 15 nebo 2013, 10, 15 nebo 2013; 11, 15 nebo 2013, 12/15/2013; 1, 15 nebo 2014, 2, 15 nebo 2014, 3, 15 nebo 2014; 4/15/2014; 5 nebo 15/2014; 6/15/2014; 7, 15 nebo 2014, 8 / 15/2014; 9, 15 nebo 2014</span><span class="sxs-lookup"><span data-stu-id="9cc97-127">Date - 1/15/2012;2/15/2012;3/15/2012;4/15/2012;5/15/2012;6/15/2012;7/15/2012;8/15/2012;9/15/2012;10/15/2012;11/15/2012;12/15/2012; 1/15/2013;2/15/2013;3/15/2013;4/15/2013;5/15/2013;6/15/2013;7/15/2013;8/15/2013;9/15/2013;10/15/2013;11/15/2013;12/15/2013; 1/15/2014;2/15/2014;3/15/2014;4/15/2014;5/15/2014;6/15/2014;7/15/2014;8/15/2014;9/15/2014</span></span>
* <span data-ttu-id="9cc97-128">Hodnota – 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509</span><span class="sxs-lookup"><span data-stu-id="9cc97-128">Value - 3.479;3.68;3.832;3.941;3.797;3.586;3.508;3.731;3.915;3.844;3.634;3.549;3.557;3.785;3.782;3.601;3.544;3.556;3.65;3.709;3.682;3.511; 3.429;3.51;3.523;3.525;3.626;3.695;3.711;3.711;3.693;3.571;3.509</span></span>

> <span data-ttu-id="9cc97-129">Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="9cc97-129">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="9cc97-130">Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/etsForecasting.aspx)).</span><span class="sxs-lookup"><span data-stu-id="9cc97-130">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/etsForecasting.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="9cc97-131">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="9cc97-131">Starting C# code for web service consumption:</span></span>
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
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }



## <a name="creation-of-web-service"></a><span data-ttu-id="9cc97-132">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="9cc97-132">Creation of web service</span></span>
> <span data-ttu-id="9cc97-133">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9cc97-133">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="9cc97-134">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="9cc97-134">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="9cc97-135">Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="9cc97-135">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="9cc97-136">Z v rámci Azure Machine Learning, která vytvořila nový prázdný experiment.</span><span class="sxs-lookup"><span data-stu-id="9cc97-136">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="9cc97-137">Ukázková vstupní data byla nahrána s předem definovaných datových schématu.</span><span class="sxs-lookup"><span data-stu-id="9cc97-137">Sample input data was uploaded with a predefined data schema.</span></span> <span data-ttu-id="9cc97-138">Propojen s daty schéma je [spustit skript jazyka R] [ execute-r-script] modul, který generuje ETS prognózy modelu s použitím funkce 'ets' a 'prognózy, z R.</span><span class="sxs-lookup"><span data-stu-id="9cc97-138">Linked to the data schema is an [Execute R Script][execute-r-script] module that generates the ETS forecasting model by using ‘ets’ and ‘forecast’ functions from R.</span></span> 

![Tok experimentu][2]

#### <a name="module-1"></a><span data-ttu-id="9cc97-140">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="9cc97-140">Module 1:</span></span>
    # Add in the CSV file with the data in the format shown below 
![Vzorek dat][3]    

#### <a name="module-2"></a><span data-ttu-id="9cc97-142">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="9cc97-142">Module 2:</span></span>
    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)

    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]

    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)

    # Fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- ets(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)

    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")

    # Data output
    maml.mapOutputPort("data.forecast");


## <a name="limitations"></a><span data-ttu-id="9cc97-143">Omezení</span><span class="sxs-lookup"><span data-stu-id="9cc97-143">Limitations</span></span>
<span data-ttu-id="9cc97-144">Toto je velmi jednoduchý příklad pro ETS prognózy.</span><span class="sxs-lookup"><span data-stu-id="9cc97-144">This is a very simple example for ETS forecasting.</span></span> <span data-ttu-id="9cc97-145">Jak je vidět z výše uvedený ukázkový kód, žádné chyby zachytávání je implementována a službu předpokládá, že jsou všechny proměnné průběžné nebo kladnou hodnoty a frekvenci musí být celé číslo větší než 1.</span><span class="sxs-lookup"><span data-stu-id="9cc97-145">As can be seen from the example code above, no error catching is implemented, and the service assumes that all the variables are continuous/positive values and the frequency should be an integer greater than 1.</span></span> <span data-ttu-id="9cc97-146">Délka hodnotu data a vektory musí být stejná.</span><span class="sxs-lookup"><span data-stu-id="9cc97-146">The length of the date and value vectors should be the same.</span></span> <span data-ttu-id="9cc97-147">Proměnnou datum by měl splňovat ve formátu ' mm/dd/rrrr'.</span><span class="sxs-lookup"><span data-stu-id="9cc97-147">The date variable should adhere to the format ‘mm/dd/yyyy’.</span></span>

## <a name="faq"></a><span data-ttu-id="9cc97-148">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="9cc97-148">FAQ</span></span>
<span data-ttu-id="9cc97-149">Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="9cc97-149">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img1.png
[2]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img2.png
[3]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
