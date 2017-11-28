---
title: "Služba Azure Instance metadat pro virtuální počítače s Linuxem | Microsoft Docs"
description: "Rozhraní rESTful získat informace o výpočetní, síťové a nadcházející údržbě události Linux Virtuálního počítače."
services: virtual-machines-linux
documentationcenter: 
author: harijay
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: harijay
ms.openlocfilehash: a61acbe0532ece3a6a26ceb366c12c69db4c304c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-instance-metadata-service-for-linux-vms"></a><span data-ttu-id="cabd8-103">Služba Azure Instance Metadata pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="cabd8-103">Azure Instance Metadata service for Linux VMs</span></span>


<span data-ttu-id="cabd8-104">Služba Azure Instance metadat poskytuje informace o spuštěných instancí virtuálního počítače, které lze použít ke správě nebo konfiguraci virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cabd8-104">The Azure Instance Metadata Service provides information about running virtual machine instances that can be used to manage and configure your virtual machines.</span></span>
<span data-ttu-id="cabd8-105">To zahrnuje informace, jako je SKU, konfigurace sítě a události nadcházející údržby.</span><span class="sxs-lookup"><span data-stu-id="cabd8-105">This includes information such as SKU, network configuration, and upcoming maintenance events.</span></span> <span data-ttu-id="cabd8-106">Další informace o jaké typy informací je k dispozici, najdete v části [metadata kategorie](#instance-metadata-data-categories).</span><span class="sxs-lookup"><span data-stu-id="cabd8-106">For more information on what type of information is available, see [metadata categories](#instance-metadata-data-categories).</span></span>

<span data-ttu-id="cabd8-107">Služba Metadata Instance Azure a je přístupný pro všechny virtuální počítače IaaS vytvořené prostřednictvím koncový bod REST [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="cabd8-107">Azure's Instance Metadata Service is a REST Endpoint accessible to all IaaS VMs created via the [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="cabd8-108">Koncový bod je k dispozici na dobře známé směrovat IP adresu (`169.254.169.254`), můžete přistupovat pouze z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cabd8-108">The endpoint is available at a well-known non-routable IP address (`169.254.169.254`) that can be accessed only from within the VM.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cabd8-109">Tato služba je **všeobecně dostupná** v globální oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="cabd8-109">This service is  **generally available** in Global Azure Regions.</span></span> <span data-ttu-id="cabd8-110">Je ve verzi Public preview pro státní, Čína a Cloud Azure němčina.</span><span class="sxs-lookup"><span data-stu-id="cabd8-110">It is in Public preview for Government, China, and German Azure Cloud.</span></span> <span data-ttu-id="cabd8-111">Pravidelně obdrží aktualizace ke zveřejnění nové informace o instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cabd8-111">It regularly receives updates to expose new information about virtual machine instances.</span></span> <span data-ttu-id="cabd8-112">Tato stránka zobrazuje aktuální [kategorie dat](#instance-metadata-data-categories) k dispozici.</span><span class="sxs-lookup"><span data-stu-id="cabd8-112">This page reflects the up-to-date [data categories](#instance-metadata-data-categories) available.</span></span>

## <a name="service-availability"></a><span data-ttu-id="cabd8-113">Dostupnost služeb</span><span class="sxs-lookup"><span data-stu-id="cabd8-113">Service availability</span></span>
<span data-ttu-id="cabd8-114">Služba je k dispozici ve všech oblastech všeobecně dostupná globální Azure.</span><span class="sxs-lookup"><span data-stu-id="cabd8-114">The service is available in all generally available Global Azure regions.</span></span> <span data-ttu-id="cabd8-115">Služba není ve verzi public preview v oblastech Government, Čína nebo Německu.</span><span class="sxs-lookup"><span data-stu-id="cabd8-115">The service is in public preview  in the Government, China, or Germany regions.</span></span>

<span data-ttu-id="cabd8-116">Oblasti</span><span class="sxs-lookup"><span data-stu-id="cabd8-116">Regions</span></span>                                        | <span data-ttu-id="cabd8-117">Dostupnost?</span><span class="sxs-lookup"><span data-stu-id="cabd8-117">Availability?</span></span>
-----------------------------------------------|-----------------------------------------------
[<span data-ttu-id="cabd8-118">Všechny všeobecně dostupná globální oblasti Azure</span><span class="sxs-lookup"><span data-stu-id="cabd8-118">All Generally Available Global Azure Regions</span></span>](https://azure.microsoft.com/regions/)     | <span data-ttu-id="cabd8-119">Obecně k dispozici</span><span class="sxs-lookup"><span data-stu-id="cabd8-119">Generally Available</span></span> 
[<span data-ttu-id="cabd8-120">Azure Government</span><span class="sxs-lookup"><span data-stu-id="cabd8-120">Azure Government</span></span>](https://azure.microsoft.com/overview/clouds/government/)              | <span data-ttu-id="cabd8-121">Ve verzi Preview</span><span class="sxs-lookup"><span data-stu-id="cabd8-121">In Preview</span></span> 
[<span data-ttu-id="cabd8-122">Azure Čína</span><span class="sxs-lookup"><span data-stu-id="cabd8-122">Azure China</span></span>](https://www.azure.cn/)                                                           | <span data-ttu-id="cabd8-123">Ve verzi Preview</span><span class="sxs-lookup"><span data-stu-id="cabd8-123">In Preview</span></span>
[<span data-ttu-id="cabd8-124">Azure Německo</span><span class="sxs-lookup"><span data-stu-id="cabd8-124">Azure Germany</span></span>](https://azure.microsoft.com/overview/clouds/germany/)                    | <span data-ttu-id="cabd8-125">Ve verzi Preview</span><span class="sxs-lookup"><span data-stu-id="cabd8-125">In Preview</span></span>

<span data-ttu-id="cabd8-126">Tato tabulka je aktualizována při služba k dispozici v ostatních cloudů Azure.</span><span class="sxs-lookup"><span data-stu-id="cabd8-126">This table is updated when the service becomes available in other Azure clouds.</span></span>

<span data-ttu-id="cabd8-127">Můžete vyzkoušet na služby metadat Instance, vytvoření virtuálního počítače z [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) nebo [portál Azure](http://portal.azure.com) v oblastech výše a postupujte podle níže uvedených příkladech.</span><span class="sxs-lookup"><span data-stu-id="cabd8-127">To try out the Instance Metadata Service, create a VM from [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) or the [Azure portal](http://portal.azure.com) in the above regions and follow the examples below.</span></span>

## <a name="usage"></a><span data-ttu-id="cabd8-128">Využití</span><span class="sxs-lookup"><span data-stu-id="cabd8-128">Usage</span></span>

### <a name="versioning"></a><span data-ttu-id="cabd8-129">Správa verzí</span><span class="sxs-lookup"><span data-stu-id="cabd8-129">Versioning</span></span>
<span data-ttu-id="cabd8-130">Služba Instance metadat je verzí.</span><span class="sxs-lookup"><span data-stu-id="cabd8-130">The Instance Metadata Service is versioned.</span></span> <span data-ttu-id="cabd8-131">Verze jsou povinné a aktuální verze je `2017-04-02`.</span><span class="sxs-lookup"><span data-stu-id="cabd8-131">Versions are mandatory and the current version is `2017-04-02`.</span></span>

> [!NOTE] 
> <span data-ttu-id="cabd8-132">Předchozí verze preview naplánované událostí podporovaných {nejnovější} jako verze rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="cabd8-132">Previous preview releases of scheduled events supported {latest} as the api-version.</span></span> <span data-ttu-id="cabd8-133">Tento formát se už nepodporuje a bude v budoucnu zastaralá.</span><span class="sxs-lookup"><span data-stu-id="cabd8-133">This format is no longer supported and will be deprecated in the future.</span></span>

<span data-ttu-id="cabd8-134">Jako přidáme novější verze, starší verze se dá dál dostat z důvodu kompatibility Pokud skripty mají závislosti na konkrétní data formátů.</span><span class="sxs-lookup"><span data-stu-id="cabd8-134">As we add newer versions, older versions can still be accessed for compatibility if your scripts have dependencies on specific data formats.</span></span> <span data-ttu-id="cabd8-135">Všimněte si však, že aktuální version(2017-03-01) preview nemusí být k dispozici, jakmile služba všeobecně dostupná.</span><span class="sxs-lookup"><span data-stu-id="cabd8-135">However, note that the current preview version(2017-03-01) may not be available once the service is generally available.</span></span>

### <a name="using-headers"></a><span data-ttu-id="cabd8-136">Používání hlaviček</span><span class="sxs-lookup"><span data-stu-id="cabd8-136">Using headers</span></span>
<span data-ttu-id="cabd8-137">Když dotazujete Metadata Instance služby, je nutné zadat hlavičku `Metadata: true` zajistit požadavek nebyl přesměrován náhodně.</span><span class="sxs-lookup"><span data-stu-id="cabd8-137">When you query the Instance Metadata Service, you must provide the header `Metadata: true` to ensure the request was not unintentionally redirected.</span></span>

### <a name="retrieving-metadata"></a><span data-ttu-id="cabd8-138">Načítání metadat</span><span class="sxs-lookup"><span data-stu-id="cabd8-138">Retrieving metadata</span></span>

<span data-ttu-id="cabd8-139">Instance metadata jsou k dispozici pro spouštění virtuálních počítačů vytvořena nebo spravovat pomocí [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="cabd8-139">Instance metadata is available for running VMs created/managed using [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="cabd8-140">Všechny kategorie dat pro instanci virtuálního počítače pomocí následující žádosti o přístup:</span><span class="sxs-lookup"><span data-stu-id="cabd8-140">Access all data categories for a virtual machine instance using the following request:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> <span data-ttu-id="cabd8-141">Všechny dotazy metadat instance rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="cabd8-141">All instance metadata queries are case-sensitive.</span></span>

### <a name="data-output"></a><span data-ttu-id="cabd8-142">Výstup dat</span><span class="sxs-lookup"><span data-stu-id="cabd8-142">Data output</span></span>
<span data-ttu-id="cabd8-143">Ve výchozím nastavení, Instance služby metadat vrací data ve formátu JSON (`Content-Type: application/json`).</span><span class="sxs-lookup"><span data-stu-id="cabd8-143">By default, the Instance Metadata Service returns data in JSON format (`Content-Type: application/json`).</span></span> <span data-ttu-id="cabd8-144">Rozhraní API pro různé však může vrátit data v různých formátech Pokud požadovaný.</span><span class="sxs-lookup"><span data-stu-id="cabd8-144">However, different APIs can return data in different formats if requested.</span></span>
<span data-ttu-id="cabd8-145">V následující tabulce je odkazem na jiných formátů dat, které můžou podporovat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cabd8-145">The following table is a reference of other data formats APIs may support.</span></span>

<span data-ttu-id="cabd8-146">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cabd8-146">API</span></span> | <span data-ttu-id="cabd8-147">Výchozí formát dat</span><span class="sxs-lookup"><span data-stu-id="cabd8-147">Default Data Format</span></span> | <span data-ttu-id="cabd8-148">Ostatní formáty</span><span class="sxs-lookup"><span data-stu-id="cabd8-148">Other Formats</span></span>
--------|---------------------|--------------
<span data-ttu-id="cabd8-149">/instance</span><span class="sxs-lookup"><span data-stu-id="cabd8-149">/instance</span></span> | <span data-ttu-id="cabd8-150">JSON</span><span class="sxs-lookup"><span data-stu-id="cabd8-150">json</span></span> | <span data-ttu-id="cabd8-151">Text</span><span class="sxs-lookup"><span data-stu-id="cabd8-151">text</span></span>
<span data-ttu-id="cabd8-152">/scheduledevents</span><span class="sxs-lookup"><span data-stu-id="cabd8-152">/scheduledevents</span></span> | <span data-ttu-id="cabd8-153">JSON</span><span class="sxs-lookup"><span data-stu-id="cabd8-153">json</span></span> | <span data-ttu-id="cabd8-154">None</span><span class="sxs-lookup"><span data-stu-id="cabd8-154">none</span></span>

<span data-ttu-id="cabd8-155">Pro přístup k odpovědi jiné než výchozí formát, zadejte požadovaný formát jako parametr řetězce dotazu v žádosti.</span><span class="sxs-lookup"><span data-stu-id="cabd8-155">To access a non-default response format, specify the requested format as a querystring parameter in the request.</span></span> <span data-ttu-id="cabd8-156">Například:</span><span class="sxs-lookup"><span data-stu-id="cabd8-156">For example:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a><span data-ttu-id="cabd8-157">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="cabd8-157">Security</span></span>
<span data-ttu-id="cabd8-158">Koncový bod služby Metadata Instance je dostupné pouze v aplikaci spuštěnou instanci virtuálního počítače na směrovat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="cabd8-158">The Instance Metadata Service endpoint is accessible only from within the running virtual machine instance on a non-routable IP address.</span></span> <span data-ttu-id="cabd8-159">Kromě toho každá žádost s `X-Forwarded-For` záhlaví byl odmítnut službou.</span><span class="sxs-lookup"><span data-stu-id="cabd8-159">In addition, any request with a `X-Forwarded-For` header is rejected by the service.</span></span>
<span data-ttu-id="cabd8-160">Je také nutné požadavky tak, aby obsahovala `Metadata: true` záhlaví zajistit, že skutečné žádost byla přímo určený a není součástí neúmyslnému přesměrování.</span><span class="sxs-lookup"><span data-stu-id="cabd8-160">We also require requests to contain a `Metadata: true` header to ensure that the actual request was directly intended and not a part of unintentional redirection.</span></span> 

### <a name="error"></a><span data-ttu-id="cabd8-161">Chyba</span><span class="sxs-lookup"><span data-stu-id="cabd8-161">Error</span></span>
<span data-ttu-id="cabd8-162">Pokud je datový prvek nebyl nalezen nebo chybně vytvořený požadavek, Instance služby metadat vrátí standardní chyby protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="cabd8-162">If there is a data element not found or a malformed request, the Instance Metadata Service returns standard HTTP errors.</span></span> <span data-ttu-id="cabd8-163">Například:</span><span class="sxs-lookup"><span data-stu-id="cabd8-163">For example:</span></span>

<span data-ttu-id="cabd8-164">Kód stavu HTTP</span><span class="sxs-lookup"><span data-stu-id="cabd8-164">HTTP Status Code</span></span> | <span data-ttu-id="cabd8-165">Důvod</span><span class="sxs-lookup"><span data-stu-id="cabd8-165">Reason</span></span>
----------------|-------
<span data-ttu-id="cabd8-166">200 OK</span><span class="sxs-lookup"><span data-stu-id="cabd8-166">200 OK</span></span> |
<span data-ttu-id="cabd8-167">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="cabd8-167">400 Bad Request</span></span> | <span data-ttu-id="cabd8-168">Chybí `Metadata: true` záhlaví</span><span class="sxs-lookup"><span data-stu-id="cabd8-168">Missing `Metadata: true` header</span></span>
<span data-ttu-id="cabd8-169">404 – Nenalezeno</span><span class="sxs-lookup"><span data-stu-id="cabd8-169">404 Not Found</span></span> | <span data-ttu-id="cabd8-170">Položka hierarchyinfoguid požadovaný element neexistuje</span><span class="sxs-lookup"><span data-stu-id="cabd8-170">The requested element does't exist</span></span> 
<span data-ttu-id="cabd8-171">405 – Metoda není povoleno</span><span class="sxs-lookup"><span data-stu-id="cabd8-171">405 Method Not Allowed</span></span> | <span data-ttu-id="cabd8-172">Pouze `GET` a `POST` jsou podporovány požadavky</span><span class="sxs-lookup"><span data-stu-id="cabd8-172">Only `GET` and `POST` requests are supported</span></span>
<span data-ttu-id="cabd8-173">429 příliš mnoho požadavků</span><span class="sxs-lookup"><span data-stu-id="cabd8-173">429 Too Many Requests</span></span> | <span data-ttu-id="cabd8-174">Rozhraní API aktuálně podporuje maximálně 5 dotazů za sekundu</span><span class="sxs-lookup"><span data-stu-id="cabd8-174">The API currently supports a maximum of 5 queries per second</span></span>
<span data-ttu-id="cabd8-175">Chyba 500 služby</span><span class="sxs-lookup"><span data-stu-id="cabd8-175">500 Service Error</span></span>     | <span data-ttu-id="cabd8-176">Po určité době opakujte</span><span class="sxs-lookup"><span data-stu-id="cabd8-176">Retry after some time</span></span>

### <a name="examples"></a><span data-ttu-id="cabd8-177">Příklady</span><span class="sxs-lookup"><span data-stu-id="cabd8-177">Examples</span></span>

> [!NOTE] 
> <span data-ttu-id="cabd8-178">Všechny odpovědi rozhraní API jsou řetězce formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="cabd8-178">All API responses are JSON strings.</span></span> <span data-ttu-id="cabd8-179">Jsou všechny tyto odpovědi příklad pretty vytisknout čitelnější.</span><span class="sxs-lookup"><span data-stu-id="cabd8-179">All following example responses  are pretty-printed for readability.</span></span>

#### <a name="retrieving-network-information"></a><span data-ttu-id="cabd8-180">Načítání informací o síti</span><span class="sxs-lookup"><span data-stu-id="cabd8-180">Retrieving network information</span></span>

<span data-ttu-id="cabd8-181">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="cabd8-181">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

<span data-ttu-id="cabd8-182">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="cabd8-182">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="cabd8-183">Odpověď je řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="cabd8-183">The response is a JSON string.</span></span> <span data-ttu-id="cabd8-184">Následující příklad odpověď je pretty vytisknout čitelnější.</span><span class="sxs-lookup"><span data-stu-id="cabd8-184">The following example response is pretty-printed for readability.</span></span>

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

#### <a name="retrieving-public-ip-address"></a><span data-ttu-id="cabd8-185">Načítání veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="cabd8-185">Retrieving public IP address</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a><span data-ttu-id="cabd8-186">Načítání metadat všechny instance</span><span class="sxs-lookup"><span data-stu-id="cabd8-186">Retrieving all metadata for an instance</span></span>

<span data-ttu-id="cabd8-187">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="cabd8-187">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

<span data-ttu-id="cabd8-188">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="cabd8-188">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="cabd8-189">Odpověď je řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="cabd8-189">The response is a JSON string.</span></span> <span data-ttu-id="cabd8-190">Následující příklad odpověď je pretty vytisknout čitelnější.</span><span class="sxs-lookup"><span data-stu-id="cabd8-190">The following example response is pretty-printed for readability.</span></span>

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

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a><span data-ttu-id="cabd8-191">Načítání metadat virtuálního počítače ve Windows</span><span class="sxs-lookup"><span data-stu-id="cabd8-191">Retrieving metadata in Windows Virtual Machine</span></span>

<span data-ttu-id="cabd8-192">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="cabd8-192">**Request**</span></span>

<span data-ttu-id="cabd8-193">Nejde načíst instance metadata v systému Windows pomocí nástroje PowerShell `curl`:</span><span class="sxs-lookup"><span data-stu-id="cabd8-193">Instance metadata can be retrieved in Windows via the PowerShell utility `curl`:</span></span> 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

<span data-ttu-id="cabd8-194">Nebo pomocí `Invoke-RestMethod` rutiny:</span><span class="sxs-lookup"><span data-stu-id="cabd8-194">Or through the `Invoke-RestMethod` cmdlet:</span></span>
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

<span data-ttu-id="cabd8-195">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="cabd8-195">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="cabd8-196">Odpověď je řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="cabd8-196">The response is a JSON string.</span></span> <span data-ttu-id="cabd8-197">Následující příklad odpověď je pretty vytisknout čitelnější.</span><span class="sxs-lookup"><span data-stu-id="cabd8-197">The following example response  is pretty-printed for readability.</span></span>

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

## <a name="instance-metadata-data-categories"></a><span data-ttu-id="cabd8-198">Kategorie dat instance metadat</span><span class="sxs-lookup"><span data-stu-id="cabd8-198">Instance metadata data categories</span></span>
<span data-ttu-id="cabd8-199">Následující kategorie dat jsou k dispozici prostřednictvím služby Instance metadat:</span><span class="sxs-lookup"><span data-stu-id="cabd8-199">The following data categories are available through the Instance Metadata Service:</span></span>

<span data-ttu-id="cabd8-200">Data</span><span class="sxs-lookup"><span data-stu-id="cabd8-200">Data</span></span> | <span data-ttu-id="cabd8-201">Popis</span><span class="sxs-lookup"><span data-stu-id="cabd8-201">Description</span></span>
-----|------------
<span data-ttu-id="cabd8-202">location</span><span class="sxs-lookup"><span data-stu-id="cabd8-202">location</span></span> | <span data-ttu-id="cabd8-203">Oblast Azure virtuální počítač běží v</span><span class="sxs-lookup"><span data-stu-id="cabd8-203">Azure Region the VM is running in</span></span>
<span data-ttu-id="cabd8-204">jméno</span><span class="sxs-lookup"><span data-stu-id="cabd8-204">name</span></span> | <span data-ttu-id="cabd8-205">Název virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cabd8-205">Name of the VM</span></span> 
<span data-ttu-id="cabd8-206">Nabídka</span><span class="sxs-lookup"><span data-stu-id="cabd8-206">offer</span></span> | <span data-ttu-id="cabd8-207">Nabízí informace o image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cabd8-207">Offer information for the VM image.</span></span> <span data-ttu-id="cabd8-208">Tato hodnota je jenom pro Image nasadit z Galerie obrázků Azure k dispozici.</span><span class="sxs-lookup"><span data-stu-id="cabd8-208">This value is only present for images deployed from Azure image gallery.</span></span>
<span data-ttu-id="cabd8-209">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="cabd8-209">publisher</span></span> | <span data-ttu-id="cabd8-210">Vydavatel image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cabd8-210">Publisher of the VM image</span></span>
<span data-ttu-id="cabd8-211">SKU</span><span class="sxs-lookup"><span data-stu-id="cabd8-211">sku</span></span> | <span data-ttu-id="cabd8-212">Konkrétní SKU pro bitovou kopii virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cabd8-212">Specific SKU for the VM image</span></span>  
<span data-ttu-id="cabd8-213">Verze</span><span class="sxs-lookup"><span data-stu-id="cabd8-213">version</span></span> | <span data-ttu-id="cabd8-214">Verze bitové kopie virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cabd8-214">Version of the VM image</span></span> 
<span data-ttu-id="cabd8-215">osType</span><span class="sxs-lookup"><span data-stu-id="cabd8-215">osType</span></span> | <span data-ttu-id="cabd8-216">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="cabd8-216">Linux or Windows</span></span> 
<span data-ttu-id="cabd8-217">platformUpdateDomain</span><span class="sxs-lookup"><span data-stu-id="cabd8-217">platformUpdateDomain</span></span> |  <span data-ttu-id="cabd8-218">[Aktualizace domény](manage-availability.md) je virtuální počítač spuštěný</span><span class="sxs-lookup"><span data-stu-id="cabd8-218">[Update domain](manage-availability.md) the VM is running in</span></span>
<span data-ttu-id="cabd8-219">platformFaultDomain</span><span class="sxs-lookup"><span data-stu-id="cabd8-219">platformFaultDomain</span></span> | <span data-ttu-id="cabd8-220">[Doména selhání](manage-availability.md) je virtuální počítač spuštěný</span><span class="sxs-lookup"><span data-stu-id="cabd8-220">[Fault domain](manage-availability.md) the VM is running in</span></span>
<span data-ttu-id="cabd8-221">vmId</span><span class="sxs-lookup"><span data-stu-id="cabd8-221">vmId</span></span> | <span data-ttu-id="cabd8-222">[Jedinečný identifikátor](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cabd8-222">[Unique identifier](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) for the VM</span></span>
<span data-ttu-id="cabd8-223">vmSize</span><span class="sxs-lookup"><span data-stu-id="cabd8-223">vmSize</span></span> | [<span data-ttu-id="cabd8-224">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cabd8-224">VM size</span></span>](sizes.md)
<span data-ttu-id="cabd8-225">IPv4/privateIpAddress</span><span class="sxs-lookup"><span data-stu-id="cabd8-225">ipv4/privateIpAddress</span></span> | <span data-ttu-id="cabd8-226">Místní adresu IPv4 virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cabd8-226">Local IPv4 address of the VM</span></span> 
<span data-ttu-id="cabd8-227">IPv4/publicIpAddress</span><span class="sxs-lookup"><span data-stu-id="cabd8-227">ipv4/publicIpAddress</span></span> | <span data-ttu-id="cabd8-228">Veřejnou IPv4 adresu virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cabd8-228">Public IPv4 address of the VM</span></span>
<span data-ttu-id="cabd8-229">Adresa podsítě /</span><span class="sxs-lookup"><span data-stu-id="cabd8-229">subnet/address</span></span> | <span data-ttu-id="cabd8-230">Adresa podsítě virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cabd8-230">Subnet address of the VM</span></span>
<span data-ttu-id="cabd8-231">Předpona podsítě /</span><span class="sxs-lookup"><span data-stu-id="cabd8-231">subnet/prefix</span></span> | <span data-ttu-id="cabd8-232">Předpona podsítě, například 24</span><span class="sxs-lookup"><span data-stu-id="cabd8-232">Subnet prefix, example 24</span></span>
<span data-ttu-id="cabd8-233">IPv6 nebo adresa IP</span><span class="sxs-lookup"><span data-stu-id="cabd8-233">ipv6/ipAddress</span></span> | <span data-ttu-id="cabd8-234">Místní adresu IPv6 virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cabd8-234">Local IPv6 address of the VM</span></span>
<span data-ttu-id="cabd8-235">MacAddress</span><span class="sxs-lookup"><span data-stu-id="cabd8-235">macAddress</span></span> | <span data-ttu-id="cabd8-236">Adresa mac virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cabd8-236">VM mac address</span></span> 
<span data-ttu-id="cabd8-237">scheduledevents</span><span class="sxs-lookup"><span data-stu-id="cabd8-237">scheduledevents</span></span> | <span data-ttu-id="cabd8-238">Aktuálně ve veřejné verzi Preview najdete [scheduledevents](scheduled-events.md)</span><span class="sxs-lookup"><span data-stu-id="cabd8-238">Currently in Public Preview See [scheduledevents](scheduled-events.md)</span></span>

## <a name="example-scenarios-for-usage"></a><span data-ttu-id="cabd8-239">Příklad scénáře použití</span><span class="sxs-lookup"><span data-stu-id="cabd8-239">Example scenarios for usage</span></span>  

### <a name="tracking-vm-running-on-azure"></a><span data-ttu-id="cabd8-240">Sledování virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="cabd8-240">Tracking VM running on Azure</span></span>

<span data-ttu-id="cabd8-241">Jako poskytovatele služeb může vyžadovat sledovat počet virtuálních počítačů spuštěných váš software nebo agenty, kteří potřebují ke sledování jedinečnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cabd8-241">As a service provider, you may require to track the number of VMs running your software or have agents that need to track uniqueness of the VM.</span></span> <span data-ttu-id="cabd8-242">Abyste mohli získat jedinečné ID pro virtuální počítač, použijte `vmId` pole z Instance Metadata služby.</span><span class="sxs-lookup"><span data-stu-id="cabd8-242">To be able to get a unique ID for a VM, use the `vmId` field from Instance Metadata Service.</span></span>

<span data-ttu-id="cabd8-243">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="cabd8-243">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

<span data-ttu-id="cabd8-244">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="cabd8-244">**Response**</span></span>

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a><span data-ttu-id="cabd8-245">Umístění kontejnery, oddíly dat na základě domény selhání nebo aktualizovat</span><span class="sxs-lookup"><span data-stu-id="cabd8-245">Placement of containers, data-partitions based fault/update domain</span></span> 

<span data-ttu-id="cabd8-246">Pro určité scénáře, umístění repliky různých datových je důležité.</span><span class="sxs-lookup"><span data-stu-id="cabd8-246">For certain scenarios, placement of different data replicas is of prime importance.</span></span> <span data-ttu-id="cabd8-247">Například [umístění repliky HDFS](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) nebo umístění kontejneru prostřednictvím [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) může vyžadovat vědět `platformFaultDomain` a `platformUpdateDomain` virtuální počítač běží na.</span><span class="sxs-lookup"><span data-stu-id="cabd8-247">For example, [HDFS replica placement](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) or container placement via an [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) may you require to know the `platformFaultDomain` and `platformUpdateDomain` the VM is running on.</span></span>
<span data-ttu-id="cabd8-248">Tato data přímo přes službu Metadata Instance se můžete dotazovat.</span><span class="sxs-lookup"><span data-stu-id="cabd8-248">You can query this data directly via the Instance Metadata Service.</span></span>

<span data-ttu-id="cabd8-249">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="cabd8-249">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

<span data-ttu-id="cabd8-250">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="cabd8-250">**Response**</span></span>

```
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a><span data-ttu-id="cabd8-251">Další informace o virtuálním počítači během případu podpory pro získávání</span><span class="sxs-lookup"><span data-stu-id="cabd8-251">Getting more information about the VM during support case</span></span> 

<span data-ttu-id="cabd8-252">Jako poskytovatele služeb může získat volání podpory kde chcete vědět, další informace o virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="cabd8-252">As a service provider, you may get a support call where you would like to know more information about the VM.</span></span> <span data-ttu-id="cabd8-253">S dotazem, zákazník sdílet metadata výpočetní poskytuje základní informace pro odborníky v oblasti vědět o druh virtuálního počítače na platformě Azure podporu.</span><span class="sxs-lookup"><span data-stu-id="cabd8-253">Asking the customer to share the compute metadata can provide basic information for the support professional to know about the kind of VM on Azure.</span></span> 

<span data-ttu-id="cabd8-254">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="cabd8-254">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

<span data-ttu-id="cabd8-255">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="cabd8-255">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="cabd8-256">Odpověď je řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="cabd8-256">The response is a JSON string.</span></span> <span data-ttu-id="cabd8-257">Následující příklad odpověď je pretty vytisknout čitelnější.</span><span class="sxs-lookup"><span data-stu-id="cabd8-257">The following example response is pretty-printed for readability.</span></span>

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

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a><span data-ttu-id="cabd8-258">Příklady volání služby metadat pomocí různých jazyků ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="cabd8-258">Examples of calling metadata service using different languages inside the VM</span></span> 

<span data-ttu-id="cabd8-259">Jazyk</span><span class="sxs-lookup"><span data-stu-id="cabd8-259">Language</span></span> | <span data-ttu-id="cabd8-260">Příklad</span><span class="sxs-lookup"><span data-stu-id="cabd8-260">Example</span></span> 
---------|----------------
<span data-ttu-id="cabd8-261">Ruby</span><span class="sxs-lookup"><span data-stu-id="cabd8-261">Ruby</span></span>     | <span data-ttu-id="cabd8-262">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB</span><span class="sxs-lookup"><span data-stu-id="cabd8-262">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb</span></span>
<span data-ttu-id="cabd8-263">Přejděte Lan</span><span class="sxs-lookup"><span data-stu-id="cabd8-263">Go Lan</span></span>   | <span data-ttu-id="cabd8-264">https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go</span><span class="sxs-lookup"><span data-stu-id="cabd8-264">https://github.com/Microsoft/azureimds/blob/master/imdssample.go</span></span>            
<span data-ttu-id="cabd8-265">python</span><span class="sxs-lookup"><span data-stu-id="cabd8-265">python</span></span>   | <span data-ttu-id="cabd8-266">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY</span><span class="sxs-lookup"><span data-stu-id="cabd8-266">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py</span></span>
<span data-ttu-id="cabd8-267">C++</span><span class="sxs-lookup"><span data-stu-id="cabd8-267">C++</span></span>      | <span data-ttu-id="cabd8-268">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp</span><span class="sxs-lookup"><span data-stu-id="cabd8-268">https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp</span></span>
<span data-ttu-id="cabd8-269">C#</span><span class="sxs-lookup"><span data-stu-id="cabd8-269">C#</span></span>       | <span data-ttu-id="cabd8-270">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.cs</span><span class="sxs-lookup"><span data-stu-id="cabd8-270">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs</span></span>
<span data-ttu-id="cabd8-271">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cabd8-271">Javascript</span></span> | <span data-ttu-id="cabd8-272">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js</span><span class="sxs-lookup"><span data-stu-id="cabd8-272">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js</span></span>
<span data-ttu-id="cabd8-273">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cabd8-273">Powershell</span></span> | <span data-ttu-id="cabd8-274">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1</span><span class="sxs-lookup"><span data-stu-id="cabd8-274">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1</span></span>
<span data-ttu-id="cabd8-275">Bash</span><span class="sxs-lookup"><span data-stu-id="cabd8-275">Bash</span></span>       | <span data-ttu-id="cabd8-276">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH</span><span class="sxs-lookup"><span data-stu-id="cabd8-276">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh</span></span>
    

## <a name="faq"></a><span data-ttu-id="cabd8-277">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="cabd8-277">FAQ</span></span>
1. <span data-ttu-id="cabd8-278">Vyskytla se chyba `400 Bad Request, Required metadata header not specified`.</span><span class="sxs-lookup"><span data-stu-id="cabd8-278">I am getting the error `400 Bad Request, Required metadata header not specified`.</span></span> <span data-ttu-id="cabd8-279">Co to znamená?</span><span class="sxs-lookup"><span data-stu-id="cabd8-279">What does this mean?</span></span>
   * <span data-ttu-id="cabd8-280">Instance Metadata služba vyžaduje, aby záhlaví `Metadata: true` předávané v požadavku.</span><span class="sxs-lookup"><span data-stu-id="cabd8-280">The Instance Metadata Service requires the header `Metadata: true` to be passed in the request.</span></span> <span data-ttu-id="cabd8-281">Předávání tuto hlavičku ve volání REST umožňuje přístup ke službě Instance metadat.</span><span class="sxs-lookup"><span data-stu-id="cabd8-281">Passing this header in the REST call allows access to the Instance Metadata Service.</span></span> 
2. <span data-ttu-id="cabd8-282">Proč není zobrazuje informace o výpočetní pro virtuální počítač?</span><span class="sxs-lookup"><span data-stu-id="cabd8-282">Why am I not getting compute information for my VM?</span></span>
   * <span data-ttu-id="cabd8-283">Instance služby Metadata aktuálně podporuje jenom instancích vytvořených pomocí Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="cabd8-283">Currently the Instance Metadata Service only supports instances created with Azure Resource Manager.</span></span> <span data-ttu-id="cabd8-284">V budoucnu jsme může přidat podporu pro virtuální počítače cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="cabd8-284">In the future, we may add support for Cloud Service VMs.</span></span>
3. <span data-ttu-id="cabd8-285">Delší dobou vytvořený virtuální počítač prostřednictvím Správce Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cabd8-285">I created my Virtual Machine through Azure Resource Manager a while back.</span></span> <span data-ttu-id="cabd8-286">Proč není najdete informace o metadatech výpočetní?</span><span class="sxs-lookup"><span data-stu-id="cabd8-286">Why am I not see compute metadata information?</span></span>
   * <span data-ttu-id="cabd8-287">Pro všechny virtuální počítače vytvořené po září 2016, přidejte [značky](../../azure-resource-manager/resource-group-using-tags.md) zahájíte zobrazuje výpočetní metadat.</span><span class="sxs-lookup"><span data-stu-id="cabd8-287">For any VMs created after Sep 2016, add a [Tag](../../azure-resource-manager/resource-group-using-tags.md) to start seeing compute metadata.</span></span> <span data-ttu-id="cabd8-288">Pro starší virtuální počítače (vytvořených před září 2016) přidat nebo odebrat rozšíření nebo data disky na virtuální počítač aktualizujte metadata.</span><span class="sxs-lookup"><span data-stu-id="cabd8-288">For older VMs (created before Sep 2016), add/remove extensions or data disks to the VM to refresh metadata.</span></span>
4. <span data-ttu-id="cabd8-289">Proč se zobrazuje chyba `500 Internal Server Error`?</span><span class="sxs-lookup"><span data-stu-id="cabd8-289">Why am I getting the error `500 Internal Server Error`?</span></span>
   * <span data-ttu-id="cabd8-290">Opakujte žádost podle exponenciální regrese systému.</span><span class="sxs-lookup"><span data-stu-id="cabd8-290">Please retry your request based on exponential back off system.</span></span> <span data-ttu-id="cabd8-291">Pokud potíže potrvají, kontaktujte podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="cabd8-291">If the issue persists contact  Azure support.</span></span>
5. <span data-ttu-id="cabd8-292">Kde mohu sdílet další dotazy nebo připomínky?</span><span class="sxs-lookup"><span data-stu-id="cabd8-292">Where do I share additional questions/comments?</span></span>
   * <span data-ttu-id="cabd8-293">Odešlete komentáře k http://feedback.azure.com.</span><span class="sxs-lookup"><span data-stu-id="cabd8-293">Send your comments on http://feedback.azure.com.</span></span>
7. <span data-ttu-id="cabd8-294">To funguje pro instanci nastavení škálování virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="cabd8-294">Would this work for Virtual Machine Scale Set Instance?</span></span>
   * <span data-ttu-id="cabd8-295">Ano je k dispozici pro škálování nastavení instance služby metadat.</span><span class="sxs-lookup"><span data-stu-id="cabd8-295">Yes Metadata service is available for Scale Set Instances.</span></span> 
6. <span data-ttu-id="cabd8-296">Jak získat podporu pro službu?</span><span class="sxs-lookup"><span data-stu-id="cabd8-296">How do I get support for the service?</span></span>
   * <span data-ttu-id="cabd8-297">Získat podporu pro službu, vytvořte problém podpory na portálu Azure pro virtuální počítač, kde není možné získat metadata odpověď po dlouhou opakování</span><span class="sxs-lookup"><span data-stu-id="cabd8-297">To get support for the service, create a support issue in Azure portal for the VM where you are not able to get metadata response after long retries</span></span> 

   ![Podpora metadat instance](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a><span data-ttu-id="cabd8-299">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cabd8-299">Next steps</span></span>

- <span data-ttu-id="cabd8-300">Další informace o [naplánované události](scheduled-events.md) rozhraní API **ve verzi public preview** poskytovaný službou Instance metadat.</span><span class="sxs-lookup"><span data-stu-id="cabd8-300">Learn more about the [Scheduled Events](scheduled-events.md) API **in public preview** provided by the Instance Metadata service.</span></span>
