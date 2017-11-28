---
title: "Připojit virtuální síť tooanother virtuální sítě: rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek vás provede propojováním virtuálních sítí s použitím Azure Resource Manageru a Azure CLI."
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
ms.openlocfilehash: 70113914bcae03c80f9ad133ff081d1cf37fc309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-azure-cli"></a><span data-ttu-id="6c787-103">Konfigurace připojení brány VPN typu VNet-to-VNet pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6c787-103">Configure a VNet-to-VNet VPN gateway connection using Azure CLI</span></span>

<span data-ttu-id="6c787-104">Tento článek ukazuje, jak toocreate připojení k bráně VPN mezi virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="6c787-104">This article shows you how toocreate a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="6c787-105">Hello virtuální sítě může být v hello stejné nebo různých oblastí, a z hello stejné nebo různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="6c787-105">hello virtual networks can be in hello same or different regions, and from hello same or different subscriptions.</span></span> <span data-ttu-id="6c787-106">Při připojování virtuální sítě z různých předplatných, odběry hello nemusí toobe přidružené hello stejné klienta služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6c787-106">When connecting VNets from different subscriptions, hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> 

<span data-ttu-id="6c787-107">Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager a používat rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6c787-107">hello steps in this article apply toohello Resource Manager deployment model and use Azure CLI.</span></span> <span data-ttu-id="6c787-108">Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="6c787-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6c787-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6c787-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="6c787-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c787-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="6c787-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6c787-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="6c787-112">Azure Portal (Classic)</span><span class="sxs-lookup"><span data-stu-id="6c787-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="6c787-113">Propojení různých modelů nasazení – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6c787-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="6c787-114">Propojení různých modelů nasazení – PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c787-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="6c787-115">Propojení virtuální sítě tooanother virtuální síť (VNet-to-VNet) je podobné tooconnecting umístění lokality tooan místní virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="6c787-115">Connecting a virtual network tooanother virtual network (VNet-to-VNet) is similar tooconnecting a VNet tooan on-premises site location.</span></span> <span data-ttu-id="6c787-116">Oba typy připojení využívají bránu tooprovide sítě VPN přes zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="6c787-116">Both connectivity types use a VPN gateway tooprovide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="6c787-117">Pokud vaše virtuální sítě jsou v hello stejné oblasti, může být vhodné tooconsider připojení pomocí virtuální sítě partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6c787-117">If your VNets are in hello same region, you may want tooconsider connecting them using VNet Peering.</span></span> <span data-ttu-id="6c787-118">Partnerské vztahy virtuálních sítí nepoužívají bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="6c787-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="6c787-119">Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6c787-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="6c787-120">Komunikaci typu VNet-to-VNet můžete kombinovat s konfiguracemi s více servery.</span><span class="sxs-lookup"><span data-stu-id="6c787-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="6c787-121">To umožňuje vytvářet topologie sítí, které spojují připojení mezi různými místy s připojením propojování virtuálních sítí, jak je znázorněno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="6c787-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in hello following diagram:</span></span>

![Informace o připojeních](./media/vpn-gateway-howto-vnet-vnet-cli/aboutconnections.png)

### <span data-ttu-id="6c787-123"><a name="why"></a>Proč propojovat virtuální sítě?</span><span class="sxs-lookup"><span data-stu-id="6c787-123"><a name="why"></a>Why connect virtual networks?</span></span>

<span data-ttu-id="6c787-124">Tooconnect virtuální sítě může být vhodné pro hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="6c787-124">You may want tooconnect virtual networks for hello following reasons:</span></span>

* <span data-ttu-id="6c787-125">**Geografická redundance napříč oblastmi a geografická přítomnost**</span><span class="sxs-lookup"><span data-stu-id="6c787-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="6c787-126">Můžete nastavit vlastní geografickou replikaci nebo synchronizaci se zabezpečeným připojením bez procházení koncovými body připojenými k internetu.</span><span class="sxs-lookup"><span data-stu-id="6c787-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="6c787-127">Pomocí Azure Traffic Manageru a služby Load Balancer je možné vytvářet úlohy s vysokou dostupností s geografickou redundancí nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="6c787-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="6c787-128">Jedním z důležitých příkladů je tooset až SQL Always On se skupinami dostupnosti nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="6c787-128">One important example is tooset up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="6c787-129">**Regionální vícevrstvé aplikace s izolací nebo administrativní hranicí**</span><span class="sxs-lookup"><span data-stu-id="6c787-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="6c787-130">Hello uvnitř stejné oblasti, můžete nastavit vícevrstvé aplikace s několika virtuálními sítěmi propojenými z důvodu tooisolation nebo požadavků na správu.</span><span class="sxs-lookup"><span data-stu-id="6c787-130">Within hello same region, you can set up multi-tier applications with multiple virtual networks connected together due tooisolation or administrative requirements.</span></span>

