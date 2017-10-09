## <a name="azure-dns"></a><span data-ttu-id="cbe7a-101">Azure DNS</span><span class="sxs-lookup"><span data-stu-id="cbe7a-101">Azure DNS</span></span>
<span data-ttu-id="cbe7a-102">Azure DNS je hostitelská služba domén DNS poskytnutí překladu názvů pomocí infrastruktury Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="cbe7a-102">Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure.</span></span>

| <span data-ttu-id="cbe7a-103">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="cbe7a-103">Property</span></span> | <span data-ttu-id="cbe7a-104">Popis</span><span class="sxs-lookup"><span data-stu-id="cbe7a-104">Description</span></span> | <span data-ttu-id="cbe7a-105">Hodnota vzorku</span><span class="sxs-lookup"><span data-stu-id="cbe7a-105">Sample Value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cbe7a-106">**DNSzones**</span><span class="sxs-lookup"><span data-stu-id="cbe7a-106">**DNSzones**</span></span> |<span data-ttu-id="cbe7a-107">Záznamy DNS domény zóny informace toohost určité domény</span><span class="sxs-lookup"><span data-stu-id="cbe7a-107">Domain zone information toohost DNS records of a particular domain</span></span> |<span data-ttu-id="cbe7a-108">/ subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com "</span><span class="sxs-lookup"><span data-stu-id="cbe7a-108">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com"</span></span> |

### <a name="dns-record-sets"></a><span data-ttu-id="cbe7a-109">Sady záznamů DNS</span><span class="sxs-lookup"><span data-stu-id="cbe7a-109">DNS record sets</span></span>
<span data-ttu-id="cbe7a-110">Zóny DNS mít podřízený objekt s názvem sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="cbe7a-110">DNS zones have a child object named record set.</span></span> <span data-ttu-id="cbe7a-111">Sady záznamů jsou kolekce záznamy hostitele podle typu zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="cbe7a-111">Record sets are a collection of host records by type for a DNS zone.</span></span> <span data-ttu-id="cbe7a-112">Typy záznamů jsou A, AAAA, CNAME, MX, NS, SOA, SRV a TXT.</span><span class="sxs-lookup"><span data-stu-id="cbe7a-112">Record types are A, AAAA, CNAME, MX, NS, SOA,SRV and TXT.</span></span>

| <span data-ttu-id="cbe7a-113">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="cbe7a-113">Property</span></span> | <span data-ttu-id="cbe7a-114">Popis</span><span class="sxs-lookup"><span data-stu-id="cbe7a-114">Description</span></span> | <span data-ttu-id="cbe7a-115">Ukázková hodnota</span><span class="sxs-lookup"><span data-stu-id="cbe7a-115">Sample value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cbe7a-116">A</span><span class="sxs-lookup"><span data-stu-id="cbe7a-116">A</span></span> |<span data-ttu-id="cbe7a-117">Typ záznamu IPv4</span><span class="sxs-lookup"><span data-stu-id="cbe7a-117">IPv4 record type</span></span> |<span data-ttu-id="cbe7a-118">/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/A/www</span><span class="sxs-lookup"><span data-stu-id="cbe7a-118">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/A/www</span></span> |
| <span data-ttu-id="cbe7a-119">AAAA</span><span class="sxs-lookup"><span data-stu-id="cbe7a-119">AAAA</span></span> |<span data-ttu-id="cbe7a-120">Typ záznamu IPv6</span><span class="sxs-lookup"><span data-stu-id="cbe7a-120">IPv6 record type</span></span> |<span data-ttu-id="cbe7a-121">/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord</span><span class="sxs-lookup"><span data-stu-id="cbe7a-121">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord</span></span> |
| <span data-ttu-id="cbe7a-122">CNAME</span><span class="sxs-lookup"><span data-stu-id="cbe7a-122">CNAME</span></span> |<span data-ttu-id="cbe7a-123">Typ záznamu kanonický název <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="cbe7a-123">canonical name record type <sup>1</sup></span></span> |<span data-ttu-id="cbe7a-124">/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/CNAME/www</span><span class="sxs-lookup"><span data-stu-id="cbe7a-124">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/CNAME/www</span></span> |
| <span data-ttu-id="cbe7a-125">MX</span><span class="sxs-lookup"><span data-stu-id="cbe7a-125">MX</span></span> |<span data-ttu-id="cbe7a-126">Typ záznamu e-mailu</span><span class="sxs-lookup"><span data-stu-id="cbe7a-126">mail record type</span></span> |<span data-ttu-id="cbe7a-127">/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/MX/mail</span><span class="sxs-lookup"><span data-stu-id="cbe7a-127">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/MX/mail</span></span> |
| <span data-ttu-id="cbe7a-128">NS</span><span class="sxs-lookup"><span data-stu-id="cbe7a-128">NS</span></span> |<span data-ttu-id="cbe7a-129">Typ záznamu název serveru</span><span class="sxs-lookup"><span data-stu-id="cbe7a-129">name server record type</span></span> |<span data-ttu-id="cbe7a-130">/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/NS/</span><span class="sxs-lookup"><span data-stu-id="cbe7a-130">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/NS/</span></span> |
| <span data-ttu-id="cbe7a-131">SOA</span><span class="sxs-lookup"><span data-stu-id="cbe7a-131">SOA</span></span> |<span data-ttu-id="cbe7a-132">Začátek typ záznamu autority <sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="cbe7a-132">Start of Authority record type <sup>2</sup></span></span> |<span data-ttu-id="cbe7a-133">/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SOA</span><span class="sxs-lookup"><span data-stu-id="cbe7a-133">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/SOA</span></span> |
| <span data-ttu-id="cbe7a-134">SRV</span><span class="sxs-lookup"><span data-stu-id="cbe7a-134">SRV</span></span> |<span data-ttu-id="cbe7a-135">Typ záznamu služby</span><span class="sxs-lookup"><span data-stu-id="cbe7a-135">service record type</span></span> |<span data-ttu-id="cbe7a-136">/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SRV</span><span class="sxs-lookup"><span data-stu-id="cbe7a-136">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/SRV</span></span> |

