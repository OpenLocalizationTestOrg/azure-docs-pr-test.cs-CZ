---
title: "Odstranit bránu virtuální sítě: portál Azure: Resource Manager | Microsoft Docs"
description: "Odstraňte bránu virtuální sítě pomocí portálu Azure v modelu nasazení Resource Manager."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: cherylmc
ms.openlocfilehash: 1d289c09465cb8d5e4bfa569441dffcbf562b3bf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="delete-a-virtual-network-gateway-using-the-portal"></a><span data-ttu-id="7c542-103">Odstranit bránu virtuální sítě pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="7c542-103">Delete a virtual network gateway using the portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7c542-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7c542-104">Azure portal</span></span>](vpn-gateway-delete-vnet-gateway-portal.md)
> * [<span data-ttu-id="7c542-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7c542-105">PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [<span data-ttu-id="7c542-106">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="7c542-106">PowerShell (classic)</span></span>](vpn-gateway-delete-vnet-gateway-classic-powershell.md)

<span data-ttu-id="7c542-107">Existuje několik způsobů, které můžete provést, pokud chcete odstranit bránu virtuální sítě pro konfiguraci brány VPN.</span><span class="sxs-lookup"><span data-stu-id="7c542-107">There are a couple of different approaches you can take when you want to delete a virtual network gateway for a VPN gateway configuration.</span></span>

- <span data-ttu-id="7c542-108">Pokud chcete odstranit vše a začít od začátku, jako v případě testovacím prostředí, můžete odstranit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c542-108">If you want to delete everything and start over, as in the case of a test environment, you can delete the resource group.</span></span> <span data-ttu-id="7c542-109">Pokud odstraníte skupinu prostředků, odstraní všechny prostředky ve skupině.</span><span class="sxs-lookup"><span data-stu-id="7c542-109">When you delete a resource group, it deletes all the resources within the group.</span></span> <span data-ttu-id="7c542-110">Toto je metoda se doporučuje jenom Pokud nechcete, aby zachovat prostředky ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c542-110">This is method is only recommended if you don't want to keep any of the resources in the resource group.</span></span> <span data-ttu-id="7c542-111">Nelze odstranit selektivně pouze několik prostředků pomocí tohoto přístupu.</span><span class="sxs-lookup"><span data-stu-id="7c542-111">You can't selectively delete only a few resources using this approach.</span></span>

- <span data-ttu-id="7c542-112">Pokud chcete zachovat některé prostředky ve vaší skupině prostředků, odstraňuje se Brána virtuální sítě je něco víc složité.</span><span class="sxs-lookup"><span data-stu-id="7c542-112">If you want to keep some of the resources in your resource group, deleting a virtual network gateway becomes slightly more complicated.</span></span> <span data-ttu-id="7c542-113">Než budete moct odstranit bránu virtuální sítě, musíte nejprve odstranit všechny prostředky, které jsou závislé na bráně.</span><span class="sxs-lookup"><span data-stu-id="7c542-113">Before you can delete the virtual network gateway, you must first delete any resources that are dependent on the gateway.</span></span> <span data-ttu-id="7c542-114">Kroky, které budete postupovat podle závisí na typu připojení, které jste vytvořili a závislé prostředky pro každé připojení.</span><span class="sxs-lookup"><span data-stu-id="7c542-114">The steps you follow depend on the type of connections that you created and the dependent resources for each connection.</span></span>

## <a name="delete-a-vpn-gateway"></a><span data-ttu-id="7c542-115">Odstranění brány VPN</span><span class="sxs-lookup"><span data-stu-id="7c542-115">Delete a VPN gateway</span></span>

<span data-ttu-id="7c542-116">Pokud chcete odstranit bránu virtuální sítě, musíte nejprve odstranit všechny prostředky, které se vztahují na bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="7c542-116">To delete a virtual network gateway, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="7c542-117">V určitém pořadí závislé položky musí dojít k odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c542-117">Resources must be deleted in a certain order due to dependencies.</span></span>

[!INCLUDE [delete gateway](../../includes/vpn-gateway-delete-vnet-gateway-portal-include.md)]

<span data-ttu-id="7c542-118">V tomto okamžiku je odstranit bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="7c542-118">At this point, the virtual network gateway is deleted.</span></span> <span data-ttu-id="7c542-119">Další kroky můžete odstranit všechny prostředky, které se již nepoužívají.</span><span class="sxs-lookup"><span data-stu-id="7c542-119">The next steps help you delete any resources that are no longer being used.</span></span>

### <a name="to-delete-the-local-network-gateway"></a><span data-ttu-id="7c542-120">Chcete-li odstranit bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="7c542-120">To delete the local network gateway</span></span>

1. <span data-ttu-id="7c542-121">V **všechny prostředky**, vyhledejte brány místní sítě, které byly přidruženy každé připojení.</span><span class="sxs-lookup"><span data-stu-id="7c542-121">In **All resources**, locate the local network gateways that were associated with each connection.</span></span>
2. <span data-ttu-id="7c542-122">Na **přehled** pro bránu místní sítě, klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="7c542-122">On the **Overview** blade for the local network gateway, click **Delete**.</span></span>

### <a name="to-delete-the-public-ip-address-resource-for-the-gateway"></a><span data-ttu-id="7c542-123">Odstranit prostředek veřejné IP adresy pro bránu</span><span class="sxs-lookup"><span data-stu-id="7c542-123">To delete the Public IP address resource for the gateway</span></span>

1. <span data-ttu-id="7c542-124">V **všechny prostředky**, vyhledejte prostředek veřejné IP adresy, který byl přidružený k bráně.</span><span class="sxs-lookup"><span data-stu-id="7c542-124">In **All resources**, locate the Public IP address resource that was associated to the gateway.</span></span> <span data-ttu-id="7c542-125">Pokud brána virtuální sítě byla aktivní aktivní, zobrazí se dvě veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="7c542-125">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span> 
2. <span data-ttu-id="7c542-126">Na **přehled** stránky pro veřejnou IP adresu, klikněte na tlačítko **odstranit**, pak **Ano** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="7c542-126">On the **Overview** page for the Public IP address, click **Delete**, then **Yes** to confirm.</span></span>

### <a name="to-delete-the-gateway-subnet"></a><span data-ttu-id="7c542-127">Chcete-li odstranit podsíť brány</span><span class="sxs-lookup"><span data-stu-id="7c542-127">To delete the gateway subnet</span></span>

1. <span data-ttu-id="7c542-128">V **všechny prostředky**, najít virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="7c542-128">In **All resources**, locate the virtual network.</span></span> 
2. <span data-ttu-id="7c542-129">Na **podsítě** okně klikněte na tlačítko **GatewaySubnet**, pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="7c542-129">On the **Subnets** blade, click the **GatewaySubnet**, then click **Delete**.</span></span> 
3. <span data-ttu-id="7c542-130">Klikněte na tlačítko **Ano** potvrďte, že chcete odstranit podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="7c542-130">Click **Yes** to confirm that you want to delete the gateway subnet.</span></span>

## <span data-ttu-id="7c542-131"><a name="deleterg"></a>Odstraňte bránu VPN odstraníte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="7c542-131"><a name="deleterg"></a>Delete a VPN gateway by deleting the resource group</span></span>

<span data-ttu-id="7c542-132">Pokud si nejste zajímá zachování některé z vašich prostředků ve skupině prostředků a chcete začít znovu, můžete odstranit celé skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c542-132">If you are not concerned about keeping any of your resources in the resource group and you just want to start over, you can delete an entire resource group.</span></span> <span data-ttu-id="7c542-133">To je rychlý způsob, jak odebrat vše.</span><span class="sxs-lookup"><span data-stu-id="7c542-133">This is a quick way to remove everything.</span></span> <span data-ttu-id="7c542-134">Následující postup se vztahuje pouze na modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7c542-134">The following steps apply only to the Resource Manager deployment model.</span></span>

1. <span data-ttu-id="7c542-135">V **všechny prostředky**, vyhledejte skupinu prostředků a klikněte na tlačítko otevřete okno.</span><span class="sxs-lookup"><span data-stu-id="7c542-135">In **All resources**, locate the resource group and click to open the blade.</span></span>
2. <span data-ttu-id="7c542-136">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="7c542-136">Click **Delete**.</span></span> <span data-ttu-id="7c542-137">V okně Odstranit zobrazení ovlivněných prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c542-137">On the Delete blade, view the affected resources.</span></span> <span data-ttu-id="7c542-138">Ujistěte se, že chcete odstranit všechny tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="7c542-138">Make sure that you want to delete all of these resources.</span></span> <span data-ttu-id="7c542-139">Pokud ne, postupujte podle kroků v [odstranit bránu VPN](#deletegw) v horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="7c542-139">If not, use the steps in [Delete a VPN gateway](#deletegw) at the top of this article.</span></span>
3. <span data-ttu-id="7c542-140">Chcete-li pokračovat, zadejte název skupiny prostředků, který chcete odstranit a pak klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="7c542-140">To proceed, type the name of the resource group that you want to delete, then click **Delete**.</span></span>