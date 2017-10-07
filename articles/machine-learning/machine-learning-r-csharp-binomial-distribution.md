---
title: "AAA(Deprecated) binomické rozdělení Suite - Azure | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 6f94436cd19abeb518d179f340c8d4f43fcf4520
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-binomial-distribution-suite"></a><span data-ttu-id="efaf0-103">(nepoužívané) Sada binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="efaf0-103">(deprecated) Binomial Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="efaf0-104">Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="efaf0-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="efaf0-105">Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="efaf0-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="efaf0-106">Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="efaf0-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="efaf0-107">Hello binomické rozdělení Suite je sada ukázkové webové služby ([binomické generátor](https://datamarket.azure.com/dataset/aml_labs/bdg5), [pravděpodobnosti kalkulačky](https://datamarket.azure.com/dataset/aml_labs/bdp4), [Quantile kalkulačky](https://datamarket.azure.com/dataset/aml_labs/bdq5)), které pomoci při generování a práci s binomické distribuce.</span><span class="sxs-lookup"><span data-stu-id="efaf0-107">hello Binomial Distribution Suite is a set of sample web services ([Binomial Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/bdp4), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/bdq5)) that help in generating and dealing with binomial distributions.</span></span> <span data-ttu-id="efaf0-108">Hello služby umožňují generování binomické rozdělení pořadí jakékoli délky výpočet quantiles z daného z daného quantile pravděpodobnosti a výpočet pravděpodobnosti.</span><span class="sxs-lookup"><span data-stu-id="efaf0-108">hello services allow generating a binomial distribution sequence of any length, calculating quantiles out of given probability and calculating probability from a given quantile.</span></span> <span data-ttu-id="efaf0-109">Všechny služby hello vysílá různých výstupů založené na službě hello vybrané (viz popis dole).</span><span class="sxs-lookup"><span data-stu-id="efaf0-109">Each of hello services emits different outputs based on hello selected service (see description below).</span></span> <span data-ttu-id="efaf0-110">Hello binomické rozdělení Suite vychází hello R funkce qbinom rbinom a pbinom, které jsou součástí balíčku statistiky R hello.</span><span class="sxs-lookup"><span data-stu-id="efaf0-110">hello Binomial Distribution Suite is based on hello R functions qbinom, rbinom, and pbinom, which are included in hello R stats package.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="efaf0-111">Tyto webové služby může spotřebovat uživatelů – potenciálně přímo na hello tržiště pomocí mobilní aplikace prostřednictvím webu, nebo i v místním počítači, např.</span><span class="sxs-lookup"><span data-stu-id="efaf0-111">These web services could be consumed by users – potentially directly on hello marketplace, through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="efaf0-112">Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="efaf0-112">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="efaf0-113">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="efaf0-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="efaf0-114">Hello webová služba může být pak publikované toohello Azure Marketplace a spotřebovávají uživatelů a zařízení napříč hello world – infrastruktury autorem hello hello webové služby není zapotřebí žádné nastavení.</span><span class="sxs-lookup"><span data-stu-id="efaf0-114">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world – no infrastructure setup by hello author of hello web service is required.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="efaf0-115">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="efaf0-115">Consumption of web service</span></span>
<span data-ttu-id="efaf0-116">Hello binomické rozdělení Suite zahrnuje následující 3 služby hello.</span><span class="sxs-lookup"><span data-stu-id="efaf0-116">hello Binomial Distribution Suite includes hello following 3 services.</span></span>

### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="efaf0-117">Kalkulačky Quantile binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="efaf0-117">Binomial Distribution Quantile Calculator</span></span>
<span data-ttu-id="efaf0-118">Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá quantile hello přidružené.</span><span class="sxs-lookup"><span data-stu-id="efaf0-118">This service accepts 4 arguments of a normal distribution and calculates hello associated quantile.</span></span>
<span data-ttu-id="efaf0-119">Hello vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="efaf0-119">hello input arguments are:</span></span>

* <span data-ttu-id="efaf0-120">p – jeden agregován pravděpodobnost více zkušební verze.</span><span class="sxs-lookup"><span data-stu-id="efaf0-120">p - A single aggregated probability of multiple trials.</span></span>  
* <span data-ttu-id="efaf0-121">velikost - hello počet pokusů.</span><span class="sxs-lookup"><span data-stu-id="efaf0-121">size - hello number of trials.</span></span>
* <span data-ttu-id="efaf0-122">PROB - hello pravděpodobnost úspěšného ve zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="efaf0-122">prob - hello probability of success in a trial.</span></span>
* <span data-ttu-id="efaf0-123">Straně - L pro dolní straně hello rozdělení hello U pro horní straně hello rozdělení hello.</span><span class="sxs-lookup"><span data-stu-id="efaf0-123">Side - L for hello lower side of hello distribution, U for hello upper side of hello distribution.</span></span> 

<span data-ttu-id="efaf0-124">výstup Hello hello služby je quantile hello vypočítat, která souvisí s danou pravděpodobnosti hello.</span><span class="sxs-lookup"><span data-stu-id="efaf0-124">hello output of hello service is hello calculated quantile that is associated with hello given probability.</span></span>

### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="efaf0-125">Binomické rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="efaf0-125">Binomial Distribution Probability Calculator</span></span>
<span data-ttu-id="efaf0-126">Tato služba přijímá 4 argumenty binomické rozdělení a vypočítá hello přidružené pravděpodobnosti.</span><span class="sxs-lookup"><span data-stu-id="efaf0-126">This service accepts 4 arguments of a binomial distribution and calculates hello associated probability.</span></span>
<span data-ttu-id="efaf0-127">Hello vstupní argumenty jsou:</span><span class="sxs-lookup"><span data-stu-id="efaf0-127">hello input arguments are:</span></span>

* <span data-ttu-id="efaf0-128">q - a jeden quantile událost binomické rozdělení.</span><span class="sxs-lookup"><span data-stu-id="efaf0-128">q - A single quantile of an event with binomial distribution.</span></span> 
* <span data-ttu-id="efaf0-129">velikost - hello počet pokusů.</span><span class="sxs-lookup"><span data-stu-id="efaf0-129">size - hello number of trials.</span></span>
* <span data-ttu-id="efaf0-130">PROB - hello pravděpodobnost úspěšného ve zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="efaf0-130">prob - hello probability of success in a trial.</span></span>
* <span data-ttu-id="efaf0-131">straně - L pro dolní straně hello hello rozšíření, U pro horní straně hello rozdělení hello nebo E, která je rovna tooa jeden počet úspěchů.</span><span class="sxs-lookup"><span data-stu-id="efaf0-131">side - L for hello lower side of hello distribution, U for hello upper side of hello distribution, or E that is equal tooa single number of successes.</span></span>

<span data-ttu-id="efaf0-132">výstup Hello hello služby je hello vypočítat pravděpodobnost, že je spojena s danou quantile hello.</span><span class="sxs-lookup"><span data-stu-id="efaf0-132">hello output of hello service is hello calculated probability that is associated with hello given quantile.</span></span>

### <a name="binomial-distribution-generator"></a><span data-ttu-id="efaf0-133">Generátor binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="efaf0-133">Binomial Distribution Generator</span></span>
<span data-ttu-id="efaf0-134">Tato služba přijímá 3 argumenty binomické rozdělení a generuje náhodné pořadí čísel binomially distribuovány.</span><span class="sxs-lookup"><span data-stu-id="efaf0-134">This service accepts 3 arguments of a binomial distribution and generates a random sequence of numbers that are binomially distributed.</span></span> <span data-ttu-id="efaf0-135">Hello následující argumenty musí být uváděny tooit v rámci žádosti o hello:</span><span class="sxs-lookup"><span data-stu-id="efaf0-135">hello following arguments should be provided tooit within hello request:</span></span>

* <span data-ttu-id="efaf0-136">n - počet pozorování.</span><span class="sxs-lookup"><span data-stu-id="efaf0-136">n - Number of observations.</span></span> 
* <span data-ttu-id="efaf0-137">velikost - počet pokusů.</span><span class="sxs-lookup"><span data-stu-id="efaf0-137">size - Number of trials.</span></span>
* <span data-ttu-id="efaf0-138">PROB - pravděpodobnost úspěchu.</span><span class="sxs-lookup"><span data-stu-id="efaf0-138">prob - Probability of success.</span></span>

<span data-ttu-id="efaf0-139">výstup Hello hello služby je posloupnost délku n s binomické rozdělení, podle velikosti a prob argumenty hello.</span><span class="sxs-lookup"><span data-stu-id="efaf0-139">hello output of hello service is a sequence of length n with a binomial distribution based on hello size and prob arguments.</span></span>

> <span data-ttu-id="efaf0-140">Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="efaf0-140">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="efaf0-141">Existuje více způsobů využívání služby hello automatizovaně (například aplikace jsou tady: [generátor](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [pravděpodobnosti kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Quantile kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)).</span><span class="sxs-lookup"><span data-stu-id="efaf0-141">There are multiple ways of consuming hello service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)).</span></span> 

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="efaf0-142">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="efaf0-142">Starting C# code for web service consumption:</span></span>
### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="efaf0-143">Kalkulačky Quantile binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="efaf0-143">Binomial Distribution Quantile Calculator</span></span>
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

### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="efaf0-144">Binomické rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="efaf0-144">Binomial Distribution Probability Calculator</span></span>
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


### <a name="binomial-distribution-generator"></a><span data-ttu-id="efaf0-145">Generátor binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="efaf0-145">Binomial Distribution Generator</span></span>
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





## <a name="creation-of-web-service"></a><span data-ttu-id="efaf0-146">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="efaf0-146">Creation of web service</span></span>
> <span data-ttu-id="efaf0-147">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="efaf0-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="efaf0-148">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="efaf0-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="efaf0-149">Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="efaf0-149">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="efaf0-150">Kalkulačky Quantile binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="efaf0-150">Binomial Distribution Quantile Calculator</span></span>
![Vytvořit pracovní prostor][4]

#### <a name="module-1"></a><span data-ttu-id="efaf0-152">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="efaf0-152">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port
#### <a name="module-2"></a><span data-ttu-id="efaf0-153">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="efaf0-153">Module 2:</span></span>
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");


### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="efaf0-154">Binomické rozdělení pravděpodobnosti kalkulačky</span><span class="sxs-lookup"><span data-stu-id="efaf0-154">Binomial Distribution Probability Calculator</span></span>
![Vytvořit pracovní prostor][5]

#### <a name="module-1"></a><span data-ttu-id="efaf0-156">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="efaf0-156">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port


#### <a name="module-2"></a><span data-ttu-id="efaf0-157">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="efaf0-157">Module 2:</span></span>
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

### <a name="binomial-distribution-generator"></a><span data-ttu-id="efaf0-158">Generátor binomické rozdělení</span><span class="sxs-lookup"><span data-stu-id="efaf0-158">Binomial Distribution Generator</span></span>
![Vytvořit pracovní prostor][6]

#### <a name="module-1"></a><span data-ttu-id="efaf0-160">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="efaf0-160">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data toooutput port

#### <a name="module-2"></a><span data-ttu-id="efaf0-161">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="efaf0-161">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="efaf0-162">Omezení</span><span class="sxs-lookup"><span data-stu-id="efaf0-162">Limitations</span></span>
<span data-ttu-id="efaf0-163">Toto jsou velmi jednoduché příklady, které obaluje hello binomické rozdělení.</span><span class="sxs-lookup"><span data-stu-id="efaf0-163">These are very simple examples surrounding hello binomial distribution.</span></span> <span data-ttu-id="efaf0-164">Jak je vidět z hello ukázkový kód výše, málo zachytávání chyb je implementováno.</span><span class="sxs-lookup"><span data-stu-id="efaf0-164">As can be seen from hello example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="efaf0-165">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="efaf0-165">FAQ</span></span>
<span data-ttu-id="efaf0-166">Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="efaf0-166">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png

