---
title: "Přehled rozhraní REST API služby Media Services operations | Microsoft Docs"
description: "Přehled Media Services REST API"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: eada8f2bcd2488d5ed36b0c24aa3d1b917815517
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="media-services-operations-rest-api-overview"></a><span data-ttu-id="f3e5c-103">Přehled rozhraní REST API operations Media Services</span><span class="sxs-lookup"><span data-stu-id="f3e5c-103">Media Services operations REST API overview</span></span>
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

<span data-ttu-id="f3e5c-104">**Media Services operace REST** rozhraní API slouží k vytvoření úlohy, prostředky, zásady přístupu a další operace na objekty v účtu služby Media Service.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-104">The **Media Services Operations REST** API is used for creating jobs, assets, access policies, and other operations on objects in a Media Service account.</span></span> <span data-ttu-id="f3e5c-105">Další informace najdete v tématu [Media Services operace REST API odkaz](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).</span><span class="sxs-lookup"><span data-stu-id="f3e5c-105">For more information, see [Media Services Operations REST API reference](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).</span></span>

<span data-ttu-id="f3e5c-106">Microsoft Azure Media Services je služba, která přijímá požadavky HTTP na základě OData a zpátky v podrobné JSON nebo atom + pub může reagovat.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-106">Microsoft Azure Media Services is a service that accepts OData-based HTTP requests and can respond back in verbose JSON or atom+pub.</span></span> <span data-ttu-id="f3e5c-107">Vzhledem k tomu, že vyhovuje pokynů pro návrh Azure Media Services, je sada požadované hlavičky HTTP, které každý klient musí použít při připojování ke službě Media Services, jakož i sadu volitelné hlavičky, které lze použít.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-107">Because Media Services conforms to Azure design guidelines, there is a set of required HTTP headers that each client must use when connecting to Media Services, as well as a set of optional headers that can be used.</span></span> <span data-ttu-id="f3e5c-108">Následující části popisují hlavičky a příkazy HTTP, můžete použít při vytváření žádostí o a příjem odpovědí ze služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-108">The following sections describe the headers and HTTP verbs you can use when creating requests and receiving responses from Media Services.</span></span>

<span data-ttu-id="f3e5c-109">Toto téma poskytuje přehled o tom, jak používat REST v2 pomocí služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-109">This topic gives an overview of how to use REST v2 with Media Services.</span></span>

## <a name="considerations"></a><span data-ttu-id="f3e5c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f3e5c-110">Considerations</span></span>

<span data-ttu-id="f3e5c-111">Při používání REST, platí následující aspekty.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-111">The following considerations apply when using REST.</span></span>

