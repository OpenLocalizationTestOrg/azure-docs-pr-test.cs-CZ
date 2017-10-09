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
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a><span data-ttu-id="9447a-103">Konfigurace brány virtuální sítě pro ExpressRoute pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="9447a-103">Configure a virtual network gateway for ExpressRoute using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9447a-104">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="9447a-104">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="9447a-105">Classic – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9447a-105">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="9447a-106">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="9447a-106">Video - Azure Portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="9447a-107">Tento článek vás provede prostřednictvím hello tooadd kroky, přizpůsobit a odebrat bránu virtuální sítě (VNet) pro už existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="9447a-107">This article will walk you through hello steps tooadd, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="9447a-108">Hello kroky pro tuto konfiguraci jsou speciálně určené pro virtuální sítě vytvořené pomocí hello **modelu nasazení classic** a který bude možné použít v konfiguraci ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9447a-108">hello steps for this configuration are specifically for VNets that were created using hello **classic deployment model** and that will be be used in an ExpressRoute configuration.</span></span> 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="9447a-109">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="9447a-109">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a><span data-ttu-id="9447a-110">Před zahájením</span><span class="sxs-lookup"><span data-stu-id="9447a-110">Before beginning</span></span>
<span data-ttu-id="9447a-111">Ověřte, zda jste nainstalovali rutin prostředí Azure PowerShell hello potřebné pro tuto konfiguraci (1.0.2 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="9447a-111">Verify that you have installed hello Azure PowerShell cmdlets needed for this configuration (1.0.2 or later).</span></span> <span data-ttu-id="9447a-112">Pokud jste nenainstalovali hello rutiny, budete potřebovat toodo Ano před zahájením hello kroky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9447a-112">If you haven't installed hello cmdlets, you'll need toodo so before beginning hello configuration steps.</span></span> <span data-ttu-id="9447a-113">Další informace o instalaci prostředí Azure PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9447a-113">For more information about installing Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="9447a-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9447a-114">Next steps</span></span>
<span data-ttu-id="9447a-115">Po vytvoření brány virtuální sítě hello, můžete se propojit vaší virtuální sítě tooan okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9447a-115">After you have created hello VNet gateway, you can link your VNet tooan ExpressRoute circuit.</span></span> <span data-ttu-id="9447a-116">V tématu [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-classic.md).</span><span class="sxs-lookup"><span data-stu-id="9447a-116">See [Link a Virtual Network tooan ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).</span></span>

