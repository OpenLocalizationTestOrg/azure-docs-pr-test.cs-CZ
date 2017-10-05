---
title: "Protokolu kolekcí dat protokolu HTTP Analytics rozhraní API | Microsoft Docs"
description: "Log Analytics HTTP dat kolekce API můžete přidat POST JSON data do úložiště analýzy protokolů z libovolného klienta, který můžete volat rozhraní REST API. Tento článek popisuje, jak použít rozhraní API a obsahuje příklady, jak publikovat data pomocí různých programovacích jazyků."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: bwren
ms.openlocfilehash: b0c45ff8c1d4c9d35fbb3c8839b38a20df277055
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="send-data-to-log-analytics-with-the-http-data-collector-api"></a><span data-ttu-id="08f87-104">Odesílání dat k analýze protokolů s rozhraním API kolekce dat protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="08f87-104">Send data to Log Analytics with the HTTP Data Collector API</span></span>
<span data-ttu-id="08f87-105">Tento článek ukazuje, jak používat rozhraní API sady kolekcí dat protokolu HTTP k odesílání dat k analýze protokolů z klienta pro REST API.</span><span class="sxs-lookup"><span data-stu-id="08f87-105">This article shows you how to use the HTTP Data Collector API to send data to Log Analytics from a REST API client.</span></span>  <span data-ttu-id="08f87-106">Popisuje, jak formátu data shromažďovaná společností skriptu nebo aplikaci, její zahrnutí do žádost a mít této žádosti autorizovat analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="08f87-106">It describes how to format data collected by your script or application, include it in a request, and have that request authorized by Log Analytics.</span></span>  <span data-ttu-id="08f87-107">Příklady jsou uvedené pro prostředí PowerShell, C# a Python.</span><span class="sxs-lookup"><span data-stu-id="08f87-107">Examples are provided for PowerShell, C#, and Python.</span></span>

## <a name="concepts"></a><span data-ttu-id="08f87-108">Koncepty</span><span class="sxs-lookup"><span data-stu-id="08f87-108">Concepts</span></span>
<span data-ttu-id="08f87-109">Rozhraní API sady kolekcí dat protokolu HTTP můžete použít k odesílání dat k analýze protokolů z libovolného klienta, který můžete volat rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="08f87-109">You can use the HTTP Data Collector API to send data to Log Analytics from any client that can call a REST API.</span></span>  <span data-ttu-id="08f87-110">To může být sady runbook ve službě Azure Automation, který shromažďuje správy dat z Azure nebo ve jiný cloud nebo může být alternativní správy systému, který používá analýzy protokolů konsolidovat a analyzovat data.</span><span class="sxs-lookup"><span data-stu-id="08f87-110">This might be a runbook in Azure Automation that collects management data from Azure or another cloud, or it might be an alternate management system that uses Log Analytics to consolidate and analyze data.</span></span>

<span data-ttu-id="08f87-111">Všechna data v úložišti analýzy protokolů uloženo jako záznam s konkrétní typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="08f87-111">All data in the Log Analytics repository is stored as a record with a particular record type.</span></span>  <span data-ttu-id="08f87-112">Formátování dat odesílat na rozhraní API sady kolekcí dat protokolu HTTP jako více záznamů ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="08f87-112">You format your data to send to the HTTP Data Collector API as multiple records in JSON.</span></span>  <span data-ttu-id="08f87-113">Při odesílání dat jednotlivých záznamů je vytvořen v úložišti pro každý záznam v datová část požadavku.</span><span class="sxs-lookup"><span data-stu-id="08f87-113">When you submit the data, an individual record is created in the repository for each record in the request payload.</span></span>


