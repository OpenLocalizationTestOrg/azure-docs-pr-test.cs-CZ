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
ms.openlocfilehash: 195a38fa45f1c514a93980e777fb0d8238aa3f3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a><span data-ttu-id="38157-103">Konfigurace brány virtuální sítě pro ExpressRoute pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="38157-103">Configure a virtual network gateway for ExpressRoute using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="38157-104">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="38157-104">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="38157-105">Classic – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="38157-105">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="38157-106">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="38157-106">Video - Azure Portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="38157-107">Tento článek vás provede kroky k přidání, změně velikosti a odebrání brány virtuální sítě (VNet) pro už existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="38157-107">This article will walk you through the steps to add, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="38157-108">Kroky pro tuto konfiguraci jsou speciálně určené pro virtuální sítě vytvořené pomocí **modelu nasazení classic** a který bude použít v konfiguraci ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="38157-108">The steps for this configuration are specifically for VNets that were created using the **classic deployment model** and that will be be used in an ExpressRoute configuration.</span></span> 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="38157-109">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="38157-109">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a><span data-ttu-id="38157-110">Před zahájením</span><span class="sxs-lookup"><span data-stu-id="38157-110">Before beginning</span></span>
<span data-ttu-id="38157-111">Ověřte, zda jste nainstalovali rutin prostředí Azure PowerShell, které jsou potřebné pro tuto konfiguraci (1.0.2 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="38157-111">Verify that you have installed the Azure PowerShell cmdlets needed for this configuration (1.0.2 or later).</span></span> <span data-ttu-id="38157-112">Pokud jste nenainstalovali rutiny, budete muset udělat před zahájením kroky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="38157-112">If you haven't installed the cmdlets, you'll need to do so before beginning the configuration steps.</span></span> <span data-ttu-id="38157-113">Další informace o instalaci prostředí Azure PowerShell najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="38157-113">For more information about installing Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="38157-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38157-114">Next steps</span></span>
<span data-ttu-id="38157-115">Po vytvoření brány virtuální sítě, můžete se propojit virtuální sítě k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="38157-115">After you have created the VNet gateway, you can link your VNet to an ExpressRoute circuit.</span></span> <span data-ttu-id="38157-116">V tématu [propojení virtuální sítě k okruhu ExpressRoute](expressroute-howto-linkvnet-classic.md).</span><span class="sxs-lookup"><span data-stu-id="38157-116">See [Link a Virtual Network to an ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).</span></span>

