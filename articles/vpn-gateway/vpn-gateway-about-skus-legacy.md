---
title: "SKU brány virtuální sítě Azure aaaLegacy | Microsoft Docs"
description: "Starý virtuální sítě SKU brány."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 710417581423d2fbc62827cab7949f2e137c5996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Práce s SKU (starší verze SKU) brány virtuální sítě

Tento článek obsahuje informace o hello starší verze (starý) virtuální sítě SKU brány. starší verze Hello SKU i nadále fungovat v obou modelech nasazení pro brány sítě VPN, které již byly vytvořeny. Classic brány VPN pokračovat toouse hello starší verze SKU pro existující brány i pro nové brány. Při vytváření nového správce prostředků VPN Gateway, použijte nové SKU brány hello. Informace o hello nové SKU, najdete v části [o službě VPN Gateway](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>SKU brány

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>Odhadovaná agregovaná propustnost podle SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Podporované konfigurace podle typu SKU a síť VPN

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Změnit velikost brány (změnit a skladová položka brány)

Můžete změnit velikost skladová položka brány v rámci hello stejná rodina SKU. Například pokud máte standardní SKU, můžete změnit velikost tooa HighPerformance SKU. Nelze změnit velikost VPN mezi hello staré SKU a hello nové rodiny SKU brány. Například nelze přejít ze standardní SKU tooa VpnGw2 SKU. 

tooresize skladová položka brány pro model nasazení classic hello hello použijte následující příkaz:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

tooresize skladová položka brány pro hello modelu nasazení Resource Manager, použijte následující příkaz hello:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>Migrace nové SKU brány toohello

Pokud pracujete s modelem nasazení Resource Manager hello, můžete migrovat toohello SKU pro nové brány. Pokud pracujete s modelem nasazení classic hello, nemůžete migrovat, toohello nové SKU a místo toho musí pokračovat toouse hello starší verze SKU.

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>Další kroky

Další informace o hello nové SKU brány najdete v tématu [SKU brány](vpn-gateway-about-vpngateways.md#gwsku).

Další informace o nastavení konfigurace najdete v tématu [nastavení konfigurace o službě VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).