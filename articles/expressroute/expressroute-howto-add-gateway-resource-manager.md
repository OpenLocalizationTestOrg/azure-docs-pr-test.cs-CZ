---
title: "Přidání tooa brány virtuální sítě VNet pro ExpressRoute: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento článek vás provede přidáním tooan brány virtuální sítě už vytvořený virtuální sítě Resource Manageru pro ExpressRoute."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 63e0bd60-abad-4963-8e27-3aa973e0d968
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: charwen
ms.openlocfilehash: 8983430b426ad7c4af766294fa16427c5e9df5c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>Konfigurace brány virtuální sítě pro ExpressRoute pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Classic – prostředí PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Tento článek vás provede kroky tooadd hello, přizpůsobit a odebrat bránu virtuální sítě (VNet) pro už existující virtuální síť. Hello kroky pro tuto konfiguraci jsou speciálně určené pro virtuální sítě vytvořené pomocí modelu nasazení Resource Manager hello, který se použije v konfiguraci ExpressRoute. Další informace o brány virtuální sítě a nastavení konfigurace brány ExpressRoute najdete v tématu [o brány virtuální sítě pro ExpressRoute](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Před zahájením
Ověřte, zda jste nainstalovali hello nejnovějších rutin Azure Powershellu. Pokud jste nenainstalovali hello nejnovější rutiny, je třeba toodo Ano před zahájením hello kroky konfigurace. Další informace najdete v článku [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Další kroky
Po vytvoření brány virtuální sítě hello, můžete se propojit vaší virtuální sítě tooan okruh ExpressRoute. V tématu [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-arm.md).

