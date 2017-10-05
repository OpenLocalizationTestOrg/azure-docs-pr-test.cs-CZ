---
title: "Řešení potíží s retraining webové služby Azure Machine Learning Classic | Microsoft Docs"
description: "Identifikujte a opravte narazil běžné problémy, když jsou retraining modelu pro webové služby Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: fc36499ebff88c86635228ff899c85e9166aabed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a><span data-ttu-id="0fa64-103">Řešení potíží s retraining Azure Machine Learning Classic webové služby</span><span class="sxs-lookup"><span data-stu-id="0fa64-103">Troubleshooting the retraining of an Azure Machine Learning Classic Web service</span></span>
## <a name="retraining-overview"></a><span data-ttu-id="0fa64-104">Retraining – přehled</span><span class="sxs-lookup"><span data-stu-id="0fa64-104">Retraining overview</span></span>
<span data-ttu-id="0fa64-105">Když nasadíte prediktivní experiment jako vyhodnocování webové služby je statický model.</span><span class="sxs-lookup"><span data-stu-id="0fa64-105">When you deploy a predictive experiment as a scoring web service it is a static model.</span></span> <span data-ttu-id="0fa64-106">Jakmile nová data k dispozici nebo pokud příjemce rozhraní API má svá vlastní data, musí být retrained modelu.</span><span class="sxs-lookup"><span data-stu-id="0fa64-106">As new data becomes available or when the consumer of the API has their own data, the model needs to be retrained.</span></span> 

