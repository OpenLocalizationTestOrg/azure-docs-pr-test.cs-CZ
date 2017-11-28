---
title: "AAA(Deprecated) Model clusteru – Azure | Microsoft Docs"
description: "(nepoužívané) Model clusteru"
services: machine-learning
documentationcenter: 
author: FrancescaLazzeri
manager: jhubbard
editor: cgronlun
ms.assetid: 51b8d012-ed44-4312-920c-9c808dfd4ff6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: lazzeri
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 7b2dffb855a8d91114752b579115e97d07210e45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-cluster-model"></a><span data-ttu-id="545bb-103">(nepoužívané) Model clusteru</span><span class="sxs-lookup"><span data-stu-id="545bb-103">(deprecated) Cluster Model</span></span>

> [!NOTE]
> <span data-ttu-id="545bb-104">Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="545bb-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="545bb-105">Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="545bb-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="545bb-106">Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="545bb-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="545bb-107">Jak jsme předpovědi skupiny platební služba chování v pořadí tooreduce hello poplatků vypnout riziko vystavitelů platební karty?</span><span class="sxs-lookup"><span data-stu-id="545bb-107">How can we predict groups of credit cardholders’ behaviors in order tooreduce hello charge-off risk of credit card issuers?</span></span> <span data-ttu-id="545bb-108">Jak jsme v můžete definovat skupiny charakteru vlastnosti zaměstnanců pořadí tooimprove výkonu v práci?</span><span class="sxs-lookup"><span data-stu-id="545bb-108">How can we define groups of personality traits of employees in order tooimprove their performance at work?</span></span> <span data-ttu-id="545bb-109">Jak můžete lékařů klasifikovat pacientů do skupin na základě charakteristik hello jejich nákaz?</span><span class="sxs-lookup"><span data-stu-id="545bb-109">How can doctors classify patients into groups based on hello characteristics of their diseases?</span></span> <span data-ttu-id="545bb-110">V zásadě všechny tyto otázky může odpovědět na tyto otázky prostřednictvím analýzy clusteru.</span><span class="sxs-lookup"><span data-stu-id="545bb-110">In principle, all of these questions can be answered through cluster analysis.</span></span>   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="545bb-111">Analýza clusteru klasifikuje sadu připomínky do dvou nebo více vzájemně se vylučuje neznámé skupin na základě kombinace proměnné.</span><span class="sxs-lookup"><span data-stu-id="545bb-111">Cluster analysis classifies a set of observations into two or more mutually exclusive unknown groups based on combinations of variables.</span></span> <span data-ttu-id="545bb-112">Hello účelem analýzy clusteru je toodiscover systém uspořádání připomínky, obvykle osoby nebo vlastností jsou do skupiny, kde členů skupin hello sdílí vlastnosti společné.</span><span class="sxs-lookup"><span data-stu-id="545bb-112">hello purpose of cluster analysis is toodiscover a system of organizing observations, usually people or their characteristics, into groups, where members of hello groups share properties in common.</span></span> <span data-ttu-id="545bb-113">To [služby](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) používá hello K-Means metodika, běžně používané clustering techniku, toocluster libovolná data do skupin.</span><span class="sxs-lookup"><span data-stu-id="545bb-113">This [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) uses hello K-Means methodology, a commonly used clustering technique, toocluster arbitrary data into groups.</span></span> <span data-ttu-id="545bb-114">Této webové služby trvá hello dat a hello počet tisíc clusterů jako vstup a vytváří předpovědi, které hello tisíc skupiny toowhich patří každý připomínky.</span><span class="sxs-lookup"><span data-stu-id="545bb-114">This web service takes hello data and hello number of k clusters as input, and produces predictions of which of hello k groups toowhich each observations belongs.</span></span> 

> <span data-ttu-id="545bb-115">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="545bb-115">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="545bb-116">Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="545bb-116">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="545bb-117">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="545bb-117">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="545bb-118">Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="545bb-118">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="545bb-119">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="545bb-119">Consumption of web service</span></span>
<span data-ttu-id="545bb-120">Tato webová služba skupiny hello dat do několika tisíc skupin a výstupy hello přiřazení skupiny pro každý řádek.</span><span class="sxs-lookup"><span data-stu-id="545bb-120">This web service groups hello data into a set of k groups and outputs hello group assignment for each row.</span></span> <span data-ttu-id="545bb-121">Webová služba Hello očekává hello koncový uživatel tooinput data jako řetězec, kde jsou řádky oddělených čárkami (,) a sloupce jsou odděleny středníkem (;).</span><span class="sxs-lookup"><span data-stu-id="545bb-121">hello web service expects hello end user tooinput data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="545bb-122">Webová služba Hello očekává 1 řádek najednou.</span><span class="sxs-lookup"><span data-stu-id="545bb-122">hello web service expects 1 row at a time.</span></span> <span data-ttu-id="545bb-123">Datové sadě služby příkladu může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="545bb-123">An example dataset could look like this:</span></span>

