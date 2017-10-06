---
title: "Připojení vaší místní síti tooan virtuální síť Azure: Site-to-Site VPN: portál | Microsoft Docs"
description: "Kroky toocreate připojení IPsec z místní sítě tooan virtuální síť Azure přes hello veřejného Internetu. Tyto kroky můžete vytvořit připojení mezi různými místy Site-to-Site VPN Gateway pomocí portálu hello."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 6f0acbaf1bf016026cefade048a116e94686103d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-site-to-site-connection-in-hello-azure-portal"></a><span data-ttu-id="1816d-104">Umožňuje vytvořit připojení Site-to-Site v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1816d-104">Create a Site-to-Site connection in hello Azure portal</span></span>

<span data-ttu-id="1816d-105">Tento článek ukazuje, jak toouse hello Azure portálu toocreate připojení k bráně VPN Site-to-Site z vaší místní síti toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1816d-105">This article shows you how toouse hello Azure portal toocreate a Site-to-Site VPN gateway connection from your on-premises network toohello VNet.</span></span> <span data-ttu-id="1816d-106">Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1816d-106">hello steps in this article apply toohello Resource Manager deployment model.</span></span> <span data-ttu-id="1816d-107">Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="1816d-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1816d-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1816d-108">Azure portal</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="1816d-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1816d-109">PowerShell</span></span>](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [<span data-ttu-id="1816d-110">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="1816d-110">CLI</span></span>](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [<span data-ttu-id="1816d-111">Azure Portal (Classic)</span><span class="sxs-lookup"><span data-stu-id="1816d-111">Azure portal (classic)</span></span>](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

<span data-ttu-id="1816d-112">Připojení k bráně VPN Site-to-Site je použité tooconnect místní sítě tooan virtuální síť Azure přes tunelové propojení VPN protokolu IPsec/IKE (IKEv1 nebo IKEv2).</span><span class="sxs-lookup"><span data-stu-id="1816d-112">A Site-to-Site VPN gateway connection is used tooconnect your on-premises network tooan Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel.</span></span> <span data-ttu-id="1816d-113">Tento typ připojení vyžaduje sítě VPN zařízení nachází v místě které má zvenčí veřejnou IP adresu přiřazenou tooit.</span><span class="sxs-lookup"><span data-stu-id="1816d-113">This type of connection requires a VPN device located on-premises that has an externally facing public IP address assigned tooit.</span></span> <span data-ttu-id="1816d-114">Další informace o bránách VPN najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="1816d-114">For more information about VPN gateways, see [About VPN gateway](vpn-gateway-about-vpngateways.md).</span></span>

![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a><span data-ttu-id="1816d-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1816d-116">Before you begin</span></span>

<span data-ttu-id="1816d-117">Ověřte, že jste splnili hello před zahájením konfigurace následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="1816d-117">Verify that you have met hello following criteria before beginning your configuration:</span></span>

* <span data-ttu-id="1816d-118">Zajistěte, aby byla kompatibilní zařízení VPN a někoho, kdo je možné tooconfigure ho.</span><span class="sxs-lookup"><span data-stu-id="1816d-118">Make sure you have a compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="1816d-119">Další informace o kompatibilních zařízeních VPN a konfiguraci zařízení najdete v tématu [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="1816d-119">For more information about compatible VPN devices and device configuration, see [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span>
* <span data-ttu-id="1816d-120">Ověřte, že máte veřejnou IPv4 adresu pro vaše zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="1816d-120">Verify that you have an externally facing public IPv4 address for your VPN device.</span></span> <span data-ttu-id="1816d-121">Tato IP adresa nesmí být umístěná za překladem adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="1816d-121">This IP address cannot be located behind a NAT.</span></span>
* <span data-ttu-id="1816d-122">Pokud jste obeznámeni s rozsahy IP adres hello umístěný v konfiguraci místní sítě, musíte toocoordinate s někým, kdo může pro vás jsou tyto podrobnosti obsaženy.</span><span class="sxs-lookup"><span data-stu-id="1816d-122">If you are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span> <span data-ttu-id="1816d-123">Když vytváříte tuto konfiguraci, musíte zadat hello IP adresa rozsahu předpony, Azure bude směrovat tooyour místní umístění.</span><span class="sxs-lookup"><span data-stu-id="1816d-123">When you create this configuration, you must specify hello IP address range prefixes that Azure will route tooyour on-premises location.</span></span> <span data-ttu-id="1816d-124">Žádná z podsítí hello vaší místní sítě můžete prostřednictvím okruhu s hello podsítě virtuální sítě, které chcete tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="1816d-124">None of hello subnets of your on-premises network can over lap with hello virtual network subnets that you want tooconnect to.</span></span> 

### <span data-ttu-id="1816d-125"><a name="values"></a>Příklady hodnot</span><span class="sxs-lookup"><span data-stu-id="1816d-125"><a name="values"></a>Example values</span></span>

<span data-ttu-id="1816d-126">Hello příklady v tomto článku používají hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1816d-126">hello examples in this article use hello following values.</span></span> <span data-ttu-id="1816d-127">Můžete použít tyto hodnoty toocreate testovací prostředí nebo odkazovat toothem toobetter pochopit hello příklady v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="1816d-127">You can use these values toocreate a test environment, or refer toothem toobetter understand hello examples in this article.</span></span>

* <span data-ttu-id="1816d-128">**Název virtuální sítě:** TestVNet1</span><span class="sxs-lookup"><span data-stu-id="1816d-128">**VNet Name:** TestVNet1</span></span>
* <span data-ttu-id="1816d-129">**Adresní prostor:**</span><span class="sxs-lookup"><span data-stu-id="1816d-129">**Address Space:**</span></span> 
  * <span data-ttu-id="1816d-130">10.11.0.0/16</span><span class="sxs-lookup"><span data-stu-id="1816d-130">10.11.0.0/16</span></span>
  * <span data-ttu-id="1816d-131">10.12.0.0/16 (volitelné pro toto cvičení)</span><span class="sxs-lookup"><span data-stu-id="1816d-131">10.12.0.0/16 (optional for this exercise)</span></span>
* <span data-ttu-id="1816d-132">**Podsítě:**</span><span class="sxs-lookup"><span data-stu-id="1816d-132">**Subnets:**</span></span>
  * <span data-ttu-id="1816d-133">FrontEnd: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="1816d-133">FrontEnd: 10.11.0.0/24</span></span>
  * <span data-ttu-id="1816d-134">BackEnd: 10.12.0.0/24 (volitelné pro toto cvičení)</span><span class="sxs-lookup"><span data-stu-id="1816d-134">BackEnd: 10.12.0.0/24 (optional for this exercise)</span></span>
* <span data-ttu-id="1816d-135">**Podsíť brány:** 10.11.255.0/27</span><span class="sxs-lookup"><span data-stu-id="1816d-135">**GatewaySubnet:** 10.11.255.0/27</span></span>
* <span data-ttu-id="1816d-136">**Skupina prostředků:** TestRG1</span><span class="sxs-lookup"><span data-stu-id="1816d-136">**Resource Group:** TestRG1</span></span>
* <span data-ttu-id="1816d-137">**Umístění:** Východní USA</span><span class="sxs-lookup"><span data-stu-id="1816d-137">**Location:** East US</span></span>
* <span data-ttu-id="1816d-138">**Server DNS:** Volitelné.</span><span class="sxs-lookup"><span data-stu-id="1816d-138">**DNS Server:** Optional.</span></span> <span data-ttu-id="1816d-139">Hello IP adresa serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="1816d-139">hello IP address of your DNS server.</span></span>
* <span data-ttu-id="1816d-140">**Název brány virtuální sítě:** VNet1GW</span><span class="sxs-lookup"><span data-stu-id="1816d-140">**Virtual Network Gateway Name:** VNet1GW</span></span>
* <span data-ttu-id="1816d-141">**Veřejná IP adresa:** VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="1816d-141">**Public IP:** VNet1GWIP</span></span>
* <span data-ttu-id="1816d-142">**Typ sítě VPN:** Trasová</span><span class="sxs-lookup"><span data-stu-id="1816d-142">**VPN Type:** Route-based</span></span>
* <span data-ttu-id="1816d-143">**Typ připojení:** Site-to-Site (protokol IPsec)</span><span class="sxs-lookup"><span data-stu-id="1816d-143">**Connection Type:** Site-to-site (IPsec)</span></span>
* <span data-ttu-id="1816d-144">**Typ brány:** Síť VPN</span><span class="sxs-lookup"><span data-stu-id="1816d-144">**Gateway Type:** VPN</span></span>
* <span data-ttu-id="1816d-145">**Název brány místní sítě:** Site2</span><span class="sxs-lookup"><span data-stu-id="1816d-145">**Local Network Gateway Name:** Site2</span></span>
* <span data-ttu-id="1816d-146">**Název připojení:** VNet1toSite2</span><span class="sxs-lookup"><span data-stu-id="1816d-146">**Connection Name:** VNet1toSite2</span></span>

## <span data-ttu-id="1816d-147"><a name="CreatVNet"></a>1. Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="1816d-147"><a name="CreatVNet"></a>1. Create a virtual network</span></span>

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-s2s-rm-portal-include.md)]

## <span data-ttu-id="1816d-148"><a name="dns"></a>2. Určení serveru DNS</span><span class="sxs-lookup"><span data-stu-id="1816d-148"><a name="dns"></a>2. Specify a DNS server</span></span>

<span data-ttu-id="1816d-149">DNS není připojení vyžaduje toocreate Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="1816d-149">DNS is not required toocreate a Site-to-Site connection.</span></span> <span data-ttu-id="1816d-150">Pokud chcete toohave překladu pro prostředky, které jsou nasazené tooyour virtuální sítě, ale měli určit DNS server.</span><span class="sxs-lookup"><span data-stu-id="1816d-150">However, if you want toohave name resolution for resources that are deployed tooyour virtual network, you should specify a DNS server.</span></span> <span data-ttu-id="1816d-151">Toto nastavení umožňuje určit server DNS hello má toouse pro překlad názvů pro tuto virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="1816d-151">This setting lets you specify hello DNS server that you want toouse for name resolution for this virtual network.</span></span> <span data-ttu-id="1816d-152">Neslouží k vytvoření serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="1816d-152">It does not create a DNS server.</span></span> <span data-ttu-id="1816d-153">Další informace o překladu IP adres najdete v tématu [Překlad názvů pro instance rolí a VM](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="1816d-153">For more information about name resolution, see [Name Resolution for VMs and role instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <span data-ttu-id="1816d-154"><a name="gatewaysubnet"></a>3. Vytvořit podsíť brány hello</span><span class="sxs-lookup"><span data-stu-id="1816d-154"><a name="gatewaysubnet"></a>3. Create hello gateway subnet</span></span>

[!INCLUDE [vpn-gateway-aboutgwsubnet](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]

## <span data-ttu-id="1816d-155"><a name="VNetGateway"></a>4. Vytvoření brány VPN hello</span><span class="sxs-lookup"><span data-stu-id="1816d-155"><a name="VNetGateway"></a>4. Create hello VPN gateway</span></span>

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <span data-ttu-id="1816d-156"><a name="LocalNetworkGateway"></a>5. Vytvoření brány místní sítě hello</span><span class="sxs-lookup"><span data-stu-id="1816d-156"><a name="LocalNetworkGateway"></a>5. Create hello local network gateway</span></span>

<span data-ttu-id="1816d-157">Hello brány místní sítě obvykle odkazuje tooyour místní umístění.</span><span class="sxs-lookup"><span data-stu-id="1816d-157">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="1816d-158">Zadejte název, pomocí kterého můžete Azure odkazovat tooit a pak zadejte IP adresu hello hello lokality z hello místní VPN zařízení toowhich se vytvoří připojení.</span><span class="sxs-lookup"><span data-stu-id="1816d-158">You give hello site a name by which Azure can refer tooit, then specify hello IP address of hello on-premises VPN device toowhich you will create a connection.</span></span> <span data-ttu-id="1816d-159">Můžete také určit hello předpony IP adres, které budou směrovány prostřednictvím zařízení VPN toohello brány sítě VPN hello.</span><span class="sxs-lookup"><span data-stu-id="1816d-159">You also specify hello IP address prefixes that will be routed through hello VPN gateway toohello VPN device.</span></span> <span data-ttu-id="1816d-160">Hello předpony, které zadáte předpony hello umístěn ve vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="1816d-160">hello address prefixes you specify are hello prefixes located on your on-premises network.</span></span> <span data-ttu-id="1816d-161">Pokud změny vaší místní sítě, nebo můžete potřebovat pro zařízení VPN hello toochange hello veřejnou IP adresu, můžete snadno aktualizovat hodnoty hello později.</span><span class="sxs-lookup"><span data-stu-id="1816d-161">If your on-premises network changes or you need toochange hello public IP address for hello VPN device, you can easily update hello values later.</span></span>

[!INCLUDE [Add local network gateway](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <span data-ttu-id="1816d-162"><a name="VPNDevice"></a>6. Konfigurace zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="1816d-162"><a name="VPNDevice"></a>6. Configure your VPN device</span></span>

<span data-ttu-id="1816d-163">Připojení Site-to-Site tooan do místní sítě vyžaduje zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="1816d-163">Site-to-Site connections tooan on-premises network require a VPN device.</span></span> <span data-ttu-id="1816d-164">V tomto kroku nakonfigurujete zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="1816d-164">In this step, you configure your VPN device.</span></span> <span data-ttu-id="1816d-165">Při konfiguraci zařízení VPN, je třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="1816d-165">When configuring your VPN device, you need hello following:</span></span>

- <span data-ttu-id="1816d-166">Sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="1816d-166">A shared key.</span></span> <span data-ttu-id="1816d-167">To je hello stejný sdílený klíč, který zadáte při vytváření připojení Site-to-Site VPN.</span><span class="sxs-lookup"><span data-stu-id="1816d-167">This is hello same shared key that you specify when creating your Site-to-Site VPN connection.</span></span> <span data-ttu-id="1816d-168">V našich ukázkách používáme základní sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="1816d-168">In our examples, we use a basic shared key.</span></span> <span data-ttu-id="1816d-169">Doporučujeme, abyste vygenerování složitější klíče toouse.</span><span class="sxs-lookup"><span data-stu-id="1816d-169">We recommend that you generate a more complex key toouse.</span></span>
- <span data-ttu-id="1816d-170">Hello veřejnou IP adresu brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1816d-170">hello Public IP address of your virtual network gateway.</span></span> <span data-ttu-id="1816d-171">Hello veřejnou IP adresu můžete zobrazit pomocí hello portálu Azure, PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1816d-171">You can view hello public IP address by using hello Azure portal, PowerShell, or CLI.</span></span> <span data-ttu-id="1816d-172">toofind hello veřejnou IP adresu brány VPN pomocí hello portál Azure, přejděte příliš**brány virtuální sítě**, pak klikněte na název své brány hello.</span><span class="sxs-lookup"><span data-stu-id="1816d-172">toofind hello Public IP address of your VPN gateway using hello Azure portal, navigate too**Virtual network gateways**, then click hello name of your gateway.</span></span>

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <span data-ttu-id="1816d-173"><a name="CreateConnection"></a>7. Vytvoření připojení VPN hello</span><span class="sxs-lookup"><span data-stu-id="1816d-173"><a name="CreateConnection"></a>7. Create hello VPN connection</span></span>

<span data-ttu-id="1816d-174">Vytvořte připojení VPN hello Site-to-Site mezi bránou virtuální sítě a vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="1816d-174">Create hello Site-to-Site VPN connection between your virtual network gateway and your on-premises VPN device.</span></span>

[!INCLUDE [Add connections](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <span data-ttu-id="1816d-175"><a name="VerifyConnection"></a>8. Ověření připojení VPN hello</span><span class="sxs-lookup"><span data-stu-id="1816d-175"><a name="VerifyConnection"></a>8. Verify hello VPN connection</span></span>

[!INCLUDE [Verify - Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <span data-ttu-id="1816d-176"><a name="connectVM"></a>tooconnect tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1816d-176"><a name="connectVM"></a>tooconnect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <span data-ttu-id="1816d-177"><a name="reset"></a>Jak tooreset brána sítě VPN</span><span class="sxs-lookup"><span data-stu-id="1816d-177"><a name="reset"></a>How tooreset a VPN gateway</span></span>

<span data-ttu-id="1816d-178">Resetování brány Azure VPN je užitečné v případě ztráty připojení VPN mezi lokalitami na jednom nebo více tunelech VPN typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="1816d-178">Resetting an Azure VPN gateway is helpful if you lose cross-premises VPN connectivity on one or more Site-to-Site VPN tunnels.</span></span> <span data-ttu-id="1816d-179">V této situaci vaše místní zařízení VPN jsou všechny funguje správně, ale nebylo možné tooestablish tunelových propojení IPsec pomocí bran Azure VPN hello.</span><span class="sxs-lookup"><span data-stu-id="1816d-179">In this situation, your on-premises VPN devices are all working correctly, but are not able tooestablish IPsec tunnels with hello Azure VPN gateways.</span></span> <span data-ttu-id="1816d-180">Pokyny najdete v tématu [Resetování brány VPN](vpn-gateway-resetgw-classic.md).</span><span class="sxs-lookup"><span data-stu-id="1816d-180">For steps, see [Reset a VPN gateway](vpn-gateway-resetgw-classic.md).</span></span>

## <span data-ttu-id="1816d-181"><a name="resize"></a>Jak toochange bránu SKU (změny velikosti brány)</span><span class="sxs-lookup"><span data-stu-id="1816d-181"><a name="resize"></a>How toochange a gateway SKU (resize a gateway)</span></span>

<span data-ttu-id="1816d-182">Hello kroky toochange skladová položka brány, najdete v části [SKU brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="1816d-182">For hello steps toochange a gateway SKU, see [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1816d-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1816d-183">Next steps</span></span>

* <span data-ttu-id="1816d-184">Informace o protokolu BGP najdete v tématu hello [přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [jak tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="1816d-184">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
* <span data-ttu-id="1816d-185">Informace o vynuceném tunelování najdete v tématu [Informace o vynuceném tunelování](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="1816d-185">For information about Forced Tunneling, see [About Forced Tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>
* <span data-ttu-id="1816d-186">Informace o vysoce dostupných připojeních typu aktivní-aktivní najdete v tématu [Připojení s vysokou dostupností mezi jednotlivými místy a VNet-to-VNet](vpn-gateway-highlyavailable.md).</span><span class="sxs-lookup"><span data-stu-id="1816d-186">For information about Highly Available Active-Active connections, see [Highly Available cross-premises and VNet-to-VNet connectivity](vpn-gateway-highlyavailable.md).</span></span>