<span data-ttu-id="cbe7a-137"><sup>1</sup> umožňuje pouze jednu hodnotu na sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="cbe7a-137"><sup>1</sup> only allows one value per record set.</span></span>

<span data-ttu-id="cbe7a-138"><sup>2</sup> umožňuje pouze jeden typ záznamu SOA za zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="cbe7a-138"><sup>2</sup> only allows one record type SOA per DNS zone.</span></span> 

<span data-ttu-id="cbe7a-139">Ukázka zónu DNS ve formátu Json:</span><span class="sxs-lookup"><span data-stu-id="cbe7a-139">Sample of DNS zone in Json format:</span></span>

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "newZoneName": {
          "type": "String",
          "metadata": {
              "description": "hello name of hello DNS zone toobe created."
          }
        },
        "newRecordName": {
          "type": "String",
          "defaultValue": "www",
          "metadata": {
              "description": "hello name of hello DNS record toobe created.  hello name is relative toohello zone, not hello FQDN."
          }
        }
      },
      "resources": 
      [
        {
          "type": "microsoft.network/dnszones",
          "name": "[parameters('newZoneName')]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": {
          }
        },
        {
          "type": "microsoft.network/dnszones/a",
          "name": "[concat(parameters('newZoneName'), concat('/', parameters('newRecordName')))]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": 
          {
            "TTL": 3600,
            "ARecords": 
            [
                {
                    "ipv4Address": "1.2.3.4"
                },
                {
                    "ipv4Address": "1.2.3.5"
                }
            ]
          },
          "dependsOn": [
            "[concat('Microsoft.Network/dnszones/', parameters('newZoneName'))]"
          ]
        }
          ]
    }

## <a name="additional-resources"></a><span data-ttu-id="cbe7a-140">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cbe7a-140">Additional resources</span></span>
<span data-ttu-id="cbe7a-141">Čtení hello [dokumentace k REST API pro zóny DNS ](https://msdn.microsoft.com/library/azure/mt130626.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="cbe7a-141">Read hello [REST API documentation for DNS zones ](https://msdn.microsoft.com/library/azure/mt130626.aspx) for more information.</span></span>

<span data-ttu-id="cbe7a-142">Čtení hello [dokumentace k REST API pro sady záznamů DNS](https://msdn.microsoft.com/library/azure/mt130627.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="cbe7a-142">Read hello [REST API documentation for DNS record sets](https://msdn.microsoft.com/library/azure/mt130627.aspx) for more information.</span></span>

