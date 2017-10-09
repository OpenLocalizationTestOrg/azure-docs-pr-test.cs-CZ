---
title: "Připojit virtuální síťové weby toomultiple pomocí brány sítě VPN a prostředí PowerShell: Classic | Microsoft Docs"
description: "Tento článek vás provede připojením více místní lokality tooa virtuální sítě pomocí brány VPN pro model nasazení classic hello."
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
ms.openlocfilehash: 5404b1c55ed3453b4dbc94dfd93e47c0812025f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection-classic"></a><span data-ttu-id="9269b-103">Přidat tooa připojení Site-to-Site virtuální sítě se existující připojení brány sítě VPN (klasické)</span><span class="sxs-lookup"><span data-stu-id="9269b-103">Add a Site-to-Site connection tooa VNet with an existing VPN gateway connection (classic)</span></span>

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9269b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9269b-104">Azure portal</span></span>](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="9269b-105">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="9269b-105">PowerShell (classic)</span></span>](vpn-gateway-multi-site.md)
>
>

<span data-ttu-id="9269b-106">Tento článek vás provede pomocí prostředí PowerShell tooadd Site-to-Site (S2S) připojení tooa VPN bránu, která má existující připojení.</span><span class="sxs-lookup"><span data-stu-id="9269b-106">This article walks you through using PowerShell tooadd Site-to-Site (S2S) connections tooa VPN gateway that has an existing connection.</span></span> <span data-ttu-id="9269b-107">Tento typ připojení je často označují tooas "s více servery" konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9269b-107">This type of connection is often referred tooas a "multi-site" configuration.</span></span> <span data-ttu-id="9269b-108">Hello kroky v tomto článku použít toovirtual sítě vytvořené pomocí modelu nasazení classic hello (také označované jako Service Management).</span><span class="sxs-lookup"><span data-stu-id="9269b-108">hello steps in this article apply toovirtual networks created using hello classic deployment model (also known as Service Management).</span></span> <span data-ttu-id="9269b-109">Tyto kroky se nevztahují konfigurace koexistujících připojení tooExpressRoute/Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="9269b-109">These steps do not apply tooExpressRoute/Site-to-Site coexisting connection configurations.</span></span>

### <a name="deployment-models-and-methods"></a><span data-ttu-id="9269b-110">Modely a metody nasazení</span><span class="sxs-lookup"><span data-stu-id="9269b-110">Deployment models and methods</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="9269b-111">Tuto tabulku aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="9269b-111">We update this table as new articles and additional tools become available for this configuration.</span></span> <span data-ttu-id="9269b-112">Když je článek k dispozici, jsme odkaz přímo tooit z této tabulky.</span><span class="sxs-lookup"><span data-stu-id="9269b-112">When an article is available, we link directly tooit from this table.</span></span>

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a><span data-ttu-id="9269b-113">O připojení</span><span class="sxs-lookup"><span data-stu-id="9269b-113">About connecting</span></span>

<span data-ttu-id="9269b-114">Více místními lokalitami tooa jedné virtuální sítě se můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="9269b-114">You can connect multiple on-premises sites tooa single virtual network.</span></span> <span data-ttu-id="9269b-115">To je zvláště atraktivní pro vytváření hybridní cloudové řešení.</span><span class="sxs-lookup"><span data-stu-id="9269b-115">This is especially attractive for building hybrid cloud solutions.</span></span> <span data-ttu-id="9269b-116">Vytvoření brány virtuální sítě Azure tooyour připojení typu Multi-Site je podobné toocreating jiná připojení Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="9269b-116">Creating a multi-site connection tooyour Azure virtual network gateway is similar toocreating other Site-to-Site connections.</span></span> <span data-ttu-id="9269b-117">Ve skutečnosti můžete použít existující bránu Azure VPN, tak dlouho, dokud hello brány je dynamický (trasové).</span><span class="sxs-lookup"><span data-stu-id="9269b-117">In fact, you can use an existing Azure VPN gateway, as long as hello gateway is dynamic (route-based).</span></span>

