---
title: "AAA(Deprecated) normálního rozdělení webové služby Suite - Azure | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 8bdb5afd9fee88587f548d7c5299480f64289bbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-normal-distribution-suite"></a><span data-ttu-id="2677a-103">(nepoužívané) Sada normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="2677a-103">(deprecated) Normal Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="2677a-104">Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="2677a-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="2677a-105">Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="2677a-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="2677a-106">Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="2677a-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="2677a-107">Hello normálního rozdělení Suite je sada ukázkové webové služby ([generátor](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile kalkulačky](https://datamarket.azure.com/dataset/aml_labs/ndq5), [pravděpodobnosti kalkulačky](https://datamarket.azure.com/dataset/aml_labs/ndp5)), které pomoci v generování a zpracování Normální distribuce.</span><span class="sxs-lookup"><span data-stu-id="2677a-107">hello Normal Distribution Suite is a set of sample web services ([Generator](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/ndq5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/ndp5)) that help in generating and handling normal distributions.</span></span> <span data-ttu-id="2677a-108">Hello services povolit generování normálního rozdělení pořadí délky, výpočet quantiles z daného pravděpodobnosti a výpočet pravděpodobnosti z daného quantile.</span><span class="sxs-lookup"><span data-stu-id="2677a-108">hello services allow generating a normal distribution sequence of any length, calculating quantiles from a given probability, and calculating probability from a given quantile.</span></span> <span data-ttu-id="2677a-109">Všechny služby hello vysílá různých výstupů založené na službě hello vybrané (viz popis dole).</span><span class="sxs-lookup"><span data-stu-id="2677a-109">Each of hello services emits different outputs based on hello selected service (see description below).</span></span> <span data-ttu-id="2677a-110">Hello normálního rozdělení Suite vychází hello R funkce qnorm rnorm a pnorm, které jsou součástí balíčku R statistiky.</span><span class="sxs-lookup"><span data-stu-id="2677a-110">hello Normal Distribution Suite is based on hello R functions qnorm, rnorm, and pnorm, which are included in R stats package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="2677a-111">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="2677a-111">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="2677a-112">Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="2677a-112">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="2677a-113">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="2677a-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="2677a-114">Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="2677a-114">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="2677a-115">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="2677a-115">Consumption of web service</span></span>
<span data-ttu-id="2677a-116">Hello normálního rozdělení Suite zahrnuje následující 3 služby hello.</span><span class="sxs-lookup"><span data-stu-id="2677a-116">hello Normal Distribution Suite includes hello following 3 services.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="2677a-117">Kalkulačky Quantile normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="2677a-117">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="2677a-118">Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá quantile hello přidružené.</span><span class="sxs-lookup"><span data-stu-id="2677a-118">This service accepts 4 arguments of a normal distribution and calculates hello associated quantile.</span></span>

<span data-ttu-id="2677a-119">Hello vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="2677a-119">hello input arguments are:</span></span>

* <span data-ttu-id="2677a-120">p – jeden pravděpodobnost událost normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="2677a-120">p - A single probability of an event with normal distribution.</span></span> 
* <span data-ttu-id="2677a-121">Střední – střední hello normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="2677a-121">Mean - hello normal distribution mean.</span></span>
* <span data-ttu-id="2677a-122">SD - směrodatnou odchylku hello normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="2677a-122">SD - hello normal distribution standard deviation.</span></span> 
* <span data-ttu-id="2677a-123">Straně - L pro dolní straně hello rozdělení hello a U pro horní straně hello rozdělení hello.</span><span class="sxs-lookup"><span data-stu-id="2677a-123">Side - L for hello lower side of hello distribution and U for hello upper side of hello distribution.</span></span>

<span data-ttu-id="2677a-124">výstup Hello hello služby je quantile hello vypočítat, která souvisí s danou pravděpodobnosti hello.</span><span class="sxs-lookup"><span data-stu-id="2677a-124">hello output of hello service is hello calculated quantile that is associated with hello given probability.</span></span>

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="2677a-125">Normální rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="2677a-125">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="2677a-126">Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá hello přidružené pravděpodobnosti.</span><span class="sxs-lookup"><span data-stu-id="2677a-126">This service accepts 4 arguments of a normal distribution and calculates hello associated probability.</span></span>

<span data-ttu-id="2677a-127">Hello vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="2677a-127">hello input arguments are:</span></span>

* <span data-ttu-id="2677a-128">q - a jeden quantile událost normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="2677a-128">q - A single quantile of an event with normal distribution.</span></span> 
* <span data-ttu-id="2677a-129">Střední – střední hello normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="2677a-129">Mean - hello normal distribution mean.</span></span>
* <span data-ttu-id="2677a-130">SD - směrodatnou odchylku hello normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="2677a-130">SD - hello normal distribution standard deviation.</span></span> 
* <span data-ttu-id="2677a-131">Straně - L pro dolní straně hello rozdělení hello a U pro horní straně hello rozdělení hello.</span><span class="sxs-lookup"><span data-stu-id="2677a-131">Side - L for hello lower side of hello distribution and U for hello upper side of hello distribution.</span></span>

