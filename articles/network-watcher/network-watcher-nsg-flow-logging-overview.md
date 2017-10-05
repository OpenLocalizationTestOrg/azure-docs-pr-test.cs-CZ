---
title: "Úvod do toku protokolování pro skupiny zabezpečení sítě s sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak pomocí protokolů NSG tok funkce sledovací proces sítě Azure"
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
ms.openlocfilehash: b7a9162d6c6219b6b1c51a49cd34b9616e9d3e8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a><span data-ttu-id="ad371-103">Úvod do toku protokolování pro skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="ad371-103">Introduction to flow logging for Network Security Groups</span></span>

<span data-ttu-id="ad371-104">Skupina zabezpečení sítě toku protokoly jsou funkce sledovací proces sítě, která vám umožní zobrazit informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="ad371-104">Network Security Group flow logs are a feature of Network Watcher that allows you to view information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="ad371-105">Tyto protokoly toku jsou zapsané ve formátu json a zobrazit příchozí a odchozí toky na základě pravidla na síťový adaptér tok se vztahuje na 5 řazené kolekce členů informace o toku (protokol IP zdroj nebo cíl, zdrojový nebo cílový Port), a pokud se povoluje nebo odepírá provoz.</span><span class="sxs-lookup"><span data-stu-id="ad371-105">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

![Přehled protokoly toku][1]

<span data-ttu-id="ad371-107">I když toku protokoluje cílové skupiny zabezpečení sítě, nejsou zobrazí stejné jako další protokoly.</span><span class="sxs-lookup"><span data-stu-id="ad371-107">While flow logs target Network Security Groups, they are not displayed the same as the other logs.</span></span> <span data-ttu-id="ad371-108">Tok protokoly se ukládají pouze v rámci účtu úložiště a následující cesta pro protokolování, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="ad371-108">Flow logs are stored only within a storage account and following the logging path as shown in the following example:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="ad371-109">Do protokolů toku použít stejné zásady uchovávání informací jako zobrazené na další protokoly.</span><span class="sxs-lookup"><span data-stu-id="ad371-109">The same retention policies as seen on other logs apply to flow logs.</span></span> <span data-ttu-id="ad371-110">Protokoly mít zásady uchovávání informací, můžete nastavit od 1 den do 365 dní.</span><span class="sxs-lookup"><span data-stu-id="ad371-110">Logs have a retention policy that can be set from 1 day to 365 days.</span></span> <span data-ttu-id="ad371-111">Pokud zásady uchovávání nejsou nastavené, protokoly se ukládají navždy.</span><span class="sxs-lookup"><span data-stu-id="ad371-111">If a retention policy is not set, the logs are maintained forever.</span></span>

## <a name="log-file"></a><span data-ttu-id="ad371-112">Soubor protokolu</span><span class="sxs-lookup"><span data-stu-id="ad371-112">Log file</span></span>

<span data-ttu-id="ad371-113">Tok protokolů mají více vlastností.</span><span class="sxs-lookup"><span data-stu-id="ad371-113">Flow logs have multiple properties.</span></span> <span data-ttu-id="ad371-114">V následujícím seznamu je seznam vlastností, které jsou vráceny v protokolu toku NSG:</span><span class="sxs-lookup"><span data-stu-id="ad371-114">The following list is a listing of the properties that are returned within the NSG flow log:</span></span>