![Ukázková data][1]

<span data-ttu-id="545bb-125">Předpokládejme, že uživatel chtěli tooseparate hello tato data do 3 vzájemně se vylučujících skupin.</span><span class="sxs-lookup"><span data-stu-id="545bb-125">Suppose hello user wanted tooseparate this data into 3 mutually exclusive groups.</span></span> <span data-ttu-id="545bb-126">Hello vstup hello výše datová sada by hello následující data: hodnota = "10, 5, 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3, 4"; k = "3".</span><span class="sxs-lookup"><span data-stu-id="545bb-126">hello data input for hello above dataset would be hello following: value = “10;5;2,18;1;6,7;5;5,22;3;4,12;2;1,10;3;4”; k=”3”.</span></span> <span data-ttu-id="545bb-127">výstup Hello je hello členství ve skupině předpokládaných pro každý hello řádků.</span><span class="sxs-lookup"><span data-stu-id="545bb-127">hello output is hello predicted group membership for each of hello rows.</span></span>

> <span data-ttu-id="545bb-128">Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="545bb-128">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="545bb-129">Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span><span class="sxs-lookup"><span data-stu-id="545bb-129">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="545bb-130">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="545bb-130">Starting C# code for web service consumption:</span></span>
    public class Input
    {
            public string value;
            public string k;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




## <a name="creation-of-web-service"></a><span data-ttu-id="545bb-131">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="545bb-131">Creation of web service</span></span>
> <span data-ttu-id="545bb-132">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="545bb-132">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="545bb-133">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="545bb-133">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="545bb-134">Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="545bb-134">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="545bb-135">Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly vyžádat do pracovního prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="545bb-135">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto hello workspace.</span></span> <span data-ttu-id="545bb-136">schéma dat Hello byl vytvořen s jednoduchou [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="545bb-136">hello data schema was created with a simple [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="545bb-137">Potom schéma dat hello byl odkazovaný toohello clusteru modelu části znovu vytvořit s [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="545bb-137">Then, hello data schema was linked toohello cluster model section, again created with an [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="545bb-138">V hello [spustit skript jazyka R] [ execute-r-script] používat pro model hello clusteru, hello webové služby pak využívá funkce hello "k-means", který je předem do hello [spustit skript jazyka R] [ execute-r-script] z Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="545bb-138">In hello [Execute R Script][execute-r-script] used for hello cluster model, hello web service then utilizes hello “k-means” function, which is prebuilt into hello [Execute R Script][execute-r-script] of Azure Machine Learning.</span></span>    

![Tok experimentu][3]

#### <a name="module-1"></a><span data-ttu-id="545bb-140">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="545bb-140">Module 1:</span></span>
    #Enter hello input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a><span data-ttu-id="545bb-141">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="545bb-141">Module 2:</span></span>
    # Map 1-based optional input ports toovariables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("mydatafinal");


## <a name="limitations"></a><span data-ttu-id="545bb-142">Omezení</span><span class="sxs-lookup"><span data-stu-id="545bb-142">Limitations</span></span>
<span data-ttu-id="545bb-143">Toto je velmi jednoduchý příklad clustering webové služby.</span><span class="sxs-lookup"><span data-stu-id="545bb-143">This is a very simple example of a clustering web service.</span></span> <span data-ttu-id="545bb-144">Jak je vidět z hello ukázkový kód výše, je implementována zachycení žádné chyby a hello služby předpokládá, že všechno, co je nepřetržitá proměnné (bez kategorií funkcí povoleny), jako hello služby pouze vstupy číselných hodnot v době hello hello vytvoření tomuto webovému Služba.</span><span class="sxs-lookup"><span data-stu-id="545bb-144">As can be seen from hello example code above, no error catching is implemented and hello service assumes everything is a continuous variable (no categorical features allowed), as hello service only inputs numeric values at hello time of hello creation of this web service.</span></span> <span data-ttu-id="545bb-145">Navíc služba hello aktuálně zpracovává velikost omezenou dat, z důvodu toohello požadavků a odpovědí povaha hello webové služby je se volání a hello skutečnost, že hello modelu odpovídat pokaždé, když je volána hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="545bb-145">Also, hello service currently handles limited data size, due toohello request/response nature of hello web service call and hello fact that hello model is being fit every time hello web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="545bb-146">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="545bb-146">FAQ</span></span>
<span data-ttu-id="545bb-147">Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="545bb-147">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

