---
title: "Připojení klasické virtuální sítě tooAzure virtuální sítě Resource Manager: prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate připojení VPN mezi klasické virtuální sítě a virtuální sítě Resource Manager pomocí brány sítě VPN a prostředí PowerShell"
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
ms.openlocfilehash: 8b1cf6ae4becf1829fa99961c5dd09a422fcc1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a><span data-ttu-id="d5e7b-103">Připojení virtuálních sítí z různých modelů nasazení pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="d5e7b-103">Connect virtual networks from different deployment models using PowerShell</span></span>



<span data-ttu-id="d5e7b-104">Tento článek ukazuje, jak tooconnect klasické virtuální sítě tooResource Správce virtuálních sítí tooallow hello prostředků umístěných v hello samostatné nasazení modely toocommunicate mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-104">This article shows you how tooconnect classic VNets tooResource Manager VNets tooallow hello resources located in hello separate deployment models toocommunicate with each other.</span></span> <span data-ttu-id="d5e7b-105">Hello kroky v tomto článku pomocí prostředí PowerShell, ale můžete také vytvořit této konfigurace pomocí portálu Azure hello výběrem hello článek z tohoto seznamu.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-105">hello steps in this article use PowerShell, but you can also create this configuration using hello Azure portal by selecting hello article from this list.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d5e7b-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d5e7b-106">Portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="d5e7b-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d5e7b-107">PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

