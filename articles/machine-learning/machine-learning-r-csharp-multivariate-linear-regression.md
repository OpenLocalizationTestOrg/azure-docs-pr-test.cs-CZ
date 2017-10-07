---
title: "AAA(Deprecated) Multivariate lineární regrese - Azure | Microsoft Docs"
description: "(nepoužívané) Multivariační lineární regrese"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 2fb78220-ced9-4564-a439-9e5df6772994
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: jaymathe
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 0ff7221cd06c0ef059b0c5bf327016588174dcfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-multivariate-linear-regression"></a><span data-ttu-id="5ee4d-103">(nepoužívané) Multivariační lineární regrese</span><span class="sxs-lookup"><span data-stu-id="5ee4d-103">(deprecated) Multivariate Linear Regression</span></span>

> [!NOTE]
> <span data-ttu-id="5ee4d-104">Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="5ee4d-105">Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="5ee4d-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="5ee4d-106">Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="5ee4d-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="5ee4d-107">Předpokládejme, že máte datovou sadu a by jako tooquickly předpovědi závislé proměnné (y) pro každého uživatele (i), na základě nezávislých proměnných.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-107">Suppose you have a dataset and would like tooquickly predict a dependent variable (y) for each individual (i) based on independent variables.</span></span> <span data-ttu-id="5ee4d-108">Lineární regrese je Oblíbené statistické technika pro takové předpovědi.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-108">Linear regression is a popular statistical technique used for such predictions.</span></span> <span data-ttu-id="5ee4d-109">Hello závislé proměnné y tady se předpokládá, že toobe průběžné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-109">Here hello dependent variable y is assumed toobe a continuous value.</span></span>  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="5ee4d-110">Může být jednoduchý scénář, kde hello zkušeností se pokouší toopredict hello váhu konkrétního (y) podle jejich výška (x).</span><span class="sxs-lookup"><span data-stu-id="5ee4d-110">A simple scenario could be where hello researcher is trying toopredict hello weight of an individual (y) based on their height (x).</span></span> <span data-ttu-id="5ee4d-111">Pokročilejších scénářích může být, kde hello zkušeností Další informace o jednotlivých (například váhy, pohlaví, soupeření) hello a pokusí toopredict hello váhu individuální hello.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-111">A more advanced scenario could be where hello researcher has additional information for hello individual (such as weight, gender, race) and attempts toopredict hello weight of hello individual.</span></span> <span data-ttu-id="5ee4d-112">To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) pro rozlišení hello data toohello model lineární regrese a výstupy hello předpovězené hodnoty (y) pro každý hello pozorování v datech hello.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) fits hello linear regression model toohello data and outputs hello predicted value (y) for each of hello observations in hello data.</span></span>

> <span data-ttu-id="5ee4d-113">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="5ee4d-114">Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-114">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="5ee4d-115">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="5ee4d-116">Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-116">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="5ee4d-117">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="5ee4d-117">Consumption of web service</span></span>
<span data-ttu-id="5ee4d-118">Tento web service poskytuje hello předpovědět hodnoty proměnné hello závislé na základě nezávislých proměnných hello všech hello připomínky.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-118">This web service gives hello predicted values of hello dependent variable based on hello independent variables for all of hello observations.</span></span> <span data-ttu-id="5ee4d-119">Webová služba Hello očekává hello koncový uživatel tooinput data jako řetězec, kde jsou řádky oddělených čárkami (,) a sloupce jsou odděleny středníkem (;).</span><span class="sxs-lookup"><span data-stu-id="5ee4d-119">hello web service expects hello end user tooinput data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="5ee4d-120">Webová služba Hello očekává 1 řádek v čase a očekává hello první sloupec toobe hello závislé proměnné.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-120">hello web service expects 1 row at a time and expects hello first column toobe hello dependent variable.</span></span> <span data-ttu-id="5ee4d-121">Datové sadě služby příkladu může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="5ee4d-121">An example dataset could look like this:</span></span>

![Ukázková data][1]

