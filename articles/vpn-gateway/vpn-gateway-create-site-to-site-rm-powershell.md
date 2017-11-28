---
title: "Připojení vaší místní síti tooan virtuální síť Azure: Site-to-Site VPN: prostředí PowerShell | Microsoft Docs"
description: "Kroky toocreate připojení IPsec z místní sítě tooan virtuální síť Azure přes hello veřejného Internetu. Tyto kroky vám pomůžou vytvořit připojení VPN Gateway typu Site-to-Site mezi různými místy pomocí PowerShellu."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: cb8db1dab3a5488816a7f7e8e63908a4c02f55db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a><span data-ttu-id="84df4-104">Vytvoření virtuální sítě pomocí připojení VPN Site-to-Site s použitím prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="84df4-104">Create a VNet with a Site-to-Site VPN connection using PowerShell</span></span>

<span data-ttu-id="84df4-105">Tento článek ukazuje, jak toouse připojení brány sítě VPN prostředí PowerShell toocreate Site-to-Site z místní sítě toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="84df4-105">This article shows you how toouse PowerShell toocreate a Site-to-Site VPN gateway connection from your on-premises network toohello VNet.</span></span> <span data-ttu-id="84df4-106">Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="84df4-106">hello steps in this article apply toohello Resource Manager deployment model.</span></span> <span data-ttu-id="84df4-107">Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="84df4-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="84df4-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="84df4-108">Azure portal</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="84df4-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="84df4-109">PowerShell</span></span>](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [<span data-ttu-id="84df4-110">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="84df4-110">CLI</span></span>](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [<span data-ttu-id="84df4-111">Azure Portal (Classic)</span><span class="sxs-lookup"><span data-stu-id="84df4-111">Azure portal (classic)</span></span>](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [<span data-ttu-id="84df4-112">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="84df4-112">Classic portal (classic)</span></span>](vpn-gateway-site-to-site-create.md)
> 
>


<span data-ttu-id="84df4-113">Připojení k bráně VPN Site-to-Site je použité tooconnect místní sítě tooan virtuální síť Azure přes tunelové propojení VPN protokolu IPsec/IKE (IKEv1 nebo IKEv2).</span><span class="sxs-lookup"><span data-stu-id="84df4-113">A Site-to-Site VPN gateway connection is used tooconnect your on-premises network tooan Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel.</span></span> <span data-ttu-id="84df4-114">Tento typ připojení vyžaduje sítě VPN zařízení nachází v místě které má zvenčí veřejnou IP adresu přiřazenou tooit.</span><span class="sxs-lookup"><span data-stu-id="84df4-114">This type of connection requires a VPN device located on-premises that has an externally facing public IP address assigned tooit.</span></span> <span data-ttu-id="84df4-115">Další informace o bránách VPN najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="84df4-115">For more information about VPN gateways, see [About VPN gateway](vpn-gateway-about-vpngateways.md).</span></span>

![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-diagram.png)

## <span data-ttu-id="84df4-117"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="84df4-117"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="84df4-118">Ověřte, že jste splnili hello před zahájením konfigurace následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="84df4-118">Verify that you have met hello following criteria before beginning your configuration:</span></span>

