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
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a><span data-ttu-id="84c54-103">Konfigurace brány virtuální sítě pro ExpressRoute pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="84c54-103">Configure a virtual network gateway for ExpressRoute using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="84c54-104">Resource Manager – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="84c54-104">Resource Manager - Azure portal</span></span>](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [<span data-ttu-id="84c54-105">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="84c54-105">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="84c54-106">Classic – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="84c54-106">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="84c54-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="84c54-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="84c54-108">Tento článek vás provede kroky tooadd hello, přizpůsobit a odebrat bránu virtuální sítě (VNet) pro už existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="84c54-108">This article walks you through hello steps tooadd, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="84c54-109">Hello kroky pro tuto konfiguraci jsou speciálně určené pro virtuální sítě vytvořené pomocí modelu nasazení Resource Manager hello, který se použije v konfiguraci ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="84c54-109">hello steps for this configuration are specifically for VNets that were created using hello Resource Manager deployment model that will be used in an ExpressRoute configuration.</span></span> <span data-ttu-id="84c54-110">Další informace o brány virtuální sítě a nastavení konfigurace brány ExpressRoute najdete v tématu [o brány virtuální sítě pro ExpressRoute](expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="84c54-110">For more information about virtual network gateways and gateway configuration settings for ExpressRoute, see [About virtual network gateways for ExpressRoute](expressroute-about-virtual-network-gateways.md).</span></span> 


## <a name="before-beginning"></a><span data-ttu-id="84c54-111">Před zahájením</span><span class="sxs-lookup"><span data-stu-id="84c54-111">Before beginning</span></span>
<span data-ttu-id="84c54-112">Ověřte, zda jste nainstalovali hello nejnovějších rutin Azure Powershellu.</span><span class="sxs-lookup"><span data-stu-id="84c54-112">Verify that you have installed hello latest Azure PowerShell cmdlets.</span></span> <span data-ttu-id="84c54-113">Pokud jste nenainstalovali hello nejnovější rutiny, je třeba toodo Ano před zahájením hello kroky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="84c54-113">If you haven't installed hello latest cmdlets, you need toodo so before beginning hello configuration steps.</span></span> <span data-ttu-id="84c54-114">Další informace najdete v článku [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="84c54-114">For more information, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="84c54-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84c54-115">Next steps</span></span>
<span data-ttu-id="84c54-116">Po vytvoření brány virtuální sítě hello, můžete se propojit vaší virtuální sítě tooan okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="84c54-116">After you have created hello VNet gateway, you can link your VNet tooan ExpressRoute circuit.</span></span> <span data-ttu-id="84c54-117">V tématu [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="84c54-117">See [Link a Virtual Network tooan ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

