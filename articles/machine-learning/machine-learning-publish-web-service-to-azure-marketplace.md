---
title: "AAA(Deprecated) publikovat strojového učení webové služby tooAzure Marketplace | Microsoft Docs"
description: "(nepoužívané) Jak toopublish vaše webová služba Azure Machine Learning toohello Azure Marketplace"
services: machine-learning
documentationcenter: 
author: BharathS
manager: jhubbard
editor: cgronlun
ms.assetid: 68e908be-3a99-4cd7-9517-e2b5f2f341b8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: machine-learning-gallery-experiments
redirect_document_id: True
ms.openlocfilehash: 149abc3df9b79c1b37d233d5e85e803592ff1020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-publish-azure-machine-learning-web-service-toohello-azure-marketplace"></a><span data-ttu-id="19da1-103">(nepoužívané) Publikovat webovou službu Azure Machine Learning toohello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="19da1-103">(deprecated) Publish Azure Machine Learning Web Service toohello Azure Marketplace</span></span>

> [!NOTE]
> <span data-ttu-id="19da1-104">DataMarket a Data služby se postupně vyřazuje z provozu a odběry vyřadí a zrušena od 3/31/2017.</span><span class="sxs-lookup"><span data-stu-id="19da1-104">DataMarket and Data Services are being retired, and existing subscriptions will be retired and cancelled starting 3/31/2017.</span></span> <span data-ttu-id="19da1-105">V důsledku toho je zastaralá v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="19da1-105">As a result, this article is being deprecated.</span></span> 
> 
> <span data-ttu-id="19da1-106">Jako alternativu, můžete publikovat vaše Machine Learning experimenty toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) hello Benefit hello datové vědy komunity.</span><span class="sxs-lookup"><span data-stu-id="19da1-106">As an alternative, you can publish your Machine Learning experiments toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) for hello benefit of hello data science community.</span></span> <span data-ttu-id="19da1-107">Další informace najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span><span class="sxs-lookup"><span data-stu-id="19da1-107">For more information, see [Share and discover resources in hello Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span></span>

<span data-ttu-id="19da1-108">Hello Azure Marketplace nabízí možnost hello webové služby Azure Machine Learning toopublish jako placené nebo bezplatné služby pro spotřeba externí zákazníky.</span><span class="sxs-lookup"><span data-stu-id="19da1-108">hello Azure Marketplace provides hello ability toopublish Azure Machine Learning web services as paid or free services for consumption by external customers.</span></span> <span data-ttu-id="19da1-109">Tento článek obsahuje přehled procesu s tooget tooguidelines odkazy, které jste spustili.</span><span class="sxs-lookup"><span data-stu-id="19da1-109">This article provides an overview of that process with links tooguidelines tooget you started.</span></span> <span data-ttu-id="19da1-110">Pomocí tohoto procesu můžete nastavit webové služby, které jsou k dispozici pro ostatní vývojáři tooconsume ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="19da1-110">By using this process, you can make your web services available for other developers tooconsume in their applications.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-hello-publishing-process"></a><span data-ttu-id="19da1-111">Přehled procesu publikování hello</span><span class="sxs-lookup"><span data-stu-id="19da1-111">Overview of hello publishing process</span></span>
<span data-ttu-id="19da1-112">Hello následují hello kroky pro publikování služby tooAzure web Azure Machine Learning Marketplace:</span><span class="sxs-lookup"><span data-stu-id="19da1-112">hello following are hello steps for publishing an Azure Machine Learning web service tooAzure Marketplace:</span></span>

1. <span data-ttu-id="19da1-113">Vytvoření a publikování služby Machine Learning požadavků a odpovědí (záznamy RR)</span><span class="sxs-lookup"><span data-stu-id="19da1-113">Create and publish a Machine Learning Request-Response service (RRS)</span></span>
2. <span data-ttu-id="19da1-114">Nasaďte tooproduction hello služby a získat informace o koncovém klíč rozhraní API a OData hello.</span><span class="sxs-lookup"><span data-stu-id="19da1-114">Deploy hello service tooproduction, and obtain hello API Key and OData endpoint information.</span></span>
3. <span data-ttu-id="19da1-115">Použití hello URL hello publikované webové služby toopublish příliš[Azure Marketplace (trhu dat)](https://publish.windowsazure.com/workspace/)</span><span class="sxs-lookup"><span data-stu-id="19da1-115">Use hello URL of hello published web service toopublish too[Azure Marketplace (Data Market)](https://publish.windowsazure.com/workspace/)</span></span> 
4. <span data-ttu-id="19da1-116">Po odeslání vaši nabídku zkontrolovány a musí toobe schválení před vašim zákazníkům začít zakoupení ho.</span><span class="sxs-lookup"><span data-stu-id="19da1-116">Once submitted, your offer is reviewed and needs toobe approved before your customers can start purchasing it.</span></span> <span data-ttu-id="19da1-117">proces publikování Hello může trvat několik pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="19da1-117">hello publishing process can take a few business days.</span></span> 

## <a name="walk-through"></a><span data-ttu-id="19da1-118">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="19da1-118">Walk through</span></span>
### <a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a><span data-ttu-id="19da1-119">Krok 1: Vytvoření a publikování služby Machine Learning požadavků a odpovědí (záznamy RR)</span><span class="sxs-lookup"><span data-stu-id="19da1-119">Step 1: Create and publish a Machine Learning Request-Response service (RRS)</span></span>
 <span data-ttu-id="19da1-120">Pokud jste to ještě neudělali, proveďte prosím podívejte se na to [provede](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="19da1-120">If you have not done this already, please take a look at this [walk through](machine-learning-walkthrough-5-publish-web-service.md).</span></span>

### <a name="step-2-deploy-hello-service-tooproduction-and-obtain-hello-api-key-and-odata-endpoint-information"></a><span data-ttu-id="19da1-121">Krok 2: Nasazení tooproduction hello služby a získat informace o koncovém klíč rozhraní API a OData hello</span><span class="sxs-lookup"><span data-stu-id="19da1-121">Step 2: Deploy hello service tooproduction, and obtain hello API Key and OData endpoint information</span></span>
1. <span data-ttu-id="19da1-122">Z hello [portálu Azure Classic](http://manage.windowsazure.com), vyberte hello **MACHINE LEARNING** možnost z hello levém navigačním panelu a vyberte pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="19da1-122">From hello [Azure Classic Portal](http://manage.windowsazure.com), select hello **MACHINE LEARNING** option from hello left navigation bar, and select your workspace.</span></span> 
2. <span data-ttu-id="19da1-123">Klikněte na hello **webové služby** kartě a webové služby vyberte hello chcete toopublish toohello marketplace.</span><span class="sxs-lookup"><span data-stu-id="19da1-123">Click on hello **WEB SERVICES** tab, and select hello web service you would like toopublish toohello marketplace.</span></span>
   
    ![Azure Marketplace][workspace]
3. <span data-ttu-id="19da1-125">Koncový bod vyberte hello by jako toohave hello marketplace spotřebujete.</span><span class="sxs-lookup"><span data-stu-id="19da1-125">Select hello endpoint you would like toohave hello marketplace consume.</span></span> <span data-ttu-id="19da1-126">Pokud jste nevytvořili žádné další koncové body, můžete si vybrat hello **výchozí** koncový bod.</span><span class="sxs-lookup"><span data-stu-id="19da1-126">If you have not created any additional endpoints, you can select hello **Default** endpoint.</span></span>
4. <span data-ttu-id="19da1-127">Po klepnutí na koncovém bodu hello, nebudete moct toosee hello **klíč rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="19da1-127">Once you have clicked on hello endpoint, you will be able toosee hello **API KEY**.</span></span> <span data-ttu-id="19da1-128">Je nutné tuto část informací později v kroku 3, proto zkontrolujte jeho kopii.</span><span class="sxs-lookup"><span data-stu-id="19da1-128">You will need this piece of information later on in Step 3, so make a copy of it.</span></span>
   
    ![Azure Marketplace][apikey]
5. <span data-ttu-id="19da1-130">Klikněte na hello **požadavků a odpovědí** metody v tomto okamžiku nepodporujeme publikování dávkového spuštění služeb toohello marketplace.</span><span class="sxs-lookup"><span data-stu-id="19da1-130">Click on hello **REQUEST/RESPONSE** method, at this point we do not support publishing batch execution services toohello marketplace.</span></span> <span data-ttu-id="19da1-131">Který vás nasměruje stránku nápovědy toohello rozhraní API pro hello metoda žádosti a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="19da1-131">That will take you toohello API help page for hello Request/Response method.</span></span>
6. <span data-ttu-id="19da1-132">Kopírování hello **adresa koncového bodu OData**, budete potřebovat tyto informace později v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="19da1-132">Copy hello **OData Endpoint Address**, you will need this information later on in Step 3.</span></span>
   
    ![Azure Marketplace][odata]

<span data-ttu-id="19da1-134">hello službu nasaďte do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="19da1-134">deploy hello service into production.</span></span>

### <a name="step-3-use-hello-url-of-hello-published-web-service-toopublish-tooazure-marketplace-datamarket"></a><span data-ttu-id="19da1-135">Krok 3: Použití hello URL hello publikované webové služby toopublish tooAzure Marketplace (DataMarket)</span><span class="sxs-lookup"><span data-stu-id="19da1-135">Step 3: Use hello URL of hello published web service toopublish tooAzure Marketplace (DataMarket)</span></span>
1. <span data-ttu-id="19da1-136">Přejděte příliš[Azure Marketplace (trhu dat)](http://datamarket.azure.com/home)</span><span class="sxs-lookup"><span data-stu-id="19da1-136">Navigate too[Azure Marketplace (Data Market)](http://datamarket.azure.com/home)</span></span> 
2. <span data-ttu-id="19da1-137">Klikněte na hello **publikovat** odkaz hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="19da1-137">Click on hello **Publish** link at hello top of hello page.</span></span> <span data-ttu-id="19da1-138">Tím přejdete toohello [publikování portálu Microsoft Azure](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="19da1-138">This will take you toohello [Microsoft Azure Publishing Portal](https://publish.windowsazure.com)</span></span>
3. <span data-ttu-id="19da1-139">Klikněte na hello **vydavatelů** části tooregister jako vydavatel.</span><span class="sxs-lookup"><span data-stu-id="19da1-139">Click on hello **publishers** section tooregister as a publisher.</span></span>
4. <span data-ttu-id="19da1-140">Při vytváření nové nabídky, vyberte **datové služby**, pak klikněte na tlačítko **vytvořit novou službu Data**.</span><span class="sxs-lookup"><span data-stu-id="19da1-140">When creating a new offer, select **Data Services**, then click **Create a New Data Service**.</span></span> 
   
   ![Azure Marketplace][image1]
   
   <br />
5. <span data-ttu-id="19da1-142">V části **plány** poskytují informace o vaší nabídky, včetně cenový plán.</span><span class="sxs-lookup"><span data-stu-id="19da1-142">Under **Plans** provide information on your offering, including a pricing plan.</span></span> <span data-ttu-id="19da1-143">Rozhodněte, pokud nabízíte volné nebo placené služby.</span><span class="sxs-lookup"><span data-stu-id="19da1-143">Decide if you will offer a free or paid service.</span></span> <span data-ttu-id="19da1-144">tooget placené, zadání platebních údajů, jako jsou vaše informace o bank a daň.</span><span class="sxs-lookup"><span data-stu-id="19da1-144">tooget paid, provide payment information such as your bank and tax information.</span></span>
6. <span data-ttu-id="19da1-145">V části **Marketing** poskytují informace o vaší nabídky, jako je například hello název a popis pro vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="19da1-145">Under **Marketing** provide information about your offer, such as hello title and description for your offer.</span></span>
7. <span data-ttu-id="19da1-146">V části **cenová** můžete nastavit hello ceny pro plánu pro konkrétní země, nebo to hello systému "autoprice" vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="19da1-146">Under **Pricing** you can set hello price for your plans for specific countries, or let hello system "autoprice" your offer.</span></span>
8. <span data-ttu-id="19da1-147">Na hello **služba dat** , klikněte na **webové služby** jako hello **zdroj dat**.</span><span class="sxs-lookup"><span data-stu-id="19da1-147">On hello **Data Service** tab, click **Web Service** as hello **Data Source**.</span></span>
   
    ![Azure Marketplace][image2]
9. <span data-ttu-id="19da1-149">Získáte klíč adresy URL a rozhraní API webové služby hello z hello portálu Azure Classic, jak je popsáno v kroku 2 výše.</span><span class="sxs-lookup"><span data-stu-id="19da1-149">Get hello web service URL and API key from hello Azure Classic Portal, as explained in step 2 above.</span></span>
10. <span data-ttu-id="19da1-150">V hello Marketplace. Data služby instalace dialogové okno, vložte do hello adresa koncového bodu OData hello **adresa URL služby** textové pole.</span><span class="sxs-lookup"><span data-stu-id="19da1-150">In hello Marketplace Data Service setup dialog box, paste hello OData endpoint address into hello **Service URL** text box.</span></span>
11. <span data-ttu-id="19da1-151">Pro **ověřování**, zvolte **záhlaví** jako hello **schéma ověřování**.</span><span class="sxs-lookup"><span data-stu-id="19da1-151">For **Authentication**, choose **Header** as hello **Authentication Scheme**.</span></span>
    
    * <span data-ttu-id="19da1-152">Zadejte "Autorizace" hello **název hlavičky**.</span><span class="sxs-lookup"><span data-stu-id="19da1-152">Enter "Authorization" for hello **Header Name**.</span></span>
    * <span data-ttu-id="19da1-153">Pro hello **hodnota hlavičky**, zadejte "Nosiče" (bez uvozovek hello), klikněte na hello **místo** panel a potom vložte klíč hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="19da1-153">For hello **Header Value**, enter "Bearer" (without hello quotation marks), click hello **Space** bar, and then paste hello API key.</span></span>
    * <span data-ttu-id="19da1-154">Vyberte hello **tato služba je OData** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="19da1-154">Select hello **This Service is OData** check box.</span></span>
    * <span data-ttu-id="19da1-155">Klikněte na tlačítko **Test připojení** tootest hello připojení.</span><span class="sxs-lookup"><span data-stu-id="19da1-155">Click **Test Connection** tootest hello connection.</span></span>
12. <span data-ttu-id="19da1-156">V části **kategorie**, ujistěte se, **Machine Learning** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="19da1-156">Under **Categories**, ensure **Machine Learning** is selected.</span></span>
13. <span data-ttu-id="19da1-157">Po dokončení všech zadávání hello metadata o vaši nabídku, klikněte na **publikovat**a potom **Push tooStaging**.</span><span class="sxs-lookup"><span data-stu-id="19da1-157">When you are done entering all hello metadata about your offer, click on **Publish**, and then **Push tooStaging**.</span></span> <span data-ttu-id="19da1-158">V tomto okamžiku jste budou informováni o všechny zbývající problémy, je nutné, aby toofix.</span><span class="sxs-lookup"><span data-stu-id="19da1-158">At this point, you will be notified of any remaining issues that you need toofix.</span></span>
14. <span data-ttu-id="19da1-159">Po dokončení všech zbývajících problémů hello zajistíte, klikněte na **požádat o schválení toopush tooProduction**.</span><span class="sxs-lookup"><span data-stu-id="19da1-159">After you have ensured completion of all hello outstanding issues, click on **Request approval toopush tooProduction**.</span></span> <span data-ttu-id="19da1-160">proces publikování Hello může trvat několik pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="19da1-160">hello publishing process can take a few business days.</span></span> 

[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png

