---
title: "Odstranit bránu virtuální sítě: prostředí PowerShell: portál Azure classic | Microsoft Docs"
description: "Odstraňte bránu virtuální sítě pomocí prostředí PowerShell v modelu nasazení classic."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: cherylmc
ms.openlocfilehash: b1bc18307227a728e2bc8fd95e30fdc1cbdb8c59
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="delete-a-virtual-network-gateway-using-powershell-classic"></a><span data-ttu-id="ca499-103">Odstranit bránu virtuální sítě pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="ca499-103">Delete a virtual network gateway using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ca499-104">Resource Manager – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ca499-104">Resource Manager - Azure portal</span></span>](vpn-gateway-delete-vnet-gateway-portal.md)
> * [<span data-ttu-id="ca499-105">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="ca499-105">Resource Manager - PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [<span data-ttu-id="ca499-106">Classic – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ca499-106">Classic - PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

<span data-ttu-id="ca499-107">Tento článek vám umožňuje odstranit bránu sítě VPN v klasickém modelu nasazení pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ca499-107">This article helps you delete a VPN gateway in the classic deployment model by using PowerShell.</span></span> <span data-ttu-id="ca499-108">Po odstranění brány virtuální sítě, upravte konfigurační soubor sítě. Chcete-li odebrat prvky, které už používáte.</span><span class="sxs-lookup"><span data-stu-id="ca499-108">After the virtual network gateway has been deleted, modify the network configuration file to remove elements that you are no longer using.</span></span>

##<span data-ttu-id="ca499-109"><a name="connect"></a>Krok 1: Připojení k Azure</span><span class="sxs-lookup"><span data-stu-id="ca499-109"><a name="connect"></a>Step 1: Connect to Azure</span></span>

### <a name="1-install-the-latest-powershell-cmdlets"></a><span data-ttu-id="ca499-110">1. Nainstalujte nejnovější rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ca499-110">1. Install the latest PowerShell cmdlets.</span></span>

<span data-ttu-id="ca499-111">Stáhněte a nainstalujte nejnovější verzi rutin prostředí PowerShell Azure Service Management (SM).</span><span class="sxs-lookup"><span data-stu-id="ca499-111">Download and install the latest version of the Azure Service Management (SM) PowerShell cmdlets.</span></span> <span data-ttu-id="ca499-112">Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ca499-112">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="2-connect-to-your-azure-account"></a><span data-ttu-id="ca499-113">2. Připojte k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="ca499-113">2. Connect to your Azure account.</span></span> 

<span data-ttu-id="ca499-114">Otevřete konzolu PowerShellu se zvýšenými oprávněními a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="ca499-114">Open your PowerShell console with elevated rights and connect to your account.</span></span> <span data-ttu-id="ca499-115">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="ca499-115">Use the following example to help you connect:</span></span>

```powershell
Add-AzureAccount
```

## <span data-ttu-id="ca499-116"><a name="export"></a>Krok 2: Export a v souboru konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="ca499-116"><a name="export"></a>Step 2: Export and view the network configuration file</span></span>

<span data-ttu-id="ca499-117">Vytvořte ve svém počítači adresář a potom do něj exportujte soubor konfigurace sítě.</span><span class="sxs-lookup"><span data-stu-id="ca499-117">Create a directory on your computer and then export the network configuration file to the directory.</span></span> <span data-ttu-id="ca499-118">Můžete použít tento soubor do obou zobrazení aktuální informace o konfiguraci a také ke změně konfigurace sítě.</span><span class="sxs-lookup"><span data-stu-id="ca499-118">You use this file to both view the current configuration information, and also to modify the network configuration.</span></span>

<span data-ttu-id="ca499-119">V tomto příkladu se soubor konfigurace sítě exportuje do adresáře C:\AzureNet.</span><span class="sxs-lookup"><span data-stu-id="ca499-119">In this example, the network configuration file is exported to C:\AzureNet.</span></span>

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="ca499-120">Otevřete soubor v textovém editoru a zobrazit název pro klasické virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="ca499-120">Open the file with a text editor and view the name for your classic VNet.</span></span> <span data-ttu-id="ca499-121">Když vytvoříte virtuální síť na portálu Azure, úplný název, který používá Azure není zobrazená v portálu.</span><span class="sxs-lookup"><span data-stu-id="ca499-121">When you create a VNet in the Azure portal, the full name that Azure uses is not visible in the portal.</span></span> <span data-ttu-id="ca499-122">Například virtuální sítě, který se zobrazí s názvem 'ClassicVNet1' na portálu Azure může mít mnoho delší název v souboru konfigurace sítě.</span><span class="sxs-lookup"><span data-stu-id="ca499-122">For example, a VNet that appears to be named 'ClassicVNet1' in the Azure portal, may have a much longer name in the network configuration file.</span></span> <span data-ttu-id="ca499-123">Název může vypadat podobně jako: 'Skupiny ClassicRG1 ClassicVNet1'.</span><span class="sxs-lookup"><span data-stu-id="ca499-123">The name might look something like: 'Group ClassicRG1 ClassicVNet1'.</span></span> <span data-ttu-id="ca499-124">Názvy virtuální sítě jsou uvedeny jako **' VirtualNetworkSite name ='**.</span><span class="sxs-lookup"><span data-stu-id="ca499-124">Virtual network names are listed as **'VirtualNetworkSite name ='**.</span></span> <span data-ttu-id="ca499-125">Použije názvy v konfiguračním souboru sítě při spuštění vaše rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ca499-125">Use the names in the network configuration file when running your PowerShell cmdlets.</span></span>

## <span data-ttu-id="ca499-126"><a name="delete"></a>Krok 3: Odstranit bránu virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="ca499-126"><a name="delete"></a>Step 3: Delete the virtual network gateway</span></span>

<span data-ttu-id="ca499-127">Při odstranění brány virtuální sítě jsou odpojené všech připojení k virtuální síti prostřednictvím brány.</span><span class="sxs-lookup"><span data-stu-id="ca499-127">When you delete a virtual network gateway, all connections to the VNet through the gateway are disconnected.</span></span> <span data-ttu-id="ca499-128">Pokud máte P2S klientů připojených k virtuální síti, budou odpojeny bez upozornění.</span><span class="sxs-lookup"><span data-stu-id="ca499-128">If you have P2S clients connected to the VNet, they will be disconnected without warning.</span></span>

<span data-ttu-id="ca499-129">Tento příklad odstraní bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ca499-129">This example deletes the virtual network gateway.</span></span> <span data-ttu-id="ca499-130">Nezapomeňte použít úplný název virtuální sítě z konfiguračního souboru sítě.</span><span class="sxs-lookup"><span data-stu-id="ca499-130">Make sure to use the full name of the virtual network from the network configuration file.</span></span>

```powershell
Remove-AzureVNetGateway -VNetName "Group ClassicRG1 ClassicVNet1"
```

<span data-ttu-id="ca499-131">V případě úspěšného návratový uvádí:</span><span class="sxs-lookup"><span data-stu-id="ca499-131">If successful, the return shows:</span></span>

```
Status : Successful
```

## <span data-ttu-id="ca499-132"><a name="modify"></a>Krok 4: Upravte konfigurační soubor sítě.</span><span class="sxs-lookup"><span data-stu-id="ca499-132"><a name="modify"></a>Step 4: Modify the network configuration file</span></span>

<span data-ttu-id="ca499-133">Pokud odstraníte bránu virtuální sítě, rutina neupravuje konfiguračního souboru sítě.</span><span class="sxs-lookup"><span data-stu-id="ca499-133">When you delete a virtual network gateway, the cmdlet does not modify the network configuration file.</span></span> <span data-ttu-id="ca499-134">Budete muset upravit soubor odebrat prvky, které se již nepoužívají.</span><span class="sxs-lookup"><span data-stu-id="ca499-134">You need to modify the file to remove the elements that are no longer being used.</span></span> <span data-ttu-id="ca499-135">V následujících částech můžete upravit konfiguračního souboru sítě, který jste si stáhli.</span><span class="sxs-lookup"><span data-stu-id="ca499-135">The following sections help you modify the network configuration file that you downloaded.</span></span>

### <span data-ttu-id="ca499-136"><a name="lnsref"></a>Odkazů na web místní sítě</span><span class="sxs-lookup"><span data-stu-id="ca499-136"><a name="lnsref"></a>Local Network Site References</span></span>

<span data-ttu-id="ca499-137">Odebrat informace o referenční lokality, provést změny konfigurace **ConnectionsToLocalNetwork/LocalNetworkSiteRef**.</span><span class="sxs-lookup"><span data-stu-id="ca499-137">To remove site reference information, make configuration changes to **ConnectionsToLocalNetwork/LocalNetworkSiteRef**.</span></span> <span data-ttu-id="ca499-138">Odebrání místní lokality odkaz aktivačních událostí Azure a odstranit přes tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="ca499-138">Removing a local site reference triggers Azure to delete a tunnel.</span></span> <span data-ttu-id="ca499-139">V závislosti na konfiguraci, kterou jste vytvořili, nemusí mít **LocalNetworkSiteRef** uvedené.</span><span class="sxs-lookup"><span data-stu-id="ca499-139">Depending on the configuration that you created, you may not have a **LocalNetworkSiteRef** listed.</span></span>

```
<Gateway>
   <ConnectionsToLocalNetwork>
     <LocalNetworkSiteRef name="D1BFC9CB_Site2">
       <Connection type="IPsec" />
     </LocalNetworkSiteRef>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

<span data-ttu-id="ca499-140">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca499-140">Example:</span></span>

```
<Gateway>
   <ConnectionsToLocalNetwork>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

###<span data-ttu-id="ca499-141"><a name="lns"></a>Místní sítě</span><span class="sxs-lookup"><span data-stu-id="ca499-141"><a name="lns"></a>Local Network Sites</span></span>

<span data-ttu-id="ca499-142">Odeberte všechny místní servery, které jsou již nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="ca499-142">Remove any local sites that you are no longer using.</span></span> <span data-ttu-id="ca499-143">V závislosti na konfiguraci, které jste vytvořili, je možné, že nemáte **LocalNetworkSite** uvedené.</span><span class="sxs-lookup"><span data-stu-id="ca499-143">Depending on the configuration you created, it is possible that you don't have a **LocalNetworkSite** listed.</span></span>

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
  <LocalNetworkSite name="Site3">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>57.179.18.164</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

<span data-ttu-id="ca499-144">V tomto příkladu jsme odebrali pouze Site3.</span><span class="sxs-lookup"><span data-stu-id="ca499-144">In this example, we removed only Site3.</span></span>

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

### <span data-ttu-id="ca499-145"><a name="clientaddresss"></a>Fond adres klienta</span><span class="sxs-lookup"><span data-stu-id="ca499-145"><a name="clientaddresss"></a>Client AddressPool</span></span>

<span data-ttu-id="ca499-146">Pokud jste měli P2S připojení k virtuální síti, budete mít **VPNClientAddressPool**.</span><span class="sxs-lookup"><span data-stu-id="ca499-146">If you had a P2S connection to your VNet, you will have a **VPNClientAddressPool**.</span></span> <span data-ttu-id="ca499-147">Odeberte fondy adres klienta, které odpovídají bránu virtuální sítě, kterou jste odstranili.</span><span class="sxs-lookup"><span data-stu-id="ca499-147">Remove the client address pools that correspond to the virtual network gateway that you deleted.</span></span>

```
<Gateway>
    <VPNClientAddressPool>
      <AddressPrefix>10.1.0.0/24</AddressPrefix>
    </VPNClientAddressPool>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

<span data-ttu-id="ca499-148">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca499-148">Example:</span></span>

```
<Gateway>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

### <span data-ttu-id="ca499-149"><a name="gwsub"></a>GatewaySubnet</span><span class="sxs-lookup"><span data-stu-id="ca499-149"><a name="gwsub"></a>GatewaySubnet</span></span>

<span data-ttu-id="ca499-150">Odstranit **GatewaySubnet** odpovídající virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ca499-150">Delete the **GatewaySubnet** that corresponds to the VNet.</span></span>

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
   <Subnet name="GatewaySubnet">
     <AddressPrefix>10.11.1.0/29</AddressPrefix>
   </Subnet>
 </Subnets>
```

<span data-ttu-id="ca499-151">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca499-151">Example:</span></span>

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
 </Subnets>
```

## <span data-ttu-id="ca499-152"><a name="upload"></a>Krok 5: Nahrát soubor konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="ca499-152"><a name="upload"></a>Step 5: Upload the network configuration file</span></span>

<span data-ttu-id="ca499-153">Změny uložte a nahrajte konfiguračního souboru sítě do Azure.</span><span class="sxs-lookup"><span data-stu-id="ca499-153">Save your changes and upload the network configuration file to Azure.</span></span> <span data-ttu-id="ca499-154">Ujistěte se, že změníte cestu k souboru v případě potřeby pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="ca499-154">Make sure you change the file path as necessary for your environment.</span></span>

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="ca499-155">V případě úspěšného návratový ukazuje něco podobně jako tento příklad:</span><span class="sxs-lookup"><span data-stu-id="ca499-155">If successful, the return shows something similar to this example:</span></span>

```
OperationDescription        OperationId                      OperationStatus                                                
--------------------        -----------                      ---------------                                           
Set-AzureVNetConfig         e0ee6e66-9167-cfa7-a746-7casb9   Succeeded