---
title: "Připojit virtuální síti Azure tooanother virtuální sítě: prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede propojováním virtuálních sítí s použitím Azure Resource Manageru a prostředí PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0683c664-9c03-40a4-b198-a6529bf1ce8b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 2da30c76867cc3f71d040e63e0dd15d153e15c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a><span data-ttu-id="4c7bc-103">Konfigurace připojení brány VPN typu VNet-to-VNet pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="4c7bc-103">Configure a VNet-to-VNet VPN gateway connection using PowerShell</span></span>

<span data-ttu-id="4c7bc-104">Tento článek ukazuje, jak toocreate připojení k bráně VPN mezi virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-104">This article shows you how toocreate a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="4c7bc-105">Hello virtuální sítě může být v hello stejné nebo různých oblastí, a z hello stejné nebo různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-105">hello virtual networks can be in hello same or different regions, and from hello same or different subscriptions.</span></span> <span data-ttu-id="4c7bc-106">Při připojování virtuální sítě z různých předplatných, odběry hello nemusí toobe přidružené hello stejné klienta služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-106">When connecting VNets from different subscriptions, hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> 

<span data-ttu-id="4c7bc-107">Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager a pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-107">hello steps in this article apply toohello Resource Manager deployment model and use PowerShell.</span></span> <span data-ttu-id="4c7bc-108">Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4c7bc-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4c7bc-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="4c7bc-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4c7bc-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="4c7bc-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4c7bc-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="4c7bc-112">Azure Portal (Classic)</span><span class="sxs-lookup"><span data-stu-id="4c7bc-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="4c7bc-113">Propojení různých modelů nasazení – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4c7bc-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="4c7bc-114">Propojení různých modelů nasazení – PowerShell</span><span class="sxs-lookup"><span data-stu-id="4c7bc-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="4c7bc-115">Propojení virtuální sítě tooanother virtuální síť (VNet-to-VNet) je podobné tooconnecting umístění lokality tooan místní virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-115">Connecting a virtual network tooanother virtual network (VNet-to-VNet) is similar tooconnecting a VNet tooan on-premises site location.</span></span> <span data-ttu-id="4c7bc-116">Oba typy připojení využívají bránu tooprovide sítě VPN přes zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-116">Both connectivity types use a VPN gateway tooprovide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="4c7bc-117">Pokud vaše virtuální sítě jsou v hello stejné oblasti, může být vhodné tooconsider připojení pomocí virtuální sítě partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-117">If your VNets are in hello same region, you may want tooconsider connecting them using VNet Peering.</span></span> <span data-ttu-id="4c7bc-118">Partnerské vztahy virtuálních sítí nepoužívají bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="4c7bc-119">Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4c7bc-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="4c7bc-120">Komunikaci typu VNet-to-VNet můžete kombinovat s konfiguracemi s více servery.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="4c7bc-121">To umožňuje vytvářet topologie sítí, které spojují připojení mezi různými místy s připojením propojování virtuálních sítí, jak je znázorněno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in hello following diagram:</span></span>

