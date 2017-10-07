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
# <a name="introduction-tooflow-logging-for-network-security-groups"></a>Úvod tooflow protokolování pro skupiny zabezpečení sítě

Skupina zabezpečení sítě toku protokoly jsou funkce sledovací proces sítě, který vám umožní tooview informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě. Tyto protokoly toku jsou zapsané ve formátu json a zobrazit odchozí a příchozí tok na základě za pravidlo hello toku hello síťový adaptér se vztahuje na 5 řazené kolekce členů informace o toku hello (zdroj nebo cíl, zdrojový nebo cílový Port, protokol IP) a pokud hello bylo povolené přenosy nebo byl odepřen.

![Přehled protokoly toku][1]

Při toku protokoluje cílové skupiny zabezpečení sítě, nejsou zobrazeny hello stejné jako hello další protokoly. Tok protokoly se ukládají pouze v rámci účtu úložiště a cesta pro následující hello protokolování, jak je znázorněno v hello následující ukázka:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

Hello stejné zásady uchovávání informací, jak je vidět další záznamy použít tooflow protokoly. Protokoly mít zásady uchovávání informací, můžete nastavit od 1 den too365 dnů. Pokud není nastavena zásady uchovávání informací, jsou navždy udržovat hello protokoly.

## <a name="log-file"></a>Soubor protokolu

Tok protokolů mají více vlastností. Hello následujícím seznamu jsou uvedeny v seznamu hello vlastnosti, které jsou vráceny v hello NSG toku protokolu:

* **čas** – čas, kdy byla zaznamenána událost hello
* **ID systému** -skupiny zabezpečení sítě ID prostředku.
* **kategorie** -hello kategorie hello události, je vždy jednat NetworkSecurityGroupFlowEvent
* **ResourceId** – prostředek Id hello NSG hello
* **operationName** -vždy NetworkSecurityGroupFlowEvents
* **vlastnosti** -kolekci vlastností hello toku
    * **Verze** -číslo verze schématu hello toku protokolu události
    * **toky** -kolekce toků. Tato vlastnost má několik záznamů pro různá pravidla
        * **pravidlo** -pravidlo, pro které hello jsou uvedeny toky
            * **toky** -kolekce toků
                * **Mac** -hello adresu MAC hello síťovou kartu pro hello virtuálního počítače, kde byl shromážděn hello toku
                * **flowTuples** -řetězec, který obsahuje více vlastností hello toku řazené kolekce členů ve formátu čárkami
                    * **Časové razítko** – tato hodnota je hello časového razítka, když hello toku došlo k chybě ve formátu UNIX EPOCH
                    * **Zdrojová adresa IP** – hello zdrojová adresa IP
                    * **Cílové IP** -hello cílovou IP adresu
                    * **Zdrojový Port** – hello zdrojový port
                    * **Cílový Port** -hello cílový Port
                    * **Protokol** -hello protokol hello toku. Platné hodnoty jsou **T** pro TCP a **U** pro UDP
                    * **Tok provozu** -hello směr toku provozu hello. Platné hodnoty jsou **I** pro příchozí a **O** pro odchozí.
                    * **Provoz** – ať povolené nebo zakázané přenosy. Platné hodnoty jsou **A** pro povolené a **D** pro odepřen.


Hello následuje příklad toku protokolu. Jak vidíte, že existuje více záznamů, které následují hello seznam vlastností popsaných v předcházející části hello. 

> [!NOTE]
> Hodnoty ve vlastnosti flowTuples hello jsou seznam oddělený čárkami.
 
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

## <a name="next-steps"></a>Další kroky

Zjistěte, jak tooenable toku protokoly navštivte stránky [povolení toku protokolování](network-watcher-nsg-flow-logging-portal.md).

Další informace o NSG protokolování, navštivte stránky [protokolu analýzy pro skupiny zabezpečení sítě (Nsg)](../virtual-network/virtual-network-nsg-manage-log.md).

Zjistěte, pokud je povolené nebo zakázané na virtuálním počítači, navštivte stránky přenosy [ověřte ověřte provoz s tokem IP](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

