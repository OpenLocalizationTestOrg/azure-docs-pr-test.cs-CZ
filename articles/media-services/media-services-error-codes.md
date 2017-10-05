---
title: "Kódy chyb Azure Media Services | Microsoft Docs"
description: "Téma nabízí přehled kódů chyb Azure Media Services."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d3a62a64-7608-4b17-8667-479b26ba0d6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 39886a955124429302609dd9d5a7c20ae7f498d9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-media-services-error-codes"></a><span data-ttu-id="ec850-103">Kódy chyb Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="ec850-103">Azure Media Services error codes</span></span>
<span data-ttu-id="ec850-104">Při použití Microsoft Azure Media Services, může se zobrazit kódy chyb HTTP ze služby v závislosti na problémy, jako je například ověřování tokenů vypršení platnosti na akce, které nejsou podporovány ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="ec850-104">When using Microsoft Azure Media Services, you may receive HTTP error codes from the service depending on issues such as authentication tokens expiring to actions that are not supported in Media Services.</span></span> <span data-ttu-id="ec850-105">Následuje seznam **kódy chyb HTTP** , mohou být vráceny Media Services a možné příčiny pro ně.</span><span class="sxs-lookup"><span data-stu-id="ec850-105">The following is a list of **HTTP error codes** that may be returned by Media Services and the possible causes for them.</span></span>  

## <a name="400-bad-request"></a><span data-ttu-id="ec850-106">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="ec850-106">400 Bad Request</span></span>
<span data-ttu-id="ec850-107">Žádost obsahuje neplatné informace a se odmítne kvůli jednomu z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="ec850-107">The request contains invalid information and is rejected due to one of the following reasons:</span></span>

