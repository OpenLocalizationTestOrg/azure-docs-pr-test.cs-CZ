---
title: "aaaIntroduction tooflow protokolování pro skupiny zabezpečení sítě s sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak protokoly toouse NSG tok funkce sledovací proces sítě Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: da85e946147b14717144cb47d1c742057c6dfa24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooflow-logging-for-network-security-groups"></a><span data-ttu-id="c12a7-103">Úvod tooflow protokolování pro skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="c12a7-103">Introduction tooflow logging for Network Security Groups</span></span>

<span data-ttu-id="c12a7-104">Skupina zabezpečení sítě toku protokoly jsou funkce sledovací proces sítě, který vám umožní tooview informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="c12a7-104">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="c12a7-105">Tyto protokoly toku jsou zapsané ve formátu json a zobrazit odchozí a příchozí tok na základě za pravidlo hello toku hello síťový adaptér se vztahuje na 5 řazené kolekce členů informace o toku hello (zdroj nebo cíl, zdrojový nebo cílový Port, protokol IP) a pokud hello bylo povolené přenosy nebo byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="c12a7-105">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

![Přehled protokoly toku][1]

<span data-ttu-id="c12a7-107">Při toku protokoluje cílové skupiny zabezpečení sítě, nejsou zobrazeny hello stejné jako hello další protokoly.</span><span class="sxs-lookup"><span data-stu-id="c12a7-107">While flow logs target Network Security Groups, they are not displayed hello same as hello other logs.</span></span> <span data-ttu-id="c12a7-108">Tok protokoly se ukládají pouze v rámci účtu úložiště a cesta pro následující hello protokolování, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="c12a7-108">Flow logs are stored only within a storage account and following hello logging path as shown in hello following example:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="c12a7-109">Hello stejné zásady uchovávání informací, jak je vidět další záznamy použít tooflow protokoly.</span><span class="sxs-lookup"><span data-stu-id="c12a7-109">hello same retention policies as seen on other logs apply tooflow logs.</span></span> <span data-ttu-id="c12a7-110">Protokoly mít zásady uchovávání informací, můžete nastavit od 1 den too365 dnů.</span><span class="sxs-lookup"><span data-stu-id="c12a7-110">Logs have a retention policy that can be set from 1 day too365 days.</span></span> <span data-ttu-id="c12a7-111">Pokud není nastavena zásady uchovávání informací, jsou navždy udržovat hello protokoly.</span><span class="sxs-lookup"><span data-stu-id="c12a7-111">If a retention policy is not set, hello logs are maintained forever.</span></span>

## <a name="log-file"></a><span data-ttu-id="c12a7-112">Soubor protokolu</span><span class="sxs-lookup"><span data-stu-id="c12a7-112">Log file</span></span>

<span data-ttu-id="c12a7-113">Tok protokolů mají více vlastností.</span><span class="sxs-lookup"><span data-stu-id="c12a7-113">Flow logs have multiple properties.</span></span> <span data-ttu-id="c12a7-114">Hello následujícím seznamu jsou uvedeny v seznamu hello vlastnosti, které jsou vráceny v hello NSG toku protokolu:</span><span class="sxs-lookup"><span data-stu-id="c12a7-114">hello following list is a listing of hello properties that are returned within hello NSG flow log:</span></span>

