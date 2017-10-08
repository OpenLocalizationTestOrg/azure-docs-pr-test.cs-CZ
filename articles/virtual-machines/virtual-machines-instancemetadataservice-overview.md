---
title: "Přehled služby Metadata Instance aaaAzure | Microsoft Docs"
description: "Rozhraní rESTful tooget informace o výpočetní, síťové a nadcházející údržbě události Virtuálního počítače."
services: virtual-machines-windows, virtual-machines-linux,virtual-machines-scale-sets, cloud-services
documentationcenter: virtual-machines
author: harijay
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 03/27/2017
ms.author: harijay
ms.openlocfilehash: e87cdf28f80b9ef8cc566b637549c48846862f0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-instance-metadata-service"></a><span data-ttu-id="88db1-103">Služba Azure Instance metadat</span><span class="sxs-lookup"><span data-stu-id="88db1-103">Azure Instance Metadata Service</span></span> 


<span data-ttu-id="88db1-104">Hello služby metadat instanci Azure poskytuje informace o spuštěných instancí virtuálního počítače, které se dají použít toomanage a konfigurovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="88db1-104">hello Azure Instance Metadata Service provides information about running virtual machine instances that can be used toomanage and configure your virtual machines.</span></span>
<span data-ttu-id="88db1-105">To zahrnuje informace, jako je SKU, konfigurace sítě a události nadcházející údržby.</span><span class="sxs-lookup"><span data-stu-id="88db1-105">This includes information such as SKU, network configuration, and upcoming maintenance events.</span></span> <span data-ttu-id="88db1-106">Další informace o jaké typy informací je k dispozici, najdete v části [metadata kategorie](#instance-metadata-data-categories).</span><span class="sxs-lookup"><span data-stu-id="88db1-106">For more information on what type of information is available, see [metadata categories](#instance-metadata-data-categories).</span></span>

<span data-ttu-id="88db1-107">Služba Azure Instance metadat je koncový bod REST tooall dostupné virtuální počítače IaaS vytvořené prostřednictvím hello [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="88db1-107">Azure's Instance Metadata Service is a REST Endpoint accessible tooall IaaS VMs created via hello [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="88db1-108">koncový bod Hello je dostupný v dobře známé směrovat IP adresu (`169.254.169.254`), můžete přistupovat pouze z v rámci hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="88db1-108">hello endpoint is available at a well-known non-routable IP address (`169.254.169.254`) that can be accessed only from within hello VM.</span></span>

### <a name="important-information"></a><span data-ttu-id="88db1-109">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="88db1-109">Important information</span></span>

<span data-ttu-id="88db1-110">Tato služba je **všeobecně dostupná** v globální oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="88db1-110">This service is  **generally available** in Global Azure Regions.</span></span> <span data-ttu-id="88db1-111">Je ve verzi Public preview pro státní, Čína a Cloud Azure němčina.</span><span class="sxs-lookup"><span data-stu-id="88db1-111">It is in Public preview for Government, China, and German Azure Cloud.</span></span> <span data-ttu-id="88db1-112">Pravidelně obdrží aktualizace tooexpose nové informace o instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="88db1-112">It regularly receives updates tooexpose new information about virtual machine instances.</span></span> <span data-ttu-id="88db1-113">Tato stránka se vztahuje k hello aktuální [kategorie dat](#instance-metadata-data-categories) k dispozici.</span><span class="sxs-lookup"><span data-stu-id="88db1-113">This page reflects hello up-to-date [data categories](#instance-metadata-data-categories) available.</span></span>

## <a name="service-availability"></a><span data-ttu-id="88db1-114">Dostupnost služby</span><span class="sxs-lookup"><span data-stu-id="88db1-114">Service Availability</span></span>
<span data-ttu-id="88db1-115">Služba Hello je k dispozici ve všech oblastech všeobecně dostupná globální Azure.</span><span class="sxs-lookup"><span data-stu-id="88db1-115">hello service is available in all generally available Global Azure regions.</span></span> <span data-ttu-id="88db1-116">Služba Hello je ve verzi public preview v oblastech Government, Čína nebo Německo hello.</span><span class="sxs-lookup"><span data-stu-id="88db1-116">hello service is in public preview  in hello Government, China, or Germany regions.</span></span>

<span data-ttu-id="88db1-117">Oblasti</span><span class="sxs-lookup"><span data-stu-id="88db1-117">Regions</span></span>                                        | <span data-ttu-id="88db1-118">Dostupnost?</span><span class="sxs-lookup"><span data-stu-id="88db1-118">Availability?</span></span>
-----------------------------------------------|-----------------------------------------------
[<span data-ttu-id="88db1-119">Všechny všeobecně dostupná globální oblasti Azure</span><span class="sxs-lookup"><span data-stu-id="88db1-119">All Generally Available Global Azure Regions</span></span>](https://azure.microsoft.com/en-us/regions/)     | <span data-ttu-id="88db1-120">Obecně k dispozici</span><span class="sxs-lookup"><span data-stu-id="88db1-120">Generally Available</span></span> 
[<span data-ttu-id="88db1-121">Azure Government</span><span class="sxs-lookup"><span data-stu-id="88db1-121">Azure Government</span></span>](https://azure.microsoft.com/en-us/overview/clouds/government/)              | <span data-ttu-id="88db1-122">Ve verzi Preview</span><span class="sxs-lookup"><span data-stu-id="88db1-122">In Preview</span></span> 
[<span data-ttu-id="88db1-123">Azure Čína</span><span class="sxs-lookup"><span data-stu-id="88db1-123">Azure China</span></span>](https://www.azure.cn/)                                                           | <span data-ttu-id="88db1-124">Ve verzi Preview</span><span class="sxs-lookup"><span data-stu-id="88db1-124">In Preview</span></span>
[<span data-ttu-id="88db1-125">Azure Německo</span><span class="sxs-lookup"><span data-stu-id="88db1-125">Azure Germany</span></span>](https://azure.microsoft.com/en-us/overview/clouds/germany/)                    | <span data-ttu-id="88db1-126">Ve verzi Preview</span><span class="sxs-lookup"><span data-stu-id="88db1-126">In Preview</span></span>

<span data-ttu-id="88db1-127">Tato tabulka je aktualizována při hello služby k dispozici v ostatních cloudů Azure.</span><span class="sxs-lookup"><span data-stu-id="88db1-127">This table is updated when hello service becomes available in other Azure clouds.</span></span>

<span data-ttu-id="88db1-128">tootry out hello Instance služby metadat, vytvoření virtuálního počítače z [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) nebo hello [portál Azure](http://portal.azure.com) v hello nad oblasti a postupujte podle níže uvedených příkladech hello.</span><span class="sxs-lookup"><span data-stu-id="88db1-128">tootry out hello Instance Metadata Service, create a VM from [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) or hello [Azure portal](http://portal.azure.com) in hello above regions and follow hello examples below.</span></span>

## <a name="usage"></a><span data-ttu-id="88db1-129">Využití</span><span class="sxs-lookup"><span data-stu-id="88db1-129">Usage</span></span>

### <a name="versioning"></a><span data-ttu-id="88db1-130">Správa verzí</span><span class="sxs-lookup"><span data-stu-id="88db1-130">Versioning</span></span>
<span data-ttu-id="88db1-131">Hello Instance služby metadat je verzí.</span><span class="sxs-lookup"><span data-stu-id="88db1-131">hello Instance Metadata Service is versioned.</span></span> <span data-ttu-id="88db1-132">Verze jsou povinné a hello aktuální verze je `2017-04-02`.</span><span class="sxs-lookup"><span data-stu-id="88db1-132">Versions are mandatory and hello current version is `2017-04-02`.</span></span>

> [!NOTE] 
> <span data-ttu-id="88db1-133">Předchozí verze preview naplánované událostí podporovaných {nejnovější} jako hello api-version.</span><span class="sxs-lookup"><span data-stu-id="88db1-133">Previous preview releases of scheduled events supported {latest} as hello api-version.</span></span> <span data-ttu-id="88db1-134">Tento formát se už nepodporuje a bude v budoucí hello nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="88db1-134">This format is no longer supported and will be deprecated in hello future.</span></span>

<span data-ttu-id="88db1-135">Jako přidáme novější verze, starší verze se dá dál dostat z důvodu kompatibility Pokud skripty mají závislosti na konkrétní data formátů.</span><span class="sxs-lookup"><span data-stu-id="88db1-135">As we add newer versions, older versions can still be accessed for compatibility if your scripts have dependencies on specific data formats.</span></span> <span data-ttu-id="88db1-136">Uvědomte si však, že tento hello aktuální preview version(2017-03-01) nemusí být k dispozici, jakmile služba hello je obecně dostupná.</span><span class="sxs-lookup"><span data-stu-id="88db1-136">However, note that hello current preview version(2017-03-01) may not be available once hello service is generally available.</span></span>

### <a name="using-headers"></a><span data-ttu-id="88db1-137">Používání hlaviček</span><span class="sxs-lookup"><span data-stu-id="88db1-137">Using Headers</span></span>
<span data-ttu-id="88db1-138">Když dotazujete hello Instance Metadata služby, je nutné zadat hello záhlaví `Metadata: true` tooensure hello požadavek nebyl přesměrován náhodně.</span><span class="sxs-lookup"><span data-stu-id="88db1-138">When you query hello Instance Metadata Service, you must provide hello header `Metadata: true` tooensure hello request was not unintentionally redirected.</span></span>

### <a name="retrieving-metadata"></a><span data-ttu-id="88db1-139">Načítání metadat</span><span class="sxs-lookup"><span data-stu-id="88db1-139">Retrieving metadata</span></span>

<span data-ttu-id="88db1-140">Instance metadata jsou k dispozici pro spouštění virtuálních počítačů vytvořena nebo spravovat pomocí [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="88db1-140">Instance metadata is available for running VMs created/managed using [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="88db1-141">Přístup k všechny kategorie dat pro instanci virtuálního počítače pomocí hello následující požadavek:</span><span class="sxs-lookup"><span data-stu-id="88db1-141">Access all data categories for a virtual machine instance using hello following request:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> <span data-ttu-id="88db1-142">Všechny dotazy metadat instance rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="88db1-142">All instance metadata queries are case-sensitive.</span></span>

### <a name="data-output"></a><span data-ttu-id="88db1-143">Výstup dat</span><span class="sxs-lookup"><span data-stu-id="88db1-143">Data output</span></span>
<span data-ttu-id="88db1-144">Ve výchozím nastavení, hello Instance Metadata služby vrací data ve formátu JSON (`Content-Type: application/json`).</span><span class="sxs-lookup"><span data-stu-id="88db1-144">By default, hello Instance Metadata Service returns data in JSON format (`Content-Type: application/json`).</span></span> <span data-ttu-id="88db1-145">Rozhraní API pro různé však může vrátit data v různých formátech Pokud požadovaný.</span><span class="sxs-lookup"><span data-stu-id="88db1-145">However, different APIs can return data in different formats if requested.</span></span>
<span data-ttu-id="88db1-146">Hello následující tabulce je odkazem na jiných formátů dat, které můžou podporovat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="88db1-146">hello following table is a reference of other data formats APIs may support.</span></span>

<span data-ttu-id="88db1-147">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="88db1-147">API</span></span> | <span data-ttu-id="88db1-148">Výchozí formát dat</span><span class="sxs-lookup"><span data-stu-id="88db1-148">Default Data Format</span></span> | <span data-ttu-id="88db1-149">Ostatní formáty</span><span class="sxs-lookup"><span data-stu-id="88db1-149">Other Formats</span></span>
--------|---------------------|--------------
<span data-ttu-id="88db1-150">/instance</span><span class="sxs-lookup"><span data-stu-id="88db1-150">/instance</span></span> | <span data-ttu-id="88db1-151">JSON</span><span class="sxs-lookup"><span data-stu-id="88db1-151">json</span></span> | <span data-ttu-id="88db1-152">Text</span><span class="sxs-lookup"><span data-stu-id="88db1-152">text</span></span>
<span data-ttu-id="88db1-153">/scheduledevents</span><span class="sxs-lookup"><span data-stu-id="88db1-153">/scheduledevents</span></span> | <span data-ttu-id="88db1-154">JSON</span><span class="sxs-lookup"><span data-stu-id="88db1-154">json</span></span> | <span data-ttu-id="88db1-155">None</span><span class="sxs-lookup"><span data-stu-id="88db1-155">none</span></span>

<span data-ttu-id="88db1-156">tooaccess odpovědi jiné než výchozí formát, zadejte požadovaný formát hello jako parametr řetězce dotazu v žádosti o hello.</span><span class="sxs-lookup"><span data-stu-id="88db1-156">tooaccess a non-default response format, specify hello requested format as a querystring parameter in hello request.</span></span> <span data-ttu-id="88db1-157">Například:</span><span class="sxs-lookup"><span data-stu-id="88db1-157">For example:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a><span data-ttu-id="88db1-158">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="88db1-158">Security</span></span>
<span data-ttu-id="88db1-159">koncový bod služby Metadata Instance Hello je dostupné pouze v aplikaci hello spuštěna instance virtuálního počítače na směrovat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="88db1-159">hello Instance Metadata Service endpoint is accessible only from within hello running virtual machine instance on a non-routable IP address.</span></span> <span data-ttu-id="88db1-160">Kromě toho každá žádost s `X-Forwarded-For` záhlaví byl odmítnut službou hello.</span><span class="sxs-lookup"><span data-stu-id="88db1-160">In addition, any request with a `X-Forwarded-For` header is rejected by hello service.</span></span>
<span data-ttu-id="88db1-161">Je také nutné toocontain požadavky `Metadata: true` tooensure hlavičky, která hello skutečné žádosti byl přímo určený a není součástí neúmyslnému přesměrování.</span><span class="sxs-lookup"><span data-stu-id="88db1-161">We also require requests toocontain a `Metadata: true` header tooensure that hello actual request was directly intended and not a part of unintentional redirection.</span></span> 

### <a name="error"></a><span data-ttu-id="88db1-162">Chyba</span><span class="sxs-lookup"><span data-stu-id="88db1-162">Error</span></span>
<span data-ttu-id="88db1-163">Pokud je datový prvek nebyl nalezen nebo chybně vytvořený požadavek, vrátí hello Instance Metadata služby standardní chyby protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="88db1-163">If there is a data element not found or a malformed request, hello Instance Metadata Service returns standard HTTP errors.</span></span> <span data-ttu-id="88db1-164">Například:</span><span class="sxs-lookup"><span data-stu-id="88db1-164">For example:</span></span>

<span data-ttu-id="88db1-165">Kód stavu HTTP</span><span class="sxs-lookup"><span data-stu-id="88db1-165">HTTP Status Code</span></span> | <span data-ttu-id="88db1-166">Důvod</span><span class="sxs-lookup"><span data-stu-id="88db1-166">Reason</span></span>
----------------|-------
<span data-ttu-id="88db1-167">200 OK</span><span class="sxs-lookup"><span data-stu-id="88db1-167">200 OK</span></span> |
<span data-ttu-id="88db1-168">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="88db1-168">400 Bad Request</span></span> | <span data-ttu-id="88db1-169">Chybí `Metadata: true` záhlaví</span><span class="sxs-lookup"><span data-stu-id="88db1-169">Missing `Metadata: true` header</span></span>
<span data-ttu-id="88db1-170">404 – Nenalezeno</span><span class="sxs-lookup"><span data-stu-id="88db1-170">404 Not Found</span></span> | <span data-ttu-id="88db1-171">Existují technologie Hello položka hierarchyinfoguid požadovaný element</span><span class="sxs-lookup"><span data-stu-id="88db1-171">hello requested element does't exist</span></span> 
<span data-ttu-id="88db1-172">405 – Metoda není povoleno</span><span class="sxs-lookup"><span data-stu-id="88db1-172">405 Method Not Allowed</span></span> | <span data-ttu-id="88db1-173">Pouze `GET` a `POST` jsou podporovány požadavky</span><span class="sxs-lookup"><span data-stu-id="88db1-173">Only `GET` and `POST` requests are supported</span></span>
<span data-ttu-id="88db1-174">429 příliš mnoho požadavků</span><span class="sxs-lookup"><span data-stu-id="88db1-174">429 Too Many Requests</span></span> | <span data-ttu-id="88db1-175">Hello rozhraní API aktuálně podporuje maximálně 5 dotazů za sekundu</span><span class="sxs-lookup"><span data-stu-id="88db1-175">hello API currently supports a maximum of 5 queries per second</span></span>
<span data-ttu-id="88db1-176">Chyba 500 služby</span><span class="sxs-lookup"><span data-stu-id="88db1-176">500 Service Error</span></span>     | <span data-ttu-id="88db1-177">Po určité době opakujte</span><span class="sxs-lookup"><span data-stu-id="88db1-177">Retry after some time</span></span>

### <a name="examples"></a><span data-ttu-id="88db1-178">Příklady</span><span class="sxs-lookup"><span data-stu-id="88db1-178">Examples</span></span>

> [!NOTE] 
> <span data-ttu-id="88db1-179">Všechny odpovědi rozhraní API jsou řetězce formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="88db1-179">All API responses are JSON strings.</span></span> <span data-ttu-id="88db1-180">Jsou všechny tyto odpovědi příklad pretty vytisknout čitelnější.</span><span class="sxs-lookup"><span data-stu-id="88db1-180">All following example responses  are pretty-printed for readability.</span></span>

#### <a name="retrieving-network-information"></a><span data-ttu-id="88db1-181">Načítání informací o síti</span><span class="sxs-lookup"><span data-stu-id="88db1-181">Retrieving network information</span></span>

<span data-ttu-id="88db1-182">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="88db1-182">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

<span data-ttu-id="88db1-183">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="88db1-183">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="88db1-184">odpověď Hello je řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="88db1-184">hello response is a JSON string.</span></span> <span data-ttu-id="88db1-185">Následující příklad odpovědi Hello je pretty vytisknout čitelnější.</span><span class="sxs-lookup"><span data-stu-id="88db1-185">hello following example response is pretty-printed for readability.</span></span>

```
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a><span data-ttu-id="88db1-186">Načítání veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="88db1-186">Retrieving public IP address</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a><span data-ttu-id="88db1-187">Načítání metadat všechny instance</span><span class="sxs-lookup"><span data-stu-id="88db1-187">Retrieving all metadata for an instance</span></span>

<span data-ttu-id="88db1-188">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="88db1-188">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

<span data-ttu-id="88db1-189">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="88db1-189">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="88db1-190">odpověď Hello je řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="88db1-190">hello response is a JSON string.</span></span> <span data-ttu-id="88db1-191">Následující příklad odpovědi Hello je pretty vytisknout čitelnější.</span><span class="sxs-lookup"><span data-stu-id="88db1-191">hello following example response is pretty-printed for readability.</span></span>

```
{
  "compute": {
    "location": "westcentralus",
    "name": "IMDSSample",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "Canonical",
    "sku": "16.04.0-LTS",
    "version": "16.04.201610200",
    "vmId": "5d33a910-a7a0-4443-9f01-6a807801b29b",
    "vmSize": "Standard_A1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.0.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.0.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3AF806EC"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a><span data-ttu-id="88db1-192">Načítání metadat virtuálního počítače ve Windows</span><span class="sxs-lookup"><span data-stu-id="88db1-192">Retrieving metadata in Windows Virtual Machine</span></span>

<span data-ttu-id="88db1-193">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="88db1-193">**Request**</span></span>

<span data-ttu-id="88db1-194">Nejde načíst instance metadata v systému Windows prostřednictvím hello nástroj PowerShell `curl`:</span><span class="sxs-lookup"><span data-stu-id="88db1-194">Instance metadata can be retrieved in Windows via hello PowerShell utility `curl`:</span></span> 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

<span data-ttu-id="88db1-195">Nebo prostřednictvím hello `Invoke-RestMethod` rutiny:</span><span class="sxs-lookup"><span data-stu-id="88db1-195">Or through hello `Invoke-RestMethod` cmdlet:</span></span>
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

<span data-ttu-id="88db1-196">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="88db1-196">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="88db1-197">odpověď Hello je řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="88db1-197">hello response is a JSON string.</span></span> <span data-ttu-id="88db1-198">Následující příklad odpovědi Hello je pretty vytisknout čitelnější.</span><span class="sxs-lookup"><span data-stu-id="88db1-198">hello following example response  is pretty-printed for readability.</span></span>

```
{
  "compute": {
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": [
            
          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a><span data-ttu-id="88db1-199">Kategorie dat instance metadat</span><span class="sxs-lookup"><span data-stu-id="88db1-199">Instance metadata data categories</span></span>
<span data-ttu-id="88db1-200">Hello následující kategorie dat jsou k dispozici prostřednictvím hello Instance Metadata služby:</span><span class="sxs-lookup"><span data-stu-id="88db1-200">hello following data categories are available through hello Instance Metadata Service:</span></span>

<span data-ttu-id="88db1-201">Data</span><span class="sxs-lookup"><span data-stu-id="88db1-201">Data</span></span> | <span data-ttu-id="88db1-202">Popis</span><span class="sxs-lookup"><span data-stu-id="88db1-202">Description</span></span>
-----|------------
<span data-ttu-id="88db1-203">location</span><span class="sxs-lookup"><span data-stu-id="88db1-203">location</span></span> | <span data-ttu-id="88db1-204">Azure oblast hello virtuální počítač spuštěný</span><span class="sxs-lookup"><span data-stu-id="88db1-204">Azure Region hello VM is running in</span></span>
<span data-ttu-id="88db1-205">jméno</span><span class="sxs-lookup"><span data-stu-id="88db1-205">name</span></span> | <span data-ttu-id="88db1-206">Název hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="88db1-206">Name of hello VM</span></span> 
<span data-ttu-id="88db1-207">Nabídka</span><span class="sxs-lookup"><span data-stu-id="88db1-207">offer</span></span> | <span data-ttu-id="88db1-208">Nabízí informace o hello image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="88db1-208">Offer information for hello VM image.</span></span> <span data-ttu-id="88db1-209">Tato hodnota je jenom pro Image nasadit z Galerie obrázků Azure k dispozici.</span><span class="sxs-lookup"><span data-stu-id="88db1-209">This value is only present for images deployed from Azure image gallery.</span></span>
<span data-ttu-id="88db1-210">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="88db1-210">publisher</span></span> | <span data-ttu-id="88db1-211">Vydavatel hello image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="88db1-211">Publisher of hello VM image</span></span>
<span data-ttu-id="88db1-212">SKU</span><span class="sxs-lookup"><span data-stu-id="88db1-212">sku</span></span> | <span data-ttu-id="88db1-213">Konkrétní SKU pro hello image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="88db1-213">Specific SKU for hello VM image</span></span>  
<span data-ttu-id="88db1-214">Verze</span><span class="sxs-lookup"><span data-stu-id="88db1-214">version</span></span> | <span data-ttu-id="88db1-215">Verze bitové kopie virtuálních počítačů hello</span><span class="sxs-lookup"><span data-stu-id="88db1-215">Version of hello VM image</span></span> 
<span data-ttu-id="88db1-216">osType</span><span class="sxs-lookup"><span data-stu-id="88db1-216">osType</span></span> | <span data-ttu-id="88db1-217">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="88db1-217">Linux or Windows</span></span> 
<span data-ttu-id="88db1-218">platformUpdateDomain</span><span class="sxs-lookup"><span data-stu-id="88db1-218">platformUpdateDomain</span></span> |  <span data-ttu-id="88db1-219">[Aktualizace domény](virtual-machines-windows-manage-availability.md) hello virtuální počítač spuštěný v</span><span class="sxs-lookup"><span data-stu-id="88db1-219">[Update domain](virtual-machines-windows-manage-availability.md) hello VM is running in</span></span>
<span data-ttu-id="88db1-220">platformFaultDomain</span><span class="sxs-lookup"><span data-stu-id="88db1-220">platformFaultDomain</span></span> | <span data-ttu-id="88db1-221">[Doména selhání](virtual-machines-windows-manage-availability.md) hello virtuální počítač spuštěný v</span><span class="sxs-lookup"><span data-stu-id="88db1-221">[Fault domain](virtual-machines-windows-manage-availability.md) hello VM is running in</span></span>
<span data-ttu-id="88db1-222">vmId</span><span class="sxs-lookup"><span data-stu-id="88db1-222">vmId</span></span> | <span data-ttu-id="88db1-223">[Jedinečný identifikátor](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) pro hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="88db1-223">[Unique identifier](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) for hello VM</span></span>
<span data-ttu-id="88db1-224">vmSize</span><span class="sxs-lookup"><span data-stu-id="88db1-224">vmSize</span></span> | [<span data-ttu-id="88db1-225">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="88db1-225">VM size</span></span>](virtual-machines-windows-sizes.md)
<span data-ttu-id="88db1-226">IPv4/privateIpAddress</span><span class="sxs-lookup"><span data-stu-id="88db1-226">ipv4/privateIpAddress</span></span> | <span data-ttu-id="88db1-227">Místní adresa IPv4 hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="88db1-227">Local IPv4 address of hello VM</span></span> 
<span data-ttu-id="88db1-228">IPv4/publicIpAddress</span><span class="sxs-lookup"><span data-stu-id="88db1-228">ipv4/publicIpAddress</span></span> | <span data-ttu-id="88db1-229">Veřejnou IPv4 adresu hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="88db1-229">Public IPv4 address of hello VM</span></span>
<span data-ttu-id="88db1-230">Adresa podsítě /</span><span class="sxs-lookup"><span data-stu-id="88db1-230">subnet/address</span></span> | <span data-ttu-id="88db1-231">Adresa podsítě hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="88db1-231">Subnet address of hello VM</span></span>
<span data-ttu-id="88db1-232">Předpona podsítě /</span><span class="sxs-lookup"><span data-stu-id="88db1-232">subnet/prefix</span></span> | <span data-ttu-id="88db1-233">Předpona podsítě, například 24</span><span class="sxs-lookup"><span data-stu-id="88db1-233">Subnet prefix, example 24</span></span>
<span data-ttu-id="88db1-234">IPv6 nebo adresa IP</span><span class="sxs-lookup"><span data-stu-id="88db1-234">ipv6/ipAddress</span></span> | <span data-ttu-id="88db1-235">Místní adresa IPv6 hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="88db1-235">Local IPv6 address of hello VM</span></span>
<span data-ttu-id="88db1-236">MacAddress</span><span class="sxs-lookup"><span data-stu-id="88db1-236">macAddress</span></span> | <span data-ttu-id="88db1-237">Adresa mac virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="88db1-237">VM mac address</span></span> 
<span data-ttu-id="88db1-238">scheduledevents</span><span class="sxs-lookup"><span data-stu-id="88db1-238">scheduledevents</span></span> | <span data-ttu-id="88db1-239">Aktuálně ve veřejné verzi Preview najdete [scheduledevents](virtual-machines-scheduled-events.md)</span><span class="sxs-lookup"><span data-stu-id="88db1-239">Currently in Public Preview See [scheduledevents](virtual-machines-scheduled-events.md)</span></span>

## <a name="example-scenarios-for-usage"></a><span data-ttu-id="88db1-240">Příklad scénáře použití</span><span class="sxs-lookup"><span data-stu-id="88db1-240">Example Scenarios for usage</span></span>  

### <a name="tracking-vm-running-on-azure"></a><span data-ttu-id="88db1-241">Sledování virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="88db1-241">Tracking VM running on Azure</span></span>

<span data-ttu-id="88db1-242">Jako poskytovatele služeb může vyžadovat tootrack hello počet virtuálních počítačů spuštěných váš software nebo agenty, kteří potřebují tootrack jedinečnosti hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="88db1-242">As a service provider, you may require tootrack hello number of VMs running your software or have agents that need tootrack uniqueness of hello VM.</span></span> <span data-ttu-id="88db1-243">možnost tooget toobe jedinečné ID pro virtuální počítač, použijte hello `vmId` pole z Instance Metadata služby.</span><span class="sxs-lookup"><span data-stu-id="88db1-243">toobe able tooget a unique ID for a VM, use hello `vmId` field from Instance Metadata Service.</span></span>

<span data-ttu-id="88db1-244">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="88db1-244">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

<span data-ttu-id="88db1-245">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="88db1-245">**Response**</span></span>

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a><span data-ttu-id="88db1-246">Umístění kontejnery, oddíly dat na základě domény selhání nebo aktualizovat</span><span class="sxs-lookup"><span data-stu-id="88db1-246">Placement of containers, data-partitions based Fault/Update domain</span></span> 

<span data-ttu-id="88db1-247">Pro určité scénáře, umístění repliky různých datových je důležité.</span><span class="sxs-lookup"><span data-stu-id="88db1-247">For certain scenarios, placement of different data replicas is of prime importance.</span></span> <span data-ttu-id="88db1-248">Například [umístění repliky HDFS](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) nebo umístění kontejneru prostřednictvím [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) může vyžadovat tooknow hello `platformFaultDomain` a `platformUpdateDomain` hello běží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="88db1-248">For example, [HDFS replica placement](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) or container placement via an [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) may you require tooknow hello `platformFaultDomain` and `platformUpdateDomain` hello VM is running on.</span></span>
<span data-ttu-id="88db1-249">Tato data přímo prostřednictvím hello Instance Metadata služby se můžete dotazovat.</span><span class="sxs-lookup"><span data-stu-id="88db1-249">You can query this data directly via hello Instance Metadata Service.</span></span>

<span data-ttu-id="88db1-250">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="88db1-250">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

<span data-ttu-id="88db1-251">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="88db1-251">**Response**</span></span>

```
0
```

### <a name="getting-more-information-about-hello-vm-during-support-case"></a><span data-ttu-id="88db1-252">Další informace o hello virtuálních počítačů během případu podpory pro získávání</span><span class="sxs-lookup"><span data-stu-id="88db1-252">Getting more information about hello VM during support case</span></span> 

<span data-ttu-id="88db1-253">Jako poskytovatele služeb může se zobrazit žádost o podporu kam chcete tooknow Další informace o hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="88db1-253">As a service provider, you may get a support call where you would like tooknow more information about hello VM.</span></span> <span data-ttu-id="88db1-254">S dotazem, tooshare zákazníka hello hello výpočetní metadat poskytuje základní informace pro odborníky v oblasti tooknow hello podpory o hello druh virtuálních počítačů v Azure.</span><span class="sxs-lookup"><span data-stu-id="88db1-254">Asking hello customer tooshare hello compute metadata can provide basic information for hello support professional tooknow about hello kind of VM on Azure.</span></span> 

<span data-ttu-id="88db1-255">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="88db1-255">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

<span data-ttu-id="88db1-256">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="88db1-256">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="88db1-257">odpověď Hello je řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="88db1-257">hello response is a JSON string.</span></span> <span data-ttu-id="88db1-258">Následující příklad odpovědi Hello je pretty vytisknout čitelnější.</span><span class="sxs-lookup"><span data-stu-id="88db1-258">hello following example response is pretty-printed for readability.</span></span>

```
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-hello-vm"></a><span data-ttu-id="88db1-259">Příklady volání služby metadat pomocí různých jazyků uvnitř hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="88db1-259">Examples of calling metadata service using different languages inside hello VM</span></span> 

<span data-ttu-id="88db1-260">Jazyk</span><span class="sxs-lookup"><span data-stu-id="88db1-260">Language</span></span> | <span data-ttu-id="88db1-261">Příklad</span><span class="sxs-lookup"><span data-stu-id="88db1-261">Example</span></span> 
---------|----------------
<span data-ttu-id="88db1-262">Ruby</span><span class="sxs-lookup"><span data-stu-id="88db1-262">Ruby</span></span>     | <span data-ttu-id="88db1-263">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB</span><span class="sxs-lookup"><span data-stu-id="88db1-263">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb</span></span>
<span data-ttu-id="88db1-264">Přejděte Lan</span><span class="sxs-lookup"><span data-stu-id="88db1-264">Go Lan</span></span>   | <span data-ttu-id="88db1-265">https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go</span><span class="sxs-lookup"><span data-stu-id="88db1-265">https://github.com/Microsoft/azureimds/blob/master/imdssample.go</span></span>            
<span data-ttu-id="88db1-266">python</span><span class="sxs-lookup"><span data-stu-id="88db1-266">python</span></span>   | <span data-ttu-id="88db1-267">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY</span><span class="sxs-lookup"><span data-stu-id="88db1-267">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py</span></span>
<span data-ttu-id="88db1-268">C++</span><span class="sxs-lookup"><span data-stu-id="88db1-268">C++</span></span>      | <span data-ttu-id="88db1-269">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp</span><span class="sxs-lookup"><span data-stu-id="88db1-269">https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp</span></span>
<span data-ttu-id="88db1-270">C#</span><span class="sxs-lookup"><span data-stu-id="88db1-270">C#</span></span>       | <span data-ttu-id="88db1-271">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.cs</span><span class="sxs-lookup"><span data-stu-id="88db1-271">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs</span></span>
<span data-ttu-id="88db1-272">JavaScript</span><span class="sxs-lookup"><span data-stu-id="88db1-272">Javascript</span></span> | <span data-ttu-id="88db1-273">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js</span><span class="sxs-lookup"><span data-stu-id="88db1-273">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js</span></span>
<span data-ttu-id="88db1-274">PowerShell</span><span class="sxs-lookup"><span data-stu-id="88db1-274">Powershell</span></span> | <span data-ttu-id="88db1-275">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1</span><span class="sxs-lookup"><span data-stu-id="88db1-275">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1</span></span>
<span data-ttu-id="88db1-276">Bash</span><span class="sxs-lookup"><span data-stu-id="88db1-276">Bash</span></span>       | <span data-ttu-id="88db1-277">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH</span><span class="sxs-lookup"><span data-stu-id="88db1-277">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh</span></span>
    

## <a name="faq"></a><span data-ttu-id="88db1-278">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="88db1-278">FAQ</span></span>
1. <span data-ttu-id="88db1-279">Vyskytla se chyba hello `400 Bad Request, Required metadata header not specified`.</span><span class="sxs-lookup"><span data-stu-id="88db1-279">I am getting hello error `400 Bad Request, Required metadata header not specified`.</span></span> <span data-ttu-id="88db1-280">Co to znamená?</span><span class="sxs-lookup"><span data-stu-id="88db1-280">What does this mean?</span></span>
   * <span data-ttu-id="88db1-281">Hello Instance Metadata služby vyžaduje hlavičku hello `Metadata: true` toobe předaná hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="88db1-281">hello Instance Metadata Service requires hello header `Metadata: true` toobe passed in hello request.</span></span> <span data-ttu-id="88db1-282">Předávání tuto hlavičku v volání REST hello umožňuje přístup toohello Instance služby metadat.</span><span class="sxs-lookup"><span data-stu-id="88db1-282">Passing this header in hello REST call allows access toohello Instance Metadata Service.</span></span> 
2. <span data-ttu-id="88db1-283">Proč není zobrazuje informace o výpočetní pro virtuální počítač?</span><span class="sxs-lookup"><span data-stu-id="88db1-283">Why am I not getting compute information for my VM?</span></span>
   * <span data-ttu-id="88db1-284">Hello Instance Metadata služby aktuálně podporuje jenom instancích vytvořených pomocí Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="88db1-284">Currently hello Instance Metadata Service only supports instances created with Azure Resource Manager.</span></span> <span data-ttu-id="88db1-285">V budoucích hello jsme může přidat podporu pro virtuální počítače cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="88db1-285">In hello future, we may add support for Cloud Service VMs.</span></span>
3. <span data-ttu-id="88db1-286">Delší dobou vytvořený virtuální počítač prostřednictvím Správce Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="88db1-286">I created my Virtual Machine through Azure Resource Manager a while back.</span></span> <span data-ttu-id="88db1-287">Proč není najdete informace o metadatech výpočetní?</span><span class="sxs-lookup"><span data-stu-id="88db1-287">Why am I not see compute metadata information?</span></span>
   * <span data-ttu-id="88db1-288">Pro všechny virtuální počítače vytvořené po září 2016, přidejte [značka](../azure-resource-manager/resource-group-using-tags.md) toostart vidět výpočetní metadat.</span><span class="sxs-lookup"><span data-stu-id="88db1-288">For any VMs created after Sep 2016, add a [Tag](../azure-resource-manager/resource-group-using-tags.md) toostart seeing compute metadata.</span></span> <span data-ttu-id="88db1-289">Pro starší virtuální počítače (vytvořených před září 2016) přidat nebo odebrat rozšíření nebo data toorefresh metadata disky toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="88db1-289">For older VMs (created before Sep 2016), add/remove extensions or data disks toohello VM toorefresh metadata.</span></span>
4. <span data-ttu-id="88db1-290">Proč se zobrazuje chyba hello `500 Internal Server Error`?</span><span class="sxs-lookup"><span data-stu-id="88db1-290">Why am I getting hello error `500 Internal Server Error`?</span></span>
   * <span data-ttu-id="88db1-291">Opakujte žádost podle exponenciální regrese systému.</span><span class="sxs-lookup"><span data-stu-id="88db1-291">Please retry your request based on exponential back off system.</span></span> <span data-ttu-id="88db1-292">Pokud hello problém přetrvávat, kontaktujte podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="88db1-292">If hello issue persists contact  Azure support.</span></span>
5. <span data-ttu-id="88db1-293">Kde mohu sdílet další dotazy nebo připomínky?</span><span class="sxs-lookup"><span data-stu-id="88db1-293">Where do I share additional questions/comments?</span></span>
   * <span data-ttu-id="88db1-294">Odešlete komentáře k http://feedback.azure.com.</span><span class="sxs-lookup"><span data-stu-id="88db1-294">Send your comments on http://feedback.azure.com.</span></span>
7. <span data-ttu-id="88db1-295">To funguje pro instanci nastavení škálování virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="88db1-295">Would this work for Virtual Machine Scale Set Instance?</span></span>
   * <span data-ttu-id="88db1-296">Ano je k dispozici pro škálování nastavení instance služby metadat.</span><span class="sxs-lookup"><span data-stu-id="88db1-296">Yes Metadata service is available for Scale Set Instances.</span></span> 
6. <span data-ttu-id="88db1-297">Jak získat podporu pro službu hello?</span><span class="sxs-lookup"><span data-stu-id="88db1-297">How do I get support for hello service?</span></span>
   * <span data-ttu-id="88db1-298">tooget podporu služby hello, vytvořit problém podpory na portálu Azure pro hello virtuálního počítače, kde se není možné tooget metadat odpovědi po dlouhou opakování</span><span class="sxs-lookup"><span data-stu-id="88db1-298">tooget support for hello service, create a support issue in Azure portal for hello VM where you are not able tooget metadata response after long retries</span></span> 

   ![Podpora metadat instance](./media/virtual-machines-instancemetadataservice-overview/InstanceMetadata-support.png)
    
## <a name="next-steps"></a><span data-ttu-id="88db1-300">Další kroky</span><span class="sxs-lookup"><span data-stu-id="88db1-300">Next Steps</span></span>

- <span data-ttu-id="88db1-301">Další informace o hello [scheduledevents](virtual-machines-scheduled-events.md) rozhraní API **ve verzi Public Preview** poskytované hello Instance služby metadat.</span><span class="sxs-lookup"><span data-stu-id="88db1-301">Learn more about hello [scheduledevents](virtual-machines-scheduled-events.md) API **In Public Preview** provided by hello Instance Metadata Service.</span></span>