<span data-ttu-id="9269b-118">Pokud již máte statická Brána virtuální sítě připojený tooyour, můžete změnit toodynamic typ brány hello bez nutnosti toorebuild hello virtuální sítě v pořadí tooaccommodate více lokalit.</span><span class="sxs-lookup"><span data-stu-id="9269b-118">If you already have a static gateway connected tooyour virtual network, you can change hello gateway type toodynamic without needing toorebuild hello virtual network in order tooaccommodate multi-site.</span></span> <span data-ttu-id="9269b-119">Před změnou typu hello směrování, ujistěte se, že vaše místní brána podporuje konfigurace sítě VPN založené na trasách.</span><span class="sxs-lookup"><span data-stu-id="9269b-119">Before changing hello routing type, make sure that your on-premises VPN gateway supports route-based VPN configurations.</span></span>

<span data-ttu-id="9269b-120">![diagram Multi-Site](./media/vpn-gateway-multi-site/multisite.png "Multi-Site")</span><span class="sxs-lookup"><span data-stu-id="9269b-120">![multi-site diagram](./media/vpn-gateway-multi-site/multisite.png "multi-site")</span></span>

## <a name="points-tooconsider"></a><span data-ttu-id="9269b-121">Body tooconsider</span><span class="sxs-lookup"><span data-stu-id="9269b-121">Points tooconsider</span></span>

<span data-ttu-id="9269b-122">**Nebudete moct toouse hello portálu toomake změny toothis virtuální sítě.**</span><span class="sxs-lookup"><span data-stu-id="9269b-122">**You won't be able toouse hello portal toomake changes toothis virtual network.**</span></span> <span data-ttu-id="9269b-123">Budete potřebovat soubor konfigurace toomake změny toohello sítě místo pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="9269b-123">You need toomake changes toohello network configuration file instead of using hello portal.</span></span> <span data-ttu-id="9269b-124">Pokud provedete změny v portálu hello, že budete přepsat nastavení odkaz na více lokalit pro tuto virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="9269b-124">If you make changes in hello portal, they'll overwrite your multi-site reference settings for this virtual network.</span></span>

<span data-ttu-id="9269b-125">Musí mít možnost hello sítě konfiguračního souboru pomocí hello čas jste dokončili postup hello více lokalit.</span><span class="sxs-lookup"><span data-stu-id="9269b-125">You should feel comfortable using hello network configuration file by hello time you've completed hello multi-site procedure.</span></span> <span data-ttu-id="9269b-126">Pokud máte více lidí pracujících na konfiguraci sítě, ale budete potřebovat toomake, že všichni ví o toto omezení.</span><span class="sxs-lookup"><span data-stu-id="9269b-126">However, if you have multiple people working on your network configuration, you'll need toomake sure that everyone knows about this limitation.</span></span> <span data-ttu-id="9269b-127">To neznamená, nelze použít hello portál vůbec.</span><span class="sxs-lookup"><span data-stu-id="9269b-127">This doesn't mean that you can't use hello portal at all.</span></span> <span data-ttu-id="9269b-128">Můžete ji všem ostatním, s výjimkou provedení konfigurace změny toothis konkrétní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9269b-128">You can use it for everything else, except making configuration changes toothis particular virtual network.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="9269b-129">Než začnete</span><span class="sxs-lookup"><span data-stu-id="9269b-129">Before you begin</span></span>

<span data-ttu-id="9269b-130">Před zahájením konfigurace, ověřte, zda máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="9269b-130">Before you begin configuration, verify that you have hello following:</span></span>

