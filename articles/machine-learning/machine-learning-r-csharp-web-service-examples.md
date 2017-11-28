---
title: "AAA(Deprecated) Machine learning webové služby vytvořené s R - Azure příklady | Microsoft Docs"
description: "(nepoužívané) Najdete užitečné sada webových služeb příklady jsou vytvořené pomocí kódu jazyka R a Machine Learning a publikovaná toohello Azure Marketplace."
keywords: "CSharp, kódu jazyka r příklady webové služby"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 97d66cb7-6a84-4ae9-8095-0b5f5ba82d7f
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
ms.openlocfilehash: 20b074d38e65aed907d40549bb61f124cb5dfe1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-web-services-examples-using-r-code-on-azure-machine-learning-and-published-toomicrosoft-azure-marketplace"></a><span data-ttu-id="1ba0f-104">(nepoužívané) Webové služby příklady použití kódu jazyka R na Azure Machine Learning a publikovaná tooMicrosoft Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="1ba0f-104">(deprecated) Web services examples using R code on Azure Machine Learning and published tooMicrosoft Azure Marketplace</span></span>

> [!NOTE]
> <span data-ttu-id="1ba0f-105">Hello Microsoft DataMarket se postupně vyřazuje z provozu a tato rozhraní API jsou zastaralé.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-105">hello Microsoft DataMarket is being retired and these APIs have been deprecated.</span></span> 
> 
> <span data-ttu-id="1ba0f-106">Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="1ba0f-106">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="1ba0f-107">Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="1ba0f-107">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="1ba0f-108">V tomto článku jsou třeba webové služby byly vytvořené pomocí Azure Machine Learning a publikovaná toohello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-108">In this article are example web services were created using Azure Machine Learning and published toohello Azure Marketplace.</span></span> <span data-ttu-id="1ba0f-109">Každý webové službu příkladu má komplexní dokumentu připojené, vkládání ukázkových datových sad pro testování hello služby a vysvětlení, jak hello uživatel může vytvářet podobnou službu sami.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-109">Each web service example has a comprehensive document attached, embedding sample data sets for testing hello services and explaining how hello user can create a similar service themselves.</span></span> 

<span data-ttu-id="1ba0f-110">V nástroji Azure Machine Learning Studio uživatelé napsat kód R a několika kliknutími publikujete ji jako webovou službu pro aplikace a zařízení tooconsume kolem hello, world.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-110">In Azure Machine Learning Studio, users can write R code and with a few clicks, publish it as a web service for applications and devices tooconsume around hello world.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="1ba0f-111">Neměly jednoduché kalkulačky, které poskytují toocreating statistické funkce vlastní prediktivní analýzy postojích text dolování nové i zkušeného R uživatelé mohou mít prospěch ze hello usnadnění, pomocí kterého můžete uživatelům Azure Machine Learning zprovoznit R kód.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-111">From producing simple calculators that provide statistical functionality toocreating a custom text-mining sentiment analysis predictor, both new and experienced R users can benefit from hello ease with which users of Azure Machine Learning can operationalize R code.</span></span> <span data-ttu-id="1ba0f-112">Když tyto webové služby, které by mohly spotřebovat uživateli, potenciálně prostřednictvím mobilní aplikace nebo webu, hello účel těchto webových služeb, příklady je tooshow, jak můžete zprovoznit Machine Learning R skriptů pro analytické účely a být použité toocreate webové služby na začátek kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-112">While these web services could be consumed by users, potentially through a mobile app or a website, hello purpose of these web services examples is tooshow how Machine Learning can operationalize R scripts for analytical purposes and be used toocreate web services on top of R code.</span></span>

<span data-ttu-id="1ba0f-113">Každý příklad obsahuje příklad jazyka C# pro používání webové služby.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-113">Each example includes a C# example for web service consumption.</span></span>

![Diagram R kódu v Azure Machine Learning: R řešení pro vlastní použití nebo publikované toohello Azure Marketplace.][1]

<span data-ttu-id="1ba0f-115">Zvažte následující scénáře hello.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-115">Consider hello following scenarios.</span></span>

## <a name="scenario-1-generic-model"></a><span data-ttu-id="1ba0f-116">Scénář 1: Obecné modelu</span><span class="sxs-lookup"><span data-stu-id="1ba0f-116">Scenario 1: Generic model</span></span>
<span data-ttu-id="1ba0f-117">Uživatel pracuje s obecné model, který může být dat použité tooa nového uživatele, například základní předpovědi časových řad dat nebo uživatelské metodu R s pokročilou analýzu.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-117">A user works with a generic model that can be applied tooa new user’s data, such as a basic forecasting of time series data or a custom-built R method with advanced analytics.</span></span> <span data-ttu-id="1ba0f-118">Tento uživatel publikuje hello model jako webovou službu pro ostatní tooconsume s jejich daty.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-118">This user publishes hello model as a web service for others tooconsume with their data.</span></span>

