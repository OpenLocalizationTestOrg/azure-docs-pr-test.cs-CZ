---
title: "kódy chyb aaaAzure Media Services | Microsoft Docs"
description: "Hello téma nabízí přehled kódů chyb Azure Media Services."
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
ms.openlocfilehash: de1ffd6dee8901a3051eb5032536c3669482d6b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-error-codes"></a><span data-ttu-id="57a64-103">Kódy chyb Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="57a64-103">Azure Media Services error codes</span></span>
<span data-ttu-id="57a64-104">Při použití Microsoft Azure Media Services, může se zobrazit kódy chyb protokolu HTTP z hello služby v závislosti na problémy, jako je například ověřování tokenů vypršení platnosti tooactions, které nejsou podporovány ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="57a64-104">When using Microsoft Azure Media Services, you may receive HTTP error codes from hello service depending on issues such as authentication tokens expiring tooactions that are not supported in Media Services.</span></span> <span data-ttu-id="57a64-105">Hello tady je seznam **kódy chyb HTTP** , mohou být vráceny Media Services a hello možné způsobí, že pro ně.</span><span class="sxs-lookup"><span data-stu-id="57a64-105">hello following is a list of **HTTP error codes** that may be returned by Media Services and hello possible causes for them.</span></span>  

## <a name="400-bad-request"></a><span data-ttu-id="57a64-106">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="57a64-106">400 Bad Request</span></span>
<span data-ttu-id="57a64-107">žádost o Hello obsahuje neplatné informace a se odmítne kvůli tooone hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="57a64-107">hello request contains invalid information and is rejected due tooone of hello following reasons:</span></span>

