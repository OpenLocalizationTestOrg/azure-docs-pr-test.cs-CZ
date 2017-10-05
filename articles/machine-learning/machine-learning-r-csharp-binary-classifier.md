---
title: "(nepoužívané) Binární třídění - Azure | Microsoft Docs"
description: "(nepoužívané) Binární třídění"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 8045038a-9dcf-44b9-a6de-7f1f8e791575
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
ms.openlocfilehash: 1a83392f90bb5a9fb183334c03ccec20dd3f3520
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-binary-classifier"></a><span data-ttu-id="cff55-103">(nepoužívané) Binární třídění</span><span class="sxs-lookup"><span data-stu-id="cff55-103">(deprecated) Binary Classifier</span></span>

> [!NOTE]
> <span data-ttu-id="cff55-104">Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="cff55-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="cff55-105">Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="cff55-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="cff55-106">Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="cff55-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="cff55-107">Předpokládejme, že už máte datovou sadu a chcete předpovídat proměnnou binární závislé na základě nezávislých proměnných.</span><span class="sxs-lookup"><span data-stu-id="cff55-107">Suppose you have a dataset and would like to predict a binary dependent variable based on the independent variables.</span></span> <span data-ttu-id="cff55-108">'Logistic Regression' je Oblíbené statistické technika pro takové předpovědi.</span><span class="sxs-lookup"><span data-stu-id="cff55-108">‘Logistic Regression’ is a popular statistical technique used for such predictions.</span></span> <span data-ttu-id="cff55-109">Tady je závislé proměnné binary nebo dichotomous a p je pravděpodobnost přítomnost vlastnosti, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="cff55-109">Here the dependent variable is binary or dichotomous, and p is the probability of presence of the characteristic of interest.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="cff55-110">Může být jednoduchého scénáře, kde výzkumným pokouší odhadnout, zda je pravděpodobné, přijmout nabídku jejich příchodu do univerzity, na základě informací (GPA střední školy, rodinného příjmu trvalé stavu, pohlaví) potenciální student.</span><span class="sxs-lookup"><span data-stu-id="cff55-110">A simple scenario could be where a researcher is trying to predict whether a prospective student is likely to accept an admission offer to a university based on information (GPA in high school, family income, resident state, gender).</span></span> <span data-ttu-id="cff55-111">Předpokládaných výsledek je pravděpodobnost potenciální student přijetím přijetí nabídky.</span><span class="sxs-lookup"><span data-stu-id="cff55-111">The predicted outcome is the probability of the prospective student accepting the admission offer.</span></span> <span data-ttu-id="cff55-112">To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/log_regression) vyhovuje logistic regresní model dat a vrací hodnotu pravděpodobnosti (y) pro každý pozorování v datech.</span><span class="sxs-lookup"><span data-stu-id="cff55-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/log_regression) fits the logistic regression model to the data and outputs the probability value (y) for each of the observations in the data.</span></span>  

> <span data-ttu-id="cff55-113">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="cff55-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="cff55-114">Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="cff55-114">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="cff55-115">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="cff55-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="cff55-116">Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="cff55-116">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="cff55-117">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="cff55-117">Consumption of web service</span></span>
<span data-ttu-id="cff55-118">Tato webová služba poskytuje předpovězené hodnoty proměnné závislé na základě nezávislých proměnných pro všechny připomínky.</span><span class="sxs-lookup"><span data-stu-id="cff55-118">This web service gives the predicted values of the dependent variable based on the independent variables for all of the observations.</span></span> <span data-ttu-id="cff55-119">Webová služba očekává, že koncovému uživateli umožňují vstupní data jako řetězec, kde řádky jsou oddělené čárkou (,) a sloupce jsou odděleny středníkem (;).</span><span class="sxs-lookup"><span data-stu-id="cff55-119">The web service expects the end user to input data as a string where rows are separated by comma (,) and columns are separated by semicolon (;).</span></span> <span data-ttu-id="cff55-120">Webová služba očekává 1 řádek v čase a očekává první sloupec jako závislé proměnné.</span><span class="sxs-lookup"><span data-stu-id="cff55-120">The web service expects 1 row at a time and expects the first column to be the dependent variable.</span></span> <span data-ttu-id="cff55-121">Datové sadě služby příkladu může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="cff55-121">An example dataset could look like this:</span></span>

![Ukázková data][1]

<span data-ttu-id="cff55-123">Připomínky bez závislé proměnné by měl zadejte jako hodnotu "NA" pro y.</span><span class="sxs-lookup"><span data-stu-id="cff55-123">Observations without a dependent variable should be input as “NA” for y.</span></span> <span data-ttu-id="cff55-124">Data vstup pro výše uvedené datová sada by být následující řetězec: "1; 5; 2,1; 1; 6,0; 5.3; 2.1,0; 5; 5,0; 3; 4,1; 2; 1, NA; 3; 4".</span><span class="sxs-lookup"><span data-stu-id="cff55-124">The data input for the above dataset would be the following string: “1;5;2,1;1;6,0;5.3;2.1,0;5;5,0;3;4,1;2;1,NA;3;4”.</span></span> <span data-ttu-id="cff55-125">Výstup je předpovězené hodnoty pro každý z jeho řádků na základě nezávislých proměnných.</span><span class="sxs-lookup"><span data-stu-id="cff55-125">The output is the predicted value for each of the rows based on the independent variables.</span></span> 

