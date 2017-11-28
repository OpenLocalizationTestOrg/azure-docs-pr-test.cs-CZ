---
title: "(nepoužívané) Rozdíl v testu proporce - Azure | Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: a08f91ca76eef2562caeb9eb64cec5e549ed2f5f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-difference-in-proportions-test"></a><span data-ttu-id="8d644-103">(nepoužívané) Rozdíl v proporce testu</span><span class="sxs-lookup"><span data-stu-id="8d644-103">(deprecated) Difference in Proportions Test</span></span>

> [!NOTE]
> <span data-ttu-id="8d644-104">Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="8d644-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="8d644-105">Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="8d644-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="8d644-106">Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="8d644-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="8d644-107">Jsou dvě proporce statisticky různých?</span><span class="sxs-lookup"><span data-stu-id="8d644-107">Are two proportions statistically different?</span></span> <span data-ttu-id="8d644-108">Předpokládejme, že uživatel chce k porovnání dvou filmy určit, pokud má jedna film výrazně vyšší poměr 'líbí, při porovnání na druhý.</span><span class="sxs-lookup"><span data-stu-id="8d644-108">Suppose a user wants to compare two movies to determine if one movie has a significantly higher proportion of ‘likes’ when compared to the other.</span></span> <span data-ttu-id="8d644-109">S ukázkovým velké může být statisticky velký rozdíl mezi proporce 0,50 a 0,51.</span><span class="sxs-lookup"><span data-stu-id="8d644-109">With a large sample, there could be a statistically significant difference between the proportions 0.50 and 0.51.</span></span> <span data-ttu-id="8d644-110">Pomocí malé ukázkové nemusí být dostatek dat. určí, jestli je ve skutečnosti různých tyto poměr stran.</span><span class="sxs-lookup"><span data-stu-id="8d644-110">With a small sample, there may not be enough data to determine if these proportions are actually different.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="8d644-111">To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/prop_test) provádí předpoklad testu rozdíl ve dvou hodnot založena na vstup uživatele počet úspěchů a celkový počet pokusů pro skupiny 2 porovnání.</span><span class="sxs-lookup"><span data-stu-id="8d644-111">This [web service](https://datamarket.azure.com/dataset/aml_labs/prop_test) conducts a hypothesis test of the difference in two proportions based on user input of the number of successes and the total number of trials for the 2 comparison groups.</span></span> <span data-ttu-id="8d644-112">V jeden z možných scénářů této webové služby může být volána v rámci filmová porovnání aplikace, která uživatele vyzve, zda mezi filmy je skutečně 'líbilo' častěji než druhý, na základě film hodnocení.</span><span class="sxs-lookup"><span data-stu-id="8d644-112">In one possible scenario, this web service could be called from within a movie comparison app, telling the user whether one of the movies is really ‘liked’ more often than the other, based on movie ratings.</span></span>

> <span data-ttu-id="8d644-113">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="8d644-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="8d644-114">Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="8d644-114">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="8d644-115">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="8d644-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="8d644-116">Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="8d644-116">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="8d644-117">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="8d644-117">Consumption of web service</span></span>
<span data-ttu-id="8d644-118">Tato služba přijímá argumenty 4 a nepodporuje testování předpoklad z poměr stran.</span><span class="sxs-lookup"><span data-stu-id="8d644-118">This service accepts 4 arguments and does a hypothesis test of proportions.</span></span>

<span data-ttu-id="8d644-119">Vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="8d644-119">The input arguments are:</span></span>

* <span data-ttu-id="8d644-120">Successes1 - počet událostí úspěšného v ukázce 1.</span><span class="sxs-lookup"><span data-stu-id="8d644-120">Successes1 - Number of success events in sample 1.</span></span>
* <span data-ttu-id="8d644-121">Successes2 - počet událostí úspěšného v ukázce 2.</span><span class="sxs-lookup"><span data-stu-id="8d644-121">Successes2 - Number of success events in sample 2.</span></span>
* <span data-ttu-id="8d644-122">Total1 – velikost vzorku 1.</span><span class="sxs-lookup"><span data-stu-id="8d644-122">Total1 -  Size of sample 1.</span></span>
* <span data-ttu-id="8d644-123">Total2 – velikost vzorku 2.</span><span class="sxs-lookup"><span data-stu-id="8d644-123">Total2 - Size of sample 2.</span></span>

<span data-ttu-id="8d644-124">Výstup služby je výsledkem předpoklad testování společně s chi-square statistiky, df, p hodnota a deklarovaná čistá nebo hrubá v ukázkové 1/2 a interval spolehlivosti hranice.</span><span class="sxs-lookup"><span data-stu-id="8d644-124">The output of the service is the result of the hypothesis test along with the chi-square statistic, df, p-value, and proportion in sample 1/2 and confidence interval bounds.</span></span>

> <span data-ttu-id="8d644-125">Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="8d644-125">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="8d644-126">Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).</span><span class="sxs-lookup"><span data-stu-id="8d644-126">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="8d644-127">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="8d644-127">Starting C# code for web service consumption:</span></span>
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


## <a name="creation-of-web-service"></a><span data-ttu-id="8d644-128">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="8d644-128">Creation of web service</span></span>
> <span data-ttu-id="8d644-129">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="8d644-129">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="8d644-130">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="8d644-130">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="8d644-131">Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="8d644-131">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="8d644-132">Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen se dvěma [spustit skript jazyka R] [ execute-r-script] moduly.</span><span class="sxs-lookup"><span data-stu-id="8d644-132">From within Azure Machine Learning, a new blank experiment was created with two [Execute R Script][execute-r-script] modules.</span></span> <span data-ttu-id="8d644-133">V první modul, který je definován schéma dat zatímco druhý modul používá příkaz prop.test v rámci R Pokud chcete provést test předpoklad 2 poměr stran.</span><span class="sxs-lookup"><span data-stu-id="8d644-133">In the first module the data schema is defined, while the second module uses the prop.test command within R to perform the hypothesis test for 2 proportions.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="8d644-134">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="8d644-134">Experiment flow:</span></span>
![Tok experimentu][2]

#### <a name="module-1"></a><span data-ttu-id="8d644-136">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="8d644-136">Module 1:</span></span>
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port


#### <a name="module-2"></a><span data-ttu-id="8d644-137">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="8d644-137">Module 2:</span></span>
    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port


## <a name="limitations"></a><span data-ttu-id="8d644-138">Omezení</span><span class="sxs-lookup"><span data-stu-id="8d644-138">Limitations</span></span>
<span data-ttu-id="8d644-139">Toto je velmi jednoduchý příklad pro test rozdíl v poměru 2.</span><span class="sxs-lookup"><span data-stu-id="8d644-139">This is a very simple example for a test of difference in 2 proportions.</span></span> <span data-ttu-id="8d644-140">Jak je vidět z výše uvedený ukázkový kód, je implementovaný zachycení žádné chyby a službu předpokládá, že se průběžné všech proměnných.</span><span class="sxs-lookup"><span data-stu-id="8d644-140">As can be seen from the example code above, no error catching is implemented and the service assumes that all the variables are continuous.</span></span>

## <a name="faq"></a><span data-ttu-id="8d644-141">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="8d644-141">FAQ</span></span>
<span data-ttu-id="8d644-142">Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="8d644-142">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

