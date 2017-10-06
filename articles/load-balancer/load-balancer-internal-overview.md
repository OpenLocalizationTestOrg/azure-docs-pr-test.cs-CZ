---
title: "Vyrovnávání zatížení aaaInternal přehled | Microsoft Docs"
description: "Přehled pro interní vyrovnávání zátěže a jejich funkce. Jak funguje nástroj pro vyrovnávání zatížení pro scénáře Azure a možné tooconfigure vnitřních koncových bodů"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 36065bfe-0ef1-46f9-a9e1-80b229105c85
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 9a901aad224d8821c154e130e142699d57282b25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="internal-load-balancer-overview"></a><span data-ttu-id="29bff-103">Přehled nástroje pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="29bff-103">Internal load balancer overview</span></span>

<span data-ttu-id="29bff-104">Na rozdíl od hello Internet přístupných Vyrovnávání zatížení vyrovnávání hello interní zatížení (ILB) směrovat provoz jenom tooresources uvnitř hello cloudové služby nebo pomocí sítě VPN tooaccess hello infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="29bff-104">Unlike hello Internet facing load balancer, hello internal load balancer (ILB) directs traffic only tooresources inside hello cloud service or using VPN tooaccess hello Azure infrastructure.</span></span> <span data-ttu-id="29bff-105">Hello infrastruktury omezuje přístup toohello skupinu s vyrovnáváním zatížení virtuální IP adresy (VIP) Cloudovou službu nebo virtuální sítě tak, aby nikdy bude Internet přímo zveřejněné tooan koncový bod.</span><span class="sxs-lookup"><span data-stu-id="29bff-105">hello infrastructure restricts access toohello load balanced virtual IP addresses (VIPs) of a Cloud Service or a Virtual Network so that they will never be directly exposed tooan Internet endpoint.</span></span> <span data-ttu-id="29bff-106">To umožňuje interní řádku toorun obchodní (LOB) aplikace v Azure a získat přístup z cloudu hello nebo z místní prostředky.</span><span class="sxs-lookup"><span data-stu-id="29bff-106">This enables internal line of business (LOB) applications toorun in Azure and be accessed from within hello cloud or from resources on-premises.</span></span>

## <a name="why-you-may-need-an-internal-load-balancer"></a><span data-ttu-id="29bff-107">Může být nutná k nástroji pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="29bff-107">Why you may need an internal load balancer</span></span>

<span data-ttu-id="29bff-108">Azure interní načíst vyrovnávání (ILB) poskytuje vyrovnávání zátěže mezi virtuální počítače, které se nacházejí v rámci cloudové služby nebo virtuální síť s místní rozsah.</span><span class="sxs-lookup"><span data-stu-id="29bff-108">Azure Internal Load Balancing (ILB) provides load balancing between virtual machines that reside inside of a cloud service or a virtual network with a regional scope.</span></span> <span data-ttu-id="29bff-109">Informace o použití hello a konfigurace virtuální sítě s místní rozsah najdete v tématu [regionálních virtuálních sítí](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) v hello Azure blog.</span><span class="sxs-lookup"><span data-stu-id="29bff-109">For information about hello use and configuration of virtual networks with a regional scope, see [Regional Virtual Networks](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) in hello Azure blog.</span></span> <span data-ttu-id="29bff-110">Existující virtuální sítě, které jsou nakonfigurované pro skupinu vztahů, nemůžou interní nástroj pro vyrovnávání zatížení používat.</span><span class="sxs-lookup"><span data-stu-id="29bff-110">Existing virtual networks that have been configured for an affinity group cannot use ILB.</span></span>

<span data-ttu-id="29bff-111">ILB umožňuje hello následující typy služby Vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="29bff-111">ILB enables hello following types of load balancing:</span></span>

