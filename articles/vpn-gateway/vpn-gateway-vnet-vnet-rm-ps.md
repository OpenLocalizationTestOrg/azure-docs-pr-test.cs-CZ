---
title: "Připojení virtuální sítě Azure k jiné virtuální síti: PowerShell | Dokumentace Microsoftu"
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
ms.openlocfilehash: 8c42c0046ccaa98c572134042fbbb7e883ef93c3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a><span data-ttu-id="b86db-103">Konfigurace připojení brány VPN typu VNet-to-VNet pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="b86db-103">Configure a VNet-to-VNet VPN gateway connection using PowerShell</span></span>

<span data-ttu-id="b86db-104">Tento článek ukazuje, jak vytvořit připojení brány VPN mezi virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="b86db-104">This article shows you how to create a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="b86db-105">Virtuální sítě se můžou nacházet ve stejné oblasti nebo v různých oblastech a můžou patřit do stejného předplatného nebo do různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="b86db-105">The virtual networks can be in the same or different regions, and from the same or different subscriptions.</span></span> <span data-ttu-id="b86db-106">Pokud připojujete virtuální sítě z různých předplatných, tato předplatná nemusí být přidružená ke stejnému tenantovi Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b86db-106">When connecting VNets from different subscriptions, the subscriptions do not need to be associated with the same Active Directory tenant.</span></span> 

<span data-ttu-id="b86db-107">Postupy v tomto článku se týkají modelu nasazení Resource Manager a používají PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b86db-107">The steps in this article apply to the Resource Manager deployment model and use PowerShell.</span></span> <span data-ttu-id="b86db-108">Tuto konfiguraci můžete vytvořit také pomocí jiného nástroje nasazení nebo pro jiný model nasazení, a to výběrem jiné možnosti z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="b86db-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b86db-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b86db-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="b86db-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b86db-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="b86db-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b86db-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="b86db-112">Azure Portal (Classic)</span><span class="sxs-lookup"><span data-stu-id="b86db-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="b86db-113">Propojení různých modelů nasazení – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b86db-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="b86db-114">Propojení různých modelů nasazení – PowerShell</span><span class="sxs-lookup"><span data-stu-id="b86db-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="b86db-115">Propojení virtuální sítě s jinou virtuální sítí (VNet-to-VNet) je podobné propojení virtuální sítě s místním serverem.</span><span class="sxs-lookup"><span data-stu-id="b86db-115">Connecting a virtual network to another virtual network (VNet-to-VNet) is similar to connecting a VNet to an on-premises site location.</span></span> <span data-ttu-id="b86db-116">Oba typy připojení využívají bránu VPN k poskytnutí zabezpečeného tunelového propojení prostřednictvím protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="b86db-116">Both connectivity types use a VPN gateway to provide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="b86db-117">Pokud se virtuální sítě nacházejí ve stejné oblasti, můžete uvažovat o jejich propojení vytvořením partnerského vztahu virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="b86db-117">If your VNets are in the same region, you may want to consider connecting them using VNet Peering.</span></span> <span data-ttu-id="b86db-118">Partnerské vztahy virtuálních sítí nepoužívají bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="b86db-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="b86db-119">Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b86db-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="b86db-120">Komunikaci typu VNet-to-VNet můžete kombinovat s konfiguracemi s více servery.</span><span class="sxs-lookup"><span data-stu-id="b86db-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="b86db-121">Díky tomu je možné vytvářet topologie sítí, ve kterých se používá propojování více míst i propojování virtuálních sítí, jak je znázorněno v následujícím schématu:</span><span class="sxs-lookup"><span data-stu-id="b86db-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in the following diagram:</span></span>

