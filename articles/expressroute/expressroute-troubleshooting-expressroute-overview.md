---
title: "Ověřování připojení: Průvodce odstraňováním potíží s Azure ExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje pokyny na řešení problémů a ověření připojení koncová okruhu ExpressRoute."
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 5a6360b56963d219ab576fb3e2636b6c51dd72ac
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="verifying-expressroute-connectivity"></a><span data-ttu-id="15795-103">Ověření připojení ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="15795-103">Verifying ExpressRoute connectivity</span></span>
<span data-ttu-id="15795-104">ExpressRoute, které zasahuje do místní sítě do cloudu Microsoftu přes privátní připojení, které usnadňují poskytovatele připojení, zahrnuje následující tři odlišné sítě zóny:</span><span class="sxs-lookup"><span data-stu-id="15795-104">ExpressRoute, which extends an on-premises network into the Microsoft cloud over a private connection that is facilitated by a connectivity provider, involves the following three distinct network zones:</span></span>

-   <span data-ttu-id="15795-105">Své sítě.</span><span class="sxs-lookup"><span data-stu-id="15795-105">Customer Network</span></span>
-   <span data-ttu-id="15795-106">Síti poskytovatele</span><span class="sxs-lookup"><span data-stu-id="15795-106">Provider Network</span></span>
-   <span data-ttu-id="15795-107">Microsoft Datacenter</span><span class="sxs-lookup"><span data-stu-id="15795-107">Microsoft Datacenter</span></span>

