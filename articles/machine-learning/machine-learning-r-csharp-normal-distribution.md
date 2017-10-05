---
title: "(nepoužívané) Normální rozdělení webové služby Suite - Azure | Microsoft Docs"
description: "(nepoužívané) Normální rozdělení webové služby sady"
services: machine-learning
documentationcenter: 
author: ireiter
manager: jhubbard
editor: cgronlun
ms.assetid: aab7b92e-953b-43d8-b0af-031394406bfe
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
ms.openlocfilehash: 79d1621330ad56b0c62ca46cfac424c2306e371f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-normal-distribution-suite"></a><span data-ttu-id="d86a0-103">(nepoužívané) Sada normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="d86a0-103">(deprecated) Normal Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="d86a0-104">Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="d86a0-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="d86a0-105">Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="d86a0-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="d86a0-106">Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="d86a0-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="d86a0-107">Sada normálního rozdělení je sada ukázkové webové služby ([generátor](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile kalkulačky](https://datamarket.azure.com/dataset/aml_labs/ndq5), [pravděpodobnosti kalkulačky](https://datamarket.azure.com/dataset/aml_labs/ndp5)), které pomoci v generování a zpracování Normální distribuce.</span><span class="sxs-lookup"><span data-stu-id="d86a0-107">The Normal Distribution Suite is a set of sample web services ([Generator](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/ndq5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/ndp5)) that help in generating and handling normal distributions.</span></span> <span data-ttu-id="d86a0-108">Služby umožňují generování posloupnost délka, výpočet quantiles z daného pravděpodobnosti a výpočet pravděpodobnosti z daného quantile normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="d86a0-108">The services allow generating a normal distribution sequence of any length, calculating quantiles from a given probability, and calculating probability from a given quantile.</span></span> <span data-ttu-id="d86a0-109">Jednotlivé služby vysílá různých výstupů založené na vybraná služba (viz popis dole).</span><span class="sxs-lookup"><span data-stu-id="d86a0-109">Each of the services emits different outputs based on the selected service (see description below).</span></span> <span data-ttu-id="d86a0-110">Sada normálního rozdělení je založena na qnorm funkce R, rnorm a pnorm, které jsou součástí balíčku R statistiky.</span><span class="sxs-lookup"><span data-stu-id="d86a0-110">The Normal Distribution Suite is based on the R functions qnorm, rnorm, and pnorm, which are included in R stats package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="d86a0-111">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="d86a0-111">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="d86a0-112">Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="d86a0-112">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="d86a0-113">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d86a0-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="d86a0-114">Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="d86a0-114">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="d86a0-115">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="d86a0-115">Consumption of web service</span></span>
<span data-ttu-id="d86a0-116">Sada normálního rozdělení zahrnuje následující 3 služby.</span><span class="sxs-lookup"><span data-stu-id="d86a0-116">The Normal Distribution Suite includes the following 3 services.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="d86a0-117">Kalkulačky Quantile normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="d86a0-117">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="d86a0-118">Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá přidružené quantile.</span><span class="sxs-lookup"><span data-stu-id="d86a0-118">This service accepts 4 arguments of a normal distribution and calculates the associated quantile.</span></span>

<span data-ttu-id="d86a0-119">Vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="d86a0-119">The input arguments are:</span></span>

* <span data-ttu-id="d86a0-120">p – jeden pravděpodobnost událost normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="d86a0-120">p - A single probability of an event with normal distribution.</span></span> 
* <span data-ttu-id="d86a0-121">Střední – střední normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="d86a0-121">Mean - The normal distribution mean.</span></span>
* <span data-ttu-id="d86a0-122">SD - směrodatnou odchylku normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="d86a0-122">SD - The normal distribution standard deviation.</span></span> 
* <span data-ttu-id="d86a0-123">Straně - L pro na dolní straně distribuce a U pro rozdělení na horní straně.</span><span class="sxs-lookup"><span data-stu-id="d86a0-123">Side - L for the lower side of the distribution and U for the upper side of the distribution.</span></span>

<span data-ttu-id="d86a0-124">Výstup služby je počítaný quantile, který je přidružen daný pravděpodobnosti.</span><span class="sxs-lookup"><span data-stu-id="d86a0-124">The output of the service is the calculated quantile that is associated with the given probability.</span></span>

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="d86a0-125">Normální rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="d86a0-125">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="d86a0-126">Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá přidružené pravděpodobnosti.</span><span class="sxs-lookup"><span data-stu-id="d86a0-126">This service accepts 4 arguments of a normal distribution and calculates the associated probability.</span></span>

<span data-ttu-id="d86a0-127">Vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="d86a0-127">The input arguments are:</span></span>

* <span data-ttu-id="d86a0-128">q - a jeden quantile událost normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="d86a0-128">q - A single quantile of an event with normal distribution.</span></span> 
* <span data-ttu-id="d86a0-129">Střední – střední normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="d86a0-129">Mean - The normal distribution mean.</span></span>
* <span data-ttu-id="d86a0-130">SD - směrodatnou odchylku normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="d86a0-130">SD - The normal distribution standard deviation.</span></span> 
* <span data-ttu-id="d86a0-131">Straně - L pro na dolní straně distribuce a U pro rozdělení na horní straně.</span><span class="sxs-lookup"><span data-stu-id="d86a0-131">Side - L for the lower side of the distribution and U for the upper side of the distribution.</span></span>

<span data-ttu-id="d86a0-132">Výstup služby je počítaný pravděpodobnost, že je přidružen daný quantile.</span><span class="sxs-lookup"><span data-stu-id="d86a0-132">The output of the service is the calculated probability that is associated with the given quantile.</span></span>

### <a name="normal-distribution-generator"></a><span data-ttu-id="d86a0-133">Generátor normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="d86a0-133">Normal Distribution Generator</span></span>
<span data-ttu-id="d86a0-134">Tato služba přijímá 3 argumenty normálního rozdělení a generuje náhodné pořadí čísel, který je obvykle distribuován.</span><span class="sxs-lookup"><span data-stu-id="d86a0-134">This service accepts 3 arguments of a normal distribution and generates a random sequence of numbers that are normally distributed.</span></span> <span data-ttu-id="d86a0-135">Následující argumenty by měly být zadané ho v rámci žádosti:</span><span class="sxs-lookup"><span data-stu-id="d86a0-135">The following arguments should be provided to it within the request:</span></span>

* <span data-ttu-id="d86a0-136">n-počet pozorování.</span><span class="sxs-lookup"><span data-stu-id="d86a0-136">n - The number of observations.</span></span> 
* <span data-ttu-id="d86a0-137">Střední – střední normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="d86a0-137">mean - The normal distribution mean.</span></span>
* <span data-ttu-id="d86a0-138">SD - směrodatnou odchylku normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="d86a0-138">sd - The normal distribution standard deviation.</span></span> 

<span data-ttu-id="d86a0-139">Výstup služby je posloupnost délku n s normální rozdělení, podle střední a sd argumenty.</span><span class="sxs-lookup"><span data-stu-id="d86a0-139">The output of the service is a sequence of length n with a normal distribution based on the mean and sd arguments.</span></span>

> <span data-ttu-id="d86a0-140">Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="d86a0-140">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="d86a0-141">Existuje více způsobů využívání služby automatizovaně (například aplikace jsou tady: [generátor](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [pravděpodobnosti kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span><span class="sxs-lookup"><span data-stu-id="d86a0-141">There are multiple ways of consuming the service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="d86a0-142">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="d86a0-142">Starting C# code for web service consumption:</span></span>
### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="d86a0-143">Kalkulačky Quantile normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="d86a0-143">Normal Distribution Quantile Calculator</span></span>
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="d86a0-144">Normální rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="d86a0-144">Normal Distribution Probability Calculator</span></span>
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

### <a name="normal-distribution-generator"></a><span data-ttu-id="d86a0-145">Generátor normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="d86a0-145">Normal Distribution Generator</span></span>
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


## <a name="creation-of-web-service"></a><span data-ttu-id="d86a0-146">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="d86a0-146">Creation of web service</span></span>
> <span data-ttu-id="d86a0-147">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d86a0-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="d86a0-148">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="d86a0-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> 
> 
> 

<span data-ttu-id="d86a0-149">Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="d86a0-149">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="d86a0-150">Kalkulačky Quantile normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="d86a0-150">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="d86a0-151">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="d86a0-151">Experiment flow:</span></span>

![Tok experimentu][2]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="d86a0-153">Normální rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="d86a0-153">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="d86a0-154">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="d86a0-154">Experiment flow:</span></span>

![Tok experimentu][3]

     #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-generator"></a><span data-ttu-id="d86a0-156">Generátor normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="d86a0-156">Normal Distribution Generator</span></span>
<span data-ttu-id="d86a0-157">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="d86a0-157">Experiment flow:</span></span>

![Tok experimentu][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="d86a0-159">Omezení</span><span class="sxs-lookup"><span data-stu-id="d86a0-159">Limitations</span></span>
<span data-ttu-id="d86a0-160">Toto jsou velmi jednoduché příklady, které obaluje normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="d86a0-160">These are very simple examples surrounding the normal distribution.</span></span> <span data-ttu-id="d86a0-161">Jak je vidět z výše uvedeném příkladu kódu, málo zachytávání chyb je implementováno.</span><span class="sxs-lookup"><span data-stu-id="d86a0-161">As can be seen from the example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="d86a0-162">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="d86a0-162">FAQ</span></span>
<span data-ttu-id="d86a0-163">Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="d86a0-163">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png