* <span data-ttu-id="84df4-119">Zajistěte, aby byla kompatibilní zařízení VPN a někoho, kdo je možné tooconfigure ho.</span><span class="sxs-lookup"><span data-stu-id="84df4-119">Make sure you have a compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="84df4-120">Další informace o kompatibilních zařízeních VPN a konfiguraci zařízení najdete v tématu [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="84df4-120">For more information about compatible VPN devices and device configuration, see [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span>
* <span data-ttu-id="84df4-121">Ověřte, že máte veřejnou IPv4 adresu pro vaše zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="84df4-121">Verify that you have an externally facing public IPv4 address for your VPN device.</span></span> <span data-ttu-id="84df4-122">Tato IP adresa nesmí být umístěná za překladem adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="84df4-122">This IP address cannot be located behind a NAT.</span></span>
* <span data-ttu-id="84df4-123">Pokud jste obeznámeni s rozsahy IP adres hello umístěný v konfiguraci místní sítě, musíte toocoordinate s někým, kdo může pro vás jsou tyto podrobnosti obsaženy.</span><span class="sxs-lookup"><span data-stu-id="84df4-123">If you are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span> <span data-ttu-id="84df4-124">Když vytváříte tuto konfiguraci, musíte zadat hello IP adresa rozsahu předpony, Azure bude směrovat tooyour místní umístění.</span><span class="sxs-lookup"><span data-stu-id="84df4-124">When you create this configuration, you must specify hello IP address range prefixes that Azure will route tooyour on-premises location.</span></span> <span data-ttu-id="84df4-125">Žádná z podsítí hello vaší místní sítě můžete prostřednictvím okruhu s hello podsítě virtuální sítě, které chcete tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="84df4-125">None of hello subnets of your on-premises network can over lap with hello virtual network subnets that you want tooconnect to.</span></span>
* <span data-ttu-id="84df4-126">Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-126">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="84df4-127">Rutiny prostředí PowerShell jsou často aktualizuje a je obvykle nutné tooupdate vaše prostředí PowerShell rutiny tooget hello nejnovější funkce funkce.</span><span class="sxs-lookup"><span data-stu-id="84df4-127">PowerShell cmdlets are updated frequently and you will typically need tooupdate your PowerShell cmdlets tooget hello latest feature functionality.</span></span> <span data-ttu-id="84df4-128">Pokud nechcete aktualizovat vaše rutiny prostředí PowerShell, hello hodnoty zadané se pravděpodobně nezdaří.</span><span class="sxs-lookup"><span data-stu-id="84df4-128">If you don't update your PowerShell cmdlets, hello values specified may fail.</span></span> <span data-ttu-id="84df4-129">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace o stažení a instalaci rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84df4-129">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about downloading and installing PowerShell cmdlets.</span></span>

### <span data-ttu-id="84df4-130"><a name="example"></a>Příklady hodnot</span><span class="sxs-lookup"><span data-stu-id="84df4-130"><a name="example"></a>Example values</span></span>

<span data-ttu-id="84df4-131">Hello příklady v tomto článku používají hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="84df4-131">hello examples in this article use hello following values.</span></span> <span data-ttu-id="84df4-132">Můžete použít tyto hodnoty toocreate testovací prostředí nebo odkazovat toothem toobetter pochopit hello příklady v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="84df4-132">You can use these values toocreate a test environment, or refer toothem toobetter understand hello examples in this article.</span></span>

```
#Example values

VnetName                = TestVNet1
ResourceGroup           = TestRG1
Location                = East US 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.1.0/28 
GatewaySubnet           = 10.11.0.0/27
LocalNetworkGatewayName = Site2
LNG Public IP           = <VPN device IP address> 
Local Address Prefixes  = 10.0.0.0/24, 20.0.0.0/24
Gateway Name            = VNet1GW
PublicIP                = VNet1GWIP
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2

```


## <span data-ttu-id="84df4-133"><a name="Login"></a>1. Připojení tooyour odběru</span><span class="sxs-lookup"><span data-stu-id="84df4-133"><a name="Login"></a>1. Connect tooyour subscription</span></span>

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <span data-ttu-id="84df4-134"><a name="VNet"></a>2. Vytvoření virtuální sítě a podsítě brány</span><span class="sxs-lookup"><span data-stu-id="84df4-134"><a name="VNet"></a>2. Create a virtual network and a gateway subnet</span></span>

<span data-ttu-id="84df4-135">Pokud ještě nemáte virtuální síť, vytvořte si ji.</span><span class="sxs-lookup"><span data-stu-id="84df4-135">If you don't already have a virtual network, create one.</span></span> <span data-ttu-id="84df4-136">Při vytváření virtuální sítě, ujistěte se, že hello adresní prostory, které zadáte nepřekrývají s hello adresní prostory, které máte ve vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="84df4-136">When creating a virtual network, make sure that hello address spaces you specify don't overlap any of hello address spaces that you have on your on-premises network.</span></span>

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [No NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

### <span data-ttu-id="84df4-137"><a name="vnet"></a>toocreate virtuální síť a podsíť brány</span><span class="sxs-lookup"><span data-stu-id="84df4-137"><a name="vnet"></a>toocreate a virtual network and a gateway subnet</span></span>

<span data-ttu-id="84df4-138">Tento příklad vytvoří virtuální síť a podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="84df4-138">This example creates a virtual network and a gateway subnet.</span></span> <span data-ttu-id="84df4-139">Pokud již máte virtuální síť, která potřebujete tooadd podsíť brány k, najdete v části [tooadd brány podsítě tooa virtuální síť jste již vytvořili](#gatewaysubnet).</span><span class="sxs-lookup"><span data-stu-id="84df4-139">If you already have a virtual network that you need tooadd a gateway subnet to, see [tooadd a gateway subnet tooa virtual network you have already created](#gatewaysubnet).</span></span>

<span data-ttu-id="84df4-140">Vytvořte skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="84df4-140">Create a resource group:</span></span>

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location 'East US'
```

<span data-ttu-id="84df4-141">Vytvořte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="84df4-141">Create your virtual network.</span></span>

1. <span data-ttu-id="84df4-142">Nastavení proměnných hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-142">Set hello variables.</span></span>

  ```powershell
  $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27
  $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix 10.11.1.0/28
  ```
2. <span data-ttu-id="84df4-143">Vytvořte hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="84df4-143">Create hello VNet.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1 `
  -Location 'East US' -AddressPrefix 10.11.0.0/16 -Subnet $subnet1, $subnet2
  ```

### <span data-ttu-id="84df4-144"><a name="gatewaysubnet"></a>tooadd brány podsítě tooa virtuální sítě, které jste už vytvořili</span><span class="sxs-lookup"><span data-stu-id="84df4-144"><a name="gatewaysubnet"></a>tooadd a gateway subnet tooa virtual network you have already created</span></span>

1. <span data-ttu-id="84df4-145">Nastavení proměnných hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-145">Set hello variables.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG1 -Name TestVet1
  ```
2. <span data-ttu-id="84df4-146">Vytvořte podsíť brány hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-146">Create hello gateway subnet.</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27 -VirtualNetwork $vnet
  ```
3. <span data-ttu-id="84df4-147">Nastavte konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-147">Set hello configuration.</span></span>

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

## <span data-ttu-id="84df4-148">3. <a name="localnet"></a>Vytvoření brány místní sítě hello</span><span class="sxs-lookup"><span data-stu-id="84df4-148">3. <a name="localnet"></a>Create hello local network gateway</span></span>

<span data-ttu-id="84df4-149">Hello brány místní sítě obvykle odkazuje tooyour místní umístění.</span><span class="sxs-lookup"><span data-stu-id="84df4-149">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="84df4-150">Zadejte název, pomocí kterého můžete Azure odkazovat tooit a pak zadejte IP adresu hello hello lokality z hello místní VPN zařízení toowhich se vytvoří připojení.</span><span class="sxs-lookup"><span data-stu-id="84df4-150">You give hello site a name by which Azure can refer tooit, then specify hello IP address of hello on-premises VPN device toowhich you will create a connection.</span></span> <span data-ttu-id="84df4-151">Můžete také určit hello předpony IP adres, které budou směrovány prostřednictvím zařízení VPN toohello brány sítě VPN hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-151">You also specify hello IP address prefixes that will be routed through hello VPN gateway toohello VPN device.</span></span> <span data-ttu-id="84df4-152">Hello předpony, které zadáte předpony hello umístěn ve vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="84df4-152">hello address prefixes you specify are hello prefixes located on your on-premises network.</span></span> <span data-ttu-id="84df4-153">Pokud se změní na místní síti, můžete snadno aktualizovat hello předpony.</span><span class="sxs-lookup"><span data-stu-id="84df4-153">If your on-premises network changes, you can easily update hello prefixes.</span></span>

<span data-ttu-id="84df4-154">Použijte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="84df4-154">Use hello following values:</span></span>

* <span data-ttu-id="84df4-155">Hello *GatewayIPAddress* je hello IP adresa vašeho místního zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="84df4-155">hello *GatewayIPAddress* is hello IP address of your on-premises VPN device.</span></span> <span data-ttu-id="84df4-156">Zařízení VPN nesmí být umístěné za překladem adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="84df4-156">Your VPN device cannot be located behind a NAT.</span></span>
* <span data-ttu-id="84df4-157">Hello *AddressPrefix* je místní adresní prostor.</span><span class="sxs-lookup"><span data-stu-id="84df4-157">hello *AddressPrefix* is your on-premises address space.</span></span>

<span data-ttu-id="84df4-158">tooadd bránu místní sítě s jednou předponou adresy:</span><span class="sxs-lookup"><span data-stu-id="84df4-158">tooadd a local network gateway with a single address prefix:</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.0.0.0/24'
  ```

<span data-ttu-id="84df4-159">tooadd bránu místní sítě s více předponami adresy:</span><span class="sxs-lookup"><span data-stu-id="84df4-159">tooadd a local network gateway with multiple address prefixes:</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
  ```

<span data-ttu-id="84df4-160">předpony toomodify IP adresy pro bránu místní sítě:</span><span class="sxs-lookup"><span data-stu-id="84df4-160">toomodify IP address prefixes for your local network gateway:</span></span><br>
<span data-ttu-id="84df4-161">Někdy dojde ke změně předpon brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="84df4-161">Sometimes your local network gateway prefixes change.</span></span> <span data-ttu-id="84df4-162">kroky Hello si toomodify vaší IP adresu, kterou předpony závisí na tom, jestli jste vytvořili připojení brány sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="84df4-162">hello steps you take toomodify your IP address prefixes depend on whether you have created a VPN gateway connection.</span></span> <span data-ttu-id="84df4-163">V tématu hello [úprava předpon IP adresy pro bránu místní sítě](#modify) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="84df4-163">See hello [Modify IP address prefixes for a local network gateway](#modify) section of this article.</span></span>

## <span data-ttu-id="84df4-164"><a name="PublicIP"></a>4. Vyžádání veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="84df4-164"><a name="PublicIP"></a>4. Request a Public IP address</span></span>

<span data-ttu-id="84df4-165">Brána VPN musí mít veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="84df4-165">A VPN gateway must have a Public IP address.</span></span> <span data-ttu-id="84df4-166">Nejprve požádali o prostředek hello IP adresy a pak odkazovat tooit při vytváření brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="84df4-166">You first request hello IP address resource, and then refer tooit when creating your virtual network gateway.</span></span> <span data-ttu-id="84df4-167">Hello je přiřazená IP adresa dynamicky toohello prostředků při vytváření brány VPN hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-167">hello IP address is dynamically assigned toohello resource when hello VPN gateway is created.</span></span> <span data-ttu-id="84df4-168">Služba VPN Gateway aktuálně podporuje pouze *dynamické* přidělení veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="84df4-168">VPN Gateway currently only supports *Dynamic* Public IP address allocation.</span></span> <span data-ttu-id="84df4-169">Nemůžete si vyžádat statické přiřazení IP adresy.</span><span class="sxs-lookup"><span data-stu-id="84df4-169">You cannot request a Static Public IP address assignment.</span></span> <span data-ttu-id="84df4-170">Však neznamená to, že hello IP adresa změní po byl přiřazen tooyour brány VPN.</span><span class="sxs-lookup"><span data-stu-id="84df4-170">However, this does not mean that hello IP address changes after it has been assigned tooyour VPN gateway.</span></span> <span data-ttu-id="84df4-171">Hello jenom jednou hello změny veřejné IP adresy je při hello brány je odstraní a znovu vytvoří.</span><span class="sxs-lookup"><span data-stu-id="84df4-171">hello only time hello Public IP address changes is when hello gateway is deleted and re-created.</span></span> <span data-ttu-id="84df4-172">V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN se nezmění.</span><span class="sxs-lookup"><span data-stu-id="84df4-172">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of your VPN gateway.</span></span>

<span data-ttu-id="84df4-173">Požádat o veřejnou IP adresu, která bude přiřazena tooyour virtuální sítě VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="84df4-173">Request a Public IP address that will be assigned tooyour virtual network VPN gateway.</span></span>

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <span data-ttu-id="84df4-174"><a name="GatewayIPConfig"></a>5. Vytvoření konfigurace adresování IP brány hello</span><span class="sxs-lookup"><span data-stu-id="84df4-174"><a name="GatewayIPConfig"></a>5. Create hello gateway IP addressing configuration</span></span>

<span data-ttu-id="84df4-175">Hello konfigurace brány definuje podsíť hello a toouse hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="84df4-175">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="84df4-176">Použijte následující příklad toocreate hello vlastní konfiguraci brány:</span><span class="sxs-lookup"><span data-stu-id="84df4-176">Use hello following example toocreate your gateway configuration:</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <span data-ttu-id="84df4-177"><a name="CreateGateway"></a>6. Vytvoření brány VPN hello</span><span class="sxs-lookup"><span data-stu-id="84df4-177"><a name="CreateGateway"></a>6. Create hello VPN gateway</span></span>

<span data-ttu-id="84df4-178">Vytvořte bránu VPN virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-178">Create hello virtual network VPN gateway.</span></span> <span data-ttu-id="84df4-179">Vytvoření brány VPN může trvat až minut too45 nebo další toocomplete.</span><span class="sxs-lookup"><span data-stu-id="84df4-179">Creating a VPN gateway can take up too45 minutes or more toocomplete.</span></span>

<span data-ttu-id="84df4-180">Použijte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="84df4-180">Use hello following values:</span></span>

* <span data-ttu-id="84df4-181">Hello *- GatewayType* pro Site-to-Site je konfigurace *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="84df4-181">hello *-GatewayType* for a Site-to-Site configuration is *Vpn*.</span></span> <span data-ttu-id="84df4-182">Typ brány Hello je vždy konkrétní toohello konfigurace, kterou implementujete.</span><span class="sxs-lookup"><span data-stu-id="84df4-182">hello gateway type is always specific toohello configuration that you are implementing.</span></span> <span data-ttu-id="84df4-183">Například jiné konfigurace brány mohou vyžadovat jako -GatewayType hodnotu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="84df4-183">For example, other gateway configurations may require -GatewayType ExpressRoute.</span></span>
* <span data-ttu-id="84df4-184">Hello *- VpnType* může být *RouteBased* (označované tooas dynamickou bránu v některé dokumentaci), nebo *PolicyBased* (označované tooas statická brána v některé dokumentaci ).</span><span class="sxs-lookup"><span data-stu-id="84df4-184">hello *-VpnType* can be *RouteBased* (referred tooas a Dynamic Gateway in some documentation), or *PolicyBased* (referred tooas a Static Gateway in some documentation).</span></span> <span data-ttu-id="84df4-185">Další informace o typech brány VPN najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="84df4-185">For more information about VPN gateway types, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="84df4-186">Vyberte hello skladová položka brány, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="84df4-186">Select hello Gateway SKU that you want toouse.</span></span> <span data-ttu-id="84df4-187">Pro určité skladové jednotky (SKU) platí omezení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="84df4-187">There are configuration limitations for certain SKUs.</span></span> <span data-ttu-id="84df4-188">Další informace najdete v části [Skladové jednotky (SKU) brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="84df4-188">For more information, see [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span> <span data-ttu-id="84df4-189">Pokud dojde k chybě při vytváření brány VPN hello týkající se hello - GatewaySku, ověřte, že jste nainstalovali nejnovější verzi rutin prostředí PowerShell hello hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-189">If you get an error when creating hello VPN gateway regarding hello -GatewaySku, verify that you have installed hello latest version of hello PowerShell cmdlets.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <span data-ttu-id="84df4-190"><a name="ConfigureVPNDevice"></a>7. Konfigurace zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="84df4-190"><a name="ConfigureVPNDevice"></a>7. Configure your VPN device</span></span>

<span data-ttu-id="84df4-191">Připojení Site-to-Site tooan do místní sítě vyžaduje zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="84df4-191">Site-to-Site connections tooan on-premises network require a VPN device.</span></span> <span data-ttu-id="84df4-192">V tomto kroku nakonfigurujete zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="84df4-192">In this step, you configure your VPN device.</span></span> <span data-ttu-id="84df4-193">Při konfiguraci zařízení VPN, je třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="84df4-193">When configuring your VPN device, you need hello following:</span></span>

- <span data-ttu-id="84df4-194">Sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="84df4-194">A shared key.</span></span> <span data-ttu-id="84df4-195">To je hello stejný sdílený klíč, který zadáte při vytváření připojení Site-to-Site VPN.</span><span class="sxs-lookup"><span data-stu-id="84df4-195">This is hello same shared key that you specify when creating your Site-to-Site VPN connection.</span></span> <span data-ttu-id="84df4-196">V našich ukázkách používáme základní sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="84df4-196">In our examples, we use a basic shared key.</span></span> <span data-ttu-id="84df4-197">Doporučujeme, abyste vygenerování složitější klíče toouse.</span><span class="sxs-lookup"><span data-stu-id="84df4-197">We recommend that you generate a more complex key toouse.</span></span>
- <span data-ttu-id="84df4-198">Hello veřejnou IP adresu brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="84df4-198">hello Public IP address of your virtual network gateway.</span></span> <span data-ttu-id="84df4-199">Hello veřejnou IP adresu můžete zobrazit pomocí hello portálu Azure, PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="84df4-199">You can view hello public IP address by using hello Azure portal, PowerShell, or CLI.</span></span> <span data-ttu-id="84df4-200">toofind hello veřejnou IP adresu brány virtuální sítě pomocí prostředí PowerShell, použijte hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="84df4-200">toofind hello Public IP address of your virtual network gateway using PowerShell, use hello following example:</span></span>

  ```powershell
  Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG1
  ```

[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <span data-ttu-id="84df4-201"><a name="CreateConnection"></a>8. Vytvoření připojení VPN hello</span><span class="sxs-lookup"><span data-stu-id="84df4-201"><a name="CreateConnection"></a>8. Create hello VPN connection</span></span>

<span data-ttu-id="84df4-202">Dále vytvořte připojení Site-to-Site VPN hello mezi bránou virtuální sítě a zařízením VPN.</span><span class="sxs-lookup"><span data-stu-id="84df4-202">Next, create hello Site-to-Site VPN connection between your virtual network gateway and your VPN device.</span></span> <span data-ttu-id="84df4-203">Být jisti tooreplace hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="84df4-203">Be sure tooreplace hello values with your own.</span></span> <span data-ttu-id="84df4-204">Hello sdílený klíč musí odpovídat hello hodnotu, která jste použili pro konfiguraci zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="84df4-204">hello shared key must match hello value you used for your VPN device configuration.</span></span> <span data-ttu-id="84df4-205">Všimněte si, že hello '-ConnectionType' pro Site-to-Site je *IPsec*.</span><span class="sxs-lookup"><span data-stu-id="84df4-205">Notice that hello '-ConnectionType' for Site-to-Site is *IPsec*.</span></span>

1. <span data-ttu-id="84df4-206">Nastavení proměnných hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-206">Set hello variables.</span></span>
  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
  $local = Get-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1
  ```

2. <span data-ttu-id="84df4-207">Vytvořte připojení hello.</span><span class="sxs-lookup"><span data-stu-id="84df4-207">Create hello connection.</span></span>
  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite2 -ResourceGroupName TestRG1 `
  -Location 'East US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

<span data-ttu-id="84df4-208">Za malou chvíli hello bude vytvořeno připojení.</span><span class="sxs-lookup"><span data-stu-id="84df4-208">After a short while, hello connection will be established.</span></span>

## <span data-ttu-id="84df4-209"><a name="toverify"></a>9. Ověření připojení VPN hello</span><span class="sxs-lookup"><span data-stu-id="84df4-209"><a name="toverify"></a>9. Verify hello VPN connection</span></span>

<span data-ttu-id="84df4-210">Existuje několik různých způsobů tooverify připojení k síti VPN.</span><span class="sxs-lookup"><span data-stu-id="84df4-210">There are a few different ways tooverify your VPN connection.</span></span>

[!INCLUDE [Verify connection](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <span data-ttu-id="84df4-211"><a name="connectVM"></a>tooconnect tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="84df4-211"><a name="connectVM"></a>tooconnect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]


## <span data-ttu-id="84df4-212"><a name="modify"></a>Úprava předpon IP adres pro bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="84df4-212"><a name="modify"></a>Modify IP address prefixes for a local network gateway</span></span>

<span data-ttu-id="84df4-213">Pokud hello předpony IP adres, které chcete směrovat tooyour místní umístění změnit, můžete upravit hello brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="84df4-213">If hello IP address prefixes that you want routed tooyour on-premises location change, you can modify hello local network gateway.</span></span> <span data-ttu-id="84df4-214">K dispozici jsou dvě sady pokynů.</span><span class="sxs-lookup"><span data-stu-id="84df4-214">Two sets of instructions are provided.</span></span> <span data-ttu-id="84df4-215">Hello pokyny, které zvolíte, závisí na tom, zda jste již vytvořili připojení brány.</span><span class="sxs-lookup"><span data-stu-id="84df4-215">hello instructions you choose depend on whether you have already created your gateway connection.</span></span>

[!INCLUDE [Modify prefixes](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <span data-ttu-id="84df4-216"><a name="modifygwipaddress"></a>Upravit IP adresu brány hello pro bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="84df4-216"><a name="modifygwipaddress"></a>Modify hello gateway IP address for a local network gateway</span></span>

[!INCLUDE [Modify gateway IP address](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="84df4-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84df4-217">Next steps</span></span>

*  <span data-ttu-id="84df4-218">Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="84df4-218">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="84df4-219">Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="84df4-219">For more information, see [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="84df4-220">Informace o protokolu BGP najdete v tématu hello [přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [jak tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="84df4-220">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
