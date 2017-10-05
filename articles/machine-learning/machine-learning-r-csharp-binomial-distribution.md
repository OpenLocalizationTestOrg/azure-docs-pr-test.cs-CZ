---
title: "(nepoužívané) Binomické rozdělení Suite - Azure | Microsoft Docs"
description: "(nepoužívané) Sada binomické rozdělení"
services: machine-learning
documentationcenter: 
author: ireiter
manager: jhubbard
editor: cgronlun
ms.assetid: 6d102d57-8f20-4ab3-be31-02fcfe4d15ed
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: ireiter
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 6f0a6d06e7401c8360a92a707a0552f41ff3657c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-binomial-distribution-suite"></a><span data-ttu-id="32559-103">(nepoužívané) Sada binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="32559-103">(deprecated) Binomial Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="32559-104">Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="32559-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="32559-105">Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="32559-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="32559-106">Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="32559-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="32559-107">Sada binomické rozdělení je sada ukázkové webové služby ([binomický generátor](https://datamarket.azure.com/dataset/aml_labs/bdg5), [pravděpodobnosti kalkulačky](https://datamarket.azure.com/dataset/aml_labs/bdp4), [Quantile kalkulačky](https://datamarket.azure.com/dataset/aml_labs/bdq5)), které pomoci v generování a plánování práce s binomické distribuce.</span><span class="sxs-lookup"><span data-stu-id="32559-107">The Binomial Distribution Suite is a set of sample web services ([Binomial Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/bdp4), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/bdq5)) that help in generating and dealing with binomial distributions.</span></span> <span data-ttu-id="32559-108">Služby umožňují generování binomické rozdělení pořadí jakékoli délky výpočet quantiles z daného z daného quantile pravděpodobnosti a výpočet pravděpodobnosti.</span><span class="sxs-lookup"><span data-stu-id="32559-108">The services allow generating a binomial distribution sequence of any length, calculating quantiles out of given probability and calculating probability from a given quantile.</span></span> <span data-ttu-id="32559-109">Jednotlivé služby vysílá různých výstupů založené na vybraná služba (viz popis dole).</span><span class="sxs-lookup"><span data-stu-id="32559-109">Each of the services emits different outputs based on the selected service (see description below).</span></span> <span data-ttu-id="32559-110">Sada binomické rozdělení je založena na qbinom funkce R, rbinom a pbinom, které jsou součástí balíček R statistiky.</span><span class="sxs-lookup"><span data-stu-id="32559-110">The Binomial Distribution Suite is based on the R functions qbinom, rbinom, and pbinom, which are included in the R stats package.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="32559-111">Tyto webové služby může spotřebovat uživatelů – potenciálně přímo na marketplace, prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, např.</span><span class="sxs-lookup"><span data-stu-id="32559-111">These web services could be consumed by users – potentially directly on the marketplace, through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="32559-112">Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="32559-112">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="32559-113">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="32559-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="32559-114">Webové služby pak můžete publikovat na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě – není zapotřebí žádné nastavení infrastruktury autorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="32559-114">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world – no infrastructure setup by the author of the web service is required.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="32559-115">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="32559-115">Consumption of web service</span></span>
<span data-ttu-id="32559-116">Sada binomické rozdělení zahrnuje následující 3 služby.</span><span class="sxs-lookup"><span data-stu-id="32559-116">The Binomial Distribution Suite includes the following 3 services.</span></span>

### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="32559-117">Kalkulačky Quantile binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="32559-117">Binomial Distribution Quantile Calculator</span></span>
<span data-ttu-id="32559-118">Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá přidružené quantile.</span><span class="sxs-lookup"><span data-stu-id="32559-118">This service accepts 4 arguments of a normal distribution and calculates the associated quantile.</span></span>
<span data-ttu-id="32559-119">Vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="32559-119">The input arguments are:</span></span>

* <span data-ttu-id="32559-120">p – jeden agregován pravděpodobnost více zkušební verze.</span><span class="sxs-lookup"><span data-stu-id="32559-120">p - A single aggregated probability of multiple trials.</span></span>  
* <span data-ttu-id="32559-121">velikost - počet pokusů.</span><span class="sxs-lookup"><span data-stu-id="32559-121">size - The number of trials.</span></span>
* <span data-ttu-id="32559-122">PROB - pravděpodobnost úspěchu ve zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="32559-122">prob - The probability of success in a trial.</span></span>
* <span data-ttu-id="32559-123">Straně - L pro dolní straně rozdělení U pro rozdělení na horní straně.</span><span class="sxs-lookup"><span data-stu-id="32559-123">Side - L for the lower side of the distribution, U for the upper side of the distribution.</span></span> 

<span data-ttu-id="32559-124">Výstup služby je počítaný quantile, který je přidružen daný pravděpodobnosti.</span><span class="sxs-lookup"><span data-stu-id="32559-124">The output of the service is the calculated quantile that is associated with the given probability.</span></span>

### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="32559-125">Binomické rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="32559-125">Binomial Distribution Probability Calculator</span></span>
<span data-ttu-id="32559-126">Tato služba přijímá 4 argumenty binomické rozdělení a vypočítá přidružené pravděpodobnosti.</span><span class="sxs-lookup"><span data-stu-id="32559-126">This service accepts 4 arguments of a binomial distribution and calculates the associated probability.</span></span>
<span data-ttu-id="32559-127">Vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="32559-127">The input arguments are:</span></span>

* <span data-ttu-id="32559-128">q - a jeden quantile událost binomické rozdělení.</span><span class="sxs-lookup"><span data-stu-id="32559-128">q - A single quantile of an event with binomial distribution.</span></span> 
* <span data-ttu-id="32559-129">velikost - počet pokusů.</span><span class="sxs-lookup"><span data-stu-id="32559-129">size - The number of trials.</span></span>
* <span data-ttu-id="32559-130">PROB - pravděpodobnost úspěchu ve zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="32559-130">prob - The probability of success in a trial.</span></span>
* <span data-ttu-id="32559-131">straně - L pro dolní straně rozdělení U pro horní strana distribuce, nebo E, která je rovna jedné počet úspěchů.</span><span class="sxs-lookup"><span data-stu-id="32559-131">side - L for the lower side of the distribution, U for the upper side of the distribution, or E that is equal to a single number of successes.</span></span>

<span data-ttu-id="32559-132">Výstup služby je počítaný pravděpodobnost, že je přidružen daný quantile.</span><span class="sxs-lookup"><span data-stu-id="32559-132">The output of the service is the calculated probability that is associated with the given quantile.</span></span>

### <a name="binomial-distribution-generator"></a><span data-ttu-id="32559-133">Generátor binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="32559-133">Binomial Distribution Generator</span></span>
<span data-ttu-id="32559-134">Tato služba přijímá 3 argumenty binomické rozdělení a generuje náhodné pořadí čísel binomially distribuovány.</span><span class="sxs-lookup"><span data-stu-id="32559-134">This service accepts 3 arguments of a binomial distribution and generates a random sequence of numbers that are binomially distributed.</span></span> <span data-ttu-id="32559-135">Následující argumenty by měly být zadané ho v rámci žádosti:</span><span class="sxs-lookup"><span data-stu-id="32559-135">The following arguments should be provided to it within the request:</span></span>

* <span data-ttu-id="32559-136">n - počet pozorování.</span><span class="sxs-lookup"><span data-stu-id="32559-136">n - Number of observations.</span></span> 
* <span data-ttu-id="32559-137">velikost - počet pokusů.</span><span class="sxs-lookup"><span data-stu-id="32559-137">size - Number of trials.</span></span>
* <span data-ttu-id="32559-138">PROB - pravděpodobnost úspěchu.</span><span class="sxs-lookup"><span data-stu-id="32559-138">prob - Probability of success.</span></span>

<span data-ttu-id="32559-139">Výstup služby je posloupnost délku n s binomické rozdělení, podle velikosti a prob argumenty.</span><span class="sxs-lookup"><span data-stu-id="32559-139">The output of the service is a sequence of length n with a binomial distribution based on the size and prob arguments.</span></span>

> <span data-ttu-id="32559-140">Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="32559-140">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="32559-141">Existuje více způsobů využívání služby automatizovaně (například aplikace jsou tady: [generátor](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [pravděpodobnosti kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Quantile kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)).</span><span class="sxs-lookup"><span data-stu-id="32559-141">There are multiple ways of consuming the service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)).</span></span> 

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="32559-142">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="32559-142">Starting C# code for web service consumption:</span></span>
### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="32559-143">Kalkulačky Quantile binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="32559-143">Binomial Distribution Quantile Calculator</span></span>
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="32559-144">Binomické rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="32559-144">Binomial Distribution Probability Calculator</span></span>
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


### <a name="binomial-distribution-generator"></a><span data-ttu-id="32559-145">Generátor binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="32559-145">Binomial Distribution Generator</span></span>
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }





