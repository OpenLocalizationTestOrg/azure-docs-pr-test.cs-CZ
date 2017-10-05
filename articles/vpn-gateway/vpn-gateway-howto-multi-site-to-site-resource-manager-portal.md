---
title: "Přidání více připojení brány Site-to-Site VPN do virtuální sítě: portálu Azure: Resource Manager | Microsoft Docs"
description: "Přidat připojení S2S více lokalit pro bránu VPN, který má existující připojení"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: cherylmc
ms.openlocfilehash: 7ec57789ee76f4ec54e4f7b68ea75c19522f3d7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a><span data-ttu-id="eeab1-103">Přidat připojení Site-to-Site k virtuální síti pomocí existujícího připojení brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="eeab1-103">Add a Site-to-Site connection to a VNet with an existing VPN gateway connection</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="eeab1-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="eeab1-104">Azure portal</span></span>](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="eeab1-105">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="eeab1-105">PowerShell (classic)</span></span>](vpn-gateway-multi-site.md)
>
> 

<span data-ttu-id="eeab1-106">Tento článek vás provede pomocí portálu Azure přidat připojení Site-to-Site (S2S) pro bránu VPN, který má existující připojení.</span><span class="sxs-lookup"><span data-stu-id="eeab1-106">This article walks you through using the Azure portal to add Site-to-Site (S2S) connections to a VPN gateway that has an existing connection.</span></span> <span data-ttu-id="eeab1-107">Tento typ připojení se často označuje jako "s více servery" konfigurace.</span><span class="sxs-lookup"><span data-stu-id="eeab1-107">This type of connection is often referred to as a "multi-site" configuration.</span></span> <span data-ttu-id="eeab1-108">Můžete přidat připojení S2S k virtuální síti, který už má připojení S2S, připojení Point-to-Site nebo připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="eeab1-108">You can add a S2S connection to a VNet that already has a S2S connection, Point-to-Site connection, or VNet-to-VNet connection.</span></span> <span data-ttu-id="eeab1-109">Při přidávání připojení existují určitá omezení.</span><span class="sxs-lookup"><span data-stu-id="eeab1-109">There are some limitations when adding connections.</span></span> <span data-ttu-id="eeab1-110">Zkontrolujte [před zahájením](#before) v tomto článku k ověření před zahájením konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="eeab1-110">Check the [Before you begin](#before) section in this article to verify before you start your configuration.</span></span> 

<span data-ttu-id="eeab1-111">Tento článek se týká virtuálních sítí vytvořených pomocí modelu nasazení Resource Manager, které mají brána sítě VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="eeab1-111">This article applies to VNets created using the Resource Manager deployment model that have a RouteBased VPN gateway.</span></span> <span data-ttu-id="eeab1-112">Tyto kroky se nevztahují na konfigurace koexistujících připojení ExpressRoute nebo Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="eeab1-112">These steps do not apply to ExpressRoute/Site-to-Site coexisting connection configurations.</span></span> <span data-ttu-id="eeab1-113">V tématu [koexistujících připojení ExpressRoute nebo S2S](../expressroute/expressroute-howto-coexist-resource-manager.md) informace o připojení, která.</span><span class="sxs-lookup"><span data-stu-id="eeab1-113">See [ExpressRoute/S2S coexisting connections](../expressroute/expressroute-howto-coexist-resource-manager.md) for information about coexisting connections.</span></span>

### <a name="deployment-models-and-methods"></a><span data-ttu-id="eeab1-114">Modely a metody nasazení</span><span class="sxs-lookup"><span data-stu-id="eeab1-114">Deployment models and methods</span></span>
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="eeab1-115">Tuto tabulku aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="eeab1-115">We update this table as new articles and additional tools become available for this configuration.</span></span> <span data-ttu-id="eeab1-116">Když je článek k dispozici, jsme přímý odkaz na něj z této tabulky.</span><span class="sxs-lookup"><span data-stu-id="eeab1-116">When an article is available, we link directly to it from this table.</span></span>

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <span data-ttu-id="eeab1-117"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="eeab1-117"><a name="before"></a>Before you begin</span></span>
<span data-ttu-id="eeab1-118">Zkontrolujte následující položky:</span><span class="sxs-lookup"><span data-stu-id="eeab1-118">Verify the following items:</span></span>

* <span data-ttu-id="eeab1-119">Nejsou vytvoření koexistujících připojení ExpressRoute nebo S2S.</span><span class="sxs-lookup"><span data-stu-id="eeab1-119">You are not creating an ExpressRoute/S2S coexisting connection.</span></span>
* <span data-ttu-id="eeab1-120">Máte virtuální síť, která byla vytvořena pomocí modelu nasazení Resource Manager stávající připojení.</span><span class="sxs-lookup"><span data-stu-id="eeab1-120">You have a virtual network that was created using the Resource Manager deployment model with an existing connection.</span></span>
* <span data-ttu-id="eeab1-121">Brána virtuální sítě pro virtuální síť je RouteBased.</span><span class="sxs-lookup"><span data-stu-id="eeab1-121">The virtual network gateway for your VNet is RouteBased.</span></span> <span data-ttu-id="eeab1-122">Pokud máte bránu PolicyBased VPN, musíte odstranit bránu virtuální sítě a vytvořit novou bránu VPN jako RouteBased.</span><span class="sxs-lookup"><span data-stu-id="eeab1-122">If you have a PolicyBased VPN gateway, you must delete the virtual network gateway and create a new VPN gateway as RouteBased.</span></span>
* <span data-ttu-id="eeab1-123">Překrývat žádné z rozsahů adres pro všechny virtuální sítě, který tuto virtuální síť se připojuje k.</span><span class="sxs-lookup"><span data-stu-id="eeab1-123">None of the address ranges overlap for any of the VNets that this VNet is connecting to.</span></span>
* <span data-ttu-id="eeab1-124">Máte kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="eeab1-124">You have compatible VPN device and someone who is able to configure it.</span></span> <span data-ttu-id="eeab1-125">Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="eeab1-125">See [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span> <span data-ttu-id="eeab1-126">Pokud nevíte, jak nakonfigurovat zařízení VPN, nebo neznáte rozsahy IP adres v konfiguraci vaší místní sítě, budete se muset spojit s někým, kdo vám s tím pomůže.</span><span class="sxs-lookup"><span data-stu-id="eeab1-126">If you aren't familiar with configuring your VPN device, or are unfamiliar with the IP address ranges located in your on-premises network configuration, you need to coordinate with someone who can provide those details for you.</span></span>
* <span data-ttu-id="eeab1-127">Máte zvenčí veřejnou IP adresu pro vaše zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="eeab1-127">You have an externally facing public IP address for your VPN device.</span></span> <span data-ttu-id="eeab1-128">Tato IP adresa nesmí být umístěná za překladem adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="eeab1-128">This IP address cannot be located behind a NAT.</span></span>

## <span data-ttu-id="eeab1-129"><a name="part1"></a>Část 1: Konfigurace připojení</span><span class="sxs-lookup"><span data-stu-id="eeab1-129"><a name="part1"></a>Part 1 - Configure a connection</span></span>
1. <span data-ttu-id="eeab1-130">V prohlížeči přejděte na portál [Azure Portal](http://portal.azure.com) a v případě potřeby se přihlaste pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="eeab1-130">From a browser, navigate to the [Azure portal](http://portal.azure.com) and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="eeab1-131">Klikněte na tlačítko **všechny prostředky** a vyhledejte vaše **brány virtuální sítě** ze seznamu prostředků a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="eeab1-131">Click **All resources** and locate your **virtual network gateway** from the list of resources and click it.</span></span>
3. <span data-ttu-id="eeab1-132">Na **Brána virtuální sítě** okně klikněte na tlačítko **připojení**.</span><span class="sxs-lookup"><span data-stu-id="eeab1-132">On the **Virtual network gateway** blade, click **Connections**.</span></span>
   
    <span data-ttu-id="eeab1-133">![Okno připojení](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")</span><span class="sxs-lookup"><span data-stu-id="eeab1-133">![Connections blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")</span></span><br>
4. <span data-ttu-id="eeab1-134">Na **připojení** okně klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="eeab1-134">On the **Connections** blade, click **+Add**.</span></span>
   
    <span data-ttu-id="eeab1-135">![Přidat tlačítko připojení](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")</span><span class="sxs-lookup"><span data-stu-id="eeab1-135">![Add connection button](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")</span></span><br>
5. <span data-ttu-id="eeab1-136">Na **přidat připojení** okno, výplň na následující pole:</span><span class="sxs-lookup"><span data-stu-id="eeab1-136">On the **Add connection** blade, fill out the following fields:</span></span>
   
   * <span data-ttu-id="eeab1-137">**Název:** název, kterou chcete přiřadit k lokalitě při vytváření připojení k.</span><span class="sxs-lookup"><span data-stu-id="eeab1-137">**Name:** The name you want to give to the site you are creating the connection to.</span></span>
   * <span data-ttu-id="eeab1-138">**Typ připojení:** vyberte **Site-to-site (IPsec)**.</span><span class="sxs-lookup"><span data-stu-id="eeab1-138">**Connection type:** Select **Site-to-site (IPsec)**.</span></span>
     
     <span data-ttu-id="eeab1-139">![Přidat připojení okno](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")</span><span class="sxs-lookup"><span data-stu-id="eeab1-139">![Add connection blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")</span></span><br>

## <span data-ttu-id="eeab1-140"><a name="part2"></a>Část 2 – přidání brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="eeab1-140"><a name="part2"></a>Part 2 - Add a local network gateway</span></span>
1. <span data-ttu-id="eeab1-141">Klikněte na tlačítko **brány místní sítě** ***zvolit bránu místní sítě***.</span><span class="sxs-lookup"><span data-stu-id="eeab1-141">Click **Local network gateway** ***Choose a local network gateway***.</span></span> <span data-ttu-id="eeab1-142">Otevře se **brány místní sítě vyberte** okno.</span><span class="sxs-lookup"><span data-stu-id="eeab1-142">This will open the **Choose local network gateway** blade.</span></span>
   
    <span data-ttu-id="eeab1-143">![Zvolit bránu místní sítě](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")</span><span class="sxs-lookup"><span data-stu-id="eeab1-143">![Choose local network gateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")</span></span><br>
2. <span data-ttu-id="eeab1-144">Klikněte na tlačítko **vytvořit nový** otevřete **vytvořit bránu místní sítě** okno.</span><span class="sxs-lookup"><span data-stu-id="eeab1-144">Click **Create new** to open the **Create local network gateway** blade.</span></span>
   
    <span data-ttu-id="eeab1-145">![Vytvoření okna brány místní sítě](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")</span><span class="sxs-lookup"><span data-stu-id="eeab1-145">![Create local network gateway blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")</span></span><br>
3. <span data-ttu-id="eeab1-146">Na **vytvořit bránu místní sítě** okno, výplň na následující pole:</span><span class="sxs-lookup"><span data-stu-id="eeab1-146">On the **Create local network gateway** blade, fill out the following fields:</span></span>
   
   * <span data-ttu-id="eeab1-147">**Název:** název chcete poskytnout prostředku brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="eeab1-147">**Name:** The name you want to give to the local network gateway resource.</span></span>
   * <span data-ttu-id="eeab1-148">**IP adresa:** veřejnou IP adresu v lokalitě, kterou chcete připojit k zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="eeab1-148">**IP address:** The public IP address of the VPN device on the site that you want to connect to.</span></span>
   * <span data-ttu-id="eeab1-149">**Adresní prostor:** adresní prostor, který chcete směrovat do nové lokality místní sítě.</span><span class="sxs-lookup"><span data-stu-id="eeab1-149">**Address space:** The address space that you want to be routed to the new local network site.</span></span>
4. <span data-ttu-id="eeab1-150">Klikněte na tlačítko **OK** na **vytvořit bránu místní sítě** okno a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="eeab1-150">Click **OK** on the **Create local network gateway** blade to save the changes.</span></span>

## <span data-ttu-id="eeab1-151"><a name="part3"></a>Část 3 – přidat sdílený klíč a vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="eeab1-151"><a name="part3"></a>Part 3 - Add the shared key and create the connection</span></span>
1. <span data-ttu-id="eeab1-152">Na **přidat připojení** okně Přidat sdílený klíč, který chcete použít k vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="eeab1-152">On the **Add connection** blade, add the shared key that you want to use to create your connection.</span></span> <span data-ttu-id="eeab1-153">Můžete získat sdílený klíč z vašeho zařízení VPN, nebo si ho vytvořit tady a pak nakonfigurovat zařízení VPN použít stejný sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="eeab1-153">You can either get the shared key from your VPN device, or make one up here and then configure your VPN device to use the same shared key.</span></span> <span data-ttu-id="eeab1-154">Důležité je, že klíče se přesně shodují.</span><span class="sxs-lookup"><span data-stu-id="eeab1-154">The important thing is that the keys are exactly the same.</span></span>
   
    <span data-ttu-id="eeab1-155">![Sdílený klíč](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")</span><span class="sxs-lookup"><span data-stu-id="eeab1-155">![Shared key](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")</span></span><br>
2. <span data-ttu-id="eeab1-156">V dolní části okna klikněte na tlačítko **OK** k vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="eeab1-156">At the bottom of the blade, click **OK** to create the connection.</span></span>

## <span data-ttu-id="eeab1-157"><a name="part4"></a>Součástí 4 – ověření připojení k síti VPN</span><span class="sxs-lookup"><span data-stu-id="eeab1-157"><a name="part4"></a>Part 4 - Verify the VPN connection</span></span>


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="eeab1-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eeab1-158">Next steps</span></span>

<span data-ttu-id="eeab1-159">Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="eeab1-159">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="eeab1-160">Další informace najdete v [naučné stezce](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="eeab1-160">See the virtual machines [learning path](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) for more information.</span></span>
