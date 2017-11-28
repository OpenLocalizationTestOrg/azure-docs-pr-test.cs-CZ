---
title: "Ověřování připojení: Průvodce odstraňováním potíží s Azure ExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje pokyny na řešení problémů a ověření připojení tooend end okruhu ExpressRoute."
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
ms.openlocfilehash: 713c39c7eafd77a4380b2a91902a9686f2ce1d85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="verifying-expressroute-connectivity"></a><span data-ttu-id="1657c-103">Ověření připojení ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="1657c-103">Verifying ExpressRoute connectivity</span></span>
<span data-ttu-id="1657c-104">ExpressRoute, které zasahuje do místní sítě do hello cloudu Microsoftu přes privátní připojení, které usnadňují poskytovatele připojení, zahrnuje následující tři odlišné sítě zóny hello:</span><span class="sxs-lookup"><span data-stu-id="1657c-104">ExpressRoute, which extends an on-premises network into hello Microsoft cloud over a private connection that is facilitated by a connectivity provider, involves hello following three distinct network zones:</span></span>

-   <span data-ttu-id="1657c-105">Své sítě.</span><span class="sxs-lookup"><span data-stu-id="1657c-105">Customer Network</span></span>
-   <span data-ttu-id="1657c-106">Síti poskytovatele</span><span class="sxs-lookup"><span data-stu-id="1657c-106">Provider Network</span></span>
-   <span data-ttu-id="1657c-107">Microsoft Datacenter</span><span class="sxs-lookup"><span data-stu-id="1657c-107">Microsoft Datacenter</span></span>

