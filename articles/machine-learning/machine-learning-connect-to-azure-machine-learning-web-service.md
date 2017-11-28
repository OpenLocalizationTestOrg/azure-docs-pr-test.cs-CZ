---
title: "aaaConnect tooa webová služba Machine Learning | Microsoft Docs"
description: "Pomocí jazyka C# nebo Python připojte tooan Azure Machine Learning webové službě pomocí autorizační klíč."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 59b07bab-b60f-48c4-a385-a162e50ec7c2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: garye
ROBOTS: NOINDEX
redirect_url: machine-learning-consume-web-services
redirect_document_id: True
ms.openlocfilehash: 0108e71e30a05539a8c0ee93d5aadb07e3d1efa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-azure-machine-learning-web-service"></a><span data-ttu-id="c8ca2-103">Připojit tooan webovou službu Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c8ca2-103">Connect tooan Azure Machine Learning Web Service</span></span>
<span data-ttu-id="c8ca2-104">Hello prostředí pro vývojáře Azure Machine Learning je Web service API toomake predikcím ze vstupní data v reálném čase nebo v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-104">hello Azure Machine Learning developer experience is a Web service API toomake predictions from input data in real time or in batch mode.</span></span> <span data-ttu-id="c8ca2-105">Pomocí Azure Machine Learning Studio toocreate předpovědi a nasazení Azure Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-105">You use Azure Machine Learning Studio toocreate predictions and deploy an Azure Machine Learning Web service.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="c8ca2-106">toolearn o toocreate a nasadit Machine Learning webové služby pomocí nástroje Machine Learning Studio:</span><span class="sxs-lookup"><span data-stu-id="c8ca2-106">toolearn about how toocreate and deploy a Machine Learning Web service using Machine Learning Studio:</span></span>

