---
title: "(nepoužívané) Multivariační lineární regrese - Azure | Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 65a8005139e920cd19593e954fc1bf836354bdf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-multivariate-linear-regression"></a><span data-ttu-id="a8e08-103">(nepoužívané) Multivariační lineární regrese</span><span class="sxs-lookup"><span data-stu-id="a8e08-103">(deprecated) Multivariate Linear Regression</span></span>

> [!NOTE]
> <span data-ttu-id="a8e08-104">Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="a8e08-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="a8e08-105">Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="a8e08-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="a8e08-106">Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="a8e08-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="a8e08-107">Předpokládejme, že máte datovou sadu a chcete rychle předpovědi závislé proměnné (y) pro každého uživatele (i), na základě nezávislých proměnných.</span><span class="sxs-lookup"><span data-stu-id="a8e08-107">Suppose you have a dataset and would like to quickly predict a dependent variable (y) for each individual (i) based on independent variables.</span></span> <span data-ttu-id="a8e08-108">Lineární regrese je Oblíbené statistické technika pro takové předpovědi.</span><span class="sxs-lookup"><span data-stu-id="a8e08-108">Linear regression is a popular statistical technique used for such predictions.</span></span> <span data-ttu-id="a8e08-109">Zde se předpokládá y závislé proměnné, jako hodnotu nepřetržitá.</span><span class="sxs-lookup"><span data-stu-id="a8e08-109">Here the dependent variable y is assumed to be a continuous value.</span></span>  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="a8e08-110">Může být jednoduchého scénáře, kde zkušeností pokouší předvídat váhu konkrétního (y) podle jejich výška (x).</span><span class="sxs-lookup"><span data-stu-id="a8e08-110">A simple scenario could be where the researcher is trying to predict the weight of an individual (y) based on their height (x).</span></span> <span data-ttu-id="a8e08-111">Může být pokročilejších scénářích, kde zkušeností Další informace o jednotlivých (například váhy, pohlaví, soupeření) a pokusí se předpovědi váhu jednotlivých.</span><span class="sxs-lookup"><span data-stu-id="a8e08-111">A more advanced scenario could be where the researcher has additional information for the individual (such as weight, gender, race) and attempts to predict the weight of the individual.</span></span> <span data-ttu-id="a8e08-112">To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) vyhovuje model lineární regrese na data a výstupy předpovězené hodnoty (y) pro každý pozorování v datech.</span><span class="sxs-lookup"><span data-stu-id="a8e08-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) fits the linear regression model to the data and outputs the predicted value (y) for each of the observations in the data.</span></span>

> <span data-ttu-id="a8e08-113">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="a8e08-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="a8e08-114">Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="a8e08-114">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="a8e08-115">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="a8e08-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="a8e08-116">Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="a8e08-116">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="a8e08-117">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="a8e08-117">Consumption of web service</span></span>
<span data-ttu-id="a8e08-118">Tato webová služba poskytuje předpovězené hodnoty proměnné závislé na základě nezávislých proměnných pro všechny připomínky.</span><span class="sxs-lookup"><span data-stu-id="a8e08-118">This web service gives the predicted values of the dependent variable based on the independent variables for all of the observations.</span></span> <span data-ttu-id="a8e08-119">Webová služba očekává, že koncovému uživateli umožňují vstupní data jako řetězec, kde jsou řádky oddělených čárkami (,) a sloupce jsou odděleny středníkem (;).</span><span class="sxs-lookup"><span data-stu-id="a8e08-119">The web service expects the end user to input data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="a8e08-120">Webová služba očekává 1 řádek v čase a očekává první sloupec jako závislé proměnné.</span><span class="sxs-lookup"><span data-stu-id="a8e08-120">The web service expects 1 row at a time and expects the first column to be the dependent variable.</span></span> <span data-ttu-id="a8e08-121">Datové sadě služby příkladu může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="a8e08-121">An example dataset could look like this:</span></span>

![Ukázková data][1]

<span data-ttu-id="a8e08-123">Připomínky bez závislé proměnné by měl zadejte jako hodnotu "NA" pro y.</span><span class="sxs-lookup"><span data-stu-id="a8e08-123">Observations without a dependent variable should be input as “NA” for y.</span></span> <span data-ttu-id="a8e08-124">Data vstup pro výše uvedené datová sada by být následující řetězec: "10, 5, 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2, 1, NA, 3, 4".</span><span class="sxs-lookup"><span data-stu-id="a8e08-124">The data input for the above dataset would be the following string: “10;5;2,18;1;6,6;5.3;2.1,7;5;5,22;3;4,12;2;1,NA;3;4”.</span></span> <span data-ttu-id="a8e08-125">Výstup je předpovězené hodnoty pro každý z jeho řádků na základě nezávislých proměnných.</span><span class="sxs-lookup"><span data-stu-id="a8e08-125">The output is the predicted value for each of the rows based on the independent variables.</span></span> 

