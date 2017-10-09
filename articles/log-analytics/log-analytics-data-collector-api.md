---
title: aaaLog API kolekce dat protokolu HTTP Analytics | Microsoft Docs
description: "Můžete použít hello Log Analytics HTTP dat kolekce API tooadd POST JSON toohello analýzy protokolů úložiště data z libovolného klienta, který můžete volat hello REST API. Tento článek popisuje, jak toouse hello rozhraní API a příklady, jak má toopublish data pomocí různých programovacích jazyků."
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
ms.openlocfilehash: c2921082831c49da764d946ac9c4fab975a38185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-data-toolog-analytics-with-hello-http-data-collector-api"></a><span data-ttu-id="61627-104">Odeslání dat tooLog Analytics s hello rozhraní API sady kolekcí dat protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="61627-104">Send data tooLog Analytics with hello HTTP Data Collector API</span></span>
<span data-ttu-id="61627-105">Tento článek ukazuje, jak toouse hello tooLog data toosend Analytics rozhraní API sady kolekcí dat protokolu HTTP od klienta pro REST API.</span><span class="sxs-lookup"><span data-stu-id="61627-105">This article shows you how toouse hello HTTP Data Collector API toosend data tooLog Analytics from a REST API client.</span></span>  <span data-ttu-id="61627-106">Se popisuje, jak tooformat data shromažďovaná společností skriptu nebo aplikace, zahrnují v požadavku a mít této žádosti autorizovat analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="61627-106">It describes how tooformat data collected by your script or application, include it in a request, and have that request authorized by Log Analytics.</span></span>  <span data-ttu-id="61627-107">Příklady jsou uvedené pro prostředí PowerShell, C# a Python.</span><span class="sxs-lookup"><span data-stu-id="61627-107">Examples are provided for PowerShell, C#, and Python.</span></span>

## <a name="concepts"></a><span data-ttu-id="61627-108">Koncepty</span><span class="sxs-lookup"><span data-stu-id="61627-108">Concepts</span></span>
<span data-ttu-id="61627-109">Můžete použít hello rozhraní API sady kolekcí dat protokolu HTTP toosend data tooLog Analytics z libovolného klienta, který můžete volat rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="61627-109">You can use hello HTTP Data Collector API toosend data tooLog Analytics from any client that can call a REST API.</span></span>  <span data-ttu-id="61627-110">To může být sady runbook ve službě Azure Automation, který shromažďuje správy dat z Azure nebo jiný cloud nebo ji mohou být alternativní správy systému, který používá tooconsolidate analýzy protokolů a analyzovat data.</span><span class="sxs-lookup"><span data-stu-id="61627-110">This might be a runbook in Azure Automation that collects management data from Azure or another cloud, or it might be an alternate management system that uses Log Analytics tooconsolidate and analyze data.</span></span>

<span data-ttu-id="61627-111">Všechna data v úložišti analýzy protokolů hello uloženo jako záznam s konkrétní typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="61627-111">All data in hello Log Analytics repository is stored as a record with a particular record type.</span></span>  <span data-ttu-id="61627-112">Vaše data toosend toohello rozhraní API sady kolekcí dat protokolu HTTP je formátovat jako více záznamů ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="61627-112">You format your data toosend toohello HTTP Data Collector API as multiple records in JSON.</span></span>  <span data-ttu-id="61627-113">Při odesílání dat hello jednotlivé záznamy se vytvoří v hello úložiště pro každý záznam v hello datová část požadavku.</span><span class="sxs-lookup"><span data-stu-id="61627-113">When you submit hello data, an individual record is created in hello repository for each record in hello request payload.</span></span>


