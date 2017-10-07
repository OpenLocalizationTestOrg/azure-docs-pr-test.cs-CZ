---
title: "aaaMonitor operace, události a čítače pro nástroj pro vyrovnávání zatížení | Microsoft Docs"
description: "Zjistěte, jak tooenable výstrahy událostí a sběru dat stavu stav protokolování pro nástroj pro vyrovnávání zatížení Azure"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 56656d74-0241-4096-88c8-aa88515d676d
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: ac53c2254e06cad780ad6144c5c30f0085d12576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-azure-load-balancer"></a>Log Analytics pro Azure Load Balancer

Můžete použít různé typy protokolů v Azure toomanage a řešení potíží s nástroje pro vyrovnávání zatížení. Některé z těchto protokolů jsou přístupné prostřednictvím portálu hello. Všechny protokoly můžete extrahovat z Azure blob storage a zobrazit v různých nástrojů, jako je například aplikace Excel a PowerBI. Další informace o různých typech hello protokolů v následujícím seznamu hello.

* **Protokoly auditu:** můžete použít [protokoly auditu Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (dříve označované jako operační protokoly) tooview všechny operace se odeslaná tooyour předplatná Azure a jejich stav. Protokoly auditu jsou ve výchozím nastavení povolené a lze zobrazit v hello portálu Azure.
* **Výstrahy protokoly událostí:** tento protokol tooview výstrahy rasied můžete použít nástroj pro vyrovnávání zatížení hello. Stav Hello nástroje pro vyrovnávání zatížení hello shromažďovaných každých pět minut. Tento protokol je zapsán pouze pokud je vyvolána o událost výstrahy nástroje pro vyrovnávání zatížení.
* **Stav testu protokoly:** můžete použít tento protokol tooview problémů zjištěných váš test stavu, jako je například hello počet instancí ve vašem fondu back-end, které nejsou přijímání požadavků z nástroje pro vyrovnávání zatížení hello z důvodu selhání kontroly stavu. Tento protokol je zapsán toowhen dojde ke změně v hello stav testu.

> [!IMPORTANT]
> Analýza protokolu aktuálně funguje pouze pro internetové nástroje pro vyrovnávání zatížení. Protokoly jsou dostupné pouze pro prostředky nasazené v modelu nasazení Resource Manager hello. Protokoly nelze použít pro prostředky v modelu nasazení classic hello. Další informace o modelech nasazení hello najdete v tématu [nasazení Resource Manager principy a nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Povolit protokolování

Pro každý prostředek Resource Manager je automaticky povolené protokolování auditu. Potřebujete tooenable události a stav testu protokolování toostart shromažďování dat hello k dispozici prostřednictvím tyto protokoly. Pomocí následujících kroků tooenable protokolování hello.

Přihlášení toohello [portál Azure](http://portal.azure.com). Pokud ještě nemáte nástroj pro vyrovnávání zatížení [vytvořit nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) než budete pokračovat.

1. Hello portálu, klikněte na tlačítko **Procházet**.
2. Vyberte **nástroje pro vyrovnávání zatížení**.

    ![portál – nástroj na Vyrovnávání zatížení](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Vyberte existující pro vyrovnávání zatížení >> **všechna nastavení**.
4. Na pravé straně hello dialogového okna hello pod názvem hello nástroje pro vyrovnávání zatížení hello, posuňte příliš**monitorování**, klikněte na tlačítko **diagnostiky**.

    ![portál – nastavení vyrovnávání zátěže](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. V hello **diagnostiky** podokně v části **stav**, vyberte **na**.
6. Klikněte na tlačítko **účet úložiště**.
7. V části **protokoly**, vybrat existující účet úložiště, nebo vytvořte novou. Pomocí posuvníku toodetermine hello počet dní, za data událostí se uloží v protokolech událostí hello. 
8. Klikněte na **Uložit**.

    ![Portál – protokolů diagnostiky](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> Protokoly auditu nevyžadují, aby účet samostatného úložiště. Hello použití úložiště pro události a stav testu protokolování bude platit poplatky služby.

## <a name="audit-log"></a>Protokol auditování

ve výchozím nastavení je generování Hello protokolu auditu. protokoly Hello se zachovají 90 dní v úložišti Azure a protokoly událostí. Další informace o tyto protokoly načtením hello [zobrazení událostí a protokolů auditování](../monitoring-and-diagnostics/insights-debugging-with-events.md) článku.

## <a name="alert-event-log"></a>Upozornění v protokolu událostí

Tento protokol je generovaný pouze v případě, že jste povolili na na základě nástroje pro vyrovnávání zatížení. Hello události se zaznamenávají ve formátu JSON a uložený v účtu storage hello, že jste zadali, pokud jste povolili protokolování hello. Hello tady je příklad události.

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

výstup ukazuje hello Hello JSON *eventname* vlastnost, která popíše hello důvod pro vyrovnávání zatížení hello vytvořit výstrahu. V takovém případě hello výstraha generována se z důvodu tooTCP port vyčerpání způsobené zdroj, který omezuje IP NAT (překládat pomocí SNAT).

## <a name="health-probe-log"></a>Stav testu protokolu

Tento protokol je generovaný pouze v případě, že jste povolili na za zatížení vyrovnávání základ popsaných výše. Hello data je uložený v účtu úložiště hello, že jste zadali, pokud jste povolili protokolování hello. Je-li vytvořit kontejner s názvem "insights loadbalancerprobehealthstatus protokoly, a je zaznamenána hello následující data:

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

výstup JSON Hello zobrazuje hello vlastnosti pole hello základní informace o hello test stavu. Hello *dipDownCount* vlastnost zobrazuje hello celkový počet instancí v hello back-end, které nejsou přijímá síťový provoz z důvodu toofailed testu odpovědi.

## <a name="view-and-analyze-hello-audit-log"></a>Zobrazení a analýza protokolů auditu hello

Můžete zobrazit a analyzovat data protokolu auditu pomocí kteréhokoli hello následující metody:

* **Nástroje Azure:** načíst informace z protokolů auditu hello pomocí Azure Powershellu hello Azure rozhraní příkazového řádku (CLI), hello REST API služby Azure, nebo hello portál Azure preview. Podrobné pokyny pro jednotlivé metody jsou podrobně popsané na hello [auditovat operace s Resource Managerem](../azure-resource-manager/resource-group-audit.md) článku.
* **Power BI:** Pokud již nemáte [Power BI](https://powerbi.microsoft.com/pricing) účet, můžete zkusit je zdarma. Pomocí hello [obsahu protokoly auditu Azure pack pro Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), můžete analyzovat svá data pomocí předem nakonfigurovaných řídicí panely, nebo můžete přizpůsobit zobrazení toosuit vašim požadavkům.

## <a name="view-and-analyze-hello-health-probe-and-event-log"></a>Zobrazení a analýza protokolů událostí a test stavu hello

Potřebujete úložiště, tooyour tooconnect účtu a načítat položky protokolu hello JSON pro kontrolu protokolů událostí a stavu. Jakmile si stáhnout soubory hello JSON, můžete je převést tooCSV a zobrazení v aplikaci Excel, PowerBI nebo jakýkoli jiný nástroj pro vizualizaci dat.

> [!TIP]
> Pokud jste obeznámeni s Visual Studio a základní koncepty změna hodnoty konstanty a proměnné v jazyce C#, můžete použít hello [protokolu nástroje Převaděč](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostupné z Githubu.

## <a name="additional-resources"></a>Další zdroje

* [Vaše protokoly auditu Azure s Power BI vizualizovat](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) příspěvku na blogu.
* [Zobrazení a analýza protokolů auditu Azure v Power BI a další](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) příspěvku na blogu.

## <a name="next-steps"></a>Další kroky

[Porozumění testům nástroje pro vyrovnávání zatížení](load-balancer-custom-probe-overview.md)