![Informace o připojeních](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a><span data-ttu-id="b86db-123">Proč propojovat virtuální sítě?</span><span class="sxs-lookup"><span data-stu-id="b86db-123">Why connect virtual networks?</span></span>

<span data-ttu-id="b86db-124">Virtuální sítě může být vhodné propojit z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="b86db-124">You may want to connect virtual networks for the following reasons:</span></span>

* <span data-ttu-id="b86db-125">**Geografická redundance napříč oblastmi a geografická přítomnost**</span><span class="sxs-lookup"><span data-stu-id="b86db-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="b86db-126">Můžete nastavit vlastní geografickou replikaci nebo synchronizaci se zabezpečeným připojením bez procházení koncovými body připojenými k internetu.</span><span class="sxs-lookup"><span data-stu-id="b86db-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="b86db-127">Pomocí Azure Traffic Manageru a služby Load Balancer je možné vytvářet úlohy s vysokou dostupností s geografickou redundancí nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="b86db-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="b86db-128">Jedním z důležitých příkladů je nastavení technologie SQL Always On se skupinami dostupnosti nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="b86db-128">One important example is to set up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="b86db-129">**Regionální vícevrstvé aplikace s izolací nebo administrativní hranicí**</span><span class="sxs-lookup"><span data-stu-id="b86db-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="b86db-130">V rámci stejné oblasti můžete vytvářet vícevrstvé aplikace s několika virtuálními sítěmi propojenými z důvodu izolace nebo požadavků na správu.</span><span class="sxs-lookup"><span data-stu-id="b86db-130">Within the same region, you can set up multi-tier applications with multiple virtual networks connected together due to isolation or administrative requirements.</span></span>

<span data-ttu-id="b86db-131">Další informace o propojeních VNet-to-VNet najdete v části [Nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="b86db-131">For more information about VNet-to-VNet connections, see the [VNet-to-VNet FAQ](#faq) at the end of this article.</span></span>

## <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="b86db-132">Kterou posloupnost kroků provést?</span><span class="sxs-lookup"><span data-stu-id="b86db-132">Which set of steps should I use?</span></span>

<span data-ttu-id="b86db-133">V tomto článku uvidíte dvě různé sady kroků.</span><span class="sxs-lookup"><span data-stu-id="b86db-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="b86db-134">Jedna sada kroků pro [virtuální sítě spadající do stejného předplatného](#samesub) a druhá sada kroků pro [virtuální sítě v různých předplatných](#difsub).</span><span class="sxs-lookup"><span data-stu-id="b86db-134">One set of steps for [VNets that reside in the same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span> <span data-ttu-id="b86db-135">Hlavní rozdíl mezi oběma postupy spočívá v tom, jestli je možné vytvářet a konfigurovat všechny prostředky virtuální sítě a brány v téže relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b86db-135">The key difference between the sets is whether you can create and configure all virtual network and gateway resources within the same PowerShell session.</span></span>

<span data-ttu-id="b86db-136">Kroky v tomto článku používají proměnné, které jsou deklarované na začátku každé části.</span><span class="sxs-lookup"><span data-stu-id="b86db-136">The steps in this article use variables that are declared at the beginning of each section.</span></span> <span data-ttu-id="b86db-137">Pokud již pracujete s existujícími virtuálními sítěmi, upravte proměnné tak, aby odrážely nastavení vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="b86db-137">If you already are working with existing VNets, modify the variables to reflect the settings in your own environment.</span></span> <span data-ttu-id="b86db-138">Pokud chcete překlad IP adres pro virtuální sítě, přečtěte si téma [Překlad IP adres](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="b86db-138">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

## <span data-ttu-id="b86db-139"><a name="samesub"></a>Postup při propojování virtuálních sítí patřících ke stejnému předplatnému</span><span class="sxs-lookup"><span data-stu-id="b86db-139"><a name="samesub"></a>How to connect VNets that are in the same subscription</span></span>

![Diagram v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="b86db-141">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b86db-141">Before you begin</span></span>

<span data-ttu-id="b86db-142">Než začnete, bude třeba nainstalovat nejnovější verzi rutin PowerShellu pro Azure Resource Manager, alespoň verzi 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b86db-142">Before beginning, you need to install the latest version of the Azure Resource Manager PowerShell cmdlets, at least 4.0 or later.</span></span> <span data-ttu-id="b86db-143">Další informace o instalaci rutin PowerShellu najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b86db-143">For more information about installing the PowerShell cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <span data-ttu-id="b86db-144"><a name="Step1"></a>Krok 1: Plánování rozsahů IP adres</span><span class="sxs-lookup"><span data-stu-id="b86db-144"><a name="Step1"></a>Step 1 - Plan your IP address ranges</span></span>

<span data-ttu-id="b86db-145">V následujících krocích vytvoříme dvě virtuální sítě spolu s příslušnými podsítěmi a konfiguracemi brány.</span><span class="sxs-lookup"><span data-stu-id="b86db-145">In the following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="b86db-146">Poté vytvoříme propojení VPN mezi oběma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="b86db-146">We then create a VPN connection between the two VNets.</span></span> <span data-ttu-id="b86db-147">Je důležité určit rozsahy IP adres pro konfiguraci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="b86db-147">It’s important to plan the IP address ranges for your network configuration.</span></span> <span data-ttu-id="b86db-148">Mějte na paměti, že je třeba zajistit, aby se žádné rozsahy virtuálních sítí ani místní síťové rozsahy žádným způsobem nepřekrývaly.</span><span class="sxs-lookup"><span data-stu-id="b86db-148">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="b86db-149">V těchto příkladech nezahrnujeme server DNS.</span><span class="sxs-lookup"><span data-stu-id="b86db-149">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="b86db-150">Pokud chcete překlad IP adres pro virtuální sítě, přečtěte si téma [Překlad IP adres](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="b86db-150">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="b86db-151">V příkladech používáme následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b86db-151">We use the following values in the examples:</span></span>

<span data-ttu-id="b86db-152">**Hodnoty pro virtuální síť TestVNet1:**</span><span class="sxs-lookup"><span data-stu-id="b86db-152">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="b86db-153">Název virtuální sítě: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="b86db-153">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="b86db-154">Skupina prostředků: TestRG1</span><span class="sxs-lookup"><span data-stu-id="b86db-154">Resource Group: TestRG1</span></span>
* <span data-ttu-id="b86db-155">Umístění: Východní USA</span><span class="sxs-lookup"><span data-stu-id="b86db-155">Location: East US</span></span>
* <span data-ttu-id="b86db-156">TestVNet1: 10.11.0.0/16 a 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="b86db-156">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="b86db-157">FrontEnd: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="b86db-157">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="b86db-158">BackEnd: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="b86db-158">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="b86db-159">GatewaySubnet: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="b86db-159">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="b86db-160">Název brány: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="b86db-160">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="b86db-161">Veřejná IP adresa: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="b86db-161">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="b86db-162">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="b86db-162">VPNType: RouteBased</span></span>
* <span data-ttu-id="b86db-163">Připojení (1 ke 4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="b86db-163">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="b86db-164">Připojení (1 k 5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="b86db-164">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="b86db-165">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="b86db-165">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="b86db-166">**Hodnoty pro virtuální síť TestVNet4:**</span><span class="sxs-lookup"><span data-stu-id="b86db-166">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="b86db-167">Název virtuální sítě: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="b86db-167">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="b86db-168">TestVNet2: 10.41.0.0/16 a 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="b86db-168">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="b86db-169">FrontEnd: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="b86db-169">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="b86db-170">BackEnd: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="b86db-170">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="b86db-171">GatewaySubnet: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="b86db-171">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="b86db-172">Skupina prostředků: TestRG4</span><span class="sxs-lookup"><span data-stu-id="b86db-172">Resource Group: TestRG4</span></span>
* <span data-ttu-id="b86db-173">Umístění: Západní USA</span><span class="sxs-lookup"><span data-stu-id="b86db-173">Location: West US</span></span>
* <span data-ttu-id="b86db-174">Název brány: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="b86db-174">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="b86db-175">Veřejná IP adresa: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="b86db-175">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="b86db-176">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="b86db-176">VPNType: RouteBased</span></span>
* <span data-ttu-id="b86db-177">Připojení: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="b86db-177">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="b86db-178">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="b86db-178">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="b86db-179"><a name="Step2"></a>Krok 2: Vytvoření a konfigurace virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="b86db-179"><a name="Step2"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="b86db-180">Deklarujte proměnné.</span><span class="sxs-lookup"><span data-stu-id="b86db-180">Declare your variables.</span></span> <span data-ttu-id="b86db-181">V tomto příkladu jsou proměnné deklarovány s použitím hodnot pro tento ukázkový postup.</span><span class="sxs-lookup"><span data-stu-id="b86db-181">This example declares the variables using the values for this exercise.</span></span> <span data-ttu-id="b86db-182">Ve většině případů byste měli hodnoty nahradit vlastními.</span><span class="sxs-lookup"><span data-stu-id="b86db-182">In most cases, you should replace the values with your own.</span></span> <span data-ttu-id="b86db-183">Tyto hodnoty proměnných ale můžete použít, pokud procházíte kroky, abyste se seznámili s tímto typem konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b86db-183">However, you can use these variables if you are running through the steps to become familiar with this type of configuration.</span></span> <span data-ttu-id="b86db-184">Upravte proměnné podle potřeby a pak je zkopírujte a vložte do konzoly PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b86db-184">Modify the variables if needed, then copy and paste them into your PowerShell console.</span></span>

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

2. <span data-ttu-id="b86db-185">Připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="b86db-185">Connect to your account.</span></span> <span data-ttu-id="b86db-186">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="b86db-186">Use the following example to help you connect:</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="b86db-187">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="b86db-187">Check the subscriptions for the account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

  <span data-ttu-id="b86db-188">Určete předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="b86db-188">Specify the subscription that you want to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub1
  ```
3. <span data-ttu-id="b86db-189">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b86db-189">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```
4. <span data-ttu-id="b86db-190">Vytvořte konfigurace podsítí pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b86db-190">Create the subnet configurations for TestVNet1.</span></span> <span data-ttu-id="b86db-191">Tato ukázka vytvoří virtuální síť s názvem TestVNet1 a tři podsítě: jednu s názvem GatewaySubnet, jednu s názvem FrontEnd a jednu s názvem BackEnd.</span><span class="sxs-lookup"><span data-stu-id="b86db-191">This example creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="b86db-192">Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="b86db-192">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="b86db-193">Pokud použijete jiný název, vytvoření brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="b86db-193">If you name it something else, your gateway creation fails.</span></span>

  <span data-ttu-id="b86db-194">Následující příklad používá proměnné, které jste nastavili dříve.</span><span class="sxs-lookup"><span data-stu-id="b86db-194">The following example uses the variables that you set earlier.</span></span> <span data-ttu-id="b86db-195">V příkladu používá podsíť brány možnost /27.</span><span class="sxs-lookup"><span data-stu-id="b86db-195">In this example, the gateway subnet is using a /27.</span></span> <span data-ttu-id="b86db-196">I když je možné vytvořit podsíť brány s minimální velikostí /29, doporučujeme vytvořit větší podsíť, která pojme více adres, tzn. vybrat velikost alespoň /28 nebo /27.</span><span class="sxs-lookup"><span data-stu-id="b86db-196">While it is possible to create a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="b86db-197">Tím vznikne dostatečný prostor pro adresy, který umožní nastavení případných dalších konfigurací v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="b86db-197">This will allow for enough addresses to accommodate possible additional configurations that you may want in the future.</span></span>

  ```powershell
  $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
  $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
  $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
  ```
5. <span data-ttu-id="b86db-198">Vytvořte virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b86db-198">Create TestVNet1.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
  ```
6. <span data-ttu-id="b86db-199">Vyžádejte si veřejnou IP adresu, která bude přidělena bráně, kterou vytvoříte pro příslušnou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="b86db-199">Request a public IP address to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="b86db-200">Všimněte si, že metoda AllocationMethod je dynamická.</span><span class="sxs-lookup"><span data-stu-id="b86db-200">Notice that the AllocationMethod is Dynamic.</span></span> <span data-ttu-id="b86db-201">Není možné určit IP adresu, kterou chcete používat.</span><span class="sxs-lookup"><span data-stu-id="b86db-201">You cannot specify the IP address that you want to use.</span></span> <span data-ttu-id="b86db-202">Přiděluje se pro bránu dynamicky.</span><span class="sxs-lookup"><span data-stu-id="b86db-202">It's dynamically allocated to your gateway.</span></span> 

  ```powershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
  ```
7. <span data-ttu-id="b86db-203">Vytvořte konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="b86db-203">Create the gateway configuration.</span></span> <span data-ttu-id="b86db-204">Konfigurace brány definuje podsíť a veřejnou IP adresu, která se bude používat.</span><span class="sxs-lookup"><span data-stu-id="b86db-204">The gateway configuration defines the subnet and the public IP address to use.</span></span> <span data-ttu-id="b86db-205">Podle následující ukázky vytvořte vlastní konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="b86db-205">Use the example to create your gateway configuration.</span></span>

  ```powershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```
8. <span data-ttu-id="b86db-206">Vytvořte bránu pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b86db-206">Create the gateway for TestVNet1.</span></span> <span data-ttu-id="b86db-207">V tomto kroku vytvoříte bránu virtuální sítě pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b86db-207">In this step, you create the virtual network gateway for your TestVNet1.</span></span> <span data-ttu-id="b86db-208">Konfigurace propojení VNet-to-VNet vyžadují typ sítě VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="b86db-208">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="b86db-209">Vytvoření brány může obvykle trvat 45 minut nebo déle, a to v závislosti na vybrané skladové jednotce (SKU) brány.</span><span class="sxs-lookup"><span data-stu-id="b86db-209">Creating a gateway can often take 45 minutes or more, depending on the selected gateway SKU.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-3---create-and-configure-testvnet4"></a><span data-ttu-id="b86db-210">Krok 3: Vytvoření a konfigurace virtuální sítě TestVNet4</span><span class="sxs-lookup"><span data-stu-id="b86db-210">Step 3 - Create and configure TestVNet4</span></span>

<span data-ttu-id="b86db-211">Po konfiguraci virtuální sítě TestVNet1 vytvořte virtuální síť TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="b86db-211">Once you've configured TestVNet1, create TestVNet4.</span></span> <span data-ttu-id="b86db-212">Postupujte podle následujících kroků a podle potřeby nahrazujte hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="b86db-212">Follow the steps below, replacing the values with your own when needed.</span></span> <span data-ttu-id="b86db-213">Tento krok lze provést v rámci stejné relace prostředí PowerShell, protože se jedná o stejné předplatné.</span><span class="sxs-lookup"><span data-stu-id="b86db-213">This step can be done within the same PowerShell session because it is in the same subscription.</span></span>

1. <span data-ttu-id="b86db-214">Deklarujte proměnné.</span><span class="sxs-lookup"><span data-stu-id="b86db-214">Declare your variables.</span></span> <span data-ttu-id="b86db-215">Nezapomeňte nahradit hodnoty těmi, které chcete použít pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b86db-215">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

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
2. <span data-ttu-id="b86db-216">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b86db-216">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG4 -Location $Location4
  ```
3. <span data-ttu-id="b86db-217">Vytvořte konfigurace podsítí pro virtuální síť TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="b86db-217">Create the subnet configurations for TestVNet4.</span></span>

  ```powershell
  $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
  $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
  $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4
  ```
4. <span data-ttu-id="b86db-218">Vytvořte virtuální síť TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="b86db-218">Create TestVNet4.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4
  ```
5. <span data-ttu-id="b86db-219">Vyžádejte si veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b86db-219">Request a public IP address.</span></span>

  ```powershell
  $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AllocationMethod Dynamic
  ```
6. <span data-ttu-id="b86db-220">Vytvořte konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="b86db-220">Create the gateway configuration.</span></span>

  ```powershell
  $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
  $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
  $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4
  ```
7. <span data-ttu-id="b86db-221">Vytvořte bránu virtuální sítě TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="b86db-221">Create the TestVNet4 gateway.</span></span> <span data-ttu-id="b86db-222">Vytvoření brány může obvykle trvat 45 minut nebo déle, a to v závislosti na vybrané skladové jednotce (SKU) brány.</span><span class="sxs-lookup"><span data-stu-id="b86db-222">Creating a gateway can often take 45 minutes or more, depending on the selected gateway SKU.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
  -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-4---create-the-connections"></a><span data-ttu-id="b86db-223">Krok 4: Vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="b86db-223">Step 4 - Create the connections</span></span>

1. <span data-ttu-id="b86db-224">Získejte obě brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b86db-224">Get both virtual network gateways.</span></span> <span data-ttu-id="b86db-225">Pokud jsou obě brány ve stejném předplatném, jako je tomu v příkladu, můžete tento krok dokončit ve stejné relaci PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="b86db-225">If both of the gateways are in the same subscription, as they are in the example, you can complete this step in the same PowerShell session.</span></span>

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4
  ```
2. <span data-ttu-id="b86db-226">Vytvořte připojení virtuální sítě TestVNet1 k virtuální síti TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="b86db-226">Create the TestVNet1 to TestVNet4 connection.</span></span> <span data-ttu-id="b86db-227">V tomto kroku vytvoříte připojení z virtuální sítě TestVNet1 do virtuální sítě TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="b86db-227">In this step, you create the connection from TestVNet1 to TestVNet4.</span></span> <span data-ttu-id="b86db-228">Zobrazí se sdílený klíč uváděný v příkladech.</span><span class="sxs-lookup"><span data-stu-id="b86db-228">You'll see a shared key referenced in the examples.</span></span> <span data-ttu-id="b86db-229">Pro sdílený klíč můžete použít vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b86db-229">You can use your own values for the shared key.</span></span> <span data-ttu-id="b86db-230">Důležité je, že se sdílený klíč pro obě připojení musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="b86db-230">The important thing is that the shared key must match for both connections.</span></span> <span data-ttu-id="b86db-231">Vytvoření připojení může nějakou dobu trvat.</span><span class="sxs-lookup"><span data-stu-id="b86db-231">Creating a connection can take a short while to complete.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
3. <span data-ttu-id="b86db-232">Vytvořte připojení virtuální sítě TestVNet4 k virtuální síti TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b86db-232">Create the TestVNet4 to TestVNet1 connection.</span></span> <span data-ttu-id="b86db-233">Tento krok je podobný předchozímu, vytváříte však připojení z virtuální sítě TestVNet4 do virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b86db-233">This step is similar to the one above, except you are creating the connection from TestVNet4 to TestVNet1.</span></span> <span data-ttu-id="b86db-234">Ověřte, že se sdílené klíče shodují.</span><span class="sxs-lookup"><span data-stu-id="b86db-234">Make sure the shared keys match.</span></span> <span data-ttu-id="b86db-235">Připojení se vytvoří během několika minut.</span><span class="sxs-lookup"><span data-stu-id="b86db-235">The connection will be established after a few minutes.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
  -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. <span data-ttu-id="b86db-236">Ověřte své propojení.</span><span class="sxs-lookup"><span data-stu-id="b86db-236">Verify your connection.</span></span> <span data-ttu-id="b86db-237">Viz část [Ověření připojení](#verify).</span><span class="sxs-lookup"><span data-stu-id="b86db-237">See the section [How to verify your connection](#verify).</span></span>

## <span data-ttu-id="b86db-238"><a name="difsub"></a>Postup při propojování virtuálních sítí patřících k různým předplatným</span><span class="sxs-lookup"><span data-stu-id="b86db-238"><a name="difsub"></a>How to connect VNets that are in different subscriptions</span></span>

![Diagram v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

<span data-ttu-id="b86db-240">V tomto scénáři propojíme sítě TestVNet1 a TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="b86db-240">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="b86db-241">Virtuální sítě TestVNet1 a TestVNet5 patří do různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="b86db-241">TestVNet1 and TestVNet5 reside in a different subscription.</span></span> <span data-ttu-id="b86db-242">Předplatná nemusí být přidružená ke stejnému tenantovi Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b86db-242">The subscriptions do not need to be associated with the same Active Directory tenant.</span></span> <span data-ttu-id="b86db-243">Rozdíl mezi těmito kroky a předchozí sadou spočívá v tom, že část kroků konfigurace je třeba provést v samostatné relaci PowerShellu v kontextu druhého předplatného.</span><span class="sxs-lookup"><span data-stu-id="b86db-243">The difference between these steps and the previous set is that some of the configuration steps need to be performed in a separate PowerShell session in the context of the second subscription.</span></span> <span data-ttu-id="b86db-244">To je zvláště podstatné, když druhé předplatné patří jiné organizaci.</span><span class="sxs-lookup"><span data-stu-id="b86db-244">Especially when the two subscriptions belong to different organizations.</span></span>

### <a name="step-5---create-and-configure-testvnet1"></a><span data-ttu-id="b86db-245">Krok 5: Vytvoření a konfigurace virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="b86db-245">Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="b86db-246">Je třeba vytvořit a konfigurovat virtuální síť TestVNet1 a bránu VPN Gateway pro virtuální síť TestVNet1 provedením [kroku 1](#Step1) a [kroku 2](#Step2) z předchozí části.</span><span class="sxs-lookup"><span data-stu-id="b86db-246">You must complete [Step 1](#Step1) and [Step 2](#Step2) from the previous section to create and configure TestVNet1 and the VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="b86db-247">Pro tuto konfiguraci není nutné vytvářet virtuální síť TestVNet4 z předchozí části, ale pokud ji vytvoříte, nebude to s těmito kroky v konfliktu.</span><span class="sxs-lookup"><span data-stu-id="b86db-247">For this configuration, you are not required to create TestVNet4 from the previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="b86db-248">Po dokončení kroků 1 a 2 pokračujte krokem 6 a vytvořte síť TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="b86db-248">Once you complete Step 1 and Step 2, continue with Step 6 to create TestVNet5.</span></span> 

### <a name="step-6---verify-the-ip-address-ranges"></a><span data-ttu-id="b86db-249">Krok 6: Ověření rozsahů IP adres</span><span class="sxs-lookup"><span data-stu-id="b86db-249">Step 6 - Verify the IP address ranges</span></span>

<span data-ttu-id="b86db-250">Je důležité zajistit, aby se prostor IP adres nové virtuální sítě TestVNet5 nepřekrýval se žádným z rozsahů virtuálních sítí ani rozsahů bran místních sítí.</span><span class="sxs-lookup"><span data-stu-id="b86db-250">It is important to make sure that the IP address space of the new virtual network, TestVNet5, does not overlap with any of your VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="b86db-251">V tomto příkladu můžou virtuální sítě různým organizacím.</span><span class="sxs-lookup"><span data-stu-id="b86db-251">In this example, the virtual networks may belong to different organizations.</span></span> <span data-ttu-id="b86db-252">Pro tento postup použijte následující hodnoty pro virtuální síť TestVNet5:</span><span class="sxs-lookup"><span data-stu-id="b86db-252">For this exercise, you can use the following values for the TestVNet5:</span></span>

<span data-ttu-id="b86db-253">**Hodnoty pro virtuální síť TestVNet5:**</span><span class="sxs-lookup"><span data-stu-id="b86db-253">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="b86db-254">Název virtuální sítě: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="b86db-254">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="b86db-255">Skupina prostředků: TestRG5</span><span class="sxs-lookup"><span data-stu-id="b86db-255">Resource Group: TestRG5</span></span>
* <span data-ttu-id="b86db-256">Umístění: Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="b86db-256">Location: Japan East</span></span>
* <span data-ttu-id="b86db-257">TestVNet5: 10.51.0.0/16 a 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="b86db-257">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="b86db-258">FrontEnd: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="b86db-258">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="b86db-259">BackEnd: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="b86db-259">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="b86db-260">GatewaySubnet: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="b86db-260">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="b86db-261">Název brány: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="b86db-261">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="b86db-262">Veřejná IP adresa: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="b86db-262">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="b86db-263">Typ sítě VPN: RouteBased</span><span class="sxs-lookup"><span data-stu-id="b86db-263">VPNType: RouteBased</span></span>
* <span data-ttu-id="b86db-264">Připojení: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="b86db-264">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="b86db-265">Typ připojení: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="b86db-265">ConnectionType: VNet2VNet</span></span>

### <a name="step-7---create-and-configure-testvnet5"></a><span data-ttu-id="b86db-266">Krok 7: Vytvoření a konfigurace virtuální sítě TestVNet5</span><span class="sxs-lookup"><span data-stu-id="b86db-266">Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="b86db-267">Tento krok je třeba provést v rámci nového předplatného.</span><span class="sxs-lookup"><span data-stu-id="b86db-267">This step must be done in the context of the new subscription.</span></span> <span data-ttu-id="b86db-268">Tuto část může provést správce v organizaci, která je vlastníkem druhého předplatného.</span><span class="sxs-lookup"><span data-stu-id="b86db-268">This part may be performed by the administrator in a different organization that owns the subscription.</span></span>

1. <span data-ttu-id="b86db-269">Deklarujte proměnné.</span><span class="sxs-lookup"><span data-stu-id="b86db-269">Declare your variables.</span></span> <span data-ttu-id="b86db-270">Nezapomeňte nahradit hodnoty těmi, které chcete použít pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b86db-270">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

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
2. <span data-ttu-id="b86db-271">Připojte se k předplatnému 5.</span><span class="sxs-lookup"><span data-stu-id="b86db-271">Connect to subscription 5.</span></span> <span data-ttu-id="b86db-272">Otevřete konzolu prostředí PowerShell a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="b86db-272">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="b86db-273">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="b86db-273">Use the following sample to help you connect:</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="b86db-274">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="b86db-274">Check the subscriptions for the account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

  <span data-ttu-id="b86db-275">Určete předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="b86db-275">Specify the subscription that you want to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub5
  ```
3. <span data-ttu-id="b86db-276">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b86db-276">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG5 -Location $Location5
  ```
4. <span data-ttu-id="b86db-277">Vytvořte konfigurace podsítí pro virtuální síť TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="b86db-277">Create the subnet configurations for TestVNet5.</span></span>

  ```powershell
  $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
  $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
  $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5
  ```
5. <span data-ttu-id="b86db-278">Vytvořte virtuální síť TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="b86db-278">Create TestVNet5.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
  -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5
  ```
6. <span data-ttu-id="b86db-279">Vyžádejte si veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b86db-279">Request a public IP address.</span></span>

  ```powershell
  $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
  -Location $Location5 -AllocationMethod Dynamic
  ```
7. <span data-ttu-id="b86db-280">Vytvořte konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="b86db-280">Create the gateway configuration.</span></span>

  ```powershell
  $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
  $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
  $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5
  ```
8. <span data-ttu-id="b86db-281">Vytvořte bránu virtuální sítě TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="b86db-281">Create the TestVNet5 gateway.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
  -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-8---create-the-connections"></a><span data-ttu-id="b86db-282">Krok 8: Vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="b86db-282">Step 8 - Create the connections</span></span>

<span data-ttu-id="b86db-283">Jelikož brány v tomto příkladu patří do různých předplatných, rozdělíme tento krok do dvou relací prostředí PowerShell označených [Předplatné 1] a [Předplatné 5].</span><span class="sxs-lookup"><span data-stu-id="b86db-283">In this example, because the gateways are in the different subscriptions, we've split this step into two PowerShell sessions marked as [Subscription 1] and [Subscription 5].</span></span>

1. <span data-ttu-id="b86db-284">**[Předplatné 1]** Získejte bránu virtuální sítě pro předplatné 1.</span><span class="sxs-lookup"><span data-stu-id="b86db-284">**[Subscription 1]** Get the virtual network gateway for Subscription 1.</span></span> <span data-ttu-id="b86db-285">Před spuštěním následujícího příkladu se přihlaste a připojte k předplatnému 1:</span><span class="sxs-lookup"><span data-stu-id="b86db-285">Log in and connect to Subscription 1 before running the following example:</span></span>

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  ```

  <span data-ttu-id="b86db-286">Zkopírujte výstup následujících prvků a pošlete je správci předplatného 5 prostřednictvím e-mailu nebo jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="b86db-286">Copy the output of the following elements and send these to the administrator of Subscription 5 via email or another method.</span></span>

  ```powershell
  $vnet1gw.Name
  $vnet1gw.Id
  ```

  <span data-ttu-id="b86db-287">Tyto dva prvky budou mít hodnoty podobné výstupu v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b86db-287">These two elements will have values similar to the following example output:</span></span>

  ```
  PS D:\> $vnet1gw.Name
  VNet1GW
  PS D:\> $vnet1gw.Id
  /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```
2. <span data-ttu-id="b86db-288">**[Předplatné 5]** Získejte bránu virtuální sítě pro předplatné 5.</span><span class="sxs-lookup"><span data-stu-id="b86db-288">**[Subscription 5]** Get the virtual network gateway for Subscription 5.</span></span> <span data-ttu-id="b86db-289">Před spuštěním následujícího příkladu se přihlaste a připojte k předplatnému 5:</span><span class="sxs-lookup"><span data-stu-id="b86db-289">Log in and connect to Subscription 5 before running the following example:</span></span>

  ```powershell
  $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5
  ```

  <span data-ttu-id="b86db-290">Zkopírujte výstup následujících prvků a pošlete jej správci předplatného 1 prostřednictvím e-mailu nebo jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="b86db-290">Copy the output of the following elements and send these to the administrator of Subscription 1 via email or another method.</span></span>

  ```powershell
  $vnet5gw.Name
  $vnet5gw.Id
  ```

  <span data-ttu-id="b86db-291">Tyto dva prvky budou mít hodnoty podobné výstupu v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b86db-291">These two elements will have values similar to the following example output:</span></span>

  ```
  PS C:\> $vnet5gw.Name
  VNet5GW
  PS C:\> $vnet5gw.Id
  /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```
3. <span data-ttu-id="b86db-292">**[Předplatné 1]** Vytvořte připojení virtuální sítě TestVNet1 k virtuální síti TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="b86db-292">**[Subscription 1]** Create the TestVNet1 to TestVNet5 connection.</span></span> <span data-ttu-id="b86db-293">V tomto kroku vytvoříte propojení z virtuální sítě TestVNet1 do sítě TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="b86db-293">In this step, you create the connection from TestVNet1 to TestVNet5.</span></span> <span data-ttu-id="b86db-294">Rozdíl zde spočívá v tom, že hodnotu $vnet5gw nelze získat přímo, protože patří do jiného předplatného.</span><span class="sxs-lookup"><span data-stu-id="b86db-294">The difference here is that $vnet5gw cannot be obtained directly because it is in a different subscription.</span></span> <span data-ttu-id="b86db-295">Je třeba vytvořit nový objekt prostředí PowerShell s hodnotami zjištěnými z předplatného 1 v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="b86db-295">You will need to create a new PowerShell object with the values communicated from Subscription 1 in the steps above.</span></span> <span data-ttu-id="b86db-296">Postupujte podle následujícího příkladu.</span><span class="sxs-lookup"><span data-stu-id="b86db-296">Use the example below.</span></span> <span data-ttu-id="b86db-297">Nahraďte název, ID a sdílený klíč vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="b86db-297">Replace the Name, Id, and shared key with your own values.</span></span> <span data-ttu-id="b86db-298">Důležité je, že se sdílený klíč pro obě připojení musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="b86db-298">The important thing is that the shared key must match for both connections.</span></span> <span data-ttu-id="b86db-299">Vytvoření připojení může nějakou dobu trvat.</span><span class="sxs-lookup"><span data-stu-id="b86db-299">Creating a connection can take a short while to complete.</span></span>

  <span data-ttu-id="b86db-300">Před spuštěním následujícího příkladu se připojte k předplatnému 1:</span><span class="sxs-lookup"><span data-stu-id="b86db-300">Connect to Subscription 1 before running the following example:</span></span>

  ```powershell
  $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet5gw.Name = "VNet5GW"
  $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
  $Connection15 = "VNet1toVNet5"
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. <span data-ttu-id="b86db-301">**[Předplatné 5]** Vytvořte připojení virtuální sítě TestVNet5 k virtuální síti TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b86db-301">**[Subscription 5]** Create the TestVNet5 to TestVNet1 connection.</span></span> <span data-ttu-id="b86db-302">Tento krok je podobný předchozímu, vytváříte však připojení z virtuální sítě TestVNet5 do virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b86db-302">This step is similar to the one above, except you are creating the connection from TestVNet5 to TestVNet1.</span></span> <span data-ttu-id="b86db-303">Stejný postup vytváření objektu prostředí PowerShell na základě hodnot zjištěných z předplatného 1 se používá i zde.</span><span class="sxs-lookup"><span data-stu-id="b86db-303">The same process of creating a PowerShell object based on the values obtained from Subscription 1 applies here as well.</span></span> <span data-ttu-id="b86db-304">V tomto kroku ověřte, že se sdílené klíče shodují.</span><span class="sxs-lookup"><span data-stu-id="b86db-304">In this step, be sure that the shared keys match.</span></span>

  <span data-ttu-id="b86db-305">Před spuštěním následujícího příkladu se připojte k předplatnému 5:</span><span class="sxs-lookup"><span data-stu-id="b86db-305">Connect to Subscription 5 before running the following example:</span></span>

  ```powershell
  $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet1gw.Name = "VNet1GW"
  $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

## <span data-ttu-id="b86db-306"><a name="verify"></a>Ověření připojení</span><span class="sxs-lookup"><span data-stu-id="b86db-306"><a name="verify"></a>How to verify a connection</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <span data-ttu-id="b86db-307"><a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="b86db-307"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="b86db-308">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b86db-308">Next steps</span></span>

* <span data-ttu-id="b86db-309">Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b86db-309">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="b86db-310">Další informace najdete v [dokumentaci ke službě Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="b86db-310">See the [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) for more information.</span></span>
* <span data-ttu-id="b86db-311">Informace o protokolu BGP najdete v tématech [Přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci protokolu BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b86db-311">For information about BGP, see the [BGP Overview](vpn-gateway-bgp-overview.md) and [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>