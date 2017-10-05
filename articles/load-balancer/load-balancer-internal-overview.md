---
title: "Nástroj pro vyrovnávání zatížení pro vnitřní přehled | Microsoft Docs"
description: "Přehled pro interní vyrovnávání zátěže a jejich funkce. Jak funguje nástroj pro vyrovnávání zatížení pro Azure a možných scénářů konfigurace vnitřních koncových bodů"
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
ms.openlocfilehash: d324aaf8ec2c8766d5cf11452158d14c19cba4d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="internal-load-balancer-overview"></a><span data-ttu-id="1ceef-103">Přehled nástroje pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="1ceef-103">Internal load balancer overview</span></span>

<span data-ttu-id="1ceef-104">Na rozdíl od internetový bod nástroj pro vyrovnávání zatížení vyrovnávání interní zatížení (ILB) přesměruje přenosy pouze na prostředků v cloudové službě nebo pomocí sítě VPN pro přístup k infrastruktuře Azure.</span><span class="sxs-lookup"><span data-stu-id="1ceef-104">Unlike the Internet facing load balancer, the internal load balancer (ILB) directs traffic only to resources inside the cloud service or using VPN to access the Azure infrastructure.</span></span> <span data-ttu-id="1ceef-105">Infrastruktury omezuje přístup ke skupině s vyrovnáváním zatížení virtuální IP adresy (VIP) Cloudovou službu nebo virtuální sítě tak, aby se nikdy se zveřejní přímo k Internetu koncový bod.</span><span class="sxs-lookup"><span data-stu-id="1ceef-105">The infrastructure restricts access to the load balanced virtual IP addresses (VIPs) of a Cloud Service or a Virtual Network so that they will never be directly exposed to an Internet endpoint.</span></span> <span data-ttu-id="1ceef-106">To umožňuje interní řádek obchodní (LOB) aplikace pro spouštění v Azure a získat přístup z cloudu nebo z místní prostředky.</span><span class="sxs-lookup"><span data-stu-id="1ceef-106">This enables internal line of business (LOB) applications to run in Azure and be accessed from within the cloud or from resources on-premises.</span></span>

## <a name="why-you-may-need-an-internal-load-balancer"></a><span data-ttu-id="1ceef-107">Může být nutná k nástroji pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="1ceef-107">Why you may need an internal load balancer</span></span>

<span data-ttu-id="1ceef-108">Azure interní načíst vyrovnávání (ILB) poskytuje vyrovnávání zátěže mezi virtuální počítače, které se nacházejí v rámci cloudové služby nebo virtuální síť s místní rozsah.</span><span class="sxs-lookup"><span data-stu-id="1ceef-108">Azure Internal Load Balancing (ILB) provides load balancing between virtual machines that reside inside of a cloud service or a virtual network with a regional scope.</span></span> <span data-ttu-id="1ceef-109">Informace o používání a konfiguraci virtuální sítě s místní rozsah najdete v tématu [regionálních virtuálních sítí](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) blog in the Azure.</span><span class="sxs-lookup"><span data-stu-id="1ceef-109">For information about the use and configuration of virtual networks with a regional scope, see [Regional Virtual Networks](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) in the Azure blog.</span></span> <span data-ttu-id="1ceef-110">Existující virtuální sítě, které jsou nakonfigurované pro skupinu vztahů, nemůžou interní nástroj pro vyrovnávání zatížení používat.</span><span class="sxs-lookup"><span data-stu-id="1ceef-110">Existing virtual networks that have been configured for an affinity group cannot use ILB.</span></span>

<span data-ttu-id="1ceef-111">ILB umožňuje následující typy Vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="1ceef-111">ILB enables the following types of load balancing:</span></span>

