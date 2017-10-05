---
title: "(nepoužívané) Publikování machine learning webové služby pro Azure Marketplace | Microsoft Docs"
description: "(nepoužívané) Jak publikovat webovou službu Azure Machine Learning v Azure Marketplace"
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
redirect_document_id: TRUE
ms.openlocfilehash: 3e3420872f0c604e027d1f309a6de6f52a5a788c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a><span data-ttu-id="bc038-103">(nepoužívané) Publikovat webovou službu Azure Machine Learning v Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="bc038-103">(deprecated) Publish Azure Machine Learning Web Service to the Azure Marketplace</span></span>

> [!NOTE]
> <span data-ttu-id="bc038-104">DataMarket a Data služby se postupně vyřazuje z provozu a odběry vyřadí a zrušena od 3/31/2017.</span><span class="sxs-lookup"><span data-stu-id="bc038-104">DataMarket and Data Services are being retired, and existing subscriptions will be retired and cancelled starting 3/31/2017.</span></span> <span data-ttu-id="bc038-105">V důsledku toho je zastaralá v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="bc038-105">As a result, this article is being deprecated.</span></span> 
> 
> <span data-ttu-id="bc038-106">Jako alternativu, můžete publikovat Machine Learning experimentů k [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) prospěch komunity vědecké účely data.</span><span class="sxs-lookup"><span data-stu-id="bc038-106">As an alternative, you can publish your Machine Learning experiments to the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) for the benefit of the data science community.</span></span> <span data-ttu-id="bc038-107">Další informace najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span><span class="sxs-lookup"><span data-stu-id="bc038-107">For more information, see [Share and discover resources in the Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span></span>

<span data-ttu-id="bc038-108">Azure Marketplace nabízí možnost publikování webové služby Azure Machine Learning jako placené nebo bezplatné služby pro spotřeba externí zákazníky.</span><span class="sxs-lookup"><span data-stu-id="bc038-108">The Azure Marketplace provides the ability to publish Azure Machine Learning web services as paid or free services for consumption by external customers.</span></span> <span data-ttu-id="bc038-109">Tento článek obsahuje přehled procesu s odkazy na pokyny, které vám pomůžou začít.</span><span class="sxs-lookup"><span data-stu-id="bc038-109">This article provides an overview of that process with links to guidelines to get you started.</span></span> <span data-ttu-id="bc038-110">Pomocí tohoto procesu můžete nastavit webové služby k dispozici pro ostatní vývojáři využívat ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="bc038-110">By using this process, you can make your web services available for other developers to consume in their applications.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a><span data-ttu-id="bc038-111">Přehled procesu publikování</span><span class="sxs-lookup"><span data-stu-id="bc038-111">Overview of the publishing process</span></span>
<span data-ttu-id="bc038-112">Tady jsou kroky pro publikování na webu Azure Marketplace webové služby Azure Machine Learning:</span><span class="sxs-lookup"><span data-stu-id="bc038-112">The following are the steps for publishing an Azure Machine Learning web service to Azure Marketplace:</span></span>

1. <span data-ttu-id="bc038-113">Vytvoření a publikování služby Machine Learning požadavků a odpovědí (záznamy RR)</span><span class="sxs-lookup"><span data-stu-id="bc038-113">Create and publish a Machine Learning Request-Response service (RRS)</span></span>
2. <span data-ttu-id="bc038-114">Nasazení služby do produkčního prostředí a získat klíč rozhraní API a OData informace koncový bod.</span><span class="sxs-lookup"><span data-stu-id="bc038-114">Deploy the service to production, and obtain the API Key and OData endpoint information.</span></span>
3. <span data-ttu-id="bc038-115">Použijte adresu URL publikované webové služby pro publikování do [Azure Marketplace (trhu dat)](https://publish.windowsazure.com/workspace/)</span><span class="sxs-lookup"><span data-stu-id="bc038-115">Use the URL of the published web service to publish to [Azure Marketplace (Data Market)](https://publish.windowsazure.com/workspace/)</span></span> 
4. <span data-ttu-id="bc038-116">Po odeslání vaši nabídku zkontrolovány a vyžaduje schválení před vašim zákazníkům můžete spustit zakoupení ho.</span><span class="sxs-lookup"><span data-stu-id="bc038-116">Once submitted, your offer is reviewed and needs to be approved before your customers can start purchasing it.</span></span> <span data-ttu-id="bc038-117">Proces publikování může trvat několik pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="bc038-117">The publishing process can take a few business days.</span></span> 

## <a name="walk-through"></a><span data-ttu-id="bc038-118">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="bc038-118">Walk through</span></span>
### <a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a><span data-ttu-id="bc038-119">Krok 1: Vytvoření a publikování služby Machine Learning požadavků a odpovědí (záznamy RR)</span><span class="sxs-lookup"><span data-stu-id="bc038-119">Step 1: Create and publish a Machine Learning Request-Response service (RRS)</span></span>
 <span data-ttu-id="bc038-120">Pokud jste to ještě neudělali, proveďte prosím podívejte se na to [provede](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="bc038-120">If you have not done this already, please take a look at this [walk through](machine-learning-walkthrough-5-publish-web-service.md).</span></span>

### <a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a><span data-ttu-id="bc038-121">Krok 2: Nasazení služby do produkčního prostředí a získat klíč rozhraní API a informace o koncový bod OData</span><span class="sxs-lookup"><span data-stu-id="bc038-121">Step 2: Deploy the service to production, and obtain the API Key and OData endpoint information</span></span>
1. <span data-ttu-id="bc038-122">Z [portálu Azure Classic](http://manage.windowsazure.com), vyberte **MACHINE LEARNING** možnost levém navigačním panelu a vyberte pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="bc038-122">From the [Azure Classic Portal](http://manage.windowsazure.com), select the **MACHINE LEARNING** option from the left navigation bar, and select your workspace.</span></span> 
2. <span data-ttu-id="bc038-123">Klikněte na **webové služby** a vyberte webovou službu, kterou chcete publikovat na webu marketplace.</span><span class="sxs-lookup"><span data-stu-id="bc038-123">Click on the **WEB SERVICES** tab, and select the web service you would like to publish to the marketplace.</span></span>
   
    ![Azure Marketplace][workspace]
3. <span data-ttu-id="bc038-125">Vyberte, chcete mít v marketplace využívat koncový bod.</span><span class="sxs-lookup"><span data-stu-id="bc038-125">Select the endpoint you would like to have the marketplace consume.</span></span> <span data-ttu-id="bc038-126">Pokud jste nevytvořili žádné další koncové body, můžete vybrat **výchozí** koncový bod.</span><span class="sxs-lookup"><span data-stu-id="bc038-126">If you have not created any additional endpoints, you can select the **Default** endpoint.</span></span>
4. <span data-ttu-id="bc038-127">Po klepnutí na koncovém bodu, bude moci zobrazit **klíč rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="bc038-127">Once you have clicked on the endpoint, you will be able to see the **API KEY**.</span></span> <span data-ttu-id="bc038-128">Je nutné tuto část informací později v kroku 3, proto zkontrolujte jeho kopii.</span><span class="sxs-lookup"><span data-stu-id="bc038-128">You will need this piece of information later on in Step 3, so make a copy of it.</span></span>
   
    ![Azure Marketplace][apikey]
5. <span data-ttu-id="bc038-130">Klikněte na **požadavků a odpovědí** metody v tomto okamžiku nepodporujeme publikování služby batch provádění na Marketplace s cílem.</span><span class="sxs-lookup"><span data-stu-id="bc038-130">Click on the **REQUEST/RESPONSE** method, at this point we do not support publishing batch execution services to the marketplace.</span></span> <span data-ttu-id="bc038-131">Která se dostanete na stránku nápovědy rozhraní API pro metodu požadavků a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="bc038-131">That will take you to the API help page for the Request/Response method.</span></span>
6. <span data-ttu-id="bc038-132">Kopírování **adresa koncového bodu OData**, budete potřebovat tyto informace později v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="bc038-132">Copy the **OData Endpoint Address**, you will need this information later on in Step 3.</span></span>
   
    ![Azure Marketplace][odata]

<span data-ttu-id="bc038-134">nasazení služby do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="bc038-134">deploy the service into production.</span></span>

### <a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a><span data-ttu-id="bc038-135">Krok 3: Použijte adresu URL publikovanou webovou službu publikování na webu Azure Marketplace (DataMarket)</span><span class="sxs-lookup"><span data-stu-id="bc038-135">Step 3: Use the URL of the published web service to publish to Azure Marketplace (DataMarket)</span></span>
1. <span data-ttu-id="bc038-136">Přejděte na [Azure Marketplace (trhu dat)](http://datamarket.azure.com/home)</span><span class="sxs-lookup"><span data-stu-id="bc038-136">Navigate to [Azure Marketplace (Data Market)](http://datamarket.azure.com/home)</span></span> 
2. <span data-ttu-id="bc038-137">Klikněte na **publikovat** odkaz v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="bc038-137">Click on the **Publish** link at the top of the page.</span></span> <span data-ttu-id="bc038-138">Bude to trvat, abyste [publikování portálu Microsoft Azure](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="bc038-138">This will take you to the [Microsoft Azure Publishing Portal](https://publish.windowsazure.com)</span></span>
3. <span data-ttu-id="bc038-139">Klikněte na **vydavatelů** části k registraci jako vydavatel.</span><span class="sxs-lookup"><span data-stu-id="bc038-139">Click on the **publishers** section to register as a publisher.</span></span>
4. <span data-ttu-id="bc038-140">Při vytváření nové nabídky, vyberte **datové služby**, pak klikněte na tlačítko **vytvořit novou službu Data**.</span><span class="sxs-lookup"><span data-stu-id="bc038-140">When creating a new offer, select **Data Services**, then click **Create a New Data Service**.</span></span> 
   
   ![Azure Marketplace][image1]
   
   <br />
5. <span data-ttu-id="bc038-142">V části **plány** poskytují informace o vaší nabídky, včetně cenový plán.</span><span class="sxs-lookup"><span data-stu-id="bc038-142">Under **Plans** provide information on your offering, including a pricing plan.</span></span> <span data-ttu-id="bc038-143">Rozhodněte, pokud nabízíte volné nebo placené služby.</span><span class="sxs-lookup"><span data-stu-id="bc038-143">Decide if you will offer a free or paid service.</span></span> <span data-ttu-id="bc038-144">K získání placené, zadání platebních údajů, jako jsou vaše informace o bank a daň.</span><span class="sxs-lookup"><span data-stu-id="bc038-144">To get paid, provide payment information such as your bank and tax information.</span></span>
6. <span data-ttu-id="bc038-145">V části **Marketing** poskytují informace o vaší nabídky, jako je název a popis pro vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="bc038-145">Under **Marketing** provide information about your offer, such as the title and description for your offer.</span></span>
7. <span data-ttu-id="bc038-146">V části **cenová** můžete nastavit ceny pro plánu pro konkrétní země, nebo aby systém "autoprice" vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="bc038-146">Under **Pricing** you can set the price for your plans for specific countries, or let the system "autoprice" your offer.</span></span>
8. <span data-ttu-id="bc038-147">Na **služba dat** , klikněte na **webové služby** jako **zdroj dat**.</span><span class="sxs-lookup"><span data-stu-id="bc038-147">On the **Data Service** tab, click **Web Service** as the **Data Source**.</span></span>
   
    ![Azure Marketplace][image2]
9. <span data-ttu-id="bc038-149">Získání klíče služby webové adresy URL a rozhraní API z klasického portálu Azure, jak je popsáno v kroku 2 výše.</span><span class="sxs-lookup"><span data-stu-id="bc038-149">Get the web service URL and API key from the Azure Classic Portal, as explained in step 2 above.</span></span>
10. <span data-ttu-id="bc038-150">V dialogovém okně Nastavení Marketplace. Data Service, vložte adresu koncový bod OData do **adresa URL služby** textové pole.</span><span class="sxs-lookup"><span data-stu-id="bc038-150">In the Marketplace Data Service setup dialog box, paste the OData endpoint address into the **Service URL** text box.</span></span>
11. <span data-ttu-id="bc038-151">Pro **ověřování**, zvolte **záhlaví** jako **schéma ověřování**.</span><span class="sxs-lookup"><span data-stu-id="bc038-151">For **Authentication**, choose **Header** as the **Authentication Scheme**.</span></span>
    
    * <span data-ttu-id="bc038-152">Zadejte "Autorizace" **název hlavičky**.</span><span class="sxs-lookup"><span data-stu-id="bc038-152">Enter "Authorization" for the **Header Name**.</span></span>
    * <span data-ttu-id="bc038-153">Pro **hodnota hlavičky**, zadejte "Nosiče" (bez uvozovek), klikněte na tlačítko **místo** panel a vložte klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="bc038-153">For the **Header Value**, enter "Bearer" (without the quotation marks), click the **Space** bar, and then paste the API key.</span></span>
    * <span data-ttu-id="bc038-154">Vyberte **tato služba je OData** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="bc038-154">Select the **This Service is OData** check box.</span></span>
    * <span data-ttu-id="bc038-155">Klikněte na tlačítko **otestovat připojení** k testování připojení.</span><span class="sxs-lookup"><span data-stu-id="bc038-155">Click **Test Connection** to test the connection.</span></span>
12. <span data-ttu-id="bc038-156">V části **kategorie**, ujistěte se, **Machine Learning** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="bc038-156">Under **Categories**, ensure **Machine Learning** is selected.</span></span>
13. <span data-ttu-id="bc038-157">Po dokončení zadávání všechna metadata o vaši nabídku, klikněte na **publikovat**a potom **nabízená pracovní**.</span><span class="sxs-lookup"><span data-stu-id="bc038-157">When you are done entering all the metadata about your offer, click on **Publish**, and then **Push to Staging**.</span></span> <span data-ttu-id="bc038-158">V tomto okamžiku budete upozorněni všechny zbývající problémy, které je třeba opravit.</span><span class="sxs-lookup"><span data-stu-id="bc038-158">At this point, you will be notified of any remaining issues that you need to fix.</span></span>
14. <span data-ttu-id="bc038-159">Po dokončení všech zbývajících problémů zajistíte, klikněte na **požádat o schválení tak, aby nabízel do produkčního prostředí**.</span><span class="sxs-lookup"><span data-stu-id="bc038-159">After you have ensured completion of all the outstanding issues, click on **Request approval to push to Production**.</span></span> <span data-ttu-id="bc038-160">Proces publikování může trvat několik pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="bc038-160">The publishing process can take a few business days.</span></span> 

[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png

