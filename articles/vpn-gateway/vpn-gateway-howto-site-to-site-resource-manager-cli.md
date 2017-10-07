---
title: "Připojení vaší místní síti tooan virtuální síť Azure: Site-to-Site VPN: rozhraní příkazového řádku | Microsoft Docs"
description: "Kroky toocreate připojení IPsec z místní sítě tooan virtuální síť Azure přes hello veřejného Internetu. Tyto kroky vám pomůžou vytvořit připojení VPN Gateway typu Site-to-Site mezi různými místy pomocí rozhraní příkazového řádku."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: c652cf2caf3928cdeb19d7dc329f6db101e5ed90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a><span data-ttu-id="19a14-104">Vytvoření virtuální sítě s připojením VPN typu Site-to-Site pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="19a14-104">Create a virtual network with a Site-to-Site VPN connection using CLI</span></span>

<span data-ttu-id="19a14-105">Tento článek ukazuje, jak toouse hello rozhraní příkazového řádku Azure toocreate připojení k bráně VPN Site-to-Site z vaší místní síti toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="19a14-105">This article shows you how toouse hello Azure CLI toocreate a Site-to-Site VPN gateway connection from your on-premises network toohello VNet.</span></span> <span data-ttu-id="19a14-106">Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="19a14-106">hello steps in this article apply toohello Resource Manager deployment model.</span></span> <span data-ttu-id="19a14-107">Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="19a14-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span><br>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="19a14-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="19a14-108">Azure portal</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="19a14-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="19a14-109">PowerShell</span></span>](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [<span data-ttu-id="19a14-110">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="19a14-110">CLI</span></span>](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [<span data-ttu-id="19a14-111">Azure Portal (Classic)</span><span class="sxs-lookup"><span data-stu-id="19a14-111">Azure portal (classic)</span></span>](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>


![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-diagram.png)