<span data-ttu-id="1657c-108">Hello účelem tohoto dokumentu je toohelp uživatele tooidentify kde (nebo i v případě) existuje problém s připojením a v rámci které zónu, a tím tooseek nápovědy odpovídající team tooresolve hello problém.</span><span class="sxs-lookup"><span data-stu-id="1657c-108">hello purpose of this document is toohelp user tooidentify where (or even if) a connectivity issue exists and within which zone, thereby tooseek help from appropriate team tooresolve hello issue.</span></span> <span data-ttu-id="1657c-109">Pokud podporu společnosti Microsoft je potřebné tooresolve problém, otevřete lístek podpory s [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="1657c-109">If Microsoft support is needed tooresolve an issue, open a support ticket with [Microsoft Support][Support].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1657c-110">Tento dokument je určený toohelp diagnostikovat a odstraňovat problémy jednoduché.</span><span class="sxs-lookup"><span data-stu-id="1657c-110">This document is intended toohelp diagnosing and fixing simple issues.</span></span> <span data-ttu-id="1657c-111">Není určený toobe náhradní server pro podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1657c-111">It is not intended toobe a replacement for Microsoft support.</span></span> <span data-ttu-id="1657c-112">Otevřete lístek podpory s [Microsoft Support] [ Support] Pokud jste problém hello nelze toosolve pomocí pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="1657c-112">Open a support ticket with [Microsoft Support][Support] if you are unable toosolve hello problem using hello guidance provided.</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="1657c-113">Přehled</span><span class="sxs-lookup"><span data-stu-id="1657c-113">Overview</span></span>
<span data-ttu-id="1657c-114">Hello následující diagram znázorňuje hello připojení logické sítě tooMicrosoft sítě zákazníka pomocí ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="1657c-114">hello following diagram shows hello logical connectivity of a customer network tooMicrosoft network using ExpressRoute.</span></span>
<span data-ttu-id="1657c-115">[![1]][1]</span><span class="sxs-lookup"><span data-stu-id="1657c-115">[![1]][1]</span></span>

<span data-ttu-id="1657c-116">V předchozím diagramu hello hello čísla označují bodů klíče sítě.</span><span class="sxs-lookup"><span data-stu-id="1657c-116">In hello preceding diagram, hello numbers indicate key network points.</span></span> <span data-ttu-id="1657c-117">Hello bodů sítě jsou odkazovány často prostřednictvím tohoto článku podle jejich přidružené číslo.</span><span class="sxs-lookup"><span data-stu-id="1657c-117">hello network points are referenced often through this article by their associated number.</span></span>

<span data-ttu-id="1657c-118">V závislosti na připojení ExpressRoute hello modelu (cloudu Exchange společné umístění, připojení Ethernet typu Point-to-Point nebo Any-to-any (IPVPN)) hello bodů sítě 3 a 4 může být přepínače (vrstvy 2 zařízení).</span><span class="sxs-lookup"><span data-stu-id="1657c-118">Depending on hello ExpressRoute connectivity model (Cloud Exchange Co-location, Point-to-Point Ethernet Connection, or Any-to-any (IPVPN)) hello network points 3 and 4 may be switches (Layer 2 devices).</span></span> <span data-ttu-id="1657c-119">Ilustrovaný bodů Hello klíče sítě jsou následující:</span><span class="sxs-lookup"><span data-stu-id="1657c-119">hello key network points illustrated are as follows:</span></span>

1.  <span data-ttu-id="1657c-120">Zákazník výpočetní zařízení (například server nebo počítače)</span><span class="sxs-lookup"><span data-stu-id="1657c-120">Customer compute device (for example, a server or PC)</span></span>
2.  <span data-ttu-id="1657c-121">Webovou službu zápis certifikátů: Hraniční směrovače zákazníka</span><span class="sxs-lookup"><span data-stu-id="1657c-121">CEs: Customer edge routers</span></span> 
3.  <span data-ttu-id="1657c-122">PEs (CE přístupem): zprostředkovatele hraniční směrovače nebo přepínače, které stojí hraniční směrovače zákazníka.</span><span class="sxs-lookup"><span data-stu-id="1657c-122">PEs (CE facing): Provider edge routers/switches that are facing customer edge routers.</span></span> <span data-ttu-id="1657c-123">Označuje tooas PE CEs v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1657c-123">Referred tooas PE-CEs in this document.</span></span>
4.  <span data-ttu-id="1657c-124">PEs (MSEE přístupem): zprostředkovatele hraniční směrovače nebo přepínače, které stojí Msee.</span><span class="sxs-lookup"><span data-stu-id="1657c-124">PEs (MSEE facing): Provider edge routers/switches that are facing MSEEs.</span></span> <span data-ttu-id="1657c-125">Označuje tooas PE Msee v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1657c-125">Referred tooas PE-MSEEs in this document.</span></span>
5.  <span data-ttu-id="1657c-126">Msee: Microsoft Edge Enterprise (MSEE) ExpressRoute směrovači</span><span class="sxs-lookup"><span data-stu-id="1657c-126">MSEEs: Microsoft Enterprise Edge (MSEE) ExpressRoute routers</span></span>
6.  <span data-ttu-id="1657c-127">Brána virtuální sítě (VNet)</span><span class="sxs-lookup"><span data-stu-id="1657c-127">Virtual Network (VNet) Gateway</span></span>
7.  <span data-ttu-id="1657c-128">Výpočetní zařízení na hello virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="1657c-128">Compute device on hello Azure VNet</span></span>

<span data-ttu-id="1657c-129">Pokud se používají modelů připojení cloudu Exchange společné umístění nebo připojení k síti Ethernet typu Point-to-Point hello, by vytvořit hraniční směrovač zákazníka hello (2) s Msee (5) partnerského vztahu protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="1657c-129">If hello Cloud Exchange Co-location or Point-to-Point Ethernet Connection connectivity models are used, hello customer edge router (2) would establish BGP peering with MSEEs (5).</span></span> <span data-ttu-id="1657c-130">Bodů sítě 3 a 4 by stále existují, ale poněkud transparentní jako zařízení vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="1657c-130">Network points 3 and 4 would still exist but be somewhat transparent as Layer 2 devices.</span></span>

<span data-ttu-id="1657c-131">Pokud se používá model připojení hello Any-to-any (IPVPN), hello PEs (MSEE přístupem) (4) by navázat s Msee (5) partnerského vztahu protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="1657c-131">If hello Any-to-any (IPVPN) connectivity model is used, hello PEs (MSEE facing) (4) would establish BGP peering with MSEEs (5).</span></span> <span data-ttu-id="1657c-132">Směrování by pak rozšíří back toohello zákazníka sítě prostřednictvím síti poskytovatele služeb IPVPN hello.</span><span class="sxs-lookup"><span data-stu-id="1657c-132">Routes would then propagate back toohello customer network via hello IPVPN service provider network.</span></span>

>[!NOTE]
><span data-ttu-id="1657c-133">Pro zajištění vysoké dostupnosti ExpressRoute vyžaduje Microsoft redundantní dvojici relací protokolu BGP mezi Msee (5) a PE-Msee (4).</span><span class="sxs-lookup"><span data-stu-id="1657c-133">For ExpressRoute high availability, Microsoft requires a redundant pair of BGP sessions between MSEEs (5) and PE-MSEEs (4).</span></span> <span data-ttu-id="1657c-134">Redundantní dvojici síťových cest je také podporována mezi síti zákazníka a PE CEs.</span><span class="sxs-lookup"><span data-stu-id="1657c-134">A redundant pair of network paths is also encouraged between customer network and PE-CEs.</span></span> <span data-ttu-id="1657c-135">Však v modelu připojení Any-to-any (IPVPN), jeden CE (2) může být zařízení připojená tooone nebo další PEs (3).</span><span class="sxs-lookup"><span data-stu-id="1657c-135">However, in Any-to-any (IPVPN) connection model, a single CE device (2) may be connected tooone or more PEs (3).</span></span>
>
>

<span data-ttu-id="1657c-136">toovalidate okruh ExpressRoute, hello následující kroky jsou popsané (s bodu sítě hello indikován hello související číslo):</span><span class="sxs-lookup"><span data-stu-id="1657c-136">toovalidate an ExpressRoute circuit, hello following steps are covered (with hello network point indicated by hello associated number):</span></span>
1. [<span data-ttu-id="1657c-137">Ověření zřizování okruhů a stavu (5)</span><span class="sxs-lookup"><span data-stu-id="1657c-137">Validate circuit provisioning and state (5)</span></span>](#validate-circuit-provisioning-and-state)
2. [<span data-ttu-id="1657c-138">Ověření alespoň jeden ExpressRoute partnerského vztahu je nakonfigurovaný (5)</span><span class="sxs-lookup"><span data-stu-id="1657c-138">Validate at least one ExpressRoute peering is configured (5)</span></span>](#validate-peering-configuration)
3. [<span data-ttu-id="1657c-139">Ověření protokolu ARP mezi poskytovatele služeb společnosti Microsoft a hello (propojení mezi 4 a 5)</span><span class="sxs-lookup"><span data-stu-id="1657c-139">Validate ARP between Microsoft and hello service provider (link between 4 and 5)</span></span>](#validate-arp-between-microsoft-and-the-service-provider)
4. [<span data-ttu-id="1657c-140">Ověření protokolu BGP a trasy na hello MSEE (BGP mezi 4 too5 a 5 too6, pokud je připojený virtuální sítě)</span><span class="sxs-lookup"><span data-stu-id="1657c-140">Validate BGP and routes on hello MSEE (BGP between 4 too5, and 5 too6 if a VNet is connected)</span></span>](#validate-bgp-and-routes-on-the-msee)
5. [<span data-ttu-id="1657c-141">Zkontrolujte hello statistiku provozu (provoz procházející 5)</span><span class="sxs-lookup"><span data-stu-id="1657c-141">Check hello Traffic Statistics (Traffic passing through 5)</span></span>](#check-the-traffic-statistics)

<span data-ttu-id="1657c-142">Další ověření a kontroly přidá v hello zpět budoucí, zkontrolujte měsíčně!</span><span class="sxs-lookup"><span data-stu-id="1657c-142">More validations and checks will be added in hello future, check back monthly!</span></span>

##<a name="validate-circuit-provisioning-and-state"></a><span data-ttu-id="1657c-143">Zřizování okruhů a stavu ověření</span><span class="sxs-lookup"><span data-stu-id="1657c-143">Validate circuit provisioning and state</span></span>
<span data-ttu-id="1657c-144">Bez ohledu na to hello modelu připojení, okruh ExpressRoute má toobe vytvořili a proto služby klíč vygenerovaný pro zřizování okruhů.</span><span class="sxs-lookup"><span data-stu-id="1657c-144">Regardless of hello connectivity model, an ExpressRoute circuit has toobe created and thus a service key generated for circuit provisioning.</span></span> <span data-ttu-id="1657c-145">Zřizování okruh ExpressRoute vytváří redundantní připojení vrstvy 2 mezi PE-Msee (4) a Msee (5).</span><span class="sxs-lookup"><span data-stu-id="1657c-145">Provisioning an ExpressRoute circuit establishes a redundant Layer 2 connections between PE-MSEEs (4) and MSEEs (5).</span></span> <span data-ttu-id="1657c-146">Další informace o tom, jak toocreate, upravit, poskytnout a ověřit okruh ExpressRoute najdete v článku hello [vytvoření a úprava okruhu ExpressRoute][CreateCircuit].</span><span class="sxs-lookup"><span data-stu-id="1657c-146">For more information on how toocreate, modify, provision, and verify an ExpressRoute circuit, see hello article [Create and modify an ExpressRoute circuit][CreateCircuit].</span></span>

>[!TIP]
><span data-ttu-id="1657c-147">Klíč služby jednoznačně identifikuje okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="1657c-147">A service key uniquely identifies an ExpressRoute circuit.</span></span> <span data-ttu-id="1657c-148">Tento klíč je požadován pro většinu příkazů prostředí powershell hello uvedených v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1657c-148">This key is required for most of hello powershell commands mentioned in this document.</span></span> <span data-ttu-id="1657c-149">Taky byste potřebovali pomoc od Microsoftu nebo od tootroubleshoot partnera ExpressRoute problém ExpressRoute poskytovat službu hello klíče tooreadily identifikovat hello okruh.</span><span class="sxs-lookup"><span data-stu-id="1657c-149">Also, should you need assistance from Microsoft or from an ExpressRoute partner tootroubleshoot an ExpressRoute issue, provide hello service key tooreadily identify hello circuit.</span></span>
>
>

###<a name="verification-via-hello-azure-portal"></a><span data-ttu-id="1657c-150">Ověření prostřednictvím hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1657c-150">Verification via hello Azure portal</span></span>
<span data-ttu-id="1657c-151">V hello portálu Azure, hello stav okruhu ExpressRoute můžete zkontrolovat výběrem ![2][2] na hello nabídce vlevo. straně panelu a potom vyberete hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="1657c-151">In hello Azure portal, hello status of an ExpressRoute circuit can be checked by selecting ![2][2] on hello left-side-bar menu and then selecting hello ExpressRoute circuit.</span></span> <span data-ttu-id="1657c-152">Výběr ExpressRoute okruhu uvedené v části "Všechny prostředky" otevře se okno okruh ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="1657c-152">Selecting an ExpressRoute circuit listed under "All resources" opens hello ExpressRoute circuit blade.</span></span> <span data-ttu-id="1657c-153">V hello ![3][3] části hello okně hello ExpressRoute essentials jsou uvedeny, jak ukazuje následující snímek obrazovky hello:</span><span class="sxs-lookup"><span data-stu-id="1657c-153">In hello ![3][3] section of hello blade, hello ExpressRoute essentials are listed as shown in hello following screen shot:</span></span>

<span data-ttu-id="1657c-154">![4][4]</span><span class="sxs-lookup"><span data-stu-id="1657c-154">![4][4]</span></span>    

<span data-ttu-id="1657c-155">V hello ExpressRoute Essentials *okruhu stav* označuje hello stav okruhu hello na hello straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1657c-155">In hello ExpressRoute Essentials, *Circuit status* indicates hello status of hello circuit on hello Microsoft side.</span></span> <span data-ttu-id="1657c-156">*Stav zprostředkovatele* označuje, zda došlo ke hello okruh *zajištěno nebo není zřízený* na straně hello poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="1657c-156">*Provider status* indicates if hello circuit has been *Provisioned/Not provisioned* on hello service-provider side.</span></span> 

<span data-ttu-id="1657c-157">ExpressRoute okruhu toobe provozní, hello *okruhu stav* musí být *povoleno* a hello *stav zprostředkovatele* musí být *zajištěno*.</span><span class="sxs-lookup"><span data-stu-id="1657c-157">For an ExpressRoute circuit toobe operational, hello *Circuit status* must be *Enabled* and hello *Provider status* must be *Provisioned*.</span></span>

>[!NOTE]
><span data-ttu-id="1657c-158">Pokud hello *okruhu stav* není povoleno, obraťte se na [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="1657c-158">If hello *Circuit status* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="1657c-159">Pokud hello *stav zprostředkovatele* není zajišťováno, obraťte se na svého poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="1657c-159">If hello *Provider status* is not provisioned, contact your service provider.</span></span>
>
>

###<a name="verification-via-powershell"></a><span data-ttu-id="1657c-160">Ověření pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1657c-160">Verification via PowerShell</span></span>
<span data-ttu-id="1657c-161">toolist všechny hello okruhy ExpressRoute ve skupině prostředků, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1657c-161">toolist all hello ExpressRoute circuits in a Resource Group, use hello following command:</span></span>

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
><span data-ttu-id="1657c-162">Název skupiny prostředků můžete získat prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1657c-162">You can get your resource group name through hello Azure portal.</span></span> <span data-ttu-id="1657c-163">Zobrazit hello předchozí část tohoto dokumentu a Všimněte si, že tento název skupiny prostředků hello je uvedena ve snímku obrazovky příklad hello.</span><span class="sxs-lookup"><span data-stu-id="1657c-163">See hello previous subsection of this document and note that hello resource group name is listed in hello example screen shot.</span></span>
>
>

<span data-ttu-id="1657c-164">tooselect konkrétní okruh ExpressRoute ve skupině prostředků, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1657c-164">tooselect a particular ExpressRoute circuit in a Resource Group, use hello following command:</span></span>

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

<span data-ttu-id="1657c-165">Ukázková odpověď je:</span><span class="sxs-lookup"><span data-stu-id="1657c-165">A sample response is:</span></span>

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

<span data-ttu-id="1657c-166">tooconfirm Pokud okruh ExpressRoute je funkční, věnovat zvláštní pozornost toohello následující pole:</span><span class="sxs-lookup"><span data-stu-id="1657c-166">tooconfirm if an ExpressRoute circuit is operational, pay particular attention toohello following fields:</span></span>

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

>[!NOTE]
><span data-ttu-id="1657c-167">Pokud hello *CircuitProvisioningState* není povoleno, obraťte se na [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="1657c-167">If hello *CircuitProvisioningState* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="1657c-168">Pokud hello *ServiceProviderProvisioningState* není zajišťováno, obraťte se na svého poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="1657c-168">If hello *ServiceProviderProvisioningState* is not provisioned, contact your service provider.</span></span>
>
>

###<a name="verification-via-powershell-classic"></a><span data-ttu-id="1657c-169">Ověření pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="1657c-169">Verification via PowerShell (Classic)</span></span>
<span data-ttu-id="1657c-170">toolist všechny hello okruhy ExpressRoute v rámci předplatného, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1657c-170">toolist all hello ExpressRoute circuits under a subscription, use hello following command:</span></span>

    Get-AzureDedicatedCircuit

<span data-ttu-id="1657c-171">tooselect konkrétní okruh ExpressRoute, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1657c-171">tooselect a particular ExpressRoute circuit, use hello following command:</span></span>

    Get-AzureDedicatedCircuit -ServiceKey **************************************

<span data-ttu-id="1657c-172">Ukázková odpověď je:</span><span class="sxs-lookup"><span data-stu-id="1657c-172">A sample response is:</span></span>

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="1657c-173">tooconfirm Pokud okruh ExpressRoute je funkční, věnovat zvláštní pozornost toohello následující pole: ServiceProviderProvisioningState: Stav zřízení: povoleno</span><span class="sxs-lookup"><span data-stu-id="1657c-173">tooconfirm if an ExpressRoute circuit is operational, pay particular attention toohello following fields: ServiceProviderProvisioningState : Provisioned Status                           : Enabled</span></span>

>[!NOTE]
><span data-ttu-id="1657c-174">Pokud hello *stav* není povoleno, obraťte se na [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="1657c-174">If hello *Status* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="1657c-175">Pokud hello *ServiceProviderProvisioningState* není zajišťováno, obraťte se na svého poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="1657c-175">If hello *ServiceProviderProvisioningState* is not provisioned, contact your service provider.</span></span>
>
>

##<a name="validate-peering-configuration"></a><span data-ttu-id="1657c-176">Ověření konfigurace partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="1657c-176">Validate Peering Configuration</span></span>
<span data-ttu-id="1657c-177">Po má poskytovatel služeb hello dokončené hello zřizování hello okruh ExpressRoute, mohou být vytvořeny konfigurace směrování přes hello okruh ExpressRoute mezi MSEE-PRs (4) a Msee (5).</span><span class="sxs-lookup"><span data-stu-id="1657c-177">After hello service provider has completed hello provisioning hello ExpressRoute circuit, a routing configuration can be created over hello ExpressRoute circuit between MSEE-PRs (4) and MSEEs (5).</span></span> <span data-ttu-id="1657c-178">Každý okruh ExpressRoute může mít jednu, dvě nebo tři směrování kontexty povoleno: soukromý partnerský vztah Azure (provoz tooprivate virtuální sítě v Azure), veřejný partnerský vztah Azure (provoz toopublic IP adresy v Azure) a (provoz tooOffice 365 partnerského vztahu Microsoftu a Dynamics 365).</span><span class="sxs-lookup"><span data-stu-id="1657c-178">Each ExpressRoute circuit can have one, two, or three routing contexts enabled: Azure private peering (traffic tooprivate virtual networks in Azure), Azure public peering (traffic toopublic IP addresses in Azure), and Microsoft peering (traffic tooOffice 365 and Dynamics 365).</span></span> <span data-ttu-id="1657c-179">Další informace o tom, toocreate a upravit konfigurace směrování, najdete v článku hello [vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="1657c-179">For more information on how toocreate and modify routing configuration, see hello article [Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>

###<a name="verification-via-hello-azure-portal"></a><span data-ttu-id="1657c-180">Ověření prostřednictvím hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1657c-180">Verification via hello Azure portal</span></span>
>[!IMPORTANT]
><span data-ttu-id="1657c-181">V portálu Azure, ve kterém jsou partnerských vztahů ExpressRoute hello je známého problému *není* uvedené na portálu hello Pokud nakonfigurovat pomocí hello poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="1657c-181">There is a known bug in hello Azure portal wherein ExpressRoute peerings are *NOT* shown in hello portal if configured by hello service provider.</span></span> <span data-ttu-id="1657c-182">Přidání partnerských vztahů ExpressRoute prostřednictvím portálu hello nebo prostředí PowerShell *přepíše nastavení poskytovatele služby hello*.</span><span class="sxs-lookup"><span data-stu-id="1657c-182">Adding ExpressRoute peerings via hello portal or PowerShell *overwrites hello service provider settings*.</span></span> <span data-ttu-id="1657c-183">Tato akce naruší hello směrování v okruhu ExpressRoute hello a vyžaduje podporu hello hello služby zprostředkovatele toorestore hello nastavení a obnovit normální směrování.</span><span class="sxs-lookup"><span data-stu-id="1657c-183">This action breaks hello routing on hello ExpressRoute circuit and requires hello support of hello service provider toorestore hello settings and reestablish normal routing.</span></span> <span data-ttu-id="1657c-184">Partnerské vztahy ExpressRoute hello změnit, jenom Pokud je jisté, že tohoto zprostředkovatele služby hello poskytuje služby vrstvy 2 pouze!</span><span class="sxs-lookup"><span data-stu-id="1657c-184">Only modify hello ExpressRoute peerings if it is certain that hello service provider is providing layer 2 services only!</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="1657c-185">Když vrstvy 3 je zadané ve hello jsou prázdné hello portálu služby zprostředkovatele a hello partnerských vztahů, může být prostředí PowerShell používané toosee hello služby zprostředkovatele nakonfigurovaná nastavení.</span><span class="sxs-lookup"><span data-stu-id="1657c-185">If layer 3 is provided by hello service provider and hello peerings are blank in hello portal, PowerShell can be used toosee hello service provider configured settings.</span></span>
>
>

<span data-ttu-id="1657c-186">V hello portálu Azure, můžete zkontrolovat stav okruhu ExpressRoute výběrem ![2][2] na hello nabídce vlevo. straně panelu a potom vyberete hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="1657c-186">In hello Azure portal, status of an ExpressRoute circuit can be checked by selecting ![2][2] on hello left-side-bar menu and then selecting hello ExpressRoute circuit.</span></span> <span data-ttu-id="1657c-187">Výběr ExpressRoute okruhu uvedené v části "Všechny prostředky" by otevřete okno okruh ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="1657c-187">Selecting an ExpressRoute circuit listed under "All resources" would open hello ExpressRoute circuit blade.</span></span> <span data-ttu-id="1657c-188">V hello ![3][3] části hello okně hello ExpressRoute, které by byly uvedeny essentials, jak ukazuje následující snímek obrazovky hello:</span><span class="sxs-lookup"><span data-stu-id="1657c-188">In hello ![3][3] section of hello blade, hello ExpressRoute essentials would be listed as shown in hello following screen shot:</span></span>

<span data-ttu-id="1657c-189">![5][5]</span><span class="sxs-lookup"><span data-stu-id="1657c-189">![5][5]</span></span>

<span data-ttu-id="1657c-190">V předchozím příkladu hello jako uvedené Azure soukromého partnerského vztahu směrování kontextu je povoleno, zatímco veřejný Azure a kontexty směrování partnerského vztahu Microsoftu nejsou povolené.</span><span class="sxs-lookup"><span data-stu-id="1657c-190">In hello preceding example, as noted Azure private peering routing context is enabled, whereas Azure public and Microsoft peering routing contexts are not enabled.</span></span> <span data-ttu-id="1657c-191">Úspěšně povolilo partnerského vztahu kontextu by měla mít také hello primární a sekundární point-to-point (povinné pro protokol BGP) podsítě uvedené.</span><span class="sxs-lookup"><span data-stu-id="1657c-191">A successfully enabled peering context would also have hello primary and secondary point-to-point (required for BGP) subnets listed.</span></span> <span data-ttu-id="1657c-192">Hello /30 podsítě se používají pro hello rozhraní IP adresu hello Msee a PE Msee.</span><span class="sxs-lookup"><span data-stu-id="1657c-192">hello /30 subnets are used for hello interface IP address of hello MSEEs and PE-MSEEs.</span></span> 

>[!NOTE]
><span data-ttu-id="1657c-193">Pokud partnerský vztah není povolena, zkontrolujte, je-li primární a sekundární podsítě hello přiřazené odpovídat konfiguraci hello v PE Msee.</span><span class="sxs-lookup"><span data-stu-id="1657c-193">If a peering is not enabled, check if hello primary and secondary subnets assigned match hello configuration on PE-MSEEs.</span></span> <span data-ttu-id="1657c-194">Pokud ne, toochange na směrovači MSEE konfigurace hello, podívejte se příliš[vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering]</span><span class="sxs-lookup"><span data-stu-id="1657c-194">If not, toochange hello configuration on MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering]</span></span>
>
>

###<a name="verification-via-powershell"></a><span data-ttu-id="1657c-195">Ověření pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1657c-195">Verification via PowerShell</span></span>
<span data-ttu-id="1657c-196">tooget hello Azure privátní partnerský vztah podrobnosti o konfiguraci, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1657c-196">tooget hello Azure private peering configuration details, use hello following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt

<span data-ttu-id="1657c-197">Ukázková odpověď, úspěšně nakonfigurovaný soukromého partnerského vztahu, je:</span><span class="sxs-lookup"><span data-stu-id="1657c-197">A sample response, for a successfully configured private peering, is:</span></span>

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

 <span data-ttu-id="1657c-198">Úspěšně povolilo partnerského vztahu kontextu by měla mít předpony adres primární a sekundární hello uvedené.</span><span class="sxs-lookup"><span data-stu-id="1657c-198">A successfully enabled peering context would have hello primary and secondary address prefixes listed.</span></span> <span data-ttu-id="1657c-199">Hello /30 podsítě se používají pro hello rozhraní IP adresu hello Msee a PE Msee.</span><span class="sxs-lookup"><span data-stu-id="1657c-199">hello /30 subnets are used for hello interface IP address of hello MSEEs and PE-MSEEs.</span></span>

<span data-ttu-id="1657c-200">tooget hello Azure veřejného partnerského vztahu podrobnosti o konfiguraci, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1657c-200">tooget hello Azure public peering configuration details, use hello following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt

<span data-ttu-id="1657c-201">tooget hello podrobností partnerského vztahu Microsoftu konfigurace, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1657c-201">tooget hello Microsoft peering configuration details, use hello following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt

<span data-ttu-id="1657c-202">Pokud není nakonfigurováno partnerský vztah, by chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="1657c-202">If a peering is not configured, there would be an error message.</span></span> <span data-ttu-id="1657c-203">Ukázková odpověď, když hello uvádí partnerského vztahu (Azure veřejný partnerský vztah v tomto příkladu) není nakonfigurované v rámci okruhu hello:</span><span class="sxs-lookup"><span data-stu-id="1657c-203">A sample response, when hello stated peering (Azure Public peering in this example) is not configured within hello circuit:</span></span>

    Get-AzureRmExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzureRmExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


<p/>
>[!NOTE]
><span data-ttu-id="1657c-204">Pokud partnerský vztah není povolena, zkontrolujte, pokud hello primární a sekundární podsítě přiřadit shodu hello konfiguraci na hello propojené PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="1657c-204">If a peering is not enabled, check if hello primary and secondary subnets assigned match hello configuration on hello linked PE-MSEE.</span></span> <span data-ttu-id="1657c-205">Také zkontrolujte, jestli hello opravit *VlanId*, *AzureASN*, a *PeerASN* se používají na Msee a pokud se tyto hodnoty mapuje toohello jsou použity na hello propojené PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="1657c-205">Also check if hello correct *VlanId*, *AzureASN*, and *PeerASN* are used on MSEEs and if these values maps toohello ones used on hello linked PE-MSEE.</span></span> <span data-ttu-id="1657c-206">Pokud zvolíte použití algoritmu hash MD5, by měla být stejná na dvojice MSEE a PE MSEE hello sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="1657c-206">If MD5 hashing is chosen, hello shared key should be same on MSEE and PE-MSEE pair.</span></span> <span data-ttu-id="1657c-207">Konfigurace hello toochange na směrovači MSEE hello, najdete v příliš [vytvoření a úprava směrování pro okruh ExpressRoute] [CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="1657c-207">toochange hello configuration on hello MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>  
>
>

### <a name="verification-via-powershell-classic"></a><span data-ttu-id="1657c-208">Ověření pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="1657c-208">Verification via PowerShell (Classic)</span></span>
<span data-ttu-id="1657c-209">tooget hello Azure privátní partnerský vztah podrobnosti o konfiguraci, použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1657c-209">tooget hello Azure private peering configuration details, use hello following command:</span></span>

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

<span data-ttu-id="1657c-210">Ukázková odpověď, úspěšně nakonfigurovaný soukromého partnerského vztahu je:</span><span class="sxs-lookup"><span data-stu-id="1657c-210">A sample response, for a successfully configured private peering is:</span></span>

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

<span data-ttu-id="1657c-211">Úspěšně, povolená A partnerského vztahu kontextu by měla mít podsítě hello primárního a sekundárního partnera uvedené.</span><span class="sxs-lookup"><span data-stu-id="1657c-211">A successfully, enabled peering context would have hello primary and secondary peer subnets listed.</span></span> <span data-ttu-id="1657c-212">Hello /30 podsítě se používají pro hello rozhraní IP adresu hello Msee a PE Msee.</span><span class="sxs-lookup"><span data-stu-id="1657c-212">hello /30 subnets are used for hello interface IP address of hello MSEEs and PE-MSEEs.</span></span>

<span data-ttu-id="1657c-213">tooget hello Azure veřejného partnerského vztahu podrobnosti o konfiguraci, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1657c-213">tooget hello Azure public peering configuration details, use hello following commands:</span></span>

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

<span data-ttu-id="1657c-214">tooget hello podrobností partnerského vztahu Microsoftu konfigurace, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1657c-214">tooget hello Microsoft peering configuration details, use hello following commands:</span></span>

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

>[!IMPORTANT]
><span data-ttu-id="1657c-215">Pokud vrstvy 3 partnerských vztahů byly nastavené zásadami hello poskytovatele služeb, nastavení hello partnerských vztahů ExpressRoute prostřednictvím portálu hello nebo prostředí PowerShell přepíše nastavení poskytovatele služby hello.</span><span class="sxs-lookup"><span data-stu-id="1657c-215">If layer 3 peerings were set by hello service provider, setting hello ExpressRoute peerings via hello portal or PowerShell overwrites hello service provider settings.</span></span> <span data-ttu-id="1657c-216">Resetování partnerského vztahu nastavení straně hello zprostředkovatele vyžaduje podporu hello hello poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="1657c-216">Resetting hello provider side peering settings requires hello support of hello service provider.</span></span> <span data-ttu-id="1657c-217">Partnerské vztahy ExpressRoute hello změnit, jenom Pokud je jisté, že tohoto zprostředkovatele služby hello poskytuje služby vrstvy 2 pouze!</span><span class="sxs-lookup"><span data-stu-id="1657c-217">Only modify hello ExpressRoute peerings if it is certain that hello service provider is providing layer 2 services only!</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="1657c-218">Pokud partnerský vztah není povolena, zkontrolujte, pokud konfigurace hello hello primárního a sekundárního partnera přiřazené podsítě shodu na hello propojené PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="1657c-218">If a peering is not enabled, check if hello primary and secondary peer subnets assigned match hello configuration on hello linked PE-MSEE.</span></span> <span data-ttu-id="1657c-219">Také zkontrolujte, jestli hello opravit *VlanId*, *AzureAsn*, a *PeerAsn* se používají na Msee a pokud se tyto hodnoty mapuje toohello jsou použity na hello propojené PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="1657c-219">Also check if hello correct *VlanId*, *AzureAsn*, and *PeerAsn* are used on MSEEs and if these values maps toohello ones used on hello linked PE-MSEE.</span></span> <span data-ttu-id="1657c-220">Konfigurace hello toochange na směrovači MSEE hello, najdete v příliš [vytvoření a úprava směrování pro okruh ExpressRoute] [CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="1657c-220">toochange hello configuration on hello MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>
>
>

## <a name="validate-arp-between-microsoft-and-hello-service-provider"></a><span data-ttu-id="1657c-221">Ověření protokolu ARP mezi poskytovatele služeb společnosti Microsoft a hello</span><span class="sxs-lookup"><span data-stu-id="1657c-221">Validate ARP between Microsoft and hello service provider</span></span>
<span data-ttu-id="1657c-222">Tato část používá příkazy prostředí PowerShell (klasické).</span><span class="sxs-lookup"><span data-stu-id="1657c-222">This section uses PowerShell (Classic) commands.</span></span> <span data-ttu-id="1657c-223">Pokud používáte příkazy prostředí PowerShell Azure Resource Manager, ujistěte se, zda máte přístup správce nebo spolusprávce předplatného toohello prostřednictvím [portál Azure classic][OldPortal].</span><span class="sxs-lookup"><span data-stu-id="1657c-223">If you have been using PowerShell Azure Resource Manager commands, ensure that you have admin/co-admin access toohello subscription via [Azure classic portal][OldPortal].</span></span> <span data-ttu-id="1657c-224">Řešení potíží s pomocí Azure Resource Manager příkazy naleznete toohello [získávání ARP tabulky v modelu nasazení Resource Manager hello] [ ARP] dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1657c-224">For troubleshooting using Azure Resource Manager commands please refer toohello [Getting ARP tables in hello Resource Manager deployment model][ARP] document.</span></span>

>[!NOTE]
><span data-ttu-id="1657c-225">tooget protokolu ARP, hello portál Azure a příkazy prostředí PowerShell Azure Resource Manager můžete použít.</span><span class="sxs-lookup"><span data-stu-id="1657c-225">tooget ARP, both hello Azure portal and Azure Resource Manager PowerShell commands can be used.</span></span> <span data-ttu-id="1657c-226">Pokud k chybám hello Azure Resource Manager příkazy prostředí PowerShell, classic příkazy prostředí PowerShell by měly fungovat jako Classic PowerShell příkazy spolupracovat taky s nástrojem okruhy ExpressRoute Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1657c-226">If errors are encountered with hello Azure Resource Manager PowerShell commands, classic PowerShell commands should work as Classic PowerShell commands also work with Azure Resource Manager ExpressRoute circuits.</span></span>
>
>

<span data-ttu-id="1657c-227">tooget hello tabulky ARP z hello primární MSEE směrovač hello soukromého partnerského vztahu, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1657c-227">tooget hello ARP table from hello primary MSEE router for hello private peering, use hello following command:</span></span>

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="1657c-228">Odpověď příklad pro příkaz hello, v případě úspěšné hello:</span><span class="sxs-lookup"><span data-stu-id="1657c-228">An example response for hello command, in hello successful scenario:</span></span>

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

<span data-ttu-id="1657c-229">Podobně můžete zkontrolovat hello tabulky ARP z hello MSEE v hello *primární*/*sekundární* cestu, pro *privátní* /  *Veřejné*/*Microsoft* partnerských vztahů.</span><span class="sxs-lookup"><span data-stu-id="1657c-229">Similarly, you can check hello ARP table from hello MSEE in hello *Primary*/*Secondary* path, for *Private*/*Public*/*Microsoft* peerings.</span></span>

<span data-ttu-id="1657c-230">Hello následující příklad, že zobrazuje hello odpovědi hello příkazu pro partnerský vztah neexistuje.</span><span class="sxs-lookup"><span data-stu-id="1657c-230">hello following example shows hello response of hello command for a peering does not exist.</span></span>

    ARP Info:
       
>[!NOTE]
><span data-ttu-id="1657c-231">Pokud nemá hello tabulky ARP IP adresy rozhraní hello mapované tooMAC adresy, hello zkontrolujte následující informace:</span><span class="sxs-lookup"><span data-stu-id="1657c-231">If hello ARP table does not have IP addresses of hello interfaces mapped tooMAC addresses, review hello following information:</span></span>
>1. <span data-ttu-id="1657c-232">Pokud přiřazená hello první IP adresa podsítě hello /30 pro hello propojení mezi hello MSEE PR a MSEE se používá na hello rozhraní MSEE PR.</span><span class="sxs-lookup"><span data-stu-id="1657c-232">If hello first IP address of hello /30 subnet assigned for hello link between hello MSEE-PR and MSEE is used on hello interface of MSEE-PR.</span></span> <span data-ttu-id="1657c-233">Azure vždy používá hello druhou IP adresu pro Msee.</span><span class="sxs-lookup"><span data-stu-id="1657c-233">Azure always uses hello second IP address for MSEEs.</span></span>
>2. <span data-ttu-id="1657c-234">Ověřte, pokud zákazník hello (C-Tag) a značky VLAN služby (S-Tag) odpovídají na dvojice MSEE PR a MSEE.</span><span class="sxs-lookup"><span data-stu-id="1657c-234">Verify if hello customer (C-Tag) and service (S-Tag) VLAN tags match both on MSEE-PR and MSEE pair.</span></span>
>
>

## <a name="validate-bgp-and-routes-on-hello-msee"></a><span data-ttu-id="1657c-235">Ověření protokolu BGP a trasy na hello MSEE</span><span class="sxs-lookup"><span data-stu-id="1657c-235">Validate BGP and routes on hello MSEE</span></span>
<span data-ttu-id="1657c-236">Tato část používá příkazy prostředí PowerShell (klasické).</span><span class="sxs-lookup"><span data-stu-id="1657c-236">This section uses PowerShell (Classic) commands.</span></span> <span data-ttu-id="1657c-237">Pokud používáte příkazy prostředí PowerShell Azure Resource Manager, ujistěte se, zda máte přístup správce nebo spolusprávce předplatného toohello prostřednictvím [portál Azure classic][OldPortal]</span><span class="sxs-lookup"><span data-stu-id="1657c-237">If you have been using PowerShell Azure Resource Manager commands, ensure that you have admin/co-admin access toohello subscription via [Azure classic portal][OldPortal]</span></span>

>[!NOTE]
><span data-ttu-id="1657c-238">tooget BGP informace, jak hello lze použít portál Azure a příkazy prostředí PowerShell Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1657c-238">tooget BGP information, both hello Azure portal and Azure Resource Manager PowerShell commands can be used.</span></span> <span data-ttu-id="1657c-239">Pokud k chybám hello Azure Resource Manager příkazy prostředí PowerShell, classic příkazy prostředí PowerShell by měly fungovat jako classic PowerShell příkazy spolupracovat taky s nástrojem okruhy ExpressRoute Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1657c-239">If errors are encountered with hello Azure Resource Manager PowerShell commands, classic PowerShell commands should work as classic PowerShell commands also work with Azure Resource Manager ExpressRoute circuits.</span></span>
>
>

<span data-ttu-id="1657c-240">tooget hello směrovací tabulky (BGP sousedním) souhrnu pro konkrétní směrování kontext, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1657c-240">tooget hello routing table (BGP neighbor) summary for a particular routing context, use hello following command:</span></span>

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="1657c-241">Odpověď příklad je:</span><span class="sxs-lookup"><span data-stu-id="1657c-241">An example response is:</span></span>

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

<span data-ttu-id="1657c-242">Jak je znázorněno v předchozím příkladu hello, příkaz hello je užitečné toodetermine pro jak dlouho je vytvořený hello směrování kontextu.</span><span class="sxs-lookup"><span data-stu-id="1657c-242">As shown in hello preceding example, hello command is useful toodetermine for how long hello routing context has been established.</span></span> <span data-ttu-id="1657c-243">Označuje také počet předpony trasy inzerované partnerského vztahu směrovačem hello.</span><span class="sxs-lookup"><span data-stu-id="1657c-243">It also indicates number of route prefixes advertised by hello peering router.</span></span>

>[!NOTE]
><span data-ttu-id="1657c-244">Pokud je stav hello v aktivní nebo nečinnosti, zkontrolujte, pokud konfigurace hello hello primárního a sekundárního partnera přiřazené podsítě shodu na hello propojené PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="1657c-244">If hello state is in Active or Idle, check if hello primary and secondary peer subnets assigned match hello configuration on hello linked PE-MSEE.</span></span> <span data-ttu-id="1657c-245">Také zkontrolujte, jestli hello opravit *VlanId*, *AzureAsn*, a *PeerAsn* se používají na Msee a pokud se tyto hodnoty mapuje toohello jsou použity na hello propojené PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="1657c-245">Also check if hello correct *VlanId*, *AzureAsn*, and *PeerAsn* are used on MSEEs and if these values maps toohello ones used on hello linked PE-MSEE.</span></span> <span data-ttu-id="1657c-246">Pokud zvolíte použití algoritmu hash MD5, by měla být stejná na dvojice MSEE a PE MSEE hello sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="1657c-246">If MD5 hashing is chosen, hello shared key should be same on MSEE and PE-MSEE pair.</span></span> <span data-ttu-id="1657c-247">Konfigurace hello toochange na směrovači MSEE hello, najdete v příliš[vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="1657c-247">toochange hello configuration on hello MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="1657c-248">Pokud některá místa určení není dostupný přes konkrétní partnerský vztah, zkontrolujte hello směrovací tabulku z hello Msee patřící toohello konkrétní partnerského vztahu kontextu.</span><span class="sxs-lookup"><span data-stu-id="1657c-248">If certain destinations are not reachable over a particular peering, check hello route table of hello MSEEs belonging toohello particular peering context.</span></span> <span data-ttu-id="1657c-249">Pokud se nachází ve směrovací tabulce hello odpovídající předpony (můžou být NATed IP), potom zkontrolujte, zda jsou brány firewall nebo nebo seznamy ACL skupiny NSG na cestu hello a v případě, že povolují provoz hello.</span><span class="sxs-lookup"><span data-stu-id="1657c-249">If a matching prefix (could be NATed IP) is present in hello routing table, then check if there are firewalls/NSG/ACLs on hello path and if they permit hello traffic.</span></span>
>
>

<span data-ttu-id="1657c-250">tooget hello úplné směrovací tabulky z MSEE na hello *primární* cestu pro konkrétní hello *privátní* směrování kontextu, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1657c-250">tooget hello full routing table from MSEE on hello *Primary* path for hello particular *Private* routing context, use hello following command:</span></span>

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="1657c-251">Je úspěšnému výsledku příklad pro příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1657c-251">An example successful outcome for hello command is:</span></span>

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

<span data-ttu-id="1657c-252">Podobně můžete zkontrolovat hello směrovací tabulky z hello MSEE v hello *primární*/*sekundární* cestu, pro *privátní* / *Veřejné*/*Microsoft* partnerského vztahu kontextu.</span><span class="sxs-lookup"><span data-stu-id="1657c-252">Similarly, you can check hello routing table from hello MSEE in hello *Primary*/*Secondary* path, for *Private*/*Public*/*Microsoft* a peering context.</span></span>

<span data-ttu-id="1657c-253">Hello následující příklad, že zobrazuje hello odpovědi hello příkazu pro partnerský vztah neexistuje:</span><span class="sxs-lookup"><span data-stu-id="1657c-253">hello following example shows hello response of hello command for a peering does not exist:</span></span>

    Route Table Info:

##<a name="check-hello-traffic-statistics"></a><span data-ttu-id="1657c-254">Zkontrolujte hello statistiky provozu</span><span class="sxs-lookup"><span data-stu-id="1657c-254">Check hello Traffic Statistics</span></span>
<span data-ttu-id="1657c-255">primární a sekundární cesta statistiku provozu – bajtů tooget hello a odhlašování – kombinaci partnerského vztahu kontextu, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1657c-255">tooget hello combined primary and secondary path traffic statistics--bytes in and out--of a peering context, use hello following command:</span></span>

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

<span data-ttu-id="1657c-256">Ukázkový výstup hello příkazu je:</span><span class="sxs-lookup"><span data-stu-id="1657c-256">A sample output of hello command is:</span></span>

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

<span data-ttu-id="1657c-257">Ukázkový výstup hello příkazu pro neexistující partnerský vztah je:</span><span class="sxs-lookup"><span data-stu-id="1657c-257">A sample output of hello command for a non-existent peering is:</span></span>

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a><span data-ttu-id="1657c-258">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1657c-258">Next Steps</span></span>
<span data-ttu-id="1657c-259">Další informace a nápovědu najdete na naší hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="1657c-259">For more information or help, check out hello following links:</span></span>

- <span data-ttu-id="1657c-260">[Podporu společnosti Microsoft][Support]</span><span class="sxs-lookup"><span data-stu-id="1657c-260">[Microsoft Support][Support]</span></span>
- <span data-ttu-id="1657c-261">[Vytvoření a úprava okruhu ExpressRoute][CreateCircuit]</span><span class="sxs-lookup"><span data-stu-id="1657c-261">[Create and modify an ExpressRoute circuit][CreateCircuit]</span></span>
- <span data-ttu-id="1657c-262">[Vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering]</span><span class="sxs-lookup"><span data-stu-id="1657c-262">[Create and modify routing for an ExpressRoute circuit][CreatePeering]</span></span>

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "Připojení k logické Express Route"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Ikona všechny prostředky"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Ikona – přehled"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "Snímek obrazovky ukázkové ExpressRoute Essentials"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "Snímek obrazovky ukázkové ExpressRoute Essentials"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[OldPortal]: https://manage.windowsazure.com
[ARP]: https://docs.microsoft.com/en-us/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






