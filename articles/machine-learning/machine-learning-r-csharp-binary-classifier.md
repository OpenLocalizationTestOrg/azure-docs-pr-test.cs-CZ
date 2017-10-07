---
title: "AAA(Deprecated) binární třídění - Azure | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 0496fcec9952ca243270caf67f55fe191b2dc9f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-binary-classifier"></a><span data-ttu-id="0de8d-103">(nepoužívané) Binární třídění</span><span class="sxs-lookup"><span data-stu-id="0de8d-103">(deprecated) Binary Classifier</span></span>

> [!NOTE]
> <span data-ttu-id="0de8d-104">Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="0de8d-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="0de8d-105">Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="0de8d-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="0de8d-106">Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="0de8d-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="0de8d-107">Předpokládejme, že máte datovou sadu a chcete toopredict proměnnou binární závislé na základě nezávislých proměnných hello.</span><span class="sxs-lookup"><span data-stu-id="0de8d-107">Suppose you have a dataset and would like toopredict a binary dependent variable based on hello independent variables.</span></span> <span data-ttu-id="0de8d-108">'Logistic Regression' je Oblíbené statistické technika pro takové předpovědi.</span><span class="sxs-lookup"><span data-stu-id="0de8d-108">‘Logistic Regression’ is a popular statistical technique used for such predictions.</span></span> <span data-ttu-id="0de8d-109">Tady je hello závislé proměnné binary nebo dichotomous a p je hello pravděpodobnost přítomnost hello vlastnosti, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="0de8d-109">Here hello dependent variable is binary or dichotomous, and p is hello probability of presence of hello characteristic of interest.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="0de8d-110">Může být jednoduchého scénáře, kde výzkumným se pokouší toopredict, jestli je potenciální student pravděpodobně tooaccept univerzity tooa nabídka jejich příchodu na základě informací (GPA střední školy, rodinného příjmu trvalé stavu, pohlaví).</span><span class="sxs-lookup"><span data-stu-id="0de8d-110">A simple scenario could be where a researcher is trying toopredict whether a prospective student is likely tooaccept an admission offer tooa university based on information (GPA in high school, family income, resident state, gender).</span></span> <span data-ttu-id="0de8d-111">výsledek předpokládaných Hello je hello pravděpodobnost hello potenciální student přijetí nabídky jejich příchodu hello.</span><span class="sxs-lookup"><span data-stu-id="0de8d-111">hello predicted outcome is hello probability of hello prospective student accepting hello admission offer.</span></span> <span data-ttu-id="0de8d-112">To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/log_regression) pro rozlišení hello data toohello modelu logistic regression a výstupy hello hodnotu pravděpodobnosti (y) pro každý hello pozorování v datech hello.</span><span class="sxs-lookup"><span data-stu-id="0de8d-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/log_regression) fits hello logistic regression model toohello data and outputs hello probability value (y) for each of hello observations in hello data.</span></span>  

> <span data-ttu-id="0de8d-113">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="0de8d-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="0de8d-114">Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="0de8d-114">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="0de8d-115">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="0de8d-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="0de8d-116">Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="0de8d-116">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="0de8d-117">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="0de8d-117">Consumption of web service</span></span>
<span data-ttu-id="0de8d-118">Tento web service poskytuje hello předpovědět hodnoty proměnné hello závislé na základě nezávislých proměnných hello všech hello připomínky.</span><span class="sxs-lookup"><span data-stu-id="0de8d-118">This web service gives hello predicted values of hello dependent variable based on hello independent variables for all of hello observations.</span></span> <span data-ttu-id="0de8d-119">Webová služba Hello očekává hello koncový uživatel tooinput data jako řetězec, kde řádky jsou oddělené čárkou (,) a sloupce jsou odděleny středníkem (;).</span><span class="sxs-lookup"><span data-stu-id="0de8d-119">hello web service expects hello end user tooinput data as a string where rows are separated by comma (,) and columns are separated by semicolon (;).</span></span> <span data-ttu-id="0de8d-120">Webová služba Hello očekává 1 řádek v čase a očekává hello první sloupec toobe hello závislé proměnné.</span><span class="sxs-lookup"><span data-stu-id="0de8d-120">hello web service expects 1 row at a time and expects hello first column toobe hello dependent variable.</span></span> <span data-ttu-id="0de8d-121">Datové sadě služby příkladu může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="0de8d-121">An example dataset could look like this:</span></span>

![Ukázková data][1]