* <span data-ttu-id="c12a7-115">**čas** – čas, kdy byla zaznamenána událost hello</span><span class="sxs-lookup"><span data-stu-id="c12a7-115">**time** - Time when hello event was logged</span></span>
* <span data-ttu-id="c12a7-116">**ID systému** -skupiny zabezpečení sítě ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="c12a7-116">**systemId** - Network Security Group resource Id.</span></span>
* <span data-ttu-id="c12a7-117">**kategorie** -hello kategorie hello události, je vždy jednat NetworkSecurityGroupFlowEvent</span><span class="sxs-lookup"><span data-stu-id="c12a7-117">**category** - hello category of hello event, this is always be NetworkSecurityGroupFlowEvent</span></span>
* <span data-ttu-id="c12a7-118">**ResourceId** – prostředek Id hello NSG hello</span><span class="sxs-lookup"><span data-stu-id="c12a7-118">**resourceid** - hello resource Id of hello NSG</span></span>
* <span data-ttu-id="c12a7-119">**operationName** -vždy NetworkSecurityGroupFlowEvents</span><span class="sxs-lookup"><span data-stu-id="c12a7-119">**operationName** - Always NetworkSecurityGroupFlowEvents</span></span>
* <span data-ttu-id="c12a7-120">**vlastnosti** -kolekci vlastností hello toku</span><span class="sxs-lookup"><span data-stu-id="c12a7-120">**properties** - A collection of properties of hello flow</span></span>
    * <span data-ttu-id="c12a7-121">**Verze** -číslo verze schématu hello toku protokolu události</span><span class="sxs-lookup"><span data-stu-id="c12a7-121">**Version** - Version number of hello Flow Log event schema</span></span>
    * <span data-ttu-id="c12a7-122">**toky** -kolekce toků.</span><span class="sxs-lookup"><span data-stu-id="c12a7-122">**flows** - A collection of flows.</span></span> <span data-ttu-id="c12a7-123">Tato vlastnost má několik záznamů pro různá pravidla</span><span class="sxs-lookup"><span data-stu-id="c12a7-123">This property has multiple entries for different rules</span></span>
        * <span data-ttu-id="c12a7-124">**pravidlo** -pravidlo, pro které hello jsou uvedeny toky</span><span class="sxs-lookup"><span data-stu-id="c12a7-124">**rule** - Rule for which hello flows are listed</span></span>
            * <span data-ttu-id="c12a7-125">**toky** -kolekce toků</span><span class="sxs-lookup"><span data-stu-id="c12a7-125">**flows** - a collection of flows</span></span>
                * <span data-ttu-id="c12a7-126">**Mac** -hello adresu MAC hello síťovou kartu pro hello virtuálního počítače, kde byl shromážděn hello toku</span><span class="sxs-lookup"><span data-stu-id="c12a7-126">**mac** - hello MAC address of hello NIC for hello VM where hello flow was collected</span></span>
                * <span data-ttu-id="c12a7-127">**flowTuples** -řetězec, který obsahuje více vlastností hello toku řazené kolekce členů ve formátu čárkami</span><span class="sxs-lookup"><span data-stu-id="c12a7-127">**flowTuples** - A string that contains multiple properties for hello flow tuple in comma-separated format</span></span>
                    * <span data-ttu-id="c12a7-128">**Časové razítko** – tato hodnota je hello časového razítka, když hello toku došlo k chybě ve formátu UNIX EPOCH</span><span class="sxs-lookup"><span data-stu-id="c12a7-128">**Time Stamp** - This value is hello time stamp of when hello flow occurred in UNIX EPOCH format</span></span>
                    * <span data-ttu-id="c12a7-129">**Zdrojová adresa IP** – hello zdrojová adresa IP</span><span class="sxs-lookup"><span data-stu-id="c12a7-129">**Source IP** - hello source IP</span></span>
                    * <span data-ttu-id="c12a7-130">**Cílové IP** -hello cílovou IP adresu</span><span class="sxs-lookup"><span data-stu-id="c12a7-130">**Destination IP** - hello destination IP</span></span>
                    * <span data-ttu-id="c12a7-131">**Zdrojový Port** – hello zdrojový port</span><span class="sxs-lookup"><span data-stu-id="c12a7-131">**Source Port** - hello source port</span></span>
                    * <span data-ttu-id="c12a7-132">**Cílový Port** -hello cílový Port</span><span class="sxs-lookup"><span data-stu-id="c12a7-132">**Destination Port** - hello destination Port</span></span>
                    * <span data-ttu-id="c12a7-133">**Protokol** -hello protokol hello toku.</span><span class="sxs-lookup"><span data-stu-id="c12a7-133">**Protocol** - hello protocol of hello flow.</span></span> <span data-ttu-id="c12a7-134">Platné hodnoty jsou **T** pro TCP a **U** pro UDP</span><span class="sxs-lookup"><span data-stu-id="c12a7-134">Valid values are **T** for TCP and **U** for UDP</span></span>
                    * <span data-ttu-id="c12a7-135">**Tok provozu** -hello směr toku provozu hello.</span><span class="sxs-lookup"><span data-stu-id="c12a7-135">**Traffic Flow** - hello direction of hello traffic flow.</span></span> <span data-ttu-id="c12a7-136">Platné hodnoty jsou **I** pro příchozí a **O** pro odchozí.</span><span class="sxs-lookup"><span data-stu-id="c12a7-136">Valid values are **I** for inbound and **O** for outbound.</span></span>
                    * <span data-ttu-id="c12a7-137">**Provoz** – ať povolené nebo zakázané přenosy.</span><span class="sxs-lookup"><span data-stu-id="c12a7-137">**Traffic** - Whether traffic was allowed or denied.</span></span> <span data-ttu-id="c12a7-138">Platné hodnoty jsou **A** pro povolené a **D** pro odepřen.</span><span class="sxs-lookup"><span data-stu-id="c12a7-138">Valid values are **A** for allowed and **D** for denied.</span></span>


<span data-ttu-id="c12a7-139">Hello následuje příklad toku protokolu.</span><span class="sxs-lookup"><span data-stu-id="c12a7-139">hello following is an example of a Flow log.</span></span> <span data-ttu-id="c12a7-140">Jak vidíte, že existuje více záznamů, které následují hello seznam vlastností popsaných v předcházející části hello.</span><span class="sxs-lookup"><span data-stu-id="c12a7-140">As you can see there are multiple records that follow hello property list described in hello preceding section.</span></span> 

> [!NOTE]
> <span data-ttu-id="c12a7-141">Hodnoty ve vlastnosti flowTuples hello jsou seznam oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="c12a7-141">Values in hello flowTuples property are a comma-separated list.</span></span>
 
```json
{
    "records": 
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a><span data-ttu-id="c12a7-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c12a7-142">Next steps</span></span>

<span data-ttu-id="c12a7-143">Zjistěte, jak tooenable toku protokoly navštivte stránky [povolení toku protokolování](network-watcher-nsg-flow-logging-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c12a7-143">Learn how tooenable Flow logs by visiting [Enabling Flow logging](network-watcher-nsg-flow-logging-portal.md).</span></span>

<span data-ttu-id="c12a7-144">Další informace o NSG protokolování, navštivte stránky [protokolu analýzy pro skupiny zabezpečení sítě (Nsg)](../virtual-network/virtual-network-nsg-manage-log.md).</span><span class="sxs-lookup"><span data-stu-id="c12a7-144">Learn about NSG logging by visiting [Log analytics for network security groups (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md).</span></span>

<span data-ttu-id="c12a7-145">Zjistěte, pokud je povolené nebo zakázané na virtuálním počítači, navštivte stránky přenosy [ověřte ověřte provoz s tokem IP](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c12a7-145">Find out if traffic is allowed or denied on a VM by visiting [Verify traffic with IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

