---
title: "Koncové body REST aaaCall pomocí protokolu HTTP + Swagger konektor pro Azure Logic Apps | Microsoft Docs"
description: "Připojit tooREST koncových bodů z aplikace logiky prostřednictvím Swagger s hello HTTP + Swagger konektoru"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: baaa57689ff41fcd052f9d86086e36619ddec46e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http--swagger-action"></a><span data-ttu-id="80b76-103">Začínáme s hello HTTP + Swagger akce</span><span class="sxs-lookup"><span data-stu-id="80b76-103">Get started with hello HTTP + Swagger action</span></span>

<span data-ttu-id="80b76-104">Můžete vytvořit koncový bod prvotřídní konektor tooany REST prostřednictvím [dokumentu Swagger](https://swagger.io) při použití hello HTTP + Swagger akce v pracovním postupu logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="80b76-104">You can create a first-class connector tooany REST endpoint through a [Swagger document](https://swagger.io) when you use hello HTTP + Swagger action in your logic app workflow.</span></span> <span data-ttu-id="80b76-105">Můžete také rozšířit logiku aplikace toocall žádný koncový bod REST prvotřídní prostředí návrhář aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="80b76-105">You can also extend logic apps toocall any REST endpoint with a first-class Logic App Designer experience.</span></span>

<span data-ttu-id="80b76-106">jak zjistit, aplikace logiky toocreate s konektory, toolearn [vytvoření nové aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="80b76-106">toolearn how toocreate logic apps with connectors, see [Create a new logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a><span data-ttu-id="80b76-107">Použít protokol HTTP + Swagger jako aktivační událost nebo akci.</span><span class="sxs-lookup"><span data-stu-id="80b76-107">Use HTTP + Swagger as a trigger or an action</span></span>

<span data-ttu-id="80b76-108">Vítejte HTTP + Swagger aktivační události a akce pracovní hello stejné jako hello [akce HTTP](connectors-native-http.md) , ale poskytuje lepší prostředí v návrháři aplikace logiky díky zpřístupnění strukturu hello rozhraní API a výstupy z hello [Swagger metadata](https://swagger.io) .</span><span class="sxs-lookup"><span data-stu-id="80b76-108">hello HTTP + Swagger trigger and action work hello same as hello [HTTP action](connectors-native-http.md) but provide a better experience in Logic App Designer by exposing hello API structure and outputs from hello [Swagger metadata](https://swagger.io).</span></span> <span data-ttu-id="80b76-109">Můžete také použít hello HTTP + Swagger konektor jako trigger.</span><span class="sxs-lookup"><span data-stu-id="80b76-109">You can also use hello HTTP + Swagger connector as a trigger.</span></span> <span data-ttu-id="80b76-110">Pokud chcete, aby tooimplement cyklického dotazování aktivační událost, postupujte podle hello dotazování vzor, který je popsán v [vytvořit vlastní toocall rozhraní API pro ostatní rozhraní API, služeb a systémů z aplikace logiky](../logic-apps/logic-apps-create-api-app.md#polling-triggers).</span><span class="sxs-lookup"><span data-stu-id="80b76-110">If you want tooimplement a polling trigger, follow hello polling pattern that's described in [Create custom APIs toocall other APIs, services, and systems from logic apps](../logic-apps/logic-apps-create-api-app.md#polling-triggers).</span></span>

<span data-ttu-id="80b76-111">Další informace o [logiku aplikace triggery a akce](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="80b76-111">Learn more about [logic app triggers and actions](connectors-overview.md).</span></span>

<span data-ttu-id="80b76-112">Tady je příklad, jak toouse hello HTTP + Swagger operace jako akce v pracovním postupu v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="80b76-112">Here's an example of how toouse hello HTTP + Swagger operation as an action in a workflow in a logic app.</span></span>

1. <span data-ttu-id="80b76-113">Vyberte hello **nový krok** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="80b76-113">Select hello **New Step** button.</span></span>
2. <span data-ttu-id="80b76-114">Vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="80b76-114">Select **Add an action**.</span></span>
3. <span data-ttu-id="80b76-115">Hello akce vyhledávacího pole zadejte **swagger** toolist hello HTTP + Swagger akce.</span><span class="sxs-lookup"><span data-stu-id="80b76-115">In hello action search box, type **swagger** toolist hello HTTP + Swagger action.</span></span>
   
    ![Vyberte HTTP + Swagger akce](./media/connectors-native-http-swagger/using-action-1.png)
4. <span data-ttu-id="80b76-117">Zadejte adresu URL hello dokumentem Swagger:</span><span class="sxs-lookup"><span data-stu-id="80b76-117">Type hello URL for a Swagger document:</span></span>
   
   * <span data-ttu-id="80b76-118">toowork z hello návrhář aplikace na základě logiky, hello adresa URL musí být koncový bod HTTPS a povolení CORS.</span><span class="sxs-lookup"><span data-stu-id="80b76-118">toowork from hello Logic App Designer, hello URL must be an HTTPS endpoint and have CORS enabled.</span></span>
   * <span data-ttu-id="80b76-119">Pokud dokumentu Swagger hello nesplňuje tento požadavek, můžete použít [Azure Storage s povolením CORS](#hosting-swagger-from-storage) toostore hello dokumentu.</span><span class="sxs-lookup"><span data-stu-id="80b76-119">If hello Swagger document doesn't meet this requirement, you can use [Azure Storage with CORS enabled](#hosting-swagger-from-storage) toostore hello document.</span></span>
5. <span data-ttu-id="80b76-120">Klikněte na tlačítko **Další** tooread a vykreslování z hello dokumentu Swagger.</span><span class="sxs-lookup"><span data-stu-id="80b76-120">Click **Next** tooread and render from hello Swagger document.</span></span>
6. <span data-ttu-id="80b76-121">Přidejte do žádné parametry, které jsou požadovány pro volání hello protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="80b76-121">Add in any parameters that are required for hello HTTP call.</span></span>
   
    ![Dokončení akce HTTP](./media/connectors-native-http-swagger/using-action-2.png)
7. <span data-ttu-id="80b76-123">toosave a publikovat aplikace logiky, klikněte na tlačítko **Uložit** na panelu nástrojů návrháře.</span><span class="sxs-lookup"><span data-stu-id="80b76-123">toosave and publish your logic app, click **Save** on designer toolbar.</span></span>

### <a name="host-swagger-from-azure-storage"></a><span data-ttu-id="80b76-124">Swagger hostitele ze služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="80b76-124">Host Swagger from Azure Storage</span></span>
<span data-ttu-id="80b76-125">Můžete chtít tooreference dokumentu Swagger, který není hostovaný nebo který nesplňuje požadavky zabezpečení a cross-origin hello hello Designer.</span><span class="sxs-lookup"><span data-stu-id="80b76-125">You might want tooreference a Swagger document that's not hosted, or that doesn't meet hello security and cross-origin requirements for hello designer.</span></span> <span data-ttu-id="80b76-126">tooresolve-li tento problém, můžete ukládat hello dokumentu Swagger ve službě Azure Storage a povolení CORS tooreference hello dokumentu.</span><span class="sxs-lookup"><span data-stu-id="80b76-126">tooresolve this issue, you can store hello Swagger document in Azure Storage and enable CORS tooreference hello document.</span></span>  

<span data-ttu-id="80b76-127">Tady jsou kroky toocreate hello, konfigurace a ukládat dokumenty Swagger ve službě Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="80b76-127">Here are hello steps toocreate, configure, and store Swagger documents in Azure Storage:</span></span>

1. <span data-ttu-id="80b76-128">[Vytvoření účtu úložiště Azure s Azure Blob storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="80b76-128">[Create an Azure storage account with Azure Blob storage](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="80b76-129">tooperform tento krok, nastavte oprávnění příliš**veřejný přístup**.</span><span class="sxs-lookup"><span data-stu-id="80b76-129">tooperform this step, set permissions too**Public Access**.</span></span>

2. <span data-ttu-id="80b76-130">Povolení CORS u objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="80b76-130">Enable CORS on hello blob.</span></span> 

   <span data-ttu-id="80b76-131">tooautomatically nakonfigurujte toto nastavení, můžete použít [tento skript prostředí PowerShell](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).</span><span class="sxs-lookup"><span data-stu-id="80b76-131">tooautomatically configure this setting, you can use [this PowerShell script](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).</span></span>

3. <span data-ttu-id="80b76-132">Nahrajte objekt blob toohello hello Swagger souboru.</span><span class="sxs-lookup"><span data-stu-id="80b76-132">Upload hello Swagger file toohello blob.</span></span> 

   <span data-ttu-id="80b76-133">Tento krok můžete provést z hello [portál Azure](https://portal.azure.com) nebo z nástroje, jako je [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="80b76-133">You can perform this step from hello [Azure portal](https://portal.azure.com) or from a tool like [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

4. <span data-ttu-id="80b76-134">Referenční dokument HTTPS odkaz toohello v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="80b76-134">Reference an HTTPS link toohello document in Azure Blob storage.</span></span> 

   <span data-ttu-id="80b76-135">odkaz Hello používá tento formát:</span><span class="sxs-lookup"><span data-stu-id="80b76-135">hello link uses this format:</span></span>

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a><span data-ttu-id="80b76-136">Technické podrobnosti</span><span class="sxs-lookup"><span data-stu-id="80b76-136">Technical details</span></span>
<span data-ttu-id="80b76-137">Následují hello podrobnosti o hello triggery a akce, které tento HTTP + Swagger konektor podporuje.</span><span class="sxs-lookup"><span data-stu-id="80b76-137">Following are hello details for hello triggers and actions that this HTTP + Swagger connector supports.</span></span>

## <a name="http--swagger-triggers"></a><span data-ttu-id="80b76-138">HTTP + aktivační události Swagger</span><span class="sxs-lookup"><span data-stu-id="80b76-138">HTTP + Swagger triggers</span></span>
<span data-ttu-id="80b76-139">Aktivační událost je událost, která lze použít toostart hello workflow, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="80b76-139">A trigger is an event that can be used toostart hello workflow that's defined in a logic app.</span></span> [<span data-ttu-id="80b76-140">Další informace o aktivační události.</span><span class="sxs-lookup"><span data-stu-id="80b76-140">Learn more about triggers.</span></span>](connectors-overview.md) <span data-ttu-id="80b76-141">Hello HTTP + Swagger konektor má jedna aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="80b76-141">hello HTTP + Swagger connector has one trigger.</span></span>

| <span data-ttu-id="80b76-142">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="80b76-142">Trigger</span></span> | <span data-ttu-id="80b76-143">Popis</span><span class="sxs-lookup"><span data-stu-id="80b76-143">Description</span></span> |
| --- | --- |
| <span data-ttu-id="80b76-144">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="80b76-144">HTTP + Swagger</span></span> |<span data-ttu-id="80b76-145">Ujistěte se, volání protokolu HTTP a vrátí obsah odpovědi hello</span><span class="sxs-lookup"><span data-stu-id="80b76-145">Make an HTTP call and return hello response content</span></span> |

## <a name="http--swagger-actions"></a><span data-ttu-id="80b76-146">HTTP + Swagger akce</span><span class="sxs-lookup"><span data-stu-id="80b76-146">HTTP + Swagger actions</span></span>
<span data-ttu-id="80b76-147">Akce je operace, která se provádí v pracovním postupu hello, která je definována v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="80b76-147">An action is an operation that's carried out by hello workflow that's defined in a logic app.</span></span> [<span data-ttu-id="80b76-148">Další informace o akcích.</span><span class="sxs-lookup"><span data-stu-id="80b76-148">Learn more about actions.</span></span>](connectors-overview.md) <span data-ttu-id="80b76-149">Hello HTTP + Swagger konektor má jednu možné akci.</span><span class="sxs-lookup"><span data-stu-id="80b76-149">hello HTTP + Swagger connector has one possible action.</span></span>

| <span data-ttu-id="80b76-150">Akce</span><span class="sxs-lookup"><span data-stu-id="80b76-150">Action</span></span> | <span data-ttu-id="80b76-151">Popis</span><span class="sxs-lookup"><span data-stu-id="80b76-151">Description</span></span> |
| --- | --- |
| <span data-ttu-id="80b76-152">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="80b76-152">HTTP + Swagger</span></span> |<span data-ttu-id="80b76-153">Ujistěte se, volání protokolu HTTP a vrátí obsah odpovědi hello</span><span class="sxs-lookup"><span data-stu-id="80b76-153">Make an HTTP call and return hello response content</span></span> |

### <a name="action-details"></a><span data-ttu-id="80b76-154">Podrobnosti akce</span><span class="sxs-lookup"><span data-stu-id="80b76-154">Action details</span></span>
<span data-ttu-id="80b76-155">Hello HTTP + Swagger konektor se dodává s možné jednu akci.</span><span class="sxs-lookup"><span data-stu-id="80b76-155">hello HTTP + Swagger connector comes with one possible action.</span></span> <span data-ttu-id="80b76-156">Dále najdete informace o jednotlivých hello akcí, jejich povinné a nepovinné vstupní pole a hello odpovídající výstup podrobnosti, které jsou spojeny s jejich využití.</span><span class="sxs-lookup"><span data-stu-id="80b76-156">Following is information about each of hello actions, their required and optional input fields, and hello corresponding output details that are associated with their usage.</span></span>

#### <a name="http--swagger"></a><span data-ttu-id="80b76-157">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="80b76-157">HTTP + Swagger</span></span>
<span data-ttu-id="80b76-158">Pomoc při metadat Swagger Zkontrolujte odchozí požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="80b76-158">Make an HTTP outbound request with assistance of Swagger metadata.</span></span>
<span data-ttu-id="80b76-159">Znak hvězdičky (*) znamená povinné pole.</span><span class="sxs-lookup"><span data-stu-id="80b76-159">An asterisk (*) means a required field.</span></span>

| <span data-ttu-id="80b76-160">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="80b76-160">Display name</span></span> | <span data-ttu-id="80b76-161">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="80b76-161">Property name</span></span> | <span data-ttu-id="80b76-162">Popis</span><span class="sxs-lookup"><span data-stu-id="80b76-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="80b76-163">Metoda *</span><span class="sxs-lookup"><span data-stu-id="80b76-163">Method*</span></span> |<span data-ttu-id="80b76-164">– Metoda</span><span class="sxs-lookup"><span data-stu-id="80b76-164">method</span></span> |<span data-ttu-id="80b76-165">Toouse operaci HTTP.</span><span class="sxs-lookup"><span data-stu-id="80b76-165">HTTP verb toouse.</span></span> |
| <span data-ttu-id="80b76-166">IDENTIFIKÁTOR URI *</span><span class="sxs-lookup"><span data-stu-id="80b76-166">URI*</span></span> |<span data-ttu-id="80b76-167">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="80b76-167">uri</span></span> |<span data-ttu-id="80b76-168">Identifikátor URI pro požadavek hello protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="80b76-168">URI for hello HTTP request.</span></span> |
| <span data-ttu-id="80b76-169">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="80b76-169">Headers</span></span> |<span data-ttu-id="80b76-170">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="80b76-170">headers</span></span> |<span data-ttu-id="80b76-171">Objekt JSON tooinclude hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="80b76-171">A JSON object of HTTP headers tooinclude.</span></span> |
| <span data-ttu-id="80b76-172">Tělo</span><span class="sxs-lookup"><span data-stu-id="80b76-172">Body</span></span> |<span data-ttu-id="80b76-173">Text</span><span class="sxs-lookup"><span data-stu-id="80b76-173">body</span></span> |<span data-ttu-id="80b76-174">Hello požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="80b76-174">hello HTTP request body.</span></span> |
| <span data-ttu-id="80b76-175">Authentication</span><span class="sxs-lookup"><span data-stu-id="80b76-175">Authentication</span></span> |<span data-ttu-id="80b76-176">Ověřování</span><span class="sxs-lookup"><span data-stu-id="80b76-176">authentication</span></span> |<span data-ttu-id="80b76-177">Ověřování toouse pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="80b76-177">Authentication toouse for request.</span></span> <span data-ttu-id="80b76-178">Další informace najdete v tématu hello [HTTP konektor](connectors-native-http.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="80b76-178">For more information, see hello [HTTP connector](connectors-native-http.md#authentication).</span></span> |

<span data-ttu-id="80b76-179">**Podrobnosti o výstupu**</span><span class="sxs-lookup"><span data-stu-id="80b76-179">**Output details**</span></span>

<span data-ttu-id="80b76-180">Odpověď HTTP</span><span class="sxs-lookup"><span data-stu-id="80b76-180">HTTP response</span></span>

| <span data-ttu-id="80b76-181">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="80b76-181">Property Name</span></span> | <span data-ttu-id="80b76-182">Datový typ</span><span class="sxs-lookup"><span data-stu-id="80b76-182">Data type</span></span> | <span data-ttu-id="80b76-183">Popis</span><span class="sxs-lookup"><span data-stu-id="80b76-183">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="80b76-184">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="80b76-184">Headers</span></span> |<span data-ttu-id="80b76-185">Objekt</span><span class="sxs-lookup"><span data-stu-id="80b76-185">object</span></span> |<span data-ttu-id="80b76-186">Hlavičky odpovědi</span><span class="sxs-lookup"><span data-stu-id="80b76-186">Response headers</span></span> |
| <span data-ttu-id="80b76-187">Tělo</span><span class="sxs-lookup"><span data-stu-id="80b76-187">Body</span></span> |<span data-ttu-id="80b76-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="80b76-188">object</span></span> |<span data-ttu-id="80b76-189">Objekt odpovědi</span><span class="sxs-lookup"><span data-stu-id="80b76-189">Response object</span></span> |
| <span data-ttu-id="80b76-190">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="80b76-190">Status Code</span></span> |<span data-ttu-id="80b76-191">celá čísla</span><span class="sxs-lookup"><span data-stu-id="80b76-191">int</span></span> |<span data-ttu-id="80b76-192">Stavový kód protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="80b76-192">HTTP status code</span></span> |

### <a name="http-responses"></a><span data-ttu-id="80b76-193">Odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="80b76-193">HTTP responses</span></span>
<span data-ttu-id="80b76-194">Při provádění akce toovarious volání, může být určité odpovědi.</span><span class="sxs-lookup"><span data-stu-id="80b76-194">When making calls toovarious actions, you might get certain responses.</span></span> <span data-ttu-id="80b76-195">Následuje tabulka, která popisuje odpovídající odpovědi a popisy.</span><span class="sxs-lookup"><span data-stu-id="80b76-195">Following is a table that outlines corresponding responses and descriptions.</span></span>

| <span data-ttu-id="80b76-196">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="80b76-196">Name</span></span> | <span data-ttu-id="80b76-197">Popis</span><span class="sxs-lookup"><span data-stu-id="80b76-197">Description</span></span> |
| --- | --- |
| <span data-ttu-id="80b76-198">200</span><span class="sxs-lookup"><span data-stu-id="80b76-198">200</span></span> |<span data-ttu-id="80b76-199">OK</span><span class="sxs-lookup"><span data-stu-id="80b76-199">OK</span></span> |
| <span data-ttu-id="80b76-200">202</span><span class="sxs-lookup"><span data-stu-id="80b76-200">202</span></span> |<span data-ttu-id="80b76-201">Přijmout</span><span class="sxs-lookup"><span data-stu-id="80b76-201">Accepted</span></span> |
| <span data-ttu-id="80b76-202">400</span><span class="sxs-lookup"><span data-stu-id="80b76-202">400</span></span> |<span data-ttu-id="80b76-203">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="80b76-203">Bad request</span></span> |
| <span data-ttu-id="80b76-204">401</span><span class="sxs-lookup"><span data-stu-id="80b76-204">401</span></span> |<span data-ttu-id="80b76-205">Neautorizováno</span><span class="sxs-lookup"><span data-stu-id="80b76-205">Unauthorized</span></span> |
| <span data-ttu-id="80b76-206">403</span><span class="sxs-lookup"><span data-stu-id="80b76-206">403</span></span> |<span data-ttu-id="80b76-207">Je zakázané</span><span class="sxs-lookup"><span data-stu-id="80b76-207">Forbidden</span></span> |
| <span data-ttu-id="80b76-208">404</span><span class="sxs-lookup"><span data-stu-id="80b76-208">404</span></span> |<span data-ttu-id="80b76-209">Nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="80b76-209">Not Found</span></span> |
| <span data-ttu-id="80b76-210">500</span><span class="sxs-lookup"><span data-stu-id="80b76-210">500</span></span> |<span data-ttu-id="80b76-211">Vnitřní chybu serveru.</span><span class="sxs-lookup"><span data-stu-id="80b76-211">Internal server error.</span></span> <span data-ttu-id="80b76-212">Došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="80b76-212">Unknown error occurred.</span></span> |

- - -
## <a name="next-steps"></a><span data-ttu-id="80b76-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80b76-213">Next steps</span></span>

* [<span data-ttu-id="80b76-214">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="80b76-214">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)
* [<span data-ttu-id="80b76-215">Vyhledání jiných konektorů</span><span class="sxs-lookup"><span data-stu-id="80b76-215">Find other connectors</span></span>](apis-list.md)