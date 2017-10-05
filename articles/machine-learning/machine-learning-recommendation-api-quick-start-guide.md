---
title: "Rychlý start: Azure Machine Learning doporučení API (verze 1) | Microsoft Docs"
description: "Azure Machine Learning doporučení – úvodní příručka"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 5bce1a4a-1ad6-473f-812b-84f800fdc09a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 0a9d0b6aa1ef734a857ecc16777ba6250909b38d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="quick-start-guide-for-the-machine-learning-recommendations-api-version-1"></a><span data-ttu-id="2f4e0-104">Úvodní příručka pro Machine Learning doporučení API (verze 1)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-104">Quick start guide for the Machine Learning Recommendations API (version 1)</span></span>

> [!NOTE]
> <span data-ttu-id="2f4e0-105">Měli byste začít používat [kognitivní služby API doporučení](https://www.microsoft.com/cognitive-services/recommendations-api) místo tuto verzi.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-105">You should start using the [Recommendations API Cognitive Service](https://www.microsoft.com/cognitive-services/recommendations-api) instead of this version.</span></span> <span data-ttu-id="2f4e0-106">Službu kognitivní doporučení budou nahrazení této služby, a všechny nové funkce bude vyvinutý existuje.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-106">The Recommendations Cognitive Service will be replacing this service, and all the new features will be developed there.</span></span> <span data-ttu-id="2f4e0-107">Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-107">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
>
> <span data-ttu-id="2f4e0-108">Další informace o [migraci na novou službu kognitivní](http://aka.ms/recomigrate).</span><span class="sxs-lookup"><span data-stu-id="2f4e0-108">Learn more about [Migrating to the new Cognitive Service](http://aka.ms/recomigrate).</span></span>
> 
> 

<span data-ttu-id="2f4e0-109">Tento dokument popisuje, jak se budou registrovat službou nebo aplikací používat Microsoft Azure Machine Learning doporučení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-109">This document describes how to onboard your service or application to use Microsoft Azure Machine Learning Recommendations.</span></span> <span data-ttu-id="2f4e0-110">Další informace naleznete na volání rozhraní API doporučení v [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).</span><span class="sxs-lookup"><span data-stu-id="2f4e0-110">You can find more details on the Recommendations API in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="general-overview"></a><span data-ttu-id="2f4e0-111">Obecné – přehled</span><span class="sxs-lookup"><span data-stu-id="2f4e0-111">General overview</span></span>
<span data-ttu-id="2f4e0-112">Pokud chcete používat Azure Machine Learning doporučení, budete muset provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-112">To use Azure Machine Learning Recommendations, you need to take the following steps:</span></span>

* <span data-ttu-id="2f4e0-113">Vytvoření modelu – model je kontejner pro data o využití, data katalogu a model doporučení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-113">Create a model - A model is a container of your usage data, catalog data, and the recommendation model.</span></span>
* <span data-ttu-id="2f4e0-114">Data katalogu import - katalog obsahuje informace o metadatech na položky.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-114">Import catalog data - A catalog contains metadata information on the items.</span></span> 
* <span data-ttu-id="2f4e0-115">Data o využití import - data o využití je možné uložit v jedné ze dvou způsobů (nebo obě):</span><span class="sxs-lookup"><span data-stu-id="2f4e0-115">Import usage data - Usage data can be uploaded in one of two ways (or both):</span></span>
  * <span data-ttu-id="2f4e0-116">Tím, že nahrajete soubor, který obsahuje data o využití.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-116">By uploading a file that contains the usage data.</span></span>
  * <span data-ttu-id="2f4e0-117">Odesláním dat pořízení události.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-117">By sending data acquisition events.</span></span>
    <span data-ttu-id="2f4e0-118">Obvykle nahrát soubor využití aby mohl vytvořit model počáteční doporučení (zavedení) a jeho použití, dokud systém shromažďuje dostatek dat pomocí formátu data pořízení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-118">Usually you upload a usage file in order to be able to create an initial recommendation model (bootstrap) and use it until the system gathers enough data by using the data acquisition format.</span></span>
* <span data-ttu-id="2f4e0-119">Vytvoření modelu doporučení – to je asynchronní operace, ve kterém systém doporučení trvá všech dat o využití a vytvoří model doporučení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-119">Build a recommendation model - This is an asynchronous operation in which the recommendation system takes all the usage data and creates a recommendation model.</span></span> <span data-ttu-id="2f4e0-120">Tato operace může trvat několik minut nebo i několik hodin v závislosti na velikosti dat a parametry konfigurace sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-120">This operation can take several minutes or several hours depending on the size of the data and the build configuration parameters.</span></span> <span data-ttu-id="2f4e0-121">Při spuštění sestavení, zobrazí se o ID sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-121">When triggering the build, you will get a build ID.</span></span> <span data-ttu-id="2f4e0-122">Použijte ho ke kontrole při ukončení procesu sestavení před zahájením využívat doporučení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-122">Use it to check when the build process has ended before starting to consume recommendations.</span></span>
* <span data-ttu-id="2f4e0-123">Využívat doporučení - Get doporučení pro určitou položku nebo seznam položek.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-123">Consume recommendations - Get recommendations for a specific item or list of items.</span></span>

<span data-ttu-id="2f4e0-124">Všechny výše uvedené kroky se provádějí pomocí rozhraní API služby Azure Machine Learning doporučení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-124">All the steps above are done through the Azure Machine Learning Recommendations API.</span></span>  <span data-ttu-id="2f4e0-125">Si můžete stáhnout ukázkovou aplikaci, která implementuje každý z těchto kroků z [také galerie.](http://1drv.ms/1xeO2F3)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-125">You can download a sample application that implements each of these steps from the [gallery as well.](http://1drv.ms/1xeO2F3)</span></span>

## <a name="limitations"></a><span data-ttu-id="2f4e0-126">Omezení</span><span class="sxs-lookup"><span data-stu-id="2f4e0-126">Limitations</span></span>
* <span data-ttu-id="2f4e0-127">Maximální počet modelů podle předplatného je 10.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-127">The maximum number of models per subscription is 10.</span></span>
* <span data-ttu-id="2f4e0-128">Maximální počet položek, které mohou být uloženy katalog je 100 000.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-128">The maximum number of items that a catalog can hold is 100,000.</span></span>
* <span data-ttu-id="2f4e0-129">Maximální počet bodů využití, které jsou zachovány je ~ 5 000 000.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-129">The maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="2f4e0-130">Nejstarší budou odstraněna, pokud nové se nahrál nebo nahlásí.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-130">The oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="2f4e0-131">Maximální velikost dat, který může odeslat v BLOGU (například import katalogu dat, data o využití import) je 200MB.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-131">The maximum size of data that can be sent in POST (for example, import catalog data, import usage data) is 200MB.</span></span>
* <span data-ttu-id="2f4e0-132">Počet transakcí za sekundu pro sestavení modelu doporučení, která není aktivní je ~ 2TPS.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-132">The number of transactions per second for a recommendation model build that is not active is ~2TPS.</span></span> <span data-ttu-id="2f4e0-133">Sestavení modelu doporučení, která je aktivní mohou být uloženy do 20TPS.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-133">A recommendation model build that is active can hold up to 20TPS.</span></span>

## <a name="integration"></a><span data-ttu-id="2f4e0-134">Integrace</span><span class="sxs-lookup"><span data-stu-id="2f4e0-134">Integration</span></span>
### <a name="authentication"></a><span data-ttu-id="2f4e0-135">Authentication</span><span class="sxs-lookup"><span data-stu-id="2f4e0-135">Authentication</span></span>
<span data-ttu-id="2f4e0-136">Microsoft Azure Marketplace podporuje základní nebo OAuth metodu ověřování.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-136">Microsoft Azure Marketplace supports either the Basic or OAuth authentication method.</span></span> <span data-ttu-id="2f4e0-137">Klíče účtu budete moci snadno najít tak, že přejdete na klíče ve marketplace v oddíle [nastavení svého účtu](https://datamarket.azure.com/account/keys).</span><span class="sxs-lookup"><span data-stu-id="2f4e0-137">You can easily find the account keys by navigating to the keys in the marketplace under [your account settings](https://datamarket.azure.com/account/keys).</span></span> 

#### <a name="basic-authentication"></a><span data-ttu-id="2f4e0-138">Základní ověřování</span><span class="sxs-lookup"><span data-stu-id="2f4e0-138">Basic Authentication</span></span>
<span data-ttu-id="2f4e0-139">Přidáte hlavičku autorizace:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-139">Add the Authorization header:</span></span>

    Authorization: Basic <creds>

    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  

<span data-ttu-id="2f4e0-140">Převést na Base64 (C#)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-140">Convert to Base64 (C#)</span></span>

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);

<span data-ttu-id="2f4e0-141">Převést na Base64 (JavaScript)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-141">Convert to Base64 (JavaScript)</span></span>

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);




### <a name="service-uri"></a><span data-ttu-id="2f4e0-142">URI služby</span><span class="sxs-lookup"><span data-stu-id="2f4e0-142">Service URI</span></span>
<span data-ttu-id="2f4e0-143">Kořenový adresář identifikátor URI pro rozhraní API služby Azure Machine Learning doporučení je [sem.](https://api.datamarket.azure.com/amla/recommendations/v2/)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-143">The service root URI for the Azure Machine Learning Recommendations APIs is [here.](https://api.datamarket.azure.com/amla/recommendations/v2/)</span></span>

<span data-ttu-id="2f4e0-144">Službu úplný identifikátor URI je vyjádřit pomocí elementů specifikace prostředí OData.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-144">The full service URI is expressed using elements of the OData specification.</span></span>

### <a name="api-version"></a><span data-ttu-id="2f4e0-145">Verze rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2f4e0-145">API version</span></span>
<span data-ttu-id="2f4e0-146">Každé volání rozhraní API bude mít na konci, parametr dotazu s názvem apiVersion, který musí být nastavena na "1.0".</span><span class="sxs-lookup"><span data-stu-id="2f4e0-146">Each API call will have, at the end, a query parameter called apiVersion that should be set to "1.0".</span></span>

### <a name="ids-are-case-sensitive"></a><span data-ttu-id="2f4e0-147">ID jsou malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="2f4e0-147">IDs are case-sensitive</span></span>
<span data-ttu-id="2f4e0-148">ID, podle rozhraní API, vrátil malých a velkých písmen a by měl být použit jako takový, při předány jako parametry při následných voláních rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-148">IDs, returned by any of the APIs, are case-sensitive and should be used as such when passed as parameters in subsequent API calls.</span></span> <span data-ttu-id="2f4e0-149">ID modelu a ID katalogu pro instanci, jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-149">For instance, model IDs and catalog IDs are case-sensitive.</span></span>

### <a name="create-a-model"></a><span data-ttu-id="2f4e0-150">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="2f4e0-150">Create a model</span></span>
<span data-ttu-id="2f4e0-151">Vytváří se žádost o "Vytvoření modelu":</span><span class="sxs-lookup"><span data-stu-id="2f4e0-151">Creating a "create model" request:</span></span>

| <span data-ttu-id="2f4e0-152">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="2f4e0-152">HTTP Method</span></span> | <span data-ttu-id="2f4e0-153">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="2f4e0-153">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-154">POST</span><span class="sxs-lookup"><span data-stu-id="2f4e0-154">POST</span></span> |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f4e0-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-155">Example:</span></span><br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f4e0-156">Název parametru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-156">Parameter Name</span></span> | <span data-ttu-id="2f4e0-157">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="2f4e0-157">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-158">%{ModelName/</span><span class="sxs-lookup"><span data-stu-id="2f4e0-158">modelName</span></span> |<span data-ttu-id="2f4e0-159">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-159">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="2f4e0-160">Maximální délka: 20</span><span class="sxs-lookup"><span data-stu-id="2f4e0-160">Max length: 20</span></span> |
| <span data-ttu-id="2f4e0-161">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f4e0-161">apiVersion</span></span> |<span data-ttu-id="2f4e0-162">1.0</span><span class="sxs-lookup"><span data-stu-id="2f4e0-162">1.0</span></span> |
|  | |
| <span data-ttu-id="2f4e0-163">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="2f4e0-163">Request Body</span></span> |<span data-ttu-id="2f4e0-164">NONE</span><span class="sxs-lookup"><span data-stu-id="2f4e0-164">NONE</span></span> |

<span data-ttu-id="2f4e0-165">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-165">**Response**:</span></span>

<span data-ttu-id="2f4e0-166">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="2f4e0-166">HTTP Status code: 200</span></span>

* <span data-ttu-id="2f4e0-167">`feed/entry/content/properties/id`-Obsahuje ID modelu.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-167">`feed/entry/content/properties/id` - Contains the model ID.</span></span>
  <span data-ttu-id="2f4e0-168">Všimněte si, že ID modelu je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-168">Note that the model ID is case-sensitive.</span></span>

<span data-ttu-id="2f4e0-169">OData XML</span><span class="sxs-lookup"><span data-stu-id="2f4e0-169">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-catalog-data"></a><span data-ttu-id="2f4e0-170">Umožňuje importovat catalog data</span><span class="sxs-lookup"><span data-stu-id="2f4e0-170">Import catalog data</span></span>
<span data-ttu-id="2f4e0-171">Pokud nahrajete několik katalog souborů do stejného modelu s několika volání, jsme vloží nové položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-171">If you upload several catalog files to the same model with several calls, we will insert only the new catalog items.</span></span> <span data-ttu-id="2f4e0-172">Existující položky zůstane s původními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-172">Existing items will remain with the original values.</span></span>

| <span data-ttu-id="2f4e0-173">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="2f4e0-173">HTTP Method</span></span> | <span data-ttu-id="2f4e0-174">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="2f4e0-174">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-175">POST</span><span class="sxs-lookup"><span data-stu-id="2f4e0-175">POST</span></span> |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f4e0-176">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-176">Example:</span></span><br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f4e0-177">Název parametru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-177">Parameter Name</span></span> | <span data-ttu-id="2f4e0-178">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="2f4e0-178">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-179">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="2f4e0-179">modelId</span></span> |<span data-ttu-id="2f4e0-180">Jedinečný identifikátor modelu (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-180">Unique identifier of the model (case-sensitive)</span></span> |
| <span data-ttu-id="2f4e0-181">Název souboru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-181">filename</span></span> |<span data-ttu-id="2f4e0-182">Textové identifikátor katalogu.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-182">Textual identifier of the catalog.</span></span><br><span data-ttu-id="2f4e0-183">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-183">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="2f4e0-184">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="2f4e0-184">Max length: 50</span></span> |
| <span data-ttu-id="2f4e0-185">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f4e0-185">apiVersion</span></span> |<span data-ttu-id="2f4e0-186">1.0</span><span class="sxs-lookup"><span data-stu-id="2f4e0-186">1.0</span></span> |
|  | |
| <span data-ttu-id="2f4e0-187">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="2f4e0-187">Request Body</span></span> |<span data-ttu-id="2f4e0-188">Data katalogu.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-188">Catalog data.</span></span> <span data-ttu-id="2f4e0-189">Formát:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-189">Format:</span></span><br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th><span data-ttu-id="2f4e0-190">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-190">Name</span></span></th><th><span data-ttu-id="2f4e0-191">Povinné</span><span class="sxs-lookup"><span data-stu-id="2f4e0-191">Mandatory</span></span></th><th><span data-ttu-id="2f4e0-192">Typ</span><span class="sxs-lookup"><span data-stu-id="2f4e0-192">Type</span></span></th><th><span data-ttu-id="2f4e0-193">Popis</span><span class="sxs-lookup"><span data-stu-id="2f4e0-193">Description</span></span></th></tr><tr><td><span data-ttu-id="2f4e0-194">Id položky</span><span class="sxs-lookup"><span data-stu-id="2f4e0-194">Item Id</span></span></td><td><span data-ttu-id="2f4e0-195">Ano</span><span class="sxs-lookup"><span data-stu-id="2f4e0-195">Yes</span></span></td><td><span data-ttu-id="2f4e0-196">Alfanumerické znaky, maximální délka 50</span><span class="sxs-lookup"><span data-stu-id="2f4e0-196">Alphanumeric, max length 50</span></span></td><td><span data-ttu-id="2f4e0-197">Jedinečný identifikátor položky</span><span class="sxs-lookup"><span data-stu-id="2f4e0-197">Unique identifier of an item</span></span></td></tr><tr><td><span data-ttu-id="2f4e0-198">Název položky</span><span class="sxs-lookup"><span data-stu-id="2f4e0-198">Item Name</span></span></td><td><span data-ttu-id="2f4e0-199">Ano</span><span class="sxs-lookup"><span data-stu-id="2f4e0-199">Yes</span></span></td><td><span data-ttu-id="2f4e0-200">Alfanumerické znaky, maximální délka 255</span><span class="sxs-lookup"><span data-stu-id="2f4e0-200">Alphanumeric, max length 255</span></span></td><td><span data-ttu-id="2f4e0-201">Název položky</span><span class="sxs-lookup"><span data-stu-id="2f4e0-201">Item name</span></span></td></tr><tr><td><span data-ttu-id="2f4e0-202">Kategorie položky</span><span class="sxs-lookup"><span data-stu-id="2f4e0-202">Item Category</span></span></td><td><span data-ttu-id="2f4e0-203">Ano</span><span class="sxs-lookup"><span data-stu-id="2f4e0-203">Yes</span></span></td><td><span data-ttu-id="2f4e0-204">Alfanumerické znaky, maximální délka 255</span><span class="sxs-lookup"><span data-stu-id="2f4e0-204">Alphanumeric, max length 255</span></span></td><td><span data-ttu-id="2f4e0-205">Kategorie, do které patří tato položka (například vaření knihy, obrázkům...)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-205">Category to which this item belongs (for example, Cooking Books, Drama…)</span></span></td></tr><tr><td><span data-ttu-id="2f4e0-206">Popis</span><span class="sxs-lookup"><span data-stu-id="2f4e0-206">Description</span></span></td><td><span data-ttu-id="2f4e0-207">Ne</span><span class="sxs-lookup"><span data-stu-id="2f4e0-207">No</span></span></td><td><span data-ttu-id="2f4e0-208">Alfanumerické znaky, maximální délka je 4000</span><span class="sxs-lookup"><span data-stu-id="2f4e0-208">Alphanumeric, max length 4000</span></span></td><td><span data-ttu-id="2f4e0-209">Popis této položky</span><span class="sxs-lookup"><span data-stu-id="2f4e0-209">Description of this item</span></span></td></tr></table><br><span data-ttu-id="2f4e0-210">Maximální velikost souboru je 200MB.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-210">Maximum file size is 200MB.</span></span><br><br><span data-ttu-id="2f4e0-211">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-211">Example:</span></span><br><code>2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book<br>21bf8088-b6c0-4509-870c-e1c7ac78304a,The Forgetting Room: A Fiction (Byzantium Book),Book<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book<br>552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book</code> |

<span data-ttu-id="2f4e0-212">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-212">**Response**:</span></span>

<span data-ttu-id="2f4e0-213">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="2f4e0-213">HTTP Status code: 200</span></span>

* <span data-ttu-id="2f4e0-214">`Feed\entry\content\properties\LineCount`-Přijaté počet řádků.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-214">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="2f4e0-215">`Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-215">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due to an error.</span></span>

<span data-ttu-id="2f4e0-216">OData XML</span><span class="sxs-lookup"><span data-stu-id="2f4e0-216">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
          <subtitle type="text">Import catalog file</subtitle>
          <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:58:04Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-usage-data"></a><span data-ttu-id="2f4e0-217">Importovat data o využití</span><span class="sxs-lookup"><span data-stu-id="2f4e0-217">Import usage data</span></span>
#### <a name="uploading-a-file"></a><span data-ttu-id="2f4e0-218">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-218">Uploading a file</span></span>
<span data-ttu-id="2f4e0-219">V této části ukazuje, jak odeslat data o využití pomocí souboru.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-219">This section shows how to upload usage data by using a file.</span></span> <span data-ttu-id="2f4e0-220">Můžete volat toto rozhraní API se data o využití.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-220">You can call this API several times with usage data.</span></span> <span data-ttu-id="2f4e0-221">Všechna data o využití se uloží pro všechna volání.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-221">All usage data will be saved for all calls.</span></span>

| <span data-ttu-id="2f4e0-222">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="2f4e0-222">HTTP Method</span></span> | <span data-ttu-id="2f4e0-223">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="2f4e0-223">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-224">POST</span><span class="sxs-lookup"><span data-stu-id="2f4e0-224">POST</span></span> |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f4e0-225">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-225">Example:</span></span><br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f4e0-226">Název parametru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-226">Parameter Name</span></span> | <span data-ttu-id="2f4e0-227">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="2f4e0-227">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-228">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="2f4e0-228">modelId</span></span> |<span data-ttu-id="2f4e0-229">Jedinečný identifikátor modelu (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-229">Unique identifier of the model (case-sensitive)</span></span> |
| <span data-ttu-id="2f4e0-230">Název souboru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-230">filename</span></span> |<span data-ttu-id="2f4e0-231">Textové identifikátor katalogu.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-231">Textual identifier of the catalog.</span></span><br><span data-ttu-id="2f4e0-232">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-232">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="2f4e0-233">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="2f4e0-233">Max length: 50</span></span> |
| <span data-ttu-id="2f4e0-234">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f4e0-234">apiVersion</span></span> |<span data-ttu-id="2f4e0-235">1.0</span><span class="sxs-lookup"><span data-stu-id="2f4e0-235">1.0</span></span> |
|  | |
| <span data-ttu-id="2f4e0-236">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="2f4e0-236">Request Body</span></span> |<span data-ttu-id="2f4e0-237">Data o využití.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-237">Usage data.</span></span> <span data-ttu-id="2f4e0-238">Formát:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-238">Format:</span></span><br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th><span data-ttu-id="2f4e0-239">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-239">Name</span></span></th><th><span data-ttu-id="2f4e0-240">Povinné</span><span class="sxs-lookup"><span data-stu-id="2f4e0-240">Mandatory</span></span></th><th><span data-ttu-id="2f4e0-241">Typ</span><span class="sxs-lookup"><span data-stu-id="2f4e0-241">Type</span></span></th><th><span data-ttu-id="2f4e0-242">Popis</span><span class="sxs-lookup"><span data-stu-id="2f4e0-242">Description</span></span></th></tr><tr><td><span data-ttu-id="2f4e0-243">Id uživatele</span><span class="sxs-lookup"><span data-stu-id="2f4e0-243">User Id</span></span></td><td><span data-ttu-id="2f4e0-244">Ano</span><span class="sxs-lookup"><span data-stu-id="2f4e0-244">Yes</span></span></td><td><span data-ttu-id="2f4e0-245">Alfanumerické</span><span class="sxs-lookup"><span data-stu-id="2f4e0-245">Alphanumeric</span></span></td><td><span data-ttu-id="2f4e0-246">Jedinečný identifikátor uživatele</span><span class="sxs-lookup"><span data-stu-id="2f4e0-246">Unique identifier of a user</span></span></td></tr><tr><td><span data-ttu-id="2f4e0-247">Id položky</span><span class="sxs-lookup"><span data-stu-id="2f4e0-247">Item Id</span></span></td><td><span data-ttu-id="2f4e0-248">Ano</span><span class="sxs-lookup"><span data-stu-id="2f4e0-248">Yes</span></span></td><td><span data-ttu-id="2f4e0-249">Alfanumerické znaky, maximální délka 50</span><span class="sxs-lookup"><span data-stu-id="2f4e0-249">Alphanumeric, max length 50</span></span></td><td><span data-ttu-id="2f4e0-250">Jedinečný identifikátor položky</span><span class="sxs-lookup"><span data-stu-id="2f4e0-250">Unique identifier of an item</span></span></td></tr><tr><td><span data-ttu-id="2f4e0-251">Čas</span><span class="sxs-lookup"><span data-stu-id="2f4e0-251">Time</span></span></td><td><span data-ttu-id="2f4e0-252">Ne</span><span class="sxs-lookup"><span data-stu-id="2f4e0-252">No</span></span></td><td><span data-ttu-id="2f4e0-253">Datum ve formátu: rrrr/MM/ddTHH (například 2013/06/20T10:00:00)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-253">Date in format: YYYY/MM/DDTHH:MM:SS (for example, 2013/06/20T10:00:00)</span></span></td><td><span data-ttu-id="2f4e0-254">Čas dat</span><span class="sxs-lookup"><span data-stu-id="2f4e0-254">Time of data</span></span></td></tr><tr><td><span data-ttu-id="2f4e0-255">Událost</span><span class="sxs-lookup"><span data-stu-id="2f4e0-255">Event</span></span></td><td><span data-ttu-id="2f4e0-256">Ne, pokud zadaný musí taky datum</span><span class="sxs-lookup"><span data-stu-id="2f4e0-256">No, if supplied then must also put date</span></span></td><td><span data-ttu-id="2f4e0-257">Jeden z následujících:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-257">One of the following:</span></span><br><span data-ttu-id="2f4e0-258">• Klikněte na tlačítko</span><span class="sxs-lookup"><span data-stu-id="2f4e0-258">• Click</span></span><br><span data-ttu-id="2f4e0-259">• RecommendationClick</span><span class="sxs-lookup"><span data-stu-id="2f4e0-259">• RecommendationClick</span></span><br><span data-ttu-id="2f4e0-260">• AddShopCart</span><span class="sxs-lookup"><span data-stu-id="2f4e0-260">•    AddShopCart</span></span><br><span data-ttu-id="2f4e0-261">• RemoveShopCart</span><span class="sxs-lookup"><span data-stu-id="2f4e0-261">• RemoveShopCart</span></span><br><span data-ttu-id="2f4e0-262">• Nákupu</span><span class="sxs-lookup"><span data-stu-id="2f4e0-262">• Purchase</span></span></td><td></td></tr></table><br><span data-ttu-id="2f4e0-263">Maximální velikost souboru je 200MB.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-263">Maximum file size is 200MB.</span></span><br><br><span data-ttu-id="2f4e0-264">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-264">Example:</span></span><br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

<span data-ttu-id="2f4e0-265">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-265">**Response**:</span></span>

<span data-ttu-id="2f4e0-266">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="2f4e0-266">HTTP Status code: 200</span></span>

* <span data-ttu-id="2f4e0-267">`Feed\entry\content\properties\LineCount`-Přijaté počet řádků.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-267">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="2f4e0-268">`Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-268">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due to an error.</span></span>
* <span data-ttu-id="2f4e0-269">`Feed\entry\content\properties\FileId`-Souboru identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-269">`Feed\entry\content\properties\FileId` - File identifier.</span></span>

<span data-ttu-id="2f4e0-270">OData XML</span><span class="sxs-lookup"><span data-stu-id="2f4e0-270">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="using-data-acquisition"></a><span data-ttu-id="2f4e0-271">Pomocí získávání dat</span><span class="sxs-lookup"><span data-stu-id="2f4e0-271">Using data acquisition</span></span>
<span data-ttu-id="2f4e0-272">V této části ukazuje, jak k odesílání událostí v reálném čase na Azure Machine Learning doporučení, obvykle z vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-272">This section shows how to send events in real time to Azure Machine Learning Recommendations, usually from your website.</span></span>

| <span data-ttu-id="2f4e0-273">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="2f4e0-273">HTTP Method</span></span> | <span data-ttu-id="2f4e0-274">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="2f4e0-274">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-275">POST</span><span class="sxs-lookup"><span data-stu-id="2f4e0-275">POST</span></span> |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f4e0-276">Název parametru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-276">Parameter Name</span></span> | <span data-ttu-id="2f4e0-277">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="2f4e0-277">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-278">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f4e0-278">apiVersion</span></span> |<span data-ttu-id="2f4e0-279">1.0</span><span class="sxs-lookup"><span data-stu-id="2f4e0-279">1.0</span></span> |
|  | |
| <span data-ttu-id="2f4e0-280">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="2f4e0-280">Request body</span></span> |<span data-ttu-id="2f4e0-281">Vkládání dat události pro všechny události, který chcete poslat.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-281">Event data entry for each event you want to send.</span></span> <span data-ttu-id="2f4e0-282">Byste měli odeslat pro stejné relace uživatele nebo prohlížeče stejné ID v poli ID relace.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-282">You should send for the same user or browser session the same ID in the SessionId field.</span></span> <span data-ttu-id="2f4e0-283">(Viz ukázka textu události, které jsou níže).</span><span class="sxs-lookup"><span data-stu-id="2f4e0-283">(See sample of event body below.)</span></span> |

* <span data-ttu-id="2f4e0-284">Příklad pro události, klikněte na tlačítko'.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-284">Example for event 'Click':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>
* <span data-ttu-id="2f4e0-285">Příklad pro událost 'RecommendationClick':</span><span class="sxs-lookup"><span data-stu-id="2f4e0-285">Example for event 'RecommendationClick':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RecommendationClick</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="2f4e0-286">Příklad pro událost 'AddShopCart':</span><span class="sxs-lookup"><span data-stu-id="2f4e0-286">Example for event 'AddShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="2f4e0-287">Příklad pro událost 'RemoveShopCart':</span><span class="sxs-lookup"><span data-stu-id="2f4e0-287">Example for event 'RemoveShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RemoveShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="2f4e0-288">Příklad pro události, nákupu'.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-288">Example for event 'Purchase':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>
* <span data-ttu-id="2f4e0-289">Příklad odesílání 2 události, klikněte na' a 'AddShopCart':</span><span class="sxs-lookup"><span data-stu-id="2f4e0-289">Example sending 2 events, 'Click' and 'AddShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>Click</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
          <ItemName>itemName</ItemName>
          <ItemDescription>item description</ItemDescription>
          <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
          </EventData>
        </Event>

<span data-ttu-id="2f4e0-290">**Odpověď**: kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="2f4e0-290">**Response**: HTTP Status code: 200</span></span>

### <a name="build-a-recommendation-model"></a><span data-ttu-id="2f4e0-291">Vytvoření modelu doporučení</span><span class="sxs-lookup"><span data-stu-id="2f4e0-291">Build a recommendation model</span></span>
| <span data-ttu-id="2f4e0-292">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="2f4e0-292">HTTP Method</span></span> | <span data-ttu-id="2f4e0-293">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="2f4e0-293">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-294">POST</span><span class="sxs-lookup"><span data-stu-id="2f4e0-294">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f4e0-295">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-295">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f4e0-296">Název parametru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-296">Parameter Name</span></span> | <span data-ttu-id="2f4e0-297">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="2f4e0-297">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-298">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="2f4e0-298">modelId</span></span> |<span data-ttu-id="2f4e0-299">Jedinečný identifikátor modelu (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-299">Unique identifier of the model (case-sensitive)</span></span> |
| <span data-ttu-id="2f4e0-300">userDescription</span><span class="sxs-lookup"><span data-stu-id="2f4e0-300">userDescription</span></span> |<span data-ttu-id="2f4e0-301">Textové identifikátor katalogu.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-301">Textual identifier of the catalog.</span></span> <span data-ttu-id="2f4e0-302">Poznámka: Pokud použijete prostory musí zakódovat ho s % 20 místo.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-302">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="2f4e0-303">Najdete v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-303">See example above.</span></span><br><span data-ttu-id="2f4e0-304">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="2f4e0-304">Max length: 50</span></span> |
| <span data-ttu-id="2f4e0-305">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f4e0-305">apiVersion</span></span> |<span data-ttu-id="2f4e0-306">1.0</span><span class="sxs-lookup"><span data-stu-id="2f4e0-306">1.0</span></span> |
|  | |
| <span data-ttu-id="2f4e0-307">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="2f4e0-307">Request Body</span></span> |<span data-ttu-id="2f4e0-308">NONE</span><span class="sxs-lookup"><span data-stu-id="2f4e0-308">NONE</span></span> |

<span data-ttu-id="2f4e0-309">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-309">**Response**:</span></span>

<span data-ttu-id="2f4e0-310">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="2f4e0-310">HTTP Status code: 200</span></span>

<span data-ttu-id="2f4e0-311">Toto je asynchronní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-311">This is an asynchronous API.</span></span> <span data-ttu-id="2f4e0-312">Zobrazí se ID sestavení jako odpověď.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-312">You will get a build ID as a response.</span></span> <span data-ttu-id="2f4e0-313">Vědět, pokud sestavení skončila, by měly volat rozhraní API "Získat sestavení stavu systému Model" a vyhledejte toto ID sestavení v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-313">To know when the build has ended, you should call the "Get Builds Status of a Model" API and locate this build ID in the response.</span></span> <span data-ttu-id="2f4e0-314">Všimněte si, že sestavení může trvat minut až hodin v závislosti na velikosti dat.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-314">Note that a build can take from minutes to hours depending on the size of the data.</span></span>

<span data-ttu-id="2f4e0-315">Nemůžete spotřebovat doporučení do sestavení ukončí.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-315">You cannot consume recommendations till the build ends.</span></span>

<span data-ttu-id="2f4e0-316">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-316">Valid build status:</span></span>

* <span data-ttu-id="2f4e0-317">Vytvoření – Model byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-317">Create – Model was created.</span></span>
* <span data-ttu-id="2f4e0-318">Ve frontě – sestavení modelu byla spuštěna a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-318">Queued – Model build was triggered and it is queued.</span></span>
* <span data-ttu-id="2f4e0-319">Sestavení – Model sestavuje.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-319">Building – Model is being built.</span></span>
* <span data-ttu-id="2f4e0-320">Úspěch – sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-320">Success – Build ended successfully.</span></span>
* <span data-ttu-id="2f4e0-321">Chyba – sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-321">Error – Build ended with a failure.</span></span>
* <span data-ttu-id="2f4e0-322">Došlo ke zrušení – sestavení bylo zrušeno.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-322">Canceled – Build was canceled.</span></span>
* <span data-ttu-id="2f4e0-323">Zrušení – sestavení probíhá její zrušení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-323">Canceling – Build is being canceled.</span></span>

<span data-ttu-id="2f4e0-324">Všimněte si, že ID sestavení naleznete v následující cestě:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="2f4e0-324">Note that the build ID can be found under the following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="2f4e0-325">OData XML</span><span class="sxs-lookup"><span data-stu-id="2f4e0-325">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="get-build-status-of-a-model"></a><span data-ttu-id="2f4e0-326">Získat stav sestavení modelu</span><span class="sxs-lookup"><span data-stu-id="2f4e0-326">Get build status of a model</span></span>
| <span data-ttu-id="2f4e0-327">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="2f4e0-327">HTTP Method</span></span> | <span data-ttu-id="2f4e0-328">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="2f4e0-328">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-329">GET</span><span class="sxs-lookup"><span data-stu-id="2f4e0-329">GET</span></span> |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f4e0-330">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-330">Example:</span></span><br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| <span data-ttu-id="2f4e0-331">Název parametru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-331">Parameter Name</span></span> | <span data-ttu-id="2f4e0-332">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="2f4e0-332">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-333">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="2f4e0-333">modelId</span></span> |<span data-ttu-id="2f4e0-334">Jedinečný identifikátor modelu (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-334">Unique identifier of the model  (case-sensitive)</span></span> |
| <span data-ttu-id="2f4e0-335">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="2f4e0-335">onlyLastBuild</span></span> |<span data-ttu-id="2f4e0-336">Označuje, zda vrátit veškerá historie sestavení modelu nebo pouze stav poslední sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-336">Indicates whether to return all the build history of the model or only the status of the most recent build.</span></span> |
| <span data-ttu-id="2f4e0-337">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f4e0-337">apiVersion</span></span> |<span data-ttu-id="2f4e0-338">1.0</span><span class="sxs-lookup"><span data-stu-id="2f4e0-338">1.0</span></span> |

<span data-ttu-id="2f4e0-339">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-339">**Response**:</span></span>

<span data-ttu-id="2f4e0-340">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="2f4e0-340">HTTP Status code: 200</span></span>

<span data-ttu-id="2f4e0-341">Odpověď obsahuje jeden záznam za sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-341">The response includes one entry per build.</span></span> <span data-ttu-id="2f4e0-342">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-342">Each entry has the following data:</span></span>

* <span data-ttu-id="2f4e0-343">`feed/entry/content/properties/UserName`– Název uživatele.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-343">`feed/entry/content/properties/UserName` – Name of the user.</span></span>
* <span data-ttu-id="2f4e0-344">`feed/entry/content/properties/ModelName`– Název modelu.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-344">`feed/entry/content/properties/ModelName` – Name of the model.</span></span>
* <span data-ttu-id="2f4e0-345">`feed/entry/content/properties/ModelId`– Model jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-345">`feed/entry/content/properties/ModelId` – Model unique identifier.</span></span>
* <span data-ttu-id="2f4e0-346">`feed/entry/content/properties/IsDeployed`– Jestli (také známa jako nasazení sestavení</span><span class="sxs-lookup"><span data-stu-id="2f4e0-346">`feed/entry/content/properties/IsDeployed` – Whether the build is deployed (a.k.a.</span></span> <span data-ttu-id="2f4e0-347">aktivní sestavení).</span><span class="sxs-lookup"><span data-stu-id="2f4e0-347">active build).</span></span>
* <span data-ttu-id="2f4e0-348">`feed/entry/content/properties/BuildId`– Vytvořte jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-348">`feed/entry/content/properties/BuildId` – Build unique identifier.</span></span>
* <span data-ttu-id="2f4e0-349">`feed/entry/content/properties/BuildType`-Typ sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-349">`feed/entry/content/properties/BuildType` - Type of the build.</span></span>
* <span data-ttu-id="2f4e0-350">`feed/entry/content/properties/Status`– Stav sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-350">`feed/entry/content/properties/Status` – Build status.</span></span> <span data-ttu-id="2f4e0-351">Může být jedna z následujících akcí: Chyba, sestavování, zařazeno do fronty, zrušení, zrušení, úspěch</span><span class="sxs-lookup"><span data-stu-id="2f4e0-351">Can be one of the following: Error, Building, Queued, Canceling, Canceled, Success</span></span>
* <span data-ttu-id="2f4e0-352">`feed/entry/content/properties/StatusMessage`– Zpráva podrobné informace o stavu (platí pouze pro konkrétní stavy).</span><span class="sxs-lookup"><span data-stu-id="2f4e0-352">`feed/entry/content/properties/StatusMessage` – Detailed status message (applies only to specific states).</span></span>
* <span data-ttu-id="2f4e0-353">`feed/entry/content/properties/Progress`– Sestavení průběh (%).</span><span class="sxs-lookup"><span data-stu-id="2f4e0-353">`feed/entry/content/properties/Progress` – Build progress (%).</span></span>
* <span data-ttu-id="2f4e0-354">`feed/entry/content/properties/StartTime`– Sestavení čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-354">`feed/entry/content/properties/StartTime` – Build start time.</span></span>
* <span data-ttu-id="2f4e0-355">`feed/entry/content/properties/EndTime`– Sestavení koncový čas.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-355">`feed/entry/content/properties/EndTime` – Build end time.</span></span>
* <span data-ttu-id="2f4e0-356">`feed/entry/content/properties/ExecutionTime`– Sestavení doba trvání.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-356">`feed/entry/content/properties/ExecutionTime` – Build duration.</span></span>
* <span data-ttu-id="2f4e0-357">`feed/entry/content/properties/ProgressStep`– Podrobnosti o aktuální fázi, který je v průběhu sestavení v.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-357">`feed/entry/content/properties/ProgressStep` – Details about the current stage that a build in progress is in.</span></span>

<span data-ttu-id="2f4e0-358">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-358">Valid build status:</span></span>

* <span data-ttu-id="2f4e0-359">Vytvořeno – položka sestavení žádost byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-359">Created – Build request entry was created.</span></span>
* <span data-ttu-id="2f4e0-360">Ve frontě – sestavení požadavku byla spuštěna a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-360">Queued – Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="2f4e0-361">Probíhá vytváření – sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-361">Building – Build is in process.</span></span>
* <span data-ttu-id="2f4e0-362">Úspěch – sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-362">Success – Build ended successfully.</span></span>
* <span data-ttu-id="2f4e0-363">Chyba – sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-363">Error – Build ended with a failure.</span></span>
* <span data-ttu-id="2f4e0-364">Došlo ke zrušení – sestavení bylo zrušeno.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-364">Canceled – Build was canceled.</span></span>
* <span data-ttu-id="2f4e0-365">Zrušení – sestavení probíhá její zrušení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-365">Canceling – Build is being canceled.</span></span>

<span data-ttu-id="2f4e0-366">Platné hodnoty pro typ sestavení:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-366">Valid values for build type:</span></span>

* <span data-ttu-id="2f4e0-367">Pořadí - pořadí sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-367">Rank - Rank build.</span></span> <span data-ttu-id="2f4e0-368">(Pro pořadí sestavení podrobnosti naleznete v dokumentu "Doporučení Machine Learning API dokumentace".)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-368">(For rank build details, please refer to the "Machine Learning Recommendation API documentation" document.)</span></span>
* <span data-ttu-id="2f4e0-369">Doporučení - doporučení sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-369">Recommendation - Recommendation build.</span></span>
* <span data-ttu-id="2f4e0-370">Společně fbt - často koupila sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-370">Fbt - Frequently bought together build.</span></span>

<span data-ttu-id="2f4e0-371">OData XML</span><span class="sxs-lookup"><span data-stu-id="2f4e0-371">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


### <a name="get-recommendations"></a><span data-ttu-id="2f4e0-372">Získejte doporučení</span><span class="sxs-lookup"><span data-stu-id="2f4e0-372">Get recommendations</span></span>
| <span data-ttu-id="2f4e0-373">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="2f4e0-373">HTTP Method</span></span> | <span data-ttu-id="2f4e0-374">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="2f4e0-374">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-375">GET</span><span class="sxs-lookup"><span data-stu-id="2f4e0-375">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f4e0-376">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-376">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="2f4e0-377">Název parametru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-377">Parameter Name</span></span> | <span data-ttu-id="2f4e0-378">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="2f4e0-378">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-379">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="2f4e0-379">modelId</span></span> |<span data-ttu-id="2f4e0-380">Jedinečný identifikátor modelu (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-380">Unique identifier of the model (case-sensitive)</span></span> |
| <span data-ttu-id="2f4e0-381">položky ItemID</span><span class="sxs-lookup"><span data-stu-id="2f4e0-381">itemIds</span></span> |<span data-ttu-id="2f4e0-382">Textový soubor s oddělovači seznam položek, doporučujeme pro.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-382">Comma-separated list of the items to recommend for.</span></span><br><span data-ttu-id="2f4e0-383">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="2f4e0-383">Max length: 1024</span></span> |
| <span data-ttu-id="2f4e0-384">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="2f4e0-384">numberOfResults</span></span> |<span data-ttu-id="2f4e0-385">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="2f4e0-385">Number of required results</span></span> |
| <span data-ttu-id="2f4e0-386">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="2f4e0-386">includeMetatadata</span></span> |<span data-ttu-id="2f4e0-387">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-387">Future use, always false</span></span> |
| <span data-ttu-id="2f4e0-388">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f4e0-388">apiVersion</span></span> |<span data-ttu-id="2f4e0-389">1.0</span><span class="sxs-lookup"><span data-stu-id="2f4e0-389">1.0</span></span> |

<span data-ttu-id="2f4e0-390">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="2f4e0-390">**Response:**</span></span>

<span data-ttu-id="2f4e0-391">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="2f4e0-391">HTTP Status code: 200</span></span>

<span data-ttu-id="2f4e0-392">Odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-392">The response includes one entry per recommended item.</span></span> <span data-ttu-id="2f4e0-393">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-393">Each entry has the following data:</span></span>

* <span data-ttu-id="2f4e0-394">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-394">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="2f4e0-395">`Feed\entry\content\properties\Name`-Název položky.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-395">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="2f4e0-396">`Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-396">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="2f4e0-397">`Feed\entry\content\properties\Reasoning`-Doporučení reasoning (například vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="2f4e0-397">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (for example, recommendation explanations).</span></span>

<span data-ttu-id="2f4e0-398">OData XML</span><span class="sxs-lookup"><span data-stu-id="2f4e0-398">OData XML</span></span>

<span data-ttu-id="2f4e0-399">Následující příklad odpověď obsahuje 10 doporučené položek:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-399">The example response below includes 10 recommended items:</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="update-model"></a><span data-ttu-id="2f4e0-400">Aktualizace modelu</span><span class="sxs-lookup"><span data-stu-id="2f4e0-400">Update model</span></span>
<span data-ttu-id="2f4e0-401">Můžete aktualizovat popis modelu nebo ID aktivního sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-401">You can update the model description or the active build ID.</span></span>
<span data-ttu-id="2f4e0-402">*ID aktivního sestavení* -každé sestavení pro každý model má sestavení ID.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-402">*Active build ID* - Every build for every model has a build ID.</span></span> <span data-ttu-id="2f4e0-403">ID aktivního sestavení je prvním úspěšném sestavení každý nový model.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-403">The active build ID is the first successful build of every new model.</span></span> <span data-ttu-id="2f4e0-404">Jakmile máte ID aktivního sestavení a proveďte další sestavení pro stejný model, je nutné explicitně nastavit jako výchozí ID sestavení Pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-404">Once you have an active build ID and you do additional builds for the same model, you need to explicitly set it as the default build ID if you want to.</span></span> <span data-ttu-id="2f4e0-405">Když budete používat doporučení, pokud nezadáte ID sestavení, které chcete použít, výchozí nastavení se použije automaticky.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-405">When you consume recommendations, if you do not specify the build ID that you want to use, the default one will be used automatically.</span></span>

<span data-ttu-id="2f4e0-406">Tento mechanismus umožňuje – až budete mít model doporučení v produkčním prostředí - pro vytvoření nových modelů a testování je před zvýšením úrovně je do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-406">This mechanism enables you - once you have a recommendation model in production - to build new models and test them before you promote them to production.</span></span>

| <span data-ttu-id="2f4e0-407">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="2f4e0-407">HTTP Method</span></span> | <span data-ttu-id="2f4e0-408">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="2f4e0-408">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-409">PUT</span><span class="sxs-lookup"><span data-stu-id="2f4e0-409">PUT</span></span> |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f4e0-410">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-410">Example:</span></span><br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f4e0-411">Název parametru</span><span class="sxs-lookup"><span data-stu-id="2f4e0-411">Parameter Name</span></span> | <span data-ttu-id="2f4e0-412">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="2f4e0-412">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f4e0-413">id</span><span class="sxs-lookup"><span data-stu-id="2f4e0-413">id</span></span> |<span data-ttu-id="2f4e0-414">Jedinečný identifikátor modelu (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="2f4e0-414">Unique identifier of the model (case-sensitive)</span></span> |
| <span data-ttu-id="2f4e0-415">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f4e0-415">apiVersion</span></span> |<span data-ttu-id="2f4e0-416">1.0</span><span class="sxs-lookup"><span data-stu-id="2f4e0-416">1.0</span></span> |
|  | |
| <span data-ttu-id="2f4e0-417">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="2f4e0-417">Request Body</span></span> |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br><span data-ttu-id="2f4e0-418">Všimněte si, že značky XML, popis a ActiveBuildId jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-418">Note that the XML tags Description and ActiveBuildId are optional.</span></span> <span data-ttu-id="2f4e0-419">Pokud nechcete nastavit popis nebo ActiveBuildId, odeberte celý značky.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-419">If you do not want to set Description or ActiveBuildId, remove the entire tag.</span></span> |

<span data-ttu-id="2f4e0-420">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="2f4e0-420">**Response**:</span></span>

<span data-ttu-id="2f4e0-421">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="2f4e0-421">HTTP Status code: 200</span></span>

<span data-ttu-id="2f4e0-422">OData XML</span><span class="sxs-lookup"><span data-stu-id="2f4e0-422">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Update an Existing Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T10:27:17Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

## <a name="legal"></a><span data-ttu-id="2f4e0-423">Právní informace</span><span class="sxs-lookup"><span data-stu-id="2f4e0-423">Legal</span></span>
<span data-ttu-id="2f4e0-424">Tento dokument je poskytován "jako-je".</span><span class="sxs-lookup"><span data-stu-id="2f4e0-424">This document is provided "as-is".</span></span> <span data-ttu-id="2f4e0-425">Informace a názory vyjádřené v tomto dokumentu včetně adres URL a dalších odkazů na internetové weby mohou změnit bez předchozího upozornění.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-425">Information and views expressed in this document, including URL and other Internet website references, may change without notice.</span></span> <span data-ttu-id="2f4e0-426">Některé příklady použité v ukázkách jsou jenom ilustrativní a smyšlené.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-426">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="2f4e0-427">Žádný skutečný vztah nebo připojení je určený nebo událostmi.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-427">No real association or connection is intended or should be inferred.</span></span> <span data-ttu-id="2f4e0-428">Tento dokument neposkytuje jste žádná zákonná práva duševního vlastnictví produktů společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-428">This document does not provide you with any legal rights to any intellectual property in any Microsoft product.</span></span> <span data-ttu-id="2f4e0-429">Můžete kopírovat a tento dokument použít pro interní referenční účely.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-429">You may copy and use this document for your internal, reference purposes.</span></span> <span data-ttu-id="2f4e0-430">© 2014 Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-430">© 2014 Microsoft.</span></span> <span data-ttu-id="2f4e0-431">Všechna práva vyhrazena.</span><span class="sxs-lookup"><span data-stu-id="2f4e0-431">All rights reserved.</span></span> 