<span data-ttu-id="5ee4d-123">Připomínky bez závislé proměnné by měl zadejte jako hodnotu "NA" pro y.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-123">Observations without a dependent variable should be input as “NA” for y.</span></span> <span data-ttu-id="5ee4d-124">Hello vstupních dat pro hello výše datová sada by být hello následující řetězec: "10, 5, 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2, 1, NA, 3, 4".</span><span class="sxs-lookup"><span data-stu-id="5ee4d-124">hello data input for hello above dataset would be hello following string: “10;5;2,18;1;6,6;5.3;2.1,7;5;5,22;3;4,12;2;1,NA;3;4”.</span></span> <span data-ttu-id="5ee4d-125">Hello výstup hello předpovězené hodnoty pro každý hello řádků podle hello nezávislých proměnných.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-125">hello output is hello predicted value for each of hello rows based on hello independent variables.</span></span> 

> <span data-ttu-id="5ee4d-126">Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-126">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="5ee4d-127">Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).</span><span class="sxs-lookup"><span data-stu-id="5ee4d-127">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="5ee4d-128">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="5ee4d-128">Starting C# code for web service consumption:</span></span>
    public class Input
    {
            public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




## <a name="creation-of-web-service"></a><span data-ttu-id="5ee4d-129">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="5ee4d-129">Creation of web service</span></span>
> <span data-ttu-id="5ee4d-130">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-130">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="5ee4d-131">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="5ee4d-131">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="5ee4d-132">Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-132">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="5ee4d-133">Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly byly vyžádány do pracovního prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-133">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules were pulled onto hello workspace.</span></span> <span data-ttu-id="5ee4d-134">Tato webová služba běží experimentu Azure Machine Learning s základní skript jazyka R.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-134">This web service runs an Azure Machine Learning experiment with an underlying R script.</span></span> <span data-ttu-id="5ee4d-135">Existují experimentovat toothis 2 částí: definice schématu a školení modelu + vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-135">There are 2 parts toothis experiment: schema definition, and training model + scoring.</span></span> <span data-ttu-id="5ee4d-136">první modul Hello definuje hello očekávána struktura hello vstupní datové sady, kde první proměnná hello je hello závislé proměnné a hello zbývající proměnné jsou nezávislé.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-136">hello first module defines hello expected structure of hello input dataset, where hello first variable is hello dependent variable and hello remaining variables are independent.</span></span> <span data-ttu-id="5ee4d-137">druhý modul Hello vyhovuje model lineární regrese obecný pro vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-137">hello second module fits a generic linear regression model for hello input data.</span></span>  

![Tok experimentu][3]

#### <a name="module-1"></a><span data-ttu-id="5ee4d-139">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="5ee4d-139">Module 1:</span></span>
#### <a name="schema-definition"></a><span data-ttu-id="5ee4d-140">Definice schématu</span><span class="sxs-lookup"><span data-stu-id="5ee4d-140">Schema definition</span></span>
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

#### <a name="module-2"></a><span data-ttu-id="5ee4d-141">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="5ee4d-141">Module 2:</span></span>
#### <a name="lm-modeling"></a><span data-ttu-id="5ee4d-142">Modelování LM</span><span class="sxs-lookup"><span data-stu-id="5ee4d-142">LM modeling</span></span>
    data <- maml.mapInputPort(1) # class: data.frame  

    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  

## <a name="limitations"></a><span data-ttu-id="5ee4d-143">Omezení</span><span class="sxs-lookup"><span data-stu-id="5ee4d-143">Limitations</span></span>
<span data-ttu-id="5ee4d-144">Toto je velmi jednoduchý příklad více lineární regrese webové služby.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-144">This is a very simple example of a multiple linear regression web service.</span></span> <span data-ttu-id="5ee4d-145">Jak je vidět z hello ukázkový kód výše, je implementována zachycení žádné chyby a hello služby předpokládá, že všechno, co je nepřetržitá proměnné (bez kategorií funkcí povoleny), jako hello služby pouze vstupy číselných hodnot v době hello hello vytvoření tomuto webovému Služba.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-145">As can be seen from hello example code above, no error catching is implemented and hello service assumes everything is a continuous variable (no categorical features allowed), as hello service only inputs numeric values at hello time of hello creation of this web service.</span></span> <span data-ttu-id="5ee4d-146">Navíc služba hello aktuálně zpracovává velikost omezenou dat, z důvodu toohello požadavků a odpovědí povaha hello webové služby je se volání a hello skutečnost, že hello modelu odpovídat pokaždé, když je volána hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="5ee4d-146">Also, hello service currently handles limited data size, due toohello request/response nature of hello web service call and hello fact that hello model is being fit every time hello web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="5ee4d-147">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="5ee4d-147">FAQ</span></span>
<span data-ttu-id="5ee4d-148">Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="5ee4d-148">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

