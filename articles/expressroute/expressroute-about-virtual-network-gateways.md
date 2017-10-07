---
title: "brány virtuální sítě aaaAbout ExpressRoute | Microsoft Docs"
description: "Další informace o brány virtuální sítě pro ExpressRoute."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 7e0d9658-bc00-45b0-848f-f7a6da648635
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: cherylmc
ms.openlocfilehash: 4daf4f96b4fadb00683d8e536e51734853008c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>O branách virtuálních sítí pro ExpressRoute
Brána virtuální sítě se používá toosend síťový provoz mezi virtuálními sítěmi Azure a místní umístění. Při konfiguraci připojení ExpressRoute, musíte vytvořit a konfigurovat bránu virtuální sítě a připojení brány virtuální sítě.

Při vytváření brány virtuální sítě je třeba zadat několik nastavení. Jedno z hello požadované nastavení určuje, zda hello brány se použije pro provoz ExpressRoute nebo VPN typu Site-to-Site. V modelu nasazení Resource Manager hello nastavení hello je '-GatewayType'.

Odeslání síťový provoz na privátní připojení používáte typ brány hello 'ExpressRoute'. Toto je také odkazované tooas bránu ExpressRoute. Při odeslání síťový provoz mezi šifrované hello veřejného Internetu, použijte typ brány hello 'Vpn'. Toto je odkazované tooas brána sítě VPN. Připojení typu Site-to-Site, Point-to-Site a VNet-to-VNet používají bránu VPN.

Každá virtuální síť může mít pouze jednu bránu virtuální sítě pro každý typ brány. Například můžete mít jednu bránu virtuální sítě, která má typ -GatewayType Vpn a jednu, který má typ -GatewayType ExpressRoute. Tento článek se zaměřuje na bránu virtuální sítě hello ExpressRoute.

## <a name="gwsku"></a>SKU brány
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Pokud chcete tooupgrade vaší brány tooa výkonnější hello brány SKU, ve většině případů, které můžete použít rutiny prostředí PowerShell, změny velikosti-AzureRmVirtualNetworkGateway'. To bude fungovat pro upgrady tooStandard a HighPerformance SKU. Ale tooupgrade toohello UltraPerformance SKU, budete potřebovat toorecreate hello brány.

### <a name="aggthroughput"></a>Odhadovaná agregovaná propustnost podle skladová položka brány
Hello následující tabulka uvádí typy hello brány a odhadovanou agregovanou propustnost hello. Tato tabulka platí tooboth hello Resource Manager a modely nasazení classic.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Propustnost aplikace závisí na několika faktorech, například hello začátku do konce latence, a hello počet přenosů toků otevře aplikace hello. Hello čísla v hello tabulce představují hello horní limit, můžete aplikace hello theorectically dosáhnout v prostředí ideální. 
> 
>

## <a name="resources"></a>Rutiny prostředí PowerShell a rozhraní REST API
Další zdroje technických informací a specifickou syntaxi požadavky při použití rozhraní REST API a rutin prostředí PowerShell pro konfiguraci brány virtuální sítě najdete v tématu hello následující stránky:

| **Classic** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://msdn.microsoft.com/library/mt270335.aspx) |[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Další kroky
V tématu [přehled ExpressRoute](expressroute-introduction.md) pro další informace o konfiguracích dostupné připojení. 