![Informace o připojeních](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a><span data-ttu-id="4c7bc-123">Proč propojovat virtuální sítě?</span><span class="sxs-lookup"><span data-stu-id="4c7bc-123">Why connect virtual networks?</span></span>

<span data-ttu-id="4c7bc-124">Tooconnect virtuální sítě může být vhodné pro hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-124">You may want tooconnect virtual networks for hello following reasons:</span></span>

* <span data-ttu-id="4c7bc-125">**Geografická redundance napříč oblastmi a geografická přítomnost**</span><span class="sxs-lookup"><span data-stu-id="4c7bc-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="4c7bc-126">Můžete nastavit vlastní geografickou replikaci nebo synchronizaci se zabezpečeným připojením bez procházení koncovými body připojenými k internetu.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="4c7bc-127">Pomocí Azure Traffic Manageru a služby Load Balancer je možné vytvářet úlohy s vysokou dostupností s geografickou redundancí nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="4c7bc-128">Jedním z důležitých příkladů je tooset až SQL Always On se skupinami dostupnosti nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-128">One important example is tooset up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="4c7bc-129">**Regionální vícevrstvé aplikace s izolací nebo administrativní hranicí**</span><span class="sxs-lookup"><span data-stu-id="4c7bc-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="4c7bc-130">Hello uvnitř stejné oblasti, můžete nastavit vícevrstvé aplikace s několika virtuálními sítěmi propojenými z důvodu tooisolation nebo požadavků na správu.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-130">Within hello same region, you can set up multi-tier applications with multiple virtual networks connected together due tooisolation or administrative requirements.</span></span>

<span data-ttu-id="4c7bc-131">Další informace o připojení VNet-to-VNet, najdete v části hello [nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-131">For more information about VNet-to-VNet connections, see hello [VNet-to-VNet FAQ](#faq) at hello end of this article.</span></span>

## <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="4c7bc-132">Kterou posloupnost kroků provést?</span><span class="sxs-lookup"><span data-stu-id="4c7bc-132">Which set of steps should I use?</span></span>

<span data-ttu-id="4c7bc-133">V tomto článku uvidíte dvě různé sady kroků.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="4c7bc-134">Jednu sadu kroky pro [hello virtuální sítě, které jsou umístěny ve stejné předplatné](#samesub)a druhý pro [patřící do různých předplatných](#difsub).</span><span class="sxs-lookup"><span data-stu-id="4c7bc-134">One set of steps for [VNets that reside in hello same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span> <span data-ttu-id="4c7bc-135">Hello klíčovým rozdílem mezi sadami hello je jestli můžete vytvořit a nakonfigurovat všechny virtuální sítě a brány prostředky v rámci hello stejné relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-135">hello key difference between hello sets is whether you can create and configure all virtual network and gateway resources within hello same PowerShell session.</span></span>

<span data-ttu-id="4c7bc-136">Hello kroky v tomto článku používají proměnné, které jsou deklarované v hello začátku každého oddílu.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-136">hello steps in this article use variables that are declared at hello beginning of each section.</span></span> <span data-ttu-id="4c7bc-137">Pokud již pracujete s existující virtuální sítě, upravte hello proměnné tooreflect hello nastavení ve svém vlastním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-137">If you already are working with existing VNets, modify hello variables tooreflect hello settings in your own environment.</span></span> <span data-ttu-id="4c7bc-138">Pokud chcete překlad IP adres pro virtuální sítě, přečtěte si téma [Překlad IP adres](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="4c7bc-138">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

## <span data-ttu-id="4c7bc-139"><a name="samesub"></a>Jak tooconnect virtuální sítě, jsou v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-139"><a name="samesub"></a>How tooconnect VNets that are in hello same subscription</span></span>

![Diagram v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="4c7bc-141">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4c7bc-141">Before you begin</span></span>

<span data-ttu-id="4c7bc-142">Než začnete, musíte tooinstall hello nejnovější verzi rutin Powershellu pro Azure Resource Manager hello alespoň 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-142">Before beginning, you need tooinstall hello latest version of hello Azure Resource Manager PowerShell cmdlets, at least 4.0 or later.</span></span> <span data-ttu-id="4c7bc-143">Další informace o instalaci rutin prostředí PowerShell hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4c7bc-143">For more information about installing hello PowerShell cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <span data-ttu-id="4c7bc-144"><a name="Step1"></a>Krok 1: Plánování rozsahů IP adres</span><span class="sxs-lookup"><span data-stu-id="4c7bc-144"><a name="Step1"></a>Step 1 - Plan your IP address ranges</span></span>

<span data-ttu-id="4c7bc-145">V hello následující kroky vytvoříme dvě virtuální sítě spolu s jejich příslušnými podsítěmi brány a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-145">In hello following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="4c7bc-146">Poté vytvoříme připojení VPN mezi hello dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-146">We then create a VPN connection between hello two VNets.</span></span> <span data-ttu-id="4c7bc-147">Je důležité tooplan rozsahy IP adres hello pro konfiguraci sítě.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-147">It’s important tooplan hello IP address ranges for your network configuration.</span></span> <span data-ttu-id="4c7bc-148">Mějte na paměti, že je třeba zajistit, aby se žádné rozsahy virtuálních sítí ani místní síťové rozsahy žádným způsobem nepřekrývaly.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-148">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="4c7bc-149">V těchto příkladech nezahrnujeme server DNS.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-149">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="4c7bc-150">Pokud chcete překlad IP adres pro virtuální sítě, přečtěte si téma [Překlad IP adres](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="4c7bc-150">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="4c7bc-151">Můžeme použít následující hodnoty v příkladech hello hello:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-151">We use hello following values in hello examples:</span></span>

<span data-ttu-id="4c7bc-152">**Hodnoty pro virtuální síť TestVNet1:**</span><span class="sxs-lookup"><span data-stu-id="4c7bc-152">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="4c7bc-153">Název virtuální sítě: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="4c7bc-153">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="4c7bc-154">Skupina prostředků: TestRG1</span><span class="sxs-lookup"><span data-stu-id="4c7bc-154">Resource Group: TestRG1</span></span>
* <span data-ttu-id="4c7bc-155">Umístění: Východní USA</span><span class="sxs-lookup"><span data-stu-id="4c7bc-155">Location: East US</span></span>
* <span data-ttu-id="4c7bc-156">TestVNet1: 10.11.0.0/16 a 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="4c7bc-156">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="4c7bc-157">FrontEnd: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="4c7bc-157">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="4c7bc-158">BackEnd: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="4c7bc-158">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="4c7bc-159">GatewaySubnet: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="4c7bc-159">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="4c7bc-160">Název brány: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="4c7bc-160">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="4c7bc-161">Veřejná IP adresa: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="4c7bc-161">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="4c7bc-162">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="4c7bc-162">VPNType: RouteBased</span></span>
* <span data-ttu-id="4c7bc-163">Připojení (1 ke 4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="4c7bc-163">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="4c7bc-164">Připojení (1 k 5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="4c7bc-164">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="4c7bc-165">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="4c7bc-165">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="4c7bc-166">**Hodnoty pro virtuální síť TestVNet4:**</span><span class="sxs-lookup"><span data-stu-id="4c7bc-166">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="4c7bc-167">Název virtuální sítě: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="4c7bc-167">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="4c7bc-168">TestVNet2: 10.41.0.0/16 a 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="4c7bc-168">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="4c7bc-169">FrontEnd: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="4c7bc-169">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="4c7bc-170">BackEnd: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="4c7bc-170">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="4c7bc-171">GatewaySubnet: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="4c7bc-171">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="4c7bc-172">Skupina prostředků: TestRG4</span><span class="sxs-lookup"><span data-stu-id="4c7bc-172">Resource Group: TestRG4</span></span>
* <span data-ttu-id="4c7bc-173">Umístění: Západní USA</span><span class="sxs-lookup"><span data-stu-id="4c7bc-173">Location: West US</span></span>
* <span data-ttu-id="4c7bc-174">Název brány: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="4c7bc-174">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="4c7bc-175">Veřejná IP adresa: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="4c7bc-175">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="4c7bc-176">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="4c7bc-176">VPNType: RouteBased</span></span>
* <span data-ttu-id="4c7bc-177">Připojení: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="4c7bc-177">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="4c7bc-178">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="4c7bc-178">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="4c7bc-179"><a name="Step2"></a>Krok 2: Vytvoření a konfigurace virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="4c7bc-179"><a name="Step2"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="4c7bc-180">Deklarujte proměnné.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-180">Declare your variables.</span></span> <span data-ttu-id="4c7bc-181">Tento příklad deklaruje hello proměnné pomocí hello hodnot pro toto cvičení.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-181">This example declares hello variables using hello values for this exercise.</span></span> <span data-ttu-id="4c7bc-182">Ve většině případů má nahradit hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-182">In most cases, you should replace hello values with your own.</span></span> <span data-ttu-id="4c7bc-183">Tyto proměnné však můžete použít, pokud používáte prostřednictvím toobecome kroky hello seznámili s tímto typem konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-183">However, you can use these variables if you are running through hello steps toobecome familiar with this type of configuration.</span></span> <span data-ttu-id="4c7bc-184">Umožňuje změnit proměnné hello v případě potřeby pak zkopírujte a vložte je do konzoly prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-184">Modify hello variables if needed, then copy and paste them into your PowerShell console.</span></span>

  ```powershell
  $Sub1 = "Replace_With_Your_Subcription_Name"
  $RG1 = "TestRG1"
  $Location1 = "East US"
  $VNetName1 = "TestVNet1"
  $FESubName1 = "FrontEnd"
  $BESubName1 = "Backend"
  $GWSubName1 = "GatewaySubnet"
  $VNetPrefix11 = "10.11.0.0/16"
  $VNetPrefix12 = "10.12.0.0/16"
  $FESubPrefix1 = "10.11.0.0/24"
  $BESubPrefix1 = "10.12.0.0/24"
  $GWSubPrefix1 = "10.12.255.0/27"
  $GWName1 = "VNet1GW"
  $GWIPName1 = "VNet1GWIP"
  $GWIPconfName1 = "gwipconf1"
  $Connection14 = "VNet1toVNet4"
  $Connection15 = "VNet1toVNet5"
  ```

2. <span data-ttu-id="4c7bc-185">Připojte tooyour účet.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-185">Connect tooyour account.</span></span> <span data-ttu-id="4c7bc-186">Použijte následující příklad toohelp, ke kterým se připojujete hello:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-186">Use hello following example toohelp you connect:</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="4c7bc-187">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-187">Check hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

  <span data-ttu-id="4c7bc-188">Zadejte hello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-188">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub1
  ```
3. <span data-ttu-id="4c7bc-189">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-189">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```
4. <span data-ttu-id="4c7bc-190">Vytvoření konfigurací podsítě pro virtuální síť TestVNet1 hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-190">Create hello subnet configurations for TestVNet1.</span></span> <span data-ttu-id="4c7bc-191">Tato ukázka vytvoří virtuální síť s názvem TestVNet1 a tři podsítě: jednu s názvem GatewaySubnet, jednu s názvem FrontEnd a jednu s názvem BackEnd.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-191">This example creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="4c7bc-192">Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-192">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="4c7bc-193">Pokud použijete jiný název, vytvoření brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-193">If you name it something else, your gateway creation fails.</span></span>

  <span data-ttu-id="4c7bc-194">Hello následující příklad používá hello proměnné, které jste nastavili dříve.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-194">hello following example uses hello variables that you set earlier.</span></span> <span data-ttu-id="4c7bc-195">V tomto příkladu používá podsíť brány hello 27.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-195">In this example, hello gateway subnet is using a /27.</span></span> <span data-ttu-id="4c7bc-196">I když je možné toocreate podsíť brány jako malé/29, doporučujeme vytvořit větší podsíť, která zahrnuje víc adres výběrem minimálně/28 nebo /27.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-196">While it is possible toocreate a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="4c7bc-197">To vám umožní dostatek adresy tooaccommodate možné další konfigurace, které můžete ve hello budoucí.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-197">This will allow for enough addresses tooaccommodate possible additional configurations that you may want in hello future.</span></span>

  ```powershell
  $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
  $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
  $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
  ```
5. <span data-ttu-id="4c7bc-198">Vytvořte virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-198">Create TestVNet1.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
  ```
6. <span data-ttu-id="4c7bc-199">Požádat o veřejné IP adresy toobe přidělené toohello bránu, které vytvoříte pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-199">Request a public IP address toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="4c7bc-200">Všimněte si, že hello AllocationMethod je dynamický.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-200">Notice that hello AllocationMethod is Dynamic.</span></span> <span data-ttu-id="4c7bc-201">Nelze zadat, které chcete toouse hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-201">You cannot specify hello IP address that you want toouse.</span></span> <span data-ttu-id="4c7bc-202">Je dynamicky přidělené tooyour brány.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-202">It's dynamically allocated tooyour gateway.</span></span> 

  ```powershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
  ```
7. <span data-ttu-id="4c7bc-203">Vytvoření konfigurace brány hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-203">Create hello gateway configuration.</span></span> <span data-ttu-id="4c7bc-204">Hello konfigurace brány definuje podsíť hello a toouse hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-204">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="4c7bc-205">Příklad toocreate hello používejte vlastní konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-205">Use hello example toocreate your gateway configuration.</span></span>

  ```powershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```
8. <span data-ttu-id="4c7bc-206">Vytvoření hello brány pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-206">Create hello gateway for TestVNet1.</span></span> <span data-ttu-id="4c7bc-207">V tomto kroku vytvoříte bránu virtuální sítě hello pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-207">In this step, you create hello virtual network gateway for your TestVNet1.</span></span> <span data-ttu-id="4c7bc-208">Konfigurace propojení VNet-to-VNet vyžadují typ sítě VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-208">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="4c7bc-209">Vytvoření brány může trvat často 45 minut nebo déle, v závislosti na vybrané skladová položka brány hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-209">Creating a gateway can often take 45 minutes or more, depending on hello selected gateway SKU.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-3---create-and-configure-testvnet4"></a><span data-ttu-id="4c7bc-210">Krok 3: Vytvoření a konfigurace virtuální sítě TestVNet4</span><span class="sxs-lookup"><span data-stu-id="4c7bc-210">Step 3 - Create and configure TestVNet4</span></span>

<span data-ttu-id="4c7bc-211">Po konfiguraci virtuální sítě TestVNet1 vytvořte virtuální síť TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-211">Once you've configured TestVNet1, create TestVNet4.</span></span> <span data-ttu-id="4c7bc-212">Postupujte podle kroků hello níže nahraďte hello hodnoty vlastními v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-212">Follow hello steps below, replacing hello values with your own when needed.</span></span> <span data-ttu-id="4c7bc-213">Tento krok lze provést v rámci hello stejné relace prostředí PowerShell protože je v hello stejné předplatné.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-213">This step can be done within hello same PowerShell session because it is in hello same subscription.</span></span>

1. <span data-ttu-id="4c7bc-214">Deklarujte proměnné.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-214">Declare your variables.</span></span> <span data-ttu-id="4c7bc-215">Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-215">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

  ```powershell
  $RG4 = "TestRG4"
  $Location4 = "West US"
  $VnetName4 = "TestVNet4"
  $FESubName4 = "FrontEnd"
  $BESubName4 = "Backend"
  $GWSubName4 = "GatewaySubnet"
  $VnetPrefix41 = "10.41.0.0/16"
  $VnetPrefix42 = "10.42.0.0/16"
  $FESubPrefix4 = "10.41.0.0/24"
  $BESubPrefix4 = "10.42.0.0/24"
  $GWSubPrefix4 = "10.42.255.0/27"
  $GWName4 = "VNet4GW"
  $GWIPName4 = "VNet4GWIP"
  $GWIPconfName4 = "gwipconf4"
  $Connection41 = "VNet4toVNet1"
  ```
2. <span data-ttu-id="4c7bc-216">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-216">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG4 -Location $Location4
  ```
3. <span data-ttu-id="4c7bc-217">Vytvoření konfigurací podsítě pro virtuální síť TestVNet4 hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-217">Create hello subnet configurations for TestVNet4.</span></span>

  ```powershell
  $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
  $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
  $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4
  ```
4. <span data-ttu-id="4c7bc-218">Vytvořte virtuální síť TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-218">Create TestVNet4.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4
  ```
5. <span data-ttu-id="4c7bc-219">Vyžádejte si veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-219">Request a public IP address.</span></span>

  ```powershell
  $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AllocationMethod Dynamic
  ```
6. <span data-ttu-id="4c7bc-220">Vytvoření konfigurace brány hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-220">Create hello gateway configuration.</span></span>

  ```powershell
  $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
  $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
  $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4
  ```
7. <span data-ttu-id="4c7bc-221">Vytvoření brány virtuální sítě TestVNet4 hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-221">Create hello TestVNet4 gateway.</span></span> <span data-ttu-id="4c7bc-222">Vytvoření brány může trvat často 45 minut nebo déle, v závislosti na vybrané skladová položka brány hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-222">Creating a gateway can often take 45 minutes or more, depending on hello selected gateway SKU.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
  -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-4---create-hello-connections"></a><span data-ttu-id="4c7bc-223">Krok 4 – vytvoření připojení hello</span><span class="sxs-lookup"><span data-stu-id="4c7bc-223">Step 4 - Create hello connections</span></span>

1. <span data-ttu-id="4c7bc-224">Získejte obě brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-224">Get both virtual network gateways.</span></span> <span data-ttu-id="4c7bc-225">Pokud jsou obě brány hello v hello stejného předplatného, jako v příkladu hello, můžete použít tento krok v hello stejné relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-225">If both of hello gateways are in hello same subscription, as they are in hello example, you can complete this step in hello same PowerShell session.</span></span>

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4
  ```
2. <span data-ttu-id="4c7bc-226">Vytvořte připojení tooTestVNet4 hello virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-226">Create hello TestVNet1 tooTestVNet4 connection.</span></span> <span data-ttu-id="4c7bc-227">V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet4.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-227">In this step, you create hello connection from TestVNet1 tooTestVNet4.</span></span> <span data-ttu-id="4c7bc-228">Zobrazí se sdílený klíč uváděný v příkladech hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-228">You'll see a shared key referenced in hello examples.</span></span> <span data-ttu-id="4c7bc-229">Můžete použít vlastní hodnoty pro sdílený klíč hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-229">You can use your own values for hello shared key.</span></span> <span data-ttu-id="4c7bc-230">pro obě připojení musí shodovat Hello důležité věc, je tento sdílený klíč hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-230">hello important thing is that hello shared key must match for both connections.</span></span> <span data-ttu-id="4c7bc-231">Vytvoření připojení může trvat malou chvíli toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-231">Creating a connection can take a short while toocomplete.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
3. <span data-ttu-id="4c7bc-232">Vytvořte připojení tooTestVNet1 virtuální sítě TestVNet4 hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-232">Create hello TestVNet4 tooTestVNet1 connection.</span></span> <span data-ttu-id="4c7bc-233">Tento krok je podobný toohello jeden vyšší, s výjimkou toho, kterou vytváříte hello připojení z virtuální sítě TestVNet4 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-233">This step is similar toohello one above, except you are creating hello connection from TestVNet4 tooTestVNet1.</span></span> <span data-ttu-id="4c7bc-234">Zkontrolujte, zda text hello sdílené klíče shodují.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-234">Make sure hello shared keys match.</span></span> <span data-ttu-id="4c7bc-235">Po několika minutách bude vytvořeno připojení Hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-235">hello connection will be established after a few minutes.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
  -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. <span data-ttu-id="4c7bc-236">Ověřte své propojení.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-236">Verify your connection.</span></span> <span data-ttu-id="4c7bc-237">Části hello [jak tooverify připojení](#verify).</span><span class="sxs-lookup"><span data-stu-id="4c7bc-237">See hello section [How tooverify your connection](#verify).</span></span>

## <span data-ttu-id="4c7bc-238"><a name="difsub"></a>Jak tooconnect virtuální sítě, jsou v různých předplatných</span><span class="sxs-lookup"><span data-stu-id="4c7bc-238"><a name="difsub"></a>How tooconnect VNets that are in different subscriptions</span></span>

![Diagram v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

<span data-ttu-id="4c7bc-240">V tomto scénáři propojíme sítě TestVNet1 a TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-240">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="4c7bc-241">Virtuální sítě TestVNet1 a TestVNet5 patří do různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-241">TestVNet1 and TestVNet5 reside in a different subscription.</span></span> <span data-ttu-id="4c7bc-242">odběry Hello nemusí toobe přidružené hello stejné klienta služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-242">hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> <span data-ttu-id="4c7bc-243">Hello rozdíl mezi tyto kroky a předchozí sadu hello je, že některé z kroků konfigurace hello nutné toobe provést v samostatné relaci prostředí PowerShell v kontextu hello hello druhého předplatného.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-243">hello difference between these steps and hello previous set is that some of hello configuration steps need toobe performed in a separate PowerShell session in hello context of hello second subscription.</span></span> <span data-ttu-id="4c7bc-244">Zejména při hello dva předplatná patří toodifferent organizace.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-244">Especially when hello two subscriptions belong toodifferent organizations.</span></span>

### <a name="step-5---create-and-configure-testvnet1"></a><span data-ttu-id="4c7bc-245">Krok 5: Vytvoření a konfigurace virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="4c7bc-245">Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="4c7bc-246">Je třeba provést [kroku 1](#Step1) a [kroku 2](#Step2) z hello předchozí část toocreate a konfigurace virtuální sítě TestVNet1 a hello brána sítě VPN pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-246">You must complete [Step 1](#Step1) and [Step 2](#Step2) from hello previous section toocreate and configure TestVNet1 and hello VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="4c7bc-247">Pro tuto konfiguraci nemůžete se vyžaduje toocreate virtuální sítě TestVNet4 z předchozí části hello, ale pokud ho vytvoříte, se nebude v konfliktu s tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-247">For this configuration, you are not required toocreate TestVNet4 from hello previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="4c7bc-248">Po dokončení kroku 1 a 2 kroku v kroku 6 toocreate virtuální sítě TestVNet5 pokračujte.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-248">Once you complete Step 1 and Step 2, continue with Step 6 toocreate TestVNet5.</span></span> 

### <a name="step-6---verify-hello-ip-address-ranges"></a><span data-ttu-id="4c7bc-249">Krok 6 – ověření hello rozsahy IP adres</span><span class="sxs-lookup"><span data-stu-id="4c7bc-249">Step 6 - Verify hello IP address ranges</span></span>

<span data-ttu-id="4c7bc-250">Je důležité toomake jistotu, že prostor IP adres hello nové virtuální sítě TestVNet5 hello nepřekrývá s žádným z rozsahů virtuálních sítí ani rozsahů bran místních sítí.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-250">It is important toomake sure that hello IP address space of hello new virtual network, TestVNet5, does not overlap with any of your VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="4c7bc-251">V tomto příkladu hello virtuální sítě může patřit toodifferent organizace.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-251">In this example, hello virtual networks may belong toodifferent organizations.</span></span> <span data-ttu-id="4c7bc-252">Pro tento postup můžete použít následující hodnoty pro hello virtuální sítě TestVNet5 hello:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-252">For this exercise, you can use hello following values for hello TestVNet5:</span></span>

<span data-ttu-id="4c7bc-253">**Hodnoty pro virtuální síť TestVNet5:**</span><span class="sxs-lookup"><span data-stu-id="4c7bc-253">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="4c7bc-254">Název virtuální sítě: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="4c7bc-254">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="4c7bc-255">Skupina prostředků: TestRG5</span><span class="sxs-lookup"><span data-stu-id="4c7bc-255">Resource Group: TestRG5</span></span>
* <span data-ttu-id="4c7bc-256">Umístění: Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="4c7bc-256">Location: Japan East</span></span>
* <span data-ttu-id="4c7bc-257">TestVNet5: 10.51.0.0/16 a 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="4c7bc-257">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="4c7bc-258">FrontEnd: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="4c7bc-258">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="4c7bc-259">BackEnd: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="4c7bc-259">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="4c7bc-260">GatewaySubnet: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="4c7bc-260">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="4c7bc-261">Název brány: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="4c7bc-261">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="4c7bc-262">Veřejná IP adresa: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="4c7bc-262">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="4c7bc-263">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="4c7bc-263">VPNType: RouteBased</span></span>
* <span data-ttu-id="4c7bc-264">Připojení: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="4c7bc-264">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="4c7bc-265">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="4c7bc-265">ConnectionType: VNet2VNet</span></span>

### <a name="step-7---create-and-configure-testvnet5"></a><span data-ttu-id="4c7bc-266">Krok 7: Vytvoření a konfigurace virtuální sítě TestVNet5</span><span class="sxs-lookup"><span data-stu-id="4c7bc-266">Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="4c7bc-267">Tento krok je třeba provést v kontextu hello hello nové předplatné.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-267">This step must be done in hello context of hello new subscription.</span></span> <span data-ttu-id="4c7bc-268">Tuto část může provést pomocí Správce hello v jiné organizaci, který vlastní hello předplatné.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-268">This part may be performed by hello administrator in a different organization that owns hello subscription.</span></span>

1. <span data-ttu-id="4c7bc-269">Deklarujte proměnné.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-269">Declare your variables.</span></span> <span data-ttu-id="4c7bc-270">Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-270">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

  ```powershell
  $Sub5 = "Replace_With_the_New_Subcription_Name"
  $RG5 = "TestRG5"
  $Location5 = "Japan East"
  $VnetName5 = "TestVNet5"
  $FESubName5 = "FrontEnd"
  $BESubName5 = "Backend"
  $GWSubName5 = "GatewaySubnet"
  $VnetPrefix51 = "10.51.0.0/16"
  $VnetPrefix52 = "10.52.0.0/16"
  $FESubPrefix5 = "10.51.0.0/24"
  $BESubPrefix5 = "10.52.0.0/24"
  $GWSubPrefix5 = "10.52.255.0/27"
  $GWName5 = "VNet5GW"
  $GWIPName5 = "VNet5GWIP"
  $GWIPconfName5 = "gwipconf5"
  $Connection51 = "VNet5toVNet1"
  ```
2. <span data-ttu-id="4c7bc-271">Připojte toosubscription 5.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-271">Connect toosubscription 5.</span></span> <span data-ttu-id="4c7bc-272">Otevřete konzolu prostředí PowerShell a připojte tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-272">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="4c7bc-273">Použijte následující ukázka toohelp, ke kterým se připojujete hello:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-273">Use hello following sample toohelp you connect:</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="4c7bc-274">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-274">Check hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

  <span data-ttu-id="4c7bc-275">Zadejte hello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-275">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub5
  ```
3. <span data-ttu-id="4c7bc-276">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-276">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG5 -Location $Location5
  ```
4. <span data-ttu-id="4c7bc-277">Vytvoření konfigurací podsítě pro virtuální sítě TestVNet5 hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-277">Create hello subnet configurations for TestVNet5.</span></span>

  ```powershell
  $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
  $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
  $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5
  ```
5. <span data-ttu-id="4c7bc-278">Vytvořte virtuální síť TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-278">Create TestVNet5.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
  -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5
  ```
6. <span data-ttu-id="4c7bc-279">Vyžádejte si veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-279">Request a public IP address.</span></span>

  ```powershell
  $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
  -Location $Location5 -AllocationMethod Dynamic
  ```
7. <span data-ttu-id="4c7bc-280">Vytvoření konfigurace brány hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-280">Create hello gateway configuration.</span></span>

  ```powershell
  $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
  $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
  $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5
  ```
8. <span data-ttu-id="4c7bc-281">Vytvoření brány virtuální sítě TestVNet5 hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-281">Create hello TestVNet5 gateway.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
  -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-8---create-hello-connections"></a><span data-ttu-id="4c7bc-282">Krok 8 – vytvoření připojení hello</span><span class="sxs-lookup"><span data-stu-id="4c7bc-282">Step 8 - Create hello connections</span></span>

<span data-ttu-id="4c7bc-283">V tomto příkladu protože hello brány jsou v hello různých předplatných, rozdělíme tento krok do dvou relací prostředí PowerShell označených [předplatné 1] a [předplatné 5].</span><span class="sxs-lookup"><span data-stu-id="4c7bc-283">In this example, because hello gateways are in hello different subscriptions, we've split this step into two PowerShell sessions marked as [Subscription 1] and [Subscription 5].</span></span>

1. <span data-ttu-id="4c7bc-284">**[Předplatné 1]**  Get hello brány virtuální sítě pro předplatné 1.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-284">**[Subscription 1]** Get hello virtual network gateway for Subscription 1.</span></span> <span data-ttu-id="4c7bc-285">Přihlaste se a připojit tooSubscription 1 před spuštěním hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-285">Log in and connect tooSubscription 1 before running hello following example:</span></span>

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  ```

  <span data-ttu-id="4c7bc-286">Zkopírujte výstup hello hello následujících prvků a pošlete tyto toohello správci předplatného 5 prostřednictvím e-mailu nebo jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-286">Copy hello output of hello following elements and send these toohello administrator of Subscription 5 via email or another method.</span></span>

  ```powershell
  $vnet1gw.Name
  $vnet1gw.Id
  ```

  <span data-ttu-id="4c7bc-287">Tyto dva prvky budou mít hodnoty podobné toohello následující příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-287">These two elements will have values similar toohello following example output:</span></span>

  ```
  PS D:\> $vnet1gw.Name
  VNet1GW
  PS D:\> $vnet1gw.Id
  /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```
2. <span data-ttu-id="4c7bc-288">**[Předplatné 5]**  Get hello brány virtuální sítě pro předplatné 5.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-288">**[Subscription 5]** Get hello virtual network gateway for Subscription 5.</span></span> <span data-ttu-id="4c7bc-289">Přihlaste se a připojit tooSubscription 5 před spuštěním hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-289">Log in and connect tooSubscription 5 before running hello following example:</span></span>

  ```powershell
  $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5
  ```

  <span data-ttu-id="4c7bc-290">Zkopírujte výstup hello hello následující prvky a odesílat tyto toohello správci předplatného 1 prostřednictvím e-mailu nebo jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-290">Copy hello output of hello following elements and send these toohello administrator of Subscription 1 via email or another method.</span></span>

  ```powershell
  $vnet5gw.Name
  $vnet5gw.Id
  ```

  <span data-ttu-id="4c7bc-291">Tyto dva prvky budou mít hodnoty podobné toohello následující příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-291">These two elements will have values similar toohello following example output:</span></span>

  ```
  PS C:\> $vnet5gw.Name
  VNet5GW
  PS C:\> $vnet5gw.Id
  /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```
3. <span data-ttu-id="4c7bc-292">**[Předplatné 1]**  Vytvořit připojení hello tooTestVNet5 virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-292">**[Subscription 1]** Create hello TestVNet1 tooTestVNet5 connection.</span></span> <span data-ttu-id="4c7bc-293">V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet5.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-293">In this step, you create hello connection from TestVNet1 tooTestVNet5.</span></span> <span data-ttu-id="4c7bc-294">Hello rozdíl zde spočívá v tom že $vnet5gw nelze získat přímo, protože je v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-294">hello difference here is that $vnet5gw cannot be obtained directly because it is in a different subscription.</span></span> <span data-ttu-id="4c7bc-295">Budete potřebovat toocreate nový objekt prostředí PowerShell s hello hodnotami zjištěnými z předplatného 1 v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-295">You will need toocreate a new PowerShell object with hello values communicated from Subscription 1 in hello steps above.</span></span> <span data-ttu-id="4c7bc-296">Použijte následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-296">Use hello example below.</span></span> <span data-ttu-id="4c7bc-297">Nahraďte hello název, Id a sdílený klíč vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-297">Replace hello Name, Id, and shared key with your own values.</span></span> <span data-ttu-id="4c7bc-298">pro obě připojení musí shodovat Hello důležité věc, je tento sdílený klíč hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-298">hello important thing is that hello shared key must match for both connections.</span></span> <span data-ttu-id="4c7bc-299">Vytvoření připojení může trvat malou chvíli toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-299">Creating a connection can take a short while toocomplete.</span></span>

  <span data-ttu-id="4c7bc-300">Připojte tooSubscription 1 před spuštěním hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-300">Connect tooSubscription 1 before running hello following example:</span></span>

  ```powershell
  $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet5gw.Name = "VNet5GW"
  $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
  $Connection15 = "VNet1toVNet5"
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. <span data-ttu-id="4c7bc-301">**[Předplatné 5]**  Vytvoření připojení virtuální sítě TestVNet5 tooTestVNet1 hello.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-301">**[Subscription 5]** Create hello TestVNet5 tooTestVNet1 connection.</span></span> <span data-ttu-id="4c7bc-302">Tento krok je podobný toohello jeden vyšší, s výjimkou toho, kterou vytváříte hello připojení z virtuální sítě TestVNet5 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-302">This step is similar toohello one above, except you are creating hello connection from TestVNet5 tooTestVNet1.</span></span> <span data-ttu-id="4c7bc-303">Hello stejný postup vytváření objektu prostředí PowerShell na základě hello hodnot zjištěných z předplatného 1 používá i zde.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-303">hello same process of creating a PowerShell object based on hello values obtained from Subscription 1 applies here as well.</span></span> <span data-ttu-id="4c7bc-304">V tomto kroku se ujistěte, že hello sdílené klíče shodují.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-304">In this step, be sure that hello shared keys match.</span></span>

  <span data-ttu-id="4c7bc-305">Připojte tooSubscription 5 před spuštěním hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="4c7bc-305">Connect tooSubscription 5 before running hello following example:</span></span>

  ```powershell
  $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet1gw.Name = "VNet1GW"
  $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

## <span data-ttu-id="4c7bc-306"><a name="verify"></a>Jak tooverify připojení</span><span class="sxs-lookup"><span data-stu-id="4c7bc-306"><a name="verify"></a>How tooverify a connection</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <span data-ttu-id="4c7bc-307"><a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="4c7bc-307"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="4c7bc-308">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c7bc-308">Next steps</span></span>

* <span data-ttu-id="4c7bc-309">Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-309">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="4c7bc-310">V tématu hello [virtuální počítače dokumentaci](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4c7bc-310">See hello [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) for more information.</span></span>
* <span data-ttu-id="4c7bc-311">Informace o protokolu BGP najdete v tématu hello [přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [jak tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4c7bc-311">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
