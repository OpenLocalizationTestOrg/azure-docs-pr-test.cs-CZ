---
title: "Připojení virtuální sítě k jiné virtuální síti: Azure CLI | Dokumentace Microsoftu"
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
ms.openlocfilehash: ae42f661b39e8b6170fd415d758404fb33009ccc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-azure-cli"></a><span data-ttu-id="89b1a-103">Konfigurace připojení brány VPN typu VNet-to-VNet pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="89b1a-103">Configure a VNet-to-VNet VPN gateway connection using Azure CLI</span></span>

<span data-ttu-id="89b1a-104">Tento článek ukazuje, jak vytvořit připojení brány VPN mezi virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="89b1a-104">This article shows you how to create a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="89b1a-105">Virtuální sítě se můžou nacházet ve stejné oblasti nebo v různých oblastech a můžou patřit do stejného předplatného nebo do různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="89b1a-105">The virtual networks can be in the same or different regions, and from the same or different subscriptions.</span></span> <span data-ttu-id="89b1a-106">Pokud připojujete virtuální sítě z různých předplatných, tato předplatná nemusí být přidružená ke stejnému tenantovi Active Directory.</span><span class="sxs-lookup"><span data-stu-id="89b1a-106">When connecting VNets from different subscriptions, the subscriptions do not need to be associated with the same Active Directory tenant.</span></span> 

<span data-ttu-id="89b1a-107">Postupy v tomto článku se týkají modelu nasazení Resource Manager a používají Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="89b1a-107">The steps in this article apply to the Resource Manager deployment model and use Azure CLI.</span></span> <span data-ttu-id="89b1a-108">Tuto konfiguraci můžete vytvořit také pomocí jiného nástroje nasazení nebo pro jiný model nasazení, a to výběrem jiné možnosti z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="89b1a-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="89b1a-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="89b1a-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="89b1a-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="89b1a-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="89b1a-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="89b1a-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="89b1a-112">Azure Portal (Classic)</span><span class="sxs-lookup"><span data-stu-id="89b1a-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="89b1a-113">Propojení různých modelů nasazení – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="89b1a-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="89b1a-114">Propojení různých modelů nasazení – PowerShell</span><span class="sxs-lookup"><span data-stu-id="89b1a-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="89b1a-115">Propojení virtuální sítě s jinou virtuální sítí (VNet-to-VNet) je podobné propojení virtuální sítě s místním serverem.</span><span class="sxs-lookup"><span data-stu-id="89b1a-115">Connecting a virtual network to another virtual network (VNet-to-VNet) is similar to connecting a VNet to an on-premises site location.</span></span> <span data-ttu-id="89b1a-116">Oba typy připojení využívají bránu VPN k poskytnutí zabezpečeného tunelového propojení prostřednictvím protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="89b1a-116">Both connectivity types use a VPN gateway to provide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="89b1a-117">Pokud se virtuální sítě nacházejí ve stejné oblasti, můžete uvažovat o jejich propojení vytvořením partnerského vztahu virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="89b1a-117">If your VNets are in the same region, you may want to consider connecting them using VNet Peering.</span></span> <span data-ttu-id="89b1a-118">Partnerské vztahy virtuálních sítí nepoužívají bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="89b1a-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="89b1a-119">Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="89b1a-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="89b1a-120">Komunikaci typu VNet-to-VNet můžete kombinovat s konfiguracemi s více servery.</span><span class="sxs-lookup"><span data-stu-id="89b1a-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="89b1a-121">Díky tomu je možné vytvářet topologie sítí, ve kterých se používá propojování více míst i propojování virtuálních sítí, jak je znázorněno v následujícím schématu:</span><span class="sxs-lookup"><span data-stu-id="89b1a-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in the following diagram:</span></span>

![Informace o připojeních](./media/vpn-gateway-howto-vnet-vnet-cli/aboutconnections.png)

### <span data-ttu-id="89b1a-123"><a name="why"></a>Proč propojovat virtuální sítě?</span><span class="sxs-lookup"><span data-stu-id="89b1a-123"><a name="why"></a>Why connect virtual networks?</span></span>

<span data-ttu-id="89b1a-124">Virtuální sítě může být vhodné propojit z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="89b1a-124">You may want to connect virtual networks for the following reasons:</span></span>

* <span data-ttu-id="89b1a-125">**Geografická redundance napříč oblastmi a geografická přítomnost**</span><span class="sxs-lookup"><span data-stu-id="89b1a-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="89b1a-126">Můžete nastavit vlastní geografickou replikaci nebo synchronizaci se zabezpečeným připojením bez procházení koncovými body připojenými k internetu.</span><span class="sxs-lookup"><span data-stu-id="89b1a-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="89b1a-127">Pomocí Azure Traffic Manageru a služby Load Balancer je možné vytvářet úlohy s vysokou dostupností s geografickou redundancí nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="89b1a-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="89b1a-128">Jedním z důležitých příkladů je nastavení technologie SQL Always On se skupinami dostupnosti nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="89b1a-128">One important example is to set up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="89b1a-129">**Regionální vícevrstvé aplikace s izolací nebo administrativní hranicí**</span><span class="sxs-lookup"><span data-stu-id="89b1a-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="89b1a-130">V rámci stejné oblasti můžete vytvářet vícevrstvé aplikace s několika virtuálními sítěmi propojenými z důvodu izolace nebo požadavků na správu.</span><span class="sxs-lookup"><span data-stu-id="89b1a-130">Within the same region, you can set up multi-tier applications with multiple virtual networks connected together due to isolation or administrative requirements.</span></span>