* <span data-ttu-id="9269b-131">Kompatibilní hardware sítě VPN pro jednotlivé místní umístění.</span><span class="sxs-lookup"><span data-stu-id="9269b-131">Compatible VPN hardware for each on-premises location.</span></span> <span data-ttu-id="9269b-132">Zkontrolujte [o zařízeních VPN pro připojení k virtuální síti](vpn-gateway-about-vpn-devices.md) tooverify Pokud hello zařízení, které chcete toouse něco, který se označuje toobe kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="9269b-132">Check [About VPN Devices for Virtual Network Connectivity](vpn-gateway-about-vpn-devices.md) tooverify if hello device that you want toouse is something that is known toobe compatible.</span></span>
* <span data-ttu-id="9269b-133">Zvenčí veřejnou IPv4 adresu IP pro každé zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="9269b-133">An externally facing public IPv4 IP address for each VPN device.</span></span> <span data-ttu-id="9269b-134">Hello IP adresa nesmí být umístěné za adres (NAT)</span><span class="sxs-lookup"><span data-stu-id="9269b-134">hello IP address cannot be located behind a NAT.</span></span> <span data-ttu-id="9269b-135">Toto je požadavek.</span><span class="sxs-lookup"><span data-stu-id="9269b-135">This is requirement.</span></span>
* <span data-ttu-id="9269b-136">Budete potřebovat tooinstall hello nejnovější verzi rutin prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="9269b-136">You'll need tooinstall hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="9269b-137">Ujistěte se, že instalujete hello služby správy (SM) verze v přidání toohello Resource Manager verzi.</span><span class="sxs-lookup"><span data-stu-id="9269b-137">Make sure you install hello Service Management (SM) version in addition toohello Resource Manager version.</span></span> <span data-ttu-id="9269b-138">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="9269b-138">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>
* <span data-ttu-id="9269b-139">Někoho, kdo je znalosti v konfiguraci hardwaru sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="9269b-139">Someone who is proficient at configuring your VPN hardware.</span></span> <span data-ttu-id="9269b-140">Budete mít toohave silné pochopení toho, jak tooconfigure zařízení VPN nebo práci s uživatelem, který nemá.</span><span class="sxs-lookup"><span data-stu-id="9269b-140">You'll have toohave a strong understanding of how tooconfigure your VPN device, or work with someone who does.</span></span>
* <span data-ttu-id="9269b-141">Hello rozsahy IP adres má toouse pro vaši virtuální síť (Pokud jste již žádný nevytvořili).</span><span class="sxs-lookup"><span data-stu-id="9269b-141">hello IP address ranges that you want toouse for your virtual network (if you haven't already created one).</span></span>
* <span data-ttu-id="9269b-142">pro každou hello místních síťových webů, které budete připojovat k rozsahy Hello IP adres.</span><span class="sxs-lookup"><span data-stu-id="9269b-142">hello IP address ranges for each of hello local network sites that you'll be connecting to.</span></span> <span data-ttu-id="9269b-143">Budete potřebovat toomake jistotu, že rozsahy hello IP adres pro každou hello místních síťových webů, které chcete tooconnect toodo není překrývají.</span><span class="sxs-lookup"><span data-stu-id="9269b-143">You'll need toomake sure that hello IP address ranges for each of hello local network sites that you want tooconnect toodo not overlap.</span></span> <span data-ttu-id="9269b-144">V opačném hello portálu nebo hello REST API odmítnou hello Konfigurace odesílání.</span><span class="sxs-lookup"><span data-stu-id="9269b-144">Otherwise, hello portal or hello REST API will reject hello configuration being uploaded.</span></span><br><span data-ttu-id="9269b-145">Například pokud máte dvě místní sítě, že oba obsahují hello IP adresa rozsahu 10.2.3.0/24 a balíčků s cílovou adresou 10.2.3.3, Azure nebude vědět, který server chcete toosend hello balíček toobecause hello rozsahy adres jsou překrývající se.</span><span class="sxs-lookup"><span data-stu-id="9269b-145">For example, if you have two local network sites that both contain hello IP address range 10.2.3.0/24 and you have a package with a destination address 10.2.3.3, Azure wouldn't know which site you want toosend hello package toobecause hello address ranges are overlapping.</span></span> <span data-ttu-id="9269b-146">tooprevent směrování problémy, Azure vám neumožňuje tooupload konfiguračního souboru, který má překrývající se rozsahy.</span><span class="sxs-lookup"><span data-stu-id="9269b-146">tooprevent routing issues, Azure doesn't allow you tooupload a configuration file that has overlapping ranges.</span></span>

## <a name="1-create-a-site-to-site-vpn"></a><span data-ttu-id="9269b-147">1. Vytvoření S2S (Site-to-site) VPN</span><span class="sxs-lookup"><span data-stu-id="9269b-147">1. Create a Site-to-Site VPN</span></span>
<span data-ttu-id="9269b-148">Pokud už máte sítě Site-to-Site VPN s brány dynamického směrování, skvěle!</span><span class="sxs-lookup"><span data-stu-id="9269b-148">If you already have a Site-to-Site VPN with a dynamic routing gateway, great!</span></span> <span data-ttu-id="9269b-149">Abyste mohli pokračovat příliš[exportovat nastavení konfigurace virtuální sítě hello](#export).</span><span class="sxs-lookup"><span data-stu-id="9269b-149">You can proceed too[Export hello virtual network configuration settings](#export).</span></span> <span data-ttu-id="9269b-150">Pokud ne, hello následující:</span><span class="sxs-lookup"><span data-stu-id="9269b-150">If not, do hello following:</span></span>

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a><span data-ttu-id="9269b-151">Pokud již máte virtuální síť Site-to-Site, ale má statické směrování brány (zásadové):</span><span class="sxs-lookup"><span data-stu-id="9269b-151">If you already have a Site-to-Site virtual network, but it has a static (policy-based) routing gateway:</span></span>
1. <span data-ttu-id="9269b-152">Změňte vaší brány typ toodynamic směrování.</span><span class="sxs-lookup"><span data-stu-id="9269b-152">Change your gateway type toodynamic routing.</span></span> <span data-ttu-id="9269b-153">Síť VPN více lokalit vyžaduje (také označované jako založené na směrování) brány dynamického směrování.</span><span class="sxs-lookup"><span data-stu-id="9269b-153">A multi-site VPN requires a dynamic (also known as route-based) routing gateway.</span></span> <span data-ttu-id="9269b-154">Zadejte toochange bránu, budete potřebovat toofirst odstranění hello existující bránu a pak vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="9269b-154">toochange your gateway type, you'll need toofirst delete hello existing gateway, then create a new one.</span></span> <span data-ttu-id="9269b-155">Pokyny najdete v tématu [jak toochange hello směrování typ sítě VPN pro bránu](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="9269b-155">For instructions, see [How toochange hello VPN routing type for your gateway](vpn-gateway-configure-vpn-gateway-mp.md).</span></span>  
2. <span data-ttu-id="9269b-156">Konfigurace nové brány a vytvořte vaše tunelového připojení sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="9269b-156">Configure your new gateway and create your VPN tunnel.</span></span> <span data-ttu-id="9269b-157">Pokyny najdete v tématu [konfigurovat bránu VPN v hello portálu Azure Classic](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="9269b-157">For instructions, see [Configure a VPN Gateway in hello Azure Classic Portal](vpn-gateway-configure-vpn-gateway-mp.md).</span></span> <span data-ttu-id="9269b-158">Nejprve změňte vaší brány typ toodynamic směrování.</span><span class="sxs-lookup"><span data-stu-id="9269b-158">First, change your gateway type toodynamic routing.</span></span>

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a><span data-ttu-id="9269b-159">Pokud nemáte virtuální síť Site-to-Site:</span><span class="sxs-lookup"><span data-stu-id="9269b-159">If you don't have a Site-to-Site virtual network:</span></span>
1. <span data-ttu-id="9269b-160">Vytvoření svojí virtuální sítě Site-to-Site pomocí těchto pokynů: [vytvoření virtuální sítě pomocí připojení Site-to-Site VPN v hello portálu Azure Classic](vpn-gateway-site-to-site-create.md).</span><span class="sxs-lookup"><span data-stu-id="9269b-160">Create your Site-to-Site virtual network using these instructions: [Create a Virtual Network with a Site-to-Site VPN Connection in hello Azure Classic Portal](vpn-gateway-site-to-site-create.md).</span></span>  
2. <span data-ttu-id="9269b-161">Konfigurace brány dynamického směrování podle těchto pokynů: [konfigurovat bránu VPN](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="9269b-161">Configure a dynamic routing gateway using these instructions: [Configure a VPN Gateway](vpn-gateway-configure-vpn-gateway-mp.md).</span></span> <span data-ttu-id="9269b-162">Být jisti tooselect **dynamické směrování** pro váš typ brány.</span><span class="sxs-lookup"><span data-stu-id="9269b-162">Be sure tooselect **dynamic routing** for your gateway type.</span></span>

## <span data-ttu-id="9269b-163"><a name="export"></a>2. Konfigurační soubor exportu hello sítě</span><span class="sxs-lookup"><span data-stu-id="9269b-163"><a name="export"></a>2. Export hello network configuration file</span></span>
<span data-ttu-id="9269b-164">Exportujte konfiguračního souboru sítě Azure tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="9269b-164">Export your Azure network configuration file by running hello following command.</span></span> <span data-ttu-id="9269b-165">Hello umístění hello tooexport tooa jiné umístění souboru v případě potřeby můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="9269b-165">You can change hello location of hello file tooexport tooa different location if necessary.</span></span>

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-hello-network-configuration-file"></a><span data-ttu-id="9269b-166">3. Otevřete hello sítě konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="9269b-166">3. Open hello network configuration file</span></span>
<span data-ttu-id="9269b-167">Otevřete hello sítě konfigurační soubor, který jste stáhli v posledním kroku hello.</span><span class="sxs-lookup"><span data-stu-id="9269b-167">Open hello network configuration file that you downloaded in hello last step.</span></span> <span data-ttu-id="9269b-168">Pomocí editoru xml, který chcete.</span><span class="sxs-lookup"><span data-stu-id="9269b-168">Use any xml editor that you like.</span></span> <span data-ttu-id="9269b-169">Hello soubor by měl vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="9269b-169">hello file should look similar toohello following:</span></span>

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

## <a name="4-add-multiple-site-references"></a><span data-ttu-id="9269b-170">4. Přidání více odkazů na web</span><span class="sxs-lookup"><span data-stu-id="9269b-170">4. Add multiple site references</span></span>
<span data-ttu-id="9269b-171">Když přidáváte nebo odebíráte lokality referenční informace, budete provedete změny konfigurace toohello ConnectionsToLocalNetwork/LocalNetworkSiteRef.</span><span class="sxs-lookup"><span data-stu-id="9269b-171">When you add or remove site reference information, you'll make configuration changes toohello ConnectionsToLocalNetwork/LocalNetworkSiteRef.</span></span> <span data-ttu-id="9269b-172">Přidání odkazu na novou místní lokality aktivuje Azure toocreate nové tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="9269b-172">Adding a new local site reference triggers Azure toocreate a new tunnel.</span></span> <span data-ttu-id="9269b-173">V příkladu hello níže hello síťové konfigurace je pro připojení k jedné lokalitě.</span><span class="sxs-lookup"><span data-stu-id="9269b-173">In hello example below, hello network configuration is for a single-site connection.</span></span> <span data-ttu-id="9269b-174">Jakmile dokončíte provádění změny, uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="9269b-174">Save hello file once you have finished making your changes.</span></span>

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

<span data-ttu-id="9269b-175">odkazy na další lokality tooadd (vytvořit konfiguraci s více servery), jednoduše přidejte další "LocalNetworkSiteRef" řádky, jak je znázorněno v následujícím příkladu hello:</span><span class="sxs-lookup"><span data-stu-id="9269b-175">tooadd additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in hello example below:</span></span>

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-hello-network-configuration-file"></a><span data-ttu-id="9269b-176">5. Soubor importu hello síťové konfigurace</span><span class="sxs-lookup"><span data-stu-id="9269b-176">5. Import hello network configuration file</span></span>
<span data-ttu-id="9269b-177">Import hello sítě konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="9269b-177">Import hello network configuration file.</span></span> <span data-ttu-id="9269b-178">Při importování tohoto souboru se změnami hello, se přidají nové tunely hello.</span><span class="sxs-lookup"><span data-stu-id="9269b-178">When you import this file with hello changes, hello new tunnels will be added.</span></span> <span data-ttu-id="9269b-179">Hello tunely použije hello dynamickou bránu, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="9269b-179">hello tunnels will use hello dynamic gateway that you created earlier.</span></span> <span data-ttu-id="9269b-180">Můžete použít buď portálu classic hello nebo soubor hello tooimport prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9269b-180">You can either use hello classic portal, or PowerShell tooimport hello file.</span></span>

## <a name="6-download-keys"></a><span data-ttu-id="9269b-181">6. Stáhněte si klíče</span><span class="sxs-lookup"><span data-stu-id="9269b-181">6. Download keys</span></span>
<span data-ttu-id="9269b-182">Po přidání vaší nové tunely, použijte hello prostředí PowerShell rutinu 'Get-AzureVNetGatewayKey' tooget hello protokolu IPsec/IKE předsdílených klíčů pro každé tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="9269b-182">Once your new tunnels have been added, use hello PowerShell cmdlet 'Get-AzureVNetGatewayKey' tooget hello IPsec/IKE pre-shared keys for each tunnel.</span></span>

<span data-ttu-id="9269b-183">Například:</span><span class="sxs-lookup"><span data-stu-id="9269b-183">For example:</span></span>

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

<span data-ttu-id="9269b-184">Pokud dáváte přednost, můžete také použít hello *získat virtuální sítě sdílený klíč brány* REST API tooget hello předsdílených klíčů.</span><span class="sxs-lookup"><span data-stu-id="9269b-184">If you prefer, you can also use hello *Get Virtual Network Gateway Shared Key* REST API tooget hello pre-shared keys.</span></span>

## <a name="7-verify-your-connections"></a><span data-ttu-id="9269b-185">7. Zkontrolujte svá připojení</span><span class="sxs-lookup"><span data-stu-id="9269b-185">7. Verify your connections</span></span>
<span data-ttu-id="9269b-186">Zkontrolujte stav hello tunelového propojení více lokalit.</span><span class="sxs-lookup"><span data-stu-id="9269b-186">Check hello multi-site tunnel status.</span></span> <span data-ttu-id="9269b-187">Po stažení hello klíče pro každé tunelové propojení, budete muset tooverify připojení.</span><span class="sxs-lookup"><span data-stu-id="9269b-187">After downloading hello keys for each tunnel, you'll want tooverify connections.</span></span> <span data-ttu-id="9269b-188">Použijte 'Get-AzureVnetConnection' tooget tunelových propojení seznam virtuální sítě, jak je znázorněno v následujícím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="9269b-188">Use 'Get-AzureVnetConnection' tooget a list of virtual network tunnels, as shown in hello example below.</span></span> <span data-ttu-id="9269b-189">VNet1 je název hello hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9269b-189">VNet1 is hello name of hello VNet.</span></span>

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

<span data-ttu-id="9269b-190">Příklad návratový:</span><span class="sxs-lookup"><span data-stu-id="9269b-190">Example return:</span></span>

```
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site1' changed from Not Connected tooConnected.
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
    LastEventMessage          : hello connectivity state for hello local network site 'Site2' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
```

## <a name="next-steps"></a><span data-ttu-id="9269b-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9269b-191">Next steps</span></span>

<span data-ttu-id="9269b-192">toolearn Další informace o branách VPN najdete v části [informace o branách VPN](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="9269b-192">toolearn more about VPN Gateways, see [About VPN Gateways](vpn-gateway-about-vpngateways.md).</span></span>