* <span data-ttu-id="c8ca2-107">Kurz týkající se jak toocreate experimentu v nástroji Machine Learning Studio, najdete v části [vytvoření prvního experimentu](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="c8ca2-107">For a tutorial on how toocreate an experiment in Machine Learning Studio, see [Create your first experiment](machine-learning-create-experiment.md).</span></span>
* <span data-ttu-id="c8ca2-108">Podrobnosti o tom najdete v části toodeploy webové služby, [nasazení služby Machine Learning webové](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="c8ca2-108">For details on how toodeploy a Web service, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="c8ca2-109">Další informace o Machine Learning obecně naleznete hello [dokumentace k centru pro Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="c8ca2-109">For more information about Machine Learning in general, visit hello [Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

## <a name="azure-machine-learning-web-service"></a><span data-ttu-id="c8ca2-110">Azure Machine Learning webové služby</span><span class="sxs-lookup"><span data-stu-id="c8ca2-110">Azure Machine Learning Web service</span></span>
<span data-ttu-id="c8ca2-111">S hello Azure Machine Learning webové služby externí aplikace komunikuje se vyhodnocování model Machine Learning pracovního postupu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-111">With hello Azure Machine Learning Web service, an external application communicates with a Machine Learning workflow scoring model in real time.</span></span> <span data-ttu-id="c8ca2-112">Volání Machine Learning webové služby vrátí výsledky předpovědi tooan externí aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-112">A Machine Learning Web service call returns prediction results tooan external application.</span></span> <span data-ttu-id="c8ca2-113">toomake volání Machine Learning webové služby, předáte klíč rozhraní API, která se vytvoří při nasazení předpovědi.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-113">toomake a Machine Learning Web service call, you pass an API key that is created when you deploy a prediction.</span></span> <span data-ttu-id="c8ca2-114">Hello Machine Learning webové služby je založena na REST, možnost populární architektury pro webové projekty programování.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-114">hello Machine Learning Web service is based on REST, a popular architecture choice for web programming projects.</span></span>

<span data-ttu-id="c8ca2-115">Azure Machine Learning zahrnuje dva typy služeb:</span><span class="sxs-lookup"><span data-stu-id="c8ca2-115">Azure Machine Learning has two types of services:</span></span>

* <span data-ttu-id="c8ca2-116">Požadavků a odpovědí služby (záznamy RR) – s nízkou latencí, vysoce škálovatelná služba, která poskytuje rozhraní toohello bezstavové modely vytvořené a nasazené z hello Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-116">Request-Response Service (RRS) – A low latency, highly scalable service that provides an interface toohello stateless models created and deployed from hello Machine Learning Studio.</span></span>
* <span data-ttu-id="c8ca2-117">Spuštění služby Batch (BES) – asynchronní služby tohoto skóre a dávky pro datových záznamů.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-117">Batch Execution Service (BES) – An asynchronous service that scores a batch for data records.</span></span>

<span data-ttu-id="c8ca2-118">Další informace o Machine Learning webových služeb najdete v tématu [nasazení služby Machine Learning webové](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="c8ca2-118">For more information about Machine Learning Web services, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="get-an-azure-machine-learning-authorization-key"></a><span data-ttu-id="c8ca2-119">Získání klíče autorizace Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c8ca2-119">Get an Azure Machine Learning authorization key</span></span>
<span data-ttu-id="c8ca2-120">Když nasadíte experimentu, klíče rozhraní API se generují pro hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-120">When you deploy your experiment, API keys are generated for hello Web service.</span></span> <span data-ttu-id="c8ca2-121">Můžete získat hello klíče v několika umístěních.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-121">You can retrieve hello keys from several locations.</span></span>

### <a name="from-hello-microsoft-azure-machine-learning-web-services-portal"></a><span data-ttu-id="c8ca2-122">Z portálu Microsoft Azure Machine Learning webové služby hello</span><span class="sxs-lookup"><span data-stu-id="c8ca2-122">From hello Microsoft Azure Machine Learning Web Services portal</span></span>
<span data-ttu-id="c8ca2-123">Přihlaste se toohello [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net) portálu.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-123">Sign in toohello [Microsoft Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span>

<span data-ttu-id="c8ca2-124">klíč tooretrieve hello rozhraní API pro nové Machine Learning webové služby:</span><span class="sxs-lookup"><span data-stu-id="c8ca2-124">tooretrieve hello API key for a New Machine Learning Web service:</span></span>

1. <span data-ttu-id="c8ca2-125">Hello portálu webové služby Azure Machine Learning, klikněte na tlačítko **webové služby** hello horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-125">In hello Azure Machine Learning Web Services portal, click **Web Services** hello top menu.</span></span>
2. <span data-ttu-id="c8ca2-126">Klikněte na tlačítko hello webové služby, pro kterou chcete tooretrieve hello klíč.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-126">Click hello Web service for which you want tooretrieve hello key.</span></span>
3. <span data-ttu-id="c8ca2-127">V horní nabídce hello, klikněte na tlačítko **spotřebě**.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-127">On hello top menu, click **Consume**.</span></span>
4. <span data-ttu-id="c8ca2-128">Zkopírujte a uložte hello **primární klíč**.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-128">Copy and save hello **Primary Key**.</span></span>

<span data-ttu-id="c8ca2-129">klíč tooretrieve hello rozhraní API pro Classic Machine Learning webové služby:</span><span class="sxs-lookup"><span data-stu-id="c8ca2-129">tooretrieve hello API key for a Classic Machine Learning Web service:</span></span>

1. <span data-ttu-id="c8ca2-130">Hello portálu webové služby Azure Machine Learning, klikněte na tlačítko **Classic webové služby** hello horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-130">In hello Azure Machine Learning Web Services portal, click **Classic Web Services** hello top menu.</span></span>
2. <span data-ttu-id="c8ca2-131">Klikněte na tlačítko hello webové služby, se kterým pracujete.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-131">Click hello Web service with which you are working.</span></span>
3. <span data-ttu-id="c8ca2-132">Klikněte na tlačítko hello koncový bod, pro kterou chcete tooretrieve hello klíč.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-132">Click hello endpoint for which you want tooretrieve hello key.</span></span>
4. <span data-ttu-id="c8ca2-133">V horní nabídce hello, klikněte na tlačítko **spotřebě**.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-133">On hello top menu, click **Consume**.</span></span>
5. <span data-ttu-id="c8ca2-134">Zkopírujte a uložte hello **primární klíč**.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-134">Copy and save hello **Primary Key**.</span></span>

### <a name="classic-web-service"></a><span data-ttu-id="c8ca2-135">Classic webové služby</span><span class="sxs-lookup"><span data-stu-id="c8ca2-135">Classic Web service</span></span>
 <span data-ttu-id="c8ca2-136">Můžete také načíst klíč pro Classic webové služby Machine Learning Studio nebo hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-136">You can also retrieve a key for a Classic Web service from Machine Learning Studio or hello Azure classic portal.</span></span>

#### <a name="machine-learning-studio"></a><span data-ttu-id="c8ca2-137">Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="c8ca2-137">Machine Learning Studio</span></span>
1. <span data-ttu-id="c8ca2-138">V nástroji Machine Learning Studio, klikněte na tlačítko **webové služby** na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-138">In Machine Learning Studio, click **WEB SERVICES** on hello left.</span></span>
2. <span data-ttu-id="c8ca2-139">Klikněte na webovou službu.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-139">Click a Web service.</span></span> <span data-ttu-id="c8ca2-140">Hello **klíč rozhraní API** na hello **řídicí panel** kartě.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-140">hello **API key** is on hello **DASHBOARD** tab.</span></span>

#### <a name="azure-classic-portal"></a><span data-ttu-id="c8ca2-141">klasický portál Azure</span><span class="sxs-lookup"><span data-stu-id="c8ca2-141">Azure classic portal</span></span>
1. <span data-ttu-id="c8ca2-142">Klikněte na tlačítko **MACHINE LEARNING** na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-142">Click **MACHINE LEARNING** on hello left.</span></span>
2. <span data-ttu-id="c8ca2-143">Klikněte na tlačítko hello prostoru, ve kterém se nachází webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-143">Click hello workspace in which your Web service is located.</span></span>
3. <span data-ttu-id="c8ca2-144">Klikněte na tlačítko **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-144">Click **WEB SERVICES**.</span></span>
4. <span data-ttu-id="c8ca2-145">Klikněte na webovou službu.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-145">Click a Web service.</span></span>
5. <span data-ttu-id="c8ca2-146">Klikněte na koncový bod.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-146">Click an endpoint.</span></span> <span data-ttu-id="c8ca2-147">Hello "API KEY" je mimo provoz v pravém dolním hello.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-147">hello “API KEY” is down at hello lower-right.</span></span>

## <span data-ttu-id="c8ca2-148"><a id="connect"></a>Připojit tooa Machine Learning webové služby</span><span class="sxs-lookup"><span data-stu-id="c8ca2-148"><a id="connect"></a>Connect tooa Machine Learning Web service</span></span>
<span data-ttu-id="c8ca2-149">Tooa Machine Learning webové službě pomocí programovací jazyk, který podporuje žádostí HTTP a odpovědí se můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-149">You can connect tooa Machine Learning Web service using any programming language that supports HTTP request and response.</span></span> <span data-ttu-id="c8ca2-150">Příklady můžete zobrazit v C#, Python a R z Machine Learning webové stránky nápovědy služby.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-150">You can view examples in C#, Python, and R from a Machine Learning Web service help page.</span></span>

<span data-ttu-id="c8ca2-151">**Počítač Learning API nápovědy** Machine Learning API nápovědy se vytvoří při nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-151">**Machine Learning API help** Machine Learning API help is created when you deploy a Web service.</span></span> <span data-ttu-id="c8ca2-152">V tématu [Azure Machine Learning návod - nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="c8ca2-152">See [Azure Machine Learning Walkthrough- Deploy Web Service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
<span data-ttu-id="c8ca2-153">Hello Machine Learning API nápovědy obsahuje podrobnosti o předpovědi webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-153">hello Machine Learning API help contains details about a prediction Web service.</span></span>

1. <span data-ttu-id="c8ca2-154">Klikněte na tlačítko hello webové služby, se kterým pracujete.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-154">Click hello Web service with which you are working.</span></span>
2. <span data-ttu-id="c8ca2-155">Klikněte na tlačítko hello koncový bod, pro které chcete tooview hello stránce nápovědy k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-155">Click hello endpoint for which you want tooview hello API Help Page.</span></span>
3. <span data-ttu-id="c8ca2-156">V horní nabídce hello, klikněte na tlačítko **spotřebě**.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-156">On hello top menu, click **Consume**.</span></span>
4. <span data-ttu-id="c8ca2-157">Klikněte na tlačítko **stránku nápovědy rozhraní API** pod hello požadavků a odpovědí nebo Batch Execution koncové body.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-157">Click **API help page** under either hello Request-Response or Batch Execution endpoints.</span></span>

<span data-ttu-id="c8ca2-158">**tooview Machine Learning API nápovědy pro novou webovou službu**</span><span class="sxs-lookup"><span data-stu-id="c8ca2-158">**tooview Machine Learning API help for a New Web service**</span></span>

<span data-ttu-id="c8ca2-159">V portálu služby Azure Machine Learning webové hello:</span><span class="sxs-lookup"><span data-stu-id="c8ca2-159">In hello Azure Machine Learning Web Services Portal:</span></span>

1. <span data-ttu-id="c8ca2-160">Klikněte na tlačítko **webové služby** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-160">Click **WEB SERVICES** on hello top menu.</span></span>
2. <span data-ttu-id="c8ca2-161">Klikněte na tlačítko hello webové služby, pro kterou chcete tooretrieve hello klíč.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-161">Click hello Web service for which you want tooretrieve hello key.</span></span>

<span data-ttu-id="c8ca2-162">Klikněte na tlačítko **spotřebě** tooget hello identifikátory URI pro hello Reposonse požadavku a spuštění služby Batch a ukázkový kód v jazyce C#, R a Python.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-162">Click **Consume** tooget hello URIs for hello Request-Reposonse and Batch Execution Services and Sample code in C#, R, and Python.</span></span>

<span data-ttu-id="c8ca2-163">Klikněte na tlačítko **rozhraní API Swaggeru** tooget Swagger založené na dokumentaci hello rozhraní API volat z hello zadané identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-163">Click **Swagger API** tooget Swagger based documentation for hello APIs called from hello supplied URIs.</span></span>

### <a name="c-sample"></a><span data-ttu-id="c8ca2-164">Ukázka C#</span><span class="sxs-lookup"><span data-stu-id="c8ca2-164">C# Sample</span></span>
<span data-ttu-id="c8ca2-165">tooconnect tooa Machine Learning webové služby, použijte **HttpClient** předávání ScoreData.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-165">tooconnect tooa Machine Learning Web service, use an **HttpClient** passing ScoreData.</span></span> <span data-ttu-id="c8ca2-166">ScoreData obsahuje FeatureVector, n dimenzí vektor číselné funkce představující hello ScoreData.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-166">ScoreData contains a FeatureVector, an n-dimensional vector of numerical features that represents hello ScoreData.</span></span> <span data-ttu-id="c8ca2-167">Ověřování služby Machine Learning toohello s klíčem rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-167">You authenticate toohello Machine Learning service with an API key.</span></span>

<span data-ttu-id="c8ca2-168">tooconnect tooa webové služby Machine Learning hello **Microsoft.AspNet.WebApi.Client** musí být nainstalován balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-168">tooconnect tooa Machine Learning Web service, hello **Microsoft.AspNet.WebApi.Client** NuGet package must be installed.</span></span>

<span data-ttu-id="c8ca2-169">**Nainstalovat Microsoft.AspNet.WebApi.Client NuGet v sadě Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="c8ca2-169">**Install Microsoft.AspNet.WebApi.Client NuGet in Visual Studio**</span></span>

1. <span data-ttu-id="c8ca2-170">Publikování hello datovou sadu stáhnout z UCI: dataset – třída pro dospělé 2 webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-170">Publish hello Download dataset from UCI: Adult 2 class dataset Web Service.</span></span>
2. <span data-ttu-id="c8ca2-171">Klikněte na **Nástroje**  >  **Správce balíčků NuGet**  >  **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-171">Click **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="c8ca2-172">Zvolte **Install-Package Microsoft.AspNet.WebApi.Client**.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-172">Choose **Install-Package Microsoft.AspNet.WebApi.Client**.</span></span>

<span data-ttu-id="c8ca2-173">**Ukázka kódu toorun hello**</span><span class="sxs-lookup"><span data-stu-id="c8ca2-173">**toorun hello code sample**</span></span>

1. <span data-ttu-id="c8ca2-174">Publikování "Příklad 1: Stáhněte datové sady z UCI: datovou sadu pro dospělé 2 – třída" experiment, součástí hello Machine Learning ukázka kolekce.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-174">Publish "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of hello Machine Learning sample collection.</span></span>
2. <span data-ttu-id="c8ca2-175">Přiřaďte apiKey klíčem hello z webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-175">Assign apiKey with hello key from a Web service.</span></span> <span data-ttu-id="c8ca2-176">V tématu **získání klíče autorizace Azure Machine Learning** výše.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-176">See **Get an Azure Machine Learning authorization key** above.</span></span>
3. <span data-ttu-id="c8ca2-177">Přiřaďte serviceUri s hello URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-177">Assign serviceUri with hello Request URI.</span></span>

### <a name="python-sample"></a><span data-ttu-id="c8ca2-178">Ukázka Pythonu</span><span class="sxs-lookup"><span data-stu-id="c8ca2-178">Python Sample</span></span>
<span data-ttu-id="c8ca2-179">tooconnect tooa Machine Learning webové služby, použijte hello **urllib2** předávání ScoreData knihovny.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-179">tooconnect tooa Machine Learning Web service, use hello **urllib2** library passing ScoreData.</span></span> <span data-ttu-id="c8ca2-180">ScoreData obsahuje FeatureVector, n dimenzí vektor číselné funkce představující hello ScoreData.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-180">ScoreData contains a FeatureVector, an n-dimensional  vector of numerical features that represents hello ScoreData.</span></span> <span data-ttu-id="c8ca2-181">Ověřování služby Machine Learning toohello s klíčem rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-181">You authenticate toohello Machine Learning service with an API key.</span></span>

<span data-ttu-id="c8ca2-182">**Ukázka kódu toorun hello**</span><span class="sxs-lookup"><span data-stu-id="c8ca2-182">**toorun hello code sample**</span></span>

1. <span data-ttu-id="c8ca2-183">Nasadit "Příklad 1: Stáhněte datové sady z UCI: datovou sadu pro dospělé 2 – třída" experiment, součástí hello Machine Learning ukázka kolekce.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-183">Deploy "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of hello Machine Learning sample collection.</span></span>
2. <span data-ttu-id="c8ca2-184">Přiřaďte apiKey klíčem hello z webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-184">Assign apiKey with hello key from a Web service.</span></span> <span data-ttu-id="c8ca2-185">V tématu hello **získání klíče autorizace Azure Machine Learning** části téměř hello začátku tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-185">See hello **Get an Azure Machine Learning authorization key** section near hello beginning of this article.</span></span>
3. <span data-ttu-id="c8ca2-186">Přiřaďte serviceUri s hello URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="c8ca2-186">Assign serviceUri with hello Request URI.</span></span>

