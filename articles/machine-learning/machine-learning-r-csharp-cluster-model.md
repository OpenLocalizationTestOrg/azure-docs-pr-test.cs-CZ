---
title: "(nepoužívané) Cluster Model – Azure | Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 7cbbbd6d4236dab638eb3051595a584557480841
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-cluster-model"></a><span data-ttu-id="a8cfc-103">(nepoužívané) Model clusteru</span><span class="sxs-lookup"><span data-stu-id="a8cfc-103">(deprecated) Cluster Model</span></span>

> [!NOTE]
> <span data-ttu-id="a8cfc-104">Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="a8cfc-105">Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="a8cfc-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="a8cfc-106">Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="a8cfc-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="a8cfc-107">Jak jsme předpovědi skupiny chování držitelům platební karty snižte riziko poplatků vypnout vystavitelů platební karty?</span><span class="sxs-lookup"><span data-stu-id="a8cfc-107">How can we predict groups of credit cardholders’ behaviors in order to reduce the charge-off risk of credit card issuers?</span></span> <span data-ttu-id="a8cfc-108">Jak jsme definovat skupiny charakteru vlastnosti zaměstnanců za účelem zlepšení výkonu v práci?</span><span class="sxs-lookup"><span data-stu-id="a8cfc-108">How can we define groups of personality traits of employees in order to improve their performance at work?</span></span> <span data-ttu-id="a8cfc-109">Jak můžete lékařů klasifikovat pacientů do skupin na základě charakteristik jejich nákaz?</span><span class="sxs-lookup"><span data-stu-id="a8cfc-109">How can doctors classify patients into groups based on the characteristics of their diseases?</span></span> <span data-ttu-id="a8cfc-110">V zásadě všechny tyto otázky může odpovědět na tyto otázky prostřednictvím analýzy clusteru.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-110">In principle, all of these questions can be answered through cluster analysis.</span></span>   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="a8cfc-111">Analýza clusteru klasifikuje sadu připomínky do dvou nebo více vzájemně se vylučuje neznámé skupin na základě kombinace proměnné.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-111">Cluster analysis classifies a set of observations into two or more mutually exclusive unknown groups based on combinations of variables.</span></span> <span data-ttu-id="a8cfc-112">Účelem analýzy clusteru je zjistit systém uspořádání připomínky, obvykle osoby nebo vlastností jsou do skupiny, kde se členy skupin sdílí vlastnosti společné.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-112">The purpose of cluster analysis is to discover a system of organizing observations, usually people or their characteristics, into groups, where members of the groups share properties in common.</span></span> <span data-ttu-id="a8cfc-113">To [služby](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) používá K-Means metodika, běžně používané clustering techniku, do clusteru libovolná data do skupin.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-113">This [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) uses the K-Means methodology, a commonly used clustering technique, to cluster arbitrary data into groups.</span></span> <span data-ttu-id="a8cfc-114">Tato webová služba přijímá data a počet tisíc clusterů jako vstup a vytváří předpovědi, které k skupin, do kterých patří každý připomínky.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-114">This web service takes the data and the number of k clusters as input, and produces predictions of which of the k groups to which each observations belongs.</span></span> 

> <span data-ttu-id="a8cfc-115">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-115">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="a8cfc-116">Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-116">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="a8cfc-117">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-117">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="a8cfc-118">Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-118">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="a8cfc-119">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="a8cfc-119">Consumption of web service</span></span>
<span data-ttu-id="a8cfc-120">Seskupení dat do sady skupin k této webové služby a výstupy přiřazení skupiny pro každý řádek.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-120">This web service groups the data into a set of k groups and outputs the group assignment for each row.</span></span> <span data-ttu-id="a8cfc-121">Webová služba očekává, že koncovému uživateli umožňují vstupní data jako řetězec, kde jsou řádky oddělených čárkami (,) a sloupce jsou odděleny středníkem (;).</span><span class="sxs-lookup"><span data-stu-id="a8cfc-121">The web service expects the end user to input data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="a8cfc-122">Webová služba očekává 1 řádek najednou.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-122">The web service expects 1 row at a time.</span></span> <span data-ttu-id="a8cfc-123">Datové sadě služby příkladu může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="a8cfc-123">An example dataset could look like this:</span></span>

