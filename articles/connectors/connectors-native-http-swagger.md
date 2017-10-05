---
title: "Volání REST koncové body pomocí protokolu HTTP + Swagger konektor pro Azure Logic Apps | Microsoft Docs"
description: "Připojit k koncové body REST z aplikace logiky prostřednictvím Swagger pomocí protokolu HTTP + Swagger konektoru"
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
ms.openlocfilehash: 3e9229d94e96aad7b769d0e55d208d856e3b80bc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-the-http--swagger-action"></a><span data-ttu-id="9b066-103">Začínáme s HTTP + Swagger akce</span><span class="sxs-lookup"><span data-stu-id="9b066-103">Get started with the HTTP + Swagger action</span></span>

<span data-ttu-id="9b066-104">Můžete vytvořit první třídy konektor pro libovolný koncový bod REST prostřednictvím [dokumentu Swagger](https://swagger.io) při použití protokolu HTTP + Swagger akce v pracovním postupu logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b066-104">You can create a first-class connector to any REST endpoint through a [Swagger document](https://swagger.io) when you use the HTTP + Swagger action in your logic app workflow.</span></span> <span data-ttu-id="9b066-105">Můžete také rozšířit logic apps, abyste volat žádný koncový bod REST prvotřídní prostředí návrhář aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="9b066-105">You can also extend logic apps to call any REST endpoint with a first-class Logic App Designer experience.</span></span>

<span data-ttu-id="9b066-106">Chcete-li informace o vytváření aplikací logiky s konektory, přečtěte si téma [vytvoření nové aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9b066-106">To learn how to create logic apps with connectors, see [Create a new logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a><span data-ttu-id="9b066-107">Použít protokol HTTP + Swagger jako aktivační událost nebo akci.</span><span class="sxs-lookup"><span data-stu-id="9b066-107">Use HTTP + Swagger as a trigger or an action</span></span>

<span data-ttu-id="9b066-108">HTTP + Swagger aktivovat a akce fungovat stejně jako [akce HTTP](connectors-native-http.md) ale zajistit lepší prostředí v návrháři aplikace logiky díky zpřístupnění strukturu rozhraní API a výstupy z [Swagger metadata](https://swagger.io).</span><span class="sxs-lookup"><span data-stu-id="9b066-108">The HTTP + Swagger trigger and action work the same as the [HTTP action](connectors-native-http.md) but provide a better experience in Logic App Designer by exposing the API structure and outputs from the [Swagger metadata](https://swagger.io).</span></span> <span data-ttu-id="9b066-109">Můžete také použít HTTP + Swagger konektor jako trigger.</span><span class="sxs-lookup"><span data-stu-id="9b066-109">You can also use the HTTP + Swagger connector as a trigger.</span></span> <span data-ttu-id="9b066-110">Pokud chcete implementovat cyklického dotazování aktivační událost, postupujte podle vzoru dotazování, která je popsána v [vytvořte vlastní rozhraní API volat další rozhraní API, služeb a systémů z aplikace logiky](../logic-apps/logic-apps-create-api-app.md#polling-triggers).</span><span class="sxs-lookup"><span data-stu-id="9b066-110">If you want to implement a polling trigger, follow the polling pattern that's described in [Create custom APIs to call other APIs, services, and systems from logic apps](../logic-apps/logic-apps-create-api-app.md#polling-triggers).</span></span>

<span data-ttu-id="9b066-111">Další informace o [logiku aplikace triggery a akce](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9b066-111">Learn more about [logic app triggers and actions](connectors-overview.md).</span></span>

<span data-ttu-id="9b066-112">Tady je příklad toho, jak pomocí HTTP + operací Swagger jako akce v pracovním postupu v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9b066-112">Here's an example of how to use the HTTP + Swagger operation as an action in a workflow in a logic app.</span></span>

1. <span data-ttu-id="9b066-113">Vyberte **nový krok** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9b066-113">Select the **New Step** button.</span></span>
2. <span data-ttu-id="9b066-114">Vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="9b066-114">Select **Add an action**.</span></span>
3. <span data-ttu-id="9b066-115">Zadejte do vyhledávacího pole Akce **swagger** seznamu HTTP + Swagger akce.</span><span class="sxs-lookup"><span data-stu-id="9b066-115">In the action search box, type **swagger** to list the HTTP + Swagger action.</span></span>
   
    ![Vyberte HTTP + Swagger akce](./media/connectors-native-http-swagger/using-action-1.png)
4. <span data-ttu-id="9b066-117">Zadejte adresu URL dokumentu Swagger:</span><span class="sxs-lookup"><span data-stu-id="9b066-117">Type the URL for a Swagger document:</span></span>
   
   * <span data-ttu-id="9b066-118">Chcete-li z návrháře aplikace logiky, adresa URL musí být koncový bod HTTPS a povolení CORS.</span><span class="sxs-lookup"><span data-stu-id="9b066-118">To work from the Logic App Designer, the URL must be an HTTPS endpoint and have CORS enabled.</span></span>
   * <span data-ttu-id="9b066-119">Pokud dokumentu Swagger nesplňuje tento požadavek, můžete použít [Azure Storage s povolením CORS](#hosting-swagger-from-storage) k uložení dokumentů.</span><span class="sxs-lookup"><span data-stu-id="9b066-119">If the Swagger document doesn't meet this requirement, you can use [Azure Storage with CORS enabled](#hosting-swagger-from-storage) to store the document.</span></span>
5. <span data-ttu-id="9b066-120">Klikněte na tlačítko **Další** ke čtení a vykreslování z dokumentu Swagger.</span><span class="sxs-lookup"><span data-stu-id="9b066-120">Click **Next** to read and render from the Swagger document.</span></span>
6. <span data-ttu-id="9b066-121">Přidejte do žádné parametry, které jsou požadovány pro volání protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b066-121">Add in any parameters that are required for the HTTP call.</span></span>
   
    ![Dokončení akce HTTP](./media/connectors-native-http-swagger/using-action-2.png)
7. <span data-ttu-id="9b066-123">Pokud chcete uložit a publikovat aplikace logiky, klikněte na tlačítko **Uložit** na panelu nástrojů návrháře.</span><span class="sxs-lookup"><span data-stu-id="9b066-123">To save and publish your logic app, click **Save** on designer toolbar.</span></span>

### <a name="host-swagger-from-azure-storage"></a><span data-ttu-id="9b066-124">Swagger hostitele ze služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="9b066-124">Host Swagger from Azure Storage</span></span>
<span data-ttu-id="9b066-125">Můžete tak, aby odkazovaly dokumentu Swagger, který není hostovaný nebo který nesplňuje požadavky cross-origin pro návrháře a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9b066-125">You might want to reference a Swagger document that's not hosted, or that doesn't meet the security and cross-origin requirements for the designer.</span></span> <span data-ttu-id="9b066-126">Chcete-li vyřešit tento problém, můžete ukládat dokumentu Swagger ve službě Azure Storage a zapnout CORS a odkazovat na dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9b066-126">To resolve this issue, you can store the Swagger document in Azure Storage and enable CORS to reference the document.</span></span>  

<span data-ttu-id="9b066-127">Zde jsou kroky pro vytvoření, konfigurace a ukládat dokumenty Swagger ve službě Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="9b066-127">Here are the steps to create, configure, and store Swagger documents in Azure Storage:</span></span>

1. <span data-ttu-id="9b066-128">[Vytvoření účtu úložiště Azure s Azure Blob storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="9b066-128">[Create an Azure storage account with Azure Blob storage](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="9b066-129">Chcete-li provést tento krok, nastavte oprávnění na **veřejný přístup**.</span><span class="sxs-lookup"><span data-stu-id="9b066-129">To perform this step, set permissions to **Public Access**.</span></span>

2. <span data-ttu-id="9b066-130">Povolení CORS u objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9b066-130">Enable CORS on the blob.</span></span> 

   <span data-ttu-id="9b066-131">Chcete-li automaticky konfigurovat toto nastavení, můžete použít [tento skript prostředí PowerShell](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).</span><span class="sxs-lookup"><span data-stu-id="9b066-131">To automatically configure this setting, you can use [this PowerShell script](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).</span></span>

3. <span data-ttu-id="9b066-132">Nahrajte soubor Swagger do objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9b066-132">Upload the Swagger file to the blob.</span></span> 

   <span data-ttu-id="9b066-133">Můžete provést tento krok z [portál Azure](https://portal.azure.com) nebo z nástroje, jako je [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="9b066-133">You can perform this step from the [Azure portal](https://portal.azure.com) or from a tool like [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

4. <span data-ttu-id="9b066-134">Referenční odkazu HTTPS v dokumentu v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="9b066-134">Reference an HTTPS link to the document in Azure Blob storage.</span></span> 

   <span data-ttu-id="9b066-135">Odkaz používá tento formát:</span><span class="sxs-lookup"><span data-stu-id="9b066-135">The link uses this format:</span></span>

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a><span data-ttu-id="9b066-136">Technické podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9b066-136">Technical details</span></span>
<span data-ttu-id="9b066-137">Následují podrobnosti triggery a akce, který tento HTTP + Swagger konektor podporuje.</span><span class="sxs-lookup"><span data-stu-id="9b066-137">Following are the details for the triggers and actions that this HTTP + Swagger connector supports.</span></span>

## <a name="http--swagger-triggers"></a><span data-ttu-id="9b066-138">HTTP + aktivační události Swagger</span><span class="sxs-lookup"><span data-stu-id="9b066-138">HTTP + Swagger triggers</span></span>
<span data-ttu-id="9b066-139">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9b066-139">A trigger is an event that can be used to start the workflow that's defined in a logic app.</span></span> [<span data-ttu-id="9b066-140">Další informace o aktivační události.</span><span class="sxs-lookup"><span data-stu-id="9b066-140">Learn more about triggers.</span></span>](connectors-overview.md) <span data-ttu-id="9b066-141">HTTP + Swagger konektor má jedna aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="9b066-141">The HTTP + Swagger connector has one trigger.</span></span>

| <span data-ttu-id="9b066-142">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="9b066-142">Trigger</span></span> | <span data-ttu-id="9b066-143">Popis</span><span class="sxs-lookup"><span data-stu-id="9b066-143">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9b066-144">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="9b066-144">HTTP + Swagger</span></span> |<span data-ttu-id="9b066-145">Ujistěte se, volání protokolu HTTP a vrátit obsahu odpovědi</span><span class="sxs-lookup"><span data-stu-id="9b066-145">Make an HTTP call and return the response content</span></span> |

## <a name="http--swagger-actions"></a><span data-ttu-id="9b066-146">HTTP + Swagger akce</span><span class="sxs-lookup"><span data-stu-id="9b066-146">HTTP + Swagger actions</span></span>
<span data-ttu-id="9b066-147">Akce je operace, která se provádí v pracovním postupu, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9b066-147">An action is an operation that's carried out by the workflow that's defined in a logic app.</span></span> [<span data-ttu-id="9b066-148">Další informace o akcích.</span><span class="sxs-lookup"><span data-stu-id="9b066-148">Learn more about actions.</span></span>](connectors-overview.md) <span data-ttu-id="9b066-149">HTTP + Swagger konektor má jednu možné akci.</span><span class="sxs-lookup"><span data-stu-id="9b066-149">The HTTP + Swagger connector has one possible action.</span></span>

| <span data-ttu-id="9b066-150">Akce</span><span class="sxs-lookup"><span data-stu-id="9b066-150">Action</span></span> | <span data-ttu-id="9b066-151">Popis</span><span class="sxs-lookup"><span data-stu-id="9b066-151">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9b066-152">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="9b066-152">HTTP + Swagger</span></span> |<span data-ttu-id="9b066-153">Ujistěte se, volání protokolu HTTP a vrátit obsahu odpovědi</span><span class="sxs-lookup"><span data-stu-id="9b066-153">Make an HTTP call and return the response content</span></span> |

### <a name="action-details"></a><span data-ttu-id="9b066-154">Podrobnosti akce</span><span class="sxs-lookup"><span data-stu-id="9b066-154">Action details</span></span>
<span data-ttu-id="9b066-155">HTTP + Swagger konektor se dodává s možné jednu akci.</span><span class="sxs-lookup"><span data-stu-id="9b066-155">The HTTP + Swagger connector comes with one possible action.</span></span> <span data-ttu-id="9b066-156">Dále najdete informace o jednotlivých akce, jejich povinné a nepovinné vstupní pole a odpovídající výstup podrobnosti, které jsou spojeny s jejich využití.</span><span class="sxs-lookup"><span data-stu-id="9b066-156">Following is information about each of the actions, their required and optional input fields, and the corresponding output details that are associated with their usage.</span></span>

#### <a name="http--swagger"></a><span data-ttu-id="9b066-157">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="9b066-157">HTTP + Swagger</span></span>
<span data-ttu-id="9b066-158">Pomoc při metadat Swagger Zkontrolujte odchozí požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b066-158">Make an HTTP outbound request with assistance of Swagger metadata.</span></span>
<span data-ttu-id="9b066-159">Znak hvězdičky (*) znamená povinné pole.</span><span class="sxs-lookup"><span data-stu-id="9b066-159">An asterisk (*) means a required field.</span></span>

| <span data-ttu-id="9b066-160">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="9b066-160">Display name</span></span> | <span data-ttu-id="9b066-161">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9b066-161">Property name</span></span> | <span data-ttu-id="9b066-162">Popis</span><span class="sxs-lookup"><span data-stu-id="9b066-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9b066-163">Metoda *</span><span class="sxs-lookup"><span data-stu-id="9b066-163">Method*</span></span> |<span data-ttu-id="9b066-164">– Metoda</span><span class="sxs-lookup"><span data-stu-id="9b066-164">method</span></span> |<span data-ttu-id="9b066-165">Příkaz HTTP, používat.</span><span class="sxs-lookup"><span data-stu-id="9b066-165">HTTP verb to use.</span></span> |
| <span data-ttu-id="9b066-166">IDENTIFIKÁTOR URI *</span><span class="sxs-lookup"><span data-stu-id="9b066-166">URI*</span></span> |<span data-ttu-id="9b066-167">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="9b066-167">uri</span></span> |<span data-ttu-id="9b066-168">Identifikátor URI pro požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b066-168">URI for the HTTP request.</span></span> |
| <span data-ttu-id="9b066-169">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="9b066-169">Headers</span></span> |<span data-ttu-id="9b066-170">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="9b066-170">headers</span></span> |<span data-ttu-id="9b066-171">Objekt JSON hlaviček HTTP, které chcete zahrnout.</span><span class="sxs-lookup"><span data-stu-id="9b066-171">A JSON object of HTTP headers to include.</span></span> |
| <span data-ttu-id="9b066-172">Tělo</span><span class="sxs-lookup"><span data-stu-id="9b066-172">Body</span></span> |<span data-ttu-id="9b066-173">Text</span><span class="sxs-lookup"><span data-stu-id="9b066-173">body</span></span> |<span data-ttu-id="9b066-174">Požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b066-174">The HTTP request body.</span></span> |
| <span data-ttu-id="9b066-175">Authentication</span><span class="sxs-lookup"><span data-stu-id="9b066-175">Authentication</span></span> |<span data-ttu-id="9b066-176">Ověřování</span><span class="sxs-lookup"><span data-stu-id="9b066-176">authentication</span></span> |<span data-ttu-id="9b066-177">Ověřování chcete použít pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="9b066-177">Authentication to use for request.</span></span> <span data-ttu-id="9b066-178">Další informace najdete v tématu [HTTP konektor](connectors-native-http.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="9b066-178">For more information, see the [HTTP connector](connectors-native-http.md#authentication).</span></span> |

<span data-ttu-id="9b066-179">**Podrobnosti o výstupu**</span><span class="sxs-lookup"><span data-stu-id="9b066-179">**Output details**</span></span>

<span data-ttu-id="9b066-180">Odpověď HTTP</span><span class="sxs-lookup"><span data-stu-id="9b066-180">HTTP response</span></span>

| <span data-ttu-id="9b066-181">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9b066-181">Property Name</span></span> | <span data-ttu-id="9b066-182">Datový typ</span><span class="sxs-lookup"><span data-stu-id="9b066-182">Data type</span></span> | <span data-ttu-id="9b066-183">Popis</span><span class="sxs-lookup"><span data-stu-id="9b066-183">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9b066-184">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="9b066-184">Headers</span></span> |<span data-ttu-id="9b066-185">Objekt</span><span class="sxs-lookup"><span data-stu-id="9b066-185">object</span></span> |<span data-ttu-id="9b066-186">Hlavičky odpovědi</span><span class="sxs-lookup"><span data-stu-id="9b066-186">Response headers</span></span> |
| <span data-ttu-id="9b066-187">Tělo</span><span class="sxs-lookup"><span data-stu-id="9b066-187">Body</span></span> |<span data-ttu-id="9b066-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="9b066-188">object</span></span> |<span data-ttu-id="9b066-189">Objekt odpovědi</span><span class="sxs-lookup"><span data-stu-id="9b066-189">Response object</span></span> |
| <span data-ttu-id="9b066-190">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="9b066-190">Status Code</span></span> |<span data-ttu-id="9b066-191">celá čísla</span><span class="sxs-lookup"><span data-stu-id="9b066-191">int</span></span> |<span data-ttu-id="9b066-192">Stavový kód protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="9b066-192">HTTP status code</span></span> |

### <a name="http-responses"></a><span data-ttu-id="9b066-193">Odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="9b066-193">HTTP responses</span></span>
<span data-ttu-id="9b066-194">Při volání různé akce, může být určité odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9b066-194">When making calls to various actions, you might get certain responses.</span></span> <span data-ttu-id="9b066-195">Následuje tabulka, která popisuje odpovídající odpovědi a popisy.</span><span class="sxs-lookup"><span data-stu-id="9b066-195">Following is a table that outlines corresponding responses and descriptions.</span></span>

| <span data-ttu-id="9b066-196">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9b066-196">Name</span></span> | <span data-ttu-id="9b066-197">Popis</span><span class="sxs-lookup"><span data-stu-id="9b066-197">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9b066-198">200</span><span class="sxs-lookup"><span data-stu-id="9b066-198">200</span></span> |<span data-ttu-id="9b066-199">OK</span><span class="sxs-lookup"><span data-stu-id="9b066-199">OK</span></span> |
| <span data-ttu-id="9b066-200">202</span><span class="sxs-lookup"><span data-stu-id="9b066-200">202</span></span> |<span data-ttu-id="9b066-201">Přijmout</span><span class="sxs-lookup"><span data-stu-id="9b066-201">Accepted</span></span> |
| <span data-ttu-id="9b066-202">400</span><span class="sxs-lookup"><span data-stu-id="9b066-202">400</span></span> |<span data-ttu-id="9b066-203">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="9b066-203">Bad request</span></span> |
| <span data-ttu-id="9b066-204">401</span><span class="sxs-lookup"><span data-stu-id="9b066-204">401</span></span> |<span data-ttu-id="9b066-205">Neautorizováno</span><span class="sxs-lookup"><span data-stu-id="9b066-205">Unauthorized</span></span> |
| <span data-ttu-id="9b066-206">403</span><span class="sxs-lookup"><span data-stu-id="9b066-206">403</span></span> |<span data-ttu-id="9b066-207">Je zakázané</span><span class="sxs-lookup"><span data-stu-id="9b066-207">Forbidden</span></span> |
| <span data-ttu-id="9b066-208">404</span><span class="sxs-lookup"><span data-stu-id="9b066-208">404</span></span> |<span data-ttu-id="9b066-209">Nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="9b066-209">Not Found</span></span> |
| <span data-ttu-id="9b066-210">500</span><span class="sxs-lookup"><span data-stu-id="9b066-210">500</span></span> |<span data-ttu-id="9b066-211">Vnitřní chybu serveru.</span><span class="sxs-lookup"><span data-stu-id="9b066-211">Internal server error.</span></span> <span data-ttu-id="9b066-212">Došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="9b066-212">Unknown error occurred.</span></span> |

- - -
## <a name="next-steps"></a><span data-ttu-id="9b066-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b066-213">Next steps</span></span>

* [<span data-ttu-id="9b066-214">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="9b066-214">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)
* [<span data-ttu-id="9b066-215">Vyhledání jiných konektorů</span><span class="sxs-lookup"><span data-stu-id="9b066-215">Find other connectors</span></span>](apis-list.md)