---
title: "aaaCreate interní nástroj pro Azure Cloud Services | Microsoft Docs"
description: "Zjistěte, jak toocreate na interní nástroj pro vyrovnávání pomocí prostředí PowerShell v modelu nasazení classic hello zatížení"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 57966056-0f46-4f95-a295-483ca1ad135d
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: fe7975bca7bec3248626b0ad0fad6823e278ade2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Začínáme vytvářet interní nástroj pro vyrovnávání zatížení (Classic) pro cloudové služby

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Cloudové služby](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](load-balancer-get-started-ilb-arm-ps.md).

## <a name="configure-internal-load-balancer-for-cloud-services"></a>Konfigurace interního nástroje pro vyrovnávání zatížení pro cloudové služby

Interní nástroj pro vyrovnávání zatížení je podporován pro virtuální počítače i cloudové služby. Koncový bod vyrovnávání interní služby load vytvořené v Cloudová služba, která je mimo regionální virtuální síť, budou přístupné pouze v rámci hello cloudové služby.

Konfigurace služby Vyrovnávání zatížení pro vnitřní Hello má toobe nastavit při vytváření hello hello první nasazení v hello cloudové služby, jak je znázorněno v ukázce hello níže.

> [!IMPORTANT]
> Požadovaných toorun hello postup je toohave virtuální síti již vytvořené pro nasazení cloudu hello. Budete potřebovat hello virtuální sítě názvem a podsíť název toocreate hello interní Vyrovnávání zatížení.

### <a name="step-1"></a>Krok 1

Otevřete konfigurační soubor služby hello (.cscfg) pro vaše nasazení cloudu v sadě Visual Studio a přidejte následující části toocreate hello interní Vyrovnávání zatížení v rámci hello poslední hello "`</Role>`" položku pro konfiguraci sítě hello.

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="name of hello load balancer">
        <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

Přidejme hello hodnoty pro hello sítě konfigurační soubor tooshow jak bude vypadat. V příkladu hello předpokládá, že jste vytvořili virtuální síť s podsítí 10.0.0.0/24 názvem test_subnet a statická IP adresa 10.0.0.4 názvem "test_vnet". Nástroj pro vyrovnávání zatížení Hello budou pojmenované testLB.

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="testLB">
        <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

Další informace o schématu pro vyrovnávání zatížení hello najdete v tématu [nástroj pro vyrovnávání zatížení přidat](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Krok 2

Změňte hello služby definice (.csdef) souboru tooadd koncové body toohello interní Vyrovnávání zatížení. Hello okamžiku se vytvoří instanci role, souboru definice služby hello přidá toohello instancí role hello interní Vyrovnávání zatížení.

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
    </Endpoints>
</WorkerRole>
```

Následující hello stejné hodnoty z hello příkladu výše přidejme souboru definice služby toohello hodnoty hello.

```xml
<WorkerRole name="WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
    </Endpoints>
</WorkerRole>
```

Hello síťových přenosů bude Vyrovnávané pomocí vyrovnávání zatížení testLB hello používá port 80 pro příchozí požadavky a odesílání tooworker instance rolí také na portu 80.

## <a name="next-steps"></a>Další kroky

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)

