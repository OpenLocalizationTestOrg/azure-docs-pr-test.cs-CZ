---
title: "Připojte počítač tooan virtuální síť Azure pomocí Point-to-Site a ověření certifikátem: prostředí PowerShell | Microsoft Docs"
description: "Bezpečně připojte virtuální síť tooyour počítače tak, že vytvoříte připojení k bráně VPN Point-to-Site ověřování pomocí certifikátů. Tento článek vztahuje toohello modelu nasazení Resource Manager a používá prostředí PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2017
ms.author: cherylmc
ms.openlocfilehash: b962e4b1946a4ae17d4eb2b920ed54437bc26b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-powershell"></a><span data-ttu-id="483ea-104">Konfigurace tooa připojení Point-to-Site virtuální síť ověřování pomocí certifikátů: prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="483ea-104">Configure a Point-to-Site connection tooa VNet using certificate authentication: PowerShell</span></span>

<span data-ttu-id="483ea-105">Tento článek ukazuje, jak toocreate virtuální síť s připojením Point-to-Site v nasazení Resource Manager hello modelu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="483ea-105">This article shows you how toocreate a VNet with a Point-to-Site connection in hello Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="483ea-106">Tato konfigurace používá certifikáty tooauthenticate hello připojení klienta.</span><span class="sxs-lookup"><span data-stu-id="483ea-106">This configuration uses certificates tooauthenticate hello connecting client.</span></span> <span data-ttu-id="483ea-107">Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="483ea-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="483ea-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="483ea-108">Azure portal</span></span>](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="483ea-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="483ea-109">PowerShell</span></span>](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [<span data-ttu-id="483ea-110">Azure Portal (Classic)</span><span class="sxs-lookup"><span data-stu-id="483ea-110">Azure portal (classic)</span></span>](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

<span data-ttu-id="483ea-111">Brána sítě VPN typu Point-to-Site (P2S) umožňuje vytvoření bezpečného připojení virtuální sítě tooyour z jednotlivých klientských počítačů.</span><span class="sxs-lookup"><span data-stu-id="483ea-111">A Point-to-Site (P2S) VPN gateway lets you create a secure connection tooyour virtual network from an individual client computer.</span></span> <span data-ttu-id="483ea-112">Připojení point-to-Site VPN jsou užitečné, když chcete, aby tooconnect tooyour virtuální síti ze vzdáleného umístění, například při jsou telefonicky z domova nebo z konference.</span><span class="sxs-lookup"><span data-stu-id="483ea-112">Point-to-Site VPN connections are useful when you want tooconnect tooyour VNet from a remote location, such when you are telecommuting from home or a conference.</span></span> <span data-ttu-id="483ea-113">Síť VPN P2S je také užitečné řešení toouse místo Site-to-Site VPN, pokud máte pouze několik klientů, kteří potřebují tooconnect tooa virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="483ea-113">A P2S VPN is also a useful solution toouse instead of a Site-to-Site VPN when you have only a few clients that need tooconnect tooa VNet.</span></span>

<span data-ttu-id="483ea-114">Používá P2S hello Secure Socket SSTP (Tunneling Protocol), což je protokol VPN založené na protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="483ea-114">P2S uses hello Secure Socket Tunneling Protocol (SSTP), which is an SSL-based VPN protocol.</span></span> <span data-ttu-id="483ea-115">Připojení P2S VPN je vytvořeno spuštěním z hello klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="483ea-115">A P2S VPN connection is established by starting it from hello client computer.</span></span>

![Připojení počítače tooan virtuální síť Azure – diagram připojení Point-to-Site](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

<span data-ttu-id="483ea-117">Připojení point-to-Site certifikát ověřování vyžadovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="483ea-117">Point-to-Site certificate authentication connections require hello following:</span></span>

* <span data-ttu-id="483ea-118">Bránu VPN typu RouteBased.</span><span class="sxs-lookup"><span data-stu-id="483ea-118">A RouteBased VPN gateway.</span></span>
* <span data-ttu-id="483ea-119">Hello veřejný klíč (soubor .cer) pro kořenový certifikát, který je nahraný tooAzure.</span><span class="sxs-lookup"><span data-stu-id="483ea-119">hello public key (.cer file) for a root certificate, which is uploaded tooAzure.</span></span> <span data-ttu-id="483ea-120">Po nahrání certifikátu hello je považován za důvěryhodný certifikát a slouží k ověřování.</span><span class="sxs-lookup"><span data-stu-id="483ea-120">Once hello certificate is uploaded, it is considered a trusted certificate and is used for authentication.</span></span>
* <span data-ttu-id="483ea-121">Klientský certifikát, který je generována z hello kořenový certifikát a nainstalovat na každý klientský počítač, který se bude připojovat toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="483ea-121">A client certificate that is generated from hello root certificate and installed on each client computer that will connect toohello VNet.</span></span> <span data-ttu-id="483ea-122">Tento certifikát se používá k ověřování klienta.</span><span class="sxs-lookup"><span data-stu-id="483ea-122">This certificate is used for client authentication.</span></span>
* <span data-ttu-id="483ea-123">Konfigurační balíček klienta VPN.</span><span class="sxs-lookup"><span data-stu-id="483ea-123">A VPN client configuration package.</span></span> <span data-ttu-id="483ea-124">balíček konfigurace klienta VPN Hello obsahuje nezbytné informace hello hello klienta tooconnect toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="483ea-124">hello VPN client configuration package contains hello necessary information for hello client tooconnect toohello VNet.</span></span> <span data-ttu-id="483ea-125">balíček Hello nakonfiguruje hello existující klienta VPN, který je nativní toohello operačního systému Windows.</span><span class="sxs-lookup"><span data-stu-id="483ea-125">hello package configures hello existing VPN client that is native toohello Windows operating system.</span></span> <span data-ttu-id="483ea-126">Každý klient, který se připojuje musí být nakonfigurovaný pomocí konfiguračního balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-126">Each client that connects must be configured using hello configuration package.</span></span>

<span data-ttu-id="483ea-127">Připojení typu Point-to-Site nevyžadují zařízení VPN ani místní veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="483ea-127">Point-to-Site connections do not require a VPN device or an on-premises public-facing IP address.</span></span> <span data-ttu-id="483ea-128">Vytvoří se Hello připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol).</span><span class="sxs-lookup"><span data-stu-id="483ea-128">hello VPN connection is created over SSTP (Secure Socket Tunneling Protocol).</span></span> <span data-ttu-id="483ea-129">Na straně serveru hello podporujeme SSTP verze 1.0, 1.1 a 1.2.</span><span class="sxs-lookup"><span data-stu-id="483ea-129">On hello server side, we support SSTP versions 1.0, 1.1, and 1.2.</span></span> <span data-ttu-id="483ea-130">Klient Hello, rozhodne se které toouse verze.</span><span class="sxs-lookup"><span data-stu-id="483ea-130">hello client decides which version toouse.</span></span> <span data-ttu-id="483ea-131">Pro Windows 8.1 a novější se standardně používá SSTP verze 1.2.</span><span class="sxs-lookup"><span data-stu-id="483ea-131">For Windows 8.1 and above, SSTP uses 1.2 by default.</span></span> 

<span data-ttu-id="483ea-132">Další informace o připojení Point-to-Site najdete v tématu hello [Point-to-Site – nejčastější dotazy](#faq) na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="483ea-132">For more information about Point-to-Site connections, see hello [Point-to-Site FAQ](#faq) at hello end of this article.</span></span>

## <a name="before-beginning"></a><span data-ttu-id="483ea-133">Před zahájením</span><span class="sxs-lookup"><span data-stu-id="483ea-133">Before beginning</span></span>

* <span data-ttu-id="483ea-134">Ověřte, že máte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="483ea-134">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="483ea-135">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="483ea-135">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="483ea-136">Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-136">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="483ea-137">Další informace o instalaci rutin prostředí PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="483ea-137">For more information about installing PowerShell cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <span data-ttu-id="483ea-138"><a name="example"></a>Příklady hodnot</span><span class="sxs-lookup"><span data-stu-id="483ea-138"><a name="example"></a>Example values</span></span>

<span data-ttu-id="483ea-139">Můžete použít hello Příklad hodnoty toocreate testovací prostředí nebo najdete hodnoty toothese toobetter pochopit hello příklady v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="483ea-139">You can use hello example values toocreate a test environment, or refer toothese values toobetter understand hello examples in this article.</span></span> <span data-ttu-id="483ea-140">Nastaví proměnné hello v části [1](#declare) hello článku.</span><span class="sxs-lookup"><span data-stu-id="483ea-140">We set hello variables in section [1](#declare) of hello article.</span></span> <span data-ttu-id="483ea-141">Můžete buď použít jako návod hello kroky a použít hello hodnoty bez jejich změny nebo změnit je tooreflect prostředí.</span><span class="sxs-lookup"><span data-stu-id="483ea-141">You can either use hello steps as a walk-through and use hello values without changing them, or change them tooreflect your environment.</span></span> 

* <span data-ttu-id="483ea-142">**Název: VNet1**</span><span class="sxs-lookup"><span data-stu-id="483ea-142">**Name: VNet1**</span></span>
* <span data-ttu-id="483ea-143">**Adresní prostor: 192.168.0.0/16** a **10.254.0.0/16**</span><span class="sxs-lookup"><span data-stu-id="483ea-143">**Address space: 192.168.0.0/16** and **10.254.0.0/16**</span></span><br><span data-ttu-id="483ea-144">V tomto příkladu používáme více než jeden tooillustrate místo adres, který tato konfigurace funguje s více adresní prostory.</span><span class="sxs-lookup"><span data-stu-id="483ea-144">For this example, we use more than one address space tooillustrate that this configuration works with multiple address spaces.</span></span> <span data-ttu-id="483ea-145">Více adresních prostorů pro ni ale není potřeba.</span><span class="sxs-lookup"><span data-stu-id="483ea-145">However, multiple address spaces are not required for this configuration.</span></span>
* <span data-ttu-id="483ea-146">**Název podsítě: FrontEnd**</span><span class="sxs-lookup"><span data-stu-id="483ea-146">**Subnet name: FrontEnd**</span></span>
  * <span data-ttu-id="483ea-147">**Rozsah adres podsítě: 192.168.1.0/24**</span><span class="sxs-lookup"><span data-stu-id="483ea-147">**Subnet address range: 192.168.1.0/24**</span></span>
* <span data-ttu-id="483ea-148">**Název podsítě: BackEnd**</span><span class="sxs-lookup"><span data-stu-id="483ea-148">**Subnet name: BackEnd**</span></span>
  * <span data-ttu-id="483ea-149">**Rozsah adres podsítě: 10.254.1.0/24**</span><span class="sxs-lookup"><span data-stu-id="483ea-149">**Subnet address range: 10.254.1.0/24**</span></span>
* <span data-ttu-id="483ea-150">**Název podsítě: GatewaySubnet**</span><span class="sxs-lookup"><span data-stu-id="483ea-150">**Subnet name: GatewaySubnet**</span></span><br><span data-ttu-id="483ea-151">název podsítě Hello *GatewaySubnet* je povinné pro toowork brány VPN hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-151">hello Subnet name *GatewaySubnet* is mandatory for hello VPN gateway toowork.</span></span>
  * <span data-ttu-id="483ea-152">**Rozsah adres podsítě brány: 192.168.200.0/24**</span><span class="sxs-lookup"><span data-stu-id="483ea-152">**GatewaySubnet address range: 192.168.200.0/24**</span></span> 
* <span data-ttu-id="483ea-153">**Fond adres klienta VPN: 172.16.201.0/24**</span><span class="sxs-lookup"><span data-stu-id="483ea-153">**VPN client address pool: 172.16.201.0/24**</span></span><br><span data-ttu-id="483ea-154">Klienti VPN, které se připojují toohello sítě VNet pomocí tohoto připojení Point-to-Site získali IP adresu z hello fond adres klienta VPN.</span><span class="sxs-lookup"><span data-stu-id="483ea-154">VPN clients that connect toohello VNet using this Point-to-Site connection receive an IP address from hello VPN client address pool.</span></span>
* <span data-ttu-id="483ea-155">**Předplatné:** Pokud máte více než jedno předplatné, ověřte, že používáte hello správná.</span><span class="sxs-lookup"><span data-stu-id="483ea-155">**Subscription:** If you have more than one subscription, verify that you are using hello correct one.</span></span>
* <span data-ttu-id="483ea-156">**Skupina prostředků: TestRG**</span><span class="sxs-lookup"><span data-stu-id="483ea-156">**Resource Group: TestRG**</span></span>
* <span data-ttu-id="483ea-157">**Umístění: Východní USA**</span><span class="sxs-lookup"><span data-stu-id="483ea-157">**Location: East US**</span></span>
* <span data-ttu-id="483ea-158">**Serveru DNS: IP adresa** chcete toouse pro překlad názvu serveru DNS hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-158">**DNS Server: IP address** of hello DNS server that you want toouse for name resolution.</span></span>
* <span data-ttu-id="483ea-159">**Název brány: Vnet1GW**</span><span class="sxs-lookup"><span data-stu-id="483ea-159">**GW Name: Vnet1GW**</span></span>
* <span data-ttu-id="483ea-160">**Název veřejné IP adresy: VNet1GWPIP**</span><span class="sxs-lookup"><span data-stu-id="483ea-160">**Public IP name: VNet1GWPIP**</span></span>
* <span data-ttu-id="483ea-161">**Typ sítě VPN: RouteBased**</span><span class="sxs-lookup"><span data-stu-id="483ea-161">**VpnType: RouteBased**</span></span> 

## <span data-ttu-id="483ea-162"><a name="declare"></a>1. Přihlášení a nastavení proměnných</span><span class="sxs-lookup"><span data-stu-id="483ea-162"><a name="declare"></a>1. Log in and set variables</span></span>

<span data-ttu-id="483ea-163">V této části se přihlaste a deklarovat hello hodnoty používané pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="483ea-163">In this section, you log in and declare hello values used for this configuration.</span></span> <span data-ttu-id="483ea-164">Hello deklarovat hodnoty se používají v hello ukázkové skripty.</span><span class="sxs-lookup"><span data-stu-id="483ea-164">hello declared values are used in hello sample scripts.</span></span> <span data-ttu-id="483ea-165">Změňte hodnoty tooreflect hello svého vlastního prostředí.</span><span class="sxs-lookup"><span data-stu-id="483ea-165">Change hello values tooreflect your own environment.</span></span> <span data-ttu-id="483ea-166">Nebo můžete použít hello deklarovaný hodnoty a projít hello kroky jako cvičení.</span><span class="sxs-lookup"><span data-stu-id="483ea-166">Or, you can use hello declared values and go through hello steps as an exercise.</span></span>

1. <span data-ttu-id="483ea-167">Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="483ea-167">Open your PowerShell console with elevated privileges, and log in tooyour Azure account.</span></span> <span data-ttu-id="483ea-168">Tato rutina vás vyzve k zadání přihlašovacích údajů hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-168">This cmdlet prompts you for hello login credentials.</span></span> <span data-ttu-id="483ea-169">Po přihlášení stahování nastavení svého účtu, aby byly k dispozici tooAzure prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="483ea-169">After logging in, it downloads your account settings so that they are available tooAzure PowerShell.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```
2. <span data-ttu-id="483ea-170">Načtěte seznam předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="483ea-170">Get a list of your Azure subscriptions.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```
3. <span data-ttu-id="483ea-171">Zadejte hello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="483ea-171">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
4. <span data-ttu-id="483ea-172">Deklarujte hello proměnné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="483ea-172">Declare hello variables that you want toouse.</span></span> <span data-ttu-id="483ea-173">Použijte hello následující ukázka, nahraďte hello hodnoty pro vlastní, pokud je to nezbytné.</span><span class="sxs-lookup"><span data-stu-id="483ea-173">Use hello following sample, substituting hello values for your own when necessary.</span></span>

  ```powershell
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $DNS = "10.1.1.3"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <span data-ttu-id="483ea-174"><a name="ConfigureVNet"></a>2. Konfigurace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="483ea-174"><a name="ConfigureVNet"></a>2. Configure a VNet</span></span>

1. <span data-ttu-id="483ea-175">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="483ea-175">Create a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG -Location $Location
  ```
2. <span data-ttu-id="483ea-176">Vytvoření konfigurací podsítě pro virtuální síť hello jejich názvů hello *front-endu*, *back-end*, a *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="483ea-176">Create hello subnet configurations for hello virtual network, naming them *FrontEnd*, *BackEnd*, and *GatewaySubnet*.</span></span> <span data-ttu-id="483ea-177">Tyto předpony musí být součástí hello adresní prostor sítě VNet, která je deklarován.</span><span class="sxs-lookup"><span data-stu-id="483ea-177">These prefixes must be part of hello VNet address space that you declared.</span></span>

  ```powershell
  $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
  $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
  $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
  ```
3. <span data-ttu-id="483ea-178">Vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-178">Create hello virtual network.</span></span>

  <span data-ttu-id="483ea-179">V tomto příkladu hello DNS server je volitelné.</span><span class="sxs-lookup"><span data-stu-id="483ea-179">In this example, hello DNS server is optional.</span></span> <span data-ttu-id="483ea-180">Zadání hodnoty nevytvoří nový server DNS.</span><span class="sxs-lookup"><span data-stu-id="483ea-180">Specifying a value does not create a new DNS server.</span></span> <span data-ttu-id="483ea-181">Hello IP adresa serveru DNS, který zadáte musí být server DNS, který může překládat názvy hello hello prostředků, ke kterému se připojujete.</span><span class="sxs-lookup"><span data-stu-id="483ea-181">hello DNS server IP address that you specify should be a DNS server that can resolve hello names for hello resources you are connecting to.</span></span> <span data-ttu-id="483ea-182">V tomto příkladu jsme použili privátní IP adresy, ale je pravděpodobné, že se nejedná hello IP adresu serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="483ea-182">For this example, we used a private IP address, but it is likely that this is not hello IP address of your DNS server.</span></span> <span data-ttu-id="483ea-183">Být jisti toouse vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="483ea-183">Be sure toouse your own values.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
  ```
4. <span data-ttu-id="483ea-184">Zadejte proměnné hello hello virtuální sítě, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="483ea-184">Specify hello variables for hello virtual network you created.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  ```
5. <span data-ttu-id="483ea-185">Brána VPN musí mít veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="483ea-185">A VPN gateway must have a Public IP address.</span></span> <span data-ttu-id="483ea-186">Nejprve požádali o prostředek hello IP adresy a pak odkazovat tooit při vytváření brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="483ea-186">You first request hello IP address resource, and then refer tooit when creating your virtual network gateway.</span></span> <span data-ttu-id="483ea-187">Hello je přiřazená IP adresa dynamicky toohello prostředků při vytváření brány VPN hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-187">hello IP address is dynamically assigned toohello resource when hello VPN gateway is created.</span></span> <span data-ttu-id="483ea-188">Služba VPN Gateway aktuálně podporuje pouze *dynamické* přidělení veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="483ea-188">VPN Gateway currently only supports *Dynamic* Public IP address allocation.</span></span> <span data-ttu-id="483ea-189">Nemůžete si vyžádat statické přiřazení IP adresy.</span><span class="sxs-lookup"><span data-stu-id="483ea-189">You cannot request a Static Public IP address assignment.</span></span> <span data-ttu-id="483ea-190">Však neznamená, že hello IP adresa změní po byl přiřazen tooyour brány VPN.</span><span class="sxs-lookup"><span data-stu-id="483ea-190">However, it doesn't mean that hello IP address changes after it has been assigned tooyour VPN gateway.</span></span> <span data-ttu-id="483ea-191">Hello jenom jednou hello změny veřejné IP adresy je při hello brány je odstraní a znovu vytvoří.</span><span class="sxs-lookup"><span data-stu-id="483ea-191">hello only time hello Public IP address changes is when hello gateway is deleted and re-created.</span></span> <span data-ttu-id="483ea-192">V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN se nezmění.</span><span class="sxs-lookup"><span data-stu-id="483ea-192">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of your VPN gateway.</span></span>

  <span data-ttu-id="483ea-193">Požádejte o dynamicky přidělovanou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="483ea-193">Request a dynamically assigned public IP address.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```

## <span data-ttu-id="483ea-194"><a name="creategateway"></a>3. Vytvoření brány VPN hello</span><span class="sxs-lookup"><span data-stu-id="483ea-194"><a name="creategateway"></a>3. Create hello VPN gateway</span></span>

<span data-ttu-id="483ea-195">Nakonfigurujte a vytvořte hello brány virtuální sítě pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="483ea-195">Configure and create hello virtual network gateway for your VNet.</span></span>

* <span data-ttu-id="483ea-196">Hello *- GatewayType* musí být **Vpn** a hello *- VpnType* musí být **RouteBased**.</span><span class="sxs-lookup"><span data-stu-id="483ea-196">hello *-GatewayType* must be **Vpn** and hello *-VpnType* must be **RouteBased**.</span></span>
* <span data-ttu-id="483ea-197">Brána sítě VPN může trvat až minut toocomplete too45, v závislosti na hello [skladová položka brány](vpn-gateway-about-vpn-gateway-settings.md) vyberete.</span><span class="sxs-lookup"><span data-stu-id="483ea-197">A VPN gateway can take up too45 minutes toocomplete, depending on hello [gateway sku](vpn-gateway-about-vpn-gateway-settings.md) you select.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 `
```

## <span data-ttu-id="483ea-198"><a name="addresspool"></a>4. Přidejte fond adres klienta VPN hello</span><span class="sxs-lookup"><span data-stu-id="483ea-198"><a name="addresspool"></a>4. Add hello VPN client address pool</span></span>

<span data-ttu-id="483ea-199">Po dokončení vytvoření brány VPN hello můžete přidat fond adres klienta VPN hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-199">After hello VPN gateway finishes creating, you can add hello VPN client address pool.</span></span> <span data-ttu-id="483ea-200">Hello fond adres klienta VPN je hello rozsah, ze kterého klienti VPN hello přijímat IP adresu pro připojení.</span><span class="sxs-lookup"><span data-stu-id="483ea-200">hello VPN client address pool is hello range from which hello VPN clients receive an IP address when connecting.</span></span> <span data-ttu-id="483ea-201">Použijte privátní rozsah IP adres, který se nepřekrývá hello místní umístění, které můžete připojit z nebo s hello virtuální sítě, který chcete tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="483ea-201">Use a private IP address range that does not overlap with hello on-premises location that you connect from, or with hello VNet that you want tooconnect to.</span></span> <span data-ttu-id="483ea-202">V tomto příkladu hello fond adres klienta VPN je deklarován jako [proměnná](#declare) v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="483ea-202">In this example, hello VPN client address pool is declared as a [variable](#declare) in Step 1.</span></span>

```powershell
$Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
```

## <span data-ttu-id="483ea-203"><a name="Certificates"></a>5. Generování certifikátů</span><span class="sxs-lookup"><span data-stu-id="483ea-203"><a name="Certificates"></a>5. Generate certificates</span></span>

<span data-ttu-id="483ea-204">Certifikáty se používají klienti VPN Azure tooauthenticate pro sítě Point-to-Site VPN.</span><span class="sxs-lookup"><span data-stu-id="483ea-204">Certificates are used by Azure tooauthenticate VPN clients for Point-to-Site VPNs.</span></span> <span data-ttu-id="483ea-205">Můžete nahrát informace veřejného klíče hello z hello kořenový certifikát tooAzure.</span><span class="sxs-lookup"><span data-stu-id="483ea-205">You upload hello public key information of hello root certificate tooAzure.</span></span> <span data-ttu-id="483ea-206">Hello veřejný klíč je pak považován za 'důvěryhodné'.</span><span class="sxs-lookup"><span data-stu-id="483ea-206">hello public key is then considered 'trusted'.</span></span> <span data-ttu-id="483ea-207">Klientské certifikáty musí být vygenerovat z hello důvěryhodného kořenového certifikátu a následně je nainstalován na každém klientském počítači v úložišti certifikátů certifikáty – aktuální uživatel nebo osobní hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-207">Client certificates must be generated from hello trusted root certificate, and then installed on each client computer in hello Certificates-Current User/Personal certificate store.</span></span> <span data-ttu-id="483ea-208">certifikát Hello je použité tooauthenticate hello klienta při zahájí toohello připojení virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="483ea-208">hello certificate is used tooauthenticate hello client when it initiates a connection toohello VNet.</span></span> 

<span data-ttu-id="483ea-209">Pokud používáte certifikáty podepsané svým držitelem, musí se vytvořit pomocí konkrétních parametrů.</span><span class="sxs-lookup"><span data-stu-id="483ea-209">If you use self-signed certificates, they must be created using specific parameters.</span></span> <span data-ttu-id="483ea-210">Můžete vytvořit certifikát podepsaný svým držitelem pomocí hello pokyny pro [prostředí PowerShell a Windows 10](vpn-gateway-certificates-point-to-site.md), nebo pokud nemáte Windows 10, můžete použít [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).</span><span class="sxs-lookup"><span data-stu-id="483ea-210">You can create a self-signed certificate using hello instructions for [PowerShell and Windows 10](vpn-gateway-certificates-point-to-site.md), or, if you don't have Windows 10, you can use [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).</span></span> <span data-ttu-id="483ea-211">Je důležité, postupujte podle kroků hello v hello pokyny, při generování vlastních kořenových certifikátů a klientské certifikáty.</span><span class="sxs-lookup"><span data-stu-id="483ea-211">It's important that you follow hello steps in hello instructions when generating self-signed root certificates and client certificates.</span></span> <span data-ttu-id="483ea-212">Jinak hodnota hello certifikátů, který generovat nebudou kompatibilní s připojení P2S a taky bude docházet k chybě připojení.</span><span class="sxs-lookup"><span data-stu-id="483ea-212">Otherwise, hello certificates you generate will not be compatible with P2S connections and you will receive a connection error.</span></span>

### <span data-ttu-id="483ea-213"><a name="cer"></a>1. Získat soubor .cer hello pro hello kořenový certifikát</span><span class="sxs-lookup"><span data-stu-id="483ea-213"><a name="cer"></a>1. Obtain hello .cer file for hello root certificate</span></span>

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <span data-ttu-id="483ea-214"><a name="generate"></a>2. Vygenerování klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="483ea-214"><a name="generate"></a>2. Generate a client certificate</span></span>

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <span data-ttu-id="483ea-215"><a name="upload"></a>6. Nahrát hello kořenový certifikát informací veřejného klíče</span><span class="sxs-lookup"><span data-stu-id="483ea-215"><a name="upload"></a>6. Upload hello root certificate public key information</span></span>

<span data-ttu-id="483ea-216">Ověřte, že se dokončilo vytváření brány VPN.</span><span class="sxs-lookup"><span data-stu-id="483ea-216">Verify that your VPN gateway has finished creating.</span></span> <span data-ttu-id="483ea-217">Po jeho dokončení můžete nahrát hello souboru .cer (obsahující informace veřejného klíče hello) pro tooAzure důvěryhodný kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="483ea-217">Once it has completed, you can upload hello .cer file (which contains hello public key information) for a trusted root certificate tooAzure.</span></span> <span data-ttu-id="483ea-218">Po nahrání souboru a.cer Azure můžete ji použít tooauthenticate klientů, které jste nainstalovali klientský certifikát generují z hello důvěryhodný kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="483ea-218">Once a.cer file is uploaded, Azure can use it tooauthenticate clients that have installed a client certificate generated from hello trusted root certificate.</span></span> <span data-ttu-id="483ea-219">V případě potřeby můžete nahrávat další důvěryhodný kořenový certifikát soubory – až tooa celkem 20 – později.</span><span class="sxs-lookup"><span data-stu-id="483ea-219">You can upload additional trusted root certificate files - up tooa total of 20 - later, if needed.</span></span>

1. <span data-ttu-id="483ea-220">Deklarujte hello proměnná pro název certifikátu, nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="483ea-220">Declare hello variable for your certificate name, replacing hello value with your own.</span></span>

  ```powershell
  $P2SRootCertName = "P2SRootCert.cer"
  ```
2. <span data-ttu-id="483ea-221">Nahraďte cesta k souboru hello vlastní a pak spusťte rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-221">Replace hello file path with your own, and then run hello cmdlets.</span></span>

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
  ```
3. <span data-ttu-id="483ea-222">Nahrajte tooAzure hello informace o veřejném klíči.</span><span class="sxs-lookup"><span data-stu-id="483ea-222">Upload hello public key information tooAzure.</span></span> <span data-ttu-id="483ea-223">Po odeslání informací o certifikátu hello Azure zvažuje tento toobe důvěryhodný kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="483ea-223">Once hello certificate information is uploaded, Azure considers this toobe a trusted root certificate.</span></span>

   ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
  ```

## <span data-ttu-id="483ea-224"><a name="clientconfig"></a>7. Stáhněte si balíček konfigurace klienta VPN hello</span><span class="sxs-lookup"><span data-stu-id="483ea-224"><a name="clientconfig"></a>7. Download hello VPN client configuration package</span></span>

<span data-ttu-id="483ea-225">tooconnect tooa sítě VNet pomocí sítě VPN Point-to-Site, každý klient musíte nainstalovat balíček konfigurace klienta, který konfiguruje hello Nativní klient VPN s nastavením hello a soubory, které jsou nezbytné tooconnect toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="483ea-225">tooconnect tooa VNet using a Point-to-Site VPN, each client must install a client configuration package that configures hello native VPN client with hello settings and files that are necessary tooconnect toohello virtual network.</span></span> <span data-ttu-id="483ea-226">balíček konfigurace klienta VPN Hello nakonfiguruje Nativní klient VPN ve Windows hello, neinstaluje klienta VPN nové nebo jiné.</span><span class="sxs-lookup"><span data-stu-id="483ea-226">hello VPN client configuration package configures hello native Windows VPN client, it doesn't install a new or different VPN client.</span></span> 

<span data-ttu-id="483ea-227">Hello stejné konfigurace klienta VPN pomocí balíčku v každém klientském počítači, můžete použít také hello verze odpovídá hello architektura pro klienta hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-227">You can use hello same VPN client configuration package on each client computer, as long as hello version matches hello architecture for hello client.</span></span> <span data-ttu-id="483ea-228">Seznam hello klientské operační systémy, které jsou podporovány, naleznete v části hello [připojeníPoint-to-Site – nejčastější dotazy](#faq) na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="483ea-228">For hello list of client operating systems that are supported, see hello [Point-to-Site connections FAQ](#faq) at hello end of this article.</span></span>

1. <span data-ttu-id="483ea-229">Po vytvoření brány hello můžete vygenerovat a stáhněte balíček konfigurace klienta hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-229">After hello gateway has been created, you can generate and download hello client configuration package.</span></span> <span data-ttu-id="483ea-230">Tento příklad stáhne hello balíček pro 64bitové klienty.</span><span class="sxs-lookup"><span data-stu-id="483ea-230">This example downloads hello package for 64-bit clients.</span></span> <span data-ttu-id="483ea-231">Pokud chcete toodownload hello 32bitová verze klienta, nahraďte 'Amd64' 'x86'.</span><span class="sxs-lookup"><span data-stu-id="483ea-231">If you want toodownload hello 32-bit client, replace 'Amd64' with 'x86'.</span></span> <span data-ttu-id="483ea-232">Můžete také stáhnout hello klienta VPN pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="483ea-232">You can also download hello VPN client by using hello Azure portal.</span></span>

  ```powershell
  Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
  -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
  ```
2. <span data-ttu-id="483ea-233">Zkopírujte a vložte hello odkaz, který je vrácen tooa webového prohlížeče toodownload hello balíčku, dbejte na to tooremove hello uvozovky, které obaluje hello odkaz.</span><span class="sxs-lookup"><span data-stu-id="483ea-233">Copy and paste hello link that is returned tooa web browser toodownload hello package, taking care tooremove hello quotes surrounding hello link.</span></span> 
3. <span data-ttu-id="483ea-234">Stáhněte a nainstalujte balíček hello na klientském počítači hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-234">Download and install hello package on hello client computer.</span></span> <span data-ttu-id="483ea-235">Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**.</span><span class="sxs-lookup"><span data-stu-id="483ea-235">If you see a SmartScreen popup, click **More info**, then **Run anyway**.</span></span> <span data-ttu-id="483ea-236">Můžete také uložit balíček tooinstall hello na další klientské počítače.</span><span class="sxs-lookup"><span data-stu-id="483ea-236">You can also save hello package tooinstall on other client computers.</span></span>
4. <span data-ttu-id="483ea-237">V počítači klienta hello přejděte příliš**nastavení sítě** a klikněte na tlačítko **VPN**.</span><span class="sxs-lookup"><span data-stu-id="483ea-237">On hello client computer, navigate too**Network Settings** and click **VPN**.</span></span> <span data-ttu-id="483ea-238">Hello připojení VPN se zobrazuje název hello hello virtuální sítě, který se připojuje ke službě.</span><span class="sxs-lookup"><span data-stu-id="483ea-238">hello VPN connection shows hello name of hello virtual network that it connects to.</span></span>

## <span data-ttu-id="483ea-239"><a name="clientcertificate"></a>8. Instalace exportovaného klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="483ea-239"><a name="clientcertificate"></a>8. Install an exported client certificate</span></span>

<span data-ttu-id="483ea-240">Pokud chcete připojení toocreate P2S z klientského počítače, než hello, kterou používá toogenerate hello klientské certifikáty, musíte tooinstall klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="483ea-240">If you want toocreate a P2S connection from a client computer other than hello one you used toogenerate hello client certificates, you need tooinstall a client certificate.</span></span> <span data-ttu-id="483ea-241">Při instalaci klientského certifikátu, je nutné hello heslo, které byla vytvořena, když byl exportován hello klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="483ea-241">When installing a client certificate, you need hello password that was created when hello client certificate was exported.</span></span> <span data-ttu-id="483ea-242">Obvykle je právě řádu dvakrát kliknete na soubor certifikátu hello a nainstalujte ho.</span><span class="sxs-lookup"><span data-stu-id="483ea-242">Typically, it is just a matter of double-clicking hello certificate and installing it.</span></span>

<span data-ttu-id="483ea-243">Zkontrolujte, zda text hello klientský certifikát byl exportován jako .pfx společně s hello celý řetěz certifikátů (což je výchozí hello).</span><span class="sxs-lookup"><span data-stu-id="483ea-243">Make sure hello client certificate was exported as a .pfx along with hello entire certificate chain (which is hello default).</span></span> <span data-ttu-id="483ea-244">Jinak hello kořenový certifikát informace není přítomna na klientském počítači hello a hello klient nebude moct tooauthenticate správně.</span><span class="sxs-lookup"><span data-stu-id="483ea-244">Otherwise, hello root certificate information isn't present on hello client computer and hello client won't be able tooauthenticate properly.</span></span> <span data-ttu-id="483ea-245">Další informace najdete v tématu [Instalace exportovaného klientského certifikátu](vpn-gateway-certificates-point-to-site.md#install).</span><span class="sxs-lookup"><span data-stu-id="483ea-245">For more information, see [Install an exported client certificate](vpn-gateway-certificates-point-to-site.md#install).</span></span> 

## <span data-ttu-id="483ea-246"><a name="connect"></a>9. Připojit tooAzure</span><span class="sxs-lookup"><span data-stu-id="483ea-246"><a name="connect"></a>9. Connect tooAzure</span></span>

1. <span data-ttu-id="483ea-247">tooconnect tooyour virtuální síť, v klientském počítači hello přejděte tooVPN připojení a vyhledejte hello připojení VPN, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="483ea-247">tooconnect tooyour VNet, on hello client computer, navigate tooVPN connections and locate hello VPN connection that you created.</span></span> <span data-ttu-id="483ea-248">Je název hello stejný název jako vaše virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="483ea-248">It is named hello same name as your virtual network.</span></span> <span data-ttu-id="483ea-249">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="483ea-249">Click **Connect**.</span></span> <span data-ttu-id="483ea-250">Zobrazí se zpráva se zobrazí s odkazuje toousing hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="483ea-250">A pop-up message may appear that refers toousing hello certificate.</span></span> <span data-ttu-id="483ea-251">Klikněte na tlačítko **pokračovat** toouse se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="483ea-251">Click **Continue** toouse elevated privileges.</span></span> 
2. <span data-ttu-id="483ea-252">Na hello **připojení** stavové stránce klikněte na tlačítko **připojit** toostart hello připojení.</span><span class="sxs-lookup"><span data-stu-id="483ea-252">On hello **Connection** status page, click **Connect** toostart hello connection.</span></span> <span data-ttu-id="483ea-253">Pokud se zobrazí **vybrat certifikát** obrazovky, ověřte, zda je zobrazení certifikátu klienta hello hello jeden, které chcete toouse tooconnect.</span><span class="sxs-lookup"><span data-stu-id="483ea-253">If you see a **Select Certificate** screen, verify that hello client certificate showing is hello one that you want toouse tooconnect.</span></span> <span data-ttu-id="483ea-254">Pokud není, použijte hello šipku tooselect hello správný certifikát a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="483ea-254">If it is not, use hello drop-down arrow tooselect hello correct certificate, and then click **OK**.</span></span>

  ![Klient VPN připojí tooAzure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. <span data-ttu-id="483ea-256">Vaše připojení bylo vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="483ea-256">Your connection is established.</span></span>

  ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

#### <a name="troubleshooting-p2s-connections"></a><span data-ttu-id="483ea-258">Řešení potíží s připojeními P2S</span><span class="sxs-lookup"><span data-stu-id="483ea-258">Troubleshooting P2S connections</span></span>

[!INCLUDE [client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <span data-ttu-id="483ea-259"><a name="verify"></a>10. Ověření stavu připojení</span><span class="sxs-lookup"><span data-stu-id="483ea-259"><a name="verify"></a>10. Verify your connection</span></span>

1. <span data-ttu-id="483ea-260">tooverify, že je připojení k síti VPN aktivní, otevřete příkazový řádek se zvýšenými oprávněními a spusťte *ipconfig/all*.</span><span class="sxs-lookup"><span data-stu-id="483ea-260">tooverify that your VPN connection is active, open an elevated command prompt, and run *ipconfig/all*.</span></span>
2. <span data-ttu-id="483ea-261">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-261">View hello results.</span></span> <span data-ttu-id="483ea-262">Všimněte si, že hello IP adresu, kterou jste obdrželi je jednou z adres hello v rámci hello Point-to-Site fond adres klienta v síti VPN, který jste zadali v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="483ea-262">Notice that hello IP address you received is one of hello addresses within hello Point-to-Site VPN Client Address Pool that you specified in your configuration.</span></span> <span data-ttu-id="483ea-263">výsledky Hello jsou podobné toothis příklad:</span><span class="sxs-lookup"><span data-stu-id="483ea-263">hello results are similar toothis example:</span></span>

  ```
  PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
  ```

## <span data-ttu-id="483ea-264"><a name="connectVM"></a>Připojit tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="483ea-264"><a name="connectVM"></a>Connect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <span data-ttu-id="483ea-265"><a name="addremovecert"></a>Přidání nebo odebrání kořenového certifikátu</span><span class="sxs-lookup"><span data-stu-id="483ea-265"><a name="addremovecert"></a>Add or remove a root certificate</span></span>

<span data-ttu-id="483ea-266">Důvěryhodný kořenový certifikát můžete do Azure přidat nebo ho z Azure odebrat.</span><span class="sxs-lookup"><span data-stu-id="483ea-266">You can add and remove trusted root certificates from Azure.</span></span> <span data-ttu-id="483ea-267">Když odeberete kořenový certifikát, klienti, kteří mají certifikát generují z hello kořenový certifikát nemůže ověřit, nebude možné tooconnect.</span><span class="sxs-lookup"><span data-stu-id="483ea-267">When you remove a root certificate, clients that have a certificate generated from hello root certificate can't authenticate and won't be able tooconnect.</span></span> <span data-ttu-id="483ea-268">Pokud chcete klienta tooauthenticate a připojit, je nutné vygenerovat nový klientský certifikát z kořenového certifikátu, který je důvěryhodný (nahrané) tooAzure tooinstall.</span><span class="sxs-lookup"><span data-stu-id="483ea-268">If you want a client tooauthenticate and connect, you need tooinstall a new client certificate generated from a root certificate that is trusted (uploaded) tooAzure.</span></span>

### <span data-ttu-id="483ea-269"><a name="addtrustedroot"></a>tooadd důvěryhodného kořenového certifikátu</span><span class="sxs-lookup"><span data-stu-id="483ea-269"><a name="addtrustedroot"></a>tooadd a trusted root certificate</span></span>

<span data-ttu-id="483ea-270">Můžete přidat až too20 kořenový certifikát .cer soubory tooAzure.</span><span class="sxs-lookup"><span data-stu-id="483ea-270">You can add up too20 root certificate .cer files tooAzure.</span></span> <span data-ttu-id="483ea-271">Hello následující kroky nápovědy přidat kořenový certifikát:</span><span class="sxs-lookup"><span data-stu-id="483ea-271">hello following steps help you add a root certificate:</span></span>

#### <span data-ttu-id="483ea-272"><a name="certmethod1"></a>Metoda 1</span><span class="sxs-lookup"><span data-stu-id="483ea-272"><a name="certmethod1"></a>Method 1</span></span>

<span data-ttu-id="483ea-273">Toto je hello nejúčinnější metoda tooupload kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="483ea-273">This is hello most efficient method tooupload a root certificate.</span></span>

1. <span data-ttu-id="483ea-274">Příprava tooupload soubor .cer hello:</span><span class="sxs-lookup"><span data-stu-id="483ea-274">Prepare hello .cer file tooupload:</span></span>

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert3.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64_3 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64_3
  ```
2. <span data-ttu-id="483ea-275">Nahrajte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-275">Upload hello file.</span></span> <span data-ttu-id="483ea-276">Najednou můžete nahrát jenom jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="483ea-276">You can only upload one file at a time.</span></span>

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64_3
  ```

3. <span data-ttu-id="483ea-277">tooverify tento soubor certifikátu hello odeslat:</span><span class="sxs-lookup"><span data-stu-id="483ea-277">tooverify that hello certificate file uploaded:</span></span>

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

#### <span data-ttu-id="483ea-278"><a name="certmethod2"></a>Metoda 2</span><span class="sxs-lookup"><span data-stu-id="483ea-278"><a name="certmethod2"></a>Method 2</span></span>

<span data-ttu-id="483ea-279">Tato metoda je nemá další kroky než metoda 1, ale má hello stejného výsledku.</span><span class="sxs-lookup"><span data-stu-id="483ea-279">This method is has more steps than Method 1, but has hello same result.</span></span> <span data-ttu-id="483ea-280">Je zahrnut v případě, že potřebujete data certifikátu tooview hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-280">It is included in case you need tooview hello certificate data.</span></span>

1. <span data-ttu-id="483ea-281">Vytvoření a přípravě hello nového kořenového certifikátu tooadd tooAzure.</span><span class="sxs-lookup"><span data-stu-id="483ea-281">Create and prepare hello new root certificate tooadd tooAzure.</span></span> <span data-ttu-id="483ea-282">Export hello veřejného klíče, protože kódování Base-64 X.509 (. CER) a otevřete jej pomocí textového editoru.</span><span class="sxs-lookup"><span data-stu-id="483ea-282">Export hello public key as a Base-64 encoded X.509 (.CER) and open it with a text editor.</span></span> <span data-ttu-id="483ea-283">Zkopírujte hodnoty hello, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="483ea-283">Copy hello values, as shown in hello following example:</span></span>

  ![certifikát](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

  > [!NOTE]
  > <span data-ttu-id="483ea-285">Při kopírování dat hello certifikátu, ujistěte se, že zkopírujete hello text jako jeden řádek průběžné bez návrat na začátek a odřádkování.</span><span class="sxs-lookup"><span data-stu-id="483ea-285">When copying hello certificate data, make sure that you copy hello text as one continuous line without carriage returns or line feeds.</span></span> <span data-ttu-id="483ea-286">Může být nutné toomodify zobrazení hello textového editoru too'Show Symbol/zobrazit všechny znaky toosee hello znaků CR vrátí a řádek informačních kanálů.</span><span class="sxs-lookup"><span data-stu-id="483ea-286">You may need toomodify your view in hello text editor too'Show Symbol/Show all characters' toosee hello carriage returns and line feeds.</span></span>
  >
  >

2. <span data-ttu-id="483ea-287">Zadejte název certifikátu hello a informace o klíči jako proměnnou.</span><span class="sxs-lookup"><span data-stu-id="483ea-287">Specify hello certificate name and key information as a variable.</span></span> <span data-ttu-id="483ea-288">Informace o hello nahraďte vlastním, jak je uvedené v hello následující příklad:</span><span class="sxs-lookup"><span data-stu-id="483ea-288">Replace hello information with your own, as shown in hello following example:</span></span>

  ```powershell
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
3. <span data-ttu-id="483ea-289">Přidáte nový certifikát kořenové hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-289">Add hello new root certificate.</span></span> <span data-ttu-id="483ea-290">Nelze přidat více certifikátů současně.</span><span class="sxs-lookup"><span data-stu-id="483ea-290">You can only add one certificate at a time.</span></span>

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
4. <span data-ttu-id="483ea-291">Můžete ověřit, že tento nový certifikát hello pomocí hello následující ukázka správně přidány:</span><span class="sxs-lookup"><span data-stu-id="483ea-291">You can verify that hello new certificate was added correctly by using hello following example:</span></span>

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

### <span data-ttu-id="483ea-292"><a name="removerootcert"></a>tooremove kořenového certifikátu</span><span class="sxs-lookup"><span data-stu-id="483ea-292"><a name="removerootcert"></a>tooremove a root certificate</span></span>

1. <span data-ttu-id="483ea-293">Deklarujte proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-293">Declare hello variables.</span></span>

  ```powershell
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
2. <span data-ttu-id="483ea-294">Odeberte certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-294">Remove hello certificate.</span></span>

  ```powershell
  Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
3. <span data-ttu-id="483ea-295">Hello použijte následující příklad tooverify, který hello certifikátu byla úspěšně odebrána.</span><span class="sxs-lookup"><span data-stu-id="483ea-295">Use hello following example tooverify that hello certificate was removed successfully.</span></span>

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

## <span data-ttu-id="483ea-296"><a name="revoke"></a>Odvolání klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="483ea-296"><a name="revoke"></a>Revoke a client certificate</span></span>

<span data-ttu-id="483ea-297">Certifikáty klientů lze odvolat.</span><span class="sxs-lookup"><span data-stu-id="483ea-297">You can revoke client certificates.</span></span> <span data-ttu-id="483ea-298">Hello certifikátu, seznamu odvolaných certifikátů můžete tooselectively Odepřít připojení Point-to-Site na základě jednotlivých klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="483ea-298">hello certificate revocation list allows you tooselectively deny Point-to-Site connectivity based on individual client certificates.</span></span> <span data-ttu-id="483ea-299">To se liší od odebrání důvěryhodného kořenového certifikátu.</span><span class="sxs-lookup"><span data-stu-id="483ea-299">This is different than removing a trusted root certificate.</span></span> <span data-ttu-id="483ea-300">Pokud odeberete z Azure důvěryhodného kořenového certifikátu .cer, odvolá hello přístup pro všechny klientské certifikáty generované podepsané pomocí hello odvolané kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="483ea-300">If you remove a trusted root certificate .cer from Azure, it revokes hello access for all client certificates generated/signed by hello revoked root certificate.</span></span> <span data-ttu-id="483ea-301">Odvolání certifikátu klienta, nikoli hello kořenový certifikát, umožňuje hello další certifikáty, které byly generovány od hello kořenový certifikát toocontinue toobe používá k ověřování.</span><span class="sxs-lookup"><span data-stu-id="483ea-301">Revoking a client certificate, rather than hello root certificate, allows hello other certificates that were generated from hello root certificate toocontinue toobe used for authentication.</span></span>

<span data-ttu-id="483ea-302">běžnou praxí Hello je toouse hello kořenový certifikát toomanage přístup na tým nebo organizace úrovních, při použití odvolané klientské certifikáty pro řízení přístupu podrobných na jednotlivé uživatele.</span><span class="sxs-lookup"><span data-stu-id="483ea-302">hello common practice is toouse hello root certificate toomanage access at team or organization levels, while using revoked client certificates for fine-grained access control on individual users.</span></span>

### <span data-ttu-id="483ea-303"><a name="revokeclientcert"></a>toorevoke certifikát klienta</span><span class="sxs-lookup"><span data-stu-id="483ea-303"><a name="revokeclientcert"></a>toorevoke a client certificate</span></span>

1. <span data-ttu-id="483ea-304">Načtěte hello kryptografický otisk certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="483ea-304">Retrieve hello client certificate thumbprint.</span></span> <span data-ttu-id="483ea-305">Další informace najdete v tématu [jak tooretrieve hello kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx).</span><span class="sxs-lookup"><span data-stu-id="483ea-305">For more information, see [How tooretrieve hello Thumbprint of a Certificate](https://msdn.microsoft.com/library/ms734695.aspx).</span></span>
2. <span data-ttu-id="483ea-306">Zkopírujte hello informace tooa textovém editoru a odeberte všechny mezery tak, aby se souvislý řetězec.</span><span class="sxs-lookup"><span data-stu-id="483ea-306">Copy hello information tooa text editor and remove all spaces so that it is a continuous string.</span></span> <span data-ttu-id="483ea-307">Tento řetězec je deklarován jako proměnné v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-307">This string is declared as a variable in hello next step.</span></span>
3. <span data-ttu-id="483ea-308">Deklarujte proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-308">Declare hello variables.</span></span> <span data-ttu-id="483ea-309">Ujistěte se, že kryptografický otisk toodeclare hello, který jste získali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-309">Make sure toodeclare hello thumbprint you retrieved in hello previous step.</span></span>

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
4. <span data-ttu-id="483ea-310">Přidejte hello kryptografický otisk toohello seznam odvolaných certifikátů.</span><span class="sxs-lookup"><span data-stu-id="483ea-310">Add hello thumbprint toohello list of revoked certificates.</span></span> <span data-ttu-id="483ea-311">Zobrazí "Succeeded", pokud byl přidán hello kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="483ea-311">You see "Succeeded" when hello thumbprint has been added.</span></span>

  ```powershell
  Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
  -Thumbprint $RevokedThumbprint1
  ```
5. <span data-ttu-id="483ea-312">Ověřte, že kryptografický otisk tohoto hello byl přidán toohello seznam odvolaných certifikátů.</span><span class="sxs-lookup"><span data-stu-id="483ea-312">Verify that hello thumbprint was added toohello certificate revocation list.</span></span>

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```
6. <span data-ttu-id="483ea-313">Po přidání hello kryptografický otisk certifikátu hello se už nedá použité tooconnect.</span><span class="sxs-lookup"><span data-stu-id="483ea-313">After hello thumbprint has been added, hello certificate can no longer be used tooconnect.</span></span> <span data-ttu-id="483ea-314">Klienti, které se pokusí použít tento certifikát tooconnect zobrazí zpráva, že tento certifikát hello již není platný.</span><span class="sxs-lookup"><span data-stu-id="483ea-314">Clients that try tooconnect using this certificate receive a message saying that hello certificate is no longer valid.</span></span>

### <span data-ttu-id="483ea-315"><a name="reinstateclientcert"></a>tooreinstate certifikát klienta</span><span class="sxs-lookup"><span data-stu-id="483ea-315"><a name="reinstateclientcert"></a>tooreinstate a client certificate</span></span>

<span data-ttu-id="483ea-316">Odebráním hello kryptografický otisk z hello seznam odvolané klientské certifikáty lze obnovit certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="483ea-316">You can reinstate a client certificate by removing hello thumbprint from hello list of revoked client certificates.</span></span>

1. <span data-ttu-id="483ea-317">Deklarujte proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-317">Declare hello variables.</span></span> <span data-ttu-id="483ea-318">Ujistěte se, že deklarujete hello správné kryptografický otisk certifikátu hello, které chcete tooreinstate.</span><span class="sxs-lookup"><span data-stu-id="483ea-318">Make sure you declare hello correct thumbprint for hello certificate that you want tooreinstate.</span></span>

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
2. <span data-ttu-id="483ea-319">Kryptografický otisk certifikátu hello odeberte ze seznamu odvolaných certifikátů hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-319">Remove hello certificate thumbprint from hello certificate revocation list.</span></span>

  ```powershell
  Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
  ```
3. <span data-ttu-id="483ea-320">Zkontrolujte, pokud kryptografický otisk hello se odebral ze seznamu odvolaných hello.</span><span class="sxs-lookup"><span data-stu-id="483ea-320">Check if hello thumbprint is removed from hello revoked list.</span></span>

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```

## <span data-ttu-id="483ea-321"><a name="faq"></a>Nejčastější dotazy týkající se připojení Point-to-Site</span><span class="sxs-lookup"><span data-stu-id="483ea-321"><a name="faq"></a>Point-to-Site FAQ</span></span>

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="483ea-322">Další kroky</span><span class="sxs-lookup"><span data-stu-id="483ea-322">Next steps</span></span>
<span data-ttu-id="483ea-323">Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="483ea-323">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="483ea-324">Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="483ea-324">For more information, see [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span> <span data-ttu-id="483ea-325">toounderstand Další informace o sítě a virtuálních počítačů najdete v části [přehled sítě Azure a virtuální počítač s Linuxem](../virtual-machines/linux/azure-vm-network-overview.md).</span><span class="sxs-lookup"><span data-stu-id="483ea-325">toounderstand more about networking and virtual machines, see [Azure and Linux VM network overview](../virtual-machines/linux/azure-vm-network-overview.md).</span></span>