## <a name="creation-of-web-service"></a><span data-ttu-id="32559-146">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="32559-146">Creation of web service</span></span>
> <span data-ttu-id="32559-147">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="32559-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="32559-148">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="32559-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="32559-149">Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="32559-149">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="32559-150">Kalkulačky Quantile binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="32559-150">Binomial Distribution Quantile Calculator</span></span>
![Vytvořit pracovní prostor][4]

#### <a name="module-1"></a><span data-ttu-id="32559-152">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="32559-152">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
#### <a name="module-2"></a><span data-ttu-id="32559-153">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="32559-153">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="32559-154">Binomické rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="32559-154">Binomial Distribution Probability Calculator</span></span>
![Vytvořit pracovní prostor][5]

#### <a name="module-1"></a><span data-ttu-id="32559-156">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="32559-156">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


#### <a name="module-2"></a><span data-ttu-id="32559-157">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="32559-157">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="binomial-distribution-generator"></a><span data-ttu-id="32559-158">Generátor binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="32559-158">Binomial Distribution Generator</span></span>
![Vytvořit pracovní prostor][6]

#### <a name="module-1"></a><span data-ttu-id="32559-160">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="32559-160">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

#### <a name="module-2"></a><span data-ttu-id="32559-161">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="32559-161">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="32559-162">Omezení</span><span class="sxs-lookup"><span data-stu-id="32559-162">Limitations</span></span>
<span data-ttu-id="32559-163">Toto jsou velmi jednoduché příklady, které obaluje binomické rozdělení.</span><span class="sxs-lookup"><span data-stu-id="32559-163">These are very simple examples surrounding the binomial distribution.</span></span> <span data-ttu-id="32559-164">Jak je vidět z výše uvedeném příkladu kódu, málo zachytávání chyb je implementováno.</span><span class="sxs-lookup"><span data-stu-id="32559-164">As can be seen from the example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="32559-165">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="32559-165">FAQ</span></span>
<span data-ttu-id="32559-166">Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="32559-166">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