![Přehled kolekcí dat protokolu HTTP](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a><span data-ttu-id="61627-115">Vytvořit žádost o</span><span class="sxs-lookup"><span data-stu-id="61627-115">Create a request</span></span>
<span data-ttu-id="61627-116">toouse hello HTTP kolekcí dat rozhraní API, můžete vytvořit požadavek POST, která obsahuje hello data toosend v JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="61627-116">toouse hello HTTP Data Collector API, you create a POST request that includes hello data toosend in JavaScript Object Notation (JSON).</span></span>  <span data-ttu-id="61627-117">Hello následující tři tabulky seznam hello atributy, které jsou požadovány pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="61627-117">hello next three tables list hello attributes that are required for each request.</span></span> <span data-ttu-id="61627-118">Jsme popisují každý atribut podrobněji dále v článku hello.</span><span class="sxs-lookup"><span data-stu-id="61627-118">We describe each attribute in more detail later in hello article.</span></span>

### <a name="request-uri"></a><span data-ttu-id="61627-119">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="61627-119">Request URI</span></span>
| <span data-ttu-id="61627-120">Atribut</span><span class="sxs-lookup"><span data-stu-id="61627-120">Attribute</span></span> | <span data-ttu-id="61627-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="61627-121">Property</span></span> |
|:--- |:--- |
| <span data-ttu-id="61627-122">Metoda</span><span class="sxs-lookup"><span data-stu-id="61627-122">Method</span></span> |<span data-ttu-id="61627-123">POST</span><span class="sxs-lookup"><span data-stu-id="61627-123">POST</span></span> |
| <span data-ttu-id="61627-124">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="61627-124">URI</span></span> |<span data-ttu-id="61627-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span><span class="sxs-lookup"><span data-stu-id="61627-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span></span> |
| <span data-ttu-id="61627-126">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="61627-126">Content type</span></span> |<span data-ttu-id="61627-127">application/json</span><span class="sxs-lookup"><span data-stu-id="61627-127">application/json</span></span> |

### <a name="request-uri-parameters"></a><span data-ttu-id="61627-128">Parametry identifikátoru URI požadavku</span><span class="sxs-lookup"><span data-stu-id="61627-128">Request URI parameters</span></span>
| <span data-ttu-id="61627-129">Parametr</span><span class="sxs-lookup"><span data-stu-id="61627-129">Parameter</span></span> | <span data-ttu-id="61627-130">Popis</span><span class="sxs-lookup"><span data-stu-id="61627-130">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="61627-131">CustomerID</span><span class="sxs-lookup"><span data-stu-id="61627-131">CustomerID</span></span> |<span data-ttu-id="61627-132">Hello jedinečný identifikátor pro pracovní prostor hello Microsoft Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="61627-132">hello unique identifier for hello Microsoft Operations Management Suite workspace.</span></span> |
| <span data-ttu-id="61627-133">Prostředek</span><span class="sxs-lookup"><span data-stu-id="61627-133">Resource</span></span> |<span data-ttu-id="61627-134">název prostředku Hello rozhraní API: / api/protokoly.</span><span class="sxs-lookup"><span data-stu-id="61627-134">hello API resource name: /api/logs.</span></span> |
| <span data-ttu-id="61627-135">Verze rozhraní API</span><span class="sxs-lookup"><span data-stu-id="61627-135">API Version</span></span> |<span data-ttu-id="61627-136">verze Hello toouse hello rozhraní API s touto žádostí.</span><span class="sxs-lookup"><span data-stu-id="61627-136">hello version of hello API toouse with this request.</span></span> <span data-ttu-id="61627-137">V současné době je 2016-04-01.</span><span class="sxs-lookup"><span data-stu-id="61627-137">Currently, it's 2016-04-01.</span></span> |

### <a name="request-headers"></a><span data-ttu-id="61627-138">Hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="61627-138">Request headers</span></span>
| <span data-ttu-id="61627-139">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="61627-139">Header</span></span> | <span data-ttu-id="61627-140">Popis</span><span class="sxs-lookup"><span data-stu-id="61627-140">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="61627-141">Autorizace</span><span class="sxs-lookup"><span data-stu-id="61627-141">Authorization</span></span> |<span data-ttu-id="61627-142">podpis Hello autorizace.</span><span class="sxs-lookup"><span data-stu-id="61627-142">hello authorization signature.</span></span> <span data-ttu-id="61627-143">Dále v článku hello, si můžete přečíst o tom, záhlaví toocreate HMAC algoritmus SHA256.</span><span class="sxs-lookup"><span data-stu-id="61627-143">Later in hello article, you can read about how toocreate an HMAC-SHA256 header.</span></span> |
| <span data-ttu-id="61627-144">Typ protokolu</span><span class="sxs-lookup"><span data-stu-id="61627-144">Log-Type</span></span> |<span data-ttu-id="61627-145">Zadejte typ záznamu hello hello dat, která je odesílána.</span><span class="sxs-lookup"><span data-stu-id="61627-145">Specify hello record type of hello data that is being submitted.</span></span> <span data-ttu-id="61627-146">Typ protokolu hello v současné době podporuje pouze alfanumerické znaky.</span><span class="sxs-lookup"><span data-stu-id="61627-146">Currently, hello log type supports only alpha characters.</span></span> <span data-ttu-id="61627-147">Nepodporuje se číslice nebo speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="61627-147">It does not support numerics or special characters.</span></span> |
| <span data-ttu-id="61627-148">x-ms datum</span><span class="sxs-lookup"><span data-stu-id="61627-148">x-ms-date</span></span> |<span data-ttu-id="61627-149">Datum Hello zpracování tohoto požadavku hello, v dokumentu RFC 1123 formátu.</span><span class="sxs-lookup"><span data-stu-id="61627-149">hello date that hello request was processed, in RFC 1123 format.</span></span> |
| <span data-ttu-id="61627-150">čas generované pole</span><span class="sxs-lookup"><span data-stu-id="61627-150">time-generated-field</span></span> |<span data-ttu-id="61627-151">Hello název pole v hello data, která obsahuje časové razítko hello položky dat hello.</span><span class="sxs-lookup"><span data-stu-id="61627-151">hello name of a field in hello data that contains hello timestamp of hello data item.</span></span> <span data-ttu-id="61627-152">Pokud určíte pole a její obsah se používají pro **TimeGenerated**.</span><span class="sxs-lookup"><span data-stu-id="61627-152">If you specify a field then its contents are used for **TimeGenerated**.</span></span> <span data-ttu-id="61627-153">Pokud toto pole není určena, výchozí hodnoty pro hello **TimeGenerated** je čas hello této hello je konzumována zprávy.</span><span class="sxs-lookup"><span data-stu-id="61627-153">If this field isn’t specified, hello default for **TimeGenerated** is hello time that hello message is ingested.</span></span> <span data-ttu-id="61627-154">postupujte podle Hello obsah pole zpráv hello hello ISO 8601 formát rrrr-MM-ddTHH.</span><span class="sxs-lookup"><span data-stu-id="61627-154">hello contents of hello message field should follow hello ISO 8601 format YYYY-MM-DDThh:mm:ssZ.</span></span> |

## <a name="authorization"></a><span data-ttu-id="61627-155">Autorizace</span><span class="sxs-lookup"><span data-stu-id="61627-155">Authorization</span></span>
<span data-ttu-id="61627-156">Všechny žádosti o toohello Log Analytics HTTP dat kolekce API musí obsahovat hlavičku autorizace.</span><span class="sxs-lookup"><span data-stu-id="61627-156">Any request toohello Log Analytics HTTP Data Collector API must include an authorization header.</span></span> <span data-ttu-id="61627-157">tooauthenticate žádost, musíte se odhlásit hello žádost s hello primární nebo sekundární klíč hello pro hello pracovní prostor, který je vytváření hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="61627-157">tooauthenticate a request, you must sign hello request with either hello primary or hello secondary key for hello workspace that is making hello request.</span></span> <span data-ttu-id="61627-158">Pak předejte tento podpis jako součást požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="61627-158">Then, pass that signature as part of hello request.</span></span>   

<span data-ttu-id="61627-159">Tady je hello formát pro hello autorizační hlavičky:</span><span class="sxs-lookup"><span data-stu-id="61627-159">Here's hello format for hello authorization header:</span></span>

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

<span data-ttu-id="61627-160">*WorkspaceID* je hello jedinečný identifikátor pro pracovní prostor služby Operations Management Suite hello.</span><span class="sxs-lookup"><span data-stu-id="61627-160">*WorkspaceID* is hello unique identifier for hello Operations Management Suite workspace.</span></span> <span data-ttu-id="61627-161">*Podpis* je [Hash-based ověřování kódu metoda HMAC (Message)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , se vytvářejí na základě požadavku hello a pak vypočítaného pomocí hello [algoritmus SHA256](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span><span class="sxs-lookup"><span data-stu-id="61627-161">*Signature* is a [Hash-based Message Authentication Code (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) that is constructed from hello request and then computed by using hello [SHA256 algorithm](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span></span> <span data-ttu-id="61627-162">Potom můžete zakódovat je pomocí kódování Base64.</span><span class="sxs-lookup"><span data-stu-id="61627-162">Then, you encode it by using Base64 encoding.</span></span>

<span data-ttu-id="61627-163">Použijte tento formát tooencode hello **SharedKey** podpis řetězec:</span><span class="sxs-lookup"><span data-stu-id="61627-163">Use this format tooencode hello **SharedKey** signature string:</span></span>

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

<span data-ttu-id="61627-164">Tady je příklad řetězce podpis:</span><span class="sxs-lookup"><span data-stu-id="61627-164">Here's an example of a signature string:</span></span>

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

<span data-ttu-id="61627-165">Pokud máte hello podpis řetězec, zakódovat je pomocí hello algoritmus HMAC s klíčem SHA256 na hello řetězec kódovaný UTF-8 a pak kódování hello výsledek jako Base64.</span><span class="sxs-lookup"><span data-stu-id="61627-165">When you have hello signature string, encode it by using hello HMAC-SHA256 algorithm on hello UTF-8-encoded string, and then encode hello result as Base64.</span></span> <span data-ttu-id="61627-166">Použijte tento formát:</span><span class="sxs-lookup"><span data-stu-id="61627-166">Use this format:</span></span>

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

<span data-ttu-id="61627-167">Ukázky Hello v dalších částech hello mít ukázkový kód toohelp vytvoříte autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="61627-167">hello samples in hello next sections have sample code toohelp you create an authorization header.</span></span>

## <a name="request-body"></a><span data-ttu-id="61627-168">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="61627-168">Request body</span></span>
<span data-ttu-id="61627-169">Hello textu hello zprávy musí být ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="61627-169">hello body of hello message must be in JSON.</span></span> <span data-ttu-id="61627-170">Musí obsahovat jeden nebo více záznamů s hello vlastnost páry název-hodnota v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="61627-170">It must include one or more records with hello property name and value pairs in this format:</span></span>

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

<span data-ttu-id="61627-171">Více záznamů společně v jedné žádosti může batch pomocí hello formátu.</span><span class="sxs-lookup"><span data-stu-id="61627-171">You can batch multiple records together in a single request by using hello following format.</span></span> <span data-ttu-id="61627-172">Všechny záznamy hello musí být hello stejný typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="61627-172">All hello records must be hello same record type.</span></span>

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

## <a name="record-type-and-properties"></a><span data-ttu-id="61627-173">Typ záznamu a vlastnosti</span><span class="sxs-lookup"><span data-stu-id="61627-173">Record type and properties</span></span>
<span data-ttu-id="61627-174">Při odesílání dat prostřednictvím hello Log Analytics HTTP dat kolekce API definujete vlastní typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="61627-174">You define a custom record type when you submit data through hello Log Analytics HTTP Data Collector API.</span></span> <span data-ttu-id="61627-175">V současné době nelze zapisovat data tooexisting typy záznamů, které byly vytvořeny tak, že jiné datové typy a řešení.</span><span class="sxs-lookup"><span data-stu-id="61627-175">Currently, you can't write data tooexisting record types that were created by other data types and solutions.</span></span> <span data-ttu-id="61627-176">Analýzy protokolů přečte hello příchozích dat a poté vytvoří vlastnosti, které odpovídají hello datové typy hello hodnoty, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="61627-176">Log Analytics reads hello incoming data and then creates properties that match hello data types of hello values that you enter.</span></span>

<span data-ttu-id="61627-177">Každý požadavek toohello musí zahrnovat Log Analytics API **typ protokolu** záhlaví s názvem hello pro typ záznamu hello.</span><span class="sxs-lookup"><span data-stu-id="61627-177">Each request toohello Log Analytics API must include a **Log-Type** header with hello name for hello record type.</span></span> <span data-ttu-id="61627-178">přípona Hello **_CL** je automaticky připojením toohello název zadejte toodistinguish z jiných protokolu typy jako vlastní protokol.</span><span class="sxs-lookup"><span data-stu-id="61627-178">hello suffix **_CL** is automatically appended toohello name you enter toodistinguish it from other log types as a custom log.</span></span> <span data-ttu-id="61627-179">Například pokud zadáte název hello **MyNewRecordType**, analýzy protokolů vytvoří záznam s typem hello **MyNewRecordType_CL**.</span><span class="sxs-lookup"><span data-stu-id="61627-179">For example, if you enter hello name **MyNewRecordType**, Log Analytics creates a record with hello type **MyNewRecordType_CL**.</span></span> <span data-ttu-id="61627-180">To pomáhá zajistit, že neexistují žádné konflikty mezi názvy typů vytvořené uživatelem a ty poskytuje současný nebo budoucí řešení společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="61627-180">This helps ensure that there are no conflicts between user-created type names and those shipped in current or future Microsoft solutions.</span></span>

<span data-ttu-id="61627-181">tooidentify vlastnost datový typ, analýzy protokolů přidává vlastnost toohello příponu.</span><span class="sxs-lookup"><span data-stu-id="61627-181">tooidentify a property's data type, Log Analytics adds a suffix toohello property name.</span></span> <span data-ttu-id="61627-182">Pokud vlastnost obsahuje hodnotu null, hello vlastnost není součástí záznamů.</span><span class="sxs-lookup"><span data-stu-id="61627-182">If a property contains a null value, hello property is not included in that record.</span></span> <span data-ttu-id="61627-183">Tato tabulka uvádí hello vlastnost datový typ a odpovídající příponu:</span><span class="sxs-lookup"><span data-stu-id="61627-183">This table lists hello property data type and corresponding suffix:</span></span>

| <span data-ttu-id="61627-184">Datový typ vlastnosti</span><span class="sxs-lookup"><span data-stu-id="61627-184">Property data type</span></span> | <span data-ttu-id="61627-185">Přípona</span><span class="sxs-lookup"><span data-stu-id="61627-185">Suffix</span></span> |
|:--- |:--- |
| <span data-ttu-id="61627-186">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61627-186">String</span></span> |<span data-ttu-id="61627-187">_M</span><span class="sxs-lookup"><span data-stu-id="61627-187">_s</span></span> |
| <span data-ttu-id="61627-188">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="61627-188">Boolean</span></span> |<span data-ttu-id="61627-189">_B</span><span class="sxs-lookup"><span data-stu-id="61627-189">_b</span></span> |
| <span data-ttu-id="61627-190">Double</span><span class="sxs-lookup"><span data-stu-id="61627-190">Double</span></span> |<span data-ttu-id="61627-191">_d</span><span class="sxs-lookup"><span data-stu-id="61627-191">_d</span></span> |
| <span data-ttu-id="61627-192">Datum a čas</span><span class="sxs-lookup"><span data-stu-id="61627-192">Date/time</span></span> |<span data-ttu-id="61627-193">_T –</span><span class="sxs-lookup"><span data-stu-id="61627-193">_t</span></span> |
| <span data-ttu-id="61627-194">IDENTIFIKÁTOR GUID</span><span class="sxs-lookup"><span data-stu-id="61627-194">GUID</span></span> |<span data-ttu-id="61627-195">_g</span><span class="sxs-lookup"><span data-stu-id="61627-195">_g</span></span> |

<span data-ttu-id="61627-196">Hello datový typ, který používá analýzy protokolů pro každou vlastnost závisí na tom, jestli typ záznamu hello nový záznam o hello již existuje.</span><span class="sxs-lookup"><span data-stu-id="61627-196">hello data type that Log Analytics uses for each property depends on whether hello record type for hello new record already exists.</span></span>

* <span data-ttu-id="61627-197">Pokud typ záznamu hello neexistuje, analýzy protokolů vytvoří novou.</span><span class="sxs-lookup"><span data-stu-id="61627-197">If hello record type does not exist, Log Analytics creates a new one.</span></span> <span data-ttu-id="61627-198">Analýzy protokolů používá hello JSON typ odvození toodetermine hello datový typ pro každou vlastnost nový záznam o hello.</span><span class="sxs-lookup"><span data-stu-id="61627-198">Log Analytics uses hello JSON type inference toodetermine hello data type for each property for hello new record.</span></span>
* <span data-ttu-id="61627-199">Pokud typ záznamu hello neexistuje, analýzy protokolů pokusí toocreate nový záznam na základě existující vlastností.</span><span class="sxs-lookup"><span data-stu-id="61627-199">If hello record type does exist, Log Analytics attempts toocreate a new record based on existing properties.</span></span> <span data-ttu-id="61627-200">Pokud hello datový typ pro vlastnost v novém záznamu hello není odpovídají a nemůže být převedená toohello existující typ nebo pokud hello záznam obsahuje vlastnosti, která neexistuje, vytvoří novou vlastnost analýzy protokolů, který má příponu relevantní hello.</span><span class="sxs-lookup"><span data-stu-id="61627-200">If hello data type for a property in hello new record doesn’t match and can’t be converted toohello existing type, or if hello record includes a property that doesn’t exist, Log Analytics creates a new property that has hello relevant suffix.</span></span>

<span data-ttu-id="61627-201">Například by tato položka odeslání vytvořit záznam s třemi vlastnostmi **number_d**, **boolean_b**, a **string_s**:</span><span class="sxs-lookup"><span data-stu-id="61627-201">For example, this submission entry would create a record with three properties, **number_d**, **boolean_b**, and **string_s**:</span></span>

![Ukázka záznamu 1](media/log-analytics-data-collector-api/record-01.png)

<span data-ttu-id="61627-203">Pokud pak odeslání této další položky, hodnoty všech formátu řetězce, nebude změnit vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="61627-203">If you then submitted this next entry, with all values formatted as strings, hello properties would not change.</span></span> <span data-ttu-id="61627-204">Tyto hodnoty mohou být převedená tooexisting datové typy:</span><span class="sxs-lookup"><span data-stu-id="61627-204">These values can be converted tooexisting data types:</span></span>

![Ukázka záznamu 2](media/log-analytics-data-collector-api/record-02.png)

<span data-ttu-id="61627-206">Ale pokud jste provedli poté tento další odeslání, analýzy protokolů by vytvořit hello nové vlastnosti **boolean_d** a **string_d**.</span><span class="sxs-lookup"><span data-stu-id="61627-206">But, if you then made this next submission, Log Analytics would create hello new properties **boolean_d** and **string_d**.</span></span> <span data-ttu-id="61627-207">Nelze převést tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="61627-207">These values can't be converted:</span></span>

![Ukázka záznamu 3](media/log-analytics-data-collector-api/record-03.png)

<span data-ttu-id="61627-209">Pokud potom odeslán hello následující položky, před vytvořením hello typ záznamu, analýzy protokolů by vytvořit záznam s třemi vlastnostmi **úspěch**, **boolean_s**, a **string_s**.</span><span class="sxs-lookup"><span data-stu-id="61627-209">If you then submitted hello following entry, before hello record type was created, Log Analytics would create a record with three properties, **number_s**, **boolean_s**, and **string_s**.</span></span> <span data-ttu-id="61627-210">V této položce každé počáteční hodnoty hello je naformátovaná jako řetězec:</span><span class="sxs-lookup"><span data-stu-id="61627-210">In this entry, each of hello initial values is formatted as a string:</span></span>

![Ukázka záznamu 4](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a><span data-ttu-id="61627-212">Omezení dat</span><span class="sxs-lookup"><span data-stu-id="61627-212">Data limits</span></span>
<span data-ttu-id="61627-213">Existují některá omezení kolem hello data odeslány toohello Log Analytics Data kolekce rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="61627-213">There are some constraints around hello data posted toohello Log Analytics Data collection API.</span></span>

* <span data-ttu-id="61627-214">Maximálně 30 MB za post tooLog analýzy dat kolekce rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="61627-214">Maximum of 30 MB per post tooLog Analytics Data Collector API.</span></span> <span data-ttu-id="61627-215">Toto je omezení velikosti pro jednu metodu post.</span><span class="sxs-lookup"><span data-stu-id="61627-215">This is a size limit for a single post.</span></span> <span data-ttu-id="61627-216">Pokud hello data z jedné post, který je delší než 30 MB, je třeba rozdělit hello bloky dat si toosmaller velikosti a odešlete je současně.</span><span class="sxs-lookup"><span data-stu-id="61627-216">If hello data from a single post that exceeds 30 MB, you should split hello data up toosmaller sized chunks and send them concurrently.</span></span>
* <span data-ttu-id="61627-217">Maximální limit 32 KB pro pole hodnot.</span><span class="sxs-lookup"><span data-stu-id="61627-217">Maximum of 32 KB limit for field values.</span></span> <span data-ttu-id="61627-218">Pokud hodnota pole hello je větší než 32 KB, bude zkrácen hello data.</span><span class="sxs-lookup"><span data-stu-id="61627-218">If hello field value is greater than 32 KB, hello data will be truncated.</span></span>
* <span data-ttu-id="61627-219">Doporučený maximální počet polí pro daný typ je 50.</span><span class="sxs-lookup"><span data-stu-id="61627-219">Recommended maximum number of fields for a given type is 50.</span></span> <span data-ttu-id="61627-220">Toto je praktické omezení z perspektivy prostředí vyhledávání a použitelnost.</span><span class="sxs-lookup"><span data-stu-id="61627-220">This is a practical limit from a usability and search experience perspective.</span></span>  

## <a name="return-codes"></a><span data-ttu-id="61627-221">Návratové kódy</span><span class="sxs-lookup"><span data-stu-id="61627-221">Return codes</span></span>
<span data-ttu-id="61627-222">Hello stavový kód HTTP 200 znamená, že byla přijata ke zpracování tohoto požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="61627-222">hello HTTP status code 200 means that hello request has been received for processing.</span></span> <span data-ttu-id="61627-223">To znamená, že hello operace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="61627-223">This indicates that hello operation completed successfully.</span></span>

<span data-ttu-id="61627-224">Tato tabulka uvádí hello kompletní sadu stavové kódy, které může služba hello vrátit:</span><span class="sxs-lookup"><span data-stu-id="61627-224">This table lists hello complete set of status codes that hello service might return:</span></span>

| <span data-ttu-id="61627-225">Kód</span><span class="sxs-lookup"><span data-stu-id="61627-225">Code</span></span> | <span data-ttu-id="61627-226">Status</span><span class="sxs-lookup"><span data-stu-id="61627-226">Status</span></span> | <span data-ttu-id="61627-227">Kód chyby</span><span class="sxs-lookup"><span data-stu-id="61627-227">Error code</span></span> | <span data-ttu-id="61627-228">Popis</span><span class="sxs-lookup"><span data-stu-id="61627-228">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="61627-229">200</span><span class="sxs-lookup"><span data-stu-id="61627-229">200</span></span> |<span data-ttu-id="61627-230">OK</span><span class="sxs-lookup"><span data-stu-id="61627-230">OK</span></span> | |<span data-ttu-id="61627-231">Hello žádost byla úspěšně přijata.</span><span class="sxs-lookup"><span data-stu-id="61627-231">hello request was successfully accepted.</span></span> |
| <span data-ttu-id="61627-232">400</span><span class="sxs-lookup"><span data-stu-id="61627-232">400</span></span> |<span data-ttu-id="61627-233">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="61627-233">Bad request</span></span> |<span data-ttu-id="61627-234">InactiveCustomer</span><span class="sxs-lookup"><span data-stu-id="61627-234">InactiveCustomer</span></span> |<span data-ttu-id="61627-235">pracovní prostor Hello bylo ukončeno.</span><span class="sxs-lookup"><span data-stu-id="61627-235">hello workspace has been closed.</span></span> |
| <span data-ttu-id="61627-236">400</span><span class="sxs-lookup"><span data-stu-id="61627-236">400</span></span> |<span data-ttu-id="61627-237">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="61627-237">Bad request</span></span> |<span data-ttu-id="61627-238">InvalidApiVersion</span><span class="sxs-lookup"><span data-stu-id="61627-238">InvalidApiVersion</span></span> |<span data-ttu-id="61627-239">verze Hello rozhraní API, který jste zadali nebyla rozpoznána službou hello.</span><span class="sxs-lookup"><span data-stu-id="61627-239">hello API version that you specified was not recognized by hello service.</span></span> |
| <span data-ttu-id="61627-240">400</span><span class="sxs-lookup"><span data-stu-id="61627-240">400</span></span> |<span data-ttu-id="61627-241">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="61627-241">Bad request</span></span> |<span data-ttu-id="61627-242">InvalidCustomerId</span><span class="sxs-lookup"><span data-stu-id="61627-242">InvalidCustomerId</span></span> |<span data-ttu-id="61627-243">Zadané ID pracovního prostoru Hello je neplatný.</span><span class="sxs-lookup"><span data-stu-id="61627-243">hello workspace ID specified is invalid.</span></span> |
| <span data-ttu-id="61627-244">400</span><span class="sxs-lookup"><span data-stu-id="61627-244">400</span></span> |<span data-ttu-id="61627-245">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="61627-245">Bad request</span></span> |<span data-ttu-id="61627-246">InvalidDataFormat</span><span class="sxs-lookup"><span data-stu-id="61627-246">InvalidDataFormat</span></span> |<span data-ttu-id="61627-247">Byla odeslána neplatná JSON.</span><span class="sxs-lookup"><span data-stu-id="61627-247">Invalid JSON was submitted.</span></span> <span data-ttu-id="61627-248">text odpovědi Hello může obsahovat další informace o tom, jak tooresolve hello chyby.</span><span class="sxs-lookup"><span data-stu-id="61627-248">hello response body might contain more information about how tooresolve hello error.</span></span> |
| <span data-ttu-id="61627-249">400</span><span class="sxs-lookup"><span data-stu-id="61627-249">400</span></span> |<span data-ttu-id="61627-250">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="61627-250">Bad request</span></span> |<span data-ttu-id="61627-251">InvalidLogType</span><span class="sxs-lookup"><span data-stu-id="61627-251">InvalidLogType</span></span> |<span data-ttu-id="61627-252">Typ protokolu Hello zadat obsahují zvláštní znaky nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="61627-252">hello log type specified contained special characters or numerics.</span></span> |
| <span data-ttu-id="61627-253">400</span><span class="sxs-lookup"><span data-stu-id="61627-253">400</span></span> |<span data-ttu-id="61627-254">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="61627-254">Bad request</span></span> |<span data-ttu-id="61627-255">MissingApiVersion</span><span class="sxs-lookup"><span data-stu-id="61627-255">MissingApiVersion</span></span> |<span data-ttu-id="61627-256">verze rozhraní API Hello nebyl zadán.</span><span class="sxs-lookup"><span data-stu-id="61627-256">hello API version wasn’t specified.</span></span> |
| <span data-ttu-id="61627-257">400</span><span class="sxs-lookup"><span data-stu-id="61627-257">400</span></span> |<span data-ttu-id="61627-258">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="61627-258">Bad request</span></span> |<span data-ttu-id="61627-259">MissingContentType</span><span class="sxs-lookup"><span data-stu-id="61627-259">MissingContentType</span></span> |<span data-ttu-id="61627-260">Typ obsahu Hello nebyl zadán.</span><span class="sxs-lookup"><span data-stu-id="61627-260">hello content type wasn’t specified.</span></span> |
| <span data-ttu-id="61627-261">400</span><span class="sxs-lookup"><span data-stu-id="61627-261">400</span></span> |<span data-ttu-id="61627-262">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="61627-262">Bad request</span></span> |<span data-ttu-id="61627-263">MissingLogType</span><span class="sxs-lookup"><span data-stu-id="61627-263">MissingLogType</span></span> |<span data-ttu-id="61627-264">Hello vyžaduje protokolu typ hodnoty nebyl zadán.</span><span class="sxs-lookup"><span data-stu-id="61627-264">hello required value log type wasn’t specified.</span></span> |
| <span data-ttu-id="61627-265">400</span><span class="sxs-lookup"><span data-stu-id="61627-265">400</span></span> |<span data-ttu-id="61627-266">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="61627-266">Bad request</span></span> |<span data-ttu-id="61627-267">UnsupportedContentType</span><span class="sxs-lookup"><span data-stu-id="61627-267">UnsupportedContentType</span></span> |<span data-ttu-id="61627-268">Typ obsahu Hello nebyl nastaven příliš**application/json**.</span><span class="sxs-lookup"><span data-stu-id="61627-268">hello content type was not set too**application/json**.</span></span> |
| <span data-ttu-id="61627-269">403</span><span class="sxs-lookup"><span data-stu-id="61627-269">403</span></span> |<span data-ttu-id="61627-270">Je zakázané</span><span class="sxs-lookup"><span data-stu-id="61627-270">Forbidden</span></span> |<span data-ttu-id="61627-271">InvalidAuthorization</span><span class="sxs-lookup"><span data-stu-id="61627-271">InvalidAuthorization</span></span> |<span data-ttu-id="61627-272">Hello služby se nezdařilo tooauthenticate hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="61627-272">hello service failed tooauthenticate hello request.</span></span> <span data-ttu-id="61627-273">Ověřte platné klíči hello prostoru ID a připojení.</span><span class="sxs-lookup"><span data-stu-id="61627-273">Verify that hello workspace ID and connection key are valid.</span></span> |
| <span data-ttu-id="61627-274">404</span><span class="sxs-lookup"><span data-stu-id="61627-274">404</span></span> |<span data-ttu-id="61627-275">Nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="61627-275">Not Found</span></span> | | <span data-ttu-id="61627-276">Buď zadaná adresa URL hello je nesprávný, nebo hello požadavku je příliš velký.</span><span class="sxs-lookup"><span data-stu-id="61627-276">Either hello URL provided is incorrect, or hello request is too large.</span></span> |
| <span data-ttu-id="61627-277">429</span><span class="sxs-lookup"><span data-stu-id="61627-277">429</span></span> |<span data-ttu-id="61627-278">Příliš mnoho požadavků</span><span class="sxs-lookup"><span data-stu-id="61627-278">Too Many Requests</span></span> | | <span data-ttu-id="61627-279">Služba Hello dochází k velkému počtu data z účtu.</span><span class="sxs-lookup"><span data-stu-id="61627-279">hello service is experiencing a high volume of data from your account.</span></span> <span data-ttu-id="61627-280">Hello požadavek opakujte akci později.</span><span class="sxs-lookup"><span data-stu-id="61627-280">Please retry hello request later.</span></span> |
| <span data-ttu-id="61627-281">500</span><span class="sxs-lookup"><span data-stu-id="61627-281">500</span></span> |<span data-ttu-id="61627-282">Vnitřní chyba serveru</span><span class="sxs-lookup"><span data-stu-id="61627-282">Internal Server Error</span></span> |<span data-ttu-id="61627-283">UnspecifiedError</span><span class="sxs-lookup"><span data-stu-id="61627-283">UnspecifiedError</span></span> |<span data-ttu-id="61627-284">Hello služby došlo k vnitřní chybě.</span><span class="sxs-lookup"><span data-stu-id="61627-284">hello service encountered an internal error.</span></span> <span data-ttu-id="61627-285">Zkuste provést požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="61627-285">Please retry hello request.</span></span> |
| <span data-ttu-id="61627-286">503</span><span class="sxs-lookup"><span data-stu-id="61627-286">503</span></span> |<span data-ttu-id="61627-287">Služba není k dispozici</span><span class="sxs-lookup"><span data-stu-id="61627-287">Service Unavailable</span></span> |<span data-ttu-id="61627-288">ServiceUnavailable</span><span class="sxs-lookup"><span data-stu-id="61627-288">ServiceUnavailable</span></span> |<span data-ttu-id="61627-289">Hello služba je aktuálně k dispozici tooreceive požadavky.</span><span class="sxs-lookup"><span data-stu-id="61627-289">hello service currently is unavailable tooreceive requests.</span></span> <span data-ttu-id="61627-290">Opakujte žádost.</span><span class="sxs-lookup"><span data-stu-id="61627-290">Please retry your request.</span></span> |

## <a name="query-data"></a><span data-ttu-id="61627-291">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="61627-291">Query data</span></span>
<span data-ttu-id="61627-292">tooquery data odeslaná při hello Log Analytics HTTP dat kolekce API, vyhledejte záznamy s **typ** který je rovna toohello **LogType** hodnotu, která jste zadali, spolu s **_CL**.</span><span class="sxs-lookup"><span data-stu-id="61627-292">tooquery data submitted by hello Log Analytics HTTP Data Collector API, search for records with **Type** that is equal toohello **LogType** value that you specified, appended with **_CL**.</span></span> <span data-ttu-id="61627-293">Pokud jste použili například **MyCustomLog**, pak by vrátit všechny záznamy s **typ = MyCustomLog_CL**.</span><span class="sxs-lookup"><span data-stu-id="61627-293">For example, if you used **MyCustomLog**, then you'd return all records with **Type=MyCustomLog_CL**.</span></span>

>[!NOTE]
> <span data-ttu-id="61627-294">Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak hello výše dotazu by změňte následující toohello.</span><span class="sxs-lookup"><span data-stu-id="61627-294">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then hello above query would change toohello following.</span></span>

> `MyCustomLog_CL`

## <a name="sample-requests"></a><span data-ttu-id="61627-295">Ukázka požadavků</span><span class="sxs-lookup"><span data-stu-id="61627-295">Sample requests</span></span>
<span data-ttu-id="61627-296">V dalších částech hello, budete najít ukázky jak toosubmit data toohello Log Analytics HTTP dat kolekce API pomocí různých programovacích jazyků.</span><span class="sxs-lookup"><span data-stu-id="61627-296">In hello next sections, you'll find samples of how toosubmit data toohello Log Analytics HTTP Data Collector API by using different programming languages.</span></span>

<span data-ttu-id="61627-297">Pro každý vzorek proveďte tyto kroky tooset hello proměnných pro hello autorizační hlavičky:</span><span class="sxs-lookup"><span data-stu-id="61627-297">For each sample, do these steps tooset hello variables for hello authorization header:</span></span>

1. <span data-ttu-id="61627-298">V hello portál Operations Management Suite, vyberte hello **nastavení** dlaždici a potom vyberte hello **připojené zdroje** kartě.</span><span class="sxs-lookup"><span data-stu-id="61627-298">In hello Operations Management Suite portal, select hello **Settings** tile, and then select hello **Connected Sources** tab.</span></span>
2. <span data-ttu-id="61627-299">toohello napravo od **ID pracovního prostoru**, vyberte ikonu kopírování hello a vložte hello ID jako hodnota hello hello **ID zákazníka** proměnné.</span><span class="sxs-lookup"><span data-stu-id="61627-299">toohello right of **Workspace ID**, select hello copy icon, and then paste hello ID as hello value of hello **Customer ID** variable.</span></span>
3. <span data-ttu-id="61627-300">toohello napravo od **primární klíč**, vyberte ikonu kopírování hello a vložte hello ID jako hodnota hello hello **sdílený klíč** proměnné.</span><span class="sxs-lookup"><span data-stu-id="61627-300">toohello right of **Primary Key**, select hello copy icon, and then paste hello ID as hello value of hello **Shared Key** variable.</span></span>

<span data-ttu-id="61627-301">Alternativně můžete změnit hello proměnných pro typ protokolu hello a JSON data.</span><span class="sxs-lookup"><span data-stu-id="61627-301">Alternatively, you can change hello variables for hello log type and JSON data.</span></span>

### <a name="powershell-sample"></a><span data-ttu-id="61627-302">Ukázkové prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="61627-302">PowerShell sample</span></span>
```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify hello name of hello record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with hello created time for hello records
$TimeStampField = "DateValue"


# Create two records with hello same set of properties toocreate
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

# Create hello function toocreate hello authorization signature
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


# Create hello function toocreate and post hello request
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

# Submit hello data toohello API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a><span data-ttu-id="61627-303">Ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="61627-303">C# sample</span></span>
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

        // Update customerId tooyour Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either hello primary or hello secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of hello event type that is being submitted tooLog Analytics
        static string LogName = "DemoExample";

        // You can use an optional field toospecify hello timestamp from hello data. If hello time field is not specified, Log Analytics assumes hello time is hello message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for hello API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build hello API signature
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

        // Send a request toohello POST API endpoint
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

### <a name="python-sample"></a><span data-ttu-id="61627-304">Ukázka Pythonu</span><span class="sxs-lookup"><span data-stu-id="61627-304">Python sample</span></span>
```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update hello customer ID tooyour Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For hello shared key, use either hello primary or hello secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# hello log type is hello name of hello event that is being submitted
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

# Build hello API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request toohello POST API
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

## <a name="next-steps"></a><span data-ttu-id="61627-305">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61627-305">Next steps</span></span>
- <span data-ttu-id="61627-306">Použití hello [rozhraní API pro vyhledávání protokolu](log-analytics-log-search-api.md) tooretrieve data z úložiště analýzy protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="61627-306">Use hello [Log Search API](log-analytics-log-search-api.md) tooretrieve data from hello Log Analytics repository.</span></span>