<span data-ttu-id="6c787-131">Další informace o připojení VNet-to-VNet, najdete v části hello [nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="6c787-131">For more information about VNet-to-VNet connections, see hello [VNet-to-VNet FAQ](#faq) at hello end of this article.</span></span>

### <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="6c787-132">Kterou posloupnost kroků provést?</span><span class="sxs-lookup"><span data-stu-id="6c787-132">Which set of steps should I use?</span></span>

<span data-ttu-id="6c787-133">V tomto článku uvidíte dvě různé sady kroků.</span><span class="sxs-lookup"><span data-stu-id="6c787-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="6c787-134">Jednu sadu kroky pro [hello virtuální sítě, které jsou umístěny ve stejné předplatné](#samesub)a druhý pro [patřící do různých předplatných](#difsub).</span><span class="sxs-lookup"><span data-stu-id="6c787-134">One set of steps for [VNets that reside in hello same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span>

## <span data-ttu-id="6c787-135"><a name="samesub"></a>Propojení virtuálních sítí, které jsou v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="6c787-135"><a name="samesub"></a>Connect VNets that are in hello same subscription</span></span>

![Diagram v2v](./media/vpn-gateway-howto-vnet-vnet-cli/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="6c787-137">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6c787-137">Before you begin</span></span>

<span data-ttu-id="6c787-138">Než začnete, nainstalujte nejnovější verzi hello hello rozhraní příkazového řádku (2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="6c787-138">Before beginning, install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="6c787-139">Informace o instalaci hello rozhraní příkazového řádku najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6c787-139">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

### <span data-ttu-id="6c787-140"><a name="Plan"></a>Plánování rozsahů IP adres</span><span class="sxs-lookup"><span data-stu-id="6c787-140"><a name="Plan"></a>Plan your IP address ranges</span></span>

<span data-ttu-id="6c787-141">V hello následující kroky vytvoříme dvě virtuální sítě spolu s jejich příslušnými podsítěmi brány a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6c787-141">In hello following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="6c787-142">Poté vytvoříme připojení VPN mezi hello dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6c787-142">We then create a VPN connection between hello two VNets.</span></span> <span data-ttu-id="6c787-143">Je důležité tooplan rozsahy IP adres hello pro konfiguraci sítě.</span><span class="sxs-lookup"><span data-stu-id="6c787-143">It’s important tooplan hello IP address ranges for your network configuration.</span></span> <span data-ttu-id="6c787-144">Mějte na paměti, že je třeba zajistit, aby se žádné rozsahy virtuálních sítí ani místní síťové rozsahy žádným způsobem nepřekrývaly.</span><span class="sxs-lookup"><span data-stu-id="6c787-144">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="6c787-145">V těchto příkladech nezahrnujeme server DNS.</span><span class="sxs-lookup"><span data-stu-id="6c787-145">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="6c787-146">Pokud chcete překlad IP adres pro virtuální sítě, přečtěte si téma [Překlad IP adres](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="6c787-146">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="6c787-147">Můžeme použít následující hodnoty v příkladech hello hello:</span><span class="sxs-lookup"><span data-stu-id="6c787-147">We use hello following values in hello examples:</span></span>

<span data-ttu-id="6c787-148">**Hodnoty pro virtuální síť TestVNet1:**</span><span class="sxs-lookup"><span data-stu-id="6c787-148">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="6c787-149">Název virtuální sítě: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="6c787-149">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="6c787-150">Skupina prostředků: TestRG1</span><span class="sxs-lookup"><span data-stu-id="6c787-150">Resource Group: TestRG1</span></span>
* <span data-ttu-id="6c787-151">Umístění: Východní USA</span><span class="sxs-lookup"><span data-stu-id="6c787-151">Location: East US</span></span>
* <span data-ttu-id="6c787-152">TestVNet1: 10.11.0.0/16 a 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6c787-152">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="6c787-153">FrontEnd: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="6c787-153">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="6c787-154">BackEnd: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="6c787-154">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="6c787-155">GatewaySubnet: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="6c787-155">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="6c787-156">Název brány: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="6c787-156">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="6c787-157">Veřejná IP adresa: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="6c787-157">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="6c787-158">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="6c787-158">VPNType: RouteBased</span></span>
* <span data-ttu-id="6c787-159">Připojení (1 ke 4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="6c787-159">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="6c787-160">Připojení (1 k 5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="6c787-160">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="6c787-161">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="6c787-161">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="6c787-162">**Hodnoty pro virtuální síť TestVNet4:**</span><span class="sxs-lookup"><span data-stu-id="6c787-162">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="6c787-163">Název virtuální sítě: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="6c787-163">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="6c787-164">TestVNet2: 10.41.0.0/16 a 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6c787-164">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="6c787-165">FrontEnd: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="6c787-165">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="6c787-166">BackEnd: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="6c787-166">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="6c787-167">GatewaySubnet: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="6c787-167">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="6c787-168">Skupina prostředků: TestRG4</span><span class="sxs-lookup"><span data-stu-id="6c787-168">Resource Group: TestRG4</span></span>
* <span data-ttu-id="6c787-169">Umístění: Západní USA</span><span class="sxs-lookup"><span data-stu-id="6c787-169">Location: West US</span></span>
* <span data-ttu-id="6c787-170">Název brány: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="6c787-170">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="6c787-171">Veřejná IP adresa: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="6c787-171">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="6c787-172">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="6c787-172">VPNType: RouteBased</span></span>
* <span data-ttu-id="6c787-173">Připojení: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="6c787-173">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="6c787-174">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="6c787-174">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="6c787-175"><a name="Connect"></a>Krok 1 – připojení tooyour odběru</span><span class="sxs-lookup"><span data-stu-id="6c787-175"><a name="Connect"></a>Step 1 - Connect tooyour subscription</span></span>

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-numbers-include.md)]

### <span data-ttu-id="6c787-176"><a name="TestVNet1"></a>Krok 2: Vytvoření a konfigurace virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="6c787-176"><a name="TestVNet1"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="6c787-177">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6c787-177">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG1  -l eastus
  ```
2. <span data-ttu-id="6c787-178">Vytvořte virtuální síť TestVNet1 a hello podsítě pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c787-178">Create TestVNet1 and hello subnets for TestVNet1.</span></span> <span data-ttu-id="6c787-179">Tento příklad vytvoří virtuální síť TestVNet1 a podsíť FrontEnd.</span><span class="sxs-lookup"><span data-stu-id="6c787-179">This example creates a virtual network named TestVNet1 and a subnet named FrontEnd.</span></span>

  ```azurecli
  az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24
  ```
3. <span data-ttu-id="6c787-180">Vytvořte další adresní prostor pro podsíť hello back-end.</span><span class="sxs-lookup"><span data-stu-id="6c787-180">Create an additional address space for hello backend subnet.</span></span> <span data-ttu-id="6c787-181">Všimněte si, že v tomto kroku jsme zadejte oba hello adresní prostor, který jsme vytvořili předtím a hello další adresní prostor, že má být tooadd.</span><span class="sxs-lookup"><span data-stu-id="6c787-181">Notice that in this step, we specify both hello address space that we created earlier, and hello additional address space that we want tooadd.</span></span> <span data-ttu-id="6c787-182">Důvodem je, že hello [aktualizace az sítě vnet](https://docs.microsoft.com/cli/azure/network/vnet#update) příkaz přepíše hello předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="6c787-182">This is because hello [az network vnet update](https://docs.microsoft.com/cli/azure/network/vnet#update) command overwrites hello previous settings.</span></span> <span data-ttu-id="6c787-183">Zajistěte, aby toospecify všechny předpon adres hello při použití tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="6c787-183">Make sure toospecify all of hello address prefixes when using this command.</span></span>

  ```azurecli
  az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestRG1
  ```
4. <span data-ttu-id="6c787-184">Vytvořte podsíť hello back-end.</span><span class="sxs-lookup"><span data-stu-id="6c787-184">Create hello backend subnet.</span></span>
  
  ```azurecli
  az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestRG1 --address-prefix 10.12.0.0/24 
  ```
5. <span data-ttu-id="6c787-185">Vytvořte podsíť brány hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-185">Create hello gateway subnet.</span></span> <span data-ttu-id="6c787-186">Všimněte si, že tuto podsíť brány hello je s názvem "GatewaySubnet".</span><span class="sxs-lookup"><span data-stu-id="6c787-186">Notice that hello gateway subnet is named 'GatewaySubnet'.</span></span> <span data-ttu-id="6c787-187">Název je povinný.</span><span class="sxs-lookup"><span data-stu-id="6c787-187">This name is required.</span></span> <span data-ttu-id="6c787-188">V tomto příkladu používá podsíť brány hello 27.</span><span class="sxs-lookup"><span data-stu-id="6c787-188">In this example, hello gateway subnet is using a /27.</span></span> <span data-ttu-id="6c787-189">I když je možné toocreate podsíť brány jako malé/29, doporučujeme vytvořit větší podsíť, která zahrnuje víc adres výběrem minimálně/28 nebo /27.</span><span class="sxs-lookup"><span data-stu-id="6c787-189">While it is possible toocreate a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="6c787-190">To vám umožní dostatek adresy tooaccommodate možné další konfigurace, které můžete ve hello budoucí.</span><span class="sxs-lookup"><span data-stu-id="6c787-190">This will allow for enough addresses tooaccommodate possible additional configurations that you may want in hello future.</span></span>

  ```azurecli 
  az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestRG1 --address-prefix 10.12.255.0/27
  ```
6. <span data-ttu-id="6c787-191">Požádat o veřejné IP adresy toobe přidělené toohello bránu, které vytvoříte pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="6c787-191">Request a public IP address toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="6c787-192">Všimněte si, že hello AllocationMethod je dynamický.</span><span class="sxs-lookup"><span data-stu-id="6c787-192">Notice that hello AllocationMethod is Dynamic.</span></span> <span data-ttu-id="6c787-193">Nelze zadat, které chcete toouse hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6c787-193">You cannot specify hello IP address that you want toouse.</span></span> <span data-ttu-id="6c787-194">Je dynamicky přidělené tooyour brány.</span><span class="sxs-lookup"><span data-stu-id="6c787-194">It's dynamically allocated tooyour gateway.</span></span>

  ```azurecli
  az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
  ```
7. <span data-ttu-id="6c787-195">Vytvoření brány virtuální sítě hello pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c787-195">Create hello virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="6c787-196">Konfigurace propojení VNet-to-VNet vyžadují typ sítě VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="6c787-196">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="6c787-197">Pokud spustíte tento příkaz pomocí hello parametr '– žádné - wait', se nezobrazí žádné zpětnou vazbu nebo výstup.</span><span class="sxs-lookup"><span data-stu-id="6c787-197">If you run this command using hello '--no-wait' parameter, you don't see any feedback or output.</span></span> <span data-ttu-id="6c787-198">Hello '– žádné - wait' parametr umožňuje toocreate hello brány v pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-198">hello '--no-wait' parameter allows hello gateway toocreate in hello background.</span></span> <span data-ttu-id="6c787-199">Neznamená to brány sítě VPN hello dokončí vytváření okamžitě.</span><span class="sxs-lookup"><span data-stu-id="6c787-199">It does not mean that hello VPN gateway finishes creating immediately.</span></span> <span data-ttu-id="6c787-200">Vytvoření brány může trvat často 45 minut nebo déle, v závislosti na hello SKU brány, můžete použít.</span><span class="sxs-lookup"><span data-stu-id="6c787-200">Creating a gateway can often take 45 minutes or more, depending on hello gateway SKU that you use.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="6c787-201"><a name="TestVNet4"></a>Krok 3: Vytvoření a konfigurace virtuální sítě TestVNet4</span><span class="sxs-lookup"><span data-stu-id="6c787-201"><a name="TestVNet4"></a>Step 3 - Create and configure TestVNet4</span></span>

1. <span data-ttu-id="6c787-202">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6c787-202">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG4  -l westus
  ```
2. <span data-ttu-id="6c787-203">Vytvořte virtuální síť TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="6c787-203">Create TestVNet4.</span></span>

  ```azurecli
  az network vnet create -n TestVNet4 -g TestRG4 --address-prefix 10.41.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.41.0.0/24
  ```

3. <span data-ttu-id="6c787-204">Vytvořte další podsítě pro virtuální síť TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="6c787-204">Create additional subnets for TestVNet4.</span></span>

  ```azurecli
  az network vnet update -n TestVNet4 --address-prefixes 10.41.0.0/16 10.42.0.0/16 -g TestRG4 
  az network vnet subnet create --vnet-name TestVNet4 -n BackEnd -g TestRG4 --address-prefix 10.42.0.0/24 
  ```
4. <span data-ttu-id="6c787-205">Vytvořte podsíť brány hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-205">Create hello gateway subnet.</span></span>

  ```azurecli
   az network vnet subnet create --vnet-name TestVNet4 -n GatewaySubnet -g TestRG4 --address-prefix 10.42.255.0/27
  ```
5. <span data-ttu-id="6c787-206">Vyžádejte si veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6c787-206">Request a Public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet4GWIP -g TestRG4 --allocation-method Dynamic
  ```
6. <span data-ttu-id="6c787-207">Vytvoření brány virtuální sítě TestVNet4 hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-207">Create hello TestVNet4 virtual network gateway.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet4GW -l westus --public-ip-address VNet4GWIP -g TestRG4 --vnet TestVNet4 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="6c787-208"><a name="createconnect"></a>Krok 4 – vytvoření připojení hello</span><span class="sxs-lookup"><span data-stu-id="6c787-208"><a name="createconnect"></a>Step 4 - Create hello connections</span></span>

<span data-ttu-id="6c787-209">Nyní máte dvě virtuální sítě s bránami VPN.</span><span class="sxs-lookup"><span data-stu-id="6c787-209">You now have two VNets with VPN gateways.</span></span> <span data-ttu-id="6c787-210">dalším krokem Hello je toocreate připojení brány VPN mezi hello brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6c787-210">hello next step is toocreate VPN gateway connections between hello virtual network gateways.</span></span> <span data-ttu-id="6c787-211">Pokud jste použili výše uvedených příkladech hello, vaší brány virtuální sítě jsou v různých skupinách prostředků.</span><span class="sxs-lookup"><span data-stu-id="6c787-211">If you used hello examples above, your VNet gateways are in different resource groups.</span></span> <span data-ttu-id="6c787-212">Když jsou brány v různých skupinách prostředků, třeba tooidentify a při navazování připojení zadat ID hello prostředků pro každou bránu.</span><span class="sxs-lookup"><span data-stu-id="6c787-212">When gateways are in different resource groups, you need tooidentify and specify hello resource IDs for each gateway when making a connection.</span></span> <span data-ttu-id="6c787-213">Pokud vaše virtuální sítě jsou v hello stejnou skupinu prostředků, můžete použít hello [druhé sadě pokyny](#samerg) protože nepotřebujete ID toospecify hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="6c787-213">If your VNets are in hello same resource group, you can use hello [second set of instructions](#samerg) because you don't need toospecify hello resource IDs.</span></span>

### <span data-ttu-id="6c787-214"><a name="diffrg"></a>tooconnect virtuální sítě, které jsou umístěny v různých skupinách prostředků</span><span class="sxs-lookup"><span data-stu-id="6c787-214"><a name="diffrg"></a>tooconnect VNets that reside in different resource groups</span></span>

1. <span data-ttu-id="6c787-215">Získání hello prostředků ID VNet1GW z hello výstup hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6c787-215">Get hello Resource ID of VNet1GW from hello output of hello following command:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="6c787-216">Ve výstupu hello najde hello "id:" řádku.</span><span class="sxs-lookup"><span data-stu-id="6c787-216">In hello output, find hello "id:" line.</span></span> <span data-ttu-id="6c787-217">Hello hodnoty v uvozovkách hello jsou potřebné toocreate hello připojení v další části hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-217">hello values within hello quotes are needed toocreate hello connection in hello next section.</span></span> <span data-ttu-id="6c787-218">Zkopírujte tyto hodnoty tooa textový editor, například Poznámkový blok, takže můžete snadno vložit je při vytváření připojení.</span><span class="sxs-lookup"><span data-stu-id="6c787-218">Copy these values tooa text editor, such as Notepad, so that you can easily paste them when creating your connection.</span></span>

  <span data-ttu-id="6c787-219">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="6c787-219">Example output:</span></span>

  ```
  "activeActive": false, 
  "bgpSettings": { 
    "asn": 65515, 
    "bgpPeeringAddress": "10.12.255.30", 
    "peerWeight": 0 
   }, 
  "enableBgp": false, 
  "etag": "W/\"ecb42bc5-c176-44e1-802f-b0ce2962ac04\"", 
  "gatewayDefaultSite": null, 
  "gatewayType": "Vpn", 
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW", 
  "ipConfigurations":
  ```

  <span data-ttu-id="6c787-220">Zkopírujte hodnoty hello po **"id":** v rámci hello uvozovky.</span><span class="sxs-lookup"><span data-stu-id="6c787-220">Copy hello values after **"id":** within hello quotes.</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
 ```

2. <span data-ttu-id="6c787-221">Získáte hello prostředků ID VNet4GW a zkopírujte hello hodnoty tooa textový editor.</span><span class="sxs-lookup"><span data-stu-id="6c787-221">Get hello Resource ID of VNet4GW and copy hello values tooa text editor.</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet4GW -g TestRG4
  ```

3. <span data-ttu-id="6c787-222">Vytvořte připojení tooTestVNet4 hello virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c787-222">Create hello TestVNet1 tooTestVNet4 connection.</span></span> <span data-ttu-id="6c787-223">V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet4.</span><span class="sxs-lookup"><span data-stu-id="6c787-223">In this step, you create hello connection from TestVNet1 tooTestVNet4.</span></span> <span data-ttu-id="6c787-224">Není sdílený klíč uváděný v příkladech hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-224">There is a shared key referenced in hello examples.</span></span> <span data-ttu-id="6c787-225">Můžete použít vlastní hodnoty pro sdílený klíč hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-225">You can use your own values for hello shared key.</span></span> <span data-ttu-id="6c787-226">pro obě připojení musí shodovat Hello důležité věc, je tento sdílený klíč hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-226">hello important thing is that hello shared key must match for both connections.</span></span> <span data-ttu-id="6c787-227">Vytvoření připojení trvá malou chvíli toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6c787-227">Creating a connection takes a short while toocomplete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW 
  ```
4. <span data-ttu-id="6c787-228">Vytvořte připojení tooTestVNet1 virtuální sítě TestVNet4 hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-228">Create hello TestVNet4 tooTestVNet1 connection.</span></span> <span data-ttu-id="6c787-229">Tento krok je podobný toohello jeden vyšší, s výjimkou toho, kterou vytváříte hello připojení z virtuální sítě TestVNet4 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c787-229">This step is similar toohello one above, except you are creating hello connection from TestVNet4 tooTestVNet1.</span></span> <span data-ttu-id="6c787-230">Zkontrolujte, zda text hello sdílené klíče shodují.</span><span class="sxs-lookup"><span data-stu-id="6c787-230">Make sure hello shared keys match.</span></span> <span data-ttu-id="6c787-231">Připojení hello tooestablish trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="6c787-231">It takes a few minutes tooestablish hello connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG4 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW -l westus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1G
  ```
5. <span data-ttu-id="6c787-232">Ověřte stav připojení.</span><span class="sxs-lookup"><span data-stu-id="6c787-232">Verify your connections.</span></span> <span data-ttu-id="6c787-233">Viz [Ověření stavu připojení](#verify).</span><span class="sxs-lookup"><span data-stu-id="6c787-233">See [Verify your connection](#verify).</span></span>

### <span data-ttu-id="6c787-234"><a name="samerg"></a>tooconnect patřící do hello stejné skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6c787-234"><a name="samerg"></a>tooconnect VNets that reside in hello same resource group</span></span>

1. <span data-ttu-id="6c787-235">Vytvořte připojení tooTestVNet4 hello virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c787-235">Create hello TestVNet1 tooTestVNet4 connection.</span></span> <span data-ttu-id="6c787-236">V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet4.</span><span class="sxs-lookup"><span data-stu-id="6c787-236">In this step, you create hello connection from TestVNet1 tooTestVNet4.</span></span> <span data-ttu-id="6c787-237">Skupiny prostředků hello oznámení jsou hello stejné v příkladech hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-237">Notice hello resource groups are hello same in hello examples.</span></span> <span data-ttu-id="6c787-238">Zobrazí také sdílený klíč uváděný v příkladech hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-238">You also see a shared key referenced in hello examples.</span></span> <span data-ttu-id="6c787-239">Pro hello sdílený klíč můžete použít vlastní hodnoty, ale musí odpovídat hello sdílený klíč pro obě připojení.</span><span class="sxs-lookup"><span data-stu-id="6c787-239">You can use your own values for hello shared key, however, hello shared key must match for both connections.</span></span> <span data-ttu-id="6c787-240">Vytvoření připojení trvá malou chvíli toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6c787-240">Creating a connection takes a short while toocomplete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet4GW
  ```
2. <span data-ttu-id="6c787-241">Vytvořte připojení tooTestVNet1 virtuální sítě TestVNet4 hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-241">Create hello TestVNet4 tooTestVNet1 connection.</span></span> <span data-ttu-id="6c787-242">Tento krok je podobný toohello jeden vyšší, s výjimkou toho, kterou vytváříte hello připojení z virtuální sítě TestVNet4 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c787-242">This step is similar toohello one above, except you are creating hello connection from TestVNet4 tooTestVNet1.</span></span> <span data-ttu-id="6c787-243">Zkontrolujte, zda text hello sdílené klíče shodují.</span><span class="sxs-lookup"><span data-stu-id="6c787-243">Make sure hello shared keys match.</span></span> <span data-ttu-id="6c787-244">Připojení hello tooestablish trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="6c787-244">It takes a few minutes tooestablish hello connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG1 --vnet-gateway1 VNet4GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet1GW
  ```
3. <span data-ttu-id="6c787-245">Ověřte stav připojení.</span><span class="sxs-lookup"><span data-stu-id="6c787-245">Verify your connections.</span></span> <span data-ttu-id="6c787-246">Viz [Ověření stavu připojení](#verify).</span><span class="sxs-lookup"><span data-stu-id="6c787-246">See [Verify your connection](#verify).</span></span>

## <span data-ttu-id="6c787-247"><a name="difsub"></a>Propojení virtuálních sítí patřících k různým předplatným</span><span class="sxs-lookup"><span data-stu-id="6c787-247"><a name="difsub"></a>Connect VNets that are in different subscriptions</span></span>

![Diagram v2v](./media/vpn-gateway-howto-vnet-vnet-cli/v2vdiffsub.png)

<span data-ttu-id="6c787-249">V tomto scénáři propojíme sítě TestVNet1 a TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="6c787-249">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="6c787-250">Hello virtuální sítě jsou umístěny na různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="6c787-250">hello VNets reside different subscriptions.</span></span> <span data-ttu-id="6c787-251">odběry Hello nemusí toobe přidružené hello stejné klienta služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6c787-251">hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> <span data-ttu-id="6c787-252">Hello kroky pro tuto konfiguraci přidat další připojení VNet-to-VNet v pořadí tooconnect tooTestVNet5 virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c787-252">hello steps for this configuration add an additional VNet-to-VNet connection in order tooconnect TestVNet1 tooTestVNet5.</span></span>

### <span data-ttu-id="6c787-253"><a name="TestVNet1diff"></a>Krok 5: Vytvoření a konfigurace virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="6c787-253"><a name="TestVNet1diff"></a>Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="6c787-254">Tyto pokyny pokračovat od hello kroky v předchozích částech hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-254">These instructions continue from hello steps in hello preceding sections.</span></span> <span data-ttu-id="6c787-255">Je třeba provést [kroku 1](#Connect) a [kroku 2](#TestVNet1) toocreate a konfigurace virtuální sítě TestVNet1 a hello brána sítě VPN pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c787-255">You must complete [Step 1](#Connect) and [Step 2](#TestVNet1) toocreate and configure TestVNet1 and hello VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="6c787-256">Pro tuto konfiguraci nemůžete se vyžaduje toocreate virtuální sítě TestVNet4 z předchozí části hello, ale pokud ho vytvoříte, se nebude v konfliktu s tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="6c787-256">For this configuration, you are not required toocreate TestVNet4 from hello previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="6c787-257">Po dokončení kroků 1 a 2 pokračujte krokem 6 (níže).</span><span class="sxs-lookup"><span data-stu-id="6c787-257">Once you complete Step 1 and Step 2, continue with Step 6 (below).</span></span>

### <span data-ttu-id="6c787-258"><a name="verifyranges"></a>Krok 6 – ověření hello rozsahy IP adres</span><span class="sxs-lookup"><span data-stu-id="6c787-258"><a name="verifyranges"></a>Step 6 - Verify hello IP address ranges</span></span>

<span data-ttu-id="6c787-259">Při vytváření další připojení, je důležité tooverify, který hello adresní prostor IP adres hello nové virtuální sítě se nepřekrývá s žádným z vaší rozsahy virtuálních sítí ani rozsahů bran místních sítí.</span><span class="sxs-lookup"><span data-stu-id="6c787-259">When creating additional connections, it's important tooverify that hello IP address space of hello new virtual network does not overlap with any of your other VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="6c787-260">Pro tento postup můžete použít následující hodnoty pro hello virtuální sítě TestVNet5 hello:</span><span class="sxs-lookup"><span data-stu-id="6c787-260">For this exercise, you can use hello following values for hello TestVNet5:</span></span>

<span data-ttu-id="6c787-261">**Hodnoty pro virtuální síť TestVNet5:**</span><span class="sxs-lookup"><span data-stu-id="6c787-261">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="6c787-262">Název virtuální sítě: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="6c787-262">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="6c787-263">Skupina prostředků: TestRG5</span><span class="sxs-lookup"><span data-stu-id="6c787-263">Resource Group: TestRG5</span></span>
* <span data-ttu-id="6c787-264">Umístění: Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="6c787-264">Location: Japan East</span></span>
* <span data-ttu-id="6c787-265">TestVNet5: 10.51.0.0/16 a 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6c787-265">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="6c787-266">FrontEnd: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="6c787-266">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="6c787-267">BackEnd: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="6c787-267">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="6c787-268">GatewaySubnet: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="6c787-268">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="6c787-269">Název brány: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="6c787-269">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="6c787-270">Veřejná IP adresa: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="6c787-270">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="6c787-271">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="6c787-271">VPNType: RouteBased</span></span>
* <span data-ttu-id="6c787-272">Připojení: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="6c787-272">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="6c787-273">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="6c787-273">ConnectionType: VNet2VNet</span></span>

### <span data-ttu-id="6c787-274"><a name="TestVNet5"></a>Krok 7: Vytvoření a konfigurace virtuální sítě TestVNet5</span><span class="sxs-lookup"><span data-stu-id="6c787-274"><a name="TestVNet5"></a>Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="6c787-275">Tento krok je třeba provést v kontextu hello hello nové předplatné, předplatné 5.</span><span class="sxs-lookup"><span data-stu-id="6c787-275">This step must be done in hello context of hello new subscription, Subscription 5.</span></span> <span data-ttu-id="6c787-276">Tuto část může provést pomocí Správce hello v jiné organizaci, který vlastní hello předplatné.</span><span class="sxs-lookup"><span data-stu-id="6c787-276">This part may be performed by hello administrator in a different organization that owns hello subscription.</span></span> <span data-ttu-id="6c787-277">tooswitch mezi odběrů použijte ' seznamu účtů az--všechny ' toolist hello účet k dispozici tooyour odběry, potom použijte ' nastaven účet az--předplatné <subscriptionID>' tooswitch toohello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="6c787-277">tooswitch between subscriptions use 'az account list --all' toolist hello subscriptions available tooyour account, then use 'az account set --subscription <subscriptionID>' tooswitch toohello subscription that you want toouse.</span></span>

1. <span data-ttu-id="6c787-278">Ujistěte se, že jsou připojené tooSubscription 5 a potom vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6c787-278">Make sure you are connected tooSubscription 5, then create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG5  -l japaneast
  ```
2. <span data-ttu-id="6c787-279">Vytvořte virtuální síť TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="6c787-279">Create TestVNet5.</span></span>

  ```azurecli
  az network vnet create -n TestVNet5 -g TestRG5 --address-prefix 10.51.0.0/16 -l japaneast --subnet-name FrontEnd --subnet-prefix 10.51.0.0/24
  ```

3. <span data-ttu-id="6c787-280">Přidejte podsítě.</span><span class="sxs-lookup"><span data-stu-id="6c787-280">Add subnets.</span></span>

  ```azurecli
  az network vnet update -n TestVNet5 --address-prefixes 10.51.0.0/16 10.52.0.0/16 -g TestRG5
  az network vnet subnet create --vnet-name TestVNet5 -n BackEnd -g TestRG5 --address-prefix 10.52.0.0/24
  ```

4. <span data-ttu-id="6c787-281">Přidání podsítě brány hello.</span><span class="sxs-lookup"><span data-stu-id="6c787-281">Add hello gateway subnet.</span></span>

  ```azurecli
  az network vnet subnet create --vnet-name TestVNet5 -n GatewaySubnet -g TestRG5 --address-prefix 10.52.255.0/27
  ```

5. <span data-ttu-id="6c787-282">Vyžádejte si veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6c787-282">Request a public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet5GWIP -g TestRG5 --allocation-method Dynamic
  ```
6. <span data-ttu-id="6c787-283">Vytvoření brány virtuální sítě TestVNet5 hello</span><span class="sxs-lookup"><span data-stu-id="6c787-283">Create hello TestVNet5 gateway</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet5GW -l japaneast --public-ip-address VNet5GWIP -g TestRG5 --vnet TestVNet5 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="6c787-284"><a name="connections5"></a>Krok 8 – vytvoření připojení hello</span><span class="sxs-lookup"><span data-stu-id="6c787-284"><a name="connections5"></a>Step 8 - Create hello connections</span></span>

<span data-ttu-id="6c787-285">Jsme rozdělit tento krok do dvou relací rozhraní příkazového řádku, označen jako **[předplatné 1]**, a **[předplatné 5]** vzhledem k tomu, že jsou v různých předplatných hello hello brány.</span><span class="sxs-lookup"><span data-stu-id="6c787-285">We split this step into two CLI sessions marked as **[Subscription 1]**, and **[Subscription 5]** because hello gateways are in hello different subscriptions.</span></span> <span data-ttu-id="6c787-286">tooswitch mezi odběrů použijte ' seznamu účtů az--všechny ' toolist hello účet k dispozici tooyour odběry, potom použijte ' nastaven účet az--předplatné <subscriptionID>' tooswitch toohello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="6c787-286">tooswitch between subscriptions use 'az account list --all' toolist hello subscriptions available tooyour account, then use 'az account set --subscription <subscriptionID>' tooswitch toohello subscription that you want toouse.</span></span>

1. <span data-ttu-id="6c787-287">**[Předplatné 1]**  Přihlásit a připojte tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="6c787-287">**[Subscription 1]** Log in and connect tooSubscription 1.</span></span> <span data-ttu-id="6c787-288">Hello spusťte následující příkaz tooget hello název a ID hello brány z výstupu hello:</span><span class="sxs-lookup"><span data-stu-id="6c787-288">Run hello following command tooget hello name and ID of hello Gateway from hello output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="6c787-289">Kopírovat výstup hello "id:".</span><span class="sxs-lookup"><span data-stu-id="6c787-289">Copy hello output for "id:".</span></span> <span data-ttu-id="6c787-290">Odešlete hello ID a název hello hello virtuální síť brány (VNet1GW) toohello správci předplatného 5 prostřednictvím e-mailu nebo jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="6c787-290">Send hello ID and hello name of hello VNet gateway (VNet1GW) toohello administrator of Subscription 5 via email or another method.</span></span>

  <span data-ttu-id="6c787-291">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="6c787-291">Example output:</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
  ```

2. <span data-ttu-id="6c787-292">**[Předplatné 5]**  Přihlásit a připojte tooSubscription 5.</span><span class="sxs-lookup"><span data-stu-id="6c787-292">**[Subscription 5]** Log in and connect tooSubscription 5.</span></span> <span data-ttu-id="6c787-293">Hello spusťte následující příkaz tooget hello název a ID hello brány z výstupu hello:</span><span class="sxs-lookup"><span data-stu-id="6c787-293">Run hello following command tooget hello name and ID of hello Gateway from hello output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet5GW -g TestRG5
  ```

  <span data-ttu-id="6c787-294">Kopírovat výstup hello "id:".</span><span class="sxs-lookup"><span data-stu-id="6c787-294">Copy hello output for "id:".</span></span> <span data-ttu-id="6c787-295">Odešlete hello ID a název hello hello virtuální síť brány (VNet5GW) toohello správci předplatného 1 prostřednictvím e-mailu nebo jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="6c787-295">Send hello ID and hello name of hello VNet gateway (VNet5GW) toohello administrator of Subscription 1 via email or another method.</span></span>

3. <span data-ttu-id="6c787-296">**[Předplatné 1]**  v tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet5.</span><span class="sxs-lookup"><span data-stu-id="6c787-296">**[Subscription 1]** In this step, you create hello connection from TestVNet1 tooTestVNet5.</span></span> <span data-ttu-id="6c787-297">Pro hello sdílený klíč můžete použít vlastní hodnoty, ale musí odpovídat hello sdílený klíč pro obě připojení.</span><span class="sxs-lookup"><span data-stu-id="6c787-297">You can use your own values for hello shared key, however, hello shared key must match for both connections.</span></span> <span data-ttu-id="6c787-298">Vytvoření připojení může trvat malou chvíli toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6c787-298">Creating a connection can take a short while toocomplete.</span></span> <span data-ttu-id="6c787-299">Ujistěte se, že jste připojeni tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="6c787-299">Make sure you connect tooSubscription 1.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet5 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```

4. <span data-ttu-id="6c787-300">**[Předplatné 5]**  Tento krok je podobný toohello jeden vyšší, s výjimkou toho, kterou vytváříte hello připojení z virtuální sítě TestVNet5 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c787-300">**[Subscription 5]** This step is similar toohello one above, except you are creating hello connection from TestVNet5 tooTestVNet1.</span></span> <span data-ttu-id="6c787-301">Ujistěte se, že tento hello sdíleného klíče shodu a že se můžete připojit tooSubscription 5.</span><span class="sxs-lookup"><span data-stu-id="6c787-301">Make sure that hello shared keys match and that you connect tooSubscription 5.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet5ToVNet1 -g TestRG5 --vnet-gateway1 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW -l japaneast --shared-key "eeffgg" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```

## <span data-ttu-id="6c787-302"><a name="verify"></a>Ověřte připojení hello</span><span class="sxs-lookup"><span data-stu-id="6c787-302"><a name="verify"></a>Verify hello connections</span></span>
[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections v2v cli](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <span data-ttu-id="6c787-303"><a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="6c787-303"><a name="faq"></a>VNet-to-VNet FAQ</span></span>
[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="6c787-304">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c787-304">Next steps</span></span>

* <span data-ttu-id="6c787-305">Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6c787-305">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="6c787-306">Další informace najdete v tématu hello [virtuální počítače dokumentaci](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="6c787-306">For more information, see hello [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="6c787-307">Informace o protokolu BGP najdete v tématu hello [přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [jak tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="6c787-307">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