* [<span data-ttu-id="1ba0f-119">Binární klasifikátor</span><span class="sxs-lookup"><span data-stu-id="1ba0f-119">Binary Classifier</span></span>](machine-learning-r-csharp-binary-classifier.md)
* [<span data-ttu-id="1ba0f-120">Model clusteru</span><span class="sxs-lookup"><span data-stu-id="1ba0f-120">Cluster Model</span></span>](machine-learning-r-csharp-cluster-model.md)
* [<span data-ttu-id="1ba0f-121">Lineární regrese s množstvím proměnných</span><span class="sxs-lookup"><span data-stu-id="1ba0f-121">Multivariate Linear Regression</span></span>](machine-learning-r-csharp-multivariate-linear-regression.md)
* [<span data-ttu-id="1ba0f-122">Vytváření prognóz – Exponenciální vyhlazování</span><span class="sxs-lookup"><span data-stu-id="1ba0f-122">Forecasting - Exponential Smoothing</span></span>](machine-learning-r-csharp-forecasting-exponential-smoothing.md)
* [<span data-ttu-id="1ba0f-123">ETS + prognózy STL</span><span class="sxs-lookup"><span data-stu-id="1ba0f-123">ETS + STL Forecasting</span></span>](machine-learning-r-csharp-retail-demand-forecasting.md)
* [<span data-ttu-id="1ba0f-124">Vytváření prognóz - Autoregressive integrované klouzavý průměr (ARIMA)</span><span class="sxs-lookup"><span data-stu-id="1ba0f-124">Forecasting - Autoregressive Integrated Moving Average (ARIMA)</span></span>](machine-learning-r-csharp-arima.md)
* [<span data-ttu-id="1ba0f-125">Analýza přežití</span><span class="sxs-lookup"><span data-stu-id="1ba0f-125">Survival Analysis</span></span>](machine-learning-r-csharp-survival-analysis.md)

## <a name="scenario-2-trained-model--specific-data"></a><span data-ttu-id="1ba0f-126">Scénář 2: Trained model – konkrétní data</span><span class="sxs-lookup"><span data-stu-id="1ba0f-126">Scenario 2: Trained model – specific data</span></span>
<span data-ttu-id="1ba0f-127">Uživatel, který má data, která poskytuje užitečné předpovědi prostřednictvím kódu jazyka R, například velké vzorku charakteru dotazníků clusterovaný prostřednictvím uživatele k-means algoritmus toopredict hello charakteru typu nebo stavu prozkoumáte data, která lze použít toopredict jednotlivého uživatele riziko pro plicní rakoviny s balíčkem analysis R základními.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-127">A user has data that provides useful predictions through R code, such as a large sample of personality questionnaires clustered through a k-means algorithm toopredict hello user’s personality type, or health survey data that can be used toopredict an individual’s risk for lung cancer with a survival analysis R package.</span></span> <span data-ttu-id="1ba0f-128">uživatel Hello publikuje data hello prostřednictvím webové služby, který bude předpovídat výsledek nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-128">hello user publishes hello data through a web service that predicts a new user’s outcome.</span></span>

## <a name="scenario-3-trained-model--generic-data"></a><span data-ttu-id="1ba0f-129">Scénář 3: Trained model – generických dat.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-129">Scenario 3: Trained model – generic data</span></span>
<span data-ttu-id="1ba0f-130">Uživatel má obecné data (například svátek text), který umožňuje toobe webové služby vytvořené a u obecně do různých typů případy použití a scénáře.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-130">A user has generic data (such as a text corpus) that allows a web service toobe built and applied generically across different types of use cases and scenarios.</span></span>

* [<span data-ttu-id="1ba0f-131">Analýza mínění na základě slovníku</span><span class="sxs-lookup"><span data-stu-id="1ba0f-131">Lexicon Based Sentiment Analysis</span></span>](machine-learning-r-csharp-lexicon-based-sentiment-analysis.md)

## <a name="scenario-4-advanced-calculator"></a><span data-ttu-id="1ba0f-132">Scénář 4: Pokročilé kalkulačky</span><span class="sxs-lookup"><span data-stu-id="1ba0f-132">Scenario 4: Advanced calculator</span></span>
<span data-ttu-id="1ba0f-133">Uživatel poskytuje pokročilé výpočty nebo simulace, které nevyžadují žádné trained model, nebo přizpůsobování modelu toohello uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="1ba0f-133">A user provides advanced calculations or simulations that don’t require any trained model or fitting of a model toohello user’s data.</span></span>

* [<span data-ttu-id="1ba0f-134">Testování rozdílu v proporcích</span><span class="sxs-lookup"><span data-stu-id="1ba0f-134">Difference in Proportions Test</span></span>](machine-learning-r-csharp-difference-in-two-proportions.md)
* [<span data-ttu-id="1ba0f-135">Sada normální distribuce</span><span class="sxs-lookup"><span data-stu-id="1ba0f-135">Normal Distribution Suite</span></span>](machine-learning-r-csharp-normal-distribution.md)
* [<span data-ttu-id="1ba0f-136">Sada binomické distribuce</span><span class="sxs-lookup"><span data-stu-id="1ba0f-136">Binomial Distribution Suite</span></span>](machine-learning-r-csharp-binomial-distribution.md)

## <a name="faq"></a><span data-ttu-id="1ba0f-137">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="1ba0f-137">FAQ</span></span>
<span data-ttu-id="1ba0f-138">Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Marketplace, najdete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="1ba0f-138">For frequently asked questions on consumption of hello web service or publishing toohello Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-web-service-examples/machine-learning-r-code-options-for-using-and-sharing-cloud.png



