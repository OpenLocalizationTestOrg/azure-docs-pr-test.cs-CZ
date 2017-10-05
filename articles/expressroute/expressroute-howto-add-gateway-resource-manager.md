---
title: "Přidat bránu virtuální sítě k virtuální síti pro ExpressRoute: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento článek vás provede procesem přidání brány virtuální sítě k již vytvořené virtuální sítě Resource Manageru pro ExpressRoute."
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
ms.openlocfilehash: 3aeddd03e0be548933775164ae790ba208fc13ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a><span data-ttu-id="842ef-103">Konfigurace brány virtuální sítě pro ExpressRoute pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="842ef-103">Configure a virtual network gateway for ExpressRoute using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="842ef-104">Resource Manager – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="842ef-104">Resource Manager - Azure portal</span></span>](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [<span data-ttu-id="842ef-105">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="842ef-105">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="842ef-106">Classic – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="842ef-106">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="842ef-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="842ef-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="842ef-108">Tento článek vás provede kroky k přidání, změně velikosti a odebrání brány virtuální sítě (VNet) pro už existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="842ef-108">This article walks you through the steps to add, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="842ef-109">Kroky pro tuto konfiguraci jsou speciálně určené pro virtuální sítě vytvořené pomocí modelu nasazení Resource Manager, který se použije v konfiguraci ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="842ef-109">The steps for this configuration are specifically for VNets that were created using the Resource Manager deployment model that will be used in an ExpressRoute configuration.</span></span> <span data-ttu-id="842ef-110">Další informace o brány virtuální sítě a nastavení konfigurace brány ExpressRoute najdete v tématu [o brány virtuální sítě pro ExpressRoute](expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="842ef-110">For more information about virtual network gateways and gateway configuration settings for ExpressRoute, see [About virtual network gateways for ExpressRoute](expressroute-about-virtual-network-gateways.md).</span></span> 


## <a name="before-beginning"></a><span data-ttu-id="842ef-111">Před zahájením</span><span class="sxs-lookup"><span data-stu-id="842ef-111">Before beginning</span></span>
<span data-ttu-id="842ef-112">Ověřte, že jste nainstalovali nejnovějších rutin Azure Powershellu.</span><span class="sxs-lookup"><span data-stu-id="842ef-112">Verify that you have installed the latest Azure PowerShell cmdlets.</span></span> <span data-ttu-id="842ef-113">Pokud jste nenainstalovali nejnovějších rutin, budete muset udělat před zahájením kroky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="842ef-113">If you haven't installed the latest cmdlets, you need to do so before beginning the configuration steps.</span></span> <span data-ttu-id="842ef-114">Další informace najdete v článku [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="842ef-114">For more information, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="842ef-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="842ef-115">Next steps</span></span>
<span data-ttu-id="842ef-116">Po vytvoření brány virtuální sítě, můžete se propojit virtuální sítě k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="842ef-116">After you have created the VNet gateway, you can link your VNet to an ExpressRoute circuit.</span></span> <span data-ttu-id="842ef-117">V tématu [propojení virtuální sítě k okruhu ExpressRoute](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="842ef-117">See [Link a Virtual Network to an ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

