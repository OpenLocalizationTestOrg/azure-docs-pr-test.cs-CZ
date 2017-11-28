---
title: "Připojit virtuální síť k více lokalitám pomocí brány sítě VPN a prostředí PowerShell: Classic | Microsoft Docs"
description: "Tento článek vás provede s více lokalit místní připojení k virtuální síti pomocí brány sítě VPN pro model nasazení classic."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: yushwang
ms.openlocfilehash: bb3129f70f5eeed99d5889226aa6727f675b6217
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection-classic"></a><span data-ttu-id="48836-103">Přidat připojení Site-to-Site k virtuální síti s existující připojení brány sítě VPN (klasické)</span><span class="sxs-lookup"><span data-stu-id="48836-103">Add a Site-to-Site connection to a VNet with an existing VPN gateway connection (classic)</span></span>

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [<span data-ttu-id="48836-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="48836-104">Azure portal</span></span>](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="48836-105">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="48836-105">PowerShell (classic)</span></span>](vpn-gateway-multi-site.md)
>
>

<span data-ttu-id="48836-106">Tento článek vás provede pomocí prostředí PowerShell pro přidání připojení Site-to-Site (S2S) do brány VPN, který má existující připojení.</span><span class="sxs-lookup"><span data-stu-id="48836-106">This article walks you through using PowerShell to add Site-to-Site (S2S) connections to a VPN gateway that has an existing connection.</span></span> <span data-ttu-id="48836-107">Tento typ připojení se často označuje jako "s více servery" konfigurace.</span><span class="sxs-lookup"><span data-stu-id="48836-107">This type of connection is often referred to as a "multi-site" configuration.</span></span> <span data-ttu-id="48836-108">Postup v tomto článku se vztahuje na virtuální sítě vytvořené pomocí modelu nasazení classic (označovaný taky jako Service Management).</span><span class="sxs-lookup"><span data-stu-id="48836-108">The steps in this article apply to virtual networks created using the classic deployment model (also known as Service Management).</span></span> <span data-ttu-id="48836-109">Tyto kroky se nevztahují na konfigurace koexistujících připojení ExpressRoute nebo Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="48836-109">These steps do not apply to ExpressRoute/Site-to-Site coexisting connection configurations.</span></span>

### <a name="deployment-models-and-methods"></a><span data-ttu-id="48836-110">Modely a metody nasazení</span><span class="sxs-lookup"><span data-stu-id="48836-110">Deployment models and methods</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="48836-111">Tuto tabulku aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="48836-111">We update this table as new articles and additional tools become available for this configuration.</span></span> <span data-ttu-id="48836-112">Když je článek k dispozici, jsme přímý odkaz na něj z této tabulky.</span><span class="sxs-lookup"><span data-stu-id="48836-112">When an article is available, we link directly to it from this table.</span></span>

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a><span data-ttu-id="48836-113">O připojení</span><span class="sxs-lookup"><span data-stu-id="48836-113">About connecting</span></span>

<span data-ttu-id="48836-114">Více místními servery můžete připojit k jedné virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="48836-114">You can connect multiple on-premises sites to a single virtual network.</span></span> <span data-ttu-id="48836-115">To je zvláště atraktivní pro vytváření hybridní cloudové řešení.</span><span class="sxs-lookup"><span data-stu-id="48836-115">This is especially attractive for building hybrid cloud solutions.</span></span> <span data-ttu-id="48836-116">Vytvoření připojení více lokalit pro bránu virtuální sítě Azure je podobná vytváření jiná připojení Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="48836-116">Creating a multi-site connection to your Azure virtual network gateway is similar to creating other Site-to-Site connections.</span></span> <span data-ttu-id="48836-117">Ve skutečnosti můžete použít existující bránu Azure VPN, tak dlouho, dokud brány je dynamický (trasové).</span><span class="sxs-lookup"><span data-stu-id="48836-117">In fact, you can use an existing Azure VPN gateway, as long as the gateway is dynamic (route-based).</span></span>

<span data-ttu-id="48836-118">Pokud již máte statické brány připojené k virtuální síti, můžete změnit typ brány na dynamické, aniž by museli znovu sestavte virtuální sítě, aby mohla pojmout více lokalit.</span><span class="sxs-lookup"><span data-stu-id="48836-118">If you already have a static gateway connected to your virtual network, you can change the gateway type to dynamic without needing to rebuild the virtual network in order to accommodate multi-site.</span></span> <span data-ttu-id="48836-119">Před změnou typ směrování, ujistěte se, že vaše místní brána podporuje konfigurace sítě VPN založené na trasách.</span><span class="sxs-lookup"><span data-stu-id="48836-119">Before changing the routing type, make sure that your on-premises VPN gateway supports route-based VPN configurations.</span></span>

<span data-ttu-id="48836-120">![diagram Multi-Site](./media/vpn-gateway-multi-site/multisite.png "Multi-Site")</span><span class="sxs-lookup"><span data-stu-id="48836-120">![multi-site diagram](./media/vpn-gateway-multi-site/multisite.png "multi-site")</span></span>

## <a name="points-to-consider"></a><span data-ttu-id="48836-121">Body, které je třeba zvážit</span><span class="sxs-lookup"><span data-stu-id="48836-121">Points to consider</span></span>

<span data-ttu-id="48836-122">**Nebudete moci provádět změny do této virtuální sítě pomocí portálu.**</span><span class="sxs-lookup"><span data-stu-id="48836-122">**You won't be able to use the portal to make changes to this virtual network.**</span></span> <span data-ttu-id="48836-123">Budete muset provést změny konfiguračního souboru sítě místo pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="48836-123">You need to make changes to the network configuration file instead of using the portal.</span></span> <span data-ttu-id="48836-124">Pokud provedete změny v portálu, že budete přepsat nastavení odkaz na více lokalit pro tuto virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="48836-124">If you make changes in the portal, they'll overwrite your multi-site reference settings for this virtual network.</span></span>

<span data-ttu-id="48836-125">Má vaše znalosti pomocí konfiguračního souboru sítě podle času, po dokončení procesu více lokalit.</span><span class="sxs-lookup"><span data-stu-id="48836-125">You should feel comfortable using the network configuration file by the time you've completed the multi-site procedure.</span></span> <span data-ttu-id="48836-126">Pokud máte více lidí pracujících na konfiguraci sítě, budete ale muset zajistěte, aby všichni ví o toto omezení.</span><span class="sxs-lookup"><span data-stu-id="48836-126">However, if you have multiple people working on your network configuration, you'll need to make sure that everyone knows about this limitation.</span></span> <span data-ttu-id="48836-127">To neznamená, že nelze použít na portálu vůbec.</span><span class="sxs-lookup"><span data-stu-id="48836-127">This doesn't mean that you can't use the portal at all.</span></span> <span data-ttu-id="48836-128">Můžete ji všem ostatním, kromě provádění změn konfigurace do této konkrétní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="48836-128">You can use it for everything else, except making configuration changes to this particular virtual network.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="48836-129">Než začnete</span><span class="sxs-lookup"><span data-stu-id="48836-129">Before you begin</span></span>

<span data-ttu-id="48836-130">Před zahájením konfigurace, ověřte, zda máte následující:</span><span class="sxs-lookup"><span data-stu-id="48836-130">Before you begin configuration, verify that you have the following:</span></span>

* <span data-ttu-id="48836-131">Kompatibilní hardware sítě VPN pro jednotlivé místní umístění.</span><span class="sxs-lookup"><span data-stu-id="48836-131">Compatible VPN hardware for each on-premises location.</span></span> <span data-ttu-id="48836-132">Zkontrolujte [o zařízeních VPN pro připojení k virtuální síti](vpn-gateway-about-vpn-devices.md) Chcete-li ověřit, zda je zařízení, které chcete použít něco, co se označuje jako kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="48836-132">Check [About VPN Devices for Virtual Network Connectivity](vpn-gateway-about-vpn-devices.md) to verify if the device that you want to use is something that is known to be compatible.</span></span>
* <span data-ttu-id="48836-133">Zvenčí veřejnou IPv4 adresu IP pro každé zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="48836-133">An externally facing public IPv4 IP address for each VPN device.</span></span> <span data-ttu-id="48836-134">IP adresa nesmí být umístěné za adres (NAT)</span><span class="sxs-lookup"><span data-stu-id="48836-134">The IP address cannot be located behind a NAT.</span></span> <span data-ttu-id="48836-135">Toto je požadavek.</span><span class="sxs-lookup"><span data-stu-id="48836-135">This is requirement.</span></span>
* <span data-ttu-id="48836-136">Budete potřebovat nainstalovat nejnovější verzi rutin Azure PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="48836-136">You'll need to install the latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="48836-137">Ujistěte se, že instalujete službu správy (SM) verze kromě verze Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="48836-137">Make sure you install the Service Management (SM) version in addition to the Resource Manager version.</span></span> <span data-ttu-id="48836-138">V tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="48836-138">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>
* <span data-ttu-id="48836-139">Někoho, kdo je znalosti v konfiguraci hardwaru sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="48836-139">Someone who is proficient at configuring your VPN hardware.</span></span> <span data-ttu-id="48836-140">Budete muset silné znalosti o tom, jak nakonfigurovat zařízení VPN nebo pracovat s uživatelem, který nemá.</span><span class="sxs-lookup"><span data-stu-id="48836-140">You'll have to have a strong understanding of how to configure your VPN device, or work with someone who does.</span></span>
* <span data-ttu-id="48836-141">Rozsahy IP adres, které chcete použít pro virtuální síť (Pokud jste již žádný nevytvořili).</span><span class="sxs-lookup"><span data-stu-id="48836-141">The IP address ranges that you want to use for your virtual network (if you haven't already created one).</span></span>
* <span data-ttu-id="48836-142">Rozsahy IP adres pro každou z místní sítě, které budete připojovat k.</span><span class="sxs-lookup"><span data-stu-id="48836-142">The IP address ranges for each of the local network sites that you'll be connecting to.</span></span> <span data-ttu-id="48836-143">Budete potřebovat, abyste měli jistotu, že rozsahy IP adres pro každou z místní sítě, které se chcete připojit k nepřekrývají.</span><span class="sxs-lookup"><span data-stu-id="48836-143">You'll need to make sure that the IP address ranges for each of the local network sites that you want to connect to do not overlap.</span></span> <span data-ttu-id="48836-144">Konfigurace odesílání bude odmítnout, jinak hodnota portálu nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="48836-144">Otherwise, the portal or the REST API will reject the configuration being uploaded.</span></span><br><span data-ttu-id="48836-145">Například pokud máte dvě místní sítě, že oba obsahují 10.2.3.0/24 rozsah adres IP a balíčků s cílovou adresou 10.2.3.3, Azure nebude vědět lokality, která chcete odeslat balíček, protože jsou překrývající se rozsahy adres.</span><span class="sxs-lookup"><span data-stu-id="48836-145">For example, if you have two local network sites that both contain the IP address range 10.2.3.0/24 and you have a package with a destination address 10.2.3.3, Azure wouldn't know which site you want to send the package to because the address ranges are overlapping.</span></span> <span data-ttu-id="48836-146">Aby se zabránilo problémům směrování, Azure vám neumožňuje nahrát konfiguračního souboru, který má překrývající se rozsahy.</span><span class="sxs-lookup"><span data-stu-id="48836-146">To prevent routing issues, Azure doesn't allow you to upload a configuration file that has overlapping ranges.</span></span>

## <a name="1-create-a-site-to-site-vpn"></a><span data-ttu-id="48836-147">1. Vytvoření S2S (Site-to-site) VPN</span><span class="sxs-lookup"><span data-stu-id="48836-147">1. Create a Site-to-Site VPN</span></span>
<span data-ttu-id="48836-148">Pokud už máte sítě Site-to-Site VPN s brány dynamického směrování, skvěle!</span><span class="sxs-lookup"><span data-stu-id="48836-148">If you already have a Site-to-Site VPN with a dynamic routing gateway, great!</span></span> <span data-ttu-id="48836-149">Můžete přejít k [exportovat nastavení konfigurace virtuální sítě](#export).</span><span class="sxs-lookup"><span data-stu-id="48836-149">You can proceed to [Export the virtual network configuration settings](#export).</span></span> <span data-ttu-id="48836-150">Pokud ne, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="48836-150">If not, do the following:</span></span>

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a><span data-ttu-id="48836-151">Pokud již máte virtuální síť Site-to-Site, ale má statické směrování brány (zásadové):</span><span class="sxs-lookup"><span data-stu-id="48836-151">If you already have a Site-to-Site virtual network, but it has a static (policy-based) routing gateway:</span></span>
1. <span data-ttu-id="48836-152">Změňte typ vaší brány na dynamické směrování.</span><span class="sxs-lookup"><span data-stu-id="48836-152">Change your gateway type to dynamic routing.</span></span> <span data-ttu-id="48836-153">Síť VPN více lokalit vyžaduje (také označované jako založené na směrování) brány dynamického směrování.</span><span class="sxs-lookup"><span data-stu-id="48836-153">A multi-site VPN requires a dynamic (also known as route-based) routing gateway.</span></span> <span data-ttu-id="48836-154">Chcete-li změnit váš typ brány, musíte nejprve odstraňte existující bránu, a poté vytvořit novou.</span><span class="sxs-lookup"><span data-stu-id="48836-154">To change your gateway type, you'll need to first delete the existing gateway, then create a new one.</span></span> <span data-ttu-id="48836-155">Pokyny najdete v tématu [jak změnit typ směrování sítě VPN pro bránu](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="48836-155">For instructions, see [How to change the VPN routing type for your gateway](vpn-gateway-configure-vpn-gateway-mp.md).</span></span>  
2. <span data-ttu-id="48836-156">Konfigurace nové brány a vytvořte vaše tunelového připojení sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="48836-156">Configure your new gateway and create your VPN tunnel.</span></span> <span data-ttu-id="48836-157">Pokyny najdete v tématu [konfigurovat bránu VPN na portálu Azure Classic](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="48836-157">For instructions, see [Configure a VPN Gateway in the Azure Classic Portal](vpn-gateway-configure-vpn-gateway-mp.md).</span></span> <span data-ttu-id="48836-158">Nejprve změňte typ vaší brány na dynamické směrování.</span><span class="sxs-lookup"><span data-stu-id="48836-158">First, change your gateway type to dynamic routing.</span></span>

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a><span data-ttu-id="48836-159">Pokud nemáte virtuální síť Site-to-Site:</span><span class="sxs-lookup"><span data-stu-id="48836-159">If you don't have a Site-to-Site virtual network:</span></span>
1. <span data-ttu-id="48836-160">Vytvoření svojí virtuální sítě Site-to-Site pomocí těchto pokynů: [vytvoření virtuální sítě pomocí připojení Site-to-Site VPN na portálu Azure Classic](vpn-gateway-site-to-site-create.md).</span><span class="sxs-lookup"><span data-stu-id="48836-160">Create your Site-to-Site virtual network using these instructions: [Create a Virtual Network with a Site-to-Site VPN Connection in the Azure Classic Portal](vpn-gateway-site-to-site-create.md).</span></span>  
2. <span data-ttu-id="48836-161">Konfigurace brány dynamického směrování podle těchto pokynů: [konfigurovat bránu VPN](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="48836-161">Configure a dynamic routing gateway using these instructions: [Configure a VPN Gateway](vpn-gateway-configure-vpn-gateway-mp.md).</span></span> <span data-ttu-id="48836-162">Je nutné vybrat **dynamické směrování** pro váš typ brány.</span><span class="sxs-lookup"><span data-stu-id="48836-162">Be sure to select **dynamic routing** for your gateway type.</span></span>

## <span data-ttu-id="48836-163"><a name="export"></a>2. Exportovat konfigurační soubor sítě</span><span class="sxs-lookup"><span data-stu-id="48836-163"><a name="export"></a>2. Export the network configuration file</span></span>
<span data-ttu-id="48836-164">Spuštěním následujícího příkazu exportujte konfiguračního souboru sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="48836-164">Export your Azure network configuration file by running the following command.</span></span> <span data-ttu-id="48836-165">Můžete změnit umístění souboru pro export do jiného umístění v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="48836-165">You can change the location of the file to export to a different location if necessary.</span></span>

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-the-network-configuration-file"></a><span data-ttu-id="48836-166">3. Otevření konfiguračního souboru sítě</span><span class="sxs-lookup"><span data-stu-id="48836-166">3. Open the network configuration file</span></span>
<span data-ttu-id="48836-167">Otevření konfiguračního souboru sítě, který jste stáhli v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="48836-167">Open the network configuration file that you downloaded in the last step.</span></span> <span data-ttu-id="48836-168">Pomocí editoru xml, který chcete.</span><span class="sxs-lookup"><span data-stu-id="48836-168">Use any xml editor that you like.</span></span> <span data-ttu-id="48836-169">Soubor by měl vypadat podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="48836-169">The file should look similar to the following:</span></span>

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a><span data-ttu-id="48836-170">4. Přidání více odkazů na web</span><span class="sxs-lookup"><span data-stu-id="48836-170">4. Add multiple site references</span></span>
<span data-ttu-id="48836-171">Když přidáváte nebo odebíráte lokality referenční informace, budete provedete změny konfigurace ConnectionsToLocalNetwork/LocalNetworkSiteRef.</span><span class="sxs-lookup"><span data-stu-id="48836-171">When you add or remove site reference information, you'll make configuration changes to the ConnectionsToLocalNetwork/LocalNetworkSiteRef.</span></span> <span data-ttu-id="48836-172">Přidání nové vyvolá odkaz místního webu Azure k vytvoření nové tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="48836-172">Adding a new local site reference triggers Azure to create a new tunnel.</span></span> <span data-ttu-id="48836-173">V následujícím příkladu je konfigurace sítě pro připojení k jedné lokalitě.</span><span class="sxs-lookup"><span data-stu-id="48836-173">In the example below, the network configuration is for a single-site connection.</span></span> <span data-ttu-id="48836-174">Jakmile dokončíte provádění změny, uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="48836-174">Save the file once you have finished making your changes.</span></span>

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

<span data-ttu-id="48836-175">Chcete-li přidat odkazy na další lokality (vytvořit konfiguraci s více servery), jednoduše přidat další řádky "LocalNetworkSiteRef", jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="48836-175">To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below:</span></span>

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-the-network-configuration-file"></a><span data-ttu-id="48836-176">5. Import konfiguračního souboru sítě</span><span class="sxs-lookup"><span data-stu-id="48836-176">5. Import the network configuration file</span></span>
<span data-ttu-id="48836-177">Importujte konfiguračního souboru sítě.</span><span class="sxs-lookup"><span data-stu-id="48836-177">Import the network configuration file.</span></span> <span data-ttu-id="48836-178">Při importování tohoto souboru se změnami, bude přidána do nové tunelových propojení.</span><span class="sxs-lookup"><span data-stu-id="48836-178">When you import this file with the changes, the new tunnels will be added.</span></span> <span data-ttu-id="48836-179">Tunely použije dynamickou bránu, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="48836-179">The tunnels will use the dynamic gateway that you created earlier.</span></span> <span data-ttu-id="48836-180">Můžete použít buď portálu classic nebo prostředí PowerShell pro import souboru.</span><span class="sxs-lookup"><span data-stu-id="48836-180">You can either use the classic portal, or PowerShell to import the file.</span></span>

## <a name="6-download-keys"></a><span data-ttu-id="48836-181">6. Stáhněte si klíče</span><span class="sxs-lookup"><span data-stu-id="48836-181">6. Download keys</span></span>
<span data-ttu-id="48836-182">Po přidání vaší nové tunely, použijte rutinu prostředí PowerShell 'Get-AzureVNetGatewayKey' získat předsdílené klíče protokolu IPsec/IKE pro každé tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="48836-182">Once your new tunnels have been added, use the PowerShell cmdlet 'Get-AzureVNetGatewayKey' to get the IPsec/IKE pre-shared keys for each tunnel.</span></span>

<span data-ttu-id="48836-183">Například:</span><span class="sxs-lookup"><span data-stu-id="48836-183">For example:</span></span>

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

<span data-ttu-id="48836-184">Pokud dáváte přednost, můžete také použít *získat virtuální sítě sdílený klíč brány* REST API získat předsdílených klíčů.</span><span class="sxs-lookup"><span data-stu-id="48836-184">If you prefer, you can also use the *Get Virtual Network Gateway Shared Key* REST API to get the pre-shared keys.</span></span>

## <a name="7-verify-your-connections"></a><span data-ttu-id="48836-185">7. Zkontrolujte svá připojení</span><span class="sxs-lookup"><span data-stu-id="48836-185">7. Verify your connections</span></span>
<span data-ttu-id="48836-186">Zkontrolujte stav tunelového propojení více lokalit.</span><span class="sxs-lookup"><span data-stu-id="48836-186">Check the multi-site tunnel status.</span></span> <span data-ttu-id="48836-187">Po stažení klíče pro každé tunelové propojení, budete chtít ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="48836-187">After downloading the keys for each tunnel, you'll want to verify connections.</span></span> <span data-ttu-id="48836-188">Použijte 'Get-AzureVnetConnection' k získání seznamu tunelových propojení virtuální sítě, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="48836-188">Use 'Get-AzureVnetConnection' to get a list of virtual network tunnels, as shown in the example below.</span></span> <span data-ttu-id="48836-189">VNet1 je název sítě vnet.</span><span class="sxs-lookup"><span data-stu-id="48836-189">VNet1 is the name of the VNet.</span></span>

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

<span data-ttu-id="48836-190">Příklad návratový:</span><span class="sxs-lookup"><span data-stu-id="48836-190">Example return:</span></span>

```
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
```

## <a name="next-steps"></a><span data-ttu-id="48836-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48836-191">Next steps</span></span>

<span data-ttu-id="48836-192">Další informace o branách VPN najdete v tématu [informace o branách VPN](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="48836-192">To learn more about VPN Gateways, see [About VPN Gateways](vpn-gateway-about-vpngateways.md).</span></span>