<span data-ttu-id="15795-108">Účelem tohoto dokumentu je pomoct uživatelům určete, kam (nebo i v případě) existuje problém s připojením a v rámci které zónu, a tím k vyhledání pomoc od týmu vhodné k vyřešení problému.</span><span class="sxs-lookup"><span data-stu-id="15795-108">The purpose of this document is to help user to identify where (or even if) a connectivity issue exists and within which zone, thereby to seek help from appropriate team to resolve the issue.</span></span> <span data-ttu-id="15795-109">Pokud vyřešíte problém, je potřeba podporu společnosti Microsoft, otevřete lístek podpory s [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="15795-109">If Microsoft support is needed to resolve an issue, open a support ticket with [Microsoft Support][Support].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="15795-110">Tento dokument je určený k diagnostikování a opravě jednoduché problémy.</span><span class="sxs-lookup"><span data-stu-id="15795-110">This document is intended to help diagnosing and fixing simple issues.</span></span> <span data-ttu-id="15795-111">Rozhraní není určeno k jako náhrada za podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="15795-111">It is not intended to be a replacement for Microsoft support.</span></span> <span data-ttu-id="15795-112">Otevřete lístek podpory s [Microsoft Support] [ Support] nejde pomocí pokynů uvedených problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="15795-112">Open a support ticket with [Microsoft Support][Support] if you are unable to solve the problem using the guidance provided.</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="15795-113">Přehled</span><span class="sxs-lookup"><span data-stu-id="15795-113">Overview</span></span>
<span data-ttu-id="15795-114">Následující diagram znázorňuje připojení logické sítě zákazníka k síti Microsoftu pomocí ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="15795-114">The following diagram shows the logical connectivity of a customer network to Microsoft network using ExpressRoute.</span></span>
<span data-ttu-id="15795-115">[![1]][1]</span><span class="sxs-lookup"><span data-stu-id="15795-115">[![1]][1]</span></span>

<span data-ttu-id="15795-116">Na předchozím obrázku označují čísla bodů klíče sítě.</span><span class="sxs-lookup"><span data-stu-id="15795-116">In the preceding diagram, the numbers indicate key network points.</span></span> <span data-ttu-id="15795-117">Body sítě jsou odkazovány často prostřednictvím tohoto článku podle jejich přidružené čísla.</span><span class="sxs-lookup"><span data-stu-id="15795-117">The network points are referenced often through this article by their associated number.</span></span>

<span data-ttu-id="15795-118">V závislosti na modelu připojení ExpressRoute (cloudu Exchange společné umístění, připojení Ethernet typu Point-to-Point nebo Any-to-any (IPVPN)) může být body sítě 3 a 4 přepínače (vrstvy 2 zařízení).</span><span class="sxs-lookup"><span data-stu-id="15795-118">Depending on the ExpressRoute connectivity model (Cloud Exchange Co-location, Point-to-Point Ethernet Connection, or Any-to-any (IPVPN)) the network points 3 and 4 may be switches (Layer 2 devices).</span></span> <span data-ttu-id="15795-119">Ilustrovaný body klíče sítě jsou následující:</span><span class="sxs-lookup"><span data-stu-id="15795-119">The key network points illustrated are as follows:</span></span>

1.  <span data-ttu-id="15795-120">Zákazník výpočetní zařízení (například server nebo počítače)</span><span class="sxs-lookup"><span data-stu-id="15795-120">Customer compute device (for example, a server or PC)</span></span>
2.  <span data-ttu-id="15795-121">Webovou službu zápis certifikátů: Hraniční směrovače zákazníka</span><span class="sxs-lookup"><span data-stu-id="15795-121">CEs: Customer edge routers</span></span> 
3.  <span data-ttu-id="15795-122">PEs (CE přístupem): zprostředkovatele hraniční směrovače nebo přepínače, které stojí hraniční směrovače zákazníka.</span><span class="sxs-lookup"><span data-stu-id="15795-122">PEs (CE facing): Provider edge routers/switches that are facing customer edge routers.</span></span> <span data-ttu-id="15795-123">Označuje jako PE CEs v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="15795-123">Referred to as PE-CEs in this document.</span></span>
4.  <span data-ttu-id="15795-124">PEs (MSEE přístupem): zprostředkovatele hraniční směrovače nebo přepínače, které stojí Msee.</span><span class="sxs-lookup"><span data-stu-id="15795-124">PEs (MSEE facing): Provider edge routers/switches that are facing MSEEs.</span></span> <span data-ttu-id="15795-125">Označuje jako PE Msee v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="15795-125">Referred to as PE-MSEEs in this document.</span></span>
5.  <span data-ttu-id="15795-126">Msee: Microsoft Edge Enterprise (MSEE) ExpressRoute směrovači</span><span class="sxs-lookup"><span data-stu-id="15795-126">MSEEs: Microsoft Enterprise Edge (MSEE) ExpressRoute routers</span></span>
6.  <span data-ttu-id="15795-127">Brána virtuální sítě (VNet)</span><span class="sxs-lookup"><span data-stu-id="15795-127">Virtual Network (VNet) Gateway</span></span>
7.  <span data-ttu-id="15795-128">Výpočetní zařízení na virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="15795-128">Compute device on the Azure VNet</span></span>

<span data-ttu-id="15795-129">Pokud používáte společné umístění Exchange cloudu nebo připojení k síti Ethernet typu Point-to-Point modelů připojení, by vytvořit hraniční směrovač zákazníka (2) s Msee (5) partnerského vztahu protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="15795-129">If the Cloud Exchange Co-location or Point-to-Point Ethernet Connection connectivity models are used, the customer edge router (2) would establish BGP peering with MSEEs (5).</span></span> <span data-ttu-id="15795-130">Bodů sítě 3 a 4 by stále existují, ale poněkud transparentní jako zařízení vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="15795-130">Network points 3 and 4 would still exist but be somewhat transparent as Layer 2 devices.</span></span>

<span data-ttu-id="15795-131">Pokud se používá model připojení Any-to-any (IPVPN), odkaz PEs (MSEE přístupem) (4) by navázat s Msee (5) partnerského vztahu protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="15795-131">If the Any-to-any (IPVPN) connectivity model is used, the PEs (MSEE facing) (4) would establish BGP peering with MSEEs (5).</span></span> <span data-ttu-id="15795-132">Zpátky k síti zákazníka prostřednictvím síti poskytovatele služeb IPVPN by pak rozšíří trasy.</span><span class="sxs-lookup"><span data-stu-id="15795-132">Routes would then propagate back to the customer network via the IPVPN service provider network.</span></span>

>[!NOTE]
><span data-ttu-id="15795-133">Pro zajištění vysoké dostupnosti ExpressRoute vyžaduje Microsoft redundantní dvojici relací protokolu BGP mezi Msee (5) a PE-Msee (4).</span><span class="sxs-lookup"><span data-stu-id="15795-133">For ExpressRoute high availability, Microsoft requires a redundant pair of BGP sessions between MSEEs (5) and PE-MSEEs (4).</span></span> <span data-ttu-id="15795-134">Redundantní dvojici síťových cest je také podporována mezi síti zákazníka a PE CEs.</span><span class="sxs-lookup"><span data-stu-id="15795-134">A redundant pair of network paths is also encouraged between customer network and PE-CEs.</span></span> <span data-ttu-id="15795-135">Model připojení Any-to-any (IPVPN), může být připojena jedno zařízení CE (2) na jeden nebo více PEs (3).</span><span class="sxs-lookup"><span data-stu-id="15795-135">However, in Any-to-any (IPVPN) connection model, a single CE device (2) may be connected to one or more PEs (3).</span></span>
>
>

<span data-ttu-id="15795-136">Ověření okruhu ExpressRoute, následující kroky jsou popsané (s bodu sítě indikován přidružená čísla):</span><span class="sxs-lookup"><span data-stu-id="15795-136">To validate an ExpressRoute circuit, the following steps are covered (with the network point indicated by the associated number):</span></span>
1. [<span data-ttu-id="15795-137">Ověření zřizování okruhů a stavu (5)</span><span class="sxs-lookup"><span data-stu-id="15795-137">Validate circuit provisioning and state (5)</span></span>](#validate-circuit-provisioning-and-state)
2. [<span data-ttu-id="15795-138">Ověření alespoň jeden ExpressRoute partnerského vztahu je nakonfigurovaný (5)</span><span class="sxs-lookup"><span data-stu-id="15795-138">Validate at least one ExpressRoute peering is configured (5)</span></span>](#validate-peering-configuration)
3. [<span data-ttu-id="15795-139">Ověření protokolu ARP mezi společností Microsoft a služby zprostředkovatele (propojení mezi 4 a 5)</span><span class="sxs-lookup"><span data-stu-id="15795-139">Validate ARP between Microsoft and the service provider (link between 4 and 5)</span></span>](#validate-arp-between-microsoft-and-the-service-provider)
4. [<span data-ttu-id="15795-140">Ověření protokolu BGP a trasy na MSEE (protokol BGP až 4 až 5, 5 až 6, pokud je připojený virtuální sítě)</span><span class="sxs-lookup"><span data-stu-id="15795-140">Validate BGP and routes on the MSEE (BGP between 4 to 5, and 5 to 6 if a VNet is connected)</span></span>](#validate-bgp-and-routes-on-the-msee)
5. [<span data-ttu-id="15795-141">Zkontrolujte statistiku provozu (provoz procházející 5)</span><span class="sxs-lookup"><span data-stu-id="15795-141">Check the Traffic Statistics (Traffic passing through 5)</span></span>](#check-the-traffic-statistics)

<span data-ttu-id="15795-142">Další ověření a kontroly bude přidána v budoucí, vraťte měsíčně!</span><span class="sxs-lookup"><span data-stu-id="15795-142">More validations and checks will be added in the future, check back monthly!</span></span>

##<a name="validate-circuit-provisioning-and-state"></a><span data-ttu-id="15795-143">Zřizování okruhů a stavu ověření</span><span class="sxs-lookup"><span data-stu-id="15795-143">Validate circuit provisioning and state</span></span>
<span data-ttu-id="15795-144">Bez ohledu na modelu připojení je potřeba vytvořit okruh ExpressRoute a proto vygenerované zřizování okruhů klíč služby.</span><span class="sxs-lookup"><span data-stu-id="15795-144">Regardless of the connectivity model, an ExpressRoute circuit has to be created and thus a service key generated for circuit provisioning.</span></span> <span data-ttu-id="15795-145">Zřizování okruh ExpressRoute vytváří redundantní připojení vrstvy 2 mezi PE-Msee (4) a Msee (5).</span><span class="sxs-lookup"><span data-stu-id="15795-145">Provisioning an ExpressRoute circuit establishes a redundant Layer 2 connections between PE-MSEEs (4) and MSEEs (5).</span></span> <span data-ttu-id="15795-146">Další informace o tom, jak vytvořit, upravit, poskytnout a ověřit okruh ExpressRoute najdete v článku [vytvoření a úprava okruhu ExpressRoute][CreateCircuit].</span><span class="sxs-lookup"><span data-stu-id="15795-146">For more information on how to create, modify, provision, and verify an ExpressRoute circuit, see the article [Create and modify an ExpressRoute circuit][CreateCircuit].</span></span>

>[!TIP]
><span data-ttu-id="15795-147">Klíč služby jednoznačně identifikuje okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="15795-147">A service key uniquely identifies an ExpressRoute circuit.</span></span> <span data-ttu-id="15795-148">Tento klíč je požadován pro většinu příkazů prostředí powershell uvedených v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="15795-148">This key is required for most of the powershell commands mentioned in this document.</span></span> <span data-ttu-id="15795-149">Taky byste potřebovali pomoc od Microsoftu nebo od partnera chcete vyřešit nějaký problém ExpressRoute, zadejte klíč služby snadno identifikovat okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="15795-149">Also, should you need assistance from Microsoft or from an ExpressRoute partner to troubleshoot an ExpressRoute issue, provide the service key to readily identify the circuit.</span></span>
>
>

###<a name="verification-via-the-azure-portal"></a><span data-ttu-id="15795-150">Ověření prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="15795-150">Verification via the Azure portal</span></span>
<span data-ttu-id="15795-151">Na portálu Azure stav okruhu ExpressRoute můžete zkontrolovat výběrem ![2][2] v nabídce vlevo. straně panelu a potom vyberete okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="15795-151">In the Azure portal, the status of an ExpressRoute circuit can be checked by selecting ![2][2] on the left-side-bar menu and then selecting the ExpressRoute circuit.</span></span> <span data-ttu-id="15795-152">Výběr ExpressRoute okruhu uvedené v části "Všechny prostředky" otevře okno okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="15795-152">Selecting an ExpressRoute circuit listed under "All resources" opens the ExpressRoute circuit blade.</span></span> <span data-ttu-id="15795-153">V ![3][3] části okna, ExpressRoute essentials jsou uvedeny, jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="15795-153">In the ![3][3] section of the blade, the ExpressRoute essentials are listed as shown in the following screen shot:</span></span>

<span data-ttu-id="15795-154">![4][4]</span><span class="sxs-lookup"><span data-stu-id="15795-154">![4][4]</span></span>    

<span data-ttu-id="15795-155">V ExpressRoute Essentials *okruhu stav* označuje stav okruhu na straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="15795-155">In the ExpressRoute Essentials, *Circuit status* indicates the status of the circuit on the Microsoft side.</span></span> <span data-ttu-id="15795-156">*Stav zprostředkovatele* označuje, zda došlo ke okruhu *zajištěno nebo není zřízený* na straně poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="15795-156">*Provider status* indicates if the circuit has been *Provisioned/Not provisioned* on the service-provider side.</span></span> 

<span data-ttu-id="15795-157">Pro okruh ExpressRoute do provozu *okruhu stav* musí být *povoleno* a *stav zprostředkovatele* musí být *zajištěno*.</span><span class="sxs-lookup"><span data-stu-id="15795-157">For an ExpressRoute circuit to be operational, the *Circuit status* must be *Enabled* and the *Provider status* must be *Provisioned*.</span></span>

>[!NOTE]
><span data-ttu-id="15795-158">Pokud *okruhu stav* není povoleno, obraťte se na [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="15795-158">If the *Circuit status* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="15795-159">Pokud *stav zprostředkovatele* není zajišťováno, obraťte se na svého poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="15795-159">If the *Provider status* is not provisioned, contact your service provider.</span></span>
>
>

###<a name="verification-via-powershell"></a><span data-ttu-id="15795-160">Ověření pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="15795-160">Verification via PowerShell</span></span>
<span data-ttu-id="15795-161">K zobrazení seznamu všech okruhy ExpressRoute ve skupině prostředků, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15795-161">To list all the ExpressRoute circuits in a Resource Group, use the following command:</span></span>

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
><span data-ttu-id="15795-162">Název skupiny prostředků můžete získat prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="15795-162">You can get your resource group name through the Azure portal.</span></span> <span data-ttu-id="15795-163">Viz předchozí část tohoto dokumentu a Všimněte si, že je název skupiny prostředků uvedené v na snímku obrazovky příklad.</span><span class="sxs-lookup"><span data-stu-id="15795-163">See the previous subsection of this document and note that the resource group name is listed in the example screen shot.</span></span>
>
>

<span data-ttu-id="15795-164">Pokud chcete vybrat konkrétní okruh ExpressRoute ve skupině prostředků, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15795-164">To select a particular ExpressRoute circuit in a Resource Group, use the following command:</span></span>

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

<span data-ttu-id="15795-165">Ukázková odpověď je:</span><span class="sxs-lookup"><span data-stu-id="15795-165">A sample response is:</span></span>

    Name                             : Test-ER-Ckt
    ResourceGroupName                : Test-ER-RG
    Location                         : westus2
    Id                               : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                        "Name": "Standard_UnlimitedData",
                                        "Tier": "Standard",
                                        "Family": "UnlimitedData"
                                        }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             : 
    ServiceProviderProperties        : {
                                        "ServiceProviderName": "****",
                                        "PeeringLocation": "******",
                                        "BandwidthInMbps": 100
                                        }
    ServiceKey                       : **************************************
    Peerings                         : []
    Authorizations                   : []

<span data-ttu-id="15795-166">Pokud chcete potvrdit, pokud okruh ExpressRoute je funkční, věnujte zvláštní pozornost následující pole:</span><span class="sxs-lookup"><span data-stu-id="15795-166">To confirm if an ExpressRoute circuit is operational, pay particular attention to the following fields:</span></span>

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

>[!NOTE]
><span data-ttu-id="15795-167">Pokud *CircuitProvisioningState* není povoleno, obraťte se na [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="15795-167">If the *CircuitProvisioningState* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="15795-168">Pokud *ServiceProviderProvisioningState* není zajišťováno, obraťte se na svého poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="15795-168">If the *ServiceProviderProvisioningState* is not provisioned, contact your service provider.</span></span>
>
>

###<a name="verification-via-powershell-classic"></a><span data-ttu-id="15795-169">Ověření pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="15795-169">Verification via PowerShell (Classic)</span></span>
<span data-ttu-id="15795-170">K zobrazení seznamu všech okruhy ExpressRoute v rámci předplatného, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15795-170">To list all the ExpressRoute circuits under a subscription, use the following command:</span></span>

    Get-AzureDedicatedCircuit

<span data-ttu-id="15795-171">Pokud chcete vybrat konkrétní okruh ExpressRoute, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15795-171">To select a particular ExpressRoute circuit, use the following command:</span></span>

    Get-AzureDedicatedCircuit -ServiceKey **************************************

<span data-ttu-id="15795-172">Ukázková odpověď je:</span><span class="sxs-lookup"><span data-stu-id="15795-172">A sample response is:</span></span>

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="15795-173">Abyste si potvrdili, pokud je provozní okruh ExpressRoute, věnovat zvláštní pozornost následující pole: ServiceProviderProvisioningState: zřízený stav: povoleno</span><span class="sxs-lookup"><span data-stu-id="15795-173">To confirm if an ExpressRoute circuit is operational, pay particular attention to the following fields: ServiceProviderProvisioningState : Provisioned Status                           : Enabled</span></span>

>[!NOTE]
><span data-ttu-id="15795-174">Pokud *stav* není povoleno, obraťte se na [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="15795-174">If the *Status* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="15795-175">Pokud *ServiceProviderProvisioningState* není zajišťováno, obraťte se na svého poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="15795-175">If the *ServiceProviderProvisioningState* is not provisioned, contact your service provider.</span></span>
>
>

##<a name="validate-peering-configuration"></a><span data-ttu-id="15795-176">Ověření konfigurace partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="15795-176">Validate Peering Configuration</span></span>
<span data-ttu-id="15795-177">Po dokončení zřizování okruh ExpressRoute poskytovatele služeb konfigurace směrování lze vytvořit nad rámec okruhu ExpressRoute mezi MSEE-PRs (4) a Msee (5).</span><span class="sxs-lookup"><span data-stu-id="15795-177">After the service provider has completed the provisioning the ExpressRoute circuit, a routing configuration can be created over the ExpressRoute circuit between MSEE-PRs (4) and MSEEs (5).</span></span> <span data-ttu-id="15795-178">Každý okruh ExpressRoute může mít jednu, dvě nebo tři směrování kontexty povoleno: soukromý partnerský vztah Azure (provoz v Azure virtuální privátní sítě), veřejný partnerský vztah Azure (provoz na veřejné IP adresy v Azure) a partnerský vztah Microsoftu (provoz do služeb Office 365 a Dynamics 365).</span><span class="sxs-lookup"><span data-stu-id="15795-178">Each ExpressRoute circuit can have one, two, or three routing contexts enabled: Azure private peering (traffic to private virtual networks in Azure), Azure public peering (traffic to public IP addresses in Azure), and Microsoft peering (traffic to Office 365 and Dynamics 365).</span></span> <span data-ttu-id="15795-179">Další informace o tom, jak vytvořit a upravit konfigurace směrování, najdete v článku [vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="15795-179">For more information on how to create and modify routing configuration, see the article [Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>

###<a name="verification-via-the-azure-portal"></a><span data-ttu-id="15795-180">Ověření prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="15795-180">Verification via the Azure portal</span></span>
>[!IMPORTANT]
><span data-ttu-id="15795-181">Na portálu Azure, ve kterém jsou partnerských vztahů ExpressRoute je známých chyba *není* uvedené na portálu, pokud nakonfigurovat pomocí poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="15795-181">There is a known bug in the Azure portal wherein ExpressRoute peerings are *NOT* shown in the portal if configured by the service provider.</span></span> <span data-ttu-id="15795-182">Přidání partnerských vztahů ExpressRoute prostřednictvím portálu nebo prostředí PowerShell *přepíše nastavení poskytovatele služby*.</span><span class="sxs-lookup"><span data-stu-id="15795-182">Adding ExpressRoute peerings via the portal or PowerShell *overwrites the service provider settings*.</span></span> <span data-ttu-id="15795-183">Tato akce naruší směrování v okruhu ExpressRoute a vyžaduje podporu poskytovatele služeb k obnovení nastavení a obnovit normální směrování.</span><span class="sxs-lookup"><span data-stu-id="15795-183">This action breaks the routing on the ExpressRoute circuit and requires the support of the service provider to restore the settings and reestablish normal routing.</span></span> <span data-ttu-id="15795-184">Pokud je jisté, že poskytovatele služeb poskytuje pouze služby vrstvy 2, změnit pouze partnerských vztahů ExpressRoute!</span><span class="sxs-lookup"><span data-stu-id="15795-184">Only modify the ExpressRoute peerings if it is certain that the service provider is providing layer 2 services only!</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="15795-185">Pokud vrstvy 3 je poskytované poskytovatelem služby a partnerských vztahů jsou prázdné na portálu, prostředí PowerShell umožňuje najdete v části Nastavení služby zprostředkovatele nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="15795-185">If layer 3 is provided by the service provider and the peerings are blank in the portal, PowerShell can be used to see the service provider configured settings.</span></span>
>
>

<span data-ttu-id="15795-186">Na portálu Azure můžete zkontrolovat stav okruhu ExpressRoute výběrem ![2][2] v nabídce vlevo. straně panelu a potom vyberete okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="15795-186">In the Azure portal, status of an ExpressRoute circuit can be checked by selecting ![2][2] on the left-side-bar menu and then selecting the ExpressRoute circuit.</span></span> <span data-ttu-id="15795-187">Výběr ExpressRoute okruhu uvedené v části "Všechny prostředky" se otevřou v okně okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="15795-187">Selecting an ExpressRoute circuit listed under "All resources" would open the ExpressRoute circuit blade.</span></span> <span data-ttu-id="15795-188">V ![3][3] části okna, ExpressRoute essentials by byly uvedeny, jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="15795-188">In the ![3][3] section of the blade, the ExpressRoute essentials would be listed as shown in the following screen shot:</span></span>

<span data-ttu-id="15795-189">![5][5]</span><span class="sxs-lookup"><span data-stu-id="15795-189">![5][5]</span></span>

<span data-ttu-id="15795-190">V předchozím příkladu jako uvedené Azure soukromého partnerského vztahu směrování kontextu je povoleno, zatímco veřejný Azure a kontexty směrování partnerského vztahu Microsoftu nejsou povolené.</span><span class="sxs-lookup"><span data-stu-id="15795-190">In the preceding example, as noted Azure private peering routing context is enabled, whereas Azure public and Microsoft peering routing contexts are not enabled.</span></span> <span data-ttu-id="15795-191">Úspěšně povolilo partnerského vztahu kontextu by měla mít také podsítě primární a sekundární point-to-point (povinné pro protokol BGP) uvedené.</span><span class="sxs-lookup"><span data-stu-id="15795-191">A successfully enabled peering context would also have the primary and secondary point-to-point (required for BGP) subnets listed.</span></span> <span data-ttu-id="15795-192">/ 30 podsítě se používají pro IP adresu rozhraní Msee a PE Msee.</span><span class="sxs-lookup"><span data-stu-id="15795-192">The /30 subnets are used for the interface IP address of the MSEEs and PE-MSEEs.</span></span> 

>[!NOTE]
><span data-ttu-id="15795-193">Pokud partnerský vztah není povolena, zkontrolujte, pokud primární a sekundární podsítě, které jsou přiřazené odpovídat konfiguraci PE Msee.</span><span class="sxs-lookup"><span data-stu-id="15795-193">If a peering is not enabled, check if the primary and secondary subnets assigned match the configuration on PE-MSEEs.</span></span> <span data-ttu-id="15795-194">Pokud není, chcete-li změnit konfiguraci na směrovači MSEE, podívejte se na [vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering]</span><span class="sxs-lookup"><span data-stu-id="15795-194">If not, to change the configuration on MSEE routers, refer to [Create and modify routing for an ExpressRoute circuit][CreatePeering]</span></span>
>
>

###<a name="verification-via-powershell"></a><span data-ttu-id="15795-195">Ověření pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="15795-195">Verification via PowerShell</span></span>
<span data-ttu-id="15795-196">Chcete-li podrobností konfigurace partnerského vztahu Azure privátní, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="15795-196">To get the Azure private peering configuration details, use the following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt

<span data-ttu-id="15795-197">Ukázková odpověď, úspěšně nakonfigurovaný soukromého partnerského vztahu, je:</span><span class="sxs-lookup"><span data-stu-id="15795-197">A sample response, for a successfully configured private peering, is:</span></span>

    Name                       : AzurePrivatePeering
    Id                         : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt/peerings/AzurePrivatePeering
    Etag                       : W/"################################"
    PeeringType                : AzurePrivatePeering
    AzureASN                   : 12076
    PeerASN                    : ####
    PrimaryPeerAddressPrefix   : 172.16.0.0/30
    SecondaryPeerAddressPrefix : 172.16.0.4/30
    PrimaryAzurePort           : 
    SecondaryAzurePort         : 
    SharedKey                  : 
    VlanId                     : 300
    MicrosoftPeeringConfig     : null
    ProvisioningState          : Succeeded

 <span data-ttu-id="15795-198">Úspěšně povolilo partnerského vztahu kontextu by měla mít uvedené primární a sekundární předpony.</span><span class="sxs-lookup"><span data-stu-id="15795-198">A successfully enabled peering context would have the primary and secondary address prefixes listed.</span></span> <span data-ttu-id="15795-199">/ 30 podsítě se používají pro IP adresu rozhraní Msee a PE Msee.</span><span class="sxs-lookup"><span data-stu-id="15795-199">The /30 subnets are used for the interface IP address of the MSEEs and PE-MSEEs.</span></span>

<span data-ttu-id="15795-200">Podrobnosti konfigurace partnerského vztahu Azure veřejné získáte pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="15795-200">To get the Azure public peering configuration details, use the following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt

<span data-ttu-id="15795-201">Konfigurace podrobností partnerského vztahu Microsoftu získáte pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="15795-201">To get the Microsoft peering configuration details, use the following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt

<span data-ttu-id="15795-202">Pokud není nakonfigurováno partnerský vztah, by chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="15795-202">If a peering is not configured, there would be an error message.</span></span> <span data-ttu-id="15795-203">Ukázka odpověď, když stanovené partnerského vztahu (Azure veřejný partnerský vztah v tomto příkladu) není nakonfigurovaný v rámci okruhu:</span><span class="sxs-lookup"><span data-stu-id="15795-203">A sample response, when the stated peering (Azure Public peering in this example) is not configured within the circuit:</span></span>

    Get-AzureRmExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzureRmExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


<p/>
>[!NOTE]
><span data-ttu-id="15795-204">Pokud partnerský vztah není povolena, zkontrolujte, pokud primární a sekundární podsítě, které jsou přiřazené odpovídat konfiguraci propojené PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="15795-204">If a peering is not enabled, check if the primary and secondary subnets assigned match the configuration on the linked PE-MSEE.</span></span> <span data-ttu-id="15795-205">Také zkontrolujte, zda správný *VlanId*, *AzureASN*, a *PeerASN* se používají na Msee a pokud tyto hodnoty se mapuje na ty, které slouží na propojené PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="15795-205">Also check if the correct *VlanId*, *AzureASN*, and *PeerASN* are used on MSEEs and if these values maps to the ones used on the linked PE-MSEE.</span></span> <span data-ttu-id="15795-206">Pokud zvolíte použití algoritmu hash MD5, by měla být stejná na dvojice MSEE a PE MSEE sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="15795-206">If MD5 hashing is chosen, the shared key should be same on MSEE and PE-MSEE pair.</span></span> <span data-ttu-id="15795-207">Změnit konfiguraci na směrovači MSEE, najdete v tématu [vytvoření a úprava směrování pro okruh ExpressRoute] [CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="15795-207">To change the configuration on the MSEE routers, refer to [Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>  
>
>

### <a name="verification-via-powershell-classic"></a><span data-ttu-id="15795-208">Ověření pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="15795-208">Verification via PowerShell (Classic)</span></span>
<span data-ttu-id="15795-209">K získání podrobností konfigurace partnerského vztahu Azure privátní, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15795-209">To get the Azure private peering configuration details, use the following command:</span></span>

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

<span data-ttu-id="15795-210">Ukázková odpověď, úspěšně nakonfigurovaný soukromého partnerského vztahu je:</span><span class="sxs-lookup"><span data-stu-id="15795-210">A sample response, for a successfully configured private peering is:</span></span>

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : ####
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100

<span data-ttu-id="15795-211">Úspěšně, povolená A partnerského vztahu kontextu by měla mít podsítě primárního a sekundárního partnera uvedené.</span><span class="sxs-lookup"><span data-stu-id="15795-211">A successfully, enabled peering context would have the primary and secondary peer subnets listed.</span></span> <span data-ttu-id="15795-212">/ 30 podsítě se používají pro IP adresu rozhraní Msee a PE Msee.</span><span class="sxs-lookup"><span data-stu-id="15795-212">The /30 subnets are used for the interface IP address of the MSEEs and PE-MSEEs.</span></span>

<span data-ttu-id="15795-213">Podrobnosti konfigurace partnerského vztahu Azure veřejné získáte pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="15795-213">To get the Azure public peering configuration details, use the following commands:</span></span>

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

<span data-ttu-id="15795-214">Konfigurace podrobností partnerského vztahu Microsoftu získáte pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="15795-214">To get the Microsoft peering configuration details, use the following commands:</span></span>

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

>[!IMPORTANT]
><span data-ttu-id="15795-215">Pokud vrstvy 3 partnerských vztahů byly nastavené zásadami poskytovatele služeb, nastavení partnerských vztahů ExpressRoute prostřednictvím portálu nebo prostředí PowerShell přepíše nastavení poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="15795-215">If layer 3 peerings were set by the service provider, setting the ExpressRoute peerings via the portal or PowerShell overwrites the service provider settings.</span></span> <span data-ttu-id="15795-216">Resetování nastavení partnerského vztahu straně zprostředkovatele vyžaduje podporu poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="15795-216">Resetting the provider side peering settings requires the support of the service provider.</span></span> <span data-ttu-id="15795-217">Pokud je jisté, že poskytovatele služeb poskytuje pouze služby vrstvy 2, změnit pouze partnerských vztahů ExpressRoute!</span><span class="sxs-lookup"><span data-stu-id="15795-217">Only modify the ExpressRoute peerings if it is certain that the service provider is providing layer 2 services only!</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="15795-218">Pokud partnerský vztah není povolena, zkontrolujte, pokud primární a sekundární sdílené podsítě přiřadit odpovídat konfiguraci propojené PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="15795-218">If a peering is not enabled, check if the primary and secondary peer subnets assigned match the configuration on the linked PE-MSEE.</span></span> <span data-ttu-id="15795-219">Také zkontrolujte, zda správný *VlanId*, *AzureAsn*, a *PeerAsn* se používají na Msee a pokud tyto hodnoty se mapuje na ty, které slouží na propojené PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="15795-219">Also check if the correct *VlanId*, *AzureAsn*, and *PeerAsn* are used on MSEEs and if these values maps to the ones used on the linked PE-MSEE.</span></span> <span data-ttu-id="15795-220">Změnit konfiguraci na směrovači MSEE, najdete v tématu [vytvoření a úprava směrování pro okruh ExpressRoute] [CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="15795-220">To change the configuration on the MSEE routers, refer to [Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>
>
>

## <a name="validate-arp-between-microsoft-and-the-service-provider"></a><span data-ttu-id="15795-221">Ověření protokolu ARP mezi společností Microsoft a poskytovatele služeb</span><span class="sxs-lookup"><span data-stu-id="15795-221">Validate ARP between Microsoft and the service provider</span></span>
<span data-ttu-id="15795-222">Tato část používá příkazy prostředí PowerShell (klasické).</span><span class="sxs-lookup"><span data-stu-id="15795-222">This section uses PowerShell (Classic) commands.</span></span> <span data-ttu-id="15795-223">Pokud používáte příkazy prostředí PowerShell Azure Resource Manager, ujistěte se, zda máte přístup správce nebo spolusprávce k předplatnému přes [portál Azure classic][OldPortal].</span><span class="sxs-lookup"><span data-stu-id="15795-223">If you have been using PowerShell Azure Resource Manager commands, ensure that you have admin/co-admin access to the subscription via [Azure classic portal][OldPortal].</span></span> <span data-ttu-id="15795-224">Řešení potíží s pomocí Azure Resource Manager příkazy naleznete [získávání ARP tabulky v modelu nasazení Resource Manager] [ ARP] dokumentu.</span><span class="sxs-lookup"><span data-stu-id="15795-224">For troubleshooting using Azure Resource Manager commands please refer to the [Getting ARP tables in the Resource Manager deployment model][ARP] document.</span></span>

>[!NOTE]
><span data-ttu-id="15795-225">Chcete-li získat protokolu ARP, lze použít portál Azure a příkazy prostředí PowerShell Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="15795-225">To get ARP, both the Azure portal and Azure Resource Manager PowerShell commands can be used.</span></span> <span data-ttu-id="15795-226">Pokud k chybám pomocí příkazů prostředí PowerShell Azure Resource Manager, classic příkazy prostředí PowerShell by měly fungovat jako Classic PowerShell příkazy spolupracovat taky s nástrojem okruhy ExpressRoute Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="15795-226">If errors are encountered with the Azure Resource Manager PowerShell commands, classic PowerShell commands should work as Classic PowerShell commands also work with Azure Resource Manager ExpressRoute circuits.</span></span>
>
>

<span data-ttu-id="15795-227">Základě tabulky ARP z primární směrovači MSEE pro soukromý partnerský vztah, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15795-227">To get the ARP table from the primary MSEE router for the private peering, use the following command:</span></span>

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="15795-228">Odpověď příklad pro příkaz, v tomto scénáři úspěšná:</span><span class="sxs-lookup"><span data-stu-id="15795-228">An example response for the command, in the successful scenario:</span></span>

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

<span data-ttu-id="15795-229">Podobně můžete zkontrolovat tabulky ARP z MSEE v *primární*/*sekundární* cestu, pro *privátní*/*veřejné*  / *Microsoft* partnerských vztahů.</span><span class="sxs-lookup"><span data-stu-id="15795-229">Similarly, you can check the ARP table from the MSEE in the *Primary*/*Secondary* path, for *Private*/*Public*/*Microsoft* peerings.</span></span>

<span data-ttu-id="15795-230">Následující příklad ukazuje, že odpověď příkazu pro partnerský vztah neexistuje.</span><span class="sxs-lookup"><span data-stu-id="15795-230">The following example shows the response of the command for a peering does not exist.</span></span>

    ARP Info:
       
>[!NOTE]
><span data-ttu-id="15795-231">Pokud základě tabulky ARP nemá IP adresy rozhraní namapované na adresy MAC, projděte si následující informace:</span><span class="sxs-lookup"><span data-stu-id="15795-231">If the ARP table does not have IP addresses of the interfaces mapped to MAC addresses, review the following information:</span></span>
>1. <span data-ttu-id="15795-232">Pokud první IP adresa/30 podsítě přiřazené pro propojení mezi MSEE PR a MSEE se používá na rozhraní MSEE PR.</span><span class="sxs-lookup"><span data-stu-id="15795-232">If the first IP address of the /30 subnet assigned for the link between the MSEE-PR and MSEE is used on the interface of MSEE-PR.</span></span> <span data-ttu-id="15795-233">Azure vždy používá druhou IP adresu pro Msee.</span><span class="sxs-lookup"><span data-stu-id="15795-233">Azure always uses the second IP address for MSEEs.</span></span>
>2. <span data-ttu-id="15795-234">Ověřte, pokud zákazník (C-Tag) a značky VLAN služby (S-Tag) odpovídají na dvojice MSEE PR a MSEE.</span><span class="sxs-lookup"><span data-stu-id="15795-234">Verify if the customer (C-Tag) and service (S-Tag) VLAN tags match both on MSEE-PR and MSEE pair.</span></span>
>
>

## <a name="validate-bgp-and-routes-on-the-msee"></a><span data-ttu-id="15795-235">Ověření protokolu BGP a tras MSEE</span><span class="sxs-lookup"><span data-stu-id="15795-235">Validate BGP and routes on the MSEE</span></span>
<span data-ttu-id="15795-236">Tato část používá příkazy prostředí PowerShell (klasické).</span><span class="sxs-lookup"><span data-stu-id="15795-236">This section uses PowerShell (Classic) commands.</span></span> <span data-ttu-id="15795-237">Pokud používáte příkazy prostředí PowerShell Azure Resource Manager, ujistěte se, zda máte přístup správce nebo spolusprávce k předplatnému přes [portál Azure classic][OldPortal]</span><span class="sxs-lookup"><span data-stu-id="15795-237">If you have been using PowerShell Azure Resource Manager commands, ensure that you have admin/co-admin access to the subscription via [Azure classic portal][OldPortal]</span></span>

>[!NOTE]
><span data-ttu-id="15795-238">Získat informace o protokolu BGP, můžete použít portál Azure a příkazy prostředí PowerShell Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="15795-238">To get BGP information, both the Azure portal and Azure Resource Manager PowerShell commands can be used.</span></span> <span data-ttu-id="15795-239">Pokud k chybám pomocí příkazů prostředí PowerShell Azure Resource Manager, classic příkazy prostředí PowerShell by měly fungovat jako classic PowerShell příkazy spolupracovat taky s nástrojem okruhy ExpressRoute Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="15795-239">If errors are encountered with the Azure Resource Manager PowerShell commands, classic PowerShell commands should work as classic PowerShell commands also work with Azure Resource Manager ExpressRoute circuits.</span></span>
>
>

<span data-ttu-id="15795-240">Postup získání shrnutí tabulce směrování (BGP sousedním) pro konkrétní směrování kontext, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15795-240">To get the routing table (BGP neighbor) summary for a particular routing context, use the following command:</span></span>

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="15795-241">Odpověď příklad je:</span><span class="sxs-lookup"><span data-stu-id="15795-241">An example response is:</span></span>

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

<span data-ttu-id="15795-242">Jak je znázorněno v předchozím příkladu, příkaz je užitečné k určení, jak dlouho směrování kontextu bylo úspěšně navázáno.</span><span class="sxs-lookup"><span data-stu-id="15795-242">As shown in the preceding example, the command is useful to determine for how long the routing context has been established.</span></span> <span data-ttu-id="15795-243">Označuje také počet trasy předpon inzerovaných směrovači partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="15795-243">It also indicates number of route prefixes advertised by the peering router.</span></span>

>[!NOTE]
><span data-ttu-id="15795-244">Pokud stav není v aktivní a nečinnosti, zkontrolujte, pokud primární a sekundární sdílené podsítě přiřadit odpovídat konfiguraci propojené PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="15795-244">If the state is in Active or Idle, check if the primary and secondary peer subnets assigned match the configuration on the linked PE-MSEE.</span></span> <span data-ttu-id="15795-245">Také zkontrolujte, zda správný *VlanId*, *AzureAsn*, a *PeerAsn* se používají na Msee a pokud tyto hodnoty se mapuje na ty, které slouží na propojené PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="15795-245">Also check if the correct *VlanId*, *AzureAsn*, and *PeerAsn* are used on MSEEs and if these values maps to the ones used on the linked PE-MSEE.</span></span> <span data-ttu-id="15795-246">Pokud zvolíte použití algoritmu hash MD5, by měla být stejná na dvojice MSEE a PE MSEE sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="15795-246">If MD5 hashing is chosen, the shared key should be same on MSEE and PE-MSEE pair.</span></span> <span data-ttu-id="15795-247">Chcete-li změnit konfiguraci na směrovači MSEE, přečtěte [vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="15795-247">To change the configuration on the MSEE routers, refer to [Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="15795-248">Pokud některá místa určení není dostupný přes konkrétní partnerský vztah, zkontrolujte tabulky trasy Msee patřící do určité partnerského vztahu kontextu.</span><span class="sxs-lookup"><span data-stu-id="15795-248">If certain destinations are not reachable over a particular peering, check the route table of the MSEEs belonging to the particular peering context.</span></span> <span data-ttu-id="15795-249">Pokud odpovídající předpony (můžou být NATed IP) se nachází ve směrovací tabulce, potom zkontrolujte, zda existují brány firewall nebo nebo seznamy ACL skupiny NSG na cestě a v případě, že povolují provoz.</span><span class="sxs-lookup"><span data-stu-id="15795-249">If a matching prefix (could be NATed IP) is present in the routing table, then check if there are firewalls/NSG/ACLs on the path and if they permit the traffic.</span></span>
>
>

<span data-ttu-id="15795-250">Chcete-li získat úplné směrovací tabulky z MSEE *primární* cestu pro konkrétní *privátní* směrování kontextu, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15795-250">To get the full routing table from MSEE on the *Primary* path for the particular *Private* routing context, use the following command:</span></span>

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="15795-251">Je úspěšnému výsledku příklad pro příkaz:</span><span class="sxs-lookup"><span data-stu-id="15795-251">An example successful outcome for the command is:</span></span>

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

<span data-ttu-id="15795-252">Podobně můžete zkontrolovat z MSEE ve směrovací tabulce *primární*/*sekundární* cestu, pro *privátní* /  *Veřejné*/*Microsoft* partnerského vztahu kontextu.</span><span class="sxs-lookup"><span data-stu-id="15795-252">Similarly, you can check the routing table from the MSEE in the *Primary*/*Secondary* path, for *Private*/*Public*/*Microsoft* a peering context.</span></span>

<span data-ttu-id="15795-253">Následující příklad ukazuje, že odpověď příkazu pro partnerský vztah neexistuje:</span><span class="sxs-lookup"><span data-stu-id="15795-253">The following example shows the response of the command for a peering does not exist:</span></span>

    Route Table Info:

##<a name="check-the-traffic-statistics"></a><span data-ttu-id="15795-254">Zkontrolujte statistiky provozu</span><span class="sxs-lookup"><span data-stu-id="15795-254">Check the Traffic Statistics</span></span>
<span data-ttu-id="15795-255">Chcete-li získat statistiku provozu kombinované primární a sekundární cesta – bajtů a odhlašování – partnerského vztahu kontextu, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15795-255">To get the combined primary and secondary path traffic statistics--bytes in and out--of a peering context, use the following command:</span></span>

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

<span data-ttu-id="15795-256">Ukázkový výstup příkazu je:</span><span class="sxs-lookup"><span data-stu-id="15795-256">A sample output of the command is:</span></span>

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

<span data-ttu-id="15795-257">Ukázkový výstup příkazu pro neexistující partnerský vztah je:</span><span class="sxs-lookup"><span data-stu-id="15795-257">A sample output of the command for a non-existent peering is:</span></span>

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a><span data-ttu-id="15795-258">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15795-258">Next Steps</span></span>
<span data-ttu-id="15795-259">Další informace a nápovědu najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="15795-259">For more information or help, check out the following links:</span></span>

- <span data-ttu-id="15795-260">[Podporu společnosti Microsoft][Support]</span><span class="sxs-lookup"><span data-stu-id="15795-260">[Microsoft Support][Support]</span></span>
- <span data-ttu-id="15795-261">[Vytvoření a úprava okruhu ExpressRoute][CreateCircuit]</span><span class="sxs-lookup"><span data-stu-id="15795-261">[Create and modify an ExpressRoute circuit][CreateCircuit]</span></span>
- <span data-ttu-id="15795-262">[Vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering]</span><span class="sxs-lookup"><span data-stu-id="15795-262">[Create and modify routing for an ExpressRoute circuit][CreatePeering]</span></span>

<!--Image References-->
<span data-ttu-id="15795-263">[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "Připojení k logické Express Route"</span><span class="sxs-lookup"><span data-stu-id="15795-263">[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "Logical Express Route Connectivity"</span></span>
<span data-ttu-id="15795-264">[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Ikona všechny prostředky"</span><span class="sxs-lookup"><span data-stu-id="15795-264">[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "All resources icon"</span></span>
<span data-ttu-id="15795-265">[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Ikona – přehled"</span><span class="sxs-lookup"><span data-stu-id="15795-265">[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Overview icon"</span></span>
<span data-ttu-id="15795-266">[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "Snímek obrazovky ukázkové ExpressRoute Essentials"</span><span class="sxs-lookup"><span data-stu-id="15795-266">[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "ExpressRoute Essentials sample screenshot"</span></span>
<span data-ttu-id="15795-267">[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "Snímek obrazovky ukázkové ExpressRoute Essentials"</span><span class="sxs-lookup"><span data-stu-id="15795-267">[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "ExpressRoute Essentials sample screenshot"</span></span>

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[OldPortal]: https://manage.windowsazure.com
[ARP]: https://docs.microsoft.com/en-us/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






