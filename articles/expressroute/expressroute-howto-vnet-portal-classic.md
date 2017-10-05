---
title: "Konfigurace virtuální sítě a brány na portálu classic pro ExpressRoute | Microsoft Docs"
description: "Tento článek vás provede procesem nastavení virtuální sítě pro ExpressRoute pomocí modelu nasazení classic a portálu classic."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 688817d9-51c8-4372-9af8-33fa098c7c5a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2016
ms.author: cherylmc
ms.openlocfilehash: f62254b2a7df50aa55a2a49009702848a9aecebd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a><span data-ttu-id="3c36e-103">Vytvoření virtuální sítě pro ExpressRoute na portálu classic</span><span class="sxs-lookup"><span data-stu-id="3c36e-103">Create a Virtual Network for ExpressRoute in the classic portal</span></span>
<span data-ttu-id="3c36e-104">Kroky v tomto článku vás provede konfiguraci virtuální sítě a brány virtuální sítě pro použití s ExpressRoute pomocí modelu nasazení classic a portálu classic.</span><span class="sxs-lookup"><span data-stu-id="3c36e-104">The steps in this article will walk you through configuring a virtual network and a virtual network gateway for use with ExpressRoute using the classic deployment model and the classic portal.</span></span>

<span data-ttu-id="3c36e-105">Pokud hledáte pokyny k modelu nasazení Resource Manager, můžete použít v následujících článcích: [vytvoření virtuální sítě pomocí prostředí PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) a [přidat bránu VPN do virtuální sítě Resource Manageru pro ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="3c36e-105">If you are looking instructions for the Resource Manager deployment model, you can use the following articles: [Create a virtual network by using PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) and [Add a VPN Gateway to a Resource Manager VNet for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="3c36e-106">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="3c36e-106">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="create-a-classic-vnet-and-gateway"></a><span data-ttu-id="3c36e-107">Vytvoření klasické virtuální sítě a brány</span><span class="sxs-lookup"><span data-stu-id="3c36e-107">Create a classic VNet and gateway</span></span>
<span data-ttu-id="3c36e-108">Následující postup vytvoření klasické virtuální sítě a brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3c36e-108">The following steps create a classic VNet and a virtual network gateway.</span></span> <span data-ttu-id="3c36e-109">Pokud již máte klasické virtuální síti, najdete v článku [nakonfigurovat existující klasické virtuální síti](#config) v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="3c36e-109">If you already have a classic VNet, see the [Configure an existing classic VNet](#config) section in this article.</span></span>

1. <span data-ttu-id="3c36e-110">Přihlaste se do [portálu Azure Classic](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="3c36e-110">Log in to the [Azure classic portal](http://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="3c36e-111">V levém dolním rohu obrazovky klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="3c36e-111">In the lower left-hand corner of the screen, click **New**.</span></span> <span data-ttu-id="3c36e-112">V podokně navigace klikněte na **Network Services**, a poté klikněte na **Virtual Network**.</span><span class="sxs-lookup"><span data-stu-id="3c36e-112">In the navigation pane, click **Network Services**, and then click **Virtual Network**.</span></span> <span data-ttu-id="3c36e-113">Kliknutím na **Vytvořit vlastní** spustíte průvodce konfigurací.</span><span class="sxs-lookup"><span data-stu-id="3c36e-113">Click **Custom Create** to begin the configuration wizard.</span></span>
3. <span data-ttu-id="3c36e-114">Na **podrobnosti virtuální sítě** stránky, zadejte následující:</span><span class="sxs-lookup"><span data-stu-id="3c36e-114">On the **Virtual Network Details** page, enter the following:</span></span>
   
   * <span data-ttu-id="3c36e-115">**Název** – název virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3c36e-115">**Name** – Name your virtual network.</span></span> <span data-ttu-id="3c36e-116">Tento název virtuální sítě budete používat při nasazení vaší virtuálních počítačů a instancí PaaS, proto nemusí chcete nenastavujte příliš komplikovaný.</span><span class="sxs-lookup"><span data-stu-id="3c36e-116">You’ll use this virtual network name when you deploy your VMs and PaaS instances, so you may not want to make the name too complicated.</span></span>
   * <span data-ttu-id="3c36e-117">**Umístění** – umístění přímo souvisí s fyzickým umístěním (oblastí), kde chcete prostředky (virtuální počítače) jsou umístěny.</span><span class="sxs-lookup"><span data-stu-id="3c36e-117">**Location** – The location is directly related to the physical location (region) where you want your resources (VMs) to reside.</span></span> <span data-ttu-id="3c36e-118">Například pokud chcete, aby virtuální počítače, které do této virtuální sítě nasadíte, byly fyzicky umístěné ve východní USA, vyberte toto umístění.</span><span class="sxs-lookup"><span data-stu-id="3c36e-118">For example, if you want the VMs that you deploy to this virtual network to be physically located in East US, select that location.</span></span> <span data-ttu-id="3c36e-119">Oblast přidruženou k virtuální síti po jejím vytvoření nedá změnit.</span><span class="sxs-lookup"><span data-stu-id="3c36e-119">You can’t change the region associated with your virtual network after you create it.</span></span>
4. <span data-ttu-id="3c36e-120">Na stránce **Servery DNS a připojení VPN** zadejte následující informace, a poté klikněte na šipku Další vpravo dole.</span><span class="sxs-lookup"><span data-stu-id="3c36e-120">On the **DNS Servers and VPN Connectivity** page, enter the following information, and then click the next arrow on the lower right.</span></span> 
   
   * <span data-ttu-id="3c36e-121">**Servery DNS** – zadejte název serveru DNS a IP adresu, nebo vyberte dříve zaregistrovaný server DNS z místní nabídky.</span><span class="sxs-lookup"><span data-stu-id="3c36e-121">**DNS Servers** - Enter the DNS server name and IP address, or select a previously registered DNS server from the shortcut menu.</span></span> <span data-ttu-id="3c36e-122">Toto nastavení neslouží k vytvoření serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="3c36e-122">This setting does not create a DNS server.</span></span> <span data-ttu-id="3c36e-123">Umožňuje určit server DNS, který chcete použít pro překlad názvů pro tuto virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="3c36e-123">It allows you to specify the DNS servers that you want to use for name resolution for this virtual network.</span></span>
   * <span data-ttu-id="3c36e-124">**Připojení Site-to-Site** – zaškrtávací políčko pro **konfigurace VPN typu site-to-site**.</span><span class="sxs-lookup"><span data-stu-id="3c36e-124">**Site-to-Site Connectivity** - Select the checkbox for **Configure a site-to-site VPN**.</span></span>
   * <span data-ttu-id="3c36e-125">**ExpressRoute** – zaškrtávací políčko **pomocí ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="3c36e-125">**ExpressRoute** – Select the checkbox **Use ExpressRoute**.</span></span> <span data-ttu-id="3c36e-126">Tato možnost se zobrazí, pouze pokud jste vybrali **konfigurace VPN typu Site-to-Site**.</span><span class="sxs-lookup"><span data-stu-id="3c36e-126">This option only appears if you selected **Configure a Site-to-Site VPN**.</span></span>
   * <span data-ttu-id="3c36e-127">**Místní sítě** -je nutné mít místní síťový web pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="3c36e-127">**Local Network** - You are required to have a local network site for ExpressRoute.</span></span> <span data-ttu-id="3c36e-128">Ale v případě připojení ExpressRoute, předpony zadané pro místní síťový web, bude ignorována.</span><span class="sxs-lookup"><span data-stu-id="3c36e-128">However, in the case of an ExpressRoute connection, the address prefixes specified for the local network site will be ignored.</span></span> <span data-ttu-id="3c36e-129">Místo toho se použije pro účely směrování předpony inzerované Microsoftu prostřednictvím okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="3c36e-129">Instead, the address prefixes advertised to Microsoft through the ExpressRoute circuit will be used for routing purposes.</span></span><BR><span data-ttu-id="3c36e-130">Pokud již máte místní síť vytvořenou pro připojení ExpressRoute, můžete ji vyberte z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3c36e-130">If you already have a local network created for your ExpressRoute connection, you can select it from the dropdown.</span></span> <span data-ttu-id="3c36e-131">Pokud ne, vyberte **zadejte novou místní síť**.</span><span class="sxs-lookup"><span data-stu-id="3c36e-131">If not, select **Specify a New Local Network**.</span></span>
5. <span data-ttu-id="3c36e-132">**Připojení Site-to-Site** stránka se zobrazí, pokud jste vybrali v předchozím kroku zadat novou místní síť.</span><span class="sxs-lookup"><span data-stu-id="3c36e-132">The **Site-to-Site Connectivity** page appears if you selected to specify a new local network in the previous step.</span></span> <span data-ttu-id="3c36e-133">Ke konfiguraci vaší místní síti, zadejte následující informace a klikněte na šipku Další.</span><span class="sxs-lookup"><span data-stu-id="3c36e-133">To configure your local network, enter the following information and then click the next arrow.</span></span> 
   
   * <span data-ttu-id="3c36e-134">**Název** -název, který chcete použít na místní (místní) Síťová lokalita.</span><span class="sxs-lookup"><span data-stu-id="3c36e-134">**Name** - The name you want to call your local (on-premises) network site.</span></span>
   * <span data-ttu-id="3c36e-135">**Adresní prostor** – včetně počáteční IP adresu a CIDR (počet adres).</span><span class="sxs-lookup"><span data-stu-id="3c36e-135">**Address space** - including Starting IP and CIDR (Address Count).</span></span> <span data-ttu-id="3c36e-136">Můžete zadat libovolný rozsah adres, tak dlouho, dokud se nepřekrývá s rozsah adres pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="3c36e-136">You can specify any address range as long as it doesn't overlap with the address range for your virtual network.</span></span> <span data-ttu-id="3c36e-137">Obvykle to by zadejte rozsahy adres pro místní sítě, ale v případě ExpressRoute, nejsou tato nastavení použít.</span><span class="sxs-lookup"><span data-stu-id="3c36e-137">Typically, this would specify the address ranges for your on-premises networks, but in the case of ExpressRoute, these settings are not used.</span></span> <span data-ttu-id="3c36e-138">Toto nastavení je však vyžadováno k vytvoření místní sítě, při použití portálu classic.</span><span class="sxs-lookup"><span data-stu-id="3c36e-138">However, this setting is required in order to create the local network when you are using the classic portal.</span></span>
   * <span data-ttu-id="3c36e-139">**Přidání adresního prostoru** – toto nastavení není relevantní pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="3c36e-139">**Add address space** - This setting is not relevant for ExpressRoute.</span></span>
6. <span data-ttu-id="3c36e-140">Na **adresní prostory virtuální sítě** stránky, zadejte následující informace a klikněte na značku zaškrtnutí vpravo dole nakonfigurujete svoji síť.</span><span class="sxs-lookup"><span data-stu-id="3c36e-140">On the **Virtual Network Address Spaces** page, enter the following information and then click the checkmark on the lower right to configure your network.</span></span> 
   
   * <span data-ttu-id="3c36e-141">**Adresní prostor** – včetně počáteční IP a počet adres.</span><span class="sxs-lookup"><span data-stu-id="3c36e-141">**Address space** - including starting IP and address count.</span></span> <span data-ttu-id="3c36e-142">Ověřte, že zadané adresní prostory nepřekrývají s adresní prostory, které máte v místní síti.</span><span class="sxs-lookup"><span data-stu-id="3c36e-142">Verify that the address spaces you specify don’t overlap any of the address spaces that you have on your local network.</span></span>
   * <span data-ttu-id="3c36e-143">**Přidání podsítě** – včetně počáteční IP adresu a počet adres.</span><span class="sxs-lookup"><span data-stu-id="3c36e-143">**Add subnet** - including Starting IP and Address Count.</span></span> <span data-ttu-id="3c36e-144">Dodatečné podsítě nejsou vyžadovány.</span><span class="sxs-lookup"><span data-stu-id="3c36e-144">Additional subnets are not required.</span></span>
   * <span data-ttu-id="3c36e-145">**Přidání podsítě brány** – klikněte na tlačítko Přidat podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="3c36e-145">**Add gateway subnet** - Click to add the gateway subnet.</span></span> <span data-ttu-id="3c36e-146">Podsíť brány se používá pouze pro bránu virtuální sítě a pro tuto konfiguraci je vyžadována.</span><span class="sxs-lookup"><span data-stu-id="3c36e-146">The gateway subnet is used only for the virtual network gateway and is required for this configuration.</span></span><BR><span data-ttu-id="3c36e-147">Podsíť brány CIDR (počet adres) pro ExpressRoute musí být velikosti/28 nebo větší (/ 27, / 26 atd.).</span><span class="sxs-lookup"><span data-stu-id="3c36e-147">The gateway subnet CIDR (address count) for ExpressRoute must be /28 or larger (/27, /26 etc.).</span></span> <span data-ttu-id="3c36e-148">To umožňuje dost IP adres v této podsíti umožňující konfigurace fungovat.</span><span class="sxs-lookup"><span data-stu-id="3c36e-148">This allows for enough IP addresses in that subnet to allow the configuration to work.</span></span> <span data-ttu-id="3c36e-149">Na portálu classic Pokud jste zaškrtli políčko použít ExpressRoute, portálu určuje podsíť brány s velikosti/28.</span><span class="sxs-lookup"><span data-stu-id="3c36e-149">In the classic portal, if you have selected the checkbox to use ExpressRoute, the portal specifies a gateway subnet with /28.</span></span>  <span data-ttu-id="3c36e-150">Nelze upravit počet adres CIDR portálu classic.</span><span class="sxs-lookup"><span data-stu-id="3c36e-150">You can't adjust the CIDR address count in the classic portal.</span></span> <span data-ttu-id="3c36e-151">Podsíť brány se zobrazí jako **brány** portálu classic, i když skutečné název podsítě brány, který se vytvoří ve skutečnosti je **GatewaySubnet**.</span><span class="sxs-lookup"><span data-stu-id="3c36e-151">The gateway subnet will appear as **Gateway** in the classic portal, although the real name of the gateway subnet that is created is actually **GatewaySubnet**.</span></span> <span data-ttu-id="3c36e-152">Tento název můžete zobrazit pomocí prostředí PowerShell nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3c36e-152">You can view this name by using PowerShell or in the Azure portal.</span></span>
7. <span data-ttu-id="3c36e-153">Kliknutím na značku zaškrtnutí v dolní části stránky zahájíte vytváření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3c36e-153">Click the checkmark on the bottom of the page and your virtual network will begin to create.</span></span> <span data-ttu-id="3c36e-154">Po dokončení se zobrazí **vytvořen** uvedené v části **stav** na **sítě** na portálu classic.</span><span class="sxs-lookup"><span data-stu-id="3c36e-154">When it completes, you will see **Created** listed under **Status** on the **Networks** page in the classic portal.</span></span>

## <span data-ttu-id="3c36e-155"><a name="gw"></a>Vytvoření brány</span><span class="sxs-lookup"><span data-stu-id="3c36e-155"><a name="gw"></a>Create the gateway</span></span>
1. <span data-ttu-id="3c36e-156">Na **sítě** stránky, klikněte na virtuální síť, kterou jste právě vytvořili a pak klikněte na tlačítko **řídicí panel** v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="3c36e-156">On the **Networks** page, click the virtual network that you just created, then click **Dashboard** at the top of the page.</span></span>
2. <span data-ttu-id="3c36e-157">V dolní části **řídicí panel** klikněte na tlačítko **vytvořit bránu** a vyberte **dynamické směrování**.</span><span class="sxs-lookup"><span data-stu-id="3c36e-157">On the bottom of the **Dashboard** page, click **Create Gateway** and select **Dynamic Routing**.</span></span> <span data-ttu-id="3c36e-158">Klikněte na tlačítko **Ano** potvrďte, že chcete vytvořit bránu.</span><span class="sxs-lookup"><span data-stu-id="3c36e-158">Click **Yes** to confirm that you want to create a gateway.</span></span>
3. <span data-ttu-id="3c36e-159">Při spuštění vytváření brány se zobrazí zpráva informací, víte, že byla spuštěna brána.</span><span class="sxs-lookup"><span data-stu-id="3c36e-159">When the gateway starts creating, you’ll see a message letting you know that the gateway has been started.</span></span> <span data-ttu-id="3c36e-160">To může trvat až 45 minut pro vytvoření brány.</span><span class="sxs-lookup"><span data-stu-id="3c36e-160">It may take up to 45 minutes for the gateway to create.</span></span>
4. <span data-ttu-id="3c36e-161">Odkaz sítě k okruhu.</span><span class="sxs-lookup"><span data-stu-id="3c36e-161">Link your network to a circuit.</span></span> <span data-ttu-id="3c36e-162">Postupujte podle pokynů v článku [způsob propojení virtuálních sítí pro okruhy ExpressRoute](expressroute-howto-linkvnet-classic.md).</span><span class="sxs-lookup"><span data-stu-id="3c36e-162">Follow the instructions in the article [How to link VNets to ExpressRoute circuits](expressroute-howto-linkvnet-classic.md).</span></span>

## <span data-ttu-id="3c36e-163"><a name="config"></a>Konfigurace existující klasické virtuální sítě pro ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="3c36e-163"><a name="config"></a>Configure an existing classic VNet for ExpressRoute</span></span>
<span data-ttu-id="3c36e-164">Pokud již máte klasické virtuální sítě, můžete nakonfigurovat na portálu classic připojení expressroute.</span><span class="sxs-lookup"><span data-stu-id="3c36e-164">If you already have a classic VNet, you can configure it to connect to ExpressRoute in the classic portal.</span></span> <span data-ttu-id="3c36e-165">Nastavení stejné jako v částech výše, jsou, takže pročtěte těchto částí se seznámit s požadovaná nastavení.</span><span class="sxs-lookup"><span data-stu-id="3c36e-165">The settings are the same as the sections above, so read through those sections to become familiar with the required settings.</span></span> <span data-ttu-id="3c36e-166">Pokud chcete vytvořit koexistujících připojení ExpressRoute nebo Site-to-Site, přečtěte si téma [v tomto článku](expressroute-howto-coexist-classic.md) kroky.</span><span class="sxs-lookup"><span data-stu-id="3c36e-166">If you want to create an ExpressRoute/Site-to-Site coexisting connection, see [this article](expressroute-howto-coexist-classic.md) for the steps.</span></span> <span data-ttu-id="3c36e-167">Jsou jiné než kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="3c36e-167">They are different than the steps in this article.</span></span>

1. <span data-ttu-id="3c36e-168">Budete muset vytvořit místní sítě, než aktualizujete zbývající nastavení virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3c36e-168">You need to create the local network before you update the rest of your VNet settings.</span></span> <span data-ttu-id="3c36e-169">Chcete-li vytvořit novou místní síť, která se vyžaduje při konfiguraci ExpressRoute prostřednictvím portálu classic, klikněte na tlačítko **nový**  **>**  **síťové služby**  **>**  **virtuální sítě**  **>**  **místní sítě přidat**.</span><span class="sxs-lookup"><span data-stu-id="3c36e-169">To create a new local network, which is required when configuring ExpressRoute through the classic portal, click **New** **>** **Network Services** **>** **Virtual Network** **>** **Add local network**.</span></span> <span data-ttu-id="3c36e-170">Postupujte podle kroků v průvodci vytvořit místní sítě.</span><span class="sxs-lookup"><span data-stu-id="3c36e-170">Follow the wizard steps to create the local network.</span></span>
2. <span data-ttu-id="3c36e-171">Použití **konfigurace** stránku aktualizovat zbytek nastavení pro virtuální síť a přidružte virtuální sítě k místní síti.</span><span class="sxs-lookup"><span data-stu-id="3c36e-171">Use **Configure** page to update the rest of the settings for your VNet and to associate the VNet to the local network.</span></span>
3. <span data-ttu-id="3c36e-172">Po konfiguraci nastavení, přejděte na [vytvoření brány](#gw) tohoto článku, chcete-li vytvořit bránu.</span><span class="sxs-lookup"><span data-stu-id="3c36e-172">After configuring the settings, go to the [Create the gateway](#gw) section of this article to create the gateway.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c36e-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c36e-173">Next steps</span></span>
* <span data-ttu-id="3c36e-174">Pokud chcete přidat virtuální počítače k virtuální síti, najdete v části [virtuální počítače kurzů](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="3c36e-174">If you want to add virtual machines to your virtual network, see [Virtual Machines learning paths](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).</span></span>
* <span data-ttu-id="3c36e-175">Pokud chcete získat další informace o ExpressRoute, přečtěte si téma [přehled ExpressRoute](expressroute-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3c36e-175">If you want to learn more about ExpressRoute, see the [ExpressRoute Overview](expressroute-introduction.md).</span></span>
