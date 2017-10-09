---
title: "Konfigurace brány virtuální sítě pro ExpressRoute pomocí prostředí PowerShell: classic: Azure | Microsoft Docs"
description: "Konfigurace brány virtuální sítě pro nasazení classic modelu virtuální sítě pomocí prostředí PowerShell konfigurace ExpressRoute."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 85ee0bc1-55be-4760-bfb4-34d9f2c96f30
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: 6f37d4d9cba546b5416ab99040f5ef6dae273380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>Konfigurace brány virtuální sítě pro ExpressRoute pomocí prostředí PowerShell (klasické)
> [!div class="op_single_selector"]
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Classic – prostředí PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Tento článek vás provede prostřednictvím hello tooadd kroky, přizpůsobit a odebrat bránu virtuální sítě (VNet) pro už existující virtuální síť. Hello kroky pro tuto konfiguraci jsou speciálně určené pro virtuální sítě vytvořené pomocí hello **modelu nasazení classic** a který bude možné použít v konfiguraci ExpressRoute. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Před zahájením
Ověřte, zda jste nainstalovali rutin prostředí Azure PowerShell hello potřebné pro tuto konfiguraci (1.0.2 nebo novější). Pokud jste nenainstalovali hello rutiny, budete potřebovat toodo Ano před zahájením hello kroky konfigurace. Další informace o instalaci prostředí Azure PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Další kroky
Po vytvoření brány virtuální sítě hello, můžete se propojit vaší virtuální sítě tooan okruh ExpressRoute. V tématu [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-classic.md).

