---
title: "(nepoužívané) Machine learning webových služeb příklady vytvořené s R - Azure | Microsoft Docs"
description: "(nepoužívané) Najít užitečné sadu webové služby příklady jsou vytvořené pomocí kódu jazyka R a Machine Learning a publikované na webu Azure Marketplace."
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
redirect_document_id: TRUE
ms.openlocfilehash: 9514025db6f812f9e7934ea2d1575e948d6585b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-web-services-examples-using-r-code-on-azure-machine-learning-and-published-to-microsoft-azure-marketplace"></a><span data-ttu-id="23907-104">(nepoužívané) Webové služby příklady použití kódu jazyka R v Azure Machine Learning a publikované na webu Microsoft Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="23907-104">(deprecated) Web services examples using R code on Azure Machine Learning and published to Microsoft Azure Marketplace</span></span>

> [!NOTE]
> <span data-ttu-id="23907-105">Microsoft DataMarket se postupně vyřazuje z provozu a tato rozhraní API jsou zastaralé.</span><span class="sxs-lookup"><span data-stu-id="23907-105">The Microsoft DataMarket is being retired and these APIs have been deprecated.</span></span> 
> 
> <span data-ttu-id="23907-106">Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="23907-106">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="23907-107">Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="23907-107">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="23907-108">V tomto článku jsou třeba webové služby byly vytvořené pomocí Azure Machine Learning a publikována ve službě Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="23907-108">In this article are example web services were created using Azure Machine Learning and published to the Azure Marketplace.</span></span> <span data-ttu-id="23907-109">Každý webové službu příkladu má komplexní dokumentu připojené, vkládání ukázkových datových sad pro testování služeb a vysvětlení, jak uživatel může vytvořit podobnou službu sami.</span><span class="sxs-lookup"><span data-stu-id="23907-109">Each web service example has a comprehensive document attached, embedding sample data sets for testing the services and explaining how the user can create a similar service themselves.</span></span> 

<span data-ttu-id="23907-110">Uživatelé v Azure Machine Learning Studio, můžete napsat kód R a několika kliknutími publikujete ji jako webové služby pro aplikace a zařízení využívají po celém světě.</span><span class="sxs-lookup"><span data-stu-id="23907-110">In Azure Machine Learning Studio, users can write R code and with a few clicks, publish it as a web service for applications and devices to consume around the world.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="23907-111">Neměly jednoduché kalkulačky, které poskytují statistické funkce pro vytváření vlastní prediktivní analýzy postojích text dolování nové i zkušeného R uživatelé mohou mít prospěch ze usnadnění, pomocí kterého můžete uživatelům Azure Machine Learning zprovoznit R kód.</span><span class="sxs-lookup"><span data-stu-id="23907-111">From producing simple calculators that provide statistical functionality to creating a custom text-mining sentiment analysis predictor, both new and experienced R users can benefit from the ease with which users of Azure Machine Learning can operationalize R code.</span></span> <span data-ttu-id="23907-112">Tyto webové, které by mohly spotřebovat služby uživateli, potenciálně prostřednictvím mobilní aplikace nebo webu, účel tyto příklady webové služby je zobrazit, jak můžete zprovoznit Machine Learning R skriptů pro analytické účely a použije k vytvoření webové služby v horní části R kódu.</span><span class="sxs-lookup"><span data-stu-id="23907-112">While these web services could be consumed by users, potentially through a mobile app or a website, the purpose of these web services examples is to show how Machine Learning can operationalize R scripts for analytical purposes and be used to create web services on top of R code.</span></span>

<span data-ttu-id="23907-113">Každý příklad obsahuje příklad jazyka C# pro používání webové služby.</span><span class="sxs-lookup"><span data-stu-id="23907-113">Each example includes a C# example for web service consumption.</span></span>

![Diagram R kódu v Azure Machine Learning: R řešení pro chráněné pomocí publikování nebo v Azure Marketplace.][1]

<span data-ttu-id="23907-115">Zvažte následující scénáře.</span><span class="sxs-lookup"><span data-stu-id="23907-115">Consider the following scenarios.</span></span>

## <a name="scenario-1-generic-model"></a><span data-ttu-id="23907-116">Scénář 1: Obecné modelu</span><span class="sxs-lookup"><span data-stu-id="23907-116">Scenario 1: Generic model</span></span>
<span data-ttu-id="23907-117">Uživatel pracuje s obecné model, který lze použít pro nové uživatele data, například základní předpovědi časových řad dat nebo uživatelské metodu R s pokročilou analýzu.</span><span class="sxs-lookup"><span data-stu-id="23907-117">A user works with a generic model that can be applied to a new user’s data, such as a basic forecasting of time series data or a custom-built R method with advanced analytics.</span></span> <span data-ttu-id="23907-118">Tento uživatel publikuje model jako webovou službu, aby jej ostatní mohli používat s jejich daty.</span><span class="sxs-lookup"><span data-stu-id="23907-118">This user publishes the model as a web service for others to consume with their data.</span></span>