* <span data-ttu-id="f3e5c-112">Při dotazování entity, existuje omezení 1000 entit vrátí najednou, protože veřejné v2 REST omezí výsledky dotazu a 1000 výsledky.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-112">When querying entities, there is a limit of 1000 entities returned at one time because public REST v2 limits query results to 1000 results.</span></span> <span data-ttu-id="f3e5c-113">Budete muset použít **přeskočit** a **trvat** (.NET) nebo **horní** (REST), jak je popsáno v [v tomto příkladu .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) a [v tomto příkladu REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities).</span><span class="sxs-lookup"><span data-stu-id="f3e5c-113">You need to use **Skip** and **Take** (.NET)/ **top** (REST) as described in [this .NET example](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) and [this REST API example](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities).</span></span> 
* <span data-ttu-id="f3e5c-114">Při použití JSON a určení pro použití **__metadata** – klíčové slovo v žádosti o (například k odkazuje na objekt odkazovaný) musíte nastavit **přijmout** hlavičky k [JSON podrobný formát](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (viz následující příklad).</span><span class="sxs-lookup"><span data-stu-id="f3e5c-114">When using JSON and specifying to use the **__metadata** keyword in the request (for example, to references a linked object) you MUST set the **Accept** header to [JSON Verbose format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (see the following example).</span></span> <span data-ttu-id="f3e5c-115">OData nerozumí **__metadata** vlastnost v požadavku, pokud je nastavena na podrobné.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-115">Odata does not understand the **__metadata** property in the request, unless you set it to verbose.</span></span>  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a><span data-ttu-id="f3e5c-116">Standardní hlavičky požadavku HTTP podporovaných službou Media Services</span><span class="sxs-lookup"><span data-stu-id="f3e5c-116">Standard HTTP request headers supported by Media Services</span></span>
<span data-ttu-id="f3e5c-117">Pro každé volání, které provedete ve službě Media Services je sada požadované hlavičky, které je nutné zahrnout vaši žádost a také sadu volitelné záhlaví může chcete zahrnout.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-117">For every call you make into Media Services, there is a set of required headers you must include in your request and also a set of optional headers you may want to include.</span></span> <span data-ttu-id="f3e5c-118">Následující tabulka uvádí požadované hlavičky:</span><span class="sxs-lookup"><span data-stu-id="f3e5c-118">The following table lists the required headers:</span></span>

| <span data-ttu-id="f3e5c-119">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="f3e5c-119">Header</span></span> | <span data-ttu-id="f3e5c-120">Typ</span><span class="sxs-lookup"><span data-stu-id="f3e5c-120">Type</span></span> | <span data-ttu-id="f3e5c-121">Hodnota</span><span class="sxs-lookup"><span data-stu-id="f3e5c-121">Value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f3e5c-122">Autorizace</span><span class="sxs-lookup"><span data-stu-id="f3e5c-122">Authorization</span></span> |<span data-ttu-id="f3e5c-123">Nosiče</span><span class="sxs-lookup"><span data-stu-id="f3e5c-123">Bearer</span></span> |<span data-ttu-id="f3e5c-124">Nosiče se jedná o jediné autorizační přijatý mechanismus.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-124">Bearer is the only accepted authorization mechanism.</span></span> <span data-ttu-id="f3e5c-125">Hodnota musí obsahovat také přístupový token poskytovaný službou ACS.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-125">The value must also include the access token provided by ACS.</span></span> |
| <span data-ttu-id="f3e5c-126">x-ms-version</span><span class="sxs-lookup"><span data-stu-id="f3e5c-126">x-ms-version</span></span> |<span data-ttu-id="f3e5c-127">Decimal</span><span class="sxs-lookup"><span data-stu-id="f3e5c-127">Decimal</span></span> |<span data-ttu-id="f3e5c-128">2.11</span><span class="sxs-lookup"><span data-stu-id="f3e5c-128">2.11</span></span> |
| <span data-ttu-id="f3e5c-129">DataServiceVersion</span><span class="sxs-lookup"><span data-stu-id="f3e5c-129">DataServiceVersion</span></span> |<span data-ttu-id="f3e5c-130">Decimal</span><span class="sxs-lookup"><span data-stu-id="f3e5c-130">Decimal</span></span> |<span data-ttu-id="f3e5c-131">3.0</span><span class="sxs-lookup"><span data-stu-id="f3e5c-131">3.0</span></span> |
| <span data-ttu-id="f3e5c-132">MaxDataServiceVersion</span><span class="sxs-lookup"><span data-stu-id="f3e5c-132">MaxDataServiceVersion</span></span> |<span data-ttu-id="f3e5c-133">Decimal</span><span class="sxs-lookup"><span data-stu-id="f3e5c-133">Decimal</span></span> |<span data-ttu-id="f3e5c-134">3.0</span><span class="sxs-lookup"><span data-stu-id="f3e5c-134">3.0</span></span> |

> [!NOTE]
> <span data-ttu-id="f3e5c-135">Protože Media Services používá ke zveřejnění své základní úložiště asset metadat prostřednictvím rozhraní API REST OData, DataServiceVersion a MaxDataServiceVersion hlavičky by měl být součástí každá žádost; ale pokud tomu tak není, pak aktuálně Media Services předpokládá, že hodnota DataServiceVersion používán je 3.0.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-135">Because Media Services uses OData to expose its underlying asset metadata repository through REST APIs, the DataServiceVersion and MaxDataServiceVersion headers should be included in any request; however, if they are not, then currently Media Services assumes the DataServiceVersion value in use is 3.0.</span></span>
> 
> 

<span data-ttu-id="f3e5c-136">Toto je sada volitelné hlavičky:</span><span class="sxs-lookup"><span data-stu-id="f3e5c-136">The following is a set of optional headers:</span></span>

| <span data-ttu-id="f3e5c-137">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="f3e5c-137">Header</span></span> | <span data-ttu-id="f3e5c-138">Typ</span><span class="sxs-lookup"><span data-stu-id="f3e5c-138">Type</span></span> | <span data-ttu-id="f3e5c-139">Hodnota</span><span class="sxs-lookup"><span data-stu-id="f3e5c-139">Value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f3e5c-140">Datum</span><span class="sxs-lookup"><span data-stu-id="f3e5c-140">Date</span></span> |<span data-ttu-id="f3e5c-141">RFC 1123 datum</span><span class="sxs-lookup"><span data-stu-id="f3e5c-141">RFC 1123 date</span></span> |<span data-ttu-id="f3e5c-142">Časové razítko požadavku</span><span class="sxs-lookup"><span data-stu-id="f3e5c-142">Timestamp of the request</span></span> |
| <span data-ttu-id="f3e5c-143">Přijmout</span><span class="sxs-lookup"><span data-stu-id="f3e5c-143">Accept</span></span> |<span data-ttu-id="f3e5c-144">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="f3e5c-144">Content type</span></span> |<span data-ttu-id="f3e5c-145">Požadovaný typ obsahu pro odpověď, například následující:</span><span class="sxs-lookup"><span data-stu-id="f3e5c-145">The requested content type for the response such as the following:</span></span><p> <span data-ttu-id="f3e5c-146">-application/json; odata = verbose</span><span class="sxs-lookup"><span data-stu-id="f3e5c-146">-application/json;odata=verbose</span></span><p> <span data-ttu-id="f3e5c-147">-application/atom + xml</span><span class="sxs-lookup"><span data-stu-id="f3e5c-147">- application/atom+xml</span></span><p> <span data-ttu-id="f3e5c-148">Odpovědí může mít jiný typ obsahu, jako je například načtení objektu blob, kde úspěšné odpovědi bude obsahovat datový proud objektu blob jako datovou část.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-148">Responses may have a different content type, such as a blob fetch, where a successful response will contain the blob stream as the payload.</span></span> |
| <span data-ttu-id="f3e5c-149">Přijměte kódování</span><span class="sxs-lookup"><span data-stu-id="f3e5c-149">Accept-Encoding</span></span> |<span data-ttu-id="f3e5c-150">GZIP, deflate</span><span class="sxs-lookup"><span data-stu-id="f3e5c-150">Gzip, deflate</span></span> |<span data-ttu-id="f3e5c-151">GZIP a DEFLATE kódování, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-151">GZIP and DEFLATE encoding, when applicable.</span></span> <span data-ttu-id="f3e5c-152">Poznámka: Pro velké prostředky Media Services může ignorovat tuto hlavičku a vrátit nekomprimované data.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-152">Note: For large resources, Media Services may ignore this header and return noncompressed data.</span></span> |
| <span data-ttu-id="f3e5c-153">Přijměte jazyk</span><span class="sxs-lookup"><span data-stu-id="f3e5c-153">Accept-Language</span></span> |<span data-ttu-id="f3e5c-154">"en", "es" a tak dále.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-154">"en", "es", and so on.</span></span> |<span data-ttu-id="f3e5c-155">Určuje upřednostňovaný jazyk pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-155">Specifies the preferred language for the response.</span></span> |
| <span data-ttu-id="f3e5c-156">Accept-Charset</span><span class="sxs-lookup"><span data-stu-id="f3e5c-156">Accept-Charset</span></span> |<span data-ttu-id="f3e5c-157">Znaková sada typu like "Znakové sady UTF-8"</span><span class="sxs-lookup"><span data-stu-id="f3e5c-157">Charset type like “UTF-8”</span></span> |<span data-ttu-id="f3e5c-158">Výchozí hodnota je UTF-8.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-158">Default is UTF-8.</span></span> |
| <span data-ttu-id="f3e5c-159">Metoda HTTP X-</span><span class="sxs-lookup"><span data-stu-id="f3e5c-159">X-HTTP-Method</span></span> |<span data-ttu-id="f3e5c-160">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="f3e5c-160">HTTP Method</span></span> |<span data-ttu-id="f3e5c-161">Umožňuje klientům nebo brány firewall, které nepodporují metod HTTP PUT nebo odstranění používat tyto metody tunelovým propojením prostřednictvím volání GET.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-161">Allows clients or firewalls that do not support HTTP methods like PUT or DELETE to use these methods, tunneled via a GET call.</span></span> |
| <span data-ttu-id="f3e5c-162">Content-Type</span><span class="sxs-lookup"><span data-stu-id="f3e5c-162">Content-Type</span></span> |<span data-ttu-id="f3e5c-163">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="f3e5c-163">Content type</span></span> |<span data-ttu-id="f3e5c-164">Typ obsahu žádosti v žádosti PUT nebo POST obsahu.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-164">Content type of the request body in PUT or POST requests.</span></span> |
| <span data-ttu-id="f3e5c-165">Client-request-id</span><span class="sxs-lookup"><span data-stu-id="f3e5c-165">client-request-id</span></span> |<span data-ttu-id="f3e5c-166">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f3e5c-166">String</span></span> |<span data-ttu-id="f3e5c-167">Volající definovanou hodnotu, který identifikuje daného požadavku.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-167">A caller-defined value that identifies the given request.</span></span> <span data-ttu-id="f3e5c-168">-Li zadána, bude tato hodnota zahrnuté ve zprávě s odpovědí jako způsob mapování požadavku.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-168">If specified, this value will be included in the response message as a way to map the request.</span></span> <p><p><span data-ttu-id="f3e5c-169">**Důležité upozornění**</span><span class="sxs-lookup"><span data-stu-id="f3e5c-169">**Important**</span></span><p><span data-ttu-id="f3e5c-170">Hodnoty musí být limitován 2096b (2 kB).</span><span class="sxs-lookup"><span data-stu-id="f3e5c-170">Values should be capped at 2096b (2k).</span></span> |

## <a name="standard-http-response-headers-supported-by-media-services"></a><span data-ttu-id="f3e5c-171">Hlavičky standardních odpovědí HTTP podporovaných službou Media Services</span><span class="sxs-lookup"><span data-stu-id="f3e5c-171">Standard HTTP response headers supported by Media Services</span></span>
<span data-ttu-id="f3e5c-172">Následuje sadu hlaviček, které mohou být vráceny vám v závislosti na prostředek, který se požaduje a akci, kterou byste chtěli provést.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-172">The following is a set of headers that may be returned to you depending on the resource you were requesting and the action you intended to perform.</span></span>

| <span data-ttu-id="f3e5c-173">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="f3e5c-173">Header</span></span> | <span data-ttu-id="f3e5c-174">Typ</span><span class="sxs-lookup"><span data-stu-id="f3e5c-174">Type</span></span> | <span data-ttu-id="f3e5c-175">Hodnota</span><span class="sxs-lookup"><span data-stu-id="f3e5c-175">Value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f3e5c-176">id žádosti</span><span class="sxs-lookup"><span data-stu-id="f3e5c-176">request-id</span></span> |<span data-ttu-id="f3e5c-177">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f3e5c-177">String</span></span> |<span data-ttu-id="f3e5c-178">Jedinečný identifikátor pro aktuální operaci, vygeneruje služby.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-178">A unique identifier for the current operation, service generated.</span></span> |
| <span data-ttu-id="f3e5c-179">Client-request-id</span><span class="sxs-lookup"><span data-stu-id="f3e5c-179">client-request-id</span></span> |<span data-ttu-id="f3e5c-180">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f3e5c-180">String</span></span> |<span data-ttu-id="f3e5c-181">Identifikátor určeného volající v původní žádost, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-181">An identifier specified by the caller in the original request, if present.</span></span> |
| <span data-ttu-id="f3e5c-182">Datum</span><span class="sxs-lookup"><span data-stu-id="f3e5c-182">Date</span></span> |<span data-ttu-id="f3e5c-183">RFC 1123 datum</span><span class="sxs-lookup"><span data-stu-id="f3e5c-183">RFC 1123 date</span></span> |<span data-ttu-id="f3e5c-184">Datum, kdy byl požadavek zpracovat.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-184">The date that the request was processed.</span></span> |
| <span data-ttu-id="f3e5c-185">Content-Type</span><span class="sxs-lookup"><span data-stu-id="f3e5c-185">Content-Type</span></span> |<span data-ttu-id="f3e5c-186">Je to různé.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-186">Varies</span></span> |<span data-ttu-id="f3e5c-187">Typ obsahu textu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-187">The content type of the response body.</span></span> |
| <span data-ttu-id="f3e5c-188">Kódování obsahu</span><span class="sxs-lookup"><span data-stu-id="f3e5c-188">Content-Encoding</span></span> |<span data-ttu-id="f3e5c-189">Je to různé.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-189">Varies</span></span> |<span data-ttu-id="f3e5c-190">Gzip a deflate podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-190">Gzip or deflate, as appropriate.</span></span> |

## <a name="standard-http-verbs-supported-by-media-services"></a><span data-ttu-id="f3e5c-191">Standardní příkazy HTTP podporovaných službou Media Services</span><span class="sxs-lookup"><span data-stu-id="f3e5c-191">Standard HTTP verbs supported by Media Services</span></span>
<span data-ttu-id="f3e5c-192">Následuje úplný seznam příkazů HTTP, které lze použít při vytváření HTTP požadavků:</span><span class="sxs-lookup"><span data-stu-id="f3e5c-192">The following is a complete list of HTTP verbs that can be used when making HTTP requests:</span></span>

| <span data-ttu-id="f3e5c-193">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f3e5c-193">Verb</span></span> | <span data-ttu-id="f3e5c-194">Popis</span><span class="sxs-lookup"><span data-stu-id="f3e5c-194">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f3e5c-195">GET</span><span class="sxs-lookup"><span data-stu-id="f3e5c-195">GET</span></span> |<span data-ttu-id="f3e5c-196">Vrací aktuální hodnotu objektu.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-196">Returns the current value of an object.</span></span> |
| <span data-ttu-id="f3e5c-197">POST</span><span class="sxs-lookup"><span data-stu-id="f3e5c-197">POST</span></span> |<span data-ttu-id="f3e5c-198">Vytvoří objekt na základě dat poskytuje nebo odešle příkaz.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-198">Creates an object based on the data provided, or submits a command.</span></span> |
| <span data-ttu-id="f3e5c-199">PUT</span><span class="sxs-lookup"><span data-stu-id="f3e5c-199">PUT</span></span> |<span data-ttu-id="f3e5c-200">Nahradí objekt nebo vytvoří objekt s názvem (v případě potřeby).</span><span class="sxs-lookup"><span data-stu-id="f3e5c-200">Replaces an object, or creates a named object (when applicable).</span></span> |
| <span data-ttu-id="f3e5c-201">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="f3e5c-201">DELETE</span></span> |<span data-ttu-id="f3e5c-202">Odstraní objekt.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-202">Deletes an object.</span></span> |
| <span data-ttu-id="f3e5c-203">SLOUČENÍ</span><span class="sxs-lookup"><span data-stu-id="f3e5c-203">MERGE</span></span> |<span data-ttu-id="f3e5c-204">Aktualizuje existující objekt s názvem vlastnosti změny.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-204">Updates an existing object with named property changes.</span></span> |
| <span data-ttu-id="f3e5c-205">HEAD</span><span class="sxs-lookup"><span data-stu-id="f3e5c-205">HEAD</span></span> |<span data-ttu-id="f3e5c-206">Vrátí metadata objektu pro GET odpověď.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-206">Returns metadata of an object for a GET response.</span></span> |

## <a name="discover-media-services-model"></a><span data-ttu-id="f3e5c-207">Zjistit modelu Media Services</span><span class="sxs-lookup"><span data-stu-id="f3e5c-207">Discover Media Services model</span></span>
<span data-ttu-id="f3e5c-208">Chcete-li zjistitelná entity Media Services, lze operaci $metadata.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-208">To make Media Services entities more discoverable, the $metadata operation can be used.</span></span> <span data-ttu-id="f3e5c-209">Umožňuje načíst všechny typy entit platný, vlastností entity, přidružení, funkce, akce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-209">It allows you to retrieve all valid entity types, entity properties, associations, functions, actions, and so on.</span></span> <span data-ttu-id="f3e5c-210">Následující příklad ukazuje, jak vytvořit identifikátor URI: https://media.windows.net/API/$ metadat.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-210">The following example shows how to construct the URI: https://media.windows.net/API/$metadata.</span></span>

<span data-ttu-id="f3e5c-211">By měl připojit "? api-version=2.x" na konec identifikátor URI, pokud chcete v prohlížeči zobrazit metadata nebo nebudou obsahovat hlavičku x-ms-version ve vaší žádosti.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-211">You should append "?api-version=2.x" to the end of the URI if you want to view the metadata in a browser, or do not include the x-ms-version header in your request.</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="f3e5c-212">Připojení ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="f3e5c-212">Connect to Media Services</span></span>

<span data-ttu-id="f3e5c-213">Informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="f3e5c-213">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> <span data-ttu-id="f3e5c-214">Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-214">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="f3e5c-215">Je nutné provést následující volání nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="f3e5c-215">You must make subsequent calls to the new URI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3e5c-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3e5c-216">Next steps</span></span>

<span data-ttu-id="f3e5c-217">Pro přístup k rozhraní API pro AMS se zbytkem, najdete v části [ověřování pomocí služby Azure AD pro přístup k rozhraní API Azure Media Services se zbytkem](media-services-rest-connect-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="f3e5c-217">To access AMS APIs with REST, see [Use Azure AD authentication to access the Azure Media Services API with REST](media-services-rest-connect-with-aad.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="f3e5c-218">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="f3e5c-218">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f3e5c-219">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="f3e5c-219">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
