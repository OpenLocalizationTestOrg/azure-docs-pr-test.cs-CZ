---
title: "Připojení klasické virtuální sítě k virtuálním sítím Azure Resource Manager: prostředí PowerShell | Microsoft Docs"
description: "Naučte se vytvořit připojení VPN mezi klasické virtuální sítě a virtuální sítě Resource Manager pomocí brány sítě VPN a prostředí PowerShell"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: f17c3bf0-5cc9-4629-9928-1b72d0c9340b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
ms.openlocfilehash: 842a32e5304977af92706cdda464286983122247
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a><span data-ttu-id="2c6af-103">Připojení virtuálních sítí z různých modelů nasazení pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="2c6af-103">Connect virtual networks from different deployment models using PowerShell</span></span>



<span data-ttu-id="2c6af-104">Tento článek ukazuje, jak se připojit virtuální sítě classic do Resource Manager virtuální sítě tak, aby prostředky, které jsou umístěné v samostatné nasazení modely pro komunikaci mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="2c6af-104">This article shows you how to connect classic VNets to Resource Manager VNets to allow the resources located in the separate deployment models to communicate with each other.</span></span> <span data-ttu-id="2c6af-105">Kroky v tomto článku pomocí prostředí PowerShell, ale můžete také vytvořit této konfigurace pomocí portálu Azure tak, že vyberete článek z tohoto seznamu.</span><span class="sxs-lookup"><span data-stu-id="2c6af-105">The steps in this article use PowerShell, but you can also create this configuration using the Azure portal by selecting the article from this list.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2c6af-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2c6af-106">Portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="2c6af-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c6af-107">PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