<span data-ttu-id="d5e7b-108">Připojení klasické virtuální sítě tooa virtuální sítě Resource Manageru je podobné tooconnecting umístění lokality tooan místní virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-108">Connecting a classic VNet tooa Resource Manager VNet is similar tooconnecting a VNet tooan on-premises site location.</span></span> <span data-ttu-id="d5e7b-109">Oba typy připojení využívají bránu tooprovide sítě VPN přes zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-109">Both connectivity types use a VPN gateway tooprovide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="d5e7b-110">Můžete vytvořit připojení mezi virtuální sítě, které jsou v různých předplatných a v různých oblastech.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-110">You can create a connection between VNets that are in different subscriptions and in different regions.</span></span> <span data-ttu-id="d5e7b-111">Virtuální sítě, které už máte připojení tooon místní sítě, můžete také připojit, pokud je hello brány, které byly nakonfigurovány k dynamické nebo založené na trasách.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-111">You can also connect VNets that already have connections tooon-premises networks, as long as hello gateway that they have been configured with is dynamic or route-based.</span></span> <span data-ttu-id="d5e7b-112">Další informace o připojení VNet-to-VNet, najdete v části hello [nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-112">For more information about VNet-to-VNet connections, see hello [VNet-to-VNet FAQ](#faq) at hello end of this article.</span></span> 

<span data-ttu-id="d5e7b-113">Pokud vaše virtuální sítě jsou v hello stejné oblasti, může být vhodné tooinstead zvažte připojení pomocí virtuální sítě partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-113">If your VNets are in hello same region, you may want tooinstead consider connecting them using VNet Peering.</span></span> <span data-ttu-id="d5e7b-114">Partnerské vztahy virtuálních sítí nepoužívají bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-114">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="d5e7b-115">Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5e7b-115">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> 

## <a name="before-beginning"></a><span data-ttu-id="d5e7b-116">Před zahájením</span><span class="sxs-lookup"><span data-stu-id="d5e7b-116">Before beginning</span></span>

<span data-ttu-id="d5e7b-117">Hello následující postup vás provede procesem hello nastavení potřebné tooconfigure bránu dynamické nebo založené na směrování pro každou virtuální síť a vytvoření připojení VPN mezi bránami hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-117">hello following steps walk you through hello settings necessary tooconfigure a dynamic or route-based gateway for each VNet and create a VPN connection between hello gateways.</span></span> <span data-ttu-id="d5e7b-118">Tato konfigurace nepodporuje statickou nebo na základě zásad brány.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-118">This configuration does not support static or policy-based gateways.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="d5e7b-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d5e7b-119">Prerequisites</span></span>

* <span data-ttu-id="d5e7b-120">Již byly vytvořeny obě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-120">Both VNets have already been created.</span></span>
* <span data-ttu-id="d5e7b-121">Hello rozsahy adres pro hello virtuální sítě není navzájem překrývají, nebo překrývat s žádným z rozsahů hello pro další připojení, které hello brány může být připojen k.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-121">hello address ranges for hello VNets do not overlap with each other, or overlap with any of hello ranges for other connections that hello gateways may be connected to.</span></span>
* <span data-ttu-id="d5e7b-122">Nainstalovali jste nejnovější rutiny prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-122">You have installed hello latest PowerShell cmdlets.</span></span> <span data-ttu-id="d5e7b-123">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-123">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span> <span data-ttu-id="d5e7b-124">Ujistěte se, že instalujete hello služby správy (SM) a hello rutiny Resource Manager (RM).</span><span class="sxs-lookup"><span data-stu-id="d5e7b-124">Make sure you install both hello Service Management (SM) and hello Resource Manager (RM) cmdlets.</span></span> 

### <span data-ttu-id="d5e7b-125"><a name="exampleref"></a>Příklady nastavení</span><span class="sxs-lookup"><span data-stu-id="d5e7b-125"><a name="exampleref"></a>Example settings</span></span>

<span data-ttu-id="d5e7b-126">Můžete použít tyto hodnoty toocreate testovací prostředí nebo odkazovat toothem toobetter pochopit hello příklady v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-126">You can use these values toocreate a test environment, or refer toothem toobetter understand hello examples in this article.</span></span>

<span data-ttu-id="d5e7b-127">**Klasické nastavení sítě VNet**</span><span class="sxs-lookup"><span data-stu-id="d5e7b-127">**Classic VNet settings**</span></span>

<span data-ttu-id="d5e7b-128">Název virtuální sítě = ClassicVNet</span><span class="sxs-lookup"><span data-stu-id="d5e7b-128">VNet Name = ClassicVNet</span></span> <br>
<span data-ttu-id="d5e7b-129">Umístění = západní USA</span><span class="sxs-lookup"><span data-stu-id="d5e7b-129">Location = West US</span></span> <br>
<span data-ttu-id="d5e7b-130">Adresní prostory virtuální sítě = 10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="d5e7b-130">Virtual Network Address Spaces = 10.0.0.0/24</span></span> <br>
<span data-ttu-id="d5e7b-131">Podsíť 1 = 10.0.0.0/27</span><span class="sxs-lookup"><span data-stu-id="d5e7b-131">Subnet-1 = 10.0.0.0/27</span></span> <br>
<span data-ttu-id="d5e7b-132">GatewaySubnet = 10.0.0.32/29</span><span class="sxs-lookup"><span data-stu-id="d5e7b-132">GatewaySubnet = 10.0.0.32/29</span></span> <br>
<span data-ttu-id="d5e7b-133">Název místní sítě = RMVNetLocal</span><span class="sxs-lookup"><span data-stu-id="d5e7b-133">Local Network Name = RMVNetLocal</span></span> <br>
<span data-ttu-id="d5e7b-134">GatewayType = DynamicRouting</span><span class="sxs-lookup"><span data-stu-id="d5e7b-134">GatewayType = DynamicRouting</span></span>

<span data-ttu-id="d5e7b-135">**Nastavení virtuální sítě Resource Manageru**</span><span class="sxs-lookup"><span data-stu-id="d5e7b-135">**Resource Manager VNet settings**</span></span>

<span data-ttu-id="d5e7b-136">Název virtuální sítě = RMVNet</span><span class="sxs-lookup"><span data-stu-id="d5e7b-136">VNet Name = RMVNet</span></span> <br>
<span data-ttu-id="d5e7b-137">Skupina prostředků = RG1</span><span class="sxs-lookup"><span data-stu-id="d5e7b-137">Resource Group = RG1</span></span> <br>
<span data-ttu-id="d5e7b-138">Virtuální síť adresní prostory IP adres = 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="d5e7b-138">Virtual Network IP Address Spaces = 192.168.0.0/16</span></span> <br>
<span data-ttu-id="d5e7b-139">Podsíť 1 = 192.168.1.0/24</span><span class="sxs-lookup"><span data-stu-id="d5e7b-139">Subnet-1 = 192.168.1.0/24</span></span> <br>
<span data-ttu-id="d5e7b-140">GatewaySubnet = 192.168.0.0/26</span><span class="sxs-lookup"><span data-stu-id="d5e7b-140">GatewaySubnet = 192.168.0.0/26</span></span> <br>
<span data-ttu-id="d5e7b-141">Umístění = východní USA</span><span class="sxs-lookup"><span data-stu-id="d5e7b-141">Location = East US</span></span> <br>
<span data-ttu-id="d5e7b-142">Název veřejné IP adresy brány = gwpip</span><span class="sxs-lookup"><span data-stu-id="d5e7b-142">Gateway public IP name = gwpip</span></span> <br>
<span data-ttu-id="d5e7b-143">Brána místní sítě = ClassicVNetLocal</span><span class="sxs-lookup"><span data-stu-id="d5e7b-143">Local Network Gateway = ClassicVNetLocal</span></span> <br>
<span data-ttu-id="d5e7b-144">Název virtuální síťová brána = RMGateway</span><span class="sxs-lookup"><span data-stu-id="d5e7b-144">Virtual Network Gateway name = RMGateway</span></span> <br>
<span data-ttu-id="d5e7b-145">Konfigurace adresování IP brány = gwipconfig</span><span class="sxs-lookup"><span data-stu-id="d5e7b-145">Gateway IP addressing configuration = gwipconfig</span></span>

## <span data-ttu-id="d5e7b-146"><a name="createsmgw"></a>Oddíl 1 – konfigurace hello klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="d5e7b-146"><a name="createsmgw"></a>Section 1 - Configure hello classic VNet</span></span>
### <a name="part-1---download-your-network-configuration-file"></a><span data-ttu-id="d5e7b-147">Část 1 – stažení souboru konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="d5e7b-147">Part 1 - Download your network configuration file</span></span>
1. <span data-ttu-id="d5e7b-148">Přihlaste se tooyour účet Azure v konzole PowerShell hello se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-148">Log in tooyour Azure account in hello PowerShell console with elevated rights.</span></span> <span data-ttu-id="d5e7b-149">Hello následující rutiny vás vyzve k zadání hello přihlašovací údaje pro účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-149">hello following cmdlet prompts you for hello login credentials for your Azure Account.</span></span> <span data-ttu-id="d5e7b-150">Po přihlášení stahování nastavení svého účtu, aby byly k dispozici tooAzure prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-150">After logging in, it downloads your account settings so that they are available tooAzure PowerShell.</span></span> <span data-ttu-id="d5e7b-151">Hello toocomplete rutiny prostředí PowerShell SM použijete tuto část konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-151">You use hello SM PowerShell cmdlets toocomplete this part of hello configuration.</span></span>

  ```powershell
  Add-AzureAccount
  ```
2. <span data-ttu-id="d5e7b-152">Exportujte konfiguračního souboru sítě Azure tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-152">Export your Azure network configuration file by running hello following command.</span></span> <span data-ttu-id="d5e7b-153">Hello umístění hello tooexport tooa jiné umístění souboru v případě potřeby můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-153">You can change hello location of hello file tooexport tooa different location if necessary.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. <span data-ttu-id="d5e7b-154">Soubor otevřete hello .xml, který jste si stáhli tooedit ho.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-154">Open hello .xml file that you downloaded tooedit it.</span></span> <span data-ttu-id="d5e7b-155">Příklad konfiguračního souboru hello sítě, naleznete v části hello [schéma konfigurace sítě](https://msdn.microsoft.com/library/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5e7b-155">For an example of hello network configuration file, see hello [Network Configuration Schema](https://msdn.microsoft.com/library/jj157100.aspx).</span></span>

### <a name="part-2--verify-hello-gateway-subnet"></a><span data-ttu-id="d5e7b-156">Část 2 - Ověřte podsíť brány hello</span><span class="sxs-lookup"><span data-stu-id="d5e7b-156">Part 2 -Verify hello gateway subnet</span></span>
<span data-ttu-id="d5e7b-157">V hello **VirtualNetworkSites** elementu, přidejte tooyour podsíť brány virtuální sítě, pokud dosud nebyla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-157">In hello **VirtualNetworkSites** element, add a gateway subnet tooyour VNet if one has not already been created.</span></span> <span data-ttu-id="d5e7b-158">Při práci s hello sítě konfigurační soubor, hello podsíť brány musí mít název "GatewaySubnet" nebo Azure nelze rozpoznat a používejte ho jako podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-158">When working with hello network configuration file, hello gateway subnet MUST be named "GatewaySubnet" or Azure cannot recognize and use it as a gateway subnet.</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

<span data-ttu-id="d5e7b-159">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="d5e7b-159">**Example:**</span></span>

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

### <a name="part-3---add-hello-local-network-site"></a><span data-ttu-id="d5e7b-160">Část 3 – Přidání hello místní síťový Web</span><span class="sxs-lookup"><span data-stu-id="d5e7b-160">Part 3 - Add hello local network site</span></span>
<span data-ttu-id="d5e7b-161">Hello místní síťový web, které přidáte představuje hello chcete tooconnect toowhich RM virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-161">hello local network site you add represents hello RM VNet toowhich you want tooconnect.</span></span> <span data-ttu-id="d5e7b-162">Přidat **LocalNetworkSites** element toohello soubor, pokud dosud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-162">Add a **LocalNetworkSites** element toohello file if one doesn't already exist.</span></span> <span data-ttu-id="d5e7b-163">V tomto okamžiku v konfiguraci hello hello VPNGatewayAddress může být jakékoli platná veřejná IP adresa protože jsme ještě nevytvořili hello brány pro hello virtuální sítě Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-163">At this point in hello configuration, hello VPNGatewayAddress can be any valid public IP address because we haven't yet created hello gateway for hello Resource Manager VNet.</span></span> <span data-ttu-id="d5e7b-164">Jakmile vytvoříme hello brány, jsme nahraďte tuto IP adresu zástupný symbol hello správné veřejnou IP adresu, která byla přiřazena toohello RM brány.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-164">Once we create hello gateway, we replace this placeholder IP address with hello correct public IP address that has been assigned toohello RM gateway.</span></span>

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-hello-vnet-with-hello-local-network-site"></a><span data-ttu-id="d5e7b-165">Součástí 4 – hello virtuální síť přidružit hello místní síťový Web</span><span class="sxs-lookup"><span data-stu-id="d5e7b-165">Part 4 - Associate hello VNet with hello local network site</span></span>
<span data-ttu-id="d5e7b-166">V této části určíme hello místní síťový web, který má tooconnect hello VNet-to.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-166">In this section, we specify hello local network site that you want tooconnect hello VNet to.</span></span> <span data-ttu-id="d5e7b-167">V takovém případě je hello virtuální sítě Resource Manageru, který jste dříve odkazuje.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-167">In this case, it is hello Resource Manager VNet that you referenced earlier.</span></span> <span data-ttu-id="d5e7b-168">Ujistěte se, zda text hello názvy shodovat.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-168">Make sure hello names match.</span></span> <span data-ttu-id="d5e7b-169">Tento krok není vytvořit bránu.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-169">This step does not create a gateway.</span></span> <span data-ttu-id="d5e7b-170">Určuje, zda text hello místní síť, kterou hello brány se budou připojovat k.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-170">It specifies hello local network that hello gateway will connect to.</span></span>

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-hello-file-and-upload"></a><span data-ttu-id="d5e7b-171">Část 5 – hello soubor uložte a nahrajte</span><span class="sxs-lookup"><span data-stu-id="d5e7b-171">Part 5 - Save hello file and upload</span></span>
<span data-ttu-id="d5e7b-172">Uložte soubor hello a potom ho importovat tooAzure tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-172">Save hello file, then import it tooAzure by running hello following command.</span></span> <span data-ttu-id="d5e7b-173">Ujistěte se, že změníte cestu k souboru hello podle potřeby pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-173">Make sure you change hello file path as necessary for your environment.</span></span>

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="d5e7b-174">Zobrazí se podobné výsledek zobrazující, že hello import proběhlo úspěšně.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-174">You will see a similar result showing that hello import succeeded.</span></span>

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-hello-gateway"></a><span data-ttu-id="d5e7b-175">Část 6 – Vytvoření brány hello</span><span class="sxs-lookup"><span data-stu-id="d5e7b-175">Part 6 - Create hello gateway</span></span>

<span data-ttu-id="d5e7b-176">Před spuštěním tohoto příkladu, najdete v toohello sítě konfigurační soubor, který jste stáhli pro hello přesný názvů této Azure očekává toosee.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-176">Before running this example, refer toohello network configuration file that you downloaded for hello exact names that Azure expects toosee.</span></span> <span data-ttu-id="d5e7b-177">Hello sítě konfigurační soubor obsahuje hello hodnoty pro klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-177">hello network configuration file contains hello values for your classic virtual networks.</span></span> <span data-ttu-id="d5e7b-178">Někdy hello hello názvy pro klasické virtuální sítě jsou změnit v konfiguračním souboru na hello sítě, při vytváření klasických nastavení virtuální sítě v portálu Azure kvůli toohello rozdíly v modelech nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-178">Sometimes hello names for classic VNets are changed in hello network configuration file when creating classic VNet settings in hello Azure portal due toohello differences in hello deployment models.</span></span> <span data-ttu-id="d5e7b-179">Například pokud jste použili Azure portálu toocreate klasické virtuální síti s názvem klasické virtuální sítě a vytvořit ve skupině prostředků s názvem "ClassicRG" hello, hello název, který je obsažen v souboru konfigurace sítě hello je převeden too'Group ClassicRG klasické virtuální sítě ".</span><span class="sxs-lookup"><span data-stu-id="d5e7b-179">For example, if you used hello Azure portal toocreate a classic VNet named 'Classic VNet' and created it in a resource group named 'ClassicRG', hello name that is contained in hello network configuration file is converted too'Group ClassicRG Classic VNet'.</span></span> <span data-ttu-id="d5e7b-180">Při zadávání hello název virtuální sítě, který obsahuje mezery, uzavřete hello hodnotu do uvozovek.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-180">When specifying hello name of a VNet that contains spaces, use quotation marks around hello value.</span></span>


<span data-ttu-id="d5e7b-181">Použijte následující příklad toocreate brány dynamického směrování hello:</span><span class="sxs-lookup"><span data-stu-id="d5e7b-181">Use hello following example toocreate a dynamic routing gateway:</span></span>

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

<span data-ttu-id="d5e7b-182">Stav hello hello brány můžete zkontrolovat pomocí hello **Get-AzureVNetGateway** rutiny.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-182">You can check hello status of hello gateway by using hello **Get-AzureVNetGateway** cmdlet.</span></span>

## <span data-ttu-id="d5e7b-183"><a name="creatermgw"></a>Část 2: Konfigurace hello bránu RM VNet</span><span class="sxs-lookup"><span data-stu-id="d5e7b-183"><a name="creatermgw"></a>Section 2: Configure hello RM VNet gateway</span></span>
<span data-ttu-id="d5e7b-184">toocreate brána sítě VPN pro hello RM sítě VNet, postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-184">toocreate a VPN gateway for hello RM VNet, follow hello following instructions.</span></span> <span data-ttu-id="d5e7b-185">Nespouštějte hello kroky až po načtení hello veřejné IP adresy pro bránu hello klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-185">Don't start hello steps until after you have retrieved hello public IP address for hello classic VNet's gateway.</span></span> 

1. <span data-ttu-id="d5e7b-186">Přihlaste se tooyour účet Azure v konzole PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-186">Log in tooyour Azure account in hello PowerShell console.</span></span> <span data-ttu-id="d5e7b-187">Hello následující rutiny vás vyzve k zadání hello přihlašovací údaje pro účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-187">hello following cmdlet prompts you for hello login credentials for your Azure Account.</span></span> <span data-ttu-id="d5e7b-188">Po přihlášení, se stáhnou nastavení svého účtu, aby byly k dispozici tooAzure prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-188">After logging in, your account settings are downloaded so that they are available tooAzure PowerShell.</span></span>

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  <span data-ttu-id="d5e7b-189">Získání seznamu předplatné Azure, pokud máte více než jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-189">Get a list of your Azure subscriptions if you have more than one subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```
   
  <span data-ttu-id="d5e7b-190">Zadejte hello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-190">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. <span data-ttu-id="d5e7b-191">Vytvoření brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-191">Create a local network gateway.</span></span> <span data-ttu-id="d5e7b-192">Ve virtuální síti hello brány místní sítě obvykle odkazuje tooyour místní umístění.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-192">In a virtual network, hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="d5e7b-193">V takovém případě brána místní sítě hello odkazuje tooyour klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-193">In this case, hello local network gateway refers tooyour Classic VNet.</span></span> <span data-ttu-id="d5e7b-194">Zadejte jeho název, pomocí kterého můžete Azure naleznete tooit a také zadat předponu adresního prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-194">Give it a name by which Azure can refer tooit, and also specify hello address space prefix.</span></span> <span data-ttu-id="d5e7b-195">Azure používá předpona IP adresy hello zadáte tooidentify které tooyour toosend provoz místní umístění.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-195">Azure uses hello IP address prefix you specify tooidentify which traffic toosend tooyour on-premises location.</span></span> <span data-ttu-id="d5e7b-196">Pokud potřebujete informace tooadjust hello později, před vytvořením brány, můžete upravit hodnoty hello a spuštění hello ukázková znovu.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-196">If you need tooadjust hello information here later, before creating your gateway, you can modify hello values and run hello sample again.</span></span>
   
   <span data-ttu-id="d5e7b-197">**-Název** je název hello chcete tooassign toorefer toohello místní síťová brána.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-197">**-Name** is hello name you want tooassign toorefer toohello local network gateway.</span></span><br><span data-ttu-id="d5e7b-198">
   **-AddressPrefix** je hello adresní prostor vaší klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-198">
   **-AddressPrefix** is hello Address Space for your classic VNet.</span></span><br><span data-ttu-id="d5e7b-199">
**-GatewayIpAddress** je hello veřejnou IP adresu brány hello klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-199">
**-GatewayIpAddress** is hello public IP address of hello classic VNet's gateway.</span></span> <span data-ttu-id="d5e7b-200">Ujistěte se, že toochange hello následující ukázka tooreflect hello správnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-200">Be sure toochange hello following sample tooreflect hello correct IP address.</span></span><br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. <span data-ttu-id="d5e7b-201">Požádat o veřejné IP adresy toobe přidělené toohello bránu virtuální sítě pro hello virtuální sítě Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-201">Request a public IP address toobe allocated toohello virtual network gateway for hello Resource Manager VNet.</span></span> <span data-ttu-id="d5e7b-202">Nelze zadat, které chcete toouse hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-202">You can't specify hello IP address that you want toouse.</span></span> <span data-ttu-id="d5e7b-203">Hello IP adresa se přidělí dynamicky toohello brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-203">hello IP address is dynamically allocated toohello virtual network gateway.</span></span> <span data-ttu-id="d5e7b-204">Však neznamená to hello změny IP adresy.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-204">However, this does not mean hello IP address changes.</span></span> <span data-ttu-id="d5e7b-205">Změna IP adresy hello virtuální sítě brány je při hello brány jenom jednou Hello je odstraněno a znovu vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-205">hello only time hello virtual network gateway IP address changes is when hello gateway is deleted and recreated.</span></span> <span data-ttu-id="d5e7b-206">Nezmění, v rámci změny velikosti, resetování nebo jiné operace údržby/upgradu hello brány.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-206">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of hello gateway.</span></span>

  <span data-ttu-id="d5e7b-207">V tomto kroku jsme také nastavit proměnné, která se používá v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-207">In this step, we also set a variable that is used in a later step.</span></span>

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. <span data-ttu-id="d5e7b-208">Ověřte, že virtuální sítě má podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-208">Verify that your virtual network has a gateway subnet.</span></span> <span data-ttu-id="d5e7b-209">Pokud neexistuje žádná podsíť brány, přidáte.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-209">If no gateway subnet exists, add one.</span></span> <span data-ttu-id="d5e7b-210">Zajistěte, aby podsíť brány hello jmenuje *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-210">Make sure hello gateway subnet is named *GatewaySubnet*.</span></span>
5. <span data-ttu-id="d5e7b-211">Načtěte hello podsítě používané pro bránu hello tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-211">Retrieve hello subnet used for hello gateway by running hello following command.</span></span> <span data-ttu-id="d5e7b-212">V tomto kroku jsme také nastavit proměnnou toobe použije v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-212">In this step, we also set a variable toobe used in hello next step.</span></span>
   
   <span data-ttu-id="d5e7b-213">**-Název** je hello název virtuální sítě Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-213">**-Name** is hello name of your Resource Manager VNet.</span></span><br><span data-ttu-id="d5e7b-214">
   **-ResourceGroupName** je skupina prostředků hello této hello je přidružený virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-214">
**-ResourceGroupName** is hello resource group that hello VNet is associated with.</span></span> <span data-ttu-id="d5e7b-215">podsíť brány Hello již musí existovat pro tuto virtuální síť a musí mít název *GatewaySubnet* toowork správně.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-215">hello gateway subnet must already exist for this VNet and must be named *GatewaySubnet* toowork properly.</span></span><br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. <span data-ttu-id="d5e7b-216">Vytvoření konfigurace adresování IP brány hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-216">Create hello gateway IP addressing configuration.</span></span> <span data-ttu-id="d5e7b-217">Hello konfigurace brány definuje podsíť hello a toouse hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-217">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="d5e7b-218">Použijte následující ukázka toocreate hello vlastní konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-218">Use hello following sample toocreate your gateway configuration.</span></span>

  <span data-ttu-id="d5e7b-219">V tomto kroku hello **- SubnetId** a **- PublicIpAddressId** parametry musí být předán vlastnost id hello z hello podsíť a IP adresu objekty, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-219">In this step, hello **-SubnetId** and **-PublicIpAddressId** parameters must be passed hello id property from hello subnet, and IP address objects, respectively.</span></span> <span data-ttu-id="d5e7b-220">Nelze použít jednoduchý řetězec.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-220">You can't use a simple string.</span></span> <span data-ttu-id="d5e7b-221">Tyto proměnné se nastavují v kroku toorequest hello veřejnou IP adresu a hello krok tooretrieve hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-221">These variables are set in hello step toorequest a public IP and hello step tooretrieve hello subnet.</span></span>

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. <span data-ttu-id="d5e7b-222">Vytvoření brány virtuální sítě Resource Manager hello tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-222">Create hello Resource Manager virtual network gateway by running hello following command.</span></span> <span data-ttu-id="d5e7b-223">Hello `-VpnType` musí být *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-223">hello `-VpnType` must be *RouteBased*.</span></span> <span data-ttu-id="d5e7b-224">Může trvat 45 minut nebo déle pro toocreate hello brány.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-224">It can take 45 minutes or more for hello gateway toocreate.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. <span data-ttu-id="d5e7b-225">Po vytvoření brány VPN hello, zkopírujte hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-225">Copy hello public IP address once hello VPN gateway has been created.</span></span> <span data-ttu-id="d5e7b-226">Použijete jej při konfiguraci nastavení místní sítě hello pro klasické virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-226">You use it when you configure hello local network settings for your Classic VNet.</span></span> <span data-ttu-id="d5e7b-227">Můžete použít následující rutiny tooretrieve hello veřejnou IP adresu hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-227">You can use hello following cmdlet tooretrieve hello public IP address.</span></span> <span data-ttu-id="d5e7b-228">Hello veřejná IP adresa je uvedena v hello návratový jako *IpAddress*.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-228">hello public IP address is listed in hello return as *IpAddress*.</span></span>

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="section-3-modify-hello-classic-vnet-local-site-settings"></a><span data-ttu-id="d5e7b-229">Část 3: Úprava hello klasické virtuální sítě místní lokality nastavení</span><span class="sxs-lookup"><span data-stu-id="d5e7b-229">Section 3: Modify hello classic VNet local site settings</span></span>

<span data-ttu-id="d5e7b-230">V této části můžete pracovat s hello klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-230">In this section, you work with hello classic VNet.</span></span> <span data-ttu-id="d5e7b-231">Nahradíte hello zástupný symbol IP adresu, která jste použili při zadávání nastavení hello místní lokality, které budou použité tooconnect toohello brány virtuální sítě Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-231">You replace hello placeholder IP address that you used when specifying hello local site settings that will be used tooconnect toohello Resource Manager VNet gateway.</span></span> 

1. <span data-ttu-id="d5e7b-232">Exportujte hello sítě konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-232">Export hello network configuration file.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. <span data-ttu-id="d5e7b-233">Pomocí textového editoru, upravte hodnotu hello pro prvek VPNGatewayAddress.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-233">Using a text editor, modify hello value for VPNGatewayAddress.</span></span> <span data-ttu-id="d5e7b-234">Nahraďte hello veřejnou IP adresu brány Resource Manager hello hello zástupný symbol IP adresu a potom uložte změny hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-234">Replace hello placeholder IP address with hello public IP address of hello Resource Manager gateway and then save hello changes.</span></span>

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. <span data-ttu-id="d5e7b-235">Import hello upravit tooAzure soubor konfigurace sítě.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-235">Import hello modified network configuration file tooAzure.</span></span>

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <span data-ttu-id="d5e7b-236"><a name="connect"></a>Oddíl 4: Vytvoření připojení mezi bránami hello</span><span class="sxs-lookup"><span data-stu-id="d5e7b-236"><a name="connect"></a>Section 4: Create a connection between hello gateways</span></span>
<span data-ttu-id="d5e7b-237">Vytvoření připojení mezi bránami hello vyžaduje rozhraní PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-237">Creating a connection between hello gateways requires PowerShell.</span></span> <span data-ttu-id="d5e7b-238">Může být nutné tooadd váš účet Azure toouse hello klasickou verzi rutin prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-238">You may need tooadd your Azure Account toouse hello classic version of hello  PowerShell cmdlets.</span></span> <span data-ttu-id="d5e7b-239">Ano, použít toodo **Add-AzureAccount**.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-239">toodo so, use **Add-AzureAccount**.</span></span>

1. <span data-ttu-id="d5e7b-240">V konzole PowerShell text hello nastavte sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-240">In hello PowerShell console, set your shared key.</span></span> <span data-ttu-id="d5e7b-241">Před spuštěním rutin hello, najdete v toohello sítě konfigurační soubor, který jste stáhli pro hello přesný názvů této Azure očekává toosee.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-241">Before running hello cmdlets, refer toohello network configuration file that you downloaded for hello exact names that Azure expects toosee.</span></span> <span data-ttu-id="d5e7b-242">Při zadávání hello název virtuální sítě, který obsahuje mezery, použijte jednoduché uvozovky hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-242">When specifying hello name of a VNet that contains spaces, use single quotation marks around hello value.</span></span><br><br><span data-ttu-id="d5e7b-243">V následujícím příkladu **- VNetName** je název hello hello klasické virtuální sítě a **- LocalNetworkSiteName** je název hello jste zadali pro hello místní síťový Web.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-243">In following example, **-VNetName** is hello name of hello classic VNet and **-LocalNetworkSiteName** is hello name you specified for hello local network site.</span></span> <span data-ttu-id="d5e7b-244">Hello **- SharedKey** je hodnota, která můžete vygenerovat a zadat.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-244">hello **-SharedKey** is a value that you generate and specify.</span></span> <span data-ttu-id="d5e7b-245">V příkladu hello jsme použili 'abc123', ale můžete vygenerovat a použít něco složitější.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-245">In hello example, we used 'abc123', but you can generate and use something more complex.</span></span> <span data-ttu-id="d5e7b-246">Důležité: co je tuto hodnotu hello, který zde určíte Hello musí být hello stejnou hodnotu, při vytváření připojení zadejte v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-246">hello important thing is that hello value you specify here must be hello same value that you specify in hello next step when you create your connection.</span></span> <span data-ttu-id="d5e7b-247">Hello návratový by měl zobrazit **stav: úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-247">hello return should show **Status: Successful**.</span></span>

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. <span data-ttu-id="d5e7b-248">Vytvořte připojení VPN hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d5e7b-248">Create hello VPN connection by running hello following commands:</span></span>
   
  <span data-ttu-id="d5e7b-249">Nastavení proměnných hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-249">Set hello variables.</span></span>

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  <span data-ttu-id="d5e7b-250">Vytvořte připojení hello.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-250">Create hello connection.</span></span> <span data-ttu-id="d5e7b-251">Všimněte si, že hello **- ConnectionType** je protokol IPsec, není Vnet2Vnet.</span><span class="sxs-lookup"><span data-stu-id="d5e7b-251">Notice that hello **-ConnectionType** is IPsec, not Vnet2Vnet.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="section-5-verify-your-connections"></a><span data-ttu-id="d5e7b-252">Část 5: Ověření připojení</span><span class="sxs-lookup"><span data-stu-id="d5e7b-252">Section 5: Verify your connections</span></span>

### <a name="tooverify-hello-connection-from-your-classic-vnet-tooyour-resource-manager-vnet"></a><span data-ttu-id="d5e7b-253">tooverify hello připojení z klasické virtuální sítě tooyour virtuální sítě Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d5e7b-253">tooverify hello connection from your classic VNet tooyour Resource Manager VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="d5e7b-254">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d5e7b-254">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="d5e7b-255">portál Azure</span><span class="sxs-lookup"><span data-stu-id="d5e7b-255">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="tooverify-hello-connection-from-your-resource-manager-vnet-tooyour-classic-vnet"></a><span data-ttu-id="d5e7b-256">tooverify hello připojení z vaší virtuální sítě Resource Manageru tooyour klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="d5e7b-256">tooverify hello connection from your Resource Manager VNet tooyour classic VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="d5e7b-257">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d5e7b-257">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="d5e7b-258">portál Azure</span><span class="sxs-lookup"><span data-stu-id="d5e7b-258">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <span data-ttu-id="d5e7b-259"><a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="d5e7b-259"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

