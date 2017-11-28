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
redirect_document_id: True
ms.openlocfilehash: d8f98e85f723a1104cb169b26d797934140979f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quick-start-guide-for-hello-machine-learning-recommendations-api-version-1"></a><span data-ttu-id="179df-104">Úvodní příručka pro hello Machine Learning doporučení API (verze 1)</span><span class="sxs-lookup"><span data-stu-id="179df-104">Quick start guide for hello Machine Learning Recommendations API (version 1)</span></span>

> [!NOTE]
> <span data-ttu-id="179df-105">Měli byste začít používat hello [kognitivní služby API doporučení](https://www.microsoft.com/cognitive-services/recommendations-api) místo tuto verzi.</span><span class="sxs-lookup"><span data-stu-id="179df-105">You should start using hello [Recommendations API Cognitive Service](https://www.microsoft.com/cognitive-services/recommendations-api) instead of this version.</span></span> <span data-ttu-id="179df-106">Hello kognitivní službu doporučení budou nahrazení této služby, a všechny nové funkce hello bude vyvinutý existuje.</span><span class="sxs-lookup"><span data-stu-id="179df-106">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="179df-107">Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.</span><span class="sxs-lookup"><span data-stu-id="179df-107">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
>
> <span data-ttu-id="179df-108">Další informace o [toohello migrace nové kognitivní služby](http://aka.ms/recomigrate).</span><span class="sxs-lookup"><span data-stu-id="179df-108">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate).</span></span>
> 
> 

<span data-ttu-id="179df-109">Tento dokument popisuje, jak tooonboard služby nebo aplikace toouse Microsoft Azure Machine Learning doporučení.</span><span class="sxs-lookup"><span data-stu-id="179df-109">This document describes how tooonboard your service or application toouse Microsoft Azure Machine Learning Recommendations.</span></span> <span data-ttu-id="179df-110">Další informace naleznete na hello API doporučení v hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).</span><span class="sxs-lookup"><span data-stu-id="179df-110">You can find more details on hello Recommendations API in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="general-overview"></a><span data-ttu-id="179df-111">Obecné – přehled</span><span class="sxs-lookup"><span data-stu-id="179df-111">General overview</span></span>
<span data-ttu-id="179df-112">toouse Azure Machine Learning doporučení, je třeba tootake hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="179df-112">toouse Azure Machine Learning Recommendations, you need tootake hello following steps:</span></span>

* <span data-ttu-id="179df-113">Vytvoření modelu – model je kontejner, data o využití, data katalogu a hello doporučení modelu.</span><span class="sxs-lookup"><span data-stu-id="179df-113">Create a model - A model is a container of your usage data, catalog data, and hello recommendation model.</span></span>
* <span data-ttu-id="179df-114">Data katalogu import - katalog obsahuje informace o metadatech na hello položky.</span><span class="sxs-lookup"><span data-stu-id="179df-114">Import catalog data - A catalog contains metadata information on hello items.</span></span> 
* <span data-ttu-id="179df-115">Data o využití import - data o využití je možné uložit v jedné ze dvou způsobů (nebo obě):</span><span class="sxs-lookup"><span data-stu-id="179df-115">Import usage data - Usage data can be uploaded in one of two ways (or both):</span></span>
  * <span data-ttu-id="179df-116">Tím, že nahrajete soubor, který obsahuje data o využití hello.</span><span class="sxs-lookup"><span data-stu-id="179df-116">By uploading a file that contains hello usage data.</span></span>
  * <span data-ttu-id="179df-117">Odesláním dat pořízení události.</span><span class="sxs-lookup"><span data-stu-id="179df-117">By sending data acquisition events.</span></span>
    <span data-ttu-id="179df-118">Obvykle nahrát soubor využití v pořadí toobe možné toocreate model počáteční doporučení (zavedení) a ho používat, dokud systém hello shromažďuje dostatek dat pomocí formátu získávání dat hello.</span><span class="sxs-lookup"><span data-stu-id="179df-118">Usually you upload a usage file in order toobe able toocreate an initial recommendation model (bootstrap) and use it until hello system gathers enough data by using hello data acquisition format.</span></span>
* <span data-ttu-id="179df-119">Vytvoření modelu doporučení – to je asynchronní operace, ve které hello doporučení systému trvá všechna data o využití hello a vytvoří model doporučení.</span><span class="sxs-lookup"><span data-stu-id="179df-119">Build a recommendation model - This is an asynchronous operation in which hello recommendation system takes all hello usage data and creates a recommendation model.</span></span> <span data-ttu-id="179df-120">Tato operace může trvat několik minut nebo několik hodin v závislosti na hello velikost dat hello a hello sestavení parametry konfigurace.</span><span class="sxs-lookup"><span data-stu-id="179df-120">This operation can take several minutes or several hours depending on hello size of hello data and hello build configuration parameters.</span></span> <span data-ttu-id="179df-121">Při spouštění hello sestavení, zobrazí se ID sestavení</span><span class="sxs-lookup"><span data-stu-id="179df-121">When triggering hello build, you will get a build ID.</span></span> <span data-ttu-id="179df-122">Používejte toocheck při ukončení procesu sestavení hello před zahájením tooconsume doporučení.</span><span class="sxs-lookup"><span data-stu-id="179df-122">Use it toocheck when hello build process has ended before starting tooconsume recommendations.</span></span>
* <span data-ttu-id="179df-123">Využívat doporučení - Get doporučení pro určitou položku nebo seznam položek.</span><span class="sxs-lookup"><span data-stu-id="179df-123">Consume recommendations - Get recommendations for a specific item or list of items.</span></span>

<span data-ttu-id="179df-124">Všechny výše uvedené kroky hello se provádí prostřednictvím hello rozhraní API služby Azure Machine Learning doporučení.</span><span class="sxs-lookup"><span data-stu-id="179df-124">All hello steps above are done through hello Azure Machine Learning Recommendations API.</span></span>  <span data-ttu-id="179df-125">Si můžete stáhnout ukázkovou aplikaci, která implementuje každý z těchto kroků z hello [také galerie.](http://1drv.ms/1xeO2F3)</span><span class="sxs-lookup"><span data-stu-id="179df-125">You can download a sample application that implements each of these steps from hello [gallery as well.](http://1drv.ms/1xeO2F3)</span></span>

## <a name="limitations"></a><span data-ttu-id="179df-126">Omezení</span><span class="sxs-lookup"><span data-stu-id="179df-126">Limitations</span></span>
* <span data-ttu-id="179df-127">maximální počet modelů jedno předplatné Hello je 10.</span><span class="sxs-lookup"><span data-stu-id="179df-127">hello maximum number of models per subscription is 10.</span></span>
* <span data-ttu-id="179df-128">maximální počet položek, které mohou být uloženy katalog Hello je 100 000.</span><span class="sxs-lookup"><span data-stu-id="179df-128">hello maximum number of items that a catalog can hold is 100,000.</span></span>
* <span data-ttu-id="179df-129">maximální počet bodů využití, které jsou zachovány Hello je ~ 5 000 000.</span><span class="sxs-lookup"><span data-stu-id="179df-129">hello maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="179df-130">Pokud nové se nahrál nebo nahlásí se odstraní nejstarší Hello.</span><span class="sxs-lookup"><span data-stu-id="179df-130">hello oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="179df-131">maximální velikost dat, který může odeslat v BLOGU (například import katalogu dat, data o využití import) Hello je 200MB.</span><span class="sxs-lookup"><span data-stu-id="179df-131">hello maximum size of data that can be sent in POST (for example, import catalog data, import usage data) is 200MB.</span></span>
* <span data-ttu-id="179df-132">Hello počet transakcí za sekundu pro sestavení modelu doporučení, která není aktivní, je ~ 2TPS.</span><span class="sxs-lookup"><span data-stu-id="179df-132">hello number of transactions per second for a recommendation model build that is not active is ~2TPS.</span></span> <span data-ttu-id="179df-133">Až too20TPS mohou být uloženy sestavení modelu doporučení, která je aktivní.</span><span class="sxs-lookup"><span data-stu-id="179df-133">A recommendation model build that is active can hold up too20TPS.</span></span>

## <a name="integration"></a><span data-ttu-id="179df-134">Integrace</span><span class="sxs-lookup"><span data-stu-id="179df-134">Integration</span></span>
### <a name="authentication"></a><span data-ttu-id="179df-135">Authentication</span><span class="sxs-lookup"><span data-stu-id="179df-135">Authentication</span></span>
<span data-ttu-id="179df-136">Microsoft Azure Marketplace podporuje buď hello metodu ověřování Basic nebo OAuth.</span><span class="sxs-lookup"><span data-stu-id="179df-136">Microsoft Azure Marketplace supports either hello Basic or OAuth authentication method.</span></span> <span data-ttu-id="179df-137">Budete moci snadno najít hello klíče účtu tak, že přejdete toohello klíče v hello marketplace v oddíle [nastavení svého účtu](https://datamarket.azure.com/account/keys).</span><span class="sxs-lookup"><span data-stu-id="179df-137">You can easily find hello account keys by navigating toohello keys in hello marketplace under [your account settings](https://datamarket.azure.com/account/keys).</span></span> 

#### <a name="basic-authentication"></a><span data-ttu-id="179df-138">Základní ověřování</span><span class="sxs-lookup"><span data-stu-id="179df-138">Basic Authentication</span></span>
<span data-ttu-id="179df-139">Přidáte hlavičku autorizace hello:</span><span class="sxs-lookup"><span data-stu-id="179df-139">Add hello Authorization header:</span></span>

    Authorization: Basic <creds>

    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  

<span data-ttu-id="179df-140">Převést tooBase64 (C#)</span><span class="sxs-lookup"><span data-stu-id="179df-140">Convert tooBase64 (C#)</span></span>

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);

<span data-ttu-id="179df-141">Převést tooBase64 (JavaScript)</span><span class="sxs-lookup"><span data-stu-id="179df-141">Convert tooBase64 (JavaScript)</span></span>

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);




### <a name="service-uri"></a><span data-ttu-id="179df-142">URI služby</span><span class="sxs-lookup"><span data-stu-id="179df-142">Service URI</span></span>
<span data-ttu-id="179df-143">Hello služby kořenová identifikátor URI pro hello rozhraní API služby Azure Machine Learning doporučení je [sem.](https://api.datamarket.azure.com/amla/recommendations/v2/)</span><span class="sxs-lookup"><span data-stu-id="179df-143">hello service root URI for hello Azure Machine Learning Recommendations APIs is [here.](https://api.datamarket.azure.com/amla/recommendations/v2/)</span></span>

<span data-ttu-id="179df-144">úplné URI služby Hello je vyjádřit pomocí elementů hello specifikace prostředí OData.</span><span class="sxs-lookup"><span data-stu-id="179df-144">hello full service URI is expressed using elements of hello OData specification.</span></span>

### <a name="api-version"></a><span data-ttu-id="179df-145">Verze rozhraní API</span><span class="sxs-lookup"><span data-stu-id="179df-145">API version</span></span>
<span data-ttu-id="179df-146">Každé volání rozhraní API bude mít, na konci hello parametr dotazu s názvem apiVersion, který by mělo být nastavené příliš "1.0".</span><span class="sxs-lookup"><span data-stu-id="179df-146">Each API call will have, at hello end, a query parameter called apiVersion that should be set too"1.0".</span></span>

### <a name="ids-are-case-sensitive"></a><span data-ttu-id="179df-147">ID jsou malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="179df-147">IDs are case-sensitive</span></span>
<span data-ttu-id="179df-148">ID, podle těchto hello rozhraní API, vrátil malých a velkých písmen a by měl být použit jako takový, při předány jako parametry při následných voláních rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="179df-148">IDs, returned by any of hello APIs, are case-sensitive and should be used as such when passed as parameters in subsequent API calls.</span></span> <span data-ttu-id="179df-149">ID modelu a ID katalogu pro instanci, jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="179df-149">For instance, model IDs and catalog IDs are case-sensitive.</span></span>

### <a name="create-a-model"></a><span data-ttu-id="179df-150">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="179df-150">Create a model</span></span>
<span data-ttu-id="179df-151">Vytváří se žádost o "Vytvoření modelu":</span><span class="sxs-lookup"><span data-stu-id="179df-151">Creating a "create model" request:</span></span>

| <span data-ttu-id="179df-152">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="179df-152">HTTP Method</span></span> | <span data-ttu-id="179df-153">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="179df-153">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-154">POST</span><span class="sxs-lookup"><span data-stu-id="179df-154">POST</span></span> |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="179df-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="179df-155">Example:</span></span><br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| <span data-ttu-id="179df-156">Název parametru</span><span class="sxs-lookup"><span data-stu-id="179df-156">Parameter Name</span></span> | <span data-ttu-id="179df-157">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="179df-157">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-158">%{ModelName/</span><span class="sxs-lookup"><span data-stu-id="179df-158">modelName</span></span> |<span data-ttu-id="179df-159">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="179df-159">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="179df-160">Maximální délka: 20</span><span class="sxs-lookup"><span data-stu-id="179df-160">Max length: 20</span></span> |
| <span data-ttu-id="179df-161">apiVersion</span><span class="sxs-lookup"><span data-stu-id="179df-161">apiVersion</span></span> |<span data-ttu-id="179df-162">1.0</span><span class="sxs-lookup"><span data-stu-id="179df-162">1.0</span></span> |
|  | |
| <span data-ttu-id="179df-163">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="179df-163">Request Body</span></span> |<span data-ttu-id="179df-164">NONE</span><span class="sxs-lookup"><span data-stu-id="179df-164">NONE</span></span> |

<span data-ttu-id="179df-165">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="179df-165">**Response**:</span></span>

<span data-ttu-id="179df-166">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="179df-166">HTTP Status code: 200</span></span>

* <span data-ttu-id="179df-167">`feed/entry/content/properties/id`-Obsahuje ID hello modelu.</span><span class="sxs-lookup"><span data-stu-id="179df-167">`feed/entry/content/properties/id` - Contains hello model ID.</span></span>
  <span data-ttu-id="179df-168">Všimněte si, že hello ID modelu je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="179df-168">Note that hello model ID is case-sensitive.</span></span>

<span data-ttu-id="179df-169">OData XML</span><span class="sxs-lookup"><span data-stu-id="179df-169">OData XML</span></span>

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


### <a name="import-catalog-data"></a><span data-ttu-id="179df-170">Umožňuje importovat catalog data</span><span class="sxs-lookup"><span data-stu-id="179df-170">Import catalog data</span></span>
<span data-ttu-id="179df-171">Pokud nahrajete několik katalogu soubory toohello stejný model s několika volání, jsme vloží pouze hello nové položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="179df-171">If you upload several catalog files toohello same model with several calls, we will insert only hello new catalog items.</span></span> <span data-ttu-id="179df-172">Existující položky zůstane s hello původní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="179df-172">Existing items will remain with hello original values.</span></span>

| <span data-ttu-id="179df-173">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="179df-173">HTTP Method</span></span> | <span data-ttu-id="179df-174">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="179df-174">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-175">POST</span><span class="sxs-lookup"><span data-stu-id="179df-175">POST</span></span> |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="179df-176">Příklad:</span><span class="sxs-lookup"><span data-stu-id="179df-176">Example:</span></span><br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="179df-177">Název parametru</span><span class="sxs-lookup"><span data-stu-id="179df-177">Parameter Name</span></span> | <span data-ttu-id="179df-178">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="179df-178">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-179">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="179df-179">modelId</span></span> |<span data-ttu-id="179df-180">Jedinečný identifikátor modelu hello (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="179df-180">Unique identifier of hello model (case-sensitive)</span></span> |
| <span data-ttu-id="179df-181">Název souboru</span><span class="sxs-lookup"><span data-stu-id="179df-181">filename</span></span> |<span data-ttu-id="179df-182">Textové identifikátor hello katalogu.</span><span class="sxs-lookup"><span data-stu-id="179df-182">Textual identifier of hello catalog.</span></span><br><span data-ttu-id="179df-183">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="179df-183">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="179df-184">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="179df-184">Max length: 50</span></span> |
| <span data-ttu-id="179df-185">apiVersion</span><span class="sxs-lookup"><span data-stu-id="179df-185">apiVersion</span></span> |<span data-ttu-id="179df-186">1.0</span><span class="sxs-lookup"><span data-stu-id="179df-186">1.0</span></span> |
|  | |
| <span data-ttu-id="179df-187">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="179df-187">Request Body</span></span> |<span data-ttu-id="179df-188">Data katalogu.</span><span class="sxs-lookup"><span data-stu-id="179df-188">Catalog data.</span></span> <span data-ttu-id="179df-189">Formát:</span><span class="sxs-lookup"><span data-stu-id="179df-189">Format:</span></span><br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th><span data-ttu-id="179df-190">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="179df-190">Name</span></span></th><th><span data-ttu-id="179df-191">Povinné</span><span class="sxs-lookup"><span data-stu-id="179df-191">Mandatory</span></span></th><th><span data-ttu-id="179df-192">Typ</span><span class="sxs-lookup"><span data-stu-id="179df-192">Type</span></span></th><th><span data-ttu-id="179df-193">Popis</span><span class="sxs-lookup"><span data-stu-id="179df-193">Description</span></span></th></tr><tr><td><span data-ttu-id="179df-194">Id položky</span><span class="sxs-lookup"><span data-stu-id="179df-194">Item Id</span></span></td><td><span data-ttu-id="179df-195">Ano</span><span class="sxs-lookup"><span data-stu-id="179df-195">Yes</span></span></td><td><span data-ttu-id="179df-196">Alfanumerické znaky, maximální délka 50</span><span class="sxs-lookup"><span data-stu-id="179df-196">Alphanumeric, max length 50</span></span></td><td><span data-ttu-id="179df-197">Jedinečný identifikátor položky</span><span class="sxs-lookup"><span data-stu-id="179df-197">Unique identifier of an item</span></span></td></tr><tr><td><span data-ttu-id="179df-198">Název položky</span><span class="sxs-lookup"><span data-stu-id="179df-198">Item Name</span></span></td><td><span data-ttu-id="179df-199">Ano</span><span class="sxs-lookup"><span data-stu-id="179df-199">Yes</span></span></td><td><span data-ttu-id="179df-200">Alfanumerické znaky, maximální délka 255</span><span class="sxs-lookup"><span data-stu-id="179df-200">Alphanumeric, max length 255</span></span></td><td><span data-ttu-id="179df-201">Název položky</span><span class="sxs-lookup"><span data-stu-id="179df-201">Item name</span></span></td></tr><tr><td><span data-ttu-id="179df-202">Kategorie položky</span><span class="sxs-lookup"><span data-stu-id="179df-202">Item Category</span></span></td><td><span data-ttu-id="179df-203">Ano</span><span class="sxs-lookup"><span data-stu-id="179df-203">Yes</span></span></td><td><span data-ttu-id="179df-204">Alfanumerické znaky, maximální délka 255</span><span class="sxs-lookup"><span data-stu-id="179df-204">Alphanumeric, max length 255</span></span></td><td><span data-ttu-id="179df-205">Toowhich kategorie, které tato položka patří (například vaření knihy, obrázkům...)</span><span class="sxs-lookup"><span data-stu-id="179df-205">Category toowhich this item belongs (for example, Cooking Books, Drama…)</span></span></td></tr><tr><td><span data-ttu-id="179df-206">Popis</span><span class="sxs-lookup"><span data-stu-id="179df-206">Description</span></span></td><td><span data-ttu-id="179df-207">Ne</span><span class="sxs-lookup"><span data-stu-id="179df-207">No</span></span></td><td><span data-ttu-id="179df-208">Alfanumerické znaky, maximální délka je 4000</span><span class="sxs-lookup"><span data-stu-id="179df-208">Alphanumeric, max length 4000</span></span></td><td><span data-ttu-id="179df-209">Popis této položky</span><span class="sxs-lookup"><span data-stu-id="179df-209">Description of this item</span></span></td></tr></table><br><span data-ttu-id="179df-210">Maximální velikost souboru je 200MB.</span><span class="sxs-lookup"><span data-stu-id="179df-210">Maximum file size is 200MB.</span></span><br><br><span data-ttu-id="179df-211">Příklad:</span><span class="sxs-lookup"><span data-stu-id="179df-211">Example:</span></span><br><code>2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book<br>21bf8088-b6c0-4509-870c-e1c7ac78304a,hello Forgetting Room: A Fiction (Byzantium Book),Book<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book<br>552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book</code> |

<span data-ttu-id="179df-212">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="179df-212">**Response**:</span></span>

<span data-ttu-id="179df-213">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="179df-213">HTTP Status code: 200</span></span>

* <span data-ttu-id="179df-214">`Feed\entry\content\properties\LineCount`-Přijaté počet řádků.</span><span class="sxs-lookup"><span data-stu-id="179df-214">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="179df-215">`Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby tooan.</span><span class="sxs-lookup"><span data-stu-id="179df-215">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due tooan error.</span></span>

<span data-ttu-id="179df-216">OData XML</span><span class="sxs-lookup"><span data-stu-id="179df-216">OData XML</span></span>

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


### <a name="import-usage-data"></a><span data-ttu-id="179df-217">Importovat data o využití</span><span class="sxs-lookup"><span data-stu-id="179df-217">Import usage data</span></span>
#### <a name="uploading-a-file"></a><span data-ttu-id="179df-218">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="179df-218">Uploading a file</span></span>
<span data-ttu-id="179df-219">Tato část uvádí, jak data o využití tooupload pomocí souboru.</span><span class="sxs-lookup"><span data-stu-id="179df-219">This section shows how tooupload usage data by using a file.</span></span> <span data-ttu-id="179df-220">Můžete volat toto rozhraní API se data o využití.</span><span class="sxs-lookup"><span data-stu-id="179df-220">You can call this API several times with usage data.</span></span> <span data-ttu-id="179df-221">Všechna data o využití se uloží pro všechna volání.</span><span class="sxs-lookup"><span data-stu-id="179df-221">All usage data will be saved for all calls.</span></span>

| <span data-ttu-id="179df-222">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="179df-222">HTTP Method</span></span> | <span data-ttu-id="179df-223">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="179df-223">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-224">POST</span><span class="sxs-lookup"><span data-stu-id="179df-224">POST</span></span> |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="179df-225">Příklad:</span><span class="sxs-lookup"><span data-stu-id="179df-225">Example:</span></span><br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="179df-226">Název parametru</span><span class="sxs-lookup"><span data-stu-id="179df-226">Parameter Name</span></span> | <span data-ttu-id="179df-227">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="179df-227">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-228">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="179df-228">modelId</span></span> |<span data-ttu-id="179df-229">Jedinečný identifikátor modelu hello (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="179df-229">Unique identifier of hello model (case-sensitive)</span></span> |
| <span data-ttu-id="179df-230">Název souboru</span><span class="sxs-lookup"><span data-stu-id="179df-230">filename</span></span> |<span data-ttu-id="179df-231">Textové identifikátor hello katalogu.</span><span class="sxs-lookup"><span data-stu-id="179df-231">Textual identifier of hello catalog.</span></span><br><span data-ttu-id="179df-232">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="179df-232">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="179df-233">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="179df-233">Max length: 50</span></span> |
| <span data-ttu-id="179df-234">apiVersion</span><span class="sxs-lookup"><span data-stu-id="179df-234">apiVersion</span></span> |<span data-ttu-id="179df-235">1.0</span><span class="sxs-lookup"><span data-stu-id="179df-235">1.0</span></span> |
|  | |
| <span data-ttu-id="179df-236">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="179df-236">Request Body</span></span> |<span data-ttu-id="179df-237">Data o využití.</span><span class="sxs-lookup"><span data-stu-id="179df-237">Usage data.</span></span> <span data-ttu-id="179df-238">Formát:</span><span class="sxs-lookup"><span data-stu-id="179df-238">Format:</span></span><br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th><span data-ttu-id="179df-239">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="179df-239">Name</span></span></th><th><span data-ttu-id="179df-240">Povinné</span><span class="sxs-lookup"><span data-stu-id="179df-240">Mandatory</span></span></th><th><span data-ttu-id="179df-241">Typ</span><span class="sxs-lookup"><span data-stu-id="179df-241">Type</span></span></th><th><span data-ttu-id="179df-242">Popis</span><span class="sxs-lookup"><span data-stu-id="179df-242">Description</span></span></th></tr><tr><td><span data-ttu-id="179df-243">Id uživatele</span><span class="sxs-lookup"><span data-stu-id="179df-243">User Id</span></span></td><td><span data-ttu-id="179df-244">Ano</span><span class="sxs-lookup"><span data-stu-id="179df-244">Yes</span></span></td><td><span data-ttu-id="179df-245">Alfanumerické</span><span class="sxs-lookup"><span data-stu-id="179df-245">Alphanumeric</span></span></td><td><span data-ttu-id="179df-246">Jedinečný identifikátor uživatele</span><span class="sxs-lookup"><span data-stu-id="179df-246">Unique identifier of a user</span></span></td></tr><tr><td><span data-ttu-id="179df-247">Id položky</span><span class="sxs-lookup"><span data-stu-id="179df-247">Item Id</span></span></td><td><span data-ttu-id="179df-248">Ano</span><span class="sxs-lookup"><span data-stu-id="179df-248">Yes</span></span></td><td><span data-ttu-id="179df-249">Alfanumerické znaky, maximální délka 50</span><span class="sxs-lookup"><span data-stu-id="179df-249">Alphanumeric, max length 50</span></span></td><td><span data-ttu-id="179df-250">Jedinečný identifikátor položky</span><span class="sxs-lookup"><span data-stu-id="179df-250">Unique identifier of an item</span></span></td></tr><tr><td><span data-ttu-id="179df-251">Čas</span><span class="sxs-lookup"><span data-stu-id="179df-251">Time</span></span></td><td><span data-ttu-id="179df-252">Ne</span><span class="sxs-lookup"><span data-stu-id="179df-252">No</span></span></td><td><span data-ttu-id="179df-253">Datum ve formátu: rrrr/MM/ddTHH (například 2013/06/20T10:00:00)</span><span class="sxs-lookup"><span data-stu-id="179df-253">Date in format: YYYY/MM/DDTHH:MM:SS (for example, 2013/06/20T10:00:00)</span></span></td><td><span data-ttu-id="179df-254">Čas dat</span><span class="sxs-lookup"><span data-stu-id="179df-254">Time of data</span></span></td></tr><tr><td><span data-ttu-id="179df-255">Událost</span><span class="sxs-lookup"><span data-stu-id="179df-255">Event</span></span></td><td><span data-ttu-id="179df-256">Ne, pokud zadaný musí taky datum</span><span class="sxs-lookup"><span data-stu-id="179df-256">No, if supplied then must also put date</span></span></td><td><span data-ttu-id="179df-257">Jedna z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="179df-257">One of hello following:</span></span><br><span data-ttu-id="179df-258">• Klikněte na tlačítko</span><span class="sxs-lookup"><span data-stu-id="179df-258">• Click</span></span><br><span data-ttu-id="179df-259">• RecommendationClick</span><span class="sxs-lookup"><span data-stu-id="179df-259">• RecommendationClick</span></span><br><span data-ttu-id="179df-260">• AddShopCart</span><span class="sxs-lookup"><span data-stu-id="179df-260">•    AddShopCart</span></span><br><span data-ttu-id="179df-261">• RemoveShopCart</span><span class="sxs-lookup"><span data-stu-id="179df-261">• RemoveShopCart</span></span><br><span data-ttu-id="179df-262">• Nákupu</span><span class="sxs-lookup"><span data-stu-id="179df-262">• Purchase</span></span></td><td></td></tr></table><br><span data-ttu-id="179df-263">Maximální velikost souboru je 200MB.</span><span class="sxs-lookup"><span data-stu-id="179df-263">Maximum file size is 200MB.</span></span><br><br><span data-ttu-id="179df-264">Příklad:</span><span class="sxs-lookup"><span data-stu-id="179df-264">Example:</span></span><br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

<span data-ttu-id="179df-265">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="179df-265">**Response**:</span></span>

<span data-ttu-id="179df-266">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="179df-266">HTTP Status code: 200</span></span>

* <span data-ttu-id="179df-267">`Feed\entry\content\properties\LineCount`-Přijaté počet řádků.</span><span class="sxs-lookup"><span data-stu-id="179df-267">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="179df-268">`Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby tooan.</span><span class="sxs-lookup"><span data-stu-id="179df-268">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due tooan error.</span></span>
* <span data-ttu-id="179df-269">`Feed\entry\content\properties\FileId`-Souboru identifikátor.</span><span class="sxs-lookup"><span data-stu-id="179df-269">`Feed\entry\content\properties\FileId` - File identifier.</span></span>

<span data-ttu-id="179df-270">OData XML</span><span class="sxs-lookup"><span data-stu-id="179df-270">OData XML</span></span>

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


#### <a name="using-data-acquisition"></a><span data-ttu-id="179df-271">Pomocí získávání dat</span><span class="sxs-lookup"><span data-stu-id="179df-271">Using data acquisition</span></span>
<span data-ttu-id="179df-272">Tato část uvádí, jak toosend událostí v reálném čas tooAzure Machine Learning doporučení, obvykle z vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="179df-272">This section shows how toosend events in real time tooAzure Machine Learning Recommendations, usually from your website.</span></span>

| <span data-ttu-id="179df-273">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="179df-273">HTTP Method</span></span> | <span data-ttu-id="179df-274">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="179df-274">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-275">POST</span><span class="sxs-lookup"><span data-stu-id="179df-275">POST</span></span> |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="179df-276">Název parametru</span><span class="sxs-lookup"><span data-stu-id="179df-276">Parameter Name</span></span> | <span data-ttu-id="179df-277">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="179df-277">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-278">apiVersion</span><span class="sxs-lookup"><span data-stu-id="179df-278">apiVersion</span></span> |<span data-ttu-id="179df-279">1.0</span><span class="sxs-lookup"><span data-stu-id="179df-279">1.0</span></span> |
|  | |
| <span data-ttu-id="179df-280">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="179df-280">Request body</span></span> |<span data-ttu-id="179df-281">Vkládání dat události pro všechny události chcete toosend.</span><span class="sxs-lookup"><span data-stu-id="179df-281">Event data entry for each event you want toosend.</span></span> <span data-ttu-id="179df-282">By měli poslat pro hello stejné relace uživatele nebo prohlížeče hello stejným ID v poli SessionId hello.</span><span class="sxs-lookup"><span data-stu-id="179df-282">You should send for hello same user or browser session hello same ID in hello SessionId field.</span></span> <span data-ttu-id="179df-283">(Viz ukázka textu události, které jsou níže).</span><span class="sxs-lookup"><span data-stu-id="179df-283">(See sample of event body below.)</span></span> |

* <span data-ttu-id="179df-284">Příklad pro události, klikněte na tlačítko'.</span><span class="sxs-lookup"><span data-stu-id="179df-284">Example for event 'Click':</span></span>
  
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
* <span data-ttu-id="179df-285">Příklad pro událost 'RecommendationClick':</span><span class="sxs-lookup"><span data-stu-id="179df-285">Example for event 'RecommendationClick':</span></span>
  
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
* <span data-ttu-id="179df-286">Příklad pro událost 'AddShopCart':</span><span class="sxs-lookup"><span data-stu-id="179df-286">Example for event 'AddShopCart':</span></span>
  
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
* <span data-ttu-id="179df-287">Příklad pro událost 'RemoveShopCart':</span><span class="sxs-lookup"><span data-stu-id="179df-287">Example for event 'RemoveShopCart':</span></span>
  
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
* <span data-ttu-id="179df-288">Příklad pro události, nákupu'.</span><span class="sxs-lookup"><span data-stu-id="179df-288">Example for event 'Purchase':</span></span>
  
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
* <span data-ttu-id="179df-289">Příklad odesílání 2 události, klikněte na' a 'AddShopCart':</span><span class="sxs-lookup"><span data-stu-id="179df-289">Example sending 2 events, 'Click' and 'AddShopCart':</span></span>
  
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

<span data-ttu-id="179df-290">**Odpověď**: kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="179df-290">**Response**: HTTP Status code: 200</span></span>

### <a name="build-a-recommendation-model"></a><span data-ttu-id="179df-291">Vytvoření modelu doporučení</span><span class="sxs-lookup"><span data-stu-id="179df-291">Build a recommendation model</span></span>
| <span data-ttu-id="179df-292">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="179df-292">HTTP Method</span></span> | <span data-ttu-id="179df-293">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="179df-293">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-294">POST</span><span class="sxs-lookup"><span data-stu-id="179df-294">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="179df-295">Příklad:</span><span class="sxs-lookup"><span data-stu-id="179df-295">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |

| <span data-ttu-id="179df-296">Název parametru</span><span class="sxs-lookup"><span data-stu-id="179df-296">Parameter Name</span></span> | <span data-ttu-id="179df-297">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="179df-297">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-298">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="179df-298">modelId</span></span> |<span data-ttu-id="179df-299">Jedinečný identifikátor modelu hello (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="179df-299">Unique identifier of hello model (case-sensitive)</span></span> |
| <span data-ttu-id="179df-300">userDescription</span><span class="sxs-lookup"><span data-stu-id="179df-300">userDescription</span></span> |<span data-ttu-id="179df-301">Textové identifikátor hello katalogu.</span><span class="sxs-lookup"><span data-stu-id="179df-301">Textual identifier of hello catalog.</span></span> <span data-ttu-id="179df-302">Poznámka: Pokud použijete prostory musí zakódovat ho s % 20 místo.</span><span class="sxs-lookup"><span data-stu-id="179df-302">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="179df-303">Najdete v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="179df-303">See example above.</span></span><br><span data-ttu-id="179df-304">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="179df-304">Max length: 50</span></span> |
| <span data-ttu-id="179df-305">apiVersion</span><span class="sxs-lookup"><span data-stu-id="179df-305">apiVersion</span></span> |<span data-ttu-id="179df-306">1.0</span><span class="sxs-lookup"><span data-stu-id="179df-306">1.0</span></span> |
|  | |
| <span data-ttu-id="179df-307">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="179df-307">Request Body</span></span> |<span data-ttu-id="179df-308">NONE</span><span class="sxs-lookup"><span data-stu-id="179df-308">NONE</span></span> |

<span data-ttu-id="179df-309">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="179df-309">**Response**:</span></span>

<span data-ttu-id="179df-310">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="179df-310">HTTP Status code: 200</span></span>

<span data-ttu-id="179df-311">Toto je asynchronní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="179df-311">This is an asynchronous API.</span></span> <span data-ttu-id="179df-312">Zobrazí se ID sestavení jako odpověď.</span><span class="sxs-lookup"><span data-stu-id="179df-312">You will get a build ID as a response.</span></span> <span data-ttu-id="179df-313">tooknow při sestavení hello skončila, měli byste volání rozhraní API "Získat sestavení stavu systému Model" hello a vyhledejte ID toto sestavení v odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="179df-313">tooknow when hello build has ended, you should call hello "Get Builds Status of a Model" API and locate this build ID in hello response.</span></span> <span data-ttu-id="179df-314">Všimněte si, že sestavení může trvat od toohours minut v závislosti na velikosti hello dat hello.</span><span class="sxs-lookup"><span data-stu-id="179df-314">Note that a build can take from minutes toohours depending on hello size of hello data.</span></span>

<span data-ttu-id="179df-315">Doporučení nelze používat, dokud hello sestavení elementy end.</span><span class="sxs-lookup"><span data-stu-id="179df-315">You cannot consume recommendations till hello build ends.</span></span>

<span data-ttu-id="179df-316">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="179df-316">Valid build status:</span></span>

* <span data-ttu-id="179df-317">Vytvoření – Model byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="179df-317">Create – Model was created.</span></span>
* <span data-ttu-id="179df-318">Ve frontě – sestavení modelu byla spuštěna a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="179df-318">Queued – Model build was triggered and it is queued.</span></span>
* <span data-ttu-id="179df-319">Sestavení – Model sestavuje.</span><span class="sxs-lookup"><span data-stu-id="179df-319">Building – Model is being built.</span></span>
* <span data-ttu-id="179df-320">Úspěch – sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="179df-320">Success – Build ended successfully.</span></span>
* <span data-ttu-id="179df-321">Chyba – sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="179df-321">Error – Build ended with a failure.</span></span>
* <span data-ttu-id="179df-322">Došlo ke zrušení – sestavení bylo zrušeno.</span><span class="sxs-lookup"><span data-stu-id="179df-322">Canceled – Build was canceled.</span></span>
* <span data-ttu-id="179df-323">Zrušení – sestavení probíhá její zrušení.</span><span class="sxs-lookup"><span data-stu-id="179df-323">Canceling – Build is being canceled.</span></span>

<span data-ttu-id="179df-324">Všimněte si, že hello sestavení, který ID naleznete v části hello následující cesty:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="179df-324">Note that hello build ID can be found under hello following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="179df-325">OData XML</span><span class="sxs-lookup"><span data-stu-id="179df-325">OData XML</span></span>

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

### <a name="get-build-status-of-a-model"></a><span data-ttu-id="179df-326">Získat stav sestavení modelu</span><span class="sxs-lookup"><span data-stu-id="179df-326">Get build status of a model</span></span>
| <span data-ttu-id="179df-327">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="179df-327">HTTP Method</span></span> | <span data-ttu-id="179df-328">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="179df-328">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-329">GET</span><span class="sxs-lookup"><span data-stu-id="179df-329">GET</span></span> |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="179df-330">Příklad:</span><span class="sxs-lookup"><span data-stu-id="179df-330">Example:</span></span><br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| <span data-ttu-id="179df-331">Název parametru</span><span class="sxs-lookup"><span data-stu-id="179df-331">Parameter Name</span></span> | <span data-ttu-id="179df-332">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="179df-332">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-333">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="179df-333">modelId</span></span> |<span data-ttu-id="179df-334">Jedinečný identifikátor modelu hello (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="179df-334">Unique identifier of hello model  (case-sensitive)</span></span> |
| <span data-ttu-id="179df-335">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="179df-335">onlyLastBuild</span></span> |<span data-ttu-id="179df-336">Určuje, zda všechny hello tooreturn sestavit historie hello modelu nebo pouze stav hello hello poslední sestavení.</span><span class="sxs-lookup"><span data-stu-id="179df-336">Indicates whether tooreturn all hello build history of hello model or only hello status of hello most recent build.</span></span> |
| <span data-ttu-id="179df-337">apiVersion</span><span class="sxs-lookup"><span data-stu-id="179df-337">apiVersion</span></span> |<span data-ttu-id="179df-338">1.0</span><span class="sxs-lookup"><span data-stu-id="179df-338">1.0</span></span> |

<span data-ttu-id="179df-339">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="179df-339">**Response**:</span></span>

<span data-ttu-id="179df-340">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="179df-340">HTTP Status code: 200</span></span>

<span data-ttu-id="179df-341">Hello odpověď obsahuje jeden záznam za sestavení.</span><span class="sxs-lookup"><span data-stu-id="179df-341">hello response includes one entry per build.</span></span> <span data-ttu-id="179df-342">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="179df-342">Each entry has hello following data:</span></span>

* <span data-ttu-id="179df-343">`feed/entry/content/properties/UserName`– Název hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="179df-343">`feed/entry/content/properties/UserName` – Name of hello user.</span></span>
* <span data-ttu-id="179df-344">`feed/entry/content/properties/ModelName`– Název modelu hello.</span><span class="sxs-lookup"><span data-stu-id="179df-344">`feed/entry/content/properties/ModelName` – Name of hello model.</span></span>
* <span data-ttu-id="179df-345">`feed/entry/content/properties/ModelId`– Model jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="179df-345">`feed/entry/content/properties/ModelId` – Model unique identifier.</span></span>
* <span data-ttu-id="179df-346">`feed/entry/content/properties/IsDeployed`– Jestli hello sestavení nasazeno do (také známa jako</span><span class="sxs-lookup"><span data-stu-id="179df-346">`feed/entry/content/properties/IsDeployed` – Whether hello build is deployed (a.k.a.</span></span> <span data-ttu-id="179df-347">aktivní sestavení).</span><span class="sxs-lookup"><span data-stu-id="179df-347">active build).</span></span>
* <span data-ttu-id="179df-348">`feed/entry/content/properties/BuildId`– Vytvořte jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="179df-348">`feed/entry/content/properties/BuildId` – Build unique identifier.</span></span>
* <span data-ttu-id="179df-349">`feed/entry/content/properties/BuildType`-Typ hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="179df-349">`feed/entry/content/properties/BuildType` - Type of hello build.</span></span>
* <span data-ttu-id="179df-350">`feed/entry/content/properties/Status`– Stav sestavení.</span><span class="sxs-lookup"><span data-stu-id="179df-350">`feed/entry/content/properties/Status` – Build status.</span></span> <span data-ttu-id="179df-351">Může být jedna z následujících hello: Chyba, sestavování, zařazeno do fronty, zrušení, zrušení, úspěch</span><span class="sxs-lookup"><span data-stu-id="179df-351">Can be one of hello following: Error, Building, Queued, Canceling, Canceled, Success</span></span>
* <span data-ttu-id="179df-352">`feed/entry/content/properties/StatusMessage`– Zpráva podrobné informace o stavu (platí pouze toospecific stavy).</span><span class="sxs-lookup"><span data-stu-id="179df-352">`feed/entry/content/properties/StatusMessage` – Detailed status message (applies only toospecific states).</span></span>
* <span data-ttu-id="179df-353">`feed/entry/content/properties/Progress`– Sestavení průběh (%).</span><span class="sxs-lookup"><span data-stu-id="179df-353">`feed/entry/content/properties/Progress` – Build progress (%).</span></span>
* <span data-ttu-id="179df-354">`feed/entry/content/properties/StartTime`– Sestavení čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="179df-354">`feed/entry/content/properties/StartTime` – Build start time.</span></span>
* <span data-ttu-id="179df-355">`feed/entry/content/properties/EndTime`– Sestavení koncový čas.</span><span class="sxs-lookup"><span data-stu-id="179df-355">`feed/entry/content/properties/EndTime` – Build end time.</span></span>
* <span data-ttu-id="179df-356">`feed/entry/content/properties/ExecutionTime`– Sestavení doba trvání.</span><span class="sxs-lookup"><span data-stu-id="179df-356">`feed/entry/content/properties/ExecutionTime` – Build duration.</span></span>
* <span data-ttu-id="179df-357">`feed/entry/content/properties/ProgressStep`– Podrobnosti o hello aktuální fázi, který je v průběhu sestavení v.</span><span class="sxs-lookup"><span data-stu-id="179df-357">`feed/entry/content/properties/ProgressStep` – Details about hello current stage that a build in progress is in.</span></span>

<span data-ttu-id="179df-358">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="179df-358">Valid build status:</span></span>

* <span data-ttu-id="179df-359">Vytvořeno – položka sestavení žádost byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="179df-359">Created – Build request entry was created.</span></span>
* <span data-ttu-id="179df-360">Ve frontě – sestavení požadavku byla spuštěna a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="179df-360">Queued – Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="179df-361">Probíhá vytváření – sestavení.</span><span class="sxs-lookup"><span data-stu-id="179df-361">Building – Build is in process.</span></span>
* <span data-ttu-id="179df-362">Úspěch – sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="179df-362">Success – Build ended successfully.</span></span>
* <span data-ttu-id="179df-363">Chyba – sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="179df-363">Error – Build ended with a failure.</span></span>
* <span data-ttu-id="179df-364">Došlo ke zrušení – sestavení bylo zrušeno.</span><span class="sxs-lookup"><span data-stu-id="179df-364">Canceled – Build was canceled.</span></span>
* <span data-ttu-id="179df-365">Zrušení – sestavení probíhá její zrušení.</span><span class="sxs-lookup"><span data-stu-id="179df-365">Canceling – Build is being canceled.</span></span>

<span data-ttu-id="179df-366">Platné hodnoty pro typ sestavení:</span><span class="sxs-lookup"><span data-stu-id="179df-366">Valid values for build type:</span></span>

* <span data-ttu-id="179df-367">Pořadí - pořadí sestavení.</span><span class="sxs-lookup"><span data-stu-id="179df-367">Rank - Rank build.</span></span> <span data-ttu-id="179df-368">(Pro pořadí sestavení podrobnosti naleznete v dokumentu "Doporučení Machine Learning API dokumentace" toohello.)</span><span class="sxs-lookup"><span data-stu-id="179df-368">(For rank build details, please refer toohello "Machine Learning Recommendation API documentation" document.)</span></span>
* <span data-ttu-id="179df-369">Doporučení - doporučení sestavení.</span><span class="sxs-lookup"><span data-stu-id="179df-369">Recommendation - Recommendation build.</span></span>
* <span data-ttu-id="179df-370">Společně fbt - často koupila sestavení.</span><span class="sxs-lookup"><span data-stu-id="179df-370">Fbt - Frequently bought together build.</span></span>

<span data-ttu-id="179df-371">OData XML</span><span class="sxs-lookup"><span data-stu-id="179df-371">OData XML</span></span>

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


### <a name="get-recommendations"></a><span data-ttu-id="179df-372">Získejte doporučení</span><span class="sxs-lookup"><span data-stu-id="179df-372">Get recommendations</span></span>
| <span data-ttu-id="179df-373">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="179df-373">HTTP Method</span></span> | <span data-ttu-id="179df-374">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="179df-374">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-375">GET</span><span class="sxs-lookup"><span data-stu-id="179df-375">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="179df-376">Příklad:</span><span class="sxs-lookup"><span data-stu-id="179df-376">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="179df-377">Název parametru</span><span class="sxs-lookup"><span data-stu-id="179df-377">Parameter Name</span></span> | <span data-ttu-id="179df-378">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="179df-378">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-379">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="179df-379">modelId</span></span> |<span data-ttu-id="179df-380">Jedinečný identifikátor modelu hello (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="179df-380">Unique identifier of hello model (case-sensitive)</span></span> |
| <span data-ttu-id="179df-381">položky ItemID</span><span class="sxs-lookup"><span data-stu-id="179df-381">itemIds</span></span> |<span data-ttu-id="179df-382">Textový soubor s oddělovači seznam hello položky toorecommend pro.</span><span class="sxs-lookup"><span data-stu-id="179df-382">Comma-separated list of hello items toorecommend for.</span></span><br><span data-ttu-id="179df-383">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="179df-383">Max length: 1024</span></span> |
| <span data-ttu-id="179df-384">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="179df-384">numberOfResults</span></span> |<span data-ttu-id="179df-385">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="179df-385">Number of required results</span></span> |
| <span data-ttu-id="179df-386">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="179df-386">includeMetatadata</span></span> |<span data-ttu-id="179df-387">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="179df-387">Future use, always false</span></span> |
| <span data-ttu-id="179df-388">apiVersion</span><span class="sxs-lookup"><span data-stu-id="179df-388">apiVersion</span></span> |<span data-ttu-id="179df-389">1.0</span><span class="sxs-lookup"><span data-stu-id="179df-389">1.0</span></span> |

<span data-ttu-id="179df-390">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="179df-390">**Response:**</span></span>

<span data-ttu-id="179df-391">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="179df-391">HTTP Status code: 200</span></span>

<span data-ttu-id="179df-392">Hello odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="179df-392">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="179df-393">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="179df-393">Each entry has hello following data:</span></span>

* <span data-ttu-id="179df-394">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="179df-394">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="179df-395">`Feed\entry\content\properties\Name`-Název položky hello.</span><span class="sxs-lookup"><span data-stu-id="179df-395">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="179df-396">`Feed\entry\content\properties\Rating`-Hodnocení hello doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="179df-396">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="179df-397">`Feed\entry\content\properties\Reasoning`-Doporučení reasoning (například vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="179df-397">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (for example, recommendation explanations).</span></span>

<span data-ttu-id="179df-398">OData XML</span><span class="sxs-lookup"><span data-stu-id="179df-398">OData XML</span></span>

<span data-ttu-id="179df-399">odpověď příklad Hello níže zahrnuje 10 položek doporučené:</span><span class="sxs-lookup"><span data-stu-id="179df-399">hello example response below includes 10 recommended items:</span></span>

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

### <a name="update-model"></a><span data-ttu-id="179df-400">Aktualizace modelu</span><span class="sxs-lookup"><span data-stu-id="179df-400">Update model</span></span>
<span data-ttu-id="179df-401">Můžete aktualizovat popis modelu hello nebo hello ID aktivního sestavení.</span><span class="sxs-lookup"><span data-stu-id="179df-401">You can update hello model description or hello active build ID.</span></span>
<span data-ttu-id="179df-402">*ID aktivního sestavení* -každé sestavení pro každý model má sestavení ID.</span><span class="sxs-lookup"><span data-stu-id="179df-402">*Active build ID* - Every build for every model has a build ID.</span></span> <span data-ttu-id="179df-403">ID aktivního sestavení Hello je hello prvním úspěšném sestavení každý nový model.</span><span class="sxs-lookup"><span data-stu-id="179df-403">hello active build ID is hello first successful build of every new model.</span></span> <span data-ttu-id="179df-404">Jakmile máte ID aktivního sestavení a provádět další sestavení pro hello stejného modelu, je nutné tooexplicitly nastavit jako hello výchozí sestavení ID Pokud budete chtít.</span><span class="sxs-lookup"><span data-stu-id="179df-404">Once you have an active build ID and you do additional builds for hello same model, you need tooexplicitly set it as hello default build ID if you want to.</span></span> <span data-ttu-id="179df-405">Když spotřebujete doporučení, pokud nezadáte ID hello sestavení, který chcete toouse, výchozí hello, jeden budou automaticky použita.</span><span class="sxs-lookup"><span data-stu-id="179df-405">When you consume recommendations, if you do not specify hello build ID that you want toouse, hello default one will be used automatically.</span></span>

<span data-ttu-id="179df-406">Tento mechanismus umožňuje - po model doporučení v produkčním prostředí - toobuild nové modely a otestovat je před zvýšením úrovně je tooproduction.</span><span class="sxs-lookup"><span data-stu-id="179df-406">This mechanism enables you - once you have a recommendation model in production - toobuild new models and test them before you promote them tooproduction.</span></span>

| <span data-ttu-id="179df-407">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="179df-407">HTTP Method</span></span> | <span data-ttu-id="179df-408">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="179df-408">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-409">PUT</span><span class="sxs-lookup"><span data-stu-id="179df-409">PUT</span></span> |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="179df-410">Příklad:</span><span class="sxs-lookup"><span data-stu-id="179df-410">Example:</span></span><br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| <span data-ttu-id="179df-411">Název parametru</span><span class="sxs-lookup"><span data-stu-id="179df-411">Parameter Name</span></span> | <span data-ttu-id="179df-412">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="179df-412">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="179df-413">id</span><span class="sxs-lookup"><span data-stu-id="179df-413">id</span></span> |<span data-ttu-id="179df-414">Jedinečný identifikátor modelu hello (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="179df-414">Unique identifier of hello model (case-sensitive)</span></span> |
| <span data-ttu-id="179df-415">apiVersion</span><span class="sxs-lookup"><span data-stu-id="179df-415">apiVersion</span></span> |<span data-ttu-id="179df-416">1.0</span><span class="sxs-lookup"><span data-stu-id="179df-416">1.0</span></span> |
|  | |
| <span data-ttu-id="179df-417">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="179df-417">Request Body</span></span> |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br><span data-ttu-id="179df-418">Všimněte si, že hello XML značky popis a ActiveBuildId jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="179df-418">Note that hello XML tags Description and ActiveBuildId are optional.</span></span> <span data-ttu-id="179df-419">Pokud nechcete, aby tooset popis nebo ActiveBuildId, odeberte celý značky hello.</span><span class="sxs-lookup"><span data-stu-id="179df-419">If you do not want tooset Description or ActiveBuildId, remove hello entire tag.</span></span> |

<span data-ttu-id="179df-420">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="179df-420">**Response**:</span></span>

<span data-ttu-id="179df-421">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="179df-421">HTTP Status code: 200</span></span>

<span data-ttu-id="179df-422">OData XML</span><span class="sxs-lookup"><span data-stu-id="179df-422">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Update an Existing Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T10:27:17Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

## <a name="legal"></a><span data-ttu-id="179df-423">Právní informace</span><span class="sxs-lookup"><span data-stu-id="179df-423">Legal</span></span>
<span data-ttu-id="179df-424">Tento dokument je poskytován "jako-je".</span><span class="sxs-lookup"><span data-stu-id="179df-424">This document is provided "as-is".</span></span> <span data-ttu-id="179df-425">Informace a názory vyjádřené v tomto dokumentu včetně adres URL a dalších odkazů na internetové weby mohou změnit bez předchozího upozornění.</span><span class="sxs-lookup"><span data-stu-id="179df-425">Information and views expressed in this document, including URL and other Internet website references, may change without notice.</span></span> <span data-ttu-id="179df-426">Některé příklady použité v ukázkách jsou jenom ilustrativní a smyšlené.</span><span class="sxs-lookup"><span data-stu-id="179df-426">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="179df-427">Žádný skutečný vztah nebo připojení je určený nebo událostmi.</span><span class="sxs-lookup"><span data-stu-id="179df-427">No real association or connection is intended or should be inferred.</span></span> <span data-ttu-id="179df-428">Tento dokument vám neposkytuje žádná zákonná práva tooany týkající se duševního vlastnictví produktů společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="179df-428">This document does not provide you with any legal rights tooany intellectual property in any Microsoft product.</span></span> <span data-ttu-id="179df-429">Můžete kopírovat a tento dokument použít pro interní referenční účely.</span><span class="sxs-lookup"><span data-stu-id="179df-429">You may copy and use this document for your internal, reference purposes.</span></span> <span data-ttu-id="179df-430">© 2014 Microsoft.</span><span class="sxs-lookup"><span data-stu-id="179df-430">© 2014 Microsoft.</span></span> <span data-ttu-id="179df-431">Všechna práva vyhrazena.</span><span class="sxs-lookup"><span data-stu-id="179df-431">All rights reserved.</span></span> 