* <span data-ttu-id="ec850-108">Je zadána Nepodporovaná verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ec850-108">An unsupported API version is specified.</span></span> <span data-ttu-id="ec850-109">Nejnovější verzi, naleznete v části [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="ec850-109">For the most current version, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
* <span data-ttu-id="ec850-110">Není určená verze rozhraní API služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="ec850-110">The API version of Media Services is not specified.</span></span> <span data-ttu-id="ec850-111">Informace o tom, jak určit verze rozhraní API najdete v tématu [média referenční dokumentace rozhraní API REST služby Operations](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).</span><span class="sxs-lookup"><span data-stu-id="ec850-111">For information on how to specify the API version, see [Media Services Operations REST API Reference](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="ec850-112">Pokud používáte k připojení ke službě Media Services .NET nebo sady Java SDK, verze rozhraní API je určena pro vás vždy, když akci a provedení několika akcí proti Media Services.</span><span class="sxs-lookup"><span data-stu-id="ec850-112">If you are using the .NET or Java SDKs to connect to Media Services, the API version is specified for you whenever you try and perform some action against Media Services.</span></span>
  > 
  > 
* <span data-ttu-id="ec850-113">Nedefinovaná vlastnost nebyla zadána.</span><span class="sxs-lookup"><span data-stu-id="ec850-113">An undefined property has been specified.</span></span> <span data-ttu-id="ec850-114">Název vlastnosti je v chybové zprávě.</span><span class="sxs-lookup"><span data-stu-id="ec850-114">The property name is in the error message.</span></span> <span data-ttu-id="ec850-115">Lze zadat pouze vlastnosti, které jsou členy dané entity.</span><span class="sxs-lookup"><span data-stu-id="ec850-115">Only those properties that are members of a given entity can be specified.</span></span> <span data-ttu-id="ec850-116">V tématu [Azure Media Services REST API – referenční informace](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) seznam entity a jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ec850-116">See [Azure Media Services REST API Reference](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) for a list of entities and their properties.</span></span>
* <span data-ttu-id="ec850-117">Byla zadána neplatnou hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ec850-117">An invalid property value has been specified.</span></span> <span data-ttu-id="ec850-118">Název vlastnosti je v chybové zprávě.</span><span class="sxs-lookup"><span data-stu-id="ec850-118">The property name is in the error message.</span></span> <span data-ttu-id="ec850-119">Najdete odkaz na předchozí platnou vlastnost typy a jejich hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ec850-119">See the previous link for valid property types and their values.</span></span>
* <span data-ttu-id="ec850-120">Hodnota vlastnosti je chybějící a musí se.</span><span class="sxs-lookup"><span data-stu-id="ec850-120">A property value is missing and is required.</span></span>
* <span data-ttu-id="ec850-121">Součást zadaná adresa URL obsahuje neplatná hodnota.</span><span class="sxs-lookup"><span data-stu-id="ec850-121">Part of the URL specified contains a bad value.</span></span>
* <span data-ttu-id="ec850-122">Došlo pokusu o aktualizujte vlastnost WriteOnce.</span><span class="sxs-lookup"><span data-stu-id="ec850-122">An attempt was made to update a WriteOnce property.</span></span>
* <span data-ttu-id="ec850-123">Došlo pokusu vytvořit úlohu, která má vstupní Asset primární AssetFile, který nebyl zadán nebo nebylo možné určit.</span><span class="sxs-lookup"><span data-stu-id="ec850-123">An attempt was made to create a Job that has an input Asset with a primary AssetFile that was not specified or could not be determined.</span></span>
* <span data-ttu-id="ec850-124">Došlo pokusu o aktualizaci lokátoru SAS.</span><span class="sxs-lookup"><span data-stu-id="ec850-124">An attempt was made to update a SAS Locator.</span></span> <span data-ttu-id="ec850-125">Lokátory SAS můžou jenom vytvořit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="ec850-125">SAS locators can only be created or deleted.</span></span> <span data-ttu-id="ec850-126">Lokátory streamování lze aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ec850-126">Streaming locators can be updated.</span></span> <span data-ttu-id="ec850-127">Další informace najdete v tématu [lokátory](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="ec850-127">For more information, see [Locators](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>
* <span data-ttu-id="ec850-128">Nepodporovanou operaci nebo dotaz byla odeslána.</span><span class="sxs-lookup"><span data-stu-id="ec850-128">An unsupported operation or query was submitted.</span></span>

## <a name="401-unauthorized"></a><span data-ttu-id="ec850-129">401 unauthorized</span><span class="sxs-lookup"><span data-stu-id="ec850-129">401 Unauthorized</span></span>
<span data-ttu-id="ec850-130">Žádost nešlo ověřit (dříve, než lze udělit oprávnění) kvůli jednomu z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="ec850-130">The request could not be authenticated (before it can be authorized) due to one of the following reasons:</span></span>

* <span data-ttu-id="ec850-131">Chybí záhlaví ověřování.</span><span class="sxs-lookup"><span data-stu-id="ec850-131">Missing authentication header.</span></span>
* <span data-ttu-id="ec850-132">Hodnota hlavičky chybný ověřování.</span><span class="sxs-lookup"><span data-stu-id="ec850-132">Bad authentication header value.</span></span>
  * <span data-ttu-id="ec850-133">Platnost tokenu vypršela.</span><span class="sxs-lookup"><span data-stu-id="ec850-133">The token has expired.</span></span> 
  * <span data-ttu-id="ec850-134">Token obsahuje neplatný podpis.</span><span class="sxs-lookup"><span data-stu-id="ec850-134">The token contains an invalid signature.</span></span>

## <a name="403-forbidden"></a><span data-ttu-id="ec850-135">403 Zakázáno</span><span class="sxs-lookup"><span data-stu-id="ec850-135">403 Forbidden</span></span>
<span data-ttu-id="ec850-136">Požadavek není povoleno z důvodu jednu z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="ec850-136">The request is not allowed due to one of the following reasons:</span></span>

* <span data-ttu-id="ec850-137">Účet Media Services nebyl nalezen nebo je odstraněný.</span><span class="sxs-lookup"><span data-stu-id="ec850-137">The Media Services account cannot be found or has been deleted.</span></span>
* <span data-ttu-id="ec850-138">Účet Media Services je zakázán a není typ požadavku HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="ec850-138">The Media Services account is disabled and the request type is not HTTP GET.</span></span> <span data-ttu-id="ec850-139">Operace služby vrátí 403 odpověď.</span><span class="sxs-lookup"><span data-stu-id="ec850-139">Service operations will return a 403 response as well.</span></span>
* <span data-ttu-id="ec850-140">Ověřovací token neobsahuje přihlašovací údaje uživatele: název účtu nebo ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="ec850-140">The authentication token does not contain the user’s credential information: AccountName and/or SubscriptionId.</span></span> <span data-ttu-id="ec850-141">Tyto informace najdete v rozšíření Media Services uživatelského rozhraní pro váš účet Media Services v Azure Management Portal.</span><span class="sxs-lookup"><span data-stu-id="ec850-141">You can find this information in the Media Services UI extension for your Media Services account in the Azure Management Portal.</span></span>
* <span data-ttu-id="ec850-142">Nelze získat přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="ec850-142">The resource cannot be accessed.</span></span>
  
  * <span data-ttu-id="ec850-143">Došlo pokusu o použití MediaProcessor, který není k dispozici pro váš účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="ec850-143">An attempt was made to use a MediaProcessor that is not available for your Media Services account.</span></span>
  * <span data-ttu-id="ec850-144">Byl proveden pokus o aktualizace JobTemplate definované Media Services.</span><span class="sxs-lookup"><span data-stu-id="ec850-144">An attempt was made to update a JobTemplate defined by Media Services.</span></span>
  * <span data-ttu-id="ec850-145">Byl proveden pokus o přepsat některé jiné účtu Media Services je lokátoru.</span><span class="sxs-lookup"><span data-stu-id="ec850-145">An attempt was made to overwrite some other Media Services account's Locator.</span></span>
  * <span data-ttu-id="ec850-146">Byl proveden pokus o přepsat ContentKey některé jiné Media Services účtu.</span><span class="sxs-lookup"><span data-stu-id="ec850-146">An attempt was made to overwrite some other Media Services account's ContentKey.</span></span>
* <span data-ttu-id="ec850-147">Prostředek nelze vytvořit z důvodu kvótu služby, bylo dosaženo pro účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="ec850-147">The resource could not be created due to a service quota that was reached for the Media Services account.</span></span> <span data-ttu-id="ec850-148">Další informace o kvót služby najdete v tématu [kvóty a omezení](media-services-quotas-and-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="ec850-148">For more information on the service quotas, see [Quotas and Limitations](media-services-quotas-and-limitations.md).</span></span>

## <a name="404-not-found"></a><span data-ttu-id="ec850-149">404 – Nenalezeno</span><span class="sxs-lookup"><span data-stu-id="ec850-149">404 Not Found</span></span>
<span data-ttu-id="ec850-150">Žádost není povolená na prostředku kvůli jednomu z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="ec850-150">The request is not allowed on a resource due to one of the following reasons:</span></span>

* <span data-ttu-id="ec850-151">Došlo pokusu o aktualizovat entitu, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ec850-151">An attempt was made to update an entity that does not exist.</span></span>
* <span data-ttu-id="ec850-152">Došlo pokusu o odstranění entity, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ec850-152">An attempt was made to delete an entity that does not exist.</span></span>
* <span data-ttu-id="ec850-153">Došlo pokusu o vytvoření entity, který odkazuje na entity, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ec850-153">An attempt was made to create an entity that links to an entity that does not exist.</span></span>
* <span data-ttu-id="ec850-154">Došlo k pokusu o NAČTENÍ entity, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ec850-154">An attempt was made to GET an entity that does not exist.</span></span>
* <span data-ttu-id="ec850-155">Došlo pokusu o zadejte účet úložiště, který není přidružen k účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="ec850-155">An attempt was made to specify a storage account that is not associated with the Media Services account.</span></span>  

## <a name="409-conflict"></a><span data-ttu-id="ec850-156">409 – konflikt</span><span class="sxs-lookup"><span data-stu-id="ec850-156">409 Conflict</span></span>
<span data-ttu-id="ec850-157">Požadavek není povoleno z důvodu jednu z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="ec850-157">The request is not allowed due to one of the following reasons:</span></span>

* <span data-ttu-id="ec850-158">Více než jeden AssetFile má zadaný název v rámci prostředku.</span><span class="sxs-lookup"><span data-stu-id="ec850-158">More than one AssetFile has the specified name within the Asset.</span></span>
* <span data-ttu-id="ec850-159">Byl proveden pokus o vytvoření druhého primární AssetFile v rámci prostředku.</span><span class="sxs-lookup"><span data-stu-id="ec850-159">An attempt was made to create a second primary AssetFile within the Asset.</span></span>
* <span data-ttu-id="ec850-160">Došlo k pokusu o vytvoření ContentKey se zadaným Id už používá.</span><span class="sxs-lookup"><span data-stu-id="ec850-160">An attempt was made to create a ContentKey with the specified Id already used.</span></span>
* <span data-ttu-id="ec850-161">Došlo pokusu vytvořit lokátor se zadaným Id už používá.</span><span class="sxs-lookup"><span data-stu-id="ec850-161">An attempt was made to create a Locator with the specified Id already used.</span></span>
* <span data-ttu-id="ec850-162">Více než jeden IngestManifestFile má zadaný název v rámci IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="ec850-162">More than one IngestManifestFile has the specified name within the IngestManifest.</span></span>
* <span data-ttu-id="ec850-163">Došlo pokusu o druhý šifrování úložiště ContentKey propojit Asset šifrovat úložiště.</span><span class="sxs-lookup"><span data-stu-id="ec850-163">An attempt was made to link a second storage encryption ContentKey to the storage-encrypted Asset.</span></span>
* <span data-ttu-id="ec850-164">Byl proveden pokus o propojení stejné ContentKey pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="ec850-164">An attempt was made to link the same ContentKey to the Asset.</span></span>
* <span data-ttu-id="ec850-165">Došlo pokusu vytvořit lokátor pro prostředek, jehož kontejner úložiště chybí nebo je už přidružený Asset.</span><span class="sxs-lookup"><span data-stu-id="ec850-165">An attempt was made to create a locator to an Asset whose storage container is missing or is no longer associated with the Asset.</span></span>
* <span data-ttu-id="ec850-166">Došlo pokusu vytvořit lokátor pro prostředek, který již má 5 lokátory používá.</span><span class="sxs-lookup"><span data-stu-id="ec850-166">An attempt was made to create a locator to an Asset which already has 5 locators in use.</span></span> <span data-ttu-id="ec850-167">(Úložiště azure vynucuje omezení pět zásady sdíleného přístupu na jeden kontejner úložiště.)</span><span class="sxs-lookup"><span data-stu-id="ec850-167">(Azure Storage enforces the limit of five shared access policies on one storage container.)</span></span>
* <span data-ttu-id="ec850-168">Propojování účtu úložiště prostředek IngestManifestAsset není stejný jako účet úložiště využívá nadřízený objekt IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="ec850-168">Linking storage account of an Asset to an IngestManifestAsset is not the same as the storage account used by the parent IngestManifest.</span></span>  

## <a name="500-internal-server-error"></a><span data-ttu-id="ec850-169">500 vnitřní chybu serveru</span><span class="sxs-lookup"><span data-stu-id="ec850-169">500 Internal Server Error</span></span>
<span data-ttu-id="ec850-170">Během zpracování požadavku, zaznamená Media Services nějaká chyba, která zabraňuje zpracování pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ec850-170">During the processing of the request, Media Services encounters some error that prevents the processing from continuing.</span></span> <span data-ttu-id="ec850-171">Může to být způsobené jedním z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="ec850-171">This could be due to one of the following reasons:</span></span>

* <span data-ttu-id="ec850-172">Vytvoření úlohy nebo Asset nezdaří, protože informace o kvótě účtu Media Services služba je dočasně nedostupná.</span><span class="sxs-lookup"><span data-stu-id="ec850-172">Creating an Asset or Job fails because the Media Services account's service quota information is temporarily unavailable.</span></span>
* <span data-ttu-id="ec850-173">Vytvoření prostředku nebo IngestManifest kontejner úložiště objektů blob se nezdaří, protože informace o účtu úložiště na účet je dočasně nedostupná.</span><span class="sxs-lookup"><span data-stu-id="ec850-173">Creating an Asset or IngestManifest blob storage container fails because the account's storage account information is temporarily unavailable.</span></span>
* <span data-ttu-id="ec850-174">Další neočekávané chyby.</span><span class="sxs-lookup"><span data-stu-id="ec850-174">Other unexpected error.</span></span>

## <a name="503-service-unavailable"></a><span data-ttu-id="ec850-175">503 Služba nedostupná</span><span class="sxs-lookup"><span data-stu-id="ec850-175">503 Service Unavailable</span></span>
<span data-ttu-id="ec850-176">Server je nyní nelze přijmout požadavky.</span><span class="sxs-lookup"><span data-stu-id="ec850-176">The server is currently unable to receive requests.</span></span> <span data-ttu-id="ec850-177">Tato chyba může být způsobeno nadměrné požadavky na službu.</span><span class="sxs-lookup"><span data-stu-id="ec850-177">This error may be caused by excessive requests to the service.</span></span> <span data-ttu-id="ec850-178">Služba Media Services omezení mechanismus omezuje využití prostředků pro aplikace, které provést nadměrné požadavek na službu.</span><span class="sxs-lookup"><span data-stu-id="ec850-178">Media Services throttling mechanism restricts the resource usage for applications that make excessive request to the service.</span></span>

> [!NOTE]
> <span data-ttu-id="ec850-179">Zkontrolujte chybovou zprávou a řetězec kódu chyby. Chcete-li získat podrobnější informace o příčině, že jste získali 503 chyby.</span><span class="sxs-lookup"><span data-stu-id="ec850-179">Check the error message and error code string to get more detailed information about the reason you got the 503 error.</span></span> <span data-ttu-id="ec850-180">Tato chyba vždy neznamená omezení.</span><span class="sxs-lookup"><span data-stu-id="ec850-180">This error does not always mean throttling.</span></span>
> 
> 

<span data-ttu-id="ec850-181">Možné stav popisy jsou:</span><span class="sxs-lookup"><span data-stu-id="ec850-181">Possible status descriptions are:</span></span>

* <span data-ttu-id="ec850-182">"Server je zaneprázdněn.</span><span class="sxs-lookup"><span data-stu-id="ec850-182">"Server is busy.</span></span> <span data-ttu-id="ec850-183">Předchozí spustí tento typ požadavku trvalo víc než {0} sekund."</span><span class="sxs-lookup"><span data-stu-id="ec850-183">Previous runs of this type of request took more than {0} seconds."</span></span>
* <span data-ttu-id="ec850-184">"Server je zaneprázdněn.</span><span class="sxs-lookup"><span data-stu-id="ec850-184">"Server is busy.</span></span> <span data-ttu-id="ec850-185">Více než {0} požadavků za sekundu může být omezena."</span><span class="sxs-lookup"><span data-stu-id="ec850-185">More than {0} requests per second can be throttled."</span></span>
* <span data-ttu-id="ec850-186">"Server je zaneprázdněn.</span><span class="sxs-lookup"><span data-stu-id="ec850-186">"Server is busy.</span></span> <span data-ttu-id="ec850-187">Více než {0} požadavky během několika sekund {1} může být omezena."</span><span class="sxs-lookup"><span data-stu-id="ec850-187">More than {0} requests within {1} seconds can be throttled."</span></span>

<span data-ttu-id="ec850-188">Pro zpracování této chybě, doporučujeme používat logiku exponenciální opakování back vypnout.</span><span class="sxs-lookup"><span data-stu-id="ec850-188">To handle this error, we recommend using exponential back-off retry logic.</span></span> <span data-ttu-id="ec850-189">To znamená, pomocí progresivně delší čeká mezi jednotlivými pokusy o po sobě jdoucích chybové odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ec850-189">That means using progressively longer waits between retries for consecutive error responses.</span></span>  <span data-ttu-id="ec850-190">Další informace najdete v tématu [přechodné chyby zpracování bloku aplikace](https://msdn.microsoft.com/library/hh680905.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec850-190">For more information, see [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/hh680905.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="ec850-191">Pokud používáte [Azure Media Services SDK pro .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), logiku opakovaných pokusů pro chyby 503 nebyla implementovaná sadou SDK.</span><span class="sxs-lookup"><span data-stu-id="ec850-191">If you are using [Azure Media Services SDK for .Net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), the retry logic for the 503 error has been implemented by the SDK.</span></span>  
> 
> 

## <a name="see-also"></a><span data-ttu-id="ec850-192">Viz také</span><span class="sxs-lookup"><span data-stu-id="ec850-192">See Also</span></span>
[<span data-ttu-id="ec850-193">Kódy chyb správy služby Media Services</span><span class="sxs-lookup"><span data-stu-id="ec850-193">Media Services Management Error Codes</span></span>](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a><span data-ttu-id="ec850-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec850-194">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ec850-195">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="ec850-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