![Ukázková data][1]

<span data-ttu-id="a8cfc-125">Předpokládejme, že uživatel chtěli oddělení tato data do 3 vzájemně se vylučujících skupin.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-125">Suppose the user wanted to separate this data into 3 mutually exclusive groups.</span></span> <span data-ttu-id="a8cfc-126">Data vstup výše datová sada může být následující: hodnota = "10, 5, 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3, 4"; k = "3".</span><span class="sxs-lookup"><span data-stu-id="a8cfc-126">The data input for the above dataset would be the following: value = “10;5;2,18;1;6,7;5;5,22;3;4,12;2;1,10;3;4”; k=”3”.</span></span> <span data-ttu-id="a8cfc-127">Výstup je členství ve skupině předpokládaných pro jednotlivé řádky.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-127">The output is the predicted group membership for each of the rows.</span></span>

> <span data-ttu-id="a8cfc-128">Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-128">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="a8cfc-129">Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span><span class="sxs-lookup"><span data-stu-id="a8cfc-129">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="a8cfc-130">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="a8cfc-130">Starting C# code for web service consumption:</span></span>
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




## <a name="creation-of-web-service"></a><span data-ttu-id="a8cfc-131">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="a8cfc-131">Creation of web service</span></span>
> <span data-ttu-id="a8cfc-132">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-132">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="a8cfc-133">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="a8cfc-133">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="a8cfc-134">Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-134">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="a8cfc-135">Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly vyžádat na pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-135">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto the workspace.</span></span> <span data-ttu-id="a8cfc-136">Schéma dat byl vytvořen s jednoduchou [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="a8cfc-136">The data schema was created with a simple [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="a8cfc-137">Potom se schéma dat propojené s části model clusteru znovu vytvořit s [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="a8cfc-137">Then, the data schema was linked to the cluster model section, again created with an [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="a8cfc-138">V [spustit skript jazyka R] [ execute-r-script] použít pro model clusteru, webovou službu pak využívá funkce "k-means", který je předem do [spustit skript jazyka R] [ execute-r-script] z Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-138">In the [Execute R Script][execute-r-script] used for the cluster model, the web service then utilizes the “k-means” function, which is prebuilt into the [Execute R Script][execute-r-script] of Azure Machine Learning.</span></span>    

![Tok experimentu][3]

#### <a name="module-1"></a><span data-ttu-id="a8cfc-140">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="a8cfc-140">Module 1:</span></span>
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a><span data-ttu-id="a8cfc-141">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="a8cfc-141">Module 2:</span></span>
    # Map 1-based optional input ports to variables
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

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");


## <a name="limitations"></a><span data-ttu-id="a8cfc-142">Omezení</span><span class="sxs-lookup"><span data-stu-id="a8cfc-142">Limitations</span></span>
<span data-ttu-id="a8cfc-143">Toto je velmi jednoduchý příklad clustering webové služby.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-143">This is a very simple example of a clustering web service.</span></span> <span data-ttu-id="a8cfc-144">Jak je vidět z výše uvedeném příkladu kódu, je implementovaný zachycení žádné chyby a službu předpokládá, že všechno, co je nepřetržitá proměnné (bez kategorií funkcí povoleny), jako služba číselné hodnoty vstupy pouze v době vytvoření této webové služby.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-144">As can be seen from the example code above, no error catching is implemented and the service assumes everything is a continuous variable (no categorical features allowed), as the service only inputs numeric values at the time of the creation of this web service.</span></span> <span data-ttu-id="a8cfc-145">Navíc služba aktuálně zpracovává velikost omezenou dat, vyřízení požadavků a odpovědí povaze volání webové služby a fakt, že je probíhá podle modelu pokaždé, když je volat webovou službu.</span><span class="sxs-lookup"><span data-stu-id="a8cfc-145">Also, the service currently handles limited data size, due to the request/response nature of the web service call and the fact that the model is being fit every time the web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="a8cfc-146">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="a8cfc-146">FAQ</span></span>
<span data-ttu-id="a8cfc-147">Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="a8cfc-147">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