> <span data-ttu-id="a8e08-126">Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="a8e08-126">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="a8e08-127">Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).</span><span class="sxs-lookup"><span data-stu-id="a8e08-127">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="a8e08-128">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="a8e08-128">Starting C# code for web service consumption:</span></span>
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




## <a name="creation-of-web-service"></a><span data-ttu-id="a8e08-129">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="a8e08-129">Creation of web service</span></span>
> <span data-ttu-id="a8e08-130">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="a8e08-130">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="a8e08-131">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="a8e08-131">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="a8e08-132">Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="a8e08-132">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="a8e08-133">Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly byly vyžádány na pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="a8e08-133">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules were pulled onto the workspace.</span></span> <span data-ttu-id="a8e08-134">Tato webová služba běží experimentu Azure Machine Learning s základní skript jazyka R.</span><span class="sxs-lookup"><span data-stu-id="a8e08-134">This web service runs an Azure Machine Learning experiment with an underlying R script.</span></span> <span data-ttu-id="a8e08-135">Existují 2 částí do tohoto experimentu: definice schématu a školení modelu + vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="a8e08-135">There are 2 parts to this experiment: schema definition, and training model + scoring.</span></span> <span data-ttu-id="a8e08-136">První modul definuje vstupní datové sady, kde je první proměnná závislé proměnné a zbývající proměnné jsou nezávislé na očekávané struktuře.</span><span class="sxs-lookup"><span data-stu-id="a8e08-136">The first module defines the expected structure of the input dataset, where the first variable is the dependent variable and the remaining variables are independent.</span></span> <span data-ttu-id="a8e08-137">Druhý modul vyhovuje model lineární regrese obecný pro vstupní data.</span><span class="sxs-lookup"><span data-stu-id="a8e08-137">The second module fits a generic linear regression model for the input data.</span></span>  

![Tok experimentu][3]

#### <a name="module-1"></a><span data-ttu-id="a8e08-139">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="a8e08-139">Module 1:</span></span>
#### <a name="schema-definition"></a><span data-ttu-id="a8e08-140">Definice schématu</span><span class="sxs-lookup"><span data-stu-id="a8e08-140">Schema definition</span></span>
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

#### <a name="module-2"></a><span data-ttu-id="a8e08-141">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="a8e08-141">Module 2:</span></span>
#### <a name="lm-modeling"></a><span data-ttu-id="a8e08-142">Modelování LM</span><span class="sxs-lookup"><span data-stu-id="a8e08-142">LM modeling</span></span>
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

## <a name="limitations"></a><span data-ttu-id="a8e08-143">Omezení</span><span class="sxs-lookup"><span data-stu-id="a8e08-143">Limitations</span></span>
<span data-ttu-id="a8e08-144">Toto je velmi jednoduchý příklad více lineární regrese webové služby.</span><span class="sxs-lookup"><span data-stu-id="a8e08-144">This is a very simple example of a multiple linear regression web service.</span></span> <span data-ttu-id="a8e08-145">Jak je vidět z výše uvedeném příkladu kódu, je implementovaný zachycení žádné chyby a službu předpokládá, že všechno, co je nepřetržitá proměnné (bez kategorií funkcí povoleny), jako služba číselné hodnoty vstupy pouze v době vytvoření této webové služby.</span><span class="sxs-lookup"><span data-stu-id="a8e08-145">As can be seen from the example code above, no error catching is implemented and the service assumes everything is a continuous variable (no categorical features allowed), as the service only inputs numeric values at the time of the creation of this web service.</span></span> <span data-ttu-id="a8e08-146">Navíc služba aktuálně zpracovává velikost omezenou dat, vyřízení požadavků a odpovědí povaze volání webové služby a fakt, že je probíhá podle modelu pokaždé, když je volat webovou službu.</span><span class="sxs-lookup"><span data-stu-id="a8e08-146">Also, the service currently handles limited data size, due to the request/response nature of the web service call and the fact that the model is being fit every time the web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="a8e08-147">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="a8e08-147">FAQ</span></span>
<span data-ttu-id="a8e08-148">Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="a8e08-148">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