* [<span data-ttu-id="23907-119">Binární klasifikátor</span><span class="sxs-lookup"><span data-stu-id="23907-119">Binary Classifier</span></span>](machine-learning-r-csharp-binary-classifier.md)
* [<span data-ttu-id="23907-120">Model clusteru</span><span class="sxs-lookup"><span data-stu-id="23907-120">Cluster Model</span></span>](machine-learning-r-csharp-cluster-model.md)
* [<span data-ttu-id="23907-121">Lineární regrese s množstvím proměnných</span><span class="sxs-lookup"><span data-stu-id="23907-121">Multivariate Linear Regression</span></span>](machine-learning-r-csharp-multivariate-linear-regression.md)
* [<span data-ttu-id="23907-122">Vytváření prognóz – Exponenciální vyhlazování</span><span class="sxs-lookup"><span data-stu-id="23907-122">Forecasting - Exponential Smoothing</span></span>](machine-learning-r-csharp-forecasting-exponential-smoothing.md)
* [<span data-ttu-id="23907-123">ETS + prognózy STL</span><span class="sxs-lookup"><span data-stu-id="23907-123">ETS + STL Forecasting</span></span>](machine-learning-r-csharp-retail-demand-forecasting.md)
* [<span data-ttu-id="23907-124">Vytváření prognóz - Autoregressive integrované klouzavý průměr (ARIMA)</span><span class="sxs-lookup"><span data-stu-id="23907-124">Forecasting - Autoregressive Integrated Moving Average (ARIMA)</span></span>](machine-learning-r-csharp-arima.md)
* [<span data-ttu-id="23907-125">Analýza přežití</span><span class="sxs-lookup"><span data-stu-id="23907-125">Survival Analysis</span></span>](machine-learning-r-csharp-survival-analysis.md)

## <a name="scenario-2-trained-model--specific-data"></a><span data-ttu-id="23907-126">Scénář 2: Trained model – konkrétní data</span><span class="sxs-lookup"><span data-stu-id="23907-126">Scenario 2: Trained model – specific data</span></span>
<span data-ttu-id="23907-127">Uživatel, který má data, která poskytuje užitečné předpovědi prostřednictvím kódu jazyka R, jako je například velké vzorku charakteru dotazníků clusteru prostřednictvím k-means algoritmus k předvídání uživatele charakteru typu nebo stavu průzkum data, která mohou být použity k předpovědi riziko jednotlivého uživatele pro plicní rakoviny s balíčkem analysis R základními.</span><span class="sxs-lookup"><span data-stu-id="23907-127">A user has data that provides useful predictions through R code, such as a large sample of personality questionnaires clustered through a k-means algorithm to predict the user’s personality type, or health survey data that can be used to predict an individual’s risk for lung cancer with a survival analysis R package.</span></span> <span data-ttu-id="23907-128">Uživatel publikuje data prostřednictvím webové služby, který bude předpovídat výsledek nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="23907-128">The user publishes the data through a web service that predicts a new user’s outcome.</span></span>

## <a name="scenario-3-trained-model--generic-data"></a><span data-ttu-id="23907-129">Scénář 3: Trained model – generických dat.</span><span class="sxs-lookup"><span data-stu-id="23907-129">Scenario 3: Trained model – generic data</span></span>
<span data-ttu-id="23907-130">Uživatel má obecný data (například svátek text), která umožňuje webové služby vytvořené a obecně uplatnit na různé typy případy použití a scénáře.</span><span class="sxs-lookup"><span data-stu-id="23907-130">A user has generic data (such as a text corpus) that allows a web service to be built and applied generically across different types of use cases and scenarios.</span></span>

* [<span data-ttu-id="23907-131">Analýza mínění na základě slovníku</span><span class="sxs-lookup"><span data-stu-id="23907-131">Lexicon Based Sentiment Analysis</span></span>](machine-learning-r-csharp-lexicon-based-sentiment-analysis.md)

## <a name="scenario-4-advanced-calculator"></a><span data-ttu-id="23907-132">Scénář 4: Pokročilé kalkulačky</span><span class="sxs-lookup"><span data-stu-id="23907-132">Scenario 4: Advanced calculator</span></span>
<span data-ttu-id="23907-133">Uživatel poskytuje pokročilé výpočty nebo simulace, které nevyžadují žádné trained model, nebo přizpůsobování modelu k datům uživatele.</span><span class="sxs-lookup"><span data-stu-id="23907-133">A user provides advanced calculations or simulations that don’t require any trained model or fitting of a model to the user’s data.</span></span>

* [<span data-ttu-id="23907-134">Testování rozdílu v proporcích</span><span class="sxs-lookup"><span data-stu-id="23907-134">Difference in Proportions Test</span></span>](machine-learning-r-csharp-difference-in-two-proportions.md)
* [<span data-ttu-id="23907-135">Sada normální distribuce</span><span class="sxs-lookup"><span data-stu-id="23907-135">Normal Distribution Suite</span></span>](machine-learning-r-csharp-normal-distribution.md)
* [<span data-ttu-id="23907-136">Sada binomické distribuce</span><span class="sxs-lookup"><span data-stu-id="23907-136">Binomial Distribution Suite</span></span>](machine-learning-r-csharp-binomial-distribution.md)

## <a name="faq"></a><span data-ttu-id="23907-137">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="23907-137">FAQ</span></span>
<span data-ttu-id="23907-138">Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Marketplace, najdete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="23907-138">For frequently asked questions on consumption of the web service or publishing to the Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-web-service-examples/machine-learning-r-code-options-for-using-and-sharing-cloud.png