* <span data-ttu-id="ad371-115">**čas** – čas při protokolu byla zaznamenána událost</span><span class="sxs-lookup"><span data-stu-id="ad371-115">**time** - Time when the event was logged</span></span>
* <span data-ttu-id="ad371-116">**ID systému** -skupiny zabezpečení sítě ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="ad371-116">**systemId** - Network Security Group resource Id.</span></span>
* <span data-ttu-id="ad371-117">**kategorie** – kategorie události, je vždy jednat NetworkSecurityGroupFlowEvent</span><span class="sxs-lookup"><span data-stu-id="ad371-117">**category** - The category of the event, this is always be NetworkSecurityGroupFlowEvent</span></span>
* <span data-ttu-id="ad371-118">**ResourceId** – prostředek Id NSG</span><span class="sxs-lookup"><span data-stu-id="ad371-118">**resourceid** - The resource Id of the NSG</span></span>
* <span data-ttu-id="ad371-119">**operationName** -vždy NetworkSecurityGroupFlowEvents</span><span class="sxs-lookup"><span data-stu-id="ad371-119">**operationName** - Always NetworkSecurityGroupFlowEvents</span></span>
* <span data-ttu-id="ad371-120">**vlastnosti** -kolekci vlastností toku</span><span class="sxs-lookup"><span data-stu-id="ad371-120">**properties** - A collection of properties of the flow</span></span>
    * <span data-ttu-id="ad371-121">**Verze** -číslo verze schématu toku protokolu událostí</span><span class="sxs-lookup"><span data-stu-id="ad371-121">**Version** - Version number of the Flow Log event schema</span></span>
    * <span data-ttu-id="ad371-122">**toky** -kolekce toků.</span><span class="sxs-lookup"><span data-stu-id="ad371-122">**flows** - A collection of flows.</span></span> <span data-ttu-id="ad371-123">Tato vlastnost má několik záznamů pro různá pravidla</span><span class="sxs-lookup"><span data-stu-id="ad371-123">This property has multiple entries for different rules</span></span>
        * <span data-ttu-id="ad371-124">**pravidlo** -pravidla pro tyto toky jsou uvedeny</span><span class="sxs-lookup"><span data-stu-id="ad371-124">**rule** - Rule for which the flows are listed</span></span>
            * <span data-ttu-id="ad371-125">**toky** -kolekce toků</span><span class="sxs-lookup"><span data-stu-id="ad371-125">**flows** - a collection of flows</span></span>
                * <span data-ttu-id="ad371-126">**Mac** – adresa MAC síťového adaptéru pro virtuální počítač, kde byl shromážděn toku</span><span class="sxs-lookup"><span data-stu-id="ad371-126">**mac** - The MAC address of the NIC for the VM where the flow was collected</span></span>
                * <span data-ttu-id="ad371-127">**flowTuples** -řetězec, který obsahuje více vlastností řazené kolekce členů toku ve formátu čárkami</span><span class="sxs-lookup"><span data-stu-id="ad371-127">**flowTuples** - A string that contains multiple properties for the flow tuple in comma-separated format</span></span>
                    * <span data-ttu-id="ad371-128">**Časové razítko** – tato hodnota je časové razítko, když toku došlo k chybě ve formátu UNIX EPOCH</span><span class="sxs-lookup"><span data-stu-id="ad371-128">**Time Stamp** - This value is the time stamp of when the flow occurred in UNIX EPOCH format</span></span>
                    * <span data-ttu-id="ad371-129">**Zdrojová adresa IP** -zdrojové IP adresy</span><span class="sxs-lookup"><span data-stu-id="ad371-129">**Source IP** - The source IP</span></span>
                    * <span data-ttu-id="ad371-130">**Cílové IP** -cílovou IP adresu</span><span class="sxs-lookup"><span data-stu-id="ad371-130">**Destination IP** - The destination IP</span></span>
                    * <span data-ttu-id="ad371-131">**Zdrojový Port** -zdrojový port</span><span class="sxs-lookup"><span data-stu-id="ad371-131">**Source Port** - The source port</span></span>
                    * <span data-ttu-id="ad371-132">**Cílový Port** – cílový Port</span><span class="sxs-lookup"><span data-stu-id="ad371-132">**Destination Port** - The destination Port</span></span>
                    * <span data-ttu-id="ad371-133">**Protokol** -protokol toku.</span><span class="sxs-lookup"><span data-stu-id="ad371-133">**Protocol** - The protocol of the flow.</span></span> <span data-ttu-id="ad371-134">Platné hodnoty jsou **T** pro TCP a **U** pro UDP</span><span class="sxs-lookup"><span data-stu-id="ad371-134">Valid values are **T** for TCP and **U** for UDP</span></span>
                    * <span data-ttu-id="ad371-135">**Tok provozu** -směr toku přenosu.</span><span class="sxs-lookup"><span data-stu-id="ad371-135">**Traffic Flow** - The direction of the traffic flow.</span></span> <span data-ttu-id="ad371-136">Platné hodnoty jsou **I** pro příchozí a **O** pro odchozí.</span><span class="sxs-lookup"><span data-stu-id="ad371-136">Valid values are **I** for inbound and **O** for outbound.</span></span>
                    * <span data-ttu-id="ad371-137">**Provoz** – ať povolené nebo zakázané přenosy.</span><span class="sxs-lookup"><span data-stu-id="ad371-137">**Traffic** - Whether traffic was allowed or denied.</span></span> <span data-ttu-id="ad371-138">Platné hodnoty jsou **A** pro povolené a **D** pro odepřen.</span><span class="sxs-lookup"><span data-stu-id="ad371-138">Valid values are **A** for allowed and **D** for denied.</span></span>


<span data-ttu-id="ad371-139">Následuje příklad toku protokolu.</span><span class="sxs-lookup"><span data-stu-id="ad371-139">The following is an example of a Flow log.</span></span> <span data-ttu-id="ad371-140">Jak vidíte, že existuje více záznamů, které následují seznam vlastností, které jsou popsané v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="ad371-140">As you can see there are multiple records that follow the property list described in the preceding section.</span></span> 

> [!NOTE]
> <span data-ttu-id="ad371-141">Hodnoty ve vlastnosti flowTuples jsou seznam oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="ad371-141">Values in the flowTuples property are a comma-separated list.</span></span>
 
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

## <a name="next-steps"></a><span data-ttu-id="ad371-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad371-142">Next steps</span></span>

<span data-ttu-id="ad371-143">Informace o povolení toku protokoly navštivte stránky [povolení toku protokolování](network-watcher-nsg-flow-logging-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ad371-143">Learn how to enable Flow logs by visiting [Enabling Flow logging](network-watcher-nsg-flow-logging-portal.md).</span></span>

<span data-ttu-id="ad371-144">Další informace o NSG protokolování, navštivte stránky [protokolu analýzy pro skupiny zabezpečení sítě (Nsg)](../virtual-network/virtual-network-nsg-manage-log.md).</span><span class="sxs-lookup"><span data-stu-id="ad371-144">Learn about NSG logging by visiting [Log analytics for network security groups (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md).</span></span>

<span data-ttu-id="ad371-145">Zjistěte, pokud je povolené nebo zakázané na virtuálním počítači, navštivte stránky přenosy [ověřte ověřte provoz s tokem IP](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ad371-145">Find out if traffic is allowed or denied on a VM by visiting [Verify traffic with IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