<span data-ttu-id="0de8d-123">Připomínky bez závislé proměnné by měl zadejte jako hodnotu "NA" pro y.</span><span class="sxs-lookup"><span data-stu-id="0de8d-123">Observations without a dependent variable should be input as “NA” for y.</span></span> <span data-ttu-id="0de8d-124">Hello vstupních dat pro hello výše datová sada by být hello následující řetězec: "1; 5; 2,1; 1; 6,0; 5.3; 2.1,0; 5; 5,0; 3; 4,1; 2; 1, NA; 3; 4".</span><span class="sxs-lookup"><span data-stu-id="0de8d-124">hello data input for hello above dataset would be hello following string: “1;5;2,1;1;6,0;5.3;2.1,0;5;5,0;3;4,1;2;1,NA;3;4”.</span></span> <span data-ttu-id="0de8d-125">Hello výstup hello předpovězené hodnoty pro každý hello řádků podle hello nezávislých proměnných.</span><span class="sxs-lookup"><span data-stu-id="0de8d-125">hello output is hello predicted value for each of hello rows based on hello independent variables.</span></span> 

> <span data-ttu-id="0de8d-126">Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="0de8d-126">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="0de8d-127">Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).</span><span class="sxs-lookup"><span data-stu-id="0de8d-127">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="0de8d-128">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="0de8d-128">Starting C# code for web service consumption:</span></span>
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


## <a name="creation-of-web-service"></a><span data-ttu-id="0de8d-129">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="0de8d-129">Creation of web service</span></span>
> <span data-ttu-id="0de8d-130">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="0de8d-130">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="0de8d-131">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="0de8d-131">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="0de8d-132">Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="0de8d-132">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="0de8d-133">Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly vyžádat do pracovního prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="0de8d-133">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto hello workspace.</span></span> <span data-ttu-id="0de8d-134">Tato webová služba běží experimentu Azure Machine Learning s základní skript jazyka R.</span><span class="sxs-lookup"><span data-stu-id="0de8d-134">This web service runs an Azure Machine Learning experiment with an underlying R script.</span></span> <span data-ttu-id="0de8d-135">Existují experimentovat toothis 2 částí: definice schématu a školení modelu + vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="0de8d-135">There are 2 parts toothis experiment: schema definition, and training model + scoring.</span></span> <span data-ttu-id="0de8d-136">první modul Hello definuje hello očekávána struktura hello vstupní datové sady, kde první proměnná hello je hello závislé proměnné a hello zbývající proměnné jsou nezávislé.</span><span class="sxs-lookup"><span data-stu-id="0de8d-136">hello first module defines hello expected structure of hello input dataset, where hello first variable is hello dependent variable and hello remaining variables are independent.</span></span> <span data-ttu-id="0de8d-137">druhý modul Hello vyhovuje obecné logistic regresní model pro vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="0de8d-137">hello second module fits a generic logistic regression model for hello input data.</span></span>    

![Tok experimentu][2]

#### <a name="module-1"></a><span data-ttu-id="0de8d-139">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="0de8d-139">Module 1:</span></span>
    #Schema definition  
    data <- data.frame(value = "1;2;3,1;5;6,0;8;9", stringsAsFactors=FALSE) 
    maml.mapOutputPort("data");  

#### <a name="module-2"></a><span data-ttu-id="0de8d-140">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="0de8d-140">Module 2:</span></span>
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


## <a name="limitations"></a><span data-ttu-id="0de8d-141">Omezení</span><span class="sxs-lookup"><span data-stu-id="0de8d-141">Limitations</span></span>
<span data-ttu-id="0de8d-142">Toto je velmi jednoduchý příklad binární klasifikace webové služby.</span><span class="sxs-lookup"><span data-stu-id="0de8d-142">This is a very simple example of a binary classification web service.</span></span> <span data-ttu-id="0de8d-143">Jak je vidět z hello ukázkový kód výše, je implementována zachycení žádné chyby a hello služby předpokládá, že všechno, co je binární, průběžné proměnné (bez kategorií funkcí povoleny), jako hello služby pouze vstupy číselných hodnot v době hello hello vytvoření tohoto webové služby.</span><span class="sxs-lookup"><span data-stu-id="0de8d-143">As can be seen from hello example code above, no error catching is implemented and hello service assumes everything is a binary/continuous variable (no categorical features allowed), as hello service only inputs numeric values at hello time of hello creation of this web service.</span></span> <span data-ttu-id="0de8d-144">Navíc služba hello aktuálně zpracovává velikost omezenou dat, z důvodu toohello požadavků a odpovědí povaha hello webové služby je se volání a hello skutečnost, že hello modelu odpovídat pokaždé, když je volána hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="0de8d-144">Also, hello service currently handles limited data size, due toohello request/response nature of hello web service call and hello fact that hello model is being fit every time hello web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="0de8d-145">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="0de8d-145">FAQ</span></span>
<span data-ttu-id="0de8d-146">Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="0de8d-146">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-binary-classifier/binary1.png
[2]: ./media/machine-learning-r-csharp-binary-classifier/binary2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

