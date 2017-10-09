---
title: "AAA(Deprecated) rozdíl v testu proporce - Azure | Microsoft Docs"
description: "(nepoužívané) Rozdíl v proporce testu"
services: machine-learning
documentationcenter: 
author: aniedea
manager: jhubbard
editor: cgronlun
ms.assetid: 9356b821-5345-44f6-8e26-719f2dea5e6d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: aniedea
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 820aad377f9dec12b0ef455974aaa95f6e8d723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-difference-in-proportions-test"></a><span data-ttu-id="4d0b1-103">(nepoužívané) Rozdíl v proporce testu</span><span class="sxs-lookup"><span data-stu-id="4d0b1-103">(deprecated) Difference in Proportions Test</span></span>

> [!NOTE]
> <span data-ttu-id="4d0b1-104">Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="4d0b1-105">Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="4d0b1-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="4d0b1-106">Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="4d0b1-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="4d0b1-107">Jsou dvě proporce statisticky různých?</span><span class="sxs-lookup"><span data-stu-id="4d0b1-107">Are two proportions statistically different?</span></span> <span data-ttu-id="4d0b1-108">Předpokládejme, že uživatel chce toodetermine toocompare dva filmy, pokud jeden film má podstatně vyšší poměr 'líbí, při porovnání toohello jiné.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-108">Suppose a user wants toocompare two movies toodetermine if one movie has a significantly higher proportion of ‘likes’ when compared toohello other.</span></span> <span data-ttu-id="4d0b1-109">S ukázkovým velké může být statisticky velký rozdíl mezi proporce hello 0,50 a 0,51.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-109">With a large sample, there could be a statistically significant difference between hello proportions 0.50 and 0.51.</span></span> <span data-ttu-id="4d0b1-110">Pomocí malé ukázkové pravděpodobně není dostatek dat toodetermine Pokud tyto proporce ve skutečnosti liší.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-110">With a small sample, there may not be enough data toodetermine if these proportions are actually different.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="4d0b1-111">To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/prop_test) provádí předpoklad testu hello rozdíl dvou hodnot založena na vstup uživatele hello počet úspěchů a celkový počet pokusů pro skupiny porovnání hello 2 hello.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-111">This [web service](https://datamarket.azure.com/dataset/aml_labs/prop_test) conducts a hypothesis test of hello difference in two proportions based on user input of hello number of successes and hello total number of trials for hello 2 comparison groups.</span></span> <span data-ttu-id="4d0b1-112">V jeden z možných scénářů této webové služby může být volána v rámci filmová porovnání aplikace hello uživatele informuje, zda jeden filmy hello je skutečně 'líbilo' častěji než hello jiných, na základě film hodnocení.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-112">In one possible scenario, this web service could be called from within a movie comparison app, telling hello user whether one of hello movies is really ‘liked’ more often than hello other, based on movie ratings.</span></span>

> <span data-ttu-id="4d0b1-113">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="4d0b1-114">Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-114">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="4d0b1-115">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="4d0b1-116">Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-116">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="4d0b1-117">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="4d0b1-117">Consumption of web service</span></span>
<span data-ttu-id="4d0b1-118">Tato služba přijímá argumenty 4 a nepodporuje testování předpoklad z poměr stran.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-118">This service accepts 4 arguments and does a hypothesis test of proportions.</span></span>

<span data-ttu-id="4d0b1-119">Hello vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="4d0b1-119">hello input arguments are:</span></span>

* <span data-ttu-id="4d0b1-120">Successes1 - počet událostí úspěšného v ukázce 1.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-120">Successes1 - Number of success events in sample 1.</span></span>
* <span data-ttu-id="4d0b1-121">Successes2 - počet událostí úspěšného v ukázce 2.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-121">Successes2 - Number of success events in sample 2.</span></span>
* <span data-ttu-id="4d0b1-122">Total1 – velikost vzorku 1.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-122">Total1 -  Size of sample 1.</span></span>
* <span data-ttu-id="4d0b1-123">Total2 – velikost vzorku 2.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-123">Total2 - Size of sample 2.</span></span>

<span data-ttu-id="4d0b1-124">výstup Hello hello služby je hello výsledek hello předpoklad testování společně s hello chi-square statistiky, df, p hodnota a deklarovaná čistá nebo hrubá v ukázkové 1/2 a interval spolehlivosti hranice.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-124">hello output of hello service is hello result of hello hypothesis test along with hello chi-square statistic, df, p-value, and proportion in sample 1/2 and confidence interval bounds.</span></span>

> <span data-ttu-id="4d0b1-125">Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-125">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="4d0b1-126">Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).</span><span class="sxs-lookup"><span data-stu-id="4d0b1-126">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="4d0b1-127">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="4d0b1-127">Starting C# code for web service consumption:</span></span>
    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


## <a name="creation-of-web-service"></a><span data-ttu-id="4d0b1-128">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="4d0b1-128">Creation of web service</span></span>
> <span data-ttu-id="4d0b1-129">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-129">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="4d0b1-130">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="4d0b1-130">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="4d0b1-131">Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-131">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="4d0b1-132">Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen se dvěma [spustit skript jazyka R] [ execute-r-script] moduly.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-132">From within Azure Machine Learning, a new blank experiment was created with two [Execute R Script][execute-r-script] modules.</span></span> <span data-ttu-id="4d0b1-133">V modulu první hello je definována schéma dat hello, druhý modul hello používá příkaz prop.test hello v rámci R tooperform hello předpoklad testovací k 2 poměr stran.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-133">In hello first module hello data schema is defined, while hello second module uses hello prop.test command within R tooperform hello hypothesis test for 2 proportions.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="4d0b1-134">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="4d0b1-134">Experiment flow:</span></span>
![Tok experimentu][2]

#### <a name="module-1"></a><span data-ttu-id="4d0b1-136">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="4d0b1-136">Module 1:</span></span>
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data toooutput port
    dataset1 = maml.mapInputPort(1) #read data from input port


#### <a name="module-2"></a><span data-ttu-id="4d0b1-137">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="4d0b1-137">Module 2:</span></span>
    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"hello proportions are different!",
    "hello proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port


## <a name="limitations"></a><span data-ttu-id="4d0b1-138">Omezení</span><span class="sxs-lookup"><span data-stu-id="4d0b1-138">Limitations</span></span>
<span data-ttu-id="4d0b1-139">Toto je velmi jednoduchý příklad pro test rozdíl v poměru 2.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-139">This is a very simple example for a test of difference in 2 proportions.</span></span> <span data-ttu-id="4d0b1-140">Jak je vidět z výše uvedený kód příklad hello, je implementovaný zachycení žádné chyby a hello služby předpokládá, že se všechny proměnné hello průběžné.</span><span class="sxs-lookup"><span data-stu-id="4d0b1-140">As can be seen from hello example code above, no error catching is implemented and hello service assumes that all hello variables are continuous.</span></span>

## <a name="faq"></a><span data-ttu-id="4d0b1-141">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="4d0b1-141">FAQ</span></span>
<span data-ttu-id="4d0b1-142">Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="4d0b1-142">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