* <span data-ttu-id="1ceef-112">V rámci cloudové služby, z virtuálních počítačů na sadu virtuálních počítačů, které se nacházejí v rámci stejné cloudové služby (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="1ceef-112">Within a cloud service, from virtual machines to a set of virtual machines that reside within the same cloud service (see Figure 1).</span></span>
* <span data-ttu-id="1ceef-113">V rámci virtuální sítě z virtuálních počítačů ve virtuální síti na sadu virtuálních počítačů, které se nacházejí v rámci stejné Cloudová služba virtuálního sítě (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="1ceef-113">Within a virtual network, from virtual machines in the virtual network to a set of virtual machines that reside within the same cloud service of the virtual network (see Figure 2).</span></span>
* <span data-ttu-id="1ceef-114">Pro virtuální síť mezi různými místy z sadu virtuálních počítačů, které se nacházejí v rámci stejné cloudové služby virtuálního počítače místní sítě (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="1ceef-114">For a cross-premises virtual network, from on-premises computers to a set of virtual machines that reside within the same cloud service of the virtual network (see Figure 3).</span></span>
* <span data-ttu-id="1ceef-115">Internetový, vícevrstvé aplikace, ve kterých vrstvy back-end nejsou internetového ale vyžadují Vyrovnávání zatížení pro provoz z internetového vrstvy.</span><span class="sxs-lookup"><span data-stu-id="1ceef-115">Internet-facing, multi-tier applications in which the back-end tiers are not Internet-facing but require load balancing for traffic from the Internet-facing tier.</span></span>
* <span data-ttu-id="1ceef-116">Vyrovnávání zatížení pro obchodní aplikace, které jsou hostované v Azure bez nutnosti další zátěž vyrovnávání hardwaru nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="1ceef-116">Load balancing for LOB applications hosted in Azure without requiring additional load balancer hardware or software.</span></span> <span data-ttu-id="1ceef-117">Včetně místních serverů v sadě počítačů, jejichž provoz zatížení vyvážit.</span><span class="sxs-lookup"><span data-stu-id="1ceef-117">Including on-premises servers in the set of computers whose traffic is load balanced.</span></span>

## <a name="internet-facing-multi-tier-applications"></a><span data-ttu-id="1ceef-118">Internetový bod vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="1ceef-118">Internet facing multi-tier applications</span></span>

<span data-ttu-id="1ceef-119">Webová vrstva obsahuje internetových koncových bodů pro internetové klienty a je součástí skupiny s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="1ceef-119">The web tier has Internet facing endpoints for Internet clients and is part of a load-balanced set.</span></span> <span data-ttu-id="1ceef-120">Nástroje pro vyrovnávání zatížení distribuuje příchozí provoz z webových klientů pro port TCP 443 (HTTPS) na webové servery.</span><span class="sxs-lookup"><span data-stu-id="1ceef-120">The load balancer  distributes incoming traffic from web clients for TCP port 443 (HTTPS) to the web servers.</span></span>

<span data-ttu-id="1ceef-121">Databázové servery jsou za koncovým bodem ILB, které webové servery používají pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="1ceef-121">The database servers are behind an ILB endpoint which the web servers use for storage.</span></span> <span data-ttu-id="1ceef-122">Koncový bod, jaký provoz je vyrovnáváno zatížení napříč databázové servery v sadě ILB s vyrovnáváním zatížení služby této databáze.</span><span class="sxs-lookup"><span data-stu-id="1ceef-122">This database service load balanced endpoint, which traffic is load balanced across the database servers in the ILB set.</span></span>

<span data-ttu-id="1ceef-123">Následující obrázek znázorňuje internetové vícevrstvé aplikace v rámci stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="1ceef-123">The following image shows the Internet facing multi-tier application within the same cloud service.</span></span>

![Interní jeden cloud službou Vyrovnávání zatížení](./media/load-balancer-internal-overview/IC736321.png)

<span data-ttu-id="1ceef-125">Obrázek 1 – internetový bod vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="1ceef-125">Figure 1 - Internet facing multi-tier application</span></span>

<span data-ttu-id="1ceef-126">Další využití pro vícevrstvé aplikace je při ILB nasazení v jiné cloudové službě než ten, který využívají službu pro ILB.</span><span class="sxs-lookup"><span data-stu-id="1ceef-126">Another possible use for a multi-tier application is when the ILB deployed to a different cloud service than the one consuming the service for the ILB.</span></span>

<span data-ttu-id="1ceef-127">Cloudové služby pomocí stejné virtuální síti budou mít přístup ke koncovému bodu ILB.</span><span class="sxs-lookup"><span data-stu-id="1ceef-127">Cloud services using the same virtual network will have access to the ILB endpoint.</span></span> <span data-ttu-id="1ceef-128">Následující obrázek znázorňuje front-end webové servery jsou v rámci různých cloudové služby z databáze back-end a pomocí koncovým bodem ILB v rámci stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="1ceef-128">The following image shows front-end web servers are in a different cloud service from the database back-end and using the ILB endpoint within the same virtual network.</span></span>

![Interní vyrovnávání zátěže mezi cloudové služby](./media/load-balancer-internal-overview/IC744147.png)

<span data-ttu-id="1ceef-130">Obrázek 2 – Front-end servery v rámci různých cloudové služby</span><span class="sxs-lookup"><span data-stu-id="1ceef-130">Figure 2 - Front-end servers in a different cloud service</span></span>

## <a name="intranet-line-of-business-applications"></a><span data-ttu-id="1ceef-131">Intranetu řádek obchodní aplikace</span><span class="sxs-lookup"><span data-stu-id="1ceef-131">Intranet line of business applications</span></span>

<span data-ttu-id="1ceef-132">Přenosy od klientů v místní síti získat Vyrovnávání zatížení mezi několik serverů LOB pomocí připojení VPN k síti Azure.</span><span class="sxs-lookup"><span data-stu-id="1ceef-132">Traffic from clients on the on-premises network get load-balanced across the set of LOB servers using VPN connection to Azure network.</span></span>

<span data-ttu-id="1ceef-133">Klientský počítač bude mít přístup k IP adrese ze služby Azure VPN pomocí bodu to-site VPN.</span><span class="sxs-lookup"><span data-stu-id="1ceef-133">The client machine will have access to an IP address from Azure VPN service using point to site VPN.</span></span> <span data-ttu-id="1ceef-134">Umožňuje použít obchodní aplikace hostované za koncovým bodem ILB.</span><span class="sxs-lookup"><span data-stu-id="1ceef-134">It allows the use the LOB application hosted behind the ILB endpoint.</span></span>

![Interní zátěže pomocí bodu to-site VPN](./media/load-balancer-internal-overview/IC744148.png)

<span data-ttu-id="1ceef-136">Obrázek 3 - obchodních aplikací, které jsou hostovány za vyrovnáváním zatížení koncového bodu</span><span class="sxs-lookup"><span data-stu-id="1ceef-136">Figure 3 - LOB applications hosted behind the LB endpoint</span></span>

<span data-ttu-id="1ceef-137">Jiné scénáře objektu LOB se se sítěmi VPN do virtuální sítě, kde je koncovým bodem ILB nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="1ceef-137">Another scenario for the LOB is to have a site to site VPN to the virtual network where the ILB endpoint is configured.</span></span> <span data-ttu-id="1ceef-138">To umožňuje místní síťový provoz směrovat na koncovým bodem ILB.</span><span class="sxs-lookup"><span data-stu-id="1ceef-138">This allows on-premises network traffic to be routed to the ILB endpoint.</span></span>

![Interní zátěže pomocí sítě site to site VPN](./media/load-balancer-internal-overview/IC744150.png)

<span data-ttu-id="1ceef-140">Obrázek 4: místní síťový provoz směrovat na koncovým bodem ILB</span><span class="sxs-lookup"><span data-stu-id="1ceef-140">Figure 4 - On-premises network traffic routed to the ILB endpoint</span></span>

## <a name="limitations"></a><span data-ttu-id="1ceef-141">Omezení</span><span class="sxs-lookup"><span data-stu-id="1ceef-141">Limitations</span></span>

<span data-ttu-id="1ceef-142">Interní nástroj pro vyrovnávání zatížení konfigurace nepodporují překládat pomocí SNAT.</span><span class="sxs-lookup"><span data-stu-id="1ceef-142">Internal Load Balancer configurations do not support SNAT.</span></span> <span data-ttu-id="1ceef-143">V kontextu tohoto dokumentu překládat pomocí SNAT odkazuje na portu podvržený zdroj překlad síťových adres.</span><span class="sxs-lookup"><span data-stu-id="1ceef-143">In the context of this document, SNAT refers to port masquerading source  network address translation.</span></span>  <span data-ttu-id="1ceef-144">To platí pro scénáře, kde je potřeba dosáhnout příslušné interní nástroj pro vyrovnávání zatížení na front-endovou IP adresu virtuálního počítače ve fondu vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="1ceef-144">This applies to scenarios where a VM in a load balancer pool needs to reach the respective internal Load Balancer's frontend IP address.</span></span> <span data-ttu-id="1ceef-145">Tento scénář není podporován pro interní nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="1ceef-145">This scenario is not supported for internal Load Balancer.</span></span> <span data-ttu-id="1ceef-146">Počet selhání připojení se stane, když tok je Vyrovnávání zatížení do virtuálního počítače, které pocházely toku.</span><span class="sxs-lookup"><span data-stu-id="1ceef-146">Connection failures will occur when the flow is load balanced to the VM which originated the flow.</span></span> <span data-ttu-id="1ceef-147">Je nutné použít styl proxy serveru, nástroje pro vyrovnávání zatížení pro takové scénáře.</span><span class="sxs-lookup"><span data-stu-id="1ceef-147">You must use a proxy style load balancer for such scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ceef-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ceef-148">Next Steps</span></span>

[<span data-ttu-id="1ceef-149">Podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="1ceef-149">Azure Resource Manager support for Azure Load Balancer</span></span>](load-balancer-arm.md)

[<span data-ttu-id="1ceef-150">Začít konfigurovat internetové nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1ceef-150">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="1ceef-151">Začněte konfiguraci Vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="1ceef-151">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="1ceef-152">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1ceef-152">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="1ceef-153">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1ceef-153">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
