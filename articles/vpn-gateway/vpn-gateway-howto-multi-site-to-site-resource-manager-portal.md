---
title: "Přidání více VPN brána Site-to-Site připojení tooa virtuální sítě: portálu Azure: Resource Manager | Microsoft Docs"
description: "Přidání více lokalit S2S připojení tooa VPN gateway, který má existující připojení"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: cherylmc
ms.openlocfilehash: b8c9ff454967f509dcef725f8bcec8564fad9b00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection"></a><span data-ttu-id="356dd-103">Přidat tooa připojení Site-to-Site virtuální síť s existujícím připojením VPN gateway</span><span class="sxs-lookup"><span data-stu-id="356dd-103">Add a Site-to-Site connection tooa VNet with an existing VPN gateway connection</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="356dd-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="356dd-104">Azure portal</span></span>](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="356dd-105">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="356dd-105">PowerShell (classic)</span></span>](vpn-gateway-multi-site.md)
>
> 

<span data-ttu-id="356dd-106">Tento článek vás provede pomocí hello Azure tooadd portálu Site-to-Site (S2S) připojení tooa VPN bránu, která má existující připojení.</span><span class="sxs-lookup"><span data-stu-id="356dd-106">This article walks you through using hello Azure portal tooadd Site-to-Site (S2S) connections tooa VPN gateway that has an existing connection.</span></span> <span data-ttu-id="356dd-107">Tento typ připojení je často označují tooas "s více servery" konfigurace.</span><span class="sxs-lookup"><span data-stu-id="356dd-107">This type of connection is often referred tooas a "multi-site" configuration.</span></span> <span data-ttu-id="356dd-108">Můžete přidat tooa připojení S2S virtuální sítě, který už má připojení S2S, připojení Point-to-Site nebo připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="356dd-108">You can add a S2S connection tooa VNet that already has a S2S connection, Point-to-Site connection, or VNet-to-VNet connection.</span></span> <span data-ttu-id="356dd-109">Při přidávání připojení existují určitá omezení.</span><span class="sxs-lookup"><span data-stu-id="356dd-109">There are some limitations when adding connections.</span></span> <span data-ttu-id="356dd-110">Zkontrolujte hello [před zahájením](#before) část v tooverify Tento článek před zahájením konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="356dd-110">Check hello [Before you begin](#before) section in this article tooverify before you start your configuration.</span></span> 

<span data-ttu-id="356dd-111">Tento článek se týká tooVNets vytvořené pomocí modelu nasazení Resource Manager hello, které mají brána sítě VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="356dd-111">This article applies tooVNets created using hello Resource Manager deployment model that have a RouteBased VPN gateway.</span></span> <span data-ttu-id="356dd-112">Tyto kroky se nevztahují konfigurace koexistujících připojení tooExpressRoute/Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="356dd-112">These steps do not apply tooExpressRoute/Site-to-Site coexisting connection configurations.</span></span> <span data-ttu-id="356dd-113">V tématu [koexistujících připojení ExpressRoute nebo S2S](../expressroute/expressroute-howto-coexist-resource-manager.md) informace o připojení, která.</span><span class="sxs-lookup"><span data-stu-id="356dd-113">See [ExpressRoute/S2S coexisting connections](../expressroute/expressroute-howto-coexist-resource-manager.md) for information about coexisting connections.</span></span>

### <a name="deployment-models-and-methods"></a><span data-ttu-id="356dd-114">Modely a metody nasazení</span><span class="sxs-lookup"><span data-stu-id="356dd-114">Deployment models and methods</span></span>
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="356dd-115">Tuto tabulku aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="356dd-115">We update this table as new articles and additional tools become available for this configuration.</span></span> <span data-ttu-id="356dd-116">Když je článek k dispozici, jsme odkaz přímo tooit z této tabulky.</span><span class="sxs-lookup"><span data-stu-id="356dd-116">When an article is available, we link directly tooit from this table.</span></span>

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <span data-ttu-id="356dd-117"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="356dd-117"><a name="before"></a>Before you begin</span></span>
<span data-ttu-id="356dd-118">Ověřte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="356dd-118">Verify hello following items:</span></span>

* <span data-ttu-id="356dd-119">Nejsou vytvoření koexistujících připojení ExpressRoute nebo S2S.</span><span class="sxs-lookup"><span data-stu-id="356dd-119">You are not creating an ExpressRoute/S2S coexisting connection.</span></span>
* <span data-ttu-id="356dd-120">Máte virtuální síť, která byla vytvořena pomocí modelu nasazení Resource Manager hello s existujícím připojením.</span><span class="sxs-lookup"><span data-stu-id="356dd-120">You have a virtual network that was created using hello Resource Manager deployment model with an existing connection.</span></span>
* <span data-ttu-id="356dd-121">Hello brány virtuální sítě pro virtuální síť je RouteBased.</span><span class="sxs-lookup"><span data-stu-id="356dd-121">hello virtual network gateway for your VNet is RouteBased.</span></span> <span data-ttu-id="356dd-122">Pokud máte bránu PolicyBased VPN, musíte odstranit bránu virtuální sítě hello a vytvořit novou bránu VPN jako RouteBased.</span><span class="sxs-lookup"><span data-stu-id="356dd-122">If you have a PolicyBased VPN gateway, you must delete hello virtual network gateway and create a new VPN gateway as RouteBased.</span></span>
* <span data-ttu-id="356dd-123">Žádný z rozsahů adres hello nepřekrývá pro všechny virtuální sítě, který tuto virtuální síť se připojuje k hello.</span><span class="sxs-lookup"><span data-stu-id="356dd-123">None of hello address ranges overlap for any of hello VNets that this VNet is connecting to.</span></span>
* <span data-ttu-id="356dd-124">Máte kompatibilní zařízení VPN a někoho, kdo je možné tooconfigure ho.</span><span class="sxs-lookup"><span data-stu-id="356dd-124">You have compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="356dd-125">Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="356dd-125">See [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span> <span data-ttu-id="356dd-126">Pokud si nejste obeznámeni s konfiguraci zařízení VPN nebo nejste obeznámeni s rozsahy IP adres hello umístěné ve vaší konfiguraci místní sítě, musíte toocoordinate s někým, kdo může pro vás jsou tyto podrobnosti obsaženy.</span><span class="sxs-lookup"><span data-stu-id="356dd-126">If you aren't familiar with configuring your VPN device, or are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span>
* <span data-ttu-id="356dd-127">Máte zvenčí veřejnou IP adresu pro vaše zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="356dd-127">You have an externally facing public IP address for your VPN device.</span></span> <span data-ttu-id="356dd-128">Tato IP adresa nesmí být umístěná za překladem adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="356dd-128">This IP address cannot be located behind a NAT.</span></span>

## <span data-ttu-id="356dd-129"><a name="part1"></a>Část 1: Konfigurace připojení</span><span class="sxs-lookup"><span data-stu-id="356dd-129"><a name="part1"></a>Part 1 - Configure a connection</span></span>
1. <span data-ttu-id="356dd-130">V prohlížeči přejděte toohello [portál Azure](http://portal.azure.com) a v případě potřeby, přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="356dd-130">From a browser, navigate toohello [Azure portal](http://portal.azure.com) and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="356dd-131">Klikněte na tlačítko **všechny prostředky** a vyhledejte vaše **brány virtuální sítě** hello seznamu prostředků a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="356dd-131">Click **All resources** and locate your **virtual network gateway** from hello list of resources and click it.</span></span>
3. <span data-ttu-id="356dd-132">Na hello **Brána virtuální sítě** okně klikněte na tlačítko **připojení**.</span><span class="sxs-lookup"><span data-stu-id="356dd-132">On hello **Virtual network gateway** blade, click **Connections**.</span></span>
   
    <span data-ttu-id="356dd-133">![Okno Připojení](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Okno Připojení")</span><span class="sxs-lookup"><span data-stu-id="356dd-133">![Connections blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")</span></span><br>
4. <span data-ttu-id="356dd-134">Na hello **připojení** okně klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="356dd-134">On hello **Connections** blade, click **+Add**.</span></span>
   
    <span data-ttu-id="356dd-135">![Tlačítko Přidat připojení](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "tlačítko Přidat připojení")</span><span class="sxs-lookup"><span data-stu-id="356dd-135">![Add connection button](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")</span></span><br>
5. <span data-ttu-id="356dd-136">Na hello **přidat připojení** okně výplně out hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="356dd-136">On hello **Add connection** blade, fill out hello following fields:</span></span>
   
   * <span data-ttu-id="356dd-137">**Název:** toogive toohello webový server při vytváření připojení hello k název hello.</span><span class="sxs-lookup"><span data-stu-id="356dd-137">**Name:** hello name you want toogive toohello site you are creating hello connection to.</span></span>
   * <span data-ttu-id="356dd-138">**Typ připojení:** vyberte **Site-to-site (IPsec)**.</span><span class="sxs-lookup"><span data-stu-id="356dd-138">**Connection type:** Select **Site-to-site (IPsec)**.</span></span>
     
     <span data-ttu-id="356dd-139">![Přidat připojení okno](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "okno Přidat připojení")</span><span class="sxs-lookup"><span data-stu-id="356dd-139">![Add connection blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")</span></span><br>

## <span data-ttu-id="356dd-140"><a name="part2"></a>Část 2 – přidání brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="356dd-140"><a name="part2"></a>Part 2 - Add a local network gateway</span></span>
1. <span data-ttu-id="356dd-141">Klikněte na tlačítko **brány místní sítě** ***zvolit bránu místní sítě***.</span><span class="sxs-lookup"><span data-stu-id="356dd-141">Click **Local network gateway** ***Choose a local network gateway***.</span></span> <span data-ttu-id="356dd-142">Otevře se hello **brány místní sítě vyberte** okno.</span><span class="sxs-lookup"><span data-stu-id="356dd-142">This will open hello **Choose local network gateway** blade.</span></span>
   
    <span data-ttu-id="356dd-143">![Zvolte bránu místní sítě](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "vyberte bránu místní sítě")</span><span class="sxs-lookup"><span data-stu-id="356dd-143">![Choose local network gateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")</span></span><br>
2. <span data-ttu-id="356dd-144">Klikněte na tlačítko **vytvořit nový** tooopen hello **vytvořit bránu místní sítě** okno.</span><span class="sxs-lookup"><span data-stu-id="356dd-144">Click **Create new** tooopen hello **Create local network gateway** blade.</span></span>
   
    <span data-ttu-id="356dd-145">![Okno brány místní sítě vytvořit](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "vytvořit bránu místní sítě")</span><span class="sxs-lookup"><span data-stu-id="356dd-145">![Create local network gateway blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")</span></span><br>
3. <span data-ttu-id="356dd-146">Na hello **vytvořit bránu místní sítě** okně výplně out hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="356dd-146">On hello **Create local network gateway** blade, fill out hello following fields:</span></span>
   
   * <span data-ttu-id="356dd-147">**Název:** hello název prostředku brány místní sítě toohello toogive.</span><span class="sxs-lookup"><span data-stu-id="356dd-147">**Name:** hello name you want toogive toohello local network gateway resource.</span></span>
   * <span data-ttu-id="356dd-148">**IP adresa:** hello veřejná IP adresa zařízení VPN hello na hello lokalitě, která chcete tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="356dd-148">**IP address:** hello public IP address of hello VPN device on hello site that you want tooconnect to.</span></span>
   * <span data-ttu-id="356dd-149">**Adresní prostor:** hello adresního prostoru, které chcete toobe směrovány toohello nové místní síťový Web.</span><span class="sxs-lookup"><span data-stu-id="356dd-149">**Address space:** hello address space that you want toobe routed toohello new local network site.</span></span>
4. <span data-ttu-id="356dd-150">Klikněte na tlačítko **OK** na hello **vytvořit bránu místní sítě** okno toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="356dd-150">Click **OK** on hello **Create local network gateway** blade toosave hello changes.</span></span>

## <span data-ttu-id="356dd-151"><a name="part3"></a>Část 3 – přidat sdílený klíč hello a vytvoření připojení hello</span><span class="sxs-lookup"><span data-stu-id="356dd-151"><a name="part3"></a>Part 3 - Add hello shared key and create hello connection</span></span>
1. <span data-ttu-id="356dd-152">Na hello **přidat připojení** okně Přidat hello sdílený klíč, které mají toouse toocreate připojení.</span><span class="sxs-lookup"><span data-stu-id="356dd-152">On hello **Add connection** blade, add hello shared key that you want toouse toocreate your connection.</span></span> <span data-ttu-id="356dd-153">Můžete buď získat sdílený klíč hello z vašeho zařízení VPN, nebo si ho vytvořit tady a pak nakonfigurujte vaší hello toouse zařízení VPN stejný sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="356dd-153">You can either get hello shared key from your VPN device, or make one up here and then configure your VPN device toouse hello same shared key.</span></span> <span data-ttu-id="356dd-154">Důležité je co přesně se klíče hello Hello hello stejné.</span><span class="sxs-lookup"><span data-stu-id="356dd-154">hello important thing is that hello keys are exactly hello same.</span></span>
   
    <span data-ttu-id="356dd-155">![Sdílený klíč](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Sdílený klíč")</span><span class="sxs-lookup"><span data-stu-id="356dd-155">![Shared key](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")</span></span><br>
2. <span data-ttu-id="356dd-156">Hello dolní části okna hello, klikněte na **OK** toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="356dd-156">At hello bottom of hello blade, click **OK** toocreate hello connection.</span></span>

## <span data-ttu-id="356dd-157"><a name="part4"></a>Součástí 4 – ověření připojení VPN hello</span><span class="sxs-lookup"><span data-stu-id="356dd-157"><a name="part4"></a>Part 4 - Verify hello VPN connection</span></span>


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="356dd-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="356dd-158">Next steps</span></span>

<span data-ttu-id="356dd-159">Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="356dd-159">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="356dd-160">V tématu hello virtuální počítače [kurzů](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) Další informace.</span><span class="sxs-lookup"><span data-stu-id="356dd-160">See hello virtual machines [learning path](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) for more information.</span></span>