<span data-ttu-id="0fa64-107">Kompletní a podrobný postup retraining procesu Classic webové služby, najdete v části [Přeučování Machine Learning modely prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="0fa64-107">For a complete walkthrough of the retraining process of a Classic Web service, see [Retrain Machine Learning Models Programmatically](machine-learning-retrain-models-programmatically.md).</span></span>

## <a name="retraining-process"></a><span data-ttu-id="0fa64-108">Retraining procesu</span><span class="sxs-lookup"><span data-stu-id="0fa64-108">Retraining process</span></span>
<span data-ttu-id="0fa64-109">Když potřebujete přeučování webovou službu, musíte přidat další některé jeho součásti:</span><span class="sxs-lookup"><span data-stu-id="0fa64-109">When you need to retrain the Web service, you must add some additional pieces:</span></span>

* <span data-ttu-id="0fa64-110">Webovou službu nasazenou z výukový Experiment.</span><span class="sxs-lookup"><span data-stu-id="0fa64-110">A Web service deployed from the Training Experiment.</span></span> <span data-ttu-id="0fa64-111">Musí mít experiment **výstup webové služby** modulu připojené k výstupu **Train Model** modulu.</span><span class="sxs-lookup"><span data-stu-id="0fa64-111">The experiment must have a **Web Service Output** module attached to the output of the **Train Model** module.</span></span>  
  
    ![Připojte k train model výstup webové služby.][image1]
* <span data-ttu-id="0fa64-113">Nový koncový bod přidán do vyhodnocování webovou službu.</span><span class="sxs-lookup"><span data-stu-id="0fa64-113">A new endpoint added to your scoring Web service.</span></span>  <span data-ttu-id="0fa64-114">Můžete přidat koncový bod programově pomocí ukázkový kód, kterou se odkazuje v Machine Learning Přeučování modelů prostřednictvím kódu programu tématu nebo prostřednictvím portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="0fa64-114">You can add the endpoint programmatically using the sample code referenced in the Retrain Machine Learning models programmatically topic or through the Azure classic portal.</span></span>

<span data-ttu-id="0fa64-115">Ukázka kódu C# ze stránky nápovědy školení webové rozhraní API pak můžete přeučování modelu.</span><span class="sxs-lookup"><span data-stu-id="0fa64-115">You can then use the sample C# code from the Training Web Service's API help page to retrain model.</span></span> <span data-ttu-id="0fa64-116">Po vyhodnocení výsledky a spokojeni s nimi, je třeba aktualizovat pro cvičný model vyhodnocování webové službě pomocí nového koncového bodu, který jste přidali.</span><span class="sxs-lookup"><span data-stu-id="0fa64-116">Once you have evaluated the results and are satisfied with them, you update the trained model scoring web service using the new endpoint that you added.</span></span>

<span data-ttu-id="0fa64-117">Hlavní kroky, které je nutné provést při programovém modelu s všechny části na místě, jsou následující:</span><span class="sxs-lookup"><span data-stu-id="0fa64-117">With all the pieces in place, the major steps you must take to retrain the model are as follows:</span></span>

1. <span data-ttu-id="0fa64-118">Volání webové služby školení: volání je do dávky spuštění služby (BES), není žádosti o odpověď služby (záznamy RR).</span><span class="sxs-lookup"><span data-stu-id="0fa64-118">Call the Training Web Service:  The call is to the Batch Execution Service (BES), not the Request Response Service (RRS).</span></span> <span data-ttu-id="0fa64-119">Ukázkový kód jazyka C# můžete na stránce nápovědy rozhraní API pro volání.</span><span class="sxs-lookup"><span data-stu-id="0fa64-119">You can use the sample C# code on the API help page to make the call.</span></span> 
2. <span data-ttu-id="0fa64-120">Najít hodnoty *BaseLocation*, *RelativeLocation*, a *SasBlobToken*: tyto hodnoty jsou vráceny ve výstupu z volání k webové službě školení.</span><span class="sxs-lookup"><span data-stu-id="0fa64-120">Find the values for the *BaseLocation*, *RelativeLocation*, and *SasBlobToken*: These values are returned in the output from your call to the Training Web Service.</span></span> 
   <span data-ttu-id="0fa64-121">![zobrazuje výstup retraining ukázka a BaseLocation, RelativeLocation a SasBlobToken hodnoty.][image6]</span><span class="sxs-lookup"><span data-stu-id="0fa64-121">![showing the output of the retraining sample and the BaseLocation, RelativeLocation, and  SasBlobToken values.][image6]</span></span>
3. <span data-ttu-id="0fa64-122">Aktualizace přidané koncového bodu z vyhodnocování webové služby s novou trénovaného modelu: pomocí ukázkový kód, který je součástí Machine Learning Přeučování modelů prostřednictvím kódu programu, aktualizaci nový koncový bod, které jste přidali do vyhodnocování model s nově trénovaného modelu z Školení webové služby.</span><span class="sxs-lookup"><span data-stu-id="0fa64-122">Update the added endpoint from the scoring web service with the new trained model: Using the sample code provided in the Retrain Machine Learning models programmatically, update the new endpoint you added to the scoring model with the newly trained model from the Training Web Service.</span></span>

## <a name="common-obstacles"></a><span data-ttu-id="0fa64-123">Běžné překážek</span><span class="sxs-lookup"><span data-stu-id="0fa64-123">Common obstacles</span></span>
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a><span data-ttu-id="0fa64-124">Zkontrolujte, jestli máte správnou adresu URL oprava</span><span class="sxs-lookup"><span data-stu-id="0fa64-124">Check to see if you have the correct PATCH URL</span></span>
<span data-ttu-id="0fa64-125">Adresa URL opravy, kterou používáte musí být přidružený nový vyhodnocovací koncový bod, které jste přidali k webové službě vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="0fa64-125">The PATCH URL you are using must be the one associated with the new scoring endpoint you added to the scoring Web service.</span></span> <span data-ttu-id="0fa64-126">Existuje několik způsobů, jak získat adresu URL opravy:</span><span class="sxs-lookup"><span data-stu-id="0fa64-126">There are a number of ways to obtain the PATCH URL:</span></span>

<span data-ttu-id="0fa64-127">**Možnost 1: programově**</span><span class="sxs-lookup"><span data-stu-id="0fa64-127">**Option 1: Programatically**</span></span>

<span data-ttu-id="0fa64-128">Pokud chcete získat správnou adresu URL opravy:</span><span class="sxs-lookup"><span data-stu-id="0fa64-128">To get the correct PATCH URL:</span></span>

1. <span data-ttu-id="0fa64-129">Spustit [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="0fa64-129">Run the [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>
2. <span data-ttu-id="0fa64-130">Z výstupu AddEndpoint, Najít *HelpLocation* hodnotu a zkopírujte adresu URL.</span><span class="sxs-lookup"><span data-stu-id="0fa64-130">From the output of AddEndpoint, find the *HelpLocation* value and copy the URL.</span></span>
   
   ![HelpLocation ve výstupu addEndpoint vzorku.][image2]
3. <span data-ttu-id="0fa64-132">Vložte adresu URL do prohlížeče, přejděte na stránku, která poskytuje odkazy na nápovědu pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="0fa64-132">Paste the URL into a browser to navigate to a page that provides help links for the Web service.</span></span>
4. <span data-ttu-id="0fa64-133">Klikněte **aktualizace prostředků** odkazu k otevření stránky nápovědy opravy.</span><span class="sxs-lookup"><span data-stu-id="0fa64-133">Click the **Update Resource** link to open the patch help page.</span></span>

<span data-ttu-id="0fa64-134">**Možnost 2: Pomocí portálu Azure classic**</span><span class="sxs-lookup"><span data-stu-id="0fa64-134">**Option 2: Use the Azure classic portal**</span></span>

1. <span data-ttu-id="0fa64-135">Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="0fa64-135">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="0fa64-136">Otevřete kartu Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="0fa64-136">Open the Machine Learning tab.</span></span> 
   <span data-ttu-id="0fa64-137">![Karta opřené počítače.][image4]</span><span class="sxs-lookup"><span data-stu-id="0fa64-137">![Machine leaning tab.][image4]</span></span>
3. <span data-ttu-id="0fa64-138">Pak klikněte na název pracovního prostoru, **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="0fa64-138">Click your workspace name, then **Web Services**.</span></span>
4. <span data-ttu-id="0fa64-139">Klikněte na tlačítko vyhodnocování webové služby, kterou pracujete.</span><span class="sxs-lookup"><span data-stu-id="0fa64-139">Click the scoring Web service you are working with.</span></span> <span data-ttu-id="0fa64-140">(Pokud jste nezměnila výchozí název webové služby, se bude končit [vyhodnocování Exp.].)</span><span class="sxs-lookup"><span data-stu-id="0fa64-140">(If you did not modify the default name of the web service, it will end in [Scoring Exp.].)</span></span>
5. <span data-ttu-id="0fa64-141">Klikněte na tlačítko **přidání koncového bodu**.</span><span class="sxs-lookup"><span data-stu-id="0fa64-141">Click **Add Endpoint**.</span></span>
6. <span data-ttu-id="0fa64-142">Po přidání koncového bodu, klikněte na název koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="0fa64-142">After the endpoint is added, click the endpoint name.</span></span> <span data-ttu-id="0fa64-143">Pak klikněte na tlačítko **aktualizace prostředků** chcete otevřít stránku nápovědy oprav.</span><span class="sxs-lookup"><span data-stu-id="0fa64-143">Then click **Update Resource** to open the patching help page.</span></span>

> [!NOTE]
> <span data-ttu-id="0fa64-144">Pokud jste přidali koncový bod k webové službě školení místo prediktivní webové služby, zobrazí se chybová zpráva po kliknutí na tlačítko **aktualizace prostředků** odkaz: je nám líto, ale tato funkce není podporována nebo k dispozici v tomto kontext.</span><span class="sxs-lookup"><span data-stu-id="0fa64-144">If you have added the endpoint to the Training Web Service instead of the Predictive Web Service, you will receive the following error when you click the **Update Resource** link: Sorry, but this feature is not supported or available in this context.</span></span> <span data-ttu-id="0fa64-145">Tato webová služba nemá žádné aktualizovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="0fa64-145">This Web Service has no updatable resources.</span></span> <span data-ttu-id="0fa64-146">Omlouváme se za nepříjemnosti a práce na zlepšení tento pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="0fa64-146">We apologize for the inconvenience and are working on improving this workflow.</span></span>
> 
> 

![Nový koncový bod řídicí panel.][image3]

<span data-ttu-id="0fa64-148">Na stránce nápovědy oprava obsahuje adresu URL opravy, je nutné použít a poskytuje ukázkový kód, které můžete použít k volání.</span><span class="sxs-lookup"><span data-stu-id="0fa64-148">The PATCH help page contains the PATCH URL you must use and provides sample code you can use to call it.</span></span>

![Adresa URL opravy.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a><span data-ttu-id="0fa64-150">Zkontrolujte aktualizaci správný vyhodnocování koncový bod</span><span class="sxs-lookup"><span data-stu-id="0fa64-150">Check to see that you are updating the correct scoring endpoint</span></span>
* <span data-ttu-id="0fa64-151">Není oprava webovou službu školení: operace opravy se musí provést na vyhodnocování webové službě.</span><span class="sxs-lookup"><span data-stu-id="0fa64-151">Do not patch the Training Web Service: The patch operation must be performed on the scoring Web service.</span></span>
* <span data-ttu-id="0fa64-152">Výchozí koncový bod webové služby není oprava: operace opravy se musí provést na nové vyhodnocování webové služby koncového bodu, který jste přidali.</span><span class="sxs-lookup"><span data-stu-id="0fa64-152">Do not patch the default endpoint on Web service: The patch operation must be performed on the new scoring Web service endpoint that you added.</span></span>

<span data-ttu-id="0fa64-153">Můžete ověřit, které webové služby, koncový bod je na pomocí portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="0fa64-153">You can verify which Web service the endpoint is on by visiting the Azure classic portal.</span></span> 

> [!NOTE]
> <span data-ttu-id="0fa64-154">Ujistěte se, že přidáváte koncový bod do prediktivní webové služby není školení webovou službu.</span><span class="sxs-lookup"><span data-stu-id="0fa64-154">Be sure you are adding the endpoint to the Predictive Web Service, not the Training Web Service.</span></span> <span data-ttu-id="0fa64-155">Pokud jste nasadili správně školení a prediktivní webové služby, měli byste vidět dvě samostatné webové služby uvedené.</span><span class="sxs-lookup"><span data-stu-id="0fa64-155">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate Web services listed.</span></span> <span data-ttu-id="0fa64-156">Prediktivní webové služby musí končit "[prediktivní exp.]".</span><span class="sxs-lookup"><span data-stu-id="0fa64-156">The Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

1. <span data-ttu-id="0fa64-157">Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="0fa64-157">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="0fa64-158">Otevřete kartu Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="0fa64-158">Open the Machine Learning tab.</span></span> 
   <span data-ttu-id="0fa64-159">![Machine learning prostoru uživatelského rozhraní.][image4]</span><span class="sxs-lookup"><span data-stu-id="0fa64-159">![Machine learning workspace UI.][image4]</span></span>
3. <span data-ttu-id="0fa64-160">Vyberte pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="0fa64-160">Select your workspace.</span></span>
4. <span data-ttu-id="0fa64-161">Klikněte na tlačítko **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="0fa64-161">Click **Web Services**.</span></span>
5. <span data-ttu-id="0fa64-162">Vyberte prediktivní webové služby.</span><span class="sxs-lookup"><span data-stu-id="0fa64-162">Select your Predictive Web Service.</span></span>
6. <span data-ttu-id="0fa64-163">Ověřte, že váš nový koncový bod byl přidán k webové službě.</span><span class="sxs-lookup"><span data-stu-id="0fa64-163">Verify that your new endpoint was added to the Web service.</span></span>

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a><span data-ttu-id="0fa64-164">Zkontrolujte pracovní prostor, který webová služba je v zajistit, že je ve správném oblasti</span><span class="sxs-lookup"><span data-stu-id="0fa64-164">Check the workspace that your web service is in to ensure it is in the correct region</span></span>
1. <span data-ttu-id="0fa64-165">Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="0fa64-165">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="0fa64-166">Z nabídky vyberte Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="0fa64-166">Select Machine Learning from the menu.</span></span>
   <span data-ttu-id="0fa64-167">![Machine learning oblast uživatelského rozhraní.][image4]</span><span class="sxs-lookup"><span data-stu-id="0fa64-167">![Machine learning region UI.][image4]</span></span>
3. <span data-ttu-id="0fa64-168">Zkontrolujte umístění pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="0fa64-168">Verify the location of your workspace.</span></span>

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png