![Přehled kolekcí dat protokolu HTTP](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a><span data-ttu-id="08f87-115">Vytvořit žádost o</span><span class="sxs-lookup"><span data-stu-id="08f87-115">Create a request</span></span>
<span data-ttu-id="08f87-116">Chcete-li použít rozhraní API sady kolekcí dat protokolu HTTP, vytvořte požadavek POST, která obsahuje data k odeslání v JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="08f87-116">To use the HTTP Data Collector API, you create a POST request that includes the data to send in JavaScript Object Notation (JSON).</span></span>  <span data-ttu-id="08f87-117">Následující tři tabulky obsahují atributy, které jsou požadovány pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="08f87-117">The next three tables list the attributes that are required for each request.</span></span> <span data-ttu-id="08f87-118">Jsme popisují každý atribut podrobněji později v článku.</span><span class="sxs-lookup"><span data-stu-id="08f87-118">We describe each attribute in more detail later in the article.</span></span>

### <a name="request-uri"></a><span data-ttu-id="08f87-119">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="08f87-119">Request URI</span></span>
| <span data-ttu-id="08f87-120">Atribut</span><span class="sxs-lookup"><span data-stu-id="08f87-120">Attribute</span></span> | <span data-ttu-id="08f87-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="08f87-121">Property</span></span> |
|:--- |:--- |
| <span data-ttu-id="08f87-122">Metoda</span><span class="sxs-lookup"><span data-stu-id="08f87-122">Method</span></span> |<span data-ttu-id="08f87-123">POST</span><span class="sxs-lookup"><span data-stu-id="08f87-123">POST</span></span> |
| <span data-ttu-id="08f87-124">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="08f87-124">URI</span></span> |<span data-ttu-id="08f87-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span><span class="sxs-lookup"><span data-stu-id="08f87-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span></span> |
| <span data-ttu-id="08f87-126">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="08f87-126">Content type</span></span> |<span data-ttu-id="08f87-127">application/json</span><span class="sxs-lookup"><span data-stu-id="08f87-127">application/json</span></span> |

### <a name="request-uri-parameters"></a><span data-ttu-id="08f87-128">Parametry identifikátoru URI požadavku</span><span class="sxs-lookup"><span data-stu-id="08f87-128">Request URI parameters</span></span>
| <span data-ttu-id="08f87-129">Parametr</span><span class="sxs-lookup"><span data-stu-id="08f87-129">Parameter</span></span> | <span data-ttu-id="08f87-130">Popis</span><span class="sxs-lookup"><span data-stu-id="08f87-130">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="08f87-131">CustomerID</span><span class="sxs-lookup"><span data-stu-id="08f87-131">CustomerID</span></span> |<span data-ttu-id="08f87-132">Jedinečný identifikátor pro pracovní prostor Microsoft Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="08f87-132">The unique identifier for the Microsoft Operations Management Suite workspace.</span></span> |
| <span data-ttu-id="08f87-133">Prostředek</span><span class="sxs-lookup"><span data-stu-id="08f87-133">Resource</span></span> |<span data-ttu-id="08f87-134">Název prostředku rozhraní API: / api/protokoly.</span><span class="sxs-lookup"><span data-stu-id="08f87-134">The API resource name: /api/logs.</span></span> |
| <span data-ttu-id="08f87-135">Verze rozhraní API</span><span class="sxs-lookup"><span data-stu-id="08f87-135">API Version</span></span> |<span data-ttu-id="08f87-136">Verze rozhraní API používat s touto žádostí.</span><span class="sxs-lookup"><span data-stu-id="08f87-136">The version of the API to use with this request.</span></span> <span data-ttu-id="08f87-137">V současné době je 2016-04-01.</span><span class="sxs-lookup"><span data-stu-id="08f87-137">Currently, it's 2016-04-01.</span></span> |

### <a name="request-headers"></a><span data-ttu-id="08f87-138">Hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="08f87-138">Request headers</span></span>
| <span data-ttu-id="08f87-139">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="08f87-139">Header</span></span> | <span data-ttu-id="08f87-140">Popis</span><span class="sxs-lookup"><span data-stu-id="08f87-140">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="08f87-141">Autorizace</span><span class="sxs-lookup"><span data-stu-id="08f87-141">Authorization</span></span> |<span data-ttu-id="08f87-142">Podpis autorizace.</span><span class="sxs-lookup"><span data-stu-id="08f87-142">The authorization signature.</span></span> <span data-ttu-id="08f87-143">Dále v tomto článku si můžete přečíst o tom, jak vytvořit hlavičku HMAC SHA256.</span><span class="sxs-lookup"><span data-stu-id="08f87-143">Later in the article, you can read about how to create an HMAC-SHA256 header.</span></span> |
| <span data-ttu-id="08f87-144">Typ protokolu</span><span class="sxs-lookup"><span data-stu-id="08f87-144">Log-Type</span></span> |<span data-ttu-id="08f87-145">Zadejte typ záznamu dat, která je odesílána.</span><span class="sxs-lookup"><span data-stu-id="08f87-145">Specify the record type of the data that is being submitted.</span></span> <span data-ttu-id="08f87-146">Typ protokolu v současné době podporuje pouze alfanumerické znaky.</span><span class="sxs-lookup"><span data-stu-id="08f87-146">Currently, the log type supports only alpha characters.</span></span> <span data-ttu-id="08f87-147">Nepodporuje se číslice nebo speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="08f87-147">It does not support numerics or special characters.</span></span> |
| <span data-ttu-id="08f87-148">x-ms datum</span><span class="sxs-lookup"><span data-stu-id="08f87-148">x-ms-date</span></span> |<span data-ttu-id="08f87-149">Datum, kdy byl požadavek zpracovat, v dokumentu RFC 1123 formátu.</span><span class="sxs-lookup"><span data-stu-id="08f87-149">The date that the request was processed, in RFC 1123 format.</span></span> |
| <span data-ttu-id="08f87-150">čas generované pole</span><span class="sxs-lookup"><span data-stu-id="08f87-150">time-generated-field</span></span> |<span data-ttu-id="08f87-151">Název pole v datech, která obsahuje časové razítko datová položka.</span><span class="sxs-lookup"><span data-stu-id="08f87-151">The name of a field in the data that contains the timestamp of the data item.</span></span> <span data-ttu-id="08f87-152">Pokud určíte pole a její obsah se používají pro **TimeGenerated**.</span><span class="sxs-lookup"><span data-stu-id="08f87-152">If you specify a field then its contents are used for **TimeGenerated**.</span></span> <span data-ttu-id="08f87-153">Pokud toto pole není určena, výchozí hodnota pro **TimeGenerated** je čas, který je konzumována zprávy.</span><span class="sxs-lookup"><span data-stu-id="08f87-153">If this field isn’t specified, the default for **TimeGenerated** is the time that the message is ingested.</span></span> <span data-ttu-id="08f87-154">Obsah zprávy pole by mělo vyhovovat formátu ISO 8601 rrrr-MM-ddTHH.</span><span class="sxs-lookup"><span data-stu-id="08f87-154">The contents of the message field should follow the ISO 8601 format YYYY-MM-DDThh:mm:ssZ.</span></span> |

## <a name="authorization"></a><span data-ttu-id="08f87-155">Autorizace</span><span class="sxs-lookup"><span data-stu-id="08f87-155">Authorization</span></span>
<span data-ttu-id="08f87-156">Každá žádost o Log Analytics HTTP dat kolekce API musí obsahovat hlavičku autorizace.</span><span class="sxs-lookup"><span data-stu-id="08f87-156">Any request to the Log Analytics HTTP Data Collector API must include an authorization header.</span></span> <span data-ttu-id="08f87-157">K ověření požadavku, musíte se odhlásit požadavek s primární nebo sekundární klíč pro pracovní prostor, který je vytvoření požadavku.</span><span class="sxs-lookup"><span data-stu-id="08f87-157">To authenticate a request, you must sign the request with either the primary or the secondary key for the workspace that is making the request.</span></span> <span data-ttu-id="08f87-158">Pak předejte tento podpis jako součást požadavku.</span><span class="sxs-lookup"><span data-stu-id="08f87-158">Then, pass that signature as part of the request.</span></span>   

<span data-ttu-id="08f87-159">Tady je formát pro hlavičku autorizace:</span><span class="sxs-lookup"><span data-stu-id="08f87-159">Here's the format for the authorization header:</span></span>

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

<span data-ttu-id="08f87-160">*WorkspaceID* je jedinečný identifikátor pro pracovní prostor služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="08f87-160">*WorkspaceID* is the unique identifier for the Operations Management Suite workspace.</span></span> <span data-ttu-id="08f87-161">*Podpis* je [Hash-based ověřování kódu metoda HMAC (Message)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , se vytvářejí na základě požadavku a potom vypočítaného pomocí [algoritmus SHA256](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span><span class="sxs-lookup"><span data-stu-id="08f87-161">*Signature* is a [Hash-based Message Authentication Code (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) that is constructed from the request and then computed by using the [SHA256 algorithm](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span></span> <span data-ttu-id="08f87-162">Potom můžete zakódovat je pomocí kódování Base64.</span><span class="sxs-lookup"><span data-stu-id="08f87-162">Then, you encode it by using Base64 encoding.</span></span>

<span data-ttu-id="08f87-163">Použijte tento formát ke kódování **SharedKey** podpis řetězec:</span><span class="sxs-lookup"><span data-stu-id="08f87-163">Use this format to encode the **SharedKey** signature string:</span></span>

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

<span data-ttu-id="08f87-164">Tady je příklad řetězce podpis:</span><span class="sxs-lookup"><span data-stu-id="08f87-164">Here's an example of a signature string:</span></span>

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

<span data-ttu-id="08f87-165">Máte-li řetězec podpis, zakódovat je pomocí algoritmus HMAC s klíčem SHA256 na řetězec kódování UTF-8 a pak kódování výsledek jako Base64.</span><span class="sxs-lookup"><span data-stu-id="08f87-165">When you have the signature string, encode it by using the HMAC-SHA256 algorithm on the UTF-8-encoded string, and then encode the result as Base64.</span></span> <span data-ttu-id="08f87-166">Použijte tento formát:</span><span class="sxs-lookup"><span data-stu-id="08f87-166">Use this format:</span></span>

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

<span data-ttu-id="08f87-167">Ukázky v dalších částech mít ukázkový kód vám pomůže vytvořit autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="08f87-167">The samples in the next sections have sample code to help you create an authorization header.</span></span>

## <a name="request-body"></a><span data-ttu-id="08f87-168">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="08f87-168">Request body</span></span>
<span data-ttu-id="08f87-169">Tělo zprávy musí být ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="08f87-169">The body of the message must be in JSON.</span></span> <span data-ttu-id="08f87-170">Musí obsahovat jeden nebo více záznamů s dvojice názvů a hodnot vlastností v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="08f87-170">It must include one or more records with the property name and value pairs in this format:</span></span>

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

<span data-ttu-id="08f87-171">Pomocí následujícího formátu můžete dávky více záznamů společně v jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="08f87-171">You can batch multiple records together in a single request by using the following format.</span></span> <span data-ttu-id="08f87-172">Všechny záznamy musí být stejného typu záznamu.</span><span class="sxs-lookup"><span data-stu-id="08f87-172">All the records must be the same record type.</span></span>

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a><span data-ttu-id="08f87-173">Typ záznamu a vlastnosti</span><span class="sxs-lookup"><span data-stu-id="08f87-173">Record type and properties</span></span>
<span data-ttu-id="08f87-174">Při odesílání dat prostřednictvím Log Analytics HTTP dat kolekce API definujete vlastní typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="08f87-174">You define a custom record type when you submit data through the Log Analytics HTTP Data Collector API.</span></span> <span data-ttu-id="08f87-175">V současné době nelze zapisovat data do existující typy záznamů, které byly vytvořeny tak, že jiné datové typy a řešení.</span><span class="sxs-lookup"><span data-stu-id="08f87-175">Currently, you can't write data to existing record types that were created by other data types and solutions.</span></span> <span data-ttu-id="08f87-176">Analýzy protokolů přečte příchozích dat a poté vytvoří vlastnosti, které odpovídají datové typy hodnot, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="08f87-176">Log Analytics reads the incoming data and then creates properties that match the data types of the values that you enter.</span></span>

<span data-ttu-id="08f87-177">Každý požadavek pro rozhraní API Log Analytics musí obsahovat **typ protokolu** záhlaví s názvem pro typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="08f87-177">Each request to the Log Analytics API must include a **Log-Type** header with the name for the record type.</span></span> <span data-ttu-id="08f87-178">Přípona **_CL** se automaticky připojí k názvu zadáte jako vlastní protokol ho odlišuje od ostatních typů protokolu.</span><span class="sxs-lookup"><span data-stu-id="08f87-178">The suffix **_CL** is automatically appended to the name you enter to distinguish it from other log types as a custom log.</span></span> <span data-ttu-id="08f87-179">Pokud zadáte název například **MyNewRecordType**, analýzy protokolů vytvoří záznam s typem **MyNewRecordType_CL**.</span><span class="sxs-lookup"><span data-stu-id="08f87-179">For example, if you enter the name **MyNewRecordType**, Log Analytics creates a record with the type **MyNewRecordType_CL**.</span></span> <span data-ttu-id="08f87-180">To pomáhá zajistit, že neexistují žádné konflikty mezi názvy typů vytvořené uživatelem a ty poskytuje současný nebo budoucí řešení společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="08f87-180">This helps ensure that there are no conflicts between user-created type names and those shipped in current or future Microsoft solutions.</span></span>

<span data-ttu-id="08f87-181">Pokud chcete identifikovat datový typ vlastnost, analýzy protokolů přidá příponu název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="08f87-181">To identify a property's data type, Log Analytics adds a suffix to the property name.</span></span> <span data-ttu-id="08f87-182">Pokud vlastnost obsahuje hodnotu null, vlastnost není součástí záznamů.</span><span class="sxs-lookup"><span data-stu-id="08f87-182">If a property contains a null value, the property is not included in that record.</span></span> <span data-ttu-id="08f87-183">Tato tabulka uvádí datový typ vlastnosti a odpovídající příponu:</span><span class="sxs-lookup"><span data-stu-id="08f87-183">This table lists the property data type and corresponding suffix:</span></span>

| <span data-ttu-id="08f87-184">Datový typ vlastnosti</span><span class="sxs-lookup"><span data-stu-id="08f87-184">Property data type</span></span> | <span data-ttu-id="08f87-185">Přípona</span><span class="sxs-lookup"><span data-stu-id="08f87-185">Suffix</span></span> |
|:--- |:--- |
| <span data-ttu-id="08f87-186">Řetězec</span><span class="sxs-lookup"><span data-stu-id="08f87-186">String</span></span> |<span data-ttu-id="08f87-187">_M</span><span class="sxs-lookup"><span data-stu-id="08f87-187">_s</span></span> |
| <span data-ttu-id="08f87-188">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="08f87-188">Boolean</span></span> |<span data-ttu-id="08f87-189">_B</span><span class="sxs-lookup"><span data-stu-id="08f87-189">_b</span></span> |
| <span data-ttu-id="08f87-190">Double</span><span class="sxs-lookup"><span data-stu-id="08f87-190">Double</span></span> |<span data-ttu-id="08f87-191">_d</span><span class="sxs-lookup"><span data-stu-id="08f87-191">_d</span></span> |
| <span data-ttu-id="08f87-192">Datum a čas</span><span class="sxs-lookup"><span data-stu-id="08f87-192">Date/time</span></span> |<span data-ttu-id="08f87-193">_T –</span><span class="sxs-lookup"><span data-stu-id="08f87-193">_t</span></span> |
| <span data-ttu-id="08f87-194">IDENTIFIKÁTOR GUID</span><span class="sxs-lookup"><span data-stu-id="08f87-194">GUID</span></span> |<span data-ttu-id="08f87-195">_g</span><span class="sxs-lookup"><span data-stu-id="08f87-195">_g</span></span> |

<span data-ttu-id="08f87-196">Datový typ, který používá analýzy protokolů pro každou vlastnost závisí na tom, jestli typ záznamu pro nový záznam již existuje.</span><span class="sxs-lookup"><span data-stu-id="08f87-196">The data type that Log Analytics uses for each property depends on whether the record type for the new record already exists.</span></span>

* <span data-ttu-id="08f87-197">Pokud typ záznamu neexistuje, analýzy protokolů vytvoří novou.</span><span class="sxs-lookup"><span data-stu-id="08f87-197">If the record type does not exist, Log Analytics creates a new one.</span></span> <span data-ttu-id="08f87-198">Analýzy protokolů používá k určení datového typu pro každou vlastnost pro nový záznam odvození typu JSON.</span><span class="sxs-lookup"><span data-stu-id="08f87-198">Log Analytics uses the JSON type inference to determine the data type for each property for the new record.</span></span>
* <span data-ttu-id="08f87-199">Pokud typ záznamu neexistuje, analýzy protokolů se pokusí vytvořit nový záznam na základě existující vlastností.</span><span class="sxs-lookup"><span data-stu-id="08f87-199">If the record type does exist, Log Analytics attempts to create a new record based on existing properties.</span></span> <span data-ttu-id="08f87-200">Pokud datový typ pro vlastnost v novém záznamu se neshoduje se a nelze převést na typ existující, nebo pokud záznam obsahuje vlastnosti, která neexistuje, analýzy protokolů vytvoří novou vlastnost s příponou relevantní.</span><span class="sxs-lookup"><span data-stu-id="08f87-200">If the data type for a property in the new record doesn’t match and can’t be converted to the existing type, or if the record includes a property that doesn’t exist, Log Analytics creates a new property that has the relevant suffix.</span></span>

<span data-ttu-id="08f87-201">Například by tato položka odeslání vytvořit záznam s třemi vlastnostmi **number_d**, **boolean_b**, a **string_s**:</span><span class="sxs-lookup"><span data-stu-id="08f87-201">For example, this submission entry would create a record with three properties, **number_d**, **boolean_b**, and **string_s**:</span></span>

![Ukázka záznamu 1](media/log-analytics-data-collector-api/record-01.png)

<span data-ttu-id="08f87-203">Pokud pak odeslání této další položky, hodnoty všech formátu řetězce, nebude změnit vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="08f87-203">If you then submitted this next entry, with all values formatted as strings, the properties would not change.</span></span> <span data-ttu-id="08f87-204">Tyto hodnoty lze převést na existující typy dat:</span><span class="sxs-lookup"><span data-stu-id="08f87-204">These values can be converted to existing data types:</span></span>

![Ukázka záznamu 2](media/log-analytics-data-collector-api/record-02.png)

<span data-ttu-id="08f87-206">Ale pokud jste provedli poté tento další odeslání, analýzy protokolů by vytvořit nové vlastnosti **boolean_d** a **string_d**.</span><span class="sxs-lookup"><span data-stu-id="08f87-206">But, if you then made this next submission, Log Analytics would create the new properties **boolean_d** and **string_d**.</span></span> <span data-ttu-id="08f87-207">Nelze převést tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="08f87-207">These values can't be converted:</span></span>

![Ukázka záznamu 3](media/log-analytics-data-collector-api/record-03.png)

<span data-ttu-id="08f87-209">Pokud tuto položku, se potom odeslán, před vytvořením typ záznamu, analýzy protokolů by vytvořit záznam s třemi vlastnostmi **úspěch**, **boolean_s**, a **string_s**.</span><span class="sxs-lookup"><span data-stu-id="08f87-209">If you then submitted the following entry, before the record type was created, Log Analytics would create a record with three properties, **number_s**, **boolean_s**, and **string_s**.</span></span> <span data-ttu-id="08f87-210">V této položce všechny počáteční hodnoty, je naformátovaná jako řetězec:</span><span class="sxs-lookup"><span data-stu-id="08f87-210">In this entry, each of the initial values is formatted as a string:</span></span>

![Ukázka záznamu 4](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a><span data-ttu-id="08f87-212">Omezení dat</span><span class="sxs-lookup"><span data-stu-id="08f87-212">Data limits</span></span>
<span data-ttu-id="08f87-213">Existují některá omezení kolem data odeslány do kolekce Log Analytics Data rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="08f87-213">There are some constraints around the data posted to the Log Analytics Data collection API.</span></span>

* <span data-ttu-id="08f87-214">Maximálně 30 MB za post protokolu analýzy dat kolekce API.</span><span class="sxs-lookup"><span data-stu-id="08f87-214">Maximum of 30 MB per post to Log Analytics Data Collector API.</span></span> <span data-ttu-id="08f87-215">Toto je omezení velikosti pro jednu metodu post.</span><span class="sxs-lookup"><span data-stu-id="08f87-215">This is a size limit for a single post.</span></span> <span data-ttu-id="08f87-216">Pokud data z jedné odeslání, který překračuje 30 MB, měli rozdělení dat až bloky s menší velikostí a odešlete je současně.</span><span class="sxs-lookup"><span data-stu-id="08f87-216">If the data from a single post that exceeds 30 MB, you should split the data up to smaller sized chunks and send them concurrently.</span></span>
* <span data-ttu-id="08f87-217">Maximální limit 32 KB pro pole hodnot.</span><span class="sxs-lookup"><span data-stu-id="08f87-217">Maximum of 32 KB limit for field values.</span></span> <span data-ttu-id="08f87-218">Pokud hodnota pole je větší než 32 KB, bude zkrácen data.</span><span class="sxs-lookup"><span data-stu-id="08f87-218">If the field value is greater than 32 KB, the data will be truncated.</span></span>
* <span data-ttu-id="08f87-219">Doporučený maximální počet polí pro daný typ je 50.</span><span class="sxs-lookup"><span data-stu-id="08f87-219">Recommended maximum number of fields for a given type is 50.</span></span> <span data-ttu-id="08f87-220">Toto je praktické omezení z perspektivy prostředí vyhledávání a použitelnost.</span><span class="sxs-lookup"><span data-stu-id="08f87-220">This is a practical limit from a usability and search experience perspective.</span></span>  

## <a name="return-codes"></a><span data-ttu-id="08f87-221">Návratové kódy</span><span class="sxs-lookup"><span data-stu-id="08f87-221">Return codes</span></span>
<span data-ttu-id="08f87-222">Stavový kód HTTP 200 znamená, že žádost byla přijata pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="08f87-222">The HTTP status code 200 means that the request has been received for processing.</span></span> <span data-ttu-id="08f87-223">To znamená, že operace úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="08f87-223">This indicates that the operation completed successfully.</span></span>

<span data-ttu-id="08f87-224">Tato tabulka uvádí kompletní sadu stavové kódy, které může vrátit službu:</span><span class="sxs-lookup"><span data-stu-id="08f87-224">This table lists the complete set of status codes that the service might return:</span></span>

| <span data-ttu-id="08f87-225">Kód</span><span class="sxs-lookup"><span data-stu-id="08f87-225">Code</span></span> | <span data-ttu-id="08f87-226">Status</span><span class="sxs-lookup"><span data-stu-id="08f87-226">Status</span></span> | <span data-ttu-id="08f87-227">Kód chyby</span><span class="sxs-lookup"><span data-stu-id="08f87-227">Error code</span></span> | <span data-ttu-id="08f87-228">Popis</span><span class="sxs-lookup"><span data-stu-id="08f87-228">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="08f87-229">200</span><span class="sxs-lookup"><span data-stu-id="08f87-229">200</span></span> |<span data-ttu-id="08f87-230">OK</span><span class="sxs-lookup"><span data-stu-id="08f87-230">OK</span></span> | |<span data-ttu-id="08f87-231">Žádost byla přijata úspěšně.</span><span class="sxs-lookup"><span data-stu-id="08f87-231">The request was successfully accepted.</span></span> |
| <span data-ttu-id="08f87-232">400</span><span class="sxs-lookup"><span data-stu-id="08f87-232">400</span></span> |<span data-ttu-id="08f87-233">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="08f87-233">Bad request</span></span> |<span data-ttu-id="08f87-234">InactiveCustomer</span><span class="sxs-lookup"><span data-stu-id="08f87-234">InactiveCustomer</span></span> |<span data-ttu-id="08f87-235">Pracovní prostor byl uzavřen.</span><span class="sxs-lookup"><span data-stu-id="08f87-235">The workspace has been closed.</span></span> |
| <span data-ttu-id="08f87-236">400</span><span class="sxs-lookup"><span data-stu-id="08f87-236">400</span></span> |<span data-ttu-id="08f87-237">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="08f87-237">Bad request</span></span> |<span data-ttu-id="08f87-238">InvalidApiVersion</span><span class="sxs-lookup"><span data-stu-id="08f87-238">InvalidApiVersion</span></span> |<span data-ttu-id="08f87-239">Služba nebyla rozpoznána verze rozhraní API, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="08f87-239">The API version that you specified was not recognized by the service.</span></span> |
| <span data-ttu-id="08f87-240">400</span><span class="sxs-lookup"><span data-stu-id="08f87-240">400</span></span> |<span data-ttu-id="08f87-241">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="08f87-241">Bad request</span></span> |<span data-ttu-id="08f87-242">InvalidCustomerId</span><span class="sxs-lookup"><span data-stu-id="08f87-242">InvalidCustomerId</span></span> |<span data-ttu-id="08f87-243">Zadané ID pracovního prostoru je neplatný.</span><span class="sxs-lookup"><span data-stu-id="08f87-243">The workspace ID specified is invalid.</span></span> |
| <span data-ttu-id="08f87-244">400</span><span class="sxs-lookup"><span data-stu-id="08f87-244">400</span></span> |<span data-ttu-id="08f87-245">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="08f87-245">Bad request</span></span> |<span data-ttu-id="08f87-246">InvalidDataFormat</span><span class="sxs-lookup"><span data-stu-id="08f87-246">InvalidDataFormat</span></span> |<span data-ttu-id="08f87-247">Byla odeslána neplatná JSON.</span><span class="sxs-lookup"><span data-stu-id="08f87-247">Invalid JSON was submitted.</span></span> <span data-ttu-id="08f87-248">Text odpovědi může obsahovat další informace o tom, jak vyřešit chyby.</span><span class="sxs-lookup"><span data-stu-id="08f87-248">The response body might contain more information about how to resolve the error.</span></span> |
| <span data-ttu-id="08f87-249">400</span><span class="sxs-lookup"><span data-stu-id="08f87-249">400</span></span> |<span data-ttu-id="08f87-250">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="08f87-250">Bad request</span></span> |<span data-ttu-id="08f87-251">InvalidLogType</span><span class="sxs-lookup"><span data-stu-id="08f87-251">InvalidLogType</span></span> |<span data-ttu-id="08f87-252">Typ protokolu zadat obsahují zvláštní znaky nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="08f87-252">The log type specified contained special characters or numerics.</span></span> |
| <span data-ttu-id="08f87-253">400</span><span class="sxs-lookup"><span data-stu-id="08f87-253">400</span></span> |<span data-ttu-id="08f87-254">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="08f87-254">Bad request</span></span> |<span data-ttu-id="08f87-255">MissingApiVersion</span><span class="sxs-lookup"><span data-stu-id="08f87-255">MissingApiVersion</span></span> |<span data-ttu-id="08f87-256">Verze rozhraní API není zadaný.</span><span class="sxs-lookup"><span data-stu-id="08f87-256">The API version wasn’t specified.</span></span> |
| <span data-ttu-id="08f87-257">400</span><span class="sxs-lookup"><span data-stu-id="08f87-257">400</span></span> |<span data-ttu-id="08f87-258">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="08f87-258">Bad request</span></span> |<span data-ttu-id="08f87-259">MissingContentType</span><span class="sxs-lookup"><span data-stu-id="08f87-259">MissingContentType</span></span> |<span data-ttu-id="08f87-260">Typ obsahu, který nebyl zadán.</span><span class="sxs-lookup"><span data-stu-id="08f87-260">The content type wasn’t specified.</span></span> |
| <span data-ttu-id="08f87-261">400</span><span class="sxs-lookup"><span data-stu-id="08f87-261">400</span></span> |<span data-ttu-id="08f87-262">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="08f87-262">Bad request</span></span> |<span data-ttu-id="08f87-263">MissingLogType</span><span class="sxs-lookup"><span data-stu-id="08f87-263">MissingLogType</span></span> |<span data-ttu-id="08f87-264">Typ protokolu požadovaná hodnota nebyl zadán.</span><span class="sxs-lookup"><span data-stu-id="08f87-264">The required value log type wasn’t specified.</span></span> |
| <span data-ttu-id="08f87-265">400</span><span class="sxs-lookup"><span data-stu-id="08f87-265">400</span></span> |<span data-ttu-id="08f87-266">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="08f87-266">Bad request</span></span> |<span data-ttu-id="08f87-267">UnsupportedContentType</span><span class="sxs-lookup"><span data-stu-id="08f87-267">UnsupportedContentType</span></span> |<span data-ttu-id="08f87-268">Typ obsahu, který nebyl nastaven na **application/json**.</span><span class="sxs-lookup"><span data-stu-id="08f87-268">The content type was not set to **application/json**.</span></span> |
| <span data-ttu-id="08f87-269">403</span><span class="sxs-lookup"><span data-stu-id="08f87-269">403</span></span> |<span data-ttu-id="08f87-270">Je zakázané</span><span class="sxs-lookup"><span data-stu-id="08f87-270">Forbidden</span></span> |<span data-ttu-id="08f87-271">InvalidAuthorization</span><span class="sxs-lookup"><span data-stu-id="08f87-271">InvalidAuthorization</span></span> |<span data-ttu-id="08f87-272">Službě se nepodařilo ověřit žádost.</span><span class="sxs-lookup"><span data-stu-id="08f87-272">The service failed to authenticate the request.</span></span> <span data-ttu-id="08f87-273">Ověření platnosti připojení ID a klíč pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="08f87-273">Verify that the workspace ID and connection key are valid.</span></span> |
| <span data-ttu-id="08f87-274">404</span><span class="sxs-lookup"><span data-stu-id="08f87-274">404</span></span> |<span data-ttu-id="08f87-275">Nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="08f87-275">Not Found</span></span> | | <span data-ttu-id="08f87-276">Buď je zadaná adresa URL nesprávná nebo požadavku je příliš velký.</span><span class="sxs-lookup"><span data-stu-id="08f87-276">Either the URL provided is incorrect, or the request is too large.</span></span> |
| <span data-ttu-id="08f87-277">429</span><span class="sxs-lookup"><span data-stu-id="08f87-277">429</span></span> |<span data-ttu-id="08f87-278">Příliš mnoho požadavků</span><span class="sxs-lookup"><span data-stu-id="08f87-278">Too Many Requests</span></span> | | <span data-ttu-id="08f87-279">Služba dochází k velkému počtu data z účtu.</span><span class="sxs-lookup"><span data-stu-id="08f87-279">The service is experiencing a high volume of data from your account.</span></span> <span data-ttu-id="08f87-280">Opakujte požadavek později.</span><span class="sxs-lookup"><span data-stu-id="08f87-280">Please retry the request later.</span></span> |
| <span data-ttu-id="08f87-281">500</span><span class="sxs-lookup"><span data-stu-id="08f87-281">500</span></span> |<span data-ttu-id="08f87-282">Vnitřní chyba serveru</span><span class="sxs-lookup"><span data-stu-id="08f87-282">Internal Server Error</span></span> |<span data-ttu-id="08f87-283">UnspecifiedError</span><span class="sxs-lookup"><span data-stu-id="08f87-283">UnspecifiedError</span></span> |<span data-ttu-id="08f87-284">Služba zjistila vnitřní chybu.</span><span class="sxs-lookup"><span data-stu-id="08f87-284">The service encountered an internal error.</span></span> <span data-ttu-id="08f87-285">Opakujte žádost.</span><span class="sxs-lookup"><span data-stu-id="08f87-285">Please retry the request.</span></span> |
| <span data-ttu-id="08f87-286">503</span><span class="sxs-lookup"><span data-stu-id="08f87-286">503</span></span> |<span data-ttu-id="08f87-287">Služba není k dispozici</span><span class="sxs-lookup"><span data-stu-id="08f87-287">Service Unavailable</span></span> |<span data-ttu-id="08f87-288">ServiceUnavailable</span><span class="sxs-lookup"><span data-stu-id="08f87-288">ServiceUnavailable</span></span> |<span data-ttu-id="08f87-289">Služba je momentálně nedostupný a nepřijímá požadavky.</span><span class="sxs-lookup"><span data-stu-id="08f87-289">The service currently is unavailable to receive requests.</span></span> <span data-ttu-id="08f87-290">Opakujte žádost.</span><span class="sxs-lookup"><span data-stu-id="08f87-290">Please retry your request.</span></span> |

## <a name="query-data"></a><span data-ttu-id="08f87-291">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="08f87-291">Query data</span></span>
<span data-ttu-id="08f87-292">K dotazování na data odeslaná vyhledejte záznamy s aktualizace Log Analytics HTTP dat kolekce API **typ** který se rovná **LogType** hodnotu, která jste zadali, spolu s **_CL**.</span><span class="sxs-lookup"><span data-stu-id="08f87-292">To query data submitted by the Log Analytics HTTP Data Collector API, search for records with **Type** that is equal to the **LogType** value that you specified, appended with **_CL**.</span></span> <span data-ttu-id="08f87-293">Pokud jste použili například **MyCustomLog**, pak by vrátit všechny záznamy s **typ = MyCustomLog_CL**.</span><span class="sxs-lookup"><span data-stu-id="08f87-293">For example, if you used **MyCustomLog**, then you'd return all records with **Type=MyCustomLog_CL**.</span></span>

>[!NOTE]
> <span data-ttu-id="08f87-294">Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak výše uvedeném dotazu by změnit na následující.</span><span class="sxs-lookup"><span data-stu-id="08f87-294">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above query would change to the following.</span></span>

> `MyCustomLog_CL`

## <a name="sample-requests"></a><span data-ttu-id="08f87-295">Ukázka požadavků</span><span class="sxs-lookup"><span data-stu-id="08f87-295">Sample requests</span></span>
<span data-ttu-id="08f87-296">V následujících částech najdete ukázky postup odesílání dat do kolekce API protokolu analýzy HTTP Data pomocí různých programovacích jazyků.</span><span class="sxs-lookup"><span data-stu-id="08f87-296">In the next sections, you'll find samples of how to submit data to the Log Analytics HTTP Data Collector API by using different programming languages.</span></span>

<span data-ttu-id="08f87-297">Pro každý vzorek proveďte tyto kroky nastavit proměnné pro hlavičku autorizace:</span><span class="sxs-lookup"><span data-stu-id="08f87-297">For each sample, do these steps to set the variables for the authorization header:</span></span>

1. <span data-ttu-id="08f87-298">Na portálu služby Operations Management Suite, vyberte **nastavení** dlaždici a potom vyberte **připojené zdroje** kartě.</span><span class="sxs-lookup"><span data-stu-id="08f87-298">In the Operations Management Suite portal, select the **Settings** tile, and then select the **Connected Sources** tab.</span></span>
2. <span data-ttu-id="08f87-299">Napravo od **ID pracovního prostoru**, vyberte ikonu kopírování a vložte ID jako hodnotu **ID zákazníka** proměnné.</span><span class="sxs-lookup"><span data-stu-id="08f87-299">To the right of **Workspace ID**, select the copy icon, and then paste the ID as the value of the **Customer ID** variable.</span></span>
3. <span data-ttu-id="08f87-300">Napravo od **primární klíč**, vyberte ikonu kopírování a vložte ID jako hodnotu **sdílený klíč** proměnné.</span><span class="sxs-lookup"><span data-stu-id="08f87-300">To the right of **Primary Key**, select the copy icon, and then paste the ID as the value of the **Shared Key** variable.</span></span>

<span data-ttu-id="08f87-301">Alternativně můžete změnit proměnné pro typ protokolu a JSON data.</span><span class="sxs-lookup"><span data-stu-id="08f87-301">Alternatively, you can change the variables for the log type and JSON data.</span></span>

### <a name="powershell-sample"></a><span data-ttu-id="08f87-302">Ukázkové prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="08f87-302">PowerShell sample</span></span>
```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a><span data-ttu-id="08f87-303">Ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="08f87-303">C# sample</span></span>
```
using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Security.Cryptography;
using System.Text;
using System.Threading.Tasks;

namespace OIAPIExample
{
    class ApiExample
    {
        // An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField3"":""DemoValue3"",""DemoField4"":""DemoValue4""}]";

        // Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

        // You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

        // Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            try
            {
                string url = "https://" + customerId + ".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";

                System.Net.Http.HttpClient client = new System.Net.Http.HttpClient();
                client.DefaultRequestHeaders.Add("Accept", "application/json");
                client.DefaultRequestHeaders.Add("Log-Type", LogName);
                client.DefaultRequestHeaders.Add("Authorization", signature);
                client.DefaultRequestHeaders.Add("x-ms-date", date);
                client.DefaultRequestHeaders.Add("time-generated-field", TimeStampField);

                System.Net.Http.HttpContent httpContent = new StringContent(json, Encoding.UTF8);
                httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                Task<System.Net.Http.HttpResponseMessage> response = client.PostAsync(new Uri(url), httpContent);

                System.Net.Http.HttpContent responseContent = response.Result.Content;
                string result = responseContent.ReadAsStringAsync().Result;
                Console.WriteLine("Return Result: " + result);
            }
            catch (Exception excep)
            {
                Console.WriteLine("API Post Exception: " + excep.Message);
            }
        }
    }
}

```

### <a name="python-sample"></a><span data-ttu-id="08f87-304">Ukázka Pythonu</span><span class="sxs-lookup"><span data-stu-id="08f87-304">Python sample</span></span>
```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code >= 200 and response.status_code <= 299):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a><span data-ttu-id="08f87-305">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08f87-305">Next steps</span></span>
- <span data-ttu-id="08f87-306">Použití [rozhraní API pro vyhledávání protokolu](log-analytics-log-search-api.md) k načtení dat z úložiště analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="08f87-306">Use the [Log Search API](log-analytics-log-search-api.md) to retrieve data from the Log Analytics repository.</span></span>