* <span data-ttu-id="29bff-112">V rámci cloudové služby, z virtuálních počítačů tooa sadu virtuálních počítačů, které se nacházejí v hello stejné cloudové služby (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="29bff-112">Within a cloud service, from virtual machines tooa set of virtual machines that reside within hello same cloud service (see Figure 1).</span></span>
* <span data-ttu-id="29bff-113">V rámci virtuální sítě z virtuálních počítačů v sadě tooa hello virtuální sítě virtuálních počítačů, které se nacházejí v hello stejné cloudové služby hello virtuální sítě (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="29bff-113">Within a virtual network, from virtual machines in hello virtual network tooa set of virtual machines that reside within hello same cloud service of hello virtual network (see Figure 2).</span></span>
* <span data-ttu-id="29bff-114">Pro virtuální síť mezi různými místy z místní počítače tooa sada virtuálních počítačů, které se nacházejí v hello stejný cloudový služby hello virtuální sítě (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="29bff-114">For a cross-premises virtual network, from on-premises computers tooa set of virtual machines that reside within hello same cloud service of hello virtual network (see Figure 3).</span></span>
* <span data-ttu-id="29bff-115">Vyrovnávání pro provoz z vrstvy internetového hello zatížení internetového, vícevrstvé aplikace, ve kterých nejsou internetového hello back-end vrstev, ale vyžadují.</span><span class="sxs-lookup"><span data-stu-id="29bff-115">Internet-facing, multi-tier applications in which hello back-end tiers are not Internet-facing but require load balancing for traffic from hello Internet-facing tier.</span></span>
* <span data-ttu-id="29bff-116">Vyrovnávání zatížení pro obchodní aplikace, které jsou hostované v Azure bez nutnosti další zátěž vyrovnávání hardwaru nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="29bff-116">Load balancing for LOB applications hosted in Azure without requiring additional load balancer hardware or software.</span></span> <span data-ttu-id="29bff-117">Včetně místních serverů v hello sadu počítačů, jejichž provoz je zatížení vyvážit.</span><span class="sxs-lookup"><span data-stu-id="29bff-117">Including on-premises servers in hello set of computers whose traffic is load balanced.</span></span>

## <a name="internet-facing-multi-tier-applications"></a><span data-ttu-id="29bff-118">Internetový bod vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="29bff-118">Internet facing multi-tier applications</span></span>

<span data-ttu-id="29bff-119">Hello webová vrstva obsahuje internetových koncových bodů pro internetové klienty a je součástí skupiny s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="29bff-119">hello web tier has Internet facing endpoints for Internet clients and is part of a load-balanced set.</span></span> <span data-ttu-id="29bff-120">Nástroj pro vyrovnávání zatížení Hello distribuuje příchozí provoz z webových klientů pro TCP port 443 (HTTPS) toohello webové servery.</span><span class="sxs-lookup"><span data-stu-id="29bff-120">hello load balancer  distributes incoming traffic from web clients for TCP port 443 (HTTPS) toohello web servers.</span></span>

<span data-ttu-id="29bff-121">Hello databázové servery jsou za koncovým bodem ILB, který hello webové servery používají pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="29bff-121">hello database servers are behind an ILB endpoint which hello web servers use for storage.</span></span> <span data-ttu-id="29bff-122">Koncový bod, jaký provoz je vyrovnáváno zatížení napříč servery databáze hello hello ILB sadu s vyrovnáváním zatížení služby této databáze.</span><span class="sxs-lookup"><span data-stu-id="29bff-122">This database service load balanced endpoint, which traffic is load balanced across hello database servers in hello ILB set.</span></span>

<span data-ttu-id="29bff-123">Následující obrázek ukazuje Hello hello internetové vícevrstvé aplikace v rámci hello stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="29bff-123">hello following image shows hello Internet facing multi-tier application within hello same cloud service.</span></span>

![Interní jeden cloud službou Vyrovnávání zatížení](./media/load-balancer-internal-overview/IC736321.png)

<span data-ttu-id="29bff-125">Obrázek 1 – internetový bod vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="29bff-125">Figure 1 - Internet facing multi-tier application</span></span>

<span data-ttu-id="29bff-126">Další využití pro vícevrstvé aplikace je při hello ILB nasazení tooa jinou cloudovou službu, než jedna služba hello náročné pro hello ILB hello.</span><span class="sxs-lookup"><span data-stu-id="29bff-126">Another possible use for a multi-tier application is when hello ILB deployed tooa different cloud service than hello one consuming hello service for hello ILB.</span></span>

<span data-ttu-id="29bff-127">Cloud services pomocí hello stejné virtuální síti bude mít přístup k koncovým bodem ILB toohello.</span><span class="sxs-lookup"><span data-stu-id="29bff-127">Cloud services using hello same virtual network will have access toohello ILB endpoint.</span></span> <span data-ttu-id="29bff-128">Následující obrázek ukazuje front-end webové servery jsou v rámci různých cloudové služby z databáze hello back-end a pomocí Hello hello koncovým bodem ILB v rámci hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="29bff-128">hello following image shows front-end web servers are in a different cloud service from hello database back-end and using hello ILB endpoint within hello same virtual network.</span></span>

![Interní vyrovnávání zátěže mezi cloudové služby](./media/load-balancer-internal-overview/IC744147.png)

<span data-ttu-id="29bff-130">Obrázek 2 – Front-end servery v rámci různých cloudové služby</span><span class="sxs-lookup"><span data-stu-id="29bff-130">Figure 2 - Front-end servers in a different cloud service</span></span>

## <a name="intranet-line-of-business-applications"></a><span data-ttu-id="29bff-131">Intranetu řádek obchodní aplikace</span><span class="sxs-lookup"><span data-stu-id="29bff-131">Intranet line of business applications</span></span>

<span data-ttu-id="29bff-132">Přenosy od klientů v místní síti hello získat Vyrovnávání zatížení napříč hello sadu serverů LOB pomocí sítě tooAzure připojení VPN.</span><span class="sxs-lookup"><span data-stu-id="29bff-132">Traffic from clients on hello on-premises network get load-balanced across hello set of LOB servers using VPN connection tooAzure network.</span></span>

<span data-ttu-id="29bff-133">Hello klientský počítač bude mít přístup tooan IP adresu ze služby Azure VPN pomocí toosite bodu VPN.</span><span class="sxs-lookup"><span data-stu-id="29bff-133">hello client machine will have access tooan IP address from Azure VPN service using point toosite VPN.</span></span> <span data-ttu-id="29bff-134">To umožňuje použití hello hello obchodní aplikace hostované za koncovým bodem ILB hello.</span><span class="sxs-lookup"><span data-stu-id="29bff-134">It allows hello use hello LOB application hosted behind hello ILB endpoint.</span></span>

![Interní rozložení zátěže pomocí toosite bodu VPN](./media/load-balancer-internal-overview/IC744148.png)

<span data-ttu-id="29bff-136">Obrázek 3 - obchodních aplikací, které jsou hostovány za hello LB koncový bod</span><span class="sxs-lookup"><span data-stu-id="29bff-136">Figure 3 - LOB applications hosted behind hello LB endpoint</span></span>

<span data-ttu-id="29bff-137">Další možností pro hello LOB je toohave lokality toosite VPN toohello virtuální síti, kde koncovým bodem ILB hello je nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="29bff-137">Another scenario for hello LOB is toohave a site toosite VPN toohello virtual network where hello ILB endpoint is configured.</span></span> <span data-ttu-id="29bff-138">To umožňuje místní síťové přenosy toobe směrovány toohello koncovým bodem ILB.</span><span class="sxs-lookup"><span data-stu-id="29bff-138">This allows on-premises network traffic toobe routed toohello ILB endpoint.</span></span>

![Interní rozložení zátěže pomocí site toosite VPN](./media/load-balancer-internal-overview/IC744150.png)

<span data-ttu-id="29bff-140">Obrázek 4 – místní síťový provoz směrovat koncovým bodem ILB toohello</span><span class="sxs-lookup"><span data-stu-id="29bff-140">Figure 4 - On-premises network traffic routed toohello ILB endpoint</span></span>

## <a name="limitations"></a><span data-ttu-id="29bff-141">Omezení</span><span class="sxs-lookup"><span data-stu-id="29bff-141">Limitations</span></span>

<span data-ttu-id="29bff-142">Interní nástroj pro vyrovnávání zatížení konfigurace nepodporují překládat pomocí SNAT.</span><span class="sxs-lookup"><span data-stu-id="29bff-142">Internal Load Balancer configurations do not support SNAT.</span></span> <span data-ttu-id="29bff-143">V kontextu tohoto dokumentu hello odkazuje překládat pomocí SNAT tooport podvržený zdroj překlad síťových adres.</span><span class="sxs-lookup"><span data-stu-id="29bff-143">In hello context of this document, SNAT refers tooport masquerading source  network address translation.</span></span>  <span data-ttu-id="29bff-144">To platí tooscenarios kde virtuálního počítače ve fondu vyrovnávání zatížení musí tooreach hello příslušné interní nástroj pro vyrovnávání zatížení na front-endovou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="29bff-144">This applies tooscenarios where a VM in a load balancer pool needs tooreach hello respective internal Load Balancer's frontend IP address.</span></span> <span data-ttu-id="29bff-145">Tento scénář není podporován pro interní nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="29bff-145">This scenario is not supported for internal Load Balancer.</span></span> <span data-ttu-id="29bff-146">Počet selhání připojení se stane, když hello tok je toohello skupinu s vyrovnáváním zatížení virtuálních počítačů, které pocházely hello toku.</span><span class="sxs-lookup"><span data-stu-id="29bff-146">Connection failures will occur when hello flow is load balanced toohello VM which originated hello flow.</span></span> <span data-ttu-id="29bff-147">Je nutné použít styl proxy serveru, nástroje pro vyrovnávání zatížení pro takové scénáře.</span><span class="sxs-lookup"><span data-stu-id="29bff-147">You must use a proxy style load balancer for such scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29bff-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29bff-148">Next Steps</span></span>

[<span data-ttu-id="29bff-149">Podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="29bff-149">Azure Resource Manager support for Azure Load Balancer</span></span>](load-balancer-arm.md)

[<span data-ttu-id="29bff-150">Začít konfigurovat internetové nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="29bff-150">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="29bff-151">Začněte konfiguraci Vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="29bff-151">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="29bff-152">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="29bff-152">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="29bff-153">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="29bff-153">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