> <span data-ttu-id="cff55-126">Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="cff55-126">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="cff55-127">Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).</span><span class="sxs-lookup"><span data-stu-id="cff55-127">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="cff55-128">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="cff55-128">Starting C# code for web service consumption:</span></span>
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


## <a name="creation-of-web-service"></a><span data-ttu-id="cff55-129">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="cff55-129">Creation of web service</span></span>
> <span data-ttu-id="cff55-130">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="cff55-130">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="cff55-131">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="cff55-131">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="cff55-132">Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="cff55-132">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="cff55-133">Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly vyžádat na pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="cff55-133">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto the workspace.</span></span> <span data-ttu-id="cff55-134">Tato webová služba běží experimentu Azure Machine Learning s základní skript jazyka R.</span><span class="sxs-lookup"><span data-stu-id="cff55-134">This web service runs an Azure Machine Learning experiment with an underlying R script.</span></span> <span data-ttu-id="cff55-135">Existují 2 částí do tohoto experimentu: definice schématu a školení modelu + vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="cff55-135">There are 2 parts to this experiment: schema definition, and training model + scoring.</span></span> <span data-ttu-id="cff55-136">První modul definuje vstupní datové sady, kde je první proměnná závislé proměnné a zbývající proměnné jsou nezávislé na očekávané struktuře.</span><span class="sxs-lookup"><span data-stu-id="cff55-136">The first module defines the expected structure of the input dataset, where the first variable is the dependent variable and the remaining variables are independent.</span></span> <span data-ttu-id="cff55-137">Druhý modul vyhovuje obecné logistic regresní model pro vstupní data.</span><span class="sxs-lookup"><span data-stu-id="cff55-137">The second module fits a generic logistic regression model for the input data.</span></span>    

![Tok experimentu][2]

#### <a name="module-1"></a><span data-ttu-id="cff55-139">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="cff55-139">Module 1:</span></span>
    #Schema definition  
    data <- data.frame(value = "1;2;3,1;5;6,0;8;9", stringsAsFactors=FALSE) 
    maml.mapOutputPort("data");  

#### <a name="module-2"></a><span data-ttu-id="cff55-140">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="cff55-140">Module 2:</span></span>
    #GLM modeling   
    data <- maml.mapInputPort(1) # class: data.frame  

    data.split <- strsplit(data[1,1], ",")[[1]] 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- as.data.frame(t(data.split)) data.split <- 
    data.matrix(data.split) 
    data.split <- data.frame(data.split) 

    model <- glm(data.split$V1 ~., family='binomial', data=data.split)  
    out <- data.frame(predict(model,data.split, type="response")) 
    pred1 <- as.data.frame(out) 
    group <- array(1:nrow(pred1)) 
    for (i in 1:nrow(pred1))  
        {
        if(as.numeric(pred1[i,])>0.5) {group[i]=1} 
        else {group[i]=0}
        } 
    pred2 <- as.data.frame(group) 
    maml.mapOutputPort("pred2");  


## <a name="limitations"></a><span data-ttu-id="cff55-141">Omezení</span><span class="sxs-lookup"><span data-stu-id="cff55-141">Limitations</span></span>
<span data-ttu-id="cff55-142">Toto je velmi jednoduchý příklad binární klasifikace webové služby.</span><span class="sxs-lookup"><span data-stu-id="cff55-142">This is a very simple example of a binary classification web service.</span></span> <span data-ttu-id="cff55-143">Jak je vidět z výše uvedeném příkladu kódu, je implementovaný zachycení žádné chyby a službu předpokládá, že všechno, co je binární, průběžné proměnné (bez kategorií funkcí povoleny), jako služba číselné hodnoty vstupy pouze v době vytvoření této webové služby.</span><span class="sxs-lookup"><span data-stu-id="cff55-143">As can be seen from the example code above, no error catching is implemented and the service assumes everything is a binary/continuous variable (no categorical features allowed), as the service only inputs numeric values at the time of the creation of this web service.</span></span> <span data-ttu-id="cff55-144">Navíc služba aktuálně zpracovává velikost omezenou dat, vyřízení požadavků a odpovědí povaze volání webové služby a fakt, že je probíhá podle modelu pokaždé, když je volat webovou službu.</span><span class="sxs-lookup"><span data-stu-id="cff55-144">Also, the service currently handles limited data size, due to the request/response nature of the web service call and the fact that the model is being fit every time the web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="cff55-145">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="cff55-145">FAQ</span></span>
<span data-ttu-id="cff55-146">Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="cff55-146">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-binary-classifier/binary1.png
[2]: ./media/machine-learning-r-csharp-binary-classifier/binary2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