<span data-ttu-id="2677a-132">výstup Hello hello služby je hello vypočítat pravděpodobnost, že je spojena s danou quantile hello.</span><span class="sxs-lookup"><span data-stu-id="2677a-132">hello output of hello service is hello calculated probability that is associated with hello given quantile.</span></span>

### <a name="normal-distribution-generator"></a><span data-ttu-id="2677a-133">Generátor normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="2677a-133">Normal Distribution Generator</span></span>
<span data-ttu-id="2677a-134">Tato služba přijímá 3 argumenty normálního rozdělení a generuje náhodné pořadí čísel, který je obvykle distribuován.</span><span class="sxs-lookup"><span data-stu-id="2677a-134">This service accepts 3 arguments of a normal distribution and generates a random sequence of numbers that are normally distributed.</span></span> <span data-ttu-id="2677a-135">Hello následující argumenty musí být uváděny tooit v rámci žádosti o hello:</span><span class="sxs-lookup"><span data-stu-id="2677a-135">hello following arguments should be provided tooit within hello request:</span></span>

* <span data-ttu-id="2677a-136">n - hello počet pozorování.</span><span class="sxs-lookup"><span data-stu-id="2677a-136">n - hello number of observations.</span></span> 
* <span data-ttu-id="2677a-137">Střední – střední hello normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="2677a-137">mean - hello normal distribution mean.</span></span>
* <span data-ttu-id="2677a-138">SD - směrodatnou odchylku hello normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="2677a-138">sd - hello normal distribution standard deviation.</span></span> 

<span data-ttu-id="2677a-139">výstup Hello hello služby je posloupnost délku n s normální rozdělení, podle střední a sd argumenty hello.</span><span class="sxs-lookup"><span data-stu-id="2677a-139">hello output of hello service is a sequence of length n with a normal distribution based on hello mean and sd arguments.</span></span>

> <span data-ttu-id="2677a-140">Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="2677a-140">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="2677a-141">Existuje více způsobů využívání služby hello automatizovaně (například aplikace jsou tady: [generátor](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [pravděpodobnosti kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span><span class="sxs-lookup"><span data-stu-id="2677a-141">There are multiple ways of consuming hello service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="2677a-142">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="2677a-142">Starting C# code for web service consumption:</span></span>
### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="2677a-143">Kalkulačky Quantile normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="2677a-143">Normal Distribution Quantile Calculator</span></span>
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


### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="2677a-144">Normální rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="2677a-144">Normal Distribution Probability Calculator</span></span>
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

### <a name="normal-distribution-generator"></a><span data-ttu-id="2677a-145">Generátor normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="2677a-145">Normal Distribution Generator</span></span>
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


## <a name="creation-of-web-service"></a><span data-ttu-id="2677a-146">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="2677a-146">Creation of web service</span></span>
> <span data-ttu-id="2677a-147">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2677a-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="2677a-148">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="2677a-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> 
> 
> 

<span data-ttu-id="2677a-149">Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="2677a-149">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="2677a-150">Kalkulačky Quantile normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="2677a-150">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="2677a-151">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="2677a-151">Experiment flow:</span></span>

![Tok experimentu][2]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port

    # Map 1-based optional input ports toovariables
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="2677a-153">Normální rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="2677a-153">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="2677a-154">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="2677a-154">Experiment flow:</span></span>

![Tok experimentu][3]

     #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port

    # Map 1-based optional input ports toovariables
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-generator"></a><span data-ttu-id="2677a-156">Generátor normálního rozdělení</span><span class="sxs-lookup"><span data-stu-id="2677a-156">Normal Distribution Generator</span></span>
<span data-ttu-id="2677a-157">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="2677a-157">Experiment flow:</span></span>

![Tok experimentu][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data toooutput port

    # Map 1-based optional input ports toovariables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="2677a-159">Omezení</span><span class="sxs-lookup"><span data-stu-id="2677a-159">Limitations</span></span>
<span data-ttu-id="2677a-160">Toto jsou velmi jednoduché příklady, které obaluje hello normálního rozdělení.</span><span class="sxs-lookup"><span data-stu-id="2677a-160">These are very simple examples surrounding hello normal distribution.</span></span> <span data-ttu-id="2677a-161">Jak je vidět z hello ukázkový kód výše, málo zachytávání chyb je implementováno.</span><span class="sxs-lookup"><span data-stu-id="2677a-161">As can be seen from hello example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="2677a-162">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="2677a-162">FAQ</span></span>
<span data-ttu-id="2677a-163">Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="2677a-163">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png

