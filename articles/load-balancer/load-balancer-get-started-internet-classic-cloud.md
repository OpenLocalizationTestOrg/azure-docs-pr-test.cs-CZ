---
title: "Vyrovnávání zatížení aaaCreate směřujících Internetu pro cloudové služby Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže v modelu nasazení classic pro cloudové služby"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 0bb16f96-56a6-429f-88f5-0de2d0136756
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: d93cf76d417cbfc744cf07ba48c43a63cc14df69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Začínáme vytvářet internetový nástroj pro vyrovnávání zatížení pro cloudové služby

> [!div class="op_single_selector"]
> * [Portál Azure Classic](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Azure Resource Manager a Klasický model. Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md). Hello dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty hello hello horní části tohoto článku. Tento článek se týká modelu nasazení classic hello. Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

Cloudové služby se automaticky nakonfigurují pomocí služby Vyrovnávání zatížení a lze přizpůsobit prostřednictvím modelu služby hello.

## <a name="create-a-load-balancer-using-hello-service-definition-file"></a>Vytvořit nástroj pro vyrovnávání zatížení, pomocí souboru definice služby hello

Hello Azure SDK pro .NET 2.5 tooupdate mohou využívat cloudové služby. Nastavení pro koncový bod pro cloudové služby se provádí v hello [služby definice](https://msdn.microsoft.com/library/azure/gg557553.aspx) souboru .csdef.

Hello následující příklad ukazuje, jak jsou nakonfigurované soubor servicedefinition.csdef pro nasazení cloudu:

Kontrola hello fragment souboru .csdef hello generované nasazení cloudu, uvidíte hello externí koncovým bodem nakonfigurovaným toouse porty HTTP na portu 10000, 10001 a 10002.

```xml
<ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
<Endpoints>
    <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
    <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
    <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

    <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

    <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
        <AllocatePublicPortFrom>
            <FixedPortRange min=“10110” max=“10120“  />
        </AllocatePublicPortFrom>
    </InstanceInputEndpoint>
    <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
</Endpoints>
    </WorkerRole>
</ServiceDefinition>
```

## <a name="check-load-balancer-health-status-for-cloud-services"></a>Kontrola stavu nástroje pro vyrovnávání zatížení pro cloudové služby

Hello následuje příklad test stavu:

```xml
<LoadBalancerProbes>
    <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
</LoadBalancerProbes>
```

Hello nástroj pro vyrovnávání zatížení kombinuje hello koncového bodu hello hello informací a toocreate hello test adresy URL ve formě hello `http://{DIP of VM}:80/Probe.aspx` který lze použít tooquery hello stavu služby hello.

Hello služby rozpozná pravidelné sondy z hello stejnou IP adresu. Toto je hello stavu zkušebního požadavku pocházející od hostitele hello hello uzlu, kde je spuštěný virtuální počítač hello. Služba Hello má toorespond s stavový kód HTTP 200 pro tooassume nástroje pro vyrovnávání zatížení hello, že služba hello je v pořádku. Další stav protokolu HTTP kódu (například 503) přímo trvá hello virtuální počítač ze otočení.

definice testu Hello také řídí hello četnost kontroly hello. V našem případě výše je nástroj pro vyrovnávání zatížení hello zjišťování hello koncového bodu každých 5 sekund. Pokud není žádná kladná odpověď pro 10 sekund (dva intervaly testu), test hello se předpokládá, že dolů a hello virtuálního počítače se provede mimo otočení. Podobně pokud je služba hello mimo otočení a přijetí kladná odpověď, hello služby je vrátit zpět toorotation hned. Pokud služba hello je kolísal mezi v pořádku a není v pořádku, nástroj pro vyrovnávání zatížení hello můžete rozhodnout toodelay hello umístění nových hello služby back toorotation, dokud byla pro počet sondy v pořádku.

Vyhledejte hello služby definice schématu hello [test stavu](https://msdn.microsoft.com/library/azure/jj151530.aspx) Další informace.

## <a name="next-steps"></a>Další kroky

[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)