<span data-ttu-id="2c6af-108">Připojení klasické virtuální sítě k virtuální síti Resource Manager je podobné propojení virtuální sítě do umístění místního webu.</span><span class="sxs-lookup"><span data-stu-id="2c6af-108">Connecting a classic VNet to a Resource Manager VNet is similar to connecting a VNet to an on-premises site location.</span></span> <span data-ttu-id="2c6af-109">Oba typy připojení využívají bránu VPN k poskytnutí zabezpečeného tunelového propojení prostřednictvím protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="2c6af-109">Both connectivity types use a VPN gateway to provide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="2c6af-110">Můžete vytvořit připojení mezi virtuální sítě, které jsou v různých předplatných a v různých oblastech.</span><span class="sxs-lookup"><span data-stu-id="2c6af-110">You can create a connection between VNets that are in different subscriptions and in different regions.</span></span> <span data-ttu-id="2c6af-111">Virtuální sítě, které už máte připojení k místní sítě, můžete také připojit, pokud je brány, které byly nakonfigurovány k dynamické nebo založené na trasách.</span><span class="sxs-lookup"><span data-stu-id="2c6af-111">You can also connect VNets that already have connections to on-premises networks, as long as the gateway that they have been configured with is dynamic or route-based.</span></span> <span data-ttu-id="2c6af-112">Další informace o propojeních VNet-to-VNet najdete v části [Nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="2c6af-112">For more information about VNet-to-VNet connections, see the [VNet-to-VNet FAQ](#faq) at the end of this article.</span></span> 

<span data-ttu-id="2c6af-113">Pokud vaše virtuální sítě jsou ve stejné oblasti, můžete místo toho zvažte připojení pomocí virtuální sítě partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="2c6af-113">If your VNets are in the same region, you may want to instead consider connecting them using VNet Peering.</span></span> <span data-ttu-id="2c6af-114">Partnerské vztahy virtuálních sítí nepoužívají bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="2c6af-114">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="2c6af-115">Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2c6af-115">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> 

## <a name="before-beginning"></a><span data-ttu-id="2c6af-116">Před zahájením</span><span class="sxs-lookup"><span data-stu-id="2c6af-116">Before beginning</span></span>

<span data-ttu-id="2c6af-117">Následující postup vás provede procesem nastavení, která je potřeba nakonfigurovat bránu dynamické nebo založené na směrování pro každou virtuální síť a vytvoření připojení VPN mezi bránami.</span><span class="sxs-lookup"><span data-stu-id="2c6af-117">The following steps walk you through the settings necessary to configure a dynamic or route-based gateway for each VNet and create a VPN connection between the gateways.</span></span> <span data-ttu-id="2c6af-118">Tato konfigurace nepodporuje statickou nebo na základě zásad brány.</span><span class="sxs-lookup"><span data-stu-id="2c6af-118">This configuration does not support static or policy-based gateways.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="2c6af-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2c6af-119">Prerequisites</span></span>

* <span data-ttu-id="2c6af-120">Již byly vytvořeny obě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2c6af-120">Both VNets have already been created.</span></span>
* <span data-ttu-id="2c6af-121">Rozsahy adres pro virtuální sítě není navzájem překrývají nebo nepřekrývá s žádným z rozsahů pro další připojení, které brány může být připojen k.</span><span class="sxs-lookup"><span data-stu-id="2c6af-121">The address ranges for the VNets do not overlap with each other, or overlap with any of the ranges for other connections that the gateways may be connected to.</span></span>
* <span data-ttu-id="2c6af-122">Nainstalovali jste nejnovější rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c6af-122">You have installed the latest PowerShell cmdlets.</span></span> <span data-ttu-id="2c6af-123">V tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="2c6af-123">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span> <span data-ttu-id="2c6af-124">Ujistěte se, že instalujete službu správy (SM) a rutiny Resource Manager (RM).</span><span class="sxs-lookup"><span data-stu-id="2c6af-124">Make sure you install both the Service Management (SM) and the Resource Manager (RM) cmdlets.</span></span> 

### <span data-ttu-id="2c6af-125"><a name="exampleref"></a>Příklady nastavení</span><span class="sxs-lookup"><span data-stu-id="2c6af-125"><a name="exampleref"></a>Example settings</span></span>

<span data-ttu-id="2c6af-126">Tyto hodnoty můžete použít k vytvoření testovacího prostředí nebo můžou sloužit k lepšímu pochopení příkladů v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="2c6af-126">You can use these values to create a test environment, or refer to them to better understand the examples in this article.</span></span>

<span data-ttu-id="2c6af-127">**Klasické nastavení sítě VNet**</span><span class="sxs-lookup"><span data-stu-id="2c6af-127">**Classic VNet settings**</span></span>

<span data-ttu-id="2c6af-128">Název virtuální sítě = ClassicVNet</span><span class="sxs-lookup"><span data-stu-id="2c6af-128">VNet Name = ClassicVNet</span></span> <br>
<span data-ttu-id="2c6af-129">Umístění = západní USA</span><span class="sxs-lookup"><span data-stu-id="2c6af-129">Location = West US</span></span> <br>
<span data-ttu-id="2c6af-130">Adresní prostory virtuální sítě = 10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="2c6af-130">Virtual Network Address Spaces = 10.0.0.0/24</span></span> <br>
<span data-ttu-id="2c6af-131">Podsíť 1 = 10.0.0.0/27</span><span class="sxs-lookup"><span data-stu-id="2c6af-131">Subnet-1 = 10.0.0.0/27</span></span> <br>
<span data-ttu-id="2c6af-132">GatewaySubnet = 10.0.0.32/29</span><span class="sxs-lookup"><span data-stu-id="2c6af-132">GatewaySubnet = 10.0.0.32/29</span></span> <br>
<span data-ttu-id="2c6af-133">Název místní sítě = RMVNetLocal</span><span class="sxs-lookup"><span data-stu-id="2c6af-133">Local Network Name = RMVNetLocal</span></span> <br>
<span data-ttu-id="2c6af-134">GatewayType = DynamicRouting</span><span class="sxs-lookup"><span data-stu-id="2c6af-134">GatewayType = DynamicRouting</span></span>

<span data-ttu-id="2c6af-135">**Nastavení virtuální sítě Resource Manageru**</span><span class="sxs-lookup"><span data-stu-id="2c6af-135">**Resource Manager VNet settings**</span></span>

<span data-ttu-id="2c6af-136">Název virtuální sítě = RMVNet</span><span class="sxs-lookup"><span data-stu-id="2c6af-136">VNet Name = RMVNet</span></span> <br>
<span data-ttu-id="2c6af-137">Skupina prostředků = RG1</span><span class="sxs-lookup"><span data-stu-id="2c6af-137">Resource Group = RG1</span></span> <br>
<span data-ttu-id="2c6af-138">Virtuální síť adresní prostory IP adres = 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="2c6af-138">Virtual Network IP Address Spaces = 192.168.0.0/16</span></span> <br>
<span data-ttu-id="2c6af-139">Podsíť 1 = 192.168.1.0/24</span><span class="sxs-lookup"><span data-stu-id="2c6af-139">Subnet-1 = 192.168.1.0/24</span></span> <br>
<span data-ttu-id="2c6af-140">GatewaySubnet = 192.168.0.0/26</span><span class="sxs-lookup"><span data-stu-id="2c6af-140">GatewaySubnet = 192.168.0.0/26</span></span> <br>
<span data-ttu-id="2c6af-141">Umístění = východní USA</span><span class="sxs-lookup"><span data-stu-id="2c6af-141">Location = East US</span></span> <br>
<span data-ttu-id="2c6af-142">Název veřejné IP adresy brány = gwpip</span><span class="sxs-lookup"><span data-stu-id="2c6af-142">Gateway public IP name = gwpip</span></span> <br>
<span data-ttu-id="2c6af-143">Brána místní sítě = ClassicVNetLocal</span><span class="sxs-lookup"><span data-stu-id="2c6af-143">Local Network Gateway = ClassicVNetLocal</span></span> <br>
<span data-ttu-id="2c6af-144">Název virtuální síťová brána = RMGateway</span><span class="sxs-lookup"><span data-stu-id="2c6af-144">Virtual Network Gateway name = RMGateway</span></span> <br>
<span data-ttu-id="2c6af-145">Konfigurace adresování IP brány = gwipconfig</span><span class="sxs-lookup"><span data-stu-id="2c6af-145">Gateway IP addressing configuration = gwipconfig</span></span>

## <span data-ttu-id="2c6af-146"><a name="createsmgw"></a>Oddíl 1 – konfigurace klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="2c6af-146"><a name="createsmgw"></a>Section 1 - Configure the classic VNet</span></span>
### <a name="part-1---download-your-network-configuration-file"></a><span data-ttu-id="2c6af-147">Část 1 – stažení souboru konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="2c6af-147">Part 1 - Download your network configuration file</span></span>
1. <span data-ttu-id="2c6af-148">Přihlaste se k účtu Azure v konzole PowerShell se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="2c6af-148">Log in to your Azure account in the PowerShell console with elevated rights.</span></span> <span data-ttu-id="2c6af-149">Následující rutiny zobrazí výzvu pro přihlašovací údaje pro účet Azure.</span><span class="sxs-lookup"><span data-stu-id="2c6af-149">The following cmdlet prompts you for the login credentials for your Azure Account.</span></span> <span data-ttu-id="2c6af-150">Po přihlášení se stáhne nastavení účtu, aby bylo dostupné v prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c6af-150">After logging in, it downloads your account settings so that they are available to Azure PowerShell.</span></span> <span data-ttu-id="2c6af-151">Dokončete tuto část konfigurace můžete pomocí rutin prostředí PowerShell SM.</span><span class="sxs-lookup"><span data-stu-id="2c6af-151">You use the SM PowerShell cmdlets to complete this part of the configuration.</span></span>

  ```powershell
  Add-AzureAccount
  ```
2. <span data-ttu-id="2c6af-152">Spuštěním následujícího příkazu exportujte konfiguračního souboru sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="2c6af-152">Export your Azure network configuration file by running the following command.</span></span> <span data-ttu-id="2c6af-153">Můžete změnit umístění souboru pro export do jiného umístění v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="2c6af-153">You can change the location of the file to export to a different location if necessary.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. <span data-ttu-id="2c6af-154">Otevřete soubor .xml, který jste stáhli, pokud ho Pokud chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="2c6af-154">Open the .xml file that you downloaded to edit it.</span></span> <span data-ttu-id="2c6af-155">Příklad konfiguračního souboru sítě, naleznete v části [schéma konfigurace sítě](https://msdn.microsoft.com/library/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="2c6af-155">For an example of the network configuration file, see the [Network Configuration Schema](https://msdn.microsoft.com/library/jj157100.aspx).</span></span>

### <a name="part-2--verify-the-gateway-subnet"></a><span data-ttu-id="2c6af-156">Část 2 - Ověřte podsíť brány</span><span class="sxs-lookup"><span data-stu-id="2c6af-156">Part 2 -Verify the gateway subnet</span></span>
<span data-ttu-id="2c6af-157">V **VirtualNetworkSites** elementu přidat podsíť brány k virtuální síti, pokud dosud nebyla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="2c6af-157">In the **VirtualNetworkSites** element, add a gateway subnet to your VNet if one has not already been created.</span></span> <span data-ttu-id="2c6af-158">Při práci s konfiguračního souboru sítě, podsítě brány musí mít název "GatewaySubnet" nebo Azure nelze rozpoznat a používejte ho jako podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="2c6af-158">When working with the network configuration file, the gateway subnet MUST be named "GatewaySubnet" or Azure cannot recognize and use it as a gateway subnet.</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

<span data-ttu-id="2c6af-159">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="2c6af-159">**Example:**</span></span>

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="part-3---add-the-local-network-site"></a><span data-ttu-id="2c6af-160">Část 3 – přidání na místní síťový Web</span><span class="sxs-lookup"><span data-stu-id="2c6af-160">Part 3 - Add the local network site</span></span>
<span data-ttu-id="2c6af-161">Na místní síťový web, které přidáte představuje RM virtuální síť, ke kterému se chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="2c6af-161">The local network site you add represents the RM VNet to which you want to connect.</span></span> <span data-ttu-id="2c6af-162">Přidat **LocalNetworkSites** element do souboru, pokud dosud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2c6af-162">Add a **LocalNetworkSites** element to the file if one doesn't already exist.</span></span> <span data-ttu-id="2c6af-163">V tomto okamžiku v konfiguraci prvek VPNGatewayAddress může být jakékoli platná veřejná IP adresa protože jsme ještě nevytvořili brány pro virtuální sítě Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="2c6af-163">At this point in the configuration, the VPNGatewayAddress can be any valid public IP address because we haven't yet created the gateway for the Resource Manager VNet.</span></span> <span data-ttu-id="2c6af-164">Jakmile se nám vytvořit bránu, jsme nahraďte tuto IP adresu zástupný symbol správné veřejnou IP adresu, která byla přiřazena k bráně RM.</span><span class="sxs-lookup"><span data-stu-id="2c6af-164">Once we create the gateway, we replace this placeholder IP address with the correct public IP address that has been assigned to the RM gateway.</span></span>

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a><span data-ttu-id="2c6af-165">Součástí 4 – přidružit na místní síťový web virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="2c6af-165">Part 4 - Associate the VNet with the local network site</span></span>
<span data-ttu-id="2c6af-166">V této části určíme na místní síťový web, který chcete připojit síť VNet.</span><span class="sxs-lookup"><span data-stu-id="2c6af-166">In this section, we specify the local network site that you want to connect the VNet to.</span></span> <span data-ttu-id="2c6af-167">V takovém případě je virtuální sítě Resource Manageru, který jste dříve odkazuje.</span><span class="sxs-lookup"><span data-stu-id="2c6af-167">In this case, it is the Resource Manager VNet that you referenced earlier.</span></span> <span data-ttu-id="2c6af-168">Ujistěte se, že názvy shodovat.</span><span class="sxs-lookup"><span data-stu-id="2c6af-168">Make sure the names match.</span></span> <span data-ttu-id="2c6af-169">Tento krok není vytvořit bránu.</span><span class="sxs-lookup"><span data-stu-id="2c6af-169">This step does not create a gateway.</span></span> <span data-ttu-id="2c6af-170">Určuje místní sítě, kterému se bude připojovat brány.</span><span class="sxs-lookup"><span data-stu-id="2c6af-170">It specifies the local network that the gateway will connect to.</span></span>

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a><span data-ttu-id="2c6af-171">Část 5 – soubor uložte a nahrajte</span><span class="sxs-lookup"><span data-stu-id="2c6af-171">Part 5 - Save the file and upload</span></span>
<span data-ttu-id="2c6af-172">Uložte soubor a potom ho importovat do Azure tak, že spustíte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="2c6af-172">Save the file, then import it to Azure by running the following command.</span></span> <span data-ttu-id="2c6af-173">Ujistěte se, že změníte cestu k souboru v případě potřeby pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="2c6af-173">Make sure you change the file path as necessary for your environment.</span></span>

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="2c6af-174">Zobrazí se podobné výsledek zobrazující, že import proběhlo úspěšně.</span><span class="sxs-lookup"><span data-stu-id="2c6af-174">You will see a similar result showing that the import succeeded.</span></span>

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a><span data-ttu-id="2c6af-175">Část 6 – Vytvoření brány</span><span class="sxs-lookup"><span data-stu-id="2c6af-175">Part 6 - Create the gateway</span></span>

<span data-ttu-id="2c6af-176">Před spuštěním tohoto příkladu, najdete v do konfiguračního souboru sítě, který jste stáhli pro přesnou názvy, které Azure očekává, že v tématu.</span><span class="sxs-lookup"><span data-stu-id="2c6af-176">Before running this example, refer to the network configuration file that you downloaded for the exact names that Azure expects to see.</span></span> <span data-ttu-id="2c6af-177">Konfigurační soubor sítě obsahuje hodnoty pro klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2c6af-177">The network configuration file contains the values for your classic virtual networks.</span></span> <span data-ttu-id="2c6af-178">Názvy pro virtuální sítě classic se někdy při vytváření classic nastavení sítě VNet na portálu Azure kvůli rozdílům v modelech nasazení změní v konfiguračním souboru na síti.</span><span class="sxs-lookup"><span data-stu-id="2c6af-178">Sometimes the names for classic VNets are changed in the network configuration file when creating classic VNet settings in the Azure portal due to the differences in the deployment models.</span></span> <span data-ttu-id="2c6af-179">Například pokud jste použili portál Azure k vytvoření klasické virtuální sítě s názvem klasické virtuální sítě a vytvořit ve skupině prostředků s názvem 'ClassicRG', název, který je obsažen v konfiguračním souboru sítě jsou převedeny na "Skupiny ClassicRG klasické virtuální sítě".</span><span class="sxs-lookup"><span data-stu-id="2c6af-179">For example, if you used the Azure portal to create a classic VNet named 'Classic VNet' and created it in a resource group named 'ClassicRG', the name that is contained in the network configuration file is converted to 'Group ClassicRG Classic VNet'.</span></span> <span data-ttu-id="2c6af-180">Při zadávání názvu virtuální sítě, který obsahuje mezery, použijte hodnotu do uvozovek.</span><span class="sxs-lookup"><span data-stu-id="2c6af-180">When specifying the name of a VNet that contains spaces, use quotation marks around the value.</span></span>


<span data-ttu-id="2c6af-181">Použijte následující příklad k vytvoření brány dynamického směrování:</span><span class="sxs-lookup"><span data-stu-id="2c6af-181">Use the following example to create a dynamic routing gateway:</span></span>

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

<span data-ttu-id="2c6af-182">Stav brány, můžete zkontrolovat pomocí **Get-AzureVNetGateway** rutiny.</span><span class="sxs-lookup"><span data-stu-id="2c6af-182">You can check the status of the gateway by using the **Get-AzureVNetGateway** cmdlet.</span></span>

## <span data-ttu-id="2c6af-183"><a name="creatermgw"></a>Část 2: Konfigurace brány virtuální sítě RM</span><span class="sxs-lookup"><span data-stu-id="2c6af-183"><a name="creatermgw"></a>Section 2: Configure the RM VNet gateway</span></span>
<span data-ttu-id="2c6af-184">Pokud chcete vytvořit bránu VPN pro virtuální síť RM, postupujte podle následujících pokynů.</span><span class="sxs-lookup"><span data-stu-id="2c6af-184">To create a VPN gateway for the RM VNet, follow the following instructions.</span></span> <span data-ttu-id="2c6af-185">Nespouštějte kroky až po načtení veřejnou IP adresu brány, klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2c6af-185">Don't start the steps until after you have retrieved the public IP address for the classic VNet's gateway.</span></span> 

1. <span data-ttu-id="2c6af-186">Přihlaste se k účtu Azure v konzole PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c6af-186">Log in to your Azure account in the PowerShell console.</span></span> <span data-ttu-id="2c6af-187">Následující rutiny zobrazí výzvu pro přihlašovací údaje pro účet Azure.</span><span class="sxs-lookup"><span data-stu-id="2c6af-187">The following cmdlet prompts you for the login credentials for your Azure Account.</span></span> <span data-ttu-id="2c6af-188">Po přihlášení, se stáhnou nastavení svého účtu, aby byly k dispozici pro prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c6af-188">After logging in, your account settings are downloaded so that they are available to Azure PowerShell.</span></span>

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  <span data-ttu-id="2c6af-189">Získání seznamu předplatné Azure, pokud máte více než jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="2c6af-189">Get a list of your Azure subscriptions if you have more than one subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```
   
  <span data-ttu-id="2c6af-190">Určete předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="2c6af-190">Specify the subscription that you want to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. <span data-ttu-id="2c6af-191">Vytvoření brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="2c6af-191">Create a local network gateway.</span></span> <span data-ttu-id="2c6af-192">Ve virtuální síti brána místní sítě obvykle odkazuje na vaše místní umístění.</span><span class="sxs-lookup"><span data-stu-id="2c6af-192">In a virtual network, the local network gateway typically refers to your on-premises location.</span></span> <span data-ttu-id="2c6af-193">V takovém případě místní síťová brána odkazuje na klasické virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="2c6af-193">In this case, the local network gateway refers to your Classic VNet.</span></span> <span data-ttu-id="2c6af-194">Zadejte jeho název, pomocí kterého můžete Azure na ni odkazuje a také zadat předponu adresního prostoru.</span><span class="sxs-lookup"><span data-stu-id="2c6af-194">Give it a name by which Azure can refer to it, and also specify the address space prefix.</span></span> <span data-ttu-id="2c6af-195">Azure pomocí zadané předpony IP adresy rozpozná, jaký provoz má zasílat na vaše místní umístění.</span><span class="sxs-lookup"><span data-stu-id="2c6af-195">Azure uses the IP address prefix you specify to identify which traffic to send to your on-premises location.</span></span> <span data-ttu-id="2c6af-196">Pokud potřebujete upravit informace sem později, před vytvořením brány, můžete upravit hodnoty a znovu spusťte vzorku.</span><span class="sxs-lookup"><span data-stu-id="2c6af-196">If you need to adjust the information here later, before creating your gateway, you can modify the values and run the sample again.</span></span>
   
   <span data-ttu-id="2c6af-197">**-Název** je název, kterou chcete přiřadit k odkazování na bránu místní sítě.</span><span class="sxs-lookup"><span data-stu-id="2c6af-197">**-Name** is the name you want to assign to refer to the local network gateway.</span></span><br><span data-ttu-id="2c6af-198">
   **-AddressPrefix** adresní prostor je pro klasické virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="2c6af-198">
   **-AddressPrefix** is the Address Space for your classic VNet.</span></span><br><span data-ttu-id="2c6af-199">
**-GatewayIpAddress** je veřejná IP adresa brány klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2c6af-199">
**-GatewayIpAddress** is the public IP address of the classic VNet's gateway.</span></span> <span data-ttu-id="2c6af-200">Nezapomeňte změnit následující ukázka tak, aby odrážela správnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2c6af-200">Be sure to change the following sample to reflect the correct IP address.</span></span><br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. <span data-ttu-id="2c6af-201">Požádat o veřejnou IP adresu, která bude přidělena pro bránu virtuální sítě pro virtuální sítě Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="2c6af-201">Request a public IP address to be allocated to the virtual network gateway for the Resource Manager VNet.</span></span> <span data-ttu-id="2c6af-202">Nelze zadat IP adresu, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="2c6af-202">You can't specify the IP address that you want to use.</span></span> <span data-ttu-id="2c6af-203">IP adresa se pro bránu virtuální sítě přidělí dynamicky.</span><span class="sxs-lookup"><span data-stu-id="2c6af-203">The IP address is dynamically allocated to the virtual network gateway.</span></span> <span data-ttu-id="2c6af-204">To ale neznamená, že se IP adresa změní.</span><span class="sxs-lookup"><span data-stu-id="2c6af-204">However, this does not mean the IP address changes.</span></span> <span data-ttu-id="2c6af-205">Změna IP adresy brány virtuální sítě je pouze při brány je odstraněno a znovu vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="2c6af-205">The only time the virtual network gateway IP address changes is when the gateway is deleted and recreated.</span></span> <span data-ttu-id="2c6af-206">Nezmění, v rámci změny velikosti, resetování nebo jiné operace údržby/upgradu brány.</span><span class="sxs-lookup"><span data-stu-id="2c6af-206">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of the gateway.</span></span>

  <span data-ttu-id="2c6af-207">V tomto kroku jsme také nastavit proměnné, která se používá v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="2c6af-207">In this step, we also set a variable that is used in a later step.</span></span>

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. <span data-ttu-id="2c6af-208">Ověřte, že virtuální sítě má podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="2c6af-208">Verify that your virtual network has a gateway subnet.</span></span> <span data-ttu-id="2c6af-209">Pokud neexistuje žádná podsíť brány, přidáte.</span><span class="sxs-lookup"><span data-stu-id="2c6af-209">If no gateway subnet exists, add one.</span></span> <span data-ttu-id="2c6af-210">Zajistěte, aby podsíť brány je s názvem *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="2c6af-210">Make sure the gateway subnet is named *GatewaySubnet*.</span></span>
5. <span data-ttu-id="2c6af-211">Načtěte podsítě používané pro bránu spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="2c6af-211">Retrieve the subnet used for the gateway by running the following command.</span></span> <span data-ttu-id="2c6af-212">V tomto kroku jsme také nastavit proměnnou, která se použije v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="2c6af-212">In this step, we also set a variable to be used in the next step.</span></span>
   
   <span data-ttu-id="2c6af-213">**-Název** je název virtuální sítě Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2c6af-213">**-Name** is the name of your Resource Manager VNet.</span></span><br><span data-ttu-id="2c6af-214">
   **-ResourceGroupName** je skupina prostředků, který je přidružený virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2c6af-214">
**-ResourceGroupName** is the resource group that the VNet is associated with.</span></span> <span data-ttu-id="2c6af-215">Podsíť brány musí již existovat pro tuto virtuální síť a musí mít název *GatewaySubnet* správně fungovat.</span><span class="sxs-lookup"><span data-stu-id="2c6af-215">The gateway subnet must already exist for this VNet and must be named *GatewaySubnet* to work properly.</span></span><br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. <span data-ttu-id="2c6af-216">Vytvoření konfigurace adresování IP brány.</span><span class="sxs-lookup"><span data-stu-id="2c6af-216">Create the gateway IP addressing configuration.</span></span> <span data-ttu-id="2c6af-217">Konfigurace brány definuje podsíť a veřejnou IP adresu, která se bude používat.</span><span class="sxs-lookup"><span data-stu-id="2c6af-217">The gateway configuration defines the subnet and the public IP address to use.</span></span> <span data-ttu-id="2c6af-218">Podle následující ukázky vytvořte vlastní konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="2c6af-218">Use the following sample to create your gateway configuration.</span></span>

  <span data-ttu-id="2c6af-219">V tomto kroku **- SubnetId** a **- PublicIpAddressId** parametry musí být předán vlastnost id z podsíť a IP adresu objekty, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2c6af-219">In this step, the **-SubnetId** and **-PublicIpAddressId** parameters must be passed the id property from the subnet, and IP address objects, respectively.</span></span> <span data-ttu-id="2c6af-220">Nelze použít jednoduchý řetězec.</span><span class="sxs-lookup"><span data-stu-id="2c6af-220">You can't use a simple string.</span></span> <span data-ttu-id="2c6af-221">Tyto proměnné jsou nastavené v kroku k vyžádání veřejné IP adresy a kroku k načtení podsíť.</span><span class="sxs-lookup"><span data-stu-id="2c6af-221">These variables are set in the step to request a public IP and the step to retrieve the subnet.</span></span>

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. <span data-ttu-id="2c6af-222">Spuštěním následujícího příkazu vytvořte bránu virtuální sítě Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2c6af-222">Create the Resource Manager virtual network gateway by running the following command.</span></span> <span data-ttu-id="2c6af-223">`-VpnType` Musí být *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="2c6af-223">The `-VpnType` must be *RouteBased*.</span></span> <span data-ttu-id="2c6af-224">45 minut nebo déle pro vytvoření brány může trvat.</span><span class="sxs-lookup"><span data-stu-id="2c6af-224">It can take 45 minutes or more for the gateway to create.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. <span data-ttu-id="2c6af-225">Po vytvoření brány sítě VPN, zkopírujte veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2c6af-225">Copy the public IP address once the VPN gateway has been created.</span></span> <span data-ttu-id="2c6af-226">Použijete jej při konfiguraci nastavení místní sítě pro vaše klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2c6af-226">You use it when you configure the local network settings for your Classic VNet.</span></span> <span data-ttu-id="2c6af-227">Následující rutiny můžete získat veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2c6af-227">You can use the following cmdlet to retrieve the public IP address.</span></span> <span data-ttu-id="2c6af-228">Veřejná IP adresa je uvedena v návratový jako *IpAddress*.</span><span class="sxs-lookup"><span data-stu-id="2c6af-228">The public IP address is listed in the return as *IpAddress*.</span></span>

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="section-3-modify-the-classic-vnet-local-site-settings"></a><span data-ttu-id="2c6af-229">Část 3: Úprava classic místní lokality nastavení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="2c6af-229">Section 3: Modify the classic VNet local site settings</span></span>

<span data-ttu-id="2c6af-230">V této části můžete pracovat s klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2c6af-230">In this section, you work with the classic VNet.</span></span> <span data-ttu-id="2c6af-231">Nahraďte zástupný symbol IP adresu, kterou jste použili při zadávání nastavení místní sítě, které se použijí pro připojení k bráně virtuální sítě Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="2c6af-231">You replace the placeholder IP address that you used when specifying the local site settings that will be used to connect to the Resource Manager VNet gateway.</span></span> 

1. <span data-ttu-id="2c6af-232">Exportujte konfigurační soubor sítě.</span><span class="sxs-lookup"><span data-stu-id="2c6af-232">Export the network configuration file.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. <span data-ttu-id="2c6af-233">Pomocí textového editoru, změňte hodnotu pro prvek VPNGatewayAddress.</span><span class="sxs-lookup"><span data-stu-id="2c6af-233">Using a text editor, modify the value for VPNGatewayAddress.</span></span> <span data-ttu-id="2c6af-234">Nahraďte zástupný symbol IP adresu s veřejnou IP adresu brány, Resource Manager a potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="2c6af-234">Replace the placeholder IP address with the public IP address of the Resource Manager gateway and then save the changes.</span></span>

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. <span data-ttu-id="2c6af-235">Importujte konfiguračního souboru upravené sítě do Azure.</span><span class="sxs-lookup"><span data-stu-id="2c6af-235">Import the modified network configuration file to Azure.</span></span>

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <span data-ttu-id="2c6af-236"><a name="connect"></a>Oddíl 4: Vytvoření připojení mezi bránami</span><span class="sxs-lookup"><span data-stu-id="2c6af-236"><a name="connect"></a>Section 4: Create a connection between the gateways</span></span>
<span data-ttu-id="2c6af-237">Vytvoření připojení mezi bránami vyžaduje rozhraní PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c6af-237">Creating a connection between the gateways requires PowerShell.</span></span> <span data-ttu-id="2c6af-238">Musíte přidat váš účet Azure použít klasickou verzi rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c6af-238">You may need to add your Azure Account to use the classic version of the  PowerShell cmdlets.</span></span> <span data-ttu-id="2c6af-239">Chcete-li to provést, použijte **Add-AzureAccount**.</span><span class="sxs-lookup"><span data-stu-id="2c6af-239">To do so, use **Add-AzureAccount**.</span></span>

1. <span data-ttu-id="2c6af-240">V konzole PowerShell nastavte sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="2c6af-240">In the PowerShell console, set your shared key.</span></span> <span data-ttu-id="2c6af-241">Před spuštěním rutin, najdete v do konfiguračního souboru sítě, který jste stáhli pro přesnou názvy, které Azure očekává, že v tématu.</span><span class="sxs-lookup"><span data-stu-id="2c6af-241">Before running the cmdlets, refer to the network configuration file that you downloaded for the exact names that Azure expects to see.</span></span> <span data-ttu-id="2c6af-242">Při zadávání názvu virtuální sítě, který obsahuje mezery, použijte jednoduché uvozovky hodnota.</span><span class="sxs-lookup"><span data-stu-id="2c6af-242">When specifying the name of a VNet that contains spaces, use single quotation marks around the value.</span></span><br><br><span data-ttu-id="2c6af-243">V následujícím příkladu **- VNetName** je název klasické virtuální sítě a **- LocalNetworkSiteName** je název zadaný pro místní síťový Web.</span><span class="sxs-lookup"><span data-stu-id="2c6af-243">In following example, **-VNetName** is the name of the classic VNet and **-LocalNetworkSiteName** is the name you specified for the local network site.</span></span> <span data-ttu-id="2c6af-244">**- SharedKey** je hodnota, která můžete vygenerovat a zadat.</span><span class="sxs-lookup"><span data-stu-id="2c6af-244">The **-SharedKey** is a value that you generate and specify.</span></span> <span data-ttu-id="2c6af-245">V příkladu jsme použili 'abc123', ale můžete vygenerovat a použít něco složitější.</span><span class="sxs-lookup"><span data-stu-id="2c6af-245">In the example, we used 'abc123', but you can generate and use something more complex.</span></span> <span data-ttu-id="2c6af-246">Důležité je, že hodnota, kterou tady zadáte, musí být stejnou hodnotou, kterou zadáte v dalším kroku, při vytváření připojení.</span><span class="sxs-lookup"><span data-stu-id="2c6af-246">The important thing is that the value you specify here must be the same value that you specify in the next step when you create your connection.</span></span> <span data-ttu-id="2c6af-247">Návratový by měl zobrazit **stav: úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="2c6af-247">The return should show **Status: Successful**.</span></span>

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. <span data-ttu-id="2c6af-248">Vytvořte připojení k síti VPN spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="2c6af-248">Create the VPN connection by running the following commands:</span></span>
   
  <span data-ttu-id="2c6af-249">Nastavte proměnné.</span><span class="sxs-lookup"><span data-stu-id="2c6af-249">Set the variables.</span></span>

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  <span data-ttu-id="2c6af-250">Vytvořte připojení.</span><span class="sxs-lookup"><span data-stu-id="2c6af-250">Create the connection.</span></span> <span data-ttu-id="2c6af-251">Všimněte si, že **- ConnectionType** je protokol IPsec, není Vnet2Vnet.</span><span class="sxs-lookup"><span data-stu-id="2c6af-251">Notice that the **-ConnectionType** is IPsec, not Vnet2Vnet.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="section-5-verify-your-connections"></a><span data-ttu-id="2c6af-252">Část 5: Ověření připojení</span><span class="sxs-lookup"><span data-stu-id="2c6af-252">Section 5: Verify your connections</span></span>

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a><span data-ttu-id="2c6af-253">Chcete-li ověřit připojení z vaší klasické virtuální sítě k virtuální síti Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2c6af-253">To verify the connection from your classic VNet to your Resource Manager VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="2c6af-254">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c6af-254">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="2c6af-255">portál Azure</span><span class="sxs-lookup"><span data-stu-id="2c6af-255">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a><span data-ttu-id="2c6af-256">Chcete-li ověřit připojení z vaší virtuální sítě Resource Manager k vaší klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="2c6af-256">To verify the connection from your Resource Manager VNet to your classic VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="2c6af-257">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c6af-257">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="2c6af-258">portál Azure</span><span class="sxs-lookup"><span data-stu-id="2c6af-258">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <span data-ttu-id="2c6af-259"><a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="2c6af-259"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

