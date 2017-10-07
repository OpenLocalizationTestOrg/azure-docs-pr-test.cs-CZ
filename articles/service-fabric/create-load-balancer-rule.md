---
title: "aaaCreate pravidlo Vyrovnávání zatížení Azure pro cluster"
description: "Konfigurace vyrovnávání zatížení Azure tooopen portů pro váš cluster Azure Service Fabric."
services: service-fabric
documentationcenter: na
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/22/2017
ms.author: adegeo
ms.openlocfilehash: 4a40f62422bd895d782be8cbaace5f4e1af81db3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-for-a-service-fabric-cluster"></a>Otevřete porty pro cluster Service Fabric

Nástroj pro vyrovnávání zatížení Hello nasazené se službou Azure Service Fabric cluster bude směrovat provoz tooyour aplikaci spuštěnou na uzlu. Pokud změníte vaší aplikace toouse jiný port, musí vystavit tento port (nebo směrovat jiný port) v hello nástroj pro vyrovnávání zatížení Azure.

Při nasazení vaší tooAzure fabric clusteru služby Vyrovnávání zatížení byl automaticky vytvořen za vás. Pokud jste nástroj pro vyrovnávání zatížení, přečtěte si téma [konfigurace vyrovnávání zatížení internetového](..\load-balancer\load-balancer-get-started-internet-portal.md).

## <a name="configure-service-fabric"></a>Konfigurace infrastruktury služby

Aplikace Service Fabric **ServiceManifest.xml** definuje koncové body hello aplikace očekává toouse konfiguračního souboru. Po hello konfigurační soubor byl aktualizovaný toodefine koncový bod, hello nástroj pro vyrovnávání zatížení musí být aktualizované tooexpose tento (nebo jiný) port. Další informace o tom, jak toocreate hello koncový bod prostředků infrastruktury služby najdete v tématu [nastavit koncový bod](service-fabric-service-manifest-resources.md).

## <a name="create-a-load-balancer-rule"></a>Vytvořit pravidlo Vyrovnávání zatížení.

Pravidlo Vyrovnávání zatížení otevře k portu internetového a předá provoz toohello interní uzlu portu, který používá vaše aplikace. Pokud jste nástroj pro vyrovnávání zatížení, přečtěte si téma [konfigurace vyrovnávání zatížení internetového](..\load-balancer\load-balancer-get-started-internet-portal.md).

toocreate pravidlo Vyrovnávání zatížení, budete potřebovat toocollect hello následující informace:

- Název služby Vyrovnávání zatížení.
- Skupiny prostředků hello zatížení vyrovnávání a service fabric cluster.
- Externí port.
- Interní port.

## <a name="azure-cli"></a>Azure CLI
>[!NOTE]
>Pokud potřebujete název hello toodetermine hello nástroj pro vyrovnávání zatížení, použijte tento příkaz get tooquickly seznam všechny skupiny prostředků hello přidružené a nástroje pro vyrovnávání zatížení.
>
>`az network lb list --query "[].{ResourceGroup: resourceGroup, Name: name}"`
>

Potrvá to jenom jeden příkaz toocreate pravidlo Vyrovnávání zatížení s hello **rozhraní příkazového řádku Azure**. Stačí tooknow obou hello název hello zatížení vyrovnávání a prostředků skupiny toocreate nové pravidlo.

```azurecli
az network lb rule create --backend-port 40000 --frontend-port 39999 --protocol Tcp --lb-name LB-svcfab3 -g svcfab_cli -n my-app-rule
```

Hello příkazu příkazového řádku Azure CLI má několik parametrů, které jsou popsány v následující tabulce hello:

| Parametr | Popis |
| --------- | ----------- |
| `--backend-port`  | Hello port hello service fabric aplikace naslouchá. |
| `--frontend-port` | Hello port hello načíst zpřístupňuje vyrovnávání pro externí připojení. |
| `-lb-name` | Název Hello hello načíst toochange vyrovnávání. |
| `-g`       | Hello skupinu prostředků, který má nástroj pro vyrovnávání zatížení hello a service fabric cluster. |
| `-n`       | Zvolený název pravidla hello Hello. |


>[!NOTE]
>Další informace o tom, jak toocreate Vyrovnávání zatížení s hello Azure CLI, najdete v části [vytvořit nástroj pro vyrovnávání zatížení s hello rozhraní příkazového řádku Azure](..\load-balancer\load-balancer-get-started-internet-arm-cli.md).

## <a name="powershell"></a>PowerShell

>[!NOTE]
>Pokud potřebujete název hello toodetermine hello nástroj pro vyrovnávání zatížení, použijte tento příkaz get tooquickly seznam všechny skupiny přidružených prostředků a nástroje pro vyrovnávání zatížení.
>
>`Get-AzureRmLoadBalancer | Select Name, ResourceGroupName`

PowerShell je trochu složitější než hello rozhraní příkazového řádku Azure. Koncepčně proveďte následující kroky toocreate hello pravidlo.

1. Nástroj pro vyrovnávání zatížení hello získáte ze služby Azure.
2. Vytvořte pravidlo.
3. Přidejte hello pravidlo toohello vyrovnávání zátěže.
4. Aktualizujte nástroj pro vyrovnávání zatížení hello.

```powershell
# Get hello load balancer
$lb = Get-AzureRmLoadBalancer -Name LB-svcfab3 -ResourceGroupName svcfab_cli

# Create hello rule based on information from hello load balancer.
$lbrule = New-AzureRmLoadBalancerRuleConfig -Name my-app-rule7 -Protocol Tcp -FrontendPort 39990 -BackendPort 40009 `
                                            -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
                                            -BackendAddressPool  $lb.BackendAddressPools[0] `
                                            -Probe $lb.Probes[0]

# Add hello rule toohello load balancer
$lb.LoadBalancingRules.Add($lbrule)

# Update hello load balancer on Azure
$lb | Set-AzureRmLoadBalancer
```

O hello `New-AzureRmLoadBalancerRuleConfig` příkaz hello `-FrontendPort` představuje hello port hello nástroj pro vyrovnávání zatížení vystaví pro externí připojení a hello `-BackendPort` prostředků infrastruktury aplikace hello service představuje hello portu naslouchá.

>[!NOTE]
>Další informace o tom, jak toocreate Vyrovnávání zatížení v prostředí PowerShell najdete v části [vytvořit nástroj pro vyrovnávání zatížení v prostředí PowerShell](..\load-balancer\load-balancer-get-started-internet-arm-ps.md).

