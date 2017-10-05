---
title: "Jak používat Azure Machine Learning webové služby | Microsoft Docs"
description: "Po nasazení machine learning služby mohou být využívány RESTFul webové služby, která je k dispozici jako službu v reálném čase požadavků a odpovědí, nebo jako spuštění služby batch."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 804f8211-9437-4982-98e9-ca841b7edf56
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/02/2017
ms.author: garye
ms.openlocfilehash: eec9f637b4b2306ab4a888dbd5ef5b9a021bcac5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-consume-an-azure-machine-learning-web-service"></a><span data-ttu-id="8723b-103">Jak používat Azure Machine Learning webové služby</span><span class="sxs-lookup"><span data-stu-id="8723b-103">How to consume an Azure Machine Learning Web service</span></span>

<span data-ttu-id="8723b-104">Po nasazení Azure Machine Learning prediktivní model jako webovou službu, můžete odeslat data a získat předpovědi rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="8723b-104">Once you deploy an Azure Machine Learning predictive model as a Web service, you can use a REST API to send it data and get predictions.</span></span> <span data-ttu-id="8723b-105">Můžete odeslat data v reálném čase nebo v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="8723b-105">You can send the data in real-time or in batch mode.</span></span>

<span data-ttu-id="8723b-106">Můžete najít další informace o tom, jak vytvořit a nasadit Machine Learning webové služby pomocí nástroje Machine Learning Studio tady:</span><span class="sxs-lookup"><span data-stu-id="8723b-106">You can find more information about how to create and deploy a Machine Learning Web service using Machine Learning Studio here:</span></span>