<span data-ttu-id="19a14-113">Připojení k bráně VPN Site-to-Site je použité tooconnect místní sítě tooan virtuální síť Azure přes tunelové propojení VPN protokolu IPsec/IKE (IKEv1 nebo IKEv2).</span><span class="sxs-lookup"><span data-stu-id="19a14-113">A Site-to-Site VPN gateway connection is used tooconnect your on-premises network tooan Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel.</span></span> <span data-ttu-id="19a14-114">Tento typ připojení vyžaduje sítě VPN zařízení nachází v místě které má zvenčí veřejnou IP adresu přiřazenou tooit.</span><span class="sxs-lookup"><span data-stu-id="19a14-114">This type of connection requires a VPN device located on-premises that has an externally facing public IP address assigned tooit.</span></span> <span data-ttu-id="19a14-115">Další informace o bránách VPN najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="19a14-115">For more information about VPN gateways, see [About VPN gateway](vpn-gateway-about-vpngateways.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="19a14-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="19a14-116">Before you begin</span></span>

<span data-ttu-id="19a14-117">Ověřte, že jste splnili hello před zahájením konfigurace následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="19a14-117">Verify that you have met hello following criteria before beginning configuration:</span></span>

* <span data-ttu-id="19a14-118">Zajistěte, aby byla kompatibilní zařízení VPN a někoho, kdo je možné tooconfigure ho.</span><span class="sxs-lookup"><span data-stu-id="19a14-118">Make sure you have a compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="19a14-119">Další informace o kompatibilních zařízeních VPN a konfiguraci zařízení najdete v tématu [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="19a14-119">For more information about compatible VPN devices and device configuration, see [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span>
* <span data-ttu-id="19a14-120">Ověřte, že máte veřejnou IPv4 adresu pro vaše zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="19a14-120">Verify that you have an externally facing public IPv4 address for your VPN device.</span></span> <span data-ttu-id="19a14-121">Tato IP adresa nesmí být umístěná za překladem adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="19a14-121">This IP address cannot be located behind a NAT.</span></span>
* <span data-ttu-id="19a14-122">Pokud jste obeznámeni s rozsahy IP adres hello umístěný v konfiguraci místní sítě, musíte toocoordinate s někým, kdo může pro vás jsou tyto podrobnosti obsaženy.</span><span class="sxs-lookup"><span data-stu-id="19a14-122">If you are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span> <span data-ttu-id="19a14-123">Když vytváříte tuto konfiguraci, musíte zadat hello IP adresa rozsahu předpony, Azure bude směrovat tooyour místní umístění.</span><span class="sxs-lookup"><span data-stu-id="19a14-123">When you create this configuration, you must specify hello IP address range prefixes that Azure will route tooyour on-premises location.</span></span> <span data-ttu-id="19a14-124">Žádná z podsítí hello vaší místní sítě můžete prostřednictvím okruhu s hello podsítě virtuální sítě, které chcete tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="19a14-124">None of hello subnets of your on-premises network can over lap with hello virtual network subnets that you want tooconnect to.</span></span>
* <span data-ttu-id="19a14-125">Ověřte, že jste nainstalovali nejnovější verzi rozhraní příkazového řádku hello (2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="19a14-125">Verify that you have installed latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="19a14-126">Informace o instalaci hello rozhraní příkazového řádku najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli) a [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="19a14-126">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli) and [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>

### <span data-ttu-id="19a14-127"><a name="example"></a>Příklady hodnot</span><span class="sxs-lookup"><span data-stu-id="19a14-127"><a name="example"></a>Example values</span></span>

<span data-ttu-id="19a14-128">Můžete použít následující hodnoty toocreate testovacím prostředí hello nebo najdete hodnoty toothese toobetter pochopit hello příklady v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="19a14-128">You can use hello following values toocreate a test environment, or refer toothese values toobetter understand hello examples in this article:</span></span>

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.0.0/24 
GatewaySubnet           = 10.11.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <span data-ttu-id="19a14-129"><a name="Login"></a>1. Připojení tooyour odběru</span><span class="sxs-lookup"><span data-stu-id="19a14-129"><a name="Login"></a>1. Connect tooyour subscription</span></span>

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

## <span data-ttu-id="19a14-130"><a name="rg"></a>2. Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="19a14-130"><a name="rg"></a>2. Create a resource group</span></span>

<span data-ttu-id="19a14-131">Hello následující příklad vytvoří skupinu prostředků s názvem 'TestRG1"hello 'eastus' umístění.</span><span class="sxs-lookup"><span data-stu-id="19a14-131">hello following example creates a resource group named 'TestRG1' in hello 'eastus' location.</span></span> <span data-ttu-id="19a14-132">Pokud už máte skupinu prostředků v hello oblasti, které chcete toocreate virtuální síť, můžete použít než místo.</span><span class="sxs-lookup"><span data-stu-id="19a14-132">If you already have a resource group in hello region that you want toocreate your VNet, you can use that one instead.</span></span>

```azurecli
az group create --name TestRG1 --location eastus
```

## <span data-ttu-id="19a14-133"><a name="VNet"></a>3. Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="19a14-133"><a name="VNet"></a>3. Create a virtual network</span></span>

<span data-ttu-id="19a14-134">Pokud ještě nemáte virtuální síť, vytvořte jednu pomocí hello [vytvoření sítě vnet az](/cli/azure/network/vnet#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="19a14-134">If you don't already have a virtual network, create one using hello [az network vnet create](/cli/azure/network/vnet#create) command.</span></span> <span data-ttu-id="19a14-135">Při vytváření virtuální sítě, ujistěte se, že hello adresní prostory, které zadáte nepřekrývají s hello adresní prostory, které máte ve vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="19a14-135">When creating a virtual network, make sure that hello address spaces you specify don't overlap any of hello address spaces that you have on your on-premises network.</span></span>

<span data-ttu-id="19a14-136">Hello následující příklad vytvoří virtuální síť s názvem "TestVNet1" a podsítě, 'Subnet1'.</span><span class="sxs-lookup"><span data-stu-id="19a14-136">hello following example creates a virtual network named 'TestVNet1' and a subnet, 'Subnet1'.</span></span>

```azurecli
az network vnet create --name TestVNet1 --resource-group TestRG1 --address-prefix 10.11.0.0/16 --location eastus --subnet-name Subnet1 --subnet-prefix 10.11.0.0/24
```

## <span data-ttu-id="19a14-137">4. <a name="gwsub"></a>Vytvořit podsíť brány hello</span><span class="sxs-lookup"><span data-stu-id="19a14-137">4. <a name="gwsub"></a>Create hello gateway subnet</span></span>

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

<span data-ttu-id="19a14-138">Pro tuto konfiguraci potřebujete také podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="19a14-138">For this configuration, you also need a gateway subnet.</span></span> <span data-ttu-id="19a14-139">Brána virtuální sítě Hello používá podsíť brány, který obsahuje hello IP adresy, které jsou používány hello služby brány VPN.</span><span class="sxs-lookup"><span data-stu-id="19a14-139">hello virtual network gateway uses a gateway subnet that contains hello IP addresses that are used by hello VPN gateway services.</span></span> <span data-ttu-id="19a14-140">Při vytváření podsítě brány je nutné ji pojmenovat GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="19a14-140">When you create a gateway subnet, it must be named 'GatewaySubnet'.</span></span> <span data-ttu-id="19a14-141">Pokud zadáte jiný název, vytvoříte sice podsíť, ale Azure ji nebude považovat za podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="19a14-141">If you name it something else, you create a subnet, but Azure won't treat it as a gateway subnet.</span></span>

<span data-ttu-id="19a14-142">velikost Hello hello podsíť brány, který zadáte závisí na konfiguraci brány VPN hello, které chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="19a14-142">hello size of hello gateway subnet that you specify depends on hello VPN gateway configuration that you want toocreate.</span></span> <span data-ttu-id="19a14-143">I když je možné toocreate podsíť brány jako malé/29, doporučujeme vytvořit větší podsíť, která zahrnuje víc adres výběrem/27 nebo velikosti/28.</span><span class="sxs-lookup"><span data-stu-id="19a14-143">While it is possible toocreate a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting /27 or /28.</span></span> <span data-ttu-id="19a14-144">Pomocí větší podsíť brány umožňuje dost IP adres tooaccommodate možné budoucí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="19a14-144">Using a larger gateway subnet allows for enough IP addresses tooaccommodate possible future configurations.</span></span>

<span data-ttu-id="19a14-145">Použití hello [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create) podsíť brány hello toocreate příkaz.</span><span class="sxs-lookup"><span data-stu-id="19a14-145">Use hello [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) command toocreate hello gateway subnet.</span></span>

```azurecli
az network vnet subnet create --address-prefix 10.11.255.0/27 --name GatewaySubnet --resource-group TestRG1 --vnet-name TestVNet1
```

## <span data-ttu-id="19a14-146"><a name="localnet"></a>5. Vytvoření brány místní sítě hello</span><span class="sxs-lookup"><span data-stu-id="19a14-146"><a name="localnet"></a>5. Create hello local network gateway</span></span>

<span data-ttu-id="19a14-147">Hello brány místní sítě obvykle odkazuje tooyour místní umístění.</span><span class="sxs-lookup"><span data-stu-id="19a14-147">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="19a14-148">Zadejte název, pomocí kterého můžete Azure odkazovat tooit a pak zadejte IP adresu hello hello lokality z hello místní VPN zařízení toowhich se vytvoří připojení.</span><span class="sxs-lookup"><span data-stu-id="19a14-148">You give hello site a name by which Azure can refer tooit, then specify hello IP address of hello on-premises VPN device toowhich you will create a connection.</span></span> <span data-ttu-id="19a14-149">Můžete také určit hello předpony IP adres, které budou směrovány prostřednictvím zařízení VPN toohello brány sítě VPN hello.</span><span class="sxs-lookup"><span data-stu-id="19a14-149">You also specify hello IP address prefixes that will be routed through hello VPN gateway toohello VPN device.</span></span> <span data-ttu-id="19a14-150">Hello předpony, které zadáte předpony hello umístěn ve vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="19a14-150">hello address prefixes you specify are hello prefixes located on your on-premises network.</span></span> <span data-ttu-id="19a14-151">Pokud se změní na místní síti, můžete snadno aktualizovat hello předpony.</span><span class="sxs-lookup"><span data-stu-id="19a14-151">If your on-premises network changes, you can easily update hello prefixes.</span></span>

<span data-ttu-id="19a14-152">Použijte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="19a14-152">Use hello following values:</span></span>

* <span data-ttu-id="19a14-153">Hello *– adresa ip brány* je hello IP adresa vašeho místního zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="19a14-153">hello *--gateway-ip-address* is hello IP address of your on-premises VPN device.</span></span> <span data-ttu-id="19a14-154">Zařízení VPN nesmí být umístěné za překladem adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="19a14-154">Your VPN device cannot be located behind a NAT.</span></span>
* <span data-ttu-id="19a14-155">Hello *– předpony adres místní* jsou místní adresní prostory.</span><span class="sxs-lookup"><span data-stu-id="19a14-155">hello *--local-address-prefixes* are your on-premises address spaces.</span></span>

<span data-ttu-id="19a14-156">Použití hello [az brány místní-vytvořit](/cli/azure/network/local-gateway#create) příkaz tooadd bránu místní sítě s více předponami adresy:</span><span class="sxs-lookup"><span data-stu-id="19a14-156">Use hello [az network local-gateway create](/cli/azure/network/local-gateway#create) command tooadd a local network gateway with multiple address prefixes:</span></span>

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --resource-group TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <span data-ttu-id="19a14-157"><a name="PublicIP"></a>6. Vyžádání veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="19a14-157"><a name="PublicIP"></a>6. Request a Public IP address</span></span>

<span data-ttu-id="19a14-158">Brána VPN musí mít veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="19a14-158">A VPN gateway must have a Public IP address.</span></span> <span data-ttu-id="19a14-159">Nejprve požádali o prostředek hello IP adresy a pak odkazovat tooit při vytváření brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="19a14-159">You first request hello IP address resource, and then refer tooit when creating your virtual network gateway.</span></span> <span data-ttu-id="19a14-160">Hello je přiřazená IP adresa dynamicky toohello prostředků při vytváření brány VPN hello.</span><span class="sxs-lookup"><span data-stu-id="19a14-160">hello IP address is dynamically assigned toohello resource when hello VPN gateway is created.</span></span> <span data-ttu-id="19a14-161">Služba VPN Gateway aktuálně podporuje pouze *dynamické* přidělení veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="19a14-161">VPN Gateway currently only supports *Dynamic* Public IP address allocation.</span></span> <span data-ttu-id="19a14-162">Nemůžete si vyžádat statické přiřazení IP adresy.</span><span class="sxs-lookup"><span data-stu-id="19a14-162">You cannot request a Static Public IP address assignment.</span></span> <span data-ttu-id="19a14-163">Však neznamená to, že hello IP adresa změní po byl přiřazen tooyour brány VPN.</span><span class="sxs-lookup"><span data-stu-id="19a14-163">However, this does not mean that hello IP address changes after it has been assigned tooyour VPN gateway.</span></span> <span data-ttu-id="19a14-164">Hello jenom jednou hello změny veřejné IP adresy je při hello brány je odstraní a znovu vytvoří.</span><span class="sxs-lookup"><span data-stu-id="19a14-164">hello only time hello Public IP address changes is when hello gateway is deleted and re-created.</span></span> <span data-ttu-id="19a14-165">V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN se nezmění.</span><span class="sxs-lookup"><span data-stu-id="19a14-165">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of your VPN gateway.</span></span>

<span data-ttu-id="19a14-166">Použití hello [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create) toorequest příkaz dynamické veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="19a14-166">Use hello [az network public-ip create](/cli/azure/network/public-ip#create) command toorequest a Dynamic Public IP address.</span></span>

```azurecli
az network public-ip create --name VNet1GWIP --resource-group TestRG1 --allocation-method Dynamic
```

## <span data-ttu-id="19a14-167"><a name="CreateGateway"></a>7. Vytvoření brány VPN hello</span><span class="sxs-lookup"><span data-stu-id="19a14-167"><a name="CreateGateway"></a>7. Create hello VPN gateway</span></span>

<span data-ttu-id="19a14-168">Vytvořte bránu VPN virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="19a14-168">Create hello virtual network VPN gateway.</span></span> <span data-ttu-id="19a14-169">Vytvoření brány VPN může trvat až minut too45 nebo další toocomplete.</span><span class="sxs-lookup"><span data-stu-id="19a14-169">Creating a VPN gateway can take up too45 minutes or more toocomplete.</span></span>

<span data-ttu-id="19a14-170">Použijte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="19a14-170">Use hello following values:</span></span>

* <span data-ttu-id="19a14-171">Hello *– typ brány* pro Site-to-Site je konfigurace *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="19a14-171">hello *--gateway-type* for a Site-to-Site configuration is *Vpn*.</span></span> <span data-ttu-id="19a14-172">Typ brány Hello je vždy konkrétní toohello konfigurace, kterou implementujete.</span><span class="sxs-lookup"><span data-stu-id="19a14-172">hello gateway type is always specific toohello configuration that you are implementing.</span></span> <span data-ttu-id="19a14-173">Další informace najdete v části [Typy bran](vpn-gateway-about-vpn-gateway-settings.md#gwtype).</span><span class="sxs-lookup"><span data-stu-id="19a14-173">For more information, see [Gateway types](vpn-gateway-about-vpn-gateway-settings.md#gwtype).</span></span>
* <span data-ttu-id="19a14-174">Hello *typ sítě vpn –* může být *RouteBased* (označované tooas dynamickou bránu v některé dokumentaci), nebo *PolicyBased* (označované tooas statická brána v některých dokumentace).</span><span class="sxs-lookup"><span data-stu-id="19a14-174">hello *--vpn-type* can be *RouteBased* (referred tooas a Dynamic Gateway in some documentation), or *PolicyBased* (referred tooas a Static Gateway in some documentation).</span></span> <span data-ttu-id="19a14-175">nastavení Hello je konkrétní toorequirements hello zařízení, které se připojujete.</span><span class="sxs-lookup"><span data-stu-id="19a14-175">hello setting is specific toorequirements of hello device that you are connecting to.</span></span> <span data-ttu-id="19a14-176">Další informace o typech bran VPN najdete v tématu [Informace o nastavení konfigurace služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#vpntype).</span><span class="sxs-lookup"><span data-stu-id="19a14-176">For more information about VPN gateway types, see [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md#vpntype).</span></span>
* <span data-ttu-id="19a14-177">Vyberte hello skladová položka brány, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="19a14-177">Select hello Gateway SKU that you want toouse.</span></span> <span data-ttu-id="19a14-178">Pro určité skladové jednotky (SKU) platí omezení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="19a14-178">There are configuration limitations for certain SKUs.</span></span> <span data-ttu-id="19a14-179">Další informace najdete v části [Skladové jednotky (SKU) brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="19a14-179">For more information, see [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span>

<span data-ttu-id="19a14-180">Vytvoření brány VPN hello pomocí hello [az brány virtuální sítě-vytvořit](/cli/azure/network/vnet-gateway#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="19a14-180">Create hello VPN gateway using hello [az network vnet-gateway create](/cli/azure/network/vnet-gateway#create) command.</span></span> <span data-ttu-id="19a14-181">Pokud spustíte tento příkaz pomocí hello parametr '– žádné - wait', se nezobrazí žádné zpětnou vazbu nebo výstup.</span><span class="sxs-lookup"><span data-stu-id="19a14-181">If you run this command using hello '--no-wait' parameter, you don't see any feedback or output.</span></span> <span data-ttu-id="19a14-182">Tento parametr umožňuje toocreate hello brány v pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="19a14-182">This parameter allows hello gateway toocreate in hello background.</span></span> <span data-ttu-id="19a14-183">Trvá přibližně 45 minut toocreate bránu.</span><span class="sxs-lookup"><span data-stu-id="19a14-183">It takes around 45 minutes toocreate a gateway.</span></span>

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWIP --resource-group TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait 
```

## <span data-ttu-id="19a14-184"><a name="VPNDevice"></a>8. Konfigurace zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="19a14-184"><a name="VPNDevice"></a>8. Configure your VPN device</span></span>

<span data-ttu-id="19a14-185">Připojení Site-to-Site tooan do místní sítě vyžaduje zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="19a14-185">Site-to-Site connections tooan on-premises network require a VPN device.</span></span> <span data-ttu-id="19a14-186">V tomto kroku nakonfigurujete zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="19a14-186">In this step, you configure your VPN device.</span></span> <span data-ttu-id="19a14-187">Při konfiguraci zařízení VPN, je třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="19a14-187">When configuring your VPN device, you need hello following:</span></span>

- <span data-ttu-id="19a14-188">Sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="19a14-188">A shared key.</span></span> <span data-ttu-id="19a14-189">To je hello stejný sdílený klíč, který zadáte při vytváření připojení Site-to-Site VPN.</span><span class="sxs-lookup"><span data-stu-id="19a14-189">This is hello same shared key that you specify when creating your Site-to-Site VPN connection.</span></span> <span data-ttu-id="19a14-190">V našich ukázkách používáme základní sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="19a14-190">In our examples, we use a basic shared key.</span></span> <span data-ttu-id="19a14-191">Doporučujeme, abyste vygenerování složitější klíče toouse.</span><span class="sxs-lookup"><span data-stu-id="19a14-191">We recommend that you generate a more complex key toouse.</span></span>
- <span data-ttu-id="19a14-192">Hello veřejnou IP adresu brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="19a14-192">hello Public IP address of your virtual network gateway.</span></span> <span data-ttu-id="19a14-193">Hello veřejnou IP adresu můžete zobrazit pomocí hello portálu Azure, PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="19a14-193">You can view hello public IP address by using hello Azure portal, PowerShell, or CLI.</span></span> <span data-ttu-id="19a14-194">toofind hello veřejnou IP adresu brány virtuální sítě, použijte hello [seznam veřejné ip sítě az](/cli/azure/network/public-ip#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="19a14-194">toofind hello public IP address of your virtual network gateway, use hello [az network public-ip list](/cli/azure/network/public-ip#list) command.</span></span> <span data-ttu-id="19a14-195">Pro snadné čtení výstup hello je formátovaný toodisplay hello seznam veřejné IP adresy ve formátu tabulky.</span><span class="sxs-lookup"><span data-stu-id="19a14-195">For easy reading, hello output is formatted toodisplay hello list of public IPs in table format.</span></span>

  ```azurecli
  az network public-ip list --resource-group TestRG1 --output table
  ```


[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <span data-ttu-id="19a14-196"><a name="CreateConnection"></a>9. Vytvoření připojení VPN hello</span><span class="sxs-lookup"><span data-stu-id="19a14-196"><a name="CreateConnection"></a>9. Create hello VPN connection</span></span>

<span data-ttu-id="19a14-197">Vytvořte připojení VPN hello Site-to-Site mezi bránou virtuální sítě a vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="19a14-197">Create hello Site-to-Site VPN connection between your virtual network gateway and your on-premises VPN device.</span></span> <span data-ttu-id="19a14-198">Platím zvláštní pozornost toohello sdílené hodnotu klíče, který musí odpovídat hello nakonfigurované sdílené hodnota klíče pro vaše zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="19a14-198">Pay particular attention toohello shared key value, which must match hello configured shared key value for your VPN device.</span></span>

<span data-ttu-id="19a14-199">Vytvoření připojení hello pomocí hello [az sítě – připojení vpn vytvářet](/cli/azure/network/vpn-connection#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="19a14-199">Create hello connection using hello [az network vpn-connection create](/cli/azure/network/vpn-connection#create) command.</span></span>

```azurecli
az network vpn-connection create --name VNet1toSite2 -resource-group TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

<span data-ttu-id="19a14-200">Za malou chvíli hello bude vytvořeno připojení.</span><span class="sxs-lookup"><span data-stu-id="19a14-200">After a short while, hello connection will be established.</span></span>

## <span data-ttu-id="19a14-201"><a name="toverify"></a>10. Ověření připojení VPN hello</span><span class="sxs-lookup"><span data-stu-id="19a14-201"><a name="toverify"></a>10. Verify hello VPN connection</span></span>

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

<span data-ttu-id="19a14-202">Pokud chcete toouse jiná metoda tooverify připojení, najdete v části [ověření připojení VPN Gateway](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="19a14-202">If you want toouse another method tooverify your connection, see [Verify a VPN Gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>

## <span data-ttu-id="19a14-203"><a name="connectVM"></a>tooconnect tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="19a14-203"><a name="connectVM"></a>tooconnect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <span data-ttu-id="19a14-204"><a name="tasks"></a>Běžné úlohy</span><span class="sxs-lookup"><span data-stu-id="19a14-204"><a name="tasks"></a>Common tasks</span></span>

<span data-ttu-id="19a14-205">Tato část obsahuje běžné příkazy, které jsou užitečné při práci s konfiguracemi typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="19a14-205">This section contains common commands that are helpful when working with site-to-site configurations.</span></span> <span data-ttu-id="19a14-206">Hello úplný seznam síťových rozhraní příkazového řádku najdete v tématu [rozhraní příkazového řádku Azure - sítě](/cli/azure/network).</span><span class="sxs-lookup"><span data-stu-id="19a14-206">For hello full list of CLI networking commands, see [Azure CLI - Networking](/cli/azure/network).</span></span>

[!INCLUDE [local network gateway common tasks](../../includes/vpn-gateway-common-tasks-cli-include.md)]

## <a name="next-steps"></a><span data-ttu-id="19a14-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19a14-207">Next steps</span></span>

* <span data-ttu-id="19a14-208">Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="19a14-208">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="19a14-209">Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="19a14-209">For more information, see [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="19a14-210">Informace o protokolu BGP najdete v tématu hello [přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [jak tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="19a14-210">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
* <span data-ttu-id="19a14-211">Informace o vynuceném tunelování najdete v tématu [Informace o vynuceném tunelování](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="19a14-211">For information about Forced Tunneling, see [About Forced Tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>
* <span data-ttu-id="19a14-212">Informace o vysoce dostupných připojeních typu aktivní-aktivní najdete v tématu [Připojení s vysokou dostupností mezi jednotlivými místy a VNet-to-VNet](vpn-gateway-highlyavailable.md).</span><span class="sxs-lookup"><span data-stu-id="19a14-212">For information about Highly Available Active-Active connections, see [Highly Available cross-premises and VNet-to-VNet connectivity](vpn-gateway-highlyavailable.md).</span></span>
* <span data-ttu-id="19a14-213">Seznam příkazů Azure CLI pro práci se sítěmi najdete v tématu věnovaném [Azure CLI](https://docs.microsoft.com/cli/azure/network).</span><span class="sxs-lookup"><span data-stu-id="19a14-213">For a list of networking Azure CLI commands, see [Azure CLI](https://docs.microsoft.com/cli/azure/network).</span></span>