* <span data-ttu-id="57a64-108">Je zadána Nepodporovaná verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="57a64-108">An unsupported API version is specified.</span></span> <span data-ttu-id="57a64-109">Nejaktuálnější verzi hello, najdete v části [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="57a64-109">For hello most current version, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
* <span data-ttu-id="57a64-110">není určená verze rozhraní API Hello Media Services.</span><span class="sxs-lookup"><span data-stu-id="57a64-110">hello API version of Media Services is not specified.</span></span> <span data-ttu-id="57a64-111">Informace o tom, jak toospecify hello verze rozhraní API najdete v tématu [média referenční dokumentace rozhraní API REST služby Operations](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).</span><span class="sxs-lookup"><span data-stu-id="57a64-111">For information on how toospecify hello API version, see [Media Services Operations REST API Reference](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="57a64-112">Pokud používáte tooconnect tooMedia hello .NET nebo sady Java SDK služby, hello verze rozhraní API je zadán pro vás vždy, když akci a provedení několika akcí proti Media Services.</span><span class="sxs-lookup"><span data-stu-id="57a64-112">If you are using hello .NET or Java SDKs tooconnect tooMedia Services, hello API version is specified for you whenever you try and perform some action against Media Services.</span></span>
  > 
  > 
* <span data-ttu-id="57a64-113">Nedefinovaná vlastnost nebyla zadána.</span><span class="sxs-lookup"><span data-stu-id="57a64-113">An undefined property has been specified.</span></span> <span data-ttu-id="57a64-114">Název vlastnosti Hello je v chybové zprávě hello.</span><span class="sxs-lookup"><span data-stu-id="57a64-114">hello property name is in hello error message.</span></span> <span data-ttu-id="57a64-115">Lze zadat pouze vlastnosti, které jsou členy dané entity.</span><span class="sxs-lookup"><span data-stu-id="57a64-115">Only those properties that are members of a given entity can be specified.</span></span> <span data-ttu-id="57a64-116">V tématu [Azure Media Services REST API – referenční informace](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) seznam entity a jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="57a64-116">See [Azure Media Services REST API Reference](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) for a list of entities and their properties.</span></span>
* <span data-ttu-id="57a64-117">Byla zadána neplatnou hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="57a64-117">An invalid property value has been specified.</span></span> <span data-ttu-id="57a64-118">Název vlastnosti Hello je v chybové zprávě hello.</span><span class="sxs-lookup"><span data-stu-id="57a64-118">hello property name is in hello error message.</span></span> <span data-ttu-id="57a64-119">Zobrazit hello předchozí odkaz pro typy platný vlastností a jejich hodnoty.</span><span class="sxs-lookup"><span data-stu-id="57a64-119">See hello previous link for valid property types and their values.</span></span>
* <span data-ttu-id="57a64-120">Hodnota vlastnosti je chybějící a musí se.</span><span class="sxs-lookup"><span data-stu-id="57a64-120">A property value is missing and is required.</span></span>
* <span data-ttu-id="57a64-121">Součást hello adresa URL obsahuje neplatná hodnota.</span><span class="sxs-lookup"><span data-stu-id="57a64-121">Part of hello URL specified contains a bad value.</span></span>
* <span data-ttu-id="57a64-122">Pokus o byl proveden tooupdate WriteOnce vlastnost.</span><span class="sxs-lookup"><span data-stu-id="57a64-122">An attempt was made tooupdate a WriteOnce property.</span></span>
* <span data-ttu-id="57a64-123">Pokus o byl proveden toocreate úlohu, která má vstupní Asset primární AssetFile, který nebyl zadán nebo nebylo možné určit.</span><span class="sxs-lookup"><span data-stu-id="57a64-123">An attempt was made toocreate a Job that has an input Asset with a primary AssetFile that was not specified or could not be determined.</span></span>
* <span data-ttu-id="57a64-124">Pokus o byl proveden tooupdate lokátoru SAS.</span><span class="sxs-lookup"><span data-stu-id="57a64-124">An attempt was made tooupdate a SAS Locator.</span></span> <span data-ttu-id="57a64-125">Lokátory SAS můžou jenom vytvořit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="57a64-125">SAS locators can only be created or deleted.</span></span> <span data-ttu-id="57a64-126">Lokátory streamování lze aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="57a64-126">Streaming locators can be updated.</span></span> <span data-ttu-id="57a64-127">Další informace najdete v tématu [lokátory](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="57a64-127">For more information, see [Locators](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>
* <span data-ttu-id="57a64-128">Nepodporovanou operaci nebo dotaz byla odeslána.</span><span class="sxs-lookup"><span data-stu-id="57a64-128">An unsupported operation or query was submitted.</span></span>

## <a name="401-unauthorized"></a><span data-ttu-id="57a64-129">401 unauthorized</span><span class="sxs-lookup"><span data-stu-id="57a64-129">401 Unauthorized</span></span>
<span data-ttu-id="57a64-130">Hello žádost nešlo ověřit (dříve, než lze udělit oprávnění) kvůli tooone hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="57a64-130">hello request could not be authenticated (before it can be authorized) due tooone of hello following reasons:</span></span>

* <span data-ttu-id="57a64-131">Chybí záhlaví ověřování.</span><span class="sxs-lookup"><span data-stu-id="57a64-131">Missing authentication header.</span></span>
* <span data-ttu-id="57a64-132">Hodnota hlavičky chybný ověřování.</span><span class="sxs-lookup"><span data-stu-id="57a64-132">Bad authentication header value.</span></span>
  * <span data-ttu-id="57a64-133">vypršela platnost tokenu Hello.</span><span class="sxs-lookup"><span data-stu-id="57a64-133">hello token has expired.</span></span> 
  * <span data-ttu-id="57a64-134">Hello token obsahuje neplatný podpis.</span><span class="sxs-lookup"><span data-stu-id="57a64-134">hello token contains an invalid signature.</span></span>

## <a name="403-forbidden"></a><span data-ttu-id="57a64-135">403 Zakázáno</span><span class="sxs-lookup"><span data-stu-id="57a64-135">403 Forbidden</span></span>
<span data-ttu-id="57a64-136">Hello žádost není povolena kvůli tooone hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="57a64-136">hello request is not allowed due tooone of hello following reasons:</span></span>

* <span data-ttu-id="57a64-137">Hello účtu Media Services nebyl nalezen nebo je odstraněný.</span><span class="sxs-lookup"><span data-stu-id="57a64-137">hello Media Services account cannot be found or has been deleted.</span></span>
* <span data-ttu-id="57a64-138">je zakázané Hello účtu Media Services a není typu hello požadavku HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="57a64-138">hello Media Services account is disabled and hello request type is not HTTP GET.</span></span> <span data-ttu-id="57a64-139">Operace služby vrátí 403 odpověď.</span><span class="sxs-lookup"><span data-stu-id="57a64-139">Service operations will return a 403 response as well.</span></span>
* <span data-ttu-id="57a64-140">Hello ověřovací token neobsahuje informace o pověření uživatelů hello: název účtu nebo ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="57a64-140">hello authentication token does not contain hello user’s credential information: AccountName and/or SubscriptionId.</span></span> <span data-ttu-id="57a64-141">Tyto informace najdete v hello rozšíření Media Services uživatelského rozhraní pro váš účet Media Services v hello portálu pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="57a64-141">You can find this information in hello Media Services UI extension for your Media Services account in hello Azure Management Portal.</span></span>
* <span data-ttu-id="57a64-142">Hello prostředků není přístupný.</span><span class="sxs-lookup"><span data-stu-id="57a64-142">hello resource cannot be accessed.</span></span>
  
  * <span data-ttu-id="57a64-143">Pokus o byl proveden toouse MediaProcessor, který není k dispozici pro váš účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="57a64-143">An attempt was made toouse a MediaProcessor that is not available for your Media Services account.</span></span>
  * <span data-ttu-id="57a64-144">Byl proveden pokus o tooupdate JobTemplate definované službou Media Services.</span><span class="sxs-lookup"><span data-stu-id="57a64-144">An attempt was made tooupdate a JobTemplate defined by Media Services.</span></span>
  * <span data-ttu-id="57a64-145">Pokus o byl provedené toooverwrite některé jiné účtu Media Services je lokátoru.</span><span class="sxs-lookup"><span data-stu-id="57a64-145">An attempt was made toooverwrite some other Media Services account's Locator.</span></span>
  * <span data-ttu-id="57a64-146">Pokus o byl provedené toooverwrite ContentKey některé jiné Media Services účtu.</span><span class="sxs-lookup"><span data-stu-id="57a64-146">An attempt was made toooverwrite some other Media Services account's ContentKey.</span></span>
* <span data-ttu-id="57a64-147">Nelze vytvořit prostředek Hello kvůli tooa služby kvótu, kterou bylo dosaženo pro účet Media Services hello.</span><span class="sxs-lookup"><span data-stu-id="57a64-147">hello resource could not be created due tooa service quota that was reached for hello Media Services account.</span></span> <span data-ttu-id="57a64-148">Další informace o hello služby kvóty, najdete v části [kvóty a omezení](media-services-quotas-and-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="57a64-148">For more information on hello service quotas, see [Quotas and Limitations](media-services-quotas-and-limitations.md).</span></span>

## <a name="404-not-found"></a><span data-ttu-id="57a64-149">404 – Nenalezeno</span><span class="sxs-lookup"><span data-stu-id="57a64-149">404 Not Found</span></span>
<span data-ttu-id="57a64-150">Hello požadavku není povolená na prostředku kvůli tooone hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="57a64-150">hello request is not allowed on a resource due tooone of hello following reasons:</span></span>

* <span data-ttu-id="57a64-151">Pokus o byl proveden tooupdate entita, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="57a64-151">An attempt was made tooupdate an entity that does not exist.</span></span>
* <span data-ttu-id="57a64-152">Pokus o byl proveden toodelete entita, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="57a64-152">An attempt was made toodelete an entity that does not exist.</span></span>
* <span data-ttu-id="57a64-153">Pokus o byl proveden toocreate entita, která propojí tooan entity, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="57a64-153">An attempt was made toocreate an entity that links tooan entity that does not exist.</span></span>
* <span data-ttu-id="57a64-154">Pokus o byl proveden tooGET entita, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="57a64-154">An attempt was made tooGET an entity that does not exist.</span></span>
* <span data-ttu-id="57a64-155">Pokus o byl proveden toospecify účet úložiště, který není spojen s hello účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="57a64-155">An attempt was made toospecify a storage account that is not associated with hello Media Services account.</span></span>  

## <a name="409-conflict"></a><span data-ttu-id="57a64-156">409 – konflikt</span><span class="sxs-lookup"><span data-stu-id="57a64-156">409 Conflict</span></span>
<span data-ttu-id="57a64-157">Hello žádost není povolena kvůli tooone hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="57a64-157">hello request is not allowed due tooone of hello following reasons:</span></span>

* <span data-ttu-id="57a64-158">Více než jeden AssetFile má zadaný název hello v rámci hello Asset.</span><span class="sxs-lookup"><span data-stu-id="57a64-158">More than one AssetFile has hello specified name within hello Asset.</span></span>
* <span data-ttu-id="57a64-159">Byl proveden pokus toocreate a sekundu primární AssetFile v rámci hello Asset.</span><span class="sxs-lookup"><span data-stu-id="57a64-159">An attempt was made toocreate a second primary AssetFile within hello Asset.</span></span>
* <span data-ttu-id="57a64-160">Byl proveden pokus o toocreate ContentKey s hello zadané Id již používá.</span><span class="sxs-lookup"><span data-stu-id="57a64-160">An attempt was made toocreate a ContentKey with hello specified Id already used.</span></span>
* <span data-ttu-id="57a64-161">Byl proveden pokus o toocreate lokátoru s hello zadané Id již používá.</span><span class="sxs-lookup"><span data-stu-id="57a64-161">An attempt was made toocreate a Locator with hello specified Id already used.</span></span>
* <span data-ttu-id="57a64-162">Více než jeden IngestManifestFile má zadaný název hello v rámci hello IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="57a64-162">More than one IngestManifestFile has hello specified name within hello IngestManifest.</span></span>
* <span data-ttu-id="57a64-163">Pokus o byl proveden toolink druhý toohello ContentKey šifrování úložiště Asset šifrovat úložiště.</span><span class="sxs-lookup"><span data-stu-id="57a64-163">An attempt was made toolink a second storage encryption ContentKey toohello storage-encrypted Asset.</span></span>
* <span data-ttu-id="57a64-164">Byl proveden pokus o toolink hello stejné ContentKey toohello Asset.</span><span class="sxs-lookup"><span data-stu-id="57a64-164">An attempt was made toolink hello same ContentKey toohello Asset.</span></span>
* <span data-ttu-id="57a64-165">Byl proveden toocreate Lokátor tooan Asset jejichž kontejner úložiště chybí nebo je už přidružený hello Asset.</span><span class="sxs-lookup"><span data-stu-id="57a64-165">An attempt was made toocreate a locator tooan Asset whose storage container is missing or is no longer associated with hello Asset.</span></span>
* <span data-ttu-id="57a64-166">Pokus o byl proveden toocreate Lokátor tooan Asset, který již má 5 lokátory používá.</span><span class="sxs-lookup"><span data-stu-id="57a64-166">An attempt was made toocreate a locator tooan Asset which already has 5 locators in use.</span></span> <span data-ttu-id="57a64-167">(Úložiště azure vynucuje omezení hello pět zásady sdíleného přístupu na jeden kontejner úložiště.)</span><span class="sxs-lookup"><span data-stu-id="57a64-167">(Azure Storage enforces hello limit of five shared access policies on one storage container.)</span></span>
* <span data-ttu-id="57a64-168">Propojení účtu úložiště Asset tooan IngestManifestAsset není hello stejný jako účet úložiště hello používá nadřazené hello IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="57a64-168">Linking storage account of an Asset tooan IngestManifestAsset is not hello same as hello storage account used by hello parent IngestManifest.</span></span>  

## <a name="500-internal-server-error"></a><span data-ttu-id="57a64-169">500 vnitřní chybu serveru</span><span class="sxs-lookup"><span data-stu-id="57a64-169">500 Internal Server Error</span></span>
<span data-ttu-id="57a64-170">Během zpracování požadavku hello hello zaznamená Media Services nějaká chyba, která zabraňuje hello zpracování pokračovat.</span><span class="sxs-lookup"><span data-stu-id="57a64-170">During hello processing of hello request, Media Services encounters some error that prevents hello processing from continuing.</span></span> <span data-ttu-id="57a64-171">To může být kvůli tooone hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="57a64-171">This could be due tooone of hello following reasons:</span></span>

* <span data-ttu-id="57a64-172">Vytvoření úlohy nebo Asset nezdaří, protože informace o kvótě účtu Media Services hello služba je dočasně nedostupná.</span><span class="sxs-lookup"><span data-stu-id="57a64-172">Creating an Asset or Job fails because hello Media Services account's service quota information is temporarily unavailable.</span></span>
* <span data-ttu-id="57a64-173">Vytvoření prostředku nebo IngestManifest kontejner úložiště objektů blob se nezdaří, protože informace o účtu úložiště hello účet je dočasně nedostupná.</span><span class="sxs-lookup"><span data-stu-id="57a64-173">Creating an Asset or IngestManifest blob storage container fails because hello account's storage account information is temporarily unavailable.</span></span>
* <span data-ttu-id="57a64-174">Další neočekávané chyby.</span><span class="sxs-lookup"><span data-stu-id="57a64-174">Other unexpected error.</span></span>

## <a name="503-service-unavailable"></a><span data-ttu-id="57a64-175">503 Služba nedostupná</span><span class="sxs-lookup"><span data-stu-id="57a64-175">503 Service Unavailable</span></span>
<span data-ttu-id="57a64-176">Hello server je momentálně nemůže tooreceive požadavky.</span><span class="sxs-lookup"><span data-stu-id="57a64-176">hello server is currently unable tooreceive requests.</span></span> <span data-ttu-id="57a64-177">Tato chyba může být způsobeno službou toohello nadměrné požadavky.</span><span class="sxs-lookup"><span data-stu-id="57a64-177">This error may be caused by excessive requests toohello service.</span></span> <span data-ttu-id="57a64-178">Služba Media Services omezení mechanismus omezuje hello využití prostředků pro aplikace, které služby toohello nadměrné požadavku.</span><span class="sxs-lookup"><span data-stu-id="57a64-178">Media Services throttling mechanism restricts hello resource usage for applications that make excessive request toohello service.</span></span>

> [!NOTE]
> <span data-ttu-id="57a64-179">Kontrola hello chyba zprávu a chyba kód řetězec tooget podrobnější informace o hello důvod, které jste získali hello 503 chyby.</span><span class="sxs-lookup"><span data-stu-id="57a64-179">Check hello error message and error code string tooget more detailed information about hello reason you got hello 503 error.</span></span> <span data-ttu-id="57a64-180">Tato chyba vždy neznamená omezení.</span><span class="sxs-lookup"><span data-stu-id="57a64-180">This error does not always mean throttling.</span></span>
> 
> 

<span data-ttu-id="57a64-181">Možné stav popisy jsou:</span><span class="sxs-lookup"><span data-stu-id="57a64-181">Possible status descriptions are:</span></span>

* <span data-ttu-id="57a64-182">"Server je zaneprázdněn.</span><span class="sxs-lookup"><span data-stu-id="57a64-182">"Server is busy.</span></span> <span data-ttu-id="57a64-183">Předchozí spustí tento typ požadavku trvalo víc než {0} sekund."</span><span class="sxs-lookup"><span data-stu-id="57a64-183">Previous runs of this type of request took more than {0} seconds."</span></span>
* <span data-ttu-id="57a64-184">"Server je zaneprázdněn.</span><span class="sxs-lookup"><span data-stu-id="57a64-184">"Server is busy.</span></span> <span data-ttu-id="57a64-185">Více než {0} požadavků za sekundu může být omezena."</span><span class="sxs-lookup"><span data-stu-id="57a64-185">More than {0} requests per second can be throttled."</span></span>
* <span data-ttu-id="57a64-186">"Server je zaneprázdněn.</span><span class="sxs-lookup"><span data-stu-id="57a64-186">"Server is busy.</span></span> <span data-ttu-id="57a64-187">Více než {0} požadavky během několika sekund {1} může být omezena."</span><span class="sxs-lookup"><span data-stu-id="57a64-187">More than {0} requests within {1} seconds can be throttled."</span></span>

<span data-ttu-id="57a64-188">toohandle této chybě, doporučujeme použít logiku exponenciální opakování back vypnout.</span><span class="sxs-lookup"><span data-stu-id="57a64-188">toohandle this error, we recommend using exponential back-off retry logic.</span></span> <span data-ttu-id="57a64-189">To znamená, pomocí progresivně delší čeká mezi jednotlivými pokusy o po sobě jdoucích chybové odpovědi.</span><span class="sxs-lookup"><span data-stu-id="57a64-189">That means using progressively longer waits between retries for consecutive error responses.</span></span>  <span data-ttu-id="57a64-190">Další informace najdete v tématu [přechodné chyby zpracování bloku aplikace](https://msdn.microsoft.com/library/hh680905.aspx).</span><span class="sxs-lookup"><span data-stu-id="57a64-190">For more information, see [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/hh680905.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="57a64-191">Pokud používáte [Azure Media Services SDK pro .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), byl implementován hello logika opakovaných pokusů pro chybu hello 503 ve hello SDK.</span><span class="sxs-lookup"><span data-stu-id="57a64-191">If you are using [Azure Media Services SDK for .Net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), hello retry logic for hello 503 error has been implemented by hello SDK.</span></span>  
> 
> 

## <a name="see-also"></a><span data-ttu-id="57a64-192">Viz také</span><span class="sxs-lookup"><span data-stu-id="57a64-192">See Also</span></span>
[<span data-ttu-id="57a64-193">Kódy chyb správy služby Media Services</span><span class="sxs-lookup"><span data-stu-id="57a64-193">Media Services Management Error Codes</span></span>](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a><span data-ttu-id="57a64-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57a64-194">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="57a64-195">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="57a64-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