* <span data-ttu-id="8723b-107">Kurz týkající se vytvoření experimentu v nástroji Machine Learning Studio, najdete v části [vytvoření prvního experimentu](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="8723b-107">For a tutorial on how to create an experiment in Machine Learning Studio, see [Create your first experiment](machine-learning-create-experiment.md).</span></span>
* <span data-ttu-id="8723b-108">Podrobnosti o tom, jak nasadit webovou službu najdete v tématu [nasazení služby Machine Learning webové](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="8723b-108">For details on how to deploy a Web service, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="8723b-109">Další informace o Machine Learning obecně naleznete [dokumentace k centru pro Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="8723b-109">For more information about Machine Learning in general, visit the [Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a><span data-ttu-id="8723b-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="8723b-110">Overview</span></span>
<span data-ttu-id="8723b-111">Pomocí Azure Machine Learning webové služby externí aplikace komunikuje se vyhodnocování model Machine Learning pracovního postupu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="8723b-111">With the Azure Machine Learning Web service, an external application communicates with a Machine Learning workflow scoring model in real time.</span></span> <span data-ttu-id="8723b-112">Volání Machine Learning webové služby vrátí výsledky předpovědi externí aplikací.</span><span class="sxs-lookup"><span data-stu-id="8723b-112">A Machine Learning Web service call returns prediction results to an external application.</span></span> <span data-ttu-id="8723b-113">Chcete-li volání Machine Learning webové služby, předáte klíč rozhraní API, která se vytvoří při nasazení předpovědi.</span><span class="sxs-lookup"><span data-stu-id="8723b-113">To make a Machine Learning Web service call, you pass an API key that is created when you deploy a prediction.</span></span> <span data-ttu-id="8723b-114">Machine Learning webové služby je založena na REST, možnost populární architektury pro webové projekty programování.</span><span class="sxs-lookup"><span data-stu-id="8723b-114">The Machine Learning Web service is based on REST, a popular architecture choice for web programming projects.</span></span>

<span data-ttu-id="8723b-115">Azure Machine Learning zahrnuje dva typy služeb:</span><span class="sxs-lookup"><span data-stu-id="8723b-115">Azure Machine Learning has two types of services:</span></span>

* <span data-ttu-id="8723b-116">Požadavků a odpovědí služby (záznamy RR) – s nízkou latencí, vysoce škálovatelná služba, která poskytuje rozhraní pro bezstavové modely vytvořené a nasazené z Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="8723b-116">Request-Response Service (RRS) – A low latency, highly scalable service that provides an interface to the stateless models created and deployed from the Machine Learning Studio.</span></span>
* <span data-ttu-id="8723b-117">Spuštění služby Batch (BES) – asynchronní služby tohoto skóre a dávky pro datových záznamů.</span><span class="sxs-lookup"><span data-stu-id="8723b-117">Batch Execution Service (BES) – An asynchronous service that scores a batch for data records.</span></span>

<span data-ttu-id="8723b-118">Další informace o Machine Learning webových služeb najdete v tématu [nasazení služby Machine Learning webové](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="8723b-118">For more information about Machine Learning Web services, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="get-an-azure-machine-learning-authorization-key"></a><span data-ttu-id="8723b-119">Získání klíče autorizace Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="8723b-119">Get an Azure Machine Learning authorization key</span></span>
<span data-ttu-id="8723b-120">Když nasadíte experimentu, klíče rozhraní API se generují pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="8723b-120">When you deploy your experiment, API keys are generated for the Web service.</span></span> <span data-ttu-id="8723b-121">Klíče můžete načíst z několika umístění.</span><span class="sxs-lookup"><span data-stu-id="8723b-121">You can retrieve the keys from several locations.</span></span>

### <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a><span data-ttu-id="8723b-122">Z portálu Microsoft Azure Machine Learning webové služby</span><span class="sxs-lookup"><span data-stu-id="8723b-122">From the Microsoft Azure Machine Learning Web Services portal</span></span>
<span data-ttu-id="8723b-123">Přihlaste se k [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net) portálu.</span><span class="sxs-lookup"><span data-stu-id="8723b-123">Sign in to the [Microsoft Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span>

<span data-ttu-id="8723b-124">Načíst klíč rozhraní API pro nové Machine Learning webové služby:</span><span class="sxs-lookup"><span data-stu-id="8723b-124">To retrieve the API key for a New Machine Learning Web service:</span></span>

1. <span data-ttu-id="8723b-125">Na portálu webové služby Azure Machine Learning, klikněte na tlačítko **webové služby** v hlavní nabídce.</span><span class="sxs-lookup"><span data-stu-id="8723b-125">In the Azure Machine Learning Web Services portal, click **Web Services** the top menu.</span></span>
2. <span data-ttu-id="8723b-126">Klikněte na webovou službu, pro který chcete načíst klíč.</span><span class="sxs-lookup"><span data-stu-id="8723b-126">Click the Web service for which you want to retrieve the key.</span></span>
3. <span data-ttu-id="8723b-127">V horní nabídce klikněte na tlačítko **spotřebě**.</span><span class="sxs-lookup"><span data-stu-id="8723b-127">On the top menu, click **Consume**.</span></span>
4. <span data-ttu-id="8723b-128">Zkopírujte a uložte **primární klíč**.</span><span class="sxs-lookup"><span data-stu-id="8723b-128">Copy and save the **Primary Key**.</span></span>

<span data-ttu-id="8723b-129">Načíst klíč rozhraní API pro Classic Machine Learning webové služby:</span><span class="sxs-lookup"><span data-stu-id="8723b-129">To retrieve the API key for a Classic Machine Learning Web service:</span></span>

1. <span data-ttu-id="8723b-130">Na portálu webové služby Azure Machine Learning, klikněte na tlačítko **Classic webové služby** v hlavní nabídce.</span><span class="sxs-lookup"><span data-stu-id="8723b-130">In the Azure Machine Learning Web Services portal, click **Classic Web Services** the top menu.</span></span>
2. <span data-ttu-id="8723b-131">Klikněte na webovou službu, se kterým pracujete.</span><span class="sxs-lookup"><span data-stu-id="8723b-131">Click the Web service with which you are working.</span></span>
3. <span data-ttu-id="8723b-132">Klikněte na koncový bod, pro který chcete načíst klíč.</span><span class="sxs-lookup"><span data-stu-id="8723b-132">Click the endpoint for which you want to retrieve the key.</span></span>
4. <span data-ttu-id="8723b-133">V horní nabídce klikněte na tlačítko **spotřebě**.</span><span class="sxs-lookup"><span data-stu-id="8723b-133">On the top menu, click **Consume**.</span></span>
5. <span data-ttu-id="8723b-134">Zkopírujte a uložte **primární klíč**.</span><span class="sxs-lookup"><span data-stu-id="8723b-134">Copy and save the **Primary Key**.</span></span>

### <a name="classic-web-service"></a><span data-ttu-id="8723b-135">Classic webové služby</span><span class="sxs-lookup"><span data-stu-id="8723b-135">Classic Web service</span></span>
 <span data-ttu-id="8723b-136">Můžete také načíst klíč pro Classic webové služby Machine Learning Studio nebo portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="8723b-136">You can also retrieve a key for a Classic Web service from Machine Learning Studio or the Azure classic portal.</span></span>

#### <a name="machine-learning-studio"></a><span data-ttu-id="8723b-137">Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="8723b-137">Machine Learning Studio</span></span>
1. <span data-ttu-id="8723b-138">V nástroji Machine Learning Studio, klikněte na tlačítko **webové služby** na levé straně.</span><span class="sxs-lookup"><span data-stu-id="8723b-138">In Machine Learning Studio, click **WEB SERVICES** on the left.</span></span>
2. <span data-ttu-id="8723b-139">Klikněte na webovou službu.</span><span class="sxs-lookup"><span data-stu-id="8723b-139">Click a Web service.</span></span> <span data-ttu-id="8723b-140">**Klíč rozhraní API** na **řídicí panel** kartě.</span><span class="sxs-lookup"><span data-stu-id="8723b-140">The **API key** is on the **DASHBOARD** tab.</span></span>

#### <a name="azure-classic-portal"></a><span data-ttu-id="8723b-141">klasický portál Azure</span><span class="sxs-lookup"><span data-stu-id="8723b-141">Azure classic portal</span></span>
1. <span data-ttu-id="8723b-142">Klikněte na tlačítko **MACHINE LEARNING** na levé straně.</span><span class="sxs-lookup"><span data-stu-id="8723b-142">Click **MACHINE LEARNING** on the left.</span></span>
2. <span data-ttu-id="8723b-143">Klikněte na pracovním prostoru, ve kterém se nachází webové služby.</span><span class="sxs-lookup"><span data-stu-id="8723b-143">Click the workspace in which your Web service is located.</span></span>
3. <span data-ttu-id="8723b-144">Klikněte na tlačítko **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="8723b-144">Click **WEB SERVICES**.</span></span>
4. <span data-ttu-id="8723b-145">Klikněte na webovou službu.</span><span class="sxs-lookup"><span data-stu-id="8723b-145">Click a Web service.</span></span>
5. <span data-ttu-id="8723b-146">Klikněte na koncový bod.</span><span class="sxs-lookup"><span data-stu-id="8723b-146">Click an endpoint.</span></span> <span data-ttu-id="8723b-147">"API KEY" je mimo provoz v pravém dolním.</span><span class="sxs-lookup"><span data-stu-id="8723b-147">The “API KEY” is down at the lower-right.</span></span>

## <span data-ttu-id="8723b-148"><a id="connect"></a>Připojení k webové služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="8723b-148"><a id="connect"></a>Connect to a Machine Learning Web service</span></span>
<span data-ttu-id="8723b-149">Můžete připojit k službě Machine Learning webové pomocí programovací jazyk, který podporuje žádostí HTTP a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="8723b-149">You can connect to a Machine Learning Web service using any programming language that supports HTTP request and response.</span></span> <span data-ttu-id="8723b-150">Příklady můžete zobrazit v C#, Python a R z Machine Learning webové stránky nápovědy služby.</span><span class="sxs-lookup"><span data-stu-id="8723b-150">You can view examples in C#, Python, and R from a Machine Learning Web service help page.</span></span>

<span data-ttu-id="8723b-151">**Počítač Learning API nápovědy** Machine Learning API nápovědy se vytvoří při nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="8723b-151">**Machine Learning API help** Machine Learning API help is created when you deploy a Web service.</span></span> <span data-ttu-id="8723b-152">V tématu [Azure Machine Learning návod - nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="8723b-152">See [Azure Machine Learning Walkthrough- Deploy Web Service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
<span data-ttu-id="8723b-153">Machine Learning API nápovědy obsahuje podrobnosti o předpovědi webové služby.</span><span class="sxs-lookup"><span data-stu-id="8723b-153">The Machine Learning API help contains details about a prediction Web service.</span></span>

1. <span data-ttu-id="8723b-154">Klikněte na webovou službu, se kterým pracujete.</span><span class="sxs-lookup"><span data-stu-id="8723b-154">Click the Web service with which you are working.</span></span>
2. <span data-ttu-id="8723b-155">Klikněte na koncový bod, pro který chcete zobrazit stránce nápovědy k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8723b-155">Click the endpoint for which you want to view the API Help Page.</span></span>
3. <span data-ttu-id="8723b-156">V horní nabídce klikněte na tlačítko **spotřebě**.</span><span class="sxs-lookup"><span data-stu-id="8723b-156">On the top menu, click **Consume**.</span></span>
4. <span data-ttu-id="8723b-157">Klikněte na tlačítko **stránku nápovědy rozhraní API** pod buď požadavků a odpovědí nebo Batch Execution koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="8723b-157">Click **API help page** under either the Request-Response or Batch Execution endpoints.</span></span>

<span data-ttu-id="8723b-158">**Zobrazení Machine Learning API pomoci pro novou webovou službu**</span><span class="sxs-lookup"><span data-stu-id="8723b-158">**To view Machine Learning API help for a New Web service**</span></span>

<span data-ttu-id="8723b-159">V Azure Machine Learning webový portál služby:</span><span class="sxs-lookup"><span data-stu-id="8723b-159">In the Azure Machine Learning Web Services Portal:</span></span>

1. <span data-ttu-id="8723b-160">Klikněte na tlačítko **webové služby** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="8723b-160">Click **WEB SERVICES** on the top menu.</span></span>
2. <span data-ttu-id="8723b-161">Klikněte na webovou službu, pro který chcete načíst klíč.</span><span class="sxs-lookup"><span data-stu-id="8723b-161">Click the Web service for which you want to retrieve the key.</span></span>

<span data-ttu-id="8723b-162">Klikněte na tlačítko **spotřebě** získat identifikátory URI pro požadavek Reposonse a spuštění služby Batch a ukázkový kód v jazyce C#, R a Python.</span><span class="sxs-lookup"><span data-stu-id="8723b-162">Click **Consume** to get the URIs for the Request-Reposonse and Batch Execution Services and Sample code in C#, R, and Python.</span></span>

<span data-ttu-id="8723b-163">Klikněte na tlačítko **rozhraní API Swaggeru** získat Swagger základě dokumentace pro rozhraní API volat z zadané identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="8723b-163">Click **Swagger API** to get Swagger based documentation for the APIs called from the supplied URIs.</span></span>

### <a name="c-sample"></a><span data-ttu-id="8723b-164">Ukázka C#</span><span class="sxs-lookup"><span data-stu-id="8723b-164">C# Sample</span></span>
<span data-ttu-id="8723b-165">Pro připojení k webové služby Machine Learning, použijte **HttpClient** předávání ScoreData.</span><span class="sxs-lookup"><span data-stu-id="8723b-165">To connect to a Machine Learning Web service, use an **HttpClient** passing ScoreData.</span></span> <span data-ttu-id="8723b-166">ScoreData obsahuje FeatureVector, n dimenzí vektor číselné funkce představující ScoreData.</span><span class="sxs-lookup"><span data-stu-id="8723b-166">ScoreData contains a FeatureVector, an n-dimensional vector of numerical features that represents the ScoreData.</span></span> <span data-ttu-id="8723b-167">Ověření ke službě Machine Learning s klíčem rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8723b-167">You authenticate to the Machine Learning service with an API key.</span></span>

<span data-ttu-id="8723b-168">Pro připojení k Machine Learning webové služby, **Microsoft.AspNet.WebApi.Client** musí být nainstalován balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="8723b-168">To connect to a Machine Learning Web service, the **Microsoft.AspNet.WebApi.Client** NuGet package must be installed.</span></span>

<span data-ttu-id="8723b-169">**Nainstalovat Microsoft.AspNet.WebApi.Client NuGet v sadě Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="8723b-169">**Install Microsoft.AspNet.WebApi.Client NuGet in Visual Studio**</span></span>

1. <span data-ttu-id="8723b-170">Publikování datovou sadu stáhnout z UCI: dataset – třída pro dospělé 2 webové služby.</span><span class="sxs-lookup"><span data-stu-id="8723b-170">Publish the Download dataset from UCI: Adult 2 class dataset Web Service.</span></span>
2. <span data-ttu-id="8723b-171">Klikněte na **Nástroje**  >  **Správce balíčků NuGet**  >  **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="8723b-171">Click **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="8723b-172">Zvolte **Install-Package Microsoft.AspNet.WebApi.Client**.</span><span class="sxs-lookup"><span data-stu-id="8723b-172">Choose **Install-Package Microsoft.AspNet.WebApi.Client**.</span></span>

<span data-ttu-id="8723b-173">**Ke spuštění ukázka kódu**</span><span class="sxs-lookup"><span data-stu-id="8723b-173">**To run the code sample**</span></span>

1. <span data-ttu-id="8723b-174">Publikování "Příklad 1: Stáhněte datové sady z UCI: datové sady pro dospělé 2 – třída" experiment, součástí kolekce ukázka Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="8723b-174">Publish "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of the Machine Learning sample collection.</span></span>
2. <span data-ttu-id="8723b-175">Přiřaďte apiKey klíčem z webové služby.</span><span class="sxs-lookup"><span data-stu-id="8723b-175">Assign apiKey with the key from a Web service.</span></span> <span data-ttu-id="8723b-176">V tématu **získání klíče autorizace Azure Machine Learning** výše.</span><span class="sxs-lookup"><span data-stu-id="8723b-176">See **Get an Azure Machine Learning authorization key** above.</span></span>
3. <span data-ttu-id="8723b-177">Přiřaďte serviceUri s identifikátor URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="8723b-177">Assign serviceUri with the Request URI.</span></span>

### <a name="python-sample"></a><span data-ttu-id="8723b-178">Ukázka Pythonu</span><span class="sxs-lookup"><span data-stu-id="8723b-178">Python Sample</span></span>
<span data-ttu-id="8723b-179">Pro připojení k webové služby Machine Learning, použijte **urllib2** předávání ScoreData knihovny.</span><span class="sxs-lookup"><span data-stu-id="8723b-179">To connect to a Machine Learning Web service, use the **urllib2** library passing ScoreData.</span></span> <span data-ttu-id="8723b-180">ScoreData obsahuje FeatureVector, n dimenzí vektor číselné funkce představující ScoreData.</span><span class="sxs-lookup"><span data-stu-id="8723b-180">ScoreData contains a FeatureVector, an n-dimensional  vector of numerical features that represents the ScoreData.</span></span> <span data-ttu-id="8723b-181">Ověření ke službě Machine Learning s klíčem rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8723b-181">You authenticate to the Machine Learning service with an API key.</span></span>

<span data-ttu-id="8723b-182">**Ke spuštění ukázka kódu**</span><span class="sxs-lookup"><span data-stu-id="8723b-182">**To run the code sample**</span></span>

1. <span data-ttu-id="8723b-183">Nasadit "Příklad 1: Stáhněte datové sady z UCI: datové sady pro dospělé 2 – třída" experiment, součástí kolekce ukázka Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="8723b-183">Deploy "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of the Machine Learning sample collection.</span></span>
2. <span data-ttu-id="8723b-184">Přiřaďte apiKey klíčem z webové služby.</span><span class="sxs-lookup"><span data-stu-id="8723b-184">Assign apiKey with the key from a Web service.</span></span> <span data-ttu-id="8723b-185">Najdete v článku **získání klíče autorizace Azure Machine Learning** části téměř začátku tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="8723b-185">See the **Get an Azure Machine Learning authorization key** section near the beginning of this article.</span></span>
3. <span data-ttu-id="8723b-186">Přiřaďte serviceUri s identifikátor URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="8723b-186">Assign serviceUri with the Request URI.</span></span>