<span data-ttu-id="89b1a-131">Další informace o propojeních VNet-to-VNet najdete v části [Nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="89b1a-131">For more information about VNet-to-VNet connections, see the [VNet-to-VNet FAQ](#faq) at the end of this article.</span></span>

### <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="89b1a-132">Kterou posloupnost kroků provést?</span><span class="sxs-lookup"><span data-stu-id="89b1a-132">Which set of steps should I use?</span></span>

<span data-ttu-id="89b1a-133">V tomto článku uvidíte dvě různé sady kroků.</span><span class="sxs-lookup"><span data-stu-id="89b1a-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="89b1a-134">Jedna sada kroků pro [virtuální sítě spadající do stejného předplatného](#samesub) a druhá sada kroků pro [virtuální sítě v různých předplatných](#difsub).</span><span class="sxs-lookup"><span data-stu-id="89b1a-134">One set of steps for [VNets that reside in the same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span>

## <span data-ttu-id="89b1a-135"><a name="samesub"></a>Propojení virtuálních sítí patřících ke stejnému předplatnému</span><span class="sxs-lookup"><span data-stu-id="89b1a-135"><a name="samesub"></a>Connect VNets that are in the same subscription</span></span>

![Diagram v2v](./media/vpn-gateway-howto-vnet-vnet-cli/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="89b1a-137">Než začnete</span><span class="sxs-lookup"><span data-stu-id="89b1a-137">Before you begin</span></span>

<span data-ttu-id="89b1a-138">Než začnete, nainstalujte si nejnovější verzi příkazů rozhraní příkazového řádku (2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="89b1a-138">Before beginning, install the latest version of the CLI commands (2.0 or later).</span></span> <span data-ttu-id="89b1a-139">Informace o instalaci příkazů rozhraní příkazového řádku najdete v tématu [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="89b1a-139">For information about installing the CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

### <span data-ttu-id="89b1a-140"><a name="Plan"></a>Plánování rozsahů IP adres</span><span class="sxs-lookup"><span data-stu-id="89b1a-140"><a name="Plan"></a>Plan your IP address ranges</span></span>

<span data-ttu-id="89b1a-141">V následujících krocích vytvoříme dvě virtuální sítě spolu s příslušnými podsítěmi a konfiguracemi brány.</span><span class="sxs-lookup"><span data-stu-id="89b1a-141">In the following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="89b1a-142">Poté vytvoříme propojení VPN mezi oběma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="89b1a-142">We then create a VPN connection between the two VNets.</span></span> <span data-ttu-id="89b1a-143">Je důležité určit rozsahy IP adres pro konfiguraci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="89b1a-143">It’s important to plan the IP address ranges for your network configuration.</span></span> <span data-ttu-id="89b1a-144">Mějte na paměti, že je třeba zajistit, aby se žádné rozsahy virtuálních sítí ani místní síťové rozsahy žádným způsobem nepřekrývaly.</span><span class="sxs-lookup"><span data-stu-id="89b1a-144">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="89b1a-145">V těchto příkladech nezahrnujeme server DNS.</span><span class="sxs-lookup"><span data-stu-id="89b1a-145">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="89b1a-146">Pokud chcete překlad IP adres pro virtuální sítě, přečtěte si téma [Překlad IP adres](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="89b1a-146">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="89b1a-147">V příkladech používáme následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="89b1a-147">We use the following values in the examples:</span></span>

<span data-ttu-id="89b1a-148">**Hodnoty pro virtuální síť TestVNet1:**</span><span class="sxs-lookup"><span data-stu-id="89b1a-148">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="89b1a-149">Název virtuální sítě: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="89b1a-149">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="89b1a-150">Skupina prostředků: TestRG1</span><span class="sxs-lookup"><span data-stu-id="89b1a-150">Resource Group: TestRG1</span></span>
* <span data-ttu-id="89b1a-151">Umístění: Východní USA</span><span class="sxs-lookup"><span data-stu-id="89b1a-151">Location: East US</span></span>
* <span data-ttu-id="89b1a-152">TestVNet1: 10.11.0.0/16 a 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="89b1a-152">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="89b1a-153">FrontEnd: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="89b1a-153">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="89b1a-154">BackEnd: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="89b1a-154">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="89b1a-155">GatewaySubnet: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="89b1a-155">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="89b1a-156">Název brány: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="89b1a-156">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="89b1a-157">Veřejná IP adresa: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="89b1a-157">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="89b1a-158">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="89b1a-158">VPNType: RouteBased</span></span>
* <span data-ttu-id="89b1a-159">Připojení (1 ke 4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="89b1a-159">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="89b1a-160">Připojení (1 k 5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="89b1a-160">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="89b1a-161">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="89b1a-161">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="89b1a-162">**Hodnoty pro virtuální síť TestVNet4:**</span><span class="sxs-lookup"><span data-stu-id="89b1a-162">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="89b1a-163">Název virtuální sítě: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="89b1a-163">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="89b1a-164">TestVNet2: 10.41.0.0/16 a 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="89b1a-164">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="89b1a-165">FrontEnd: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="89b1a-165">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="89b1a-166">BackEnd: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="89b1a-166">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="89b1a-167">GatewaySubnet: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="89b1a-167">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="89b1a-168">Skupina prostředků: TestRG4</span><span class="sxs-lookup"><span data-stu-id="89b1a-168">Resource Group: TestRG4</span></span>
* <span data-ttu-id="89b1a-169">Umístění: Západní USA</span><span class="sxs-lookup"><span data-stu-id="89b1a-169">Location: West US</span></span>
* <span data-ttu-id="89b1a-170">Název brány: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="89b1a-170">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="89b1a-171">Veřejná IP adresa: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="89b1a-171">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="89b1a-172">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="89b1a-172">VPNType: RouteBased</span></span>
* <span data-ttu-id="89b1a-173">Připojení: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="89b1a-173">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="89b1a-174">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="89b1a-174">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="89b1a-175"><a name="Connect"></a>Krok 1: Připojení k vašemu předplatnému</span><span class="sxs-lookup"><span data-stu-id="89b1a-175"><a name="Connect"></a>Step 1 - Connect to your subscription</span></span>

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-numbers-include.md)]

### <span data-ttu-id="89b1a-176"><a name="TestVNet1"></a>Krok 2: Vytvoření a konfigurace virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="89b1a-176"><a name="TestVNet1"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="89b1a-177">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="89b1a-177">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG1  -l eastus
  ```
2. <span data-ttu-id="89b1a-178">Vytvořte virtuální síť TestVNet1 a její podsítě.</span><span class="sxs-lookup"><span data-stu-id="89b1a-178">Create TestVNet1 and the subnets for TestVNet1.</span></span> <span data-ttu-id="89b1a-179">Tento příklad vytvoří virtuální síť TestVNet1 a podsíť FrontEnd.</span><span class="sxs-lookup"><span data-stu-id="89b1a-179">This example creates a virtual network named TestVNet1 and a subnet named FrontEnd.</span></span>

  ```azurecli
  az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24
  ```
3. <span data-ttu-id="89b1a-180">Vytvořte další adresní prostor pro podsíť back-endu.</span><span class="sxs-lookup"><span data-stu-id="89b1a-180">Create an additional address space for the backend subnet.</span></span> <span data-ttu-id="89b1a-181">Všimněte si, že v tomto kroku zadáváme jak adresní prostor, který jsme vytvořili dříve, tak i další adresní prostor, který chceme přidat.</span><span class="sxs-lookup"><span data-stu-id="89b1a-181">Notice that in this step, we specify both the address space that we created earlier, and the additional address space that we want to add.</span></span> <span data-ttu-id="89b1a-182">Důvodem je, že příkaz [az network vnet update](https://docs.microsoft.com/cli/azure/network/vnet#update) přepíše předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="89b1a-182">This is because the [az network vnet update](https://docs.microsoft.com/cli/azure/network/vnet#update) command overwrites the previous settings.</span></span> <span data-ttu-id="89b1a-183">Nezapomeňte při použití tohoto příkazu zadat všechny předpony adres.</span><span class="sxs-lookup"><span data-stu-id="89b1a-183">Make sure to specify all of the address prefixes when using this command.</span></span>

  ```azurecli
  az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestRG1
  ```
4. <span data-ttu-id="89b1a-184">Vytvořte podsíť back-endu.</span><span class="sxs-lookup"><span data-stu-id="89b1a-184">Create the backend subnet.</span></span>
  
  ```azurecli
  az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestRG1 --address-prefix 10.12.0.0/24 
  ```
5. <span data-ttu-id="89b1a-185">Vytvořte podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="89b1a-185">Create the gateway subnet.</span></span> <span data-ttu-id="89b1a-186">Všimněte si, že podsíť brány má název GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="89b1a-186">Notice that the gateway subnet is named 'GatewaySubnet'.</span></span> <span data-ttu-id="89b1a-187">Název je povinný.</span><span class="sxs-lookup"><span data-stu-id="89b1a-187">This name is required.</span></span> <span data-ttu-id="89b1a-188">V příkladu používá podsíť brány možnost /27.</span><span class="sxs-lookup"><span data-stu-id="89b1a-188">In this example, the gateway subnet is using a /27.</span></span> <span data-ttu-id="89b1a-189">I když je možné vytvořit podsíť brány s minimální velikostí /29, doporučujeme vytvořit větší podsíť, která pojme více adres, tzn. vybrat velikost alespoň /28 nebo /27.</span><span class="sxs-lookup"><span data-stu-id="89b1a-189">While it is possible to create a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="89b1a-190">Tím vznikne dostatečný prostor pro adresy, který umožní nastavení případných dalších konfigurací v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="89b1a-190">This will allow for enough addresses to accommodate possible additional configurations that you may want in the future.</span></span>

  ```azurecli 
  az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestRG1 --address-prefix 10.12.255.0/27
  ```
6. <span data-ttu-id="89b1a-191">Vyžádejte si veřejnou IP adresu, která bude přidělena bráně, kterou vytvoříte pro příslušnou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="89b1a-191">Request a public IP address to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="89b1a-192">Všimněte si, že metoda AllocationMethod je dynamická.</span><span class="sxs-lookup"><span data-stu-id="89b1a-192">Notice that the AllocationMethod is Dynamic.</span></span> <span data-ttu-id="89b1a-193">Není možné určit IP adresu, kterou chcete používat.</span><span class="sxs-lookup"><span data-stu-id="89b1a-193">You cannot specify the IP address that you want to use.</span></span> <span data-ttu-id="89b1a-194">Přiděluje se pro bránu dynamicky.</span><span class="sxs-lookup"><span data-stu-id="89b1a-194">It's dynamically allocated to your gateway.</span></span>

  ```azurecli
  az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
  ```
7. <span data-ttu-id="89b1a-195">Vytvořte bránu virtuální sítě pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="89b1a-195">Create the virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="89b1a-196">Konfigurace propojení VNet-to-VNet vyžadují typ sítě VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="89b1a-196">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="89b1a-197">Pokud tento příkaz spustíte s použitím parametru --no-wait, nezobrazí se žádná zpětná vazba ani výstup.</span><span class="sxs-lookup"><span data-stu-id="89b1a-197">If you run this command using the '--no-wait' parameter, you don't see any feedback or output.</span></span> <span data-ttu-id="89b1a-198">Parametr --no-wait umožňuje bránu vytvořit na pozadí.</span><span class="sxs-lookup"><span data-stu-id="89b1a-198">The '--no-wait' parameter allows the gateway to create in the background.</span></span> <span data-ttu-id="89b1a-199">Neznamená to, že se brána VPN vytvoří okamžitě.</span><span class="sxs-lookup"><span data-stu-id="89b1a-199">It does not mean that the VPN gateway finishes creating immediately.</span></span> <span data-ttu-id="89b1a-200">Vytvoření brány může obvykle trvat 45 minut nebo déle, a to v závislosti na použité skladové jednotce (SKU) brány.</span><span class="sxs-lookup"><span data-stu-id="89b1a-200">Creating a gateway can often take 45 minutes or more, depending on the gateway SKU that you use.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="89b1a-201"><a name="TestVNet4"></a>Krok 3: Vytvoření a konfigurace virtuální sítě TestVNet4</span><span class="sxs-lookup"><span data-stu-id="89b1a-201"><a name="TestVNet4"></a>Step 3 - Create and configure TestVNet4</span></span>

1. <span data-ttu-id="89b1a-202">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="89b1a-202">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG4  -l westus
  ```
2. <span data-ttu-id="89b1a-203">Vytvořte virtuální síť TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="89b1a-203">Create TestVNet4.</span></span>

  ```azurecli
  az network vnet create -n TestVNet4 -g TestRG4 --address-prefix 10.41.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.41.0.0/24
  ```

3. <span data-ttu-id="89b1a-204">Vytvořte další podsítě pro virtuální síť TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="89b1a-204">Create additional subnets for TestVNet4.</span></span>

  ```azurecli
  az network vnet update -n TestVNet4 --address-prefixes 10.41.0.0/16 10.42.0.0/16 -g TestRG4 
  az network vnet subnet create --vnet-name TestVNet4 -n BackEnd -g TestRG4 --address-prefix 10.42.0.0/24 
  ```
4. <span data-ttu-id="89b1a-205">Vytvořte podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="89b1a-205">Create the gateway subnet.</span></span>

  ```azurecli
   az network vnet subnet create --vnet-name TestVNet4 -n GatewaySubnet -g TestRG4 --address-prefix 10.42.255.0/27
  ```
5. <span data-ttu-id="89b1a-206">Vyžádejte si veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="89b1a-206">Request a Public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet4GWIP -g TestRG4 --allocation-method Dynamic
  ```
6. <span data-ttu-id="89b1a-207">Vytvořte bránu virtuální sítě TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="89b1a-207">Create the TestVNet4 virtual network gateway.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet4GW -l westus --public-ip-address VNet4GWIP -g TestRG4 --vnet TestVNet4 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="89b1a-208"><a name="createconnect"></a>Krok 4: Vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="89b1a-208"><a name="createconnect"></a>Step 4 - Create the connections</span></span>

<span data-ttu-id="89b1a-209">Nyní máte dvě virtuální sítě s bránami VPN.</span><span class="sxs-lookup"><span data-stu-id="89b1a-209">You now have two VNets with VPN gateways.</span></span> <span data-ttu-id="89b1a-210">Dalším krokem je vytvoření propojení bran VPN mezi bránami virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="89b1a-210">The next step is to create VPN gateway connections between the virtual network gateways.</span></span> <span data-ttu-id="89b1a-211">Pokud jste použili výše uvedené příklady, vaše brány virtuálních sítí jsou v různých skupinách prostředků.</span><span class="sxs-lookup"><span data-stu-id="89b1a-211">If you used the examples above, your VNet gateways are in different resource groups.</span></span> <span data-ttu-id="89b1a-212">Když jsou brány v různých skupinách prostředků, musíte při vytváření propojení identifikovat a zadat ID prostředků pro každou bránu.</span><span class="sxs-lookup"><span data-stu-id="89b1a-212">When gateways are in different resource groups, you need to identify and specify the resource IDs for each gateway when making a connection.</span></span> <span data-ttu-id="89b1a-213">Pokud jsou vaše virtuální sítě ve stejné skupině prostředků, můžete použít [druhou sadu pokynů](#samerg), protože nemusíte zadávat ID prostředků.</span><span class="sxs-lookup"><span data-stu-id="89b1a-213">If your VNets are in the same resource group, you can use the [second set of instructions](#samerg) because you don't need to specify the resource IDs.</span></span>

### <span data-ttu-id="89b1a-214"><a name="diffrg"></a>Propojení virtuálních sítí patřících do různých skupin prostředků</span><span class="sxs-lookup"><span data-stu-id="89b1a-214"><a name="diffrg"></a>To connect VNets that reside in different resource groups</span></span>

1. <span data-ttu-id="89b1a-215">Z výstupu následujícího příkazu získejte ID prostředku brány VNet1GW:</span><span class="sxs-lookup"><span data-stu-id="89b1a-215">Get the Resource ID of VNet1GW from the output of the following command:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="89b1a-216">Ve výstupu vyhledejte řádek, který začíná na "id:".</span><span class="sxs-lookup"><span data-stu-id="89b1a-216">In the output, find the "id:" line.</span></span> <span data-ttu-id="89b1a-217">Hodnoty v uvozovkách budete potřebovat pro vytvoření propojení v další části.</span><span class="sxs-lookup"><span data-stu-id="89b1a-217">The values within the quotes are needed to create the connection in the next section.</span></span> <span data-ttu-id="89b1a-218">Zkopírujte tyto hodnoty do textového editoru, jako je Poznámkový blok, abyste je při vytváření propojení mohli jednoduše vložit.</span><span class="sxs-lookup"><span data-stu-id="89b1a-218">Copy these values to a text editor, such as Notepad, so that you can easily paste them when creating your connection.</span></span>

  <span data-ttu-id="89b1a-219">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="89b1a-219">Example output:</span></span>

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

  <span data-ttu-id="89b1a-220">Zkopírujte hodnoty v uvozovkách následující po **"id":**.</span><span class="sxs-lookup"><span data-stu-id="89b1a-220">Copy the values after **"id":** within the quotes.</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
 ```

2. <span data-ttu-id="89b1a-221">Získejte ID prostředku brány VNet4GW a zkopírujte hodnoty do textového editoru.</span><span class="sxs-lookup"><span data-stu-id="89b1a-221">Get the Resource ID of VNet4GW and copy the values to a text editor.</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet4GW -g TestRG4
  ```

3. <span data-ttu-id="89b1a-222">Vytvořte připojení virtuální sítě TestVNet1 k virtuální síti TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="89b1a-222">Create the TestVNet1 to TestVNet4 connection.</span></span> <span data-ttu-id="89b1a-223">V tomto kroku vytvoříte připojení z virtuální sítě TestVNet1 do virtuální sítě TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="89b1a-223">In this step, you create the connection from TestVNet1 to TestVNet4.</span></span> <span data-ttu-id="89b1a-224">V příkladech se uvádí sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="89b1a-224">There is a shared key referenced in the examples.</span></span> <span data-ttu-id="89b1a-225">Pro sdílený klíč můžete použít vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="89b1a-225">You can use your own values for the shared key.</span></span> <span data-ttu-id="89b1a-226">Důležité je, že se sdílený klíč pro obě připojení musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="89b1a-226">The important thing is that the shared key must match for both connections.</span></span> <span data-ttu-id="89b1a-227">Vytvoření připojení nějakou dobu trvá.</span><span class="sxs-lookup"><span data-stu-id="89b1a-227">Creating a connection takes a short while to complete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW 
  ```
4. <span data-ttu-id="89b1a-228">Vytvořte připojení virtuální sítě TestVNet4 k virtuální síti TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="89b1a-228">Create the TestVNet4 to TestVNet1 connection.</span></span> <span data-ttu-id="89b1a-229">Tento krok je podobný předchozímu, vytváříte však připojení z virtuální sítě TestVNet4 do virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="89b1a-229">This step is similar to the one above, except you are creating the connection from TestVNet4 to TestVNet1.</span></span> <span data-ttu-id="89b1a-230">Ověřte, že se sdílené klíče shodují.</span><span class="sxs-lookup"><span data-stu-id="89b1a-230">Make sure the shared keys match.</span></span> <span data-ttu-id="89b1a-231">Navázání připojení trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="89b1a-231">It takes a few minutes to establish the connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG4 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW -l westus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1G
  ```
5. <span data-ttu-id="89b1a-232">Ověřte stav připojení.</span><span class="sxs-lookup"><span data-stu-id="89b1a-232">Verify your connections.</span></span> <span data-ttu-id="89b1a-233">Viz [Ověření stavu připojení](#verify).</span><span class="sxs-lookup"><span data-stu-id="89b1a-233">See [Verify your connection](#verify).</span></span>

### <span data-ttu-id="89b1a-234"><a name="samerg"></a>Propojení virtuálních sítí patřících do stejné skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="89b1a-234"><a name="samerg"></a>To connect VNets that reside in the same resource group</span></span>

1. <span data-ttu-id="89b1a-235">Vytvořte připojení virtuální sítě TestVNet1 k virtuální síti TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="89b1a-235">Create the TestVNet1 to TestVNet4 connection.</span></span> <span data-ttu-id="89b1a-236">V tomto kroku vytvoříte připojení z virtuální sítě TestVNet1 do virtuální sítě TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="89b1a-236">In this step, you create the connection from TestVNet1 to TestVNet4.</span></span> <span data-ttu-id="89b1a-237">Všimněte si, že skupiny prostředků v příkladech jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="89b1a-237">Notice the resource groups are the same in the examples.</span></span> <span data-ttu-id="89b1a-238">V příkladech také vidíte uvedený sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="89b1a-238">You also see a shared key referenced in the examples.</span></span> <span data-ttu-id="89b1a-239">Pro sdílený klíč můžete použít vlastní hodnoty, ale sdílené klíče pro obě připojení se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="89b1a-239">You can use your own values for the shared key, however, the shared key must match for both connections.</span></span> <span data-ttu-id="89b1a-240">Vytvoření připojení nějakou dobu trvá.</span><span class="sxs-lookup"><span data-stu-id="89b1a-240">Creating a connection takes a short while to complete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet4GW
  ```
2. <span data-ttu-id="89b1a-241">Vytvořte připojení virtuální sítě TestVNet4 k virtuální síti TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="89b1a-241">Create the TestVNet4 to TestVNet1 connection.</span></span> <span data-ttu-id="89b1a-242">Tento krok je podobný předchozímu, vytváříte však připojení z virtuální sítě TestVNet4 do virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="89b1a-242">This step is similar to the one above, except you are creating the connection from TestVNet4 to TestVNet1.</span></span> <span data-ttu-id="89b1a-243">Ověřte, že se sdílené klíče shodují.</span><span class="sxs-lookup"><span data-stu-id="89b1a-243">Make sure the shared keys match.</span></span> <span data-ttu-id="89b1a-244">Navázání připojení trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="89b1a-244">It takes a few minutes to establish the connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG1 --vnet-gateway1 VNet4GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet1GW
  ```
3. <span data-ttu-id="89b1a-245">Ověřte stav připojení.</span><span class="sxs-lookup"><span data-stu-id="89b1a-245">Verify your connections.</span></span> <span data-ttu-id="89b1a-246">Viz [Ověření stavu připojení](#verify).</span><span class="sxs-lookup"><span data-stu-id="89b1a-246">See [Verify your connection](#verify).</span></span>

## <span data-ttu-id="89b1a-247"><a name="difsub"></a>Propojení virtuálních sítí patřících k různým předplatným</span><span class="sxs-lookup"><span data-stu-id="89b1a-247"><a name="difsub"></a>Connect VNets that are in different subscriptions</span></span>

![Diagram v2v](./media/vpn-gateway-howto-vnet-vnet-cli/v2vdiffsub.png)

<span data-ttu-id="89b1a-249">V tomto scénáři propojíme sítě TestVNet1 a TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="89b1a-249">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="89b1a-250">Virtuální sítě patří k různým předplatným.</span><span class="sxs-lookup"><span data-stu-id="89b1a-250">The VNets reside different subscriptions.</span></span> <span data-ttu-id="89b1a-251">Předplatná nemusí být přidružená ke stejnému tenantovi Active Directory.</span><span class="sxs-lookup"><span data-stu-id="89b1a-251">The subscriptions do not need to be associated with the same Active Directory tenant.</span></span> <span data-ttu-id="89b1a-252">Tento postup přidá nové propojení VNet-to-VNet pro připojení virtuální sítě TestVNet1 k virtuální síti TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="89b1a-252">The steps for this configuration add an additional VNet-to-VNet connection in order to connect TestVNet1 to TestVNet5.</span></span>

### <span data-ttu-id="89b1a-253"><a name="TestVNet1diff"></a>Krok 5: Vytvoření a konfigurace virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="89b1a-253"><a name="TestVNet1diff"></a>Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="89b1a-254">Tyto pokyny navazují na kroky v předchozích částech.</span><span class="sxs-lookup"><span data-stu-id="89b1a-254">These instructions continue from the steps in the preceding sections.</span></span> <span data-ttu-id="89b1a-255">Je třeba vytvořit a konfigurovat virtuální síť TestVNet1 a bránu VPN pro virtuální síť TestVNet1 provedením [kroku 1](#Connect) a [kroku 2](#TestVNet1).</span><span class="sxs-lookup"><span data-stu-id="89b1a-255">You must complete [Step 1](#Connect) and [Step 2](#TestVNet1) to create and configure TestVNet1 and the VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="89b1a-256">Pro tuto konfiguraci není nutné vytvářet virtuální síť TestVNet4 z předchozí části, ale pokud ji vytvoříte, nebude to s těmito kroky v konfliktu.</span><span class="sxs-lookup"><span data-stu-id="89b1a-256">For this configuration, you are not required to create TestVNet4 from the previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="89b1a-257">Po dokončení kroků 1 a 2 pokračujte krokem 6 (níže).</span><span class="sxs-lookup"><span data-stu-id="89b1a-257">Once you complete Step 1 and Step 2, continue with Step 6 (below).</span></span>

### <span data-ttu-id="89b1a-258"><a name="verifyranges"></a>Krok 6: Ověření rozsahů IP adres</span><span class="sxs-lookup"><span data-stu-id="89b1a-258"><a name="verifyranges"></a>Step 6 - Verify the IP address ranges</span></span>

<span data-ttu-id="89b1a-259">Při vytváření dalších připojení je důležité ověřit, že se adresní prostor IP adres nové virtuální sítě nepřekrývá se žádným z rozsahů jiných virtuálních sítí ani rozsahů bran místních sítí.</span><span class="sxs-lookup"><span data-stu-id="89b1a-259">When creating additional connections, it's important to verify that the IP address space of the new virtual network does not overlap with any of your other VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="89b1a-260">Pro tento postup použijte následující hodnoty pro virtuální síť TestVNet5:</span><span class="sxs-lookup"><span data-stu-id="89b1a-260">For this exercise, you can use the following values for the TestVNet5:</span></span>

<span data-ttu-id="89b1a-261">**Hodnoty pro virtuální síť TestVNet5:**</span><span class="sxs-lookup"><span data-stu-id="89b1a-261">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="89b1a-262">Název virtuální sítě: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="89b1a-262">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="89b1a-263">Skupina prostředků: TestRG5</span><span class="sxs-lookup"><span data-stu-id="89b1a-263">Resource Group: TestRG5</span></span>
* <span data-ttu-id="89b1a-264">Umístění: Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="89b1a-264">Location: Japan East</span></span>
* <span data-ttu-id="89b1a-265">TestVNet5: 10.51.0.0/16 a 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="89b1a-265">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="89b1a-266">FrontEnd: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="89b1a-266">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="89b1a-267">BackEnd: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="89b1a-267">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="89b1a-268">GatewaySubnet: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="89b1a-268">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="89b1a-269">Název brány: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="89b1a-269">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="89b1a-270">Veřejná IP adresa: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="89b1a-270">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="89b1a-271">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="89b1a-271">VPNType: RouteBased</span></span>
* <span data-ttu-id="89b1a-272">Připojení: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="89b1a-272">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="89b1a-273">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="89b1a-273">ConnectionType: VNet2VNet</span></span>

### <span data-ttu-id="89b1a-274"><a name="TestVNet5"></a>Krok 7: Vytvoření a konfigurace virtuální sítě TestVNet5</span><span class="sxs-lookup"><span data-stu-id="89b1a-274"><a name="TestVNet5"></a>Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="89b1a-275">Tento krok je třeba provést v rámci nového předplatného (předplatné 5).</span><span class="sxs-lookup"><span data-stu-id="89b1a-275">This step must be done in the context of the new subscription, Subscription 5.</span></span> <span data-ttu-id="89b1a-276">Tuto část může provést správce v organizaci, která je vlastníkem druhého předplatného.</span><span class="sxs-lookup"><span data-stu-id="89b1a-276">This part may be performed by the administrator in a different organization that owns the subscription.</span></span> <span data-ttu-id="89b1a-277">Pro přepínání mezi předplatnými použijte příkaz „az account list --all“, který vypíše dostupná předplatná pro váš účet, a pak pomocí příkazu „az account set --subscription <subscriptionID>“ přepněte na předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="89b1a-277">To switch between subscriptions use 'az account list --all' to list the subscriptions available to your account, then use 'az account set --subscription <subscriptionID>' to switch to the subscription that you want to use.</span></span>

1. <span data-ttu-id="89b1a-278">Ujistěte se, že jste připojeni k předplatnému 5, a pak vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="89b1a-278">Make sure you are connected to Subscription 5, then create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG5  -l japaneast
  ```
2. <span data-ttu-id="89b1a-279">Vytvořte virtuální síť TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="89b1a-279">Create TestVNet5.</span></span>

  ```azurecli
  az network vnet create -n TestVNet5 -g TestRG5 --address-prefix 10.51.0.0/16 -l japaneast --subnet-name FrontEnd --subnet-prefix 10.51.0.0/24
  ```

3. <span data-ttu-id="89b1a-280">Přidejte podsítě.</span><span class="sxs-lookup"><span data-stu-id="89b1a-280">Add subnets.</span></span>

  ```azurecli
  az network vnet update -n TestVNet5 --address-prefixes 10.51.0.0/16 10.52.0.0/16 -g TestRG5
  az network vnet subnet create --vnet-name TestVNet5 -n BackEnd -g TestRG5 --address-prefix 10.52.0.0/24
  ```

4. <span data-ttu-id="89b1a-281">Přidejte podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="89b1a-281">Add the gateway subnet.</span></span>

  ```azurecli
  az network vnet subnet create --vnet-name TestVNet5 -n GatewaySubnet -g TestRG5 --address-prefix 10.52.255.0/27
  ```

5. <span data-ttu-id="89b1a-282">Vyžádejte si veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="89b1a-282">Request a public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet5GWIP -g TestRG5 --allocation-method Dynamic
  ```
6. <span data-ttu-id="89b1a-283">Vytvoření brány virtuální sítě TestVNet5</span><span class="sxs-lookup"><span data-stu-id="89b1a-283">Create the TestVNet5 gateway</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet5GW -l japaneast --public-ip-address VNet5GWIP -g TestRG5 --vnet TestVNet5 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="89b1a-284"><a name="connections5"></a>Krok 8: Vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="89b1a-284"><a name="connections5"></a>Step 8 - Create the connections</span></span>

<span data-ttu-id="89b1a-285">Jelikož brány patří do různých předplatných, rozdělíme tento krok do dvou relací rozhraní příkazového řádku označených jako **[Předplatné 1]** a **[Předplatné 5]**.</span><span class="sxs-lookup"><span data-stu-id="89b1a-285">We split this step into two CLI sessions marked as **[Subscription 1]**, and **[Subscription 5]** because the gateways are in the different subscriptions.</span></span> <span data-ttu-id="89b1a-286">Pro přepínání mezi předplatnými použijte příkaz „az account list --all“, který vypíše dostupná předplatná pro váš účet, a pak pomocí příkazu „az account set --subscription <subscriptionID>“ přepněte na předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="89b1a-286">To switch between subscriptions use 'az account list --all' to list the subscriptions available to your account, then use 'az account set --subscription <subscriptionID>' to switch to the subscription that you want to use.</span></span>

1. <span data-ttu-id="89b1a-287">**[Předplatné 1]** Přihlaste a připojte se k předplatnému 1.</span><span class="sxs-lookup"><span data-stu-id="89b1a-287">**[Subscription 1]** Log in and connect to Subscription 1.</span></span> <span data-ttu-id="89b1a-288">Spusťte následující příkaz a z výstupu získejte název a ID brány:</span><span class="sxs-lookup"><span data-stu-id="89b1a-288">Run the following command to get the name and ID of the Gateway from the output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="89b1a-289">Zkopírujte část výstupu uvedeného textem „id:“.</span><span class="sxs-lookup"><span data-stu-id="89b1a-289">Copy the output for "id:".</span></span> <span data-ttu-id="89b1a-290">E-mailem nebo jiným způsobem odešlete ID a název brány virtuální sítě (VNet1GW) správci předplatného 5.</span><span class="sxs-lookup"><span data-stu-id="89b1a-290">Send the ID and the name of the VNet gateway (VNet1GW) to the administrator of Subscription 5 via email or another method.</span></span>

  <span data-ttu-id="89b1a-291">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="89b1a-291">Example output:</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
  ```

2. <span data-ttu-id="89b1a-292">**[Předplatné 5]** Přihlaste a připojte se k předplatnému 5.</span><span class="sxs-lookup"><span data-stu-id="89b1a-292">**[Subscription 5]** Log in and connect to Subscription 5.</span></span> <span data-ttu-id="89b1a-293">Spusťte následující příkaz a z výstupu získejte název a ID brány:</span><span class="sxs-lookup"><span data-stu-id="89b1a-293">Run the following command to get the name and ID of the Gateway from the output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet5GW -g TestRG5
  ```

  <span data-ttu-id="89b1a-294">Zkopírujte část výstupu uvedeného textem „id:“.</span><span class="sxs-lookup"><span data-stu-id="89b1a-294">Copy the output for "id:".</span></span> <span data-ttu-id="89b1a-295">E-mailem nebo jiným způsobem odešlete ID a název brány virtuální sítě (VNet5GW) správci předplatného 1.</span><span class="sxs-lookup"><span data-stu-id="89b1a-295">Send the ID and the name of the VNet gateway (VNet5GW) to the administrator of Subscription 1 via email or another method.</span></span>

3. <span data-ttu-id="89b1a-296">**[Předplatné 1]** V tomto kroku vytvoříte připojení z virtuální sítě TestVNet1 k virtuální síti TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="89b1a-296">**[Subscription 1]** In this step, you create the connection from TestVNet1 to TestVNet5.</span></span> <span data-ttu-id="89b1a-297">Pro sdílený klíč můžete použít vlastní hodnoty, ale sdílené klíče pro obě připojení se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="89b1a-297">You can use your own values for the shared key, however, the shared key must match for both connections.</span></span> <span data-ttu-id="89b1a-298">Vytvoření připojení může nějakou dobu trvat.</span><span class="sxs-lookup"><span data-stu-id="89b1a-298">Creating a connection can take a short while to complete.</span></span> <span data-ttu-id="89b1a-299">Ujistěte se, že jste připojeni k předplatnému 1.</span><span class="sxs-lookup"><span data-stu-id="89b1a-299">Make sure you connect to Subscription 1.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet5 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```

4. <span data-ttu-id="89b1a-300">**[Předplatné 5]** Tento krok je podobný předchozímu, vytváříte však připojení z virtuální sítě TestVNet5 k virtuální síti TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="89b1a-300">**[Subscription 5]** This step is similar to the one above, except you are creating the connection from TestVNet5 to TestVNet1.</span></span> <span data-ttu-id="89b1a-301">Zkontrolujte, že se sdílené klíče shodují a že se připojujete k předplatnému 5.</span><span class="sxs-lookup"><span data-stu-id="89b1a-301">Make sure that the shared keys match and that you connect to Subscription 5.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet5ToVNet1 -g TestRG5 --vnet-gateway1 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW -l japaneast --shared-key "eeffgg" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```

## <span data-ttu-id="89b1a-302"><a name="verify"></a>Ověření stavu připojení</span><span class="sxs-lookup"><span data-stu-id="89b1a-302"><a name="verify"></a>Verify the connections</span></span>
[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections v2v cli](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <span data-ttu-id="89b1a-303"><a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="89b1a-303"><a name="faq"></a>VNet-to-VNet FAQ</span></span>
[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="89b1a-304">Další kroky</span><span class="sxs-lookup"><span data-stu-id="89b1a-304">Next steps</span></span>

* <span data-ttu-id="89b1a-305">Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="89b1a-305">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="89b1a-306">Další informace najdete v [dokumentaci ke službě Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="89b1a-306">For more information, see the [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="89b1a-307">Informace o protokolu BGP najdete v tématech [Přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci protokolu BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="89b1a-307">For information about BGP, see the [BGP Overview](vpn-gateway-bgp-overview.md) and [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
