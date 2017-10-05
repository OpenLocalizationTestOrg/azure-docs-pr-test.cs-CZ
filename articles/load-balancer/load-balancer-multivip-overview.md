---
title: "Nástroj pro vyrovnávání zatížení několika virtuálními IP adresami pro Azure. | Microsoft Docs"
description: "Přehled několika virtuálními IP adresami v nástroji pro vyrovnávání zatížení Azure"
services: load-balancer
documentationcenter: na
author: chkuhtz
manager: narayan
editor: 
ms.assetid: 748e50cd-3087-4c2e-a9e1-ac0ecce4f869
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2016
ms.author: chkuhtz
ms.openlocfilehash: d9e88b859020be2a96a57a01e5624052ed134b64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="multiple-vips-for-azure-load-balancer"></a><span data-ttu-id="df3b1-103">Více virtuálních IP adres pro Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="df3b1-103">Multiple VIPs for Azure Load Balancer</span></span>

<span data-ttu-id="df3b1-104">Azure Vyrovnávání zatížení můžete načíst vyrovnat služby na více porty, více IP adres nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="df3b1-104">Azure Load Balancer allows you to load balance services on multiple ports, multiple IP addresses, or both.</span></span> <span data-ttu-id="df3b1-105">Definice nástroje pro vyrovnávání zatížení veřejné a interní můžete použít pro toky Vyrovnávání zatížení v rámci sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="df3b1-105">You can use public and internal load balancer definitions to load balance flows across a set of VMs.</span></span>

<span data-ttu-id="df3b1-106">Tento článek popisuje základní informace o této schopnosti, důležité koncepty a omezení.</span><span class="sxs-lookup"><span data-stu-id="df3b1-106">This article describes the fundamentals of this ability, important concepts, and constraints.</span></span> <span data-ttu-id="df3b1-107">Pokud chcete vystavit služby na jednu IP adresu, najdete pokyny, zjednodušené [veřejné](load-balancer-get-started-internet-portal.md) nebo [interní](load-balancer-get-started-ilb-arm-portal.md) konfigurací služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="df3b1-107">If you only intend to expose services on one IP address, you can find simplified instructions for [public](load-balancer-get-started-internet-portal.md) or [internal](load-balancer-get-started-ilb-arm-portal.md) load balancer configurations.</span></span> <span data-ttu-id="df3b1-108">Přidání několika virtuálními IP adresami není přírůstkové k konfigurací jedné virtuální IP adresy.</span><span class="sxs-lookup"><span data-stu-id="df3b1-108">Adding Multiple VIPs is incremental to a single VIP configuration.</span></span> <span data-ttu-id="df3b1-109">Pomocí koncepty v tomto článku, můžete rozbalit zjednodušená konfigurace kdykoli.</span><span class="sxs-lookup"><span data-stu-id="df3b1-109">Using the concepts in this article, you can expand a simplified configuration at any time.</span></span>

<span data-ttu-id="df3b1-110">Když definujete k pro vyrovnávání zatížení Azure, front-end a back-end konfigurace jsou připojené pomocí pravidel.</span><span class="sxs-lookup"><span data-stu-id="df3b1-110">When you define an Azure Load Balancer, a frontend and a backend configuration are connected with rules.</span></span> <span data-ttu-id="df3b1-111">Test stavu odkazuje pravidlo se používá k určení jak nové toků se odesílají do uzlu ve fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="df3b1-111">The health probe referenced by the rule is used to determine how new flows are sent to a node in the backend pool.</span></span> <span data-ttu-id="df3b1-112">Front-endu je definována pomocí virtuální IP (VIP), což je 3 řazené kolekce členů skládá z adresy IP (veřejné nebo interní), přenosový protokol (UDP nebo TCP) a číslo portu.</span><span class="sxs-lookup"><span data-stu-id="df3b1-112">The frontend is defined by a Virtual IP (VIP), which is a 3-tuple comprised of an IP address (public or internal), a transport protocol (UDP or TCP), and a port number.</span></span> <span data-ttu-id="df3b1-113">Vyhrazené IP adresy je IP adresa na Azure virtuální síťovou kartu připojenou k virtuálnímu počítači ve fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="df3b1-113">A DIP is an IP address on an Azure virtual NIC attached to a VM in the backend pool.</span></span>

<span data-ttu-id="df3b1-114">Následující tabulka obsahuje některé příklady front-endové konfigurace:</span><span class="sxs-lookup"><span data-stu-id="df3b1-114">The following table contains some example frontend configurations:</span></span>

| <span data-ttu-id="df3b1-115">VIRTUÁLNÍ ADRESA IP.</span><span class="sxs-lookup"><span data-stu-id="df3b1-115">VIP</span></span> | <span data-ttu-id="df3b1-116">IP adresa</span><span class="sxs-lookup"><span data-stu-id="df3b1-116">IP address</span></span> | <span data-ttu-id="df3b1-117">Protokol</span><span class="sxs-lookup"><span data-stu-id="df3b1-117">protocol</span></span> | <span data-ttu-id="df3b1-118">port</span><span class="sxs-lookup"><span data-stu-id="df3b1-118">port</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="df3b1-119">1</span><span class="sxs-lookup"><span data-stu-id="df3b1-119">1</span></span> |<span data-ttu-id="df3b1-120">65.52.0.1</span><span class="sxs-lookup"><span data-stu-id="df3b1-120">65.52.0.1</span></span> |<span data-ttu-id="df3b1-121">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-121">TCP</span></span> |<span data-ttu-id="df3b1-122">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-122">80</span></span> |
| <span data-ttu-id="df3b1-123">2</span><span class="sxs-lookup"><span data-stu-id="df3b1-123">2</span></span> |<span data-ttu-id="df3b1-124">65.52.0.1</span><span class="sxs-lookup"><span data-stu-id="df3b1-124">65.52.0.1</span></span> |<span data-ttu-id="df3b1-125">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-125">TCP</span></span> |<span data-ttu-id="df3b1-126">*8080*</span><span class="sxs-lookup"><span data-stu-id="df3b1-126">*8080*</span></span> |
| <span data-ttu-id="df3b1-127">3</span><span class="sxs-lookup"><span data-stu-id="df3b1-127">3</span></span> |<span data-ttu-id="df3b1-128">65.52.0.1</span><span class="sxs-lookup"><span data-stu-id="df3b1-128">65.52.0.1</span></span> |<span data-ttu-id="df3b1-129">*UDP*</span><span class="sxs-lookup"><span data-stu-id="df3b1-129">*UDP*</span></span> |<span data-ttu-id="df3b1-130">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-130">80</span></span> |
| <span data-ttu-id="df3b1-131">4</span><span class="sxs-lookup"><span data-stu-id="df3b1-131">4</span></span> |<span data-ttu-id="df3b1-132">*65.52.0.2*</span><span class="sxs-lookup"><span data-stu-id="df3b1-132">*65.52.0.2*</span></span> |<span data-ttu-id="df3b1-133">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-133">TCP</span></span> |<span data-ttu-id="df3b1-134">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-134">80</span></span> |

<span data-ttu-id="df3b1-135">V tabulce jsou čtyři různé frontends.</span><span class="sxs-lookup"><span data-stu-id="df3b1-135">The table shows four different frontends.</span></span> <span data-ttu-id="df3b1-136">Frontends č. 1, #2 a #3 jsou jedné virtuální IP adresy s více pravidel.</span><span class="sxs-lookup"><span data-stu-id="df3b1-136">Frontends #1, #2 and #3 are a single VIP with multiple rules.</span></span> <span data-ttu-id="df3b1-137">Se používá stejnou IP adresu, ale portu nebo protokolu se liší pro každý front-endu.</span><span class="sxs-lookup"><span data-stu-id="df3b1-137">The same IP address is used but the port or protocol is different for each frontend.</span></span> <span data-ttu-id="df3b1-138">Frontends č. 1 a #4 jsou příklad několika virtuálními IP adresami, kde stejný front-endu protokol a port se opětovně použít napříč několika virtuálními IP adresami.</span><span class="sxs-lookup"><span data-stu-id="df3b1-138">Frontends #1 and #4 are an example of Multiple VIPs, where the same frontend protocol and port are reused across multiple VIPs.</span></span>

<span data-ttu-id="df3b1-139">Azure Vyrovnávání zatížení poskytuje flexibilitu při definování pravidel Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="df3b1-139">Azure Load Balancer provides flexibility in defining the load balancing rules.</span></span> <span data-ttu-id="df3b1-140">Pravidlo deklaruje, jak adresu a port front-endu je namapovaný na cílovou adresu a port na back-end.</span><span class="sxs-lookup"><span data-stu-id="df3b1-140">A rule declares how an address and port on the frontend is mapped to the destination address and port on the backend.</span></span> <span data-ttu-id="df3b1-141">Zda jsou porty back-end opětovně použít napříč pravidla závisí na typu pravidla.</span><span class="sxs-lookup"><span data-stu-id="df3b1-141">Whether or not backend ports are reused across rules depends on the type of the rule.</span></span> <span data-ttu-id="df3b1-142">Každý typ pravidla má specifické požadavky, které můžou ovlivnit návrh a konfigurace testu hostitele.</span><span class="sxs-lookup"><span data-stu-id="df3b1-142">Each type of rule has specific requirements that can affect host configuration and probe design.</span></span> <span data-ttu-id="df3b1-143">Existují dva typy pravidel:</span><span class="sxs-lookup"><span data-stu-id="df3b1-143">There are two types of rules:</span></span>

1. <span data-ttu-id="df3b1-144">Výchozí pravidlo s opakované použití žádné back-end port</span><span class="sxs-lookup"><span data-stu-id="df3b1-144">The default rule with no backend port reuse</span></span>
2. <span data-ttu-id="df3b1-145">Pravidlo plovoucí IP adresa, kde jsou opakovaně porty back-end</span><span class="sxs-lookup"><span data-stu-id="df3b1-145">The Floating IP rule where backend ports are reused</span></span>

<span data-ttu-id="df3b1-146">Azure Vyrovnávání zatížení můžete kombinovat oba typy pravidel na stejnou konfiguraci služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="df3b1-146">Azure Load Balancer allows you to mix both rule types on the same load balancer configuration.</span></span> <span data-ttu-id="df3b1-147">Nástroje pro vyrovnávání zatížení můžete používat současně pro daný virtuální počítač nebo libovolnou kombinaci tak dlouho, dokud dodržovat omezení pravidla.</span><span class="sxs-lookup"><span data-stu-id="df3b1-147">The load balancer can use them simultaneously for a given VM, or any combination, as long as you abide by the constraints of the rule.</span></span> <span data-ttu-id="df3b1-148">Jaký typ pravidla zvolíte závisí na požadavky na aplikace a složitost podpora této konfigurace.</span><span class="sxs-lookup"><span data-stu-id="df3b1-148">Which rule type you choose depends on the requirements of your application and the complexity of supporting that configuration.</span></span> <span data-ttu-id="df3b1-149">Byste měli zvážit typů pravidlo, které jsou vhodné pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="df3b1-149">You should evaluate which rule types are best for your scenario.</span></span>

<span data-ttu-id="df3b1-150">Nám prozkoumat tyto další scénáře od výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="df3b1-150">We explore these scenarios further by starting with the default behavior.</span></span>

## <a name="rule-type-1-no-backend-port-reuse"></a><span data-ttu-id="df3b1-151">Pravidlo typu #1: žádné opakované použití portu back-end</span><span class="sxs-lookup"><span data-stu-id="df3b1-151">Rule type #1: No backend port reuse</span></span>

![Obrázek víc virtuálními IP adresami](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

<span data-ttu-id="df3b1-153">V tomto scénáři se virtuální IP adresy front-endu nakonfigurovány takto:</span><span class="sxs-lookup"><span data-stu-id="df3b1-153">In this scenario, the frontend VIPs are configured as follows:</span></span>

| <span data-ttu-id="df3b1-154">VIRTUÁLNÍ ADRESA IP.</span><span class="sxs-lookup"><span data-stu-id="df3b1-154">VIP</span></span> | <span data-ttu-id="df3b1-155">IP adresa</span><span class="sxs-lookup"><span data-stu-id="df3b1-155">IP address</span></span> | <span data-ttu-id="df3b1-156">Protokol</span><span class="sxs-lookup"><span data-stu-id="df3b1-156">protocol</span></span> | <span data-ttu-id="df3b1-157">port</span><span class="sxs-lookup"><span data-stu-id="df3b1-157">port</span></span> |
| --- | --- | --- | --- |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) <span data-ttu-id="df3b1-159">1</span><span class="sxs-lookup"><span data-stu-id="df3b1-159">1</span></span> |<span data-ttu-id="df3b1-160">65.52.0.1</span><span class="sxs-lookup"><span data-stu-id="df3b1-160">65.52.0.1</span></span> |<span data-ttu-id="df3b1-161">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-161">TCP</span></span> |<span data-ttu-id="df3b1-162">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-162">80</span></span> |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) <span data-ttu-id="df3b1-164">2</span><span class="sxs-lookup"><span data-stu-id="df3b1-164">2</span></span> |<span data-ttu-id="df3b1-165">*65.52.0.2*</span><span class="sxs-lookup"><span data-stu-id="df3b1-165">*65.52.0.2*</span></span> |<span data-ttu-id="df3b1-166">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-166">TCP</span></span> |<span data-ttu-id="df3b1-167">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-167">80</span></span> |

<span data-ttu-id="df3b1-168">DIP je cílem příchozí tok.</span><span class="sxs-lookup"><span data-stu-id="df3b1-168">The DIP is the destination of the inbound flow.</span></span> <span data-ttu-id="df3b1-169">Každý virtuální počítač ve fondu back-end zpřístupní požadované služby v jedinečný port vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="df3b1-169">In the backend pool, each VM exposes the desired service on a unique port on a DIP.</span></span> <span data-ttu-id="df3b1-170">Tato služba je spojeno s front-endu prostřednictvím definice pravidla.</span><span class="sxs-lookup"><span data-stu-id="df3b1-170">This service is associated with the frontend through a rule definition.</span></span>

<span data-ttu-id="df3b1-171">Jsme definovali dvě pravidla:</span><span class="sxs-lookup"><span data-stu-id="df3b1-171">We define two rules:</span></span>

| <span data-ttu-id="df3b1-172">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="df3b1-172">Rule</span></span> | <span data-ttu-id="df3b1-173">Mapování front-endu</span><span class="sxs-lookup"><span data-stu-id="df3b1-173">Map frontend</span></span> | <span data-ttu-id="df3b1-174">Do fondu back-end</span><span class="sxs-lookup"><span data-stu-id="df3b1-174">To backend pool</span></span> |
| --- | --- | --- |
| <span data-ttu-id="df3b1-175">1</span><span class="sxs-lookup"><span data-stu-id="df3b1-175">1</span></span> |![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) <span data-ttu-id="df3b1-177">VIP1:80</span><span class="sxs-lookup"><span data-stu-id="df3b1-177">VIP1:80</span></span> |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) <span data-ttu-id="df3b1-179">DIP1:80,</span><span class="sxs-lookup"><span data-stu-id="df3b1-179">DIP1:80,</span></span> ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) <span data-ttu-id="df3b1-181">DIP2:80</span><span class="sxs-lookup"><span data-stu-id="df3b1-181">DIP2:80</span></span> |
| <span data-ttu-id="df3b1-182">2</span><span class="sxs-lookup"><span data-stu-id="df3b1-182">2</span></span> |![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) <span data-ttu-id="df3b1-184">VIP2:80</span><span class="sxs-lookup"><span data-stu-id="df3b1-184">VIP2:80</span></span> |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) <span data-ttu-id="df3b1-186">DIP1:81,</span><span class="sxs-lookup"><span data-stu-id="df3b1-186">DIP1:81,</span></span> ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) <span data-ttu-id="df3b1-188">DIP2:81</span><span class="sxs-lookup"><span data-stu-id="df3b1-188">DIP2:81</span></span> |

<span data-ttu-id="df3b1-189">Dokončení mapování nástroji pro vyrovnávání zatížení Azure je teď následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="df3b1-189">The complete mapping in Azure Load Balancer is now as follows:</span></span>

| <span data-ttu-id="df3b1-190">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="df3b1-190">Rule</span></span> | <span data-ttu-id="df3b1-191">Virtuální IP adresy IP adresu</span><span class="sxs-lookup"><span data-stu-id="df3b1-191">VIP IP address</span></span> | <span data-ttu-id="df3b1-192">Protokol</span><span class="sxs-lookup"><span data-stu-id="df3b1-192">protocol</span></span> | <span data-ttu-id="df3b1-193">port</span><span class="sxs-lookup"><span data-stu-id="df3b1-193">port</span></span> | <span data-ttu-id="df3b1-194">Cíl</span><span class="sxs-lookup"><span data-stu-id="df3b1-194">Destination</span></span> | <span data-ttu-id="df3b1-195">port</span><span class="sxs-lookup"><span data-stu-id="df3b1-195">port</span></span> |
| --- | --- | --- | --- | --- | --- |
| ![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) <span data-ttu-id="df3b1-197">1</span><span class="sxs-lookup"><span data-stu-id="df3b1-197">1</span></span> |<span data-ttu-id="df3b1-198">65.52.0.1</span><span class="sxs-lookup"><span data-stu-id="df3b1-198">65.52.0.1</span></span> |<span data-ttu-id="df3b1-199">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-199">TCP</span></span> |<span data-ttu-id="df3b1-200">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-200">80</span></span> |<span data-ttu-id="df3b1-201">Vyhrazené IP adresy IP adresu</span><span class="sxs-lookup"><span data-stu-id="df3b1-201">DIP IP Address</span></span> |<span data-ttu-id="df3b1-202">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-202">80</span></span> |
| ![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) <span data-ttu-id="df3b1-204">2</span><span class="sxs-lookup"><span data-stu-id="df3b1-204">2</span></span> |<span data-ttu-id="df3b1-205">65.52.0.2</span><span class="sxs-lookup"><span data-stu-id="df3b1-205">65.52.0.2</span></span> |<span data-ttu-id="df3b1-206">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-206">TCP</span></span> |<span data-ttu-id="df3b1-207">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-207">80</span></span> |<span data-ttu-id="df3b1-208">Vyhrazené IP adresy IP adresu</span><span class="sxs-lookup"><span data-stu-id="df3b1-208">DIP IP Address</span></span> |<span data-ttu-id="df3b1-209">81</span><span class="sxs-lookup"><span data-stu-id="df3b1-209">81</span></span> |

<span data-ttu-id="df3b1-210">Každé pravidlo musí vytvořit toku s jedinečnou kombinaci cílové IP adresy a cílového portu.</span><span class="sxs-lookup"><span data-stu-id="df3b1-210">Each rule must produce a flow with a unique combination of destination IP address and destination port.</span></span> <span data-ttu-id="df3b1-211">Pomocí různých cílový port toku, více pravidel doručovat toky do stejné DIP na jiné porty.</span><span class="sxs-lookup"><span data-stu-id="df3b1-211">By varying the destination port of the flow, multiple rules can deliver flows to the same DIP on different ports.</span></span>

<span data-ttu-id="df3b1-212">Sondy stavu jsou vždy směrovala vyhrazené IP adresy virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="df3b1-212">Health probes are always directed to the DIP of a VM.</span></span> <span data-ttu-id="df3b1-213">Musíte si zajistěte, aby vaše testu odráží stav virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="df3b1-213">You must ensure you that your probe reflects the health of the VM.</span></span>

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a><span data-ttu-id="df3b1-214">Pravidlo typu #2: opakované použití portu back-end pomocí plovoucí IP adresa</span><span class="sxs-lookup"><span data-stu-id="df3b1-214">Rule type #2: backend port reuse by using Floating IP</span></span>

<span data-ttu-id="df3b1-215">Azure nástroj pro vyrovnávání zatížení poskytuje možnost opakovaně použít front-endový port mezi několika virtuálními IP adresami bez ohledu na typ pravidla používat.</span><span class="sxs-lookup"><span data-stu-id="df3b1-215">Azure Load Balancer provides the flexibility to reuse the frontend port across multiple VIPs regardless of the rule type used.</span></span> <span data-ttu-id="df3b1-216">Kromě toho některé aplikace scénáře raději nebo vyžadovat stejný port, který bude používat více instancí aplikace na jeden virtuální počítač ve fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="df3b1-216">Additionally, some application scenarios prefer or require the same port to be used by multiple application instances on a single VM in the backend pool.</span></span> <span data-ttu-id="df3b1-217">Běžných příkladů opakované použití portu obsahovat clustering pro vysokou dostupnost, sítě, virtuální zařízení a vystavení několik koncových bodů protokolu TLS bez znova šifrovat.</span><span class="sxs-lookup"><span data-stu-id="df3b1-217">Common examples of port reuse include clustering for high availability, network virtual appliances, and exposing multiple TLS endpoints without re-encryption.</span></span>

<span data-ttu-id="df3b1-218">Pokud chcete znovu použít back-endový port napříč více pravidel, je nutné povolit plovoucí IP adresa v definice pravidla.</span><span class="sxs-lookup"><span data-stu-id="df3b1-218">If you want to reuse the backend port across multiple rules, you must enable Floating IP in the rule definition.</span></span>

<span data-ttu-id="df3b1-219">Plovoucí IP adresa je část, která se označuje jako přímý Server vrátit (DSR).</span><span class="sxs-lookup"><span data-stu-id="df3b1-219">Floating IP is a portion of what is known as Direct Server Return (DSR).</span></span> <span data-ttu-id="df3b1-220">DSR se skládá ze dvou částí: topologii toku a IP adres schéma mapování.</span><span class="sxs-lookup"><span data-stu-id="df3b1-220">DSR consists of two parts: a flow topology and an IP address mapping scheme.</span></span> <span data-ttu-id="df3b1-221">Na úrovni platformy nástroj pro vyrovnávání zatížení Azure vždy funguje v topologii DSR toku bez ohledu na to, jestli je zapnutá plovoucí IP adresa, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="df3b1-221">At a platform level, Azure Load Balancer always operates in a DSR flow topology regardless of whether Floating IP is enabled or not.</span></span> <span data-ttu-id="df3b1-222">To znamená, že část odchozí tok je vždy správně přepisuje, aby toku přímo zpět k počátku.</span><span class="sxs-lookup"><span data-stu-id="df3b1-222">This means that the outbound part of a flow is always correctly rewritten to flow directly back to the origin.</span></span>

<span data-ttu-id="df3b1-223">S typem výchozí pravidla zpřístupní Azure tradiční schéma mapování IP adres pro snadné použití vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="df3b1-223">With the default rule type, Azure exposes a traditional load balancing IP address mapping scheme for ease of use.</span></span> <span data-ttu-id="df3b1-224">Povolení plovoucí IP adresa změní schéma mapování IP adres, povolit za účelem vyšší flexibility, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="df3b1-224">Enabling Floating IP changes the IP address mapping scheme to allow for additional flexibility as explained below.</span></span>

<span data-ttu-id="df3b1-225">Následující obrázek znázorňuje tuto konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="df3b1-225">The following diagram illustrates this configuration:</span></span>

![Obrázek víc virtuálními IP adresami](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

<span data-ttu-id="df3b1-227">V tomto scénáři má každý virtuální počítač ve fondu back-end tři síťových rozhraní:</span><span class="sxs-lookup"><span data-stu-id="df3b1-227">For this scenario, every VM in the backend pool has three network interfaces:</span></span>

* <span data-ttu-id="df3b1-228">Vyhrazené IP adresy: virtuální síťový adaptér přidružený virtuální počítač (Konfigurace IP Síťových prostředků Azure)</span><span class="sxs-lookup"><span data-stu-id="df3b1-228">DIP: a Virtual NIC associated with the VM (IP configuration of Azure's NIC resource)</span></span>
* <span data-ttu-id="df3b1-229">VIP1: rozhraní zpětné smyčky v rámci hostovaného operačního systému, který je nakonfigurovaný s IP adresou VIP1</span><span class="sxs-lookup"><span data-stu-id="df3b1-229">VIP1: a loopback interface within guest OS that is configured with IP address of VIP1</span></span>
* <span data-ttu-id="df3b1-230">VIP2: rozhraní zpětné smyčky v rámci hostovaného operačního systému, který je nakonfigurovaný s IP adresou VIP2</span><span class="sxs-lookup"><span data-stu-id="df3b1-230">VIP2: a loopback interface within guest OS that is configured with IP address of VIP2</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df3b1-231">Konfigurace logické rozhraní se provádí v rámci hostovaného operačního systému.</span><span class="sxs-lookup"><span data-stu-id="df3b1-231">The configuration of the logical interfaces is performed within the guest OS.</span></span> <span data-ttu-id="df3b1-232">Tato konfigurace není provést nebo spravované přes Azure.</span><span class="sxs-lookup"><span data-stu-id="df3b1-232">This configuration is not performed or managed by Azure.</span></span> <span data-ttu-id="df3b1-233">Bez této konfiguraci nebude fungovat pravidla.</span><span class="sxs-lookup"><span data-stu-id="df3b1-233">Without this configuration, the rules will not function.</span></span> <span data-ttu-id="df3b1-234">Stav testu definice používat vyhrazené IP adresy virtuálního počítače, nikoli logické VIP.</span><span class="sxs-lookup"><span data-stu-id="df3b1-234">Health probe definitions use the DIP of the VM rather than the logical VIP.</span></span> <span data-ttu-id="df3b1-235">Proto služby, musíte zadat odpovědi testu portu vyhrazené IP adresy, které odráží stav služby nabízených na logické VIP.</span><span class="sxs-lookup"><span data-stu-id="df3b1-235">Therefore, your service must provide probe responses on a DIP port that reflect the status of the service offered on the logical VIP.</span></span>

<span data-ttu-id="df3b1-236">Předpokládejme stejný front-endovou konfiguraci jako v předchozím scénáři:</span><span class="sxs-lookup"><span data-stu-id="df3b1-236">Let's assume the same frontend configuration as in the previous scenario:</span></span>

| <span data-ttu-id="df3b1-237">VIRTUÁLNÍ ADRESA IP.</span><span class="sxs-lookup"><span data-stu-id="df3b1-237">VIP</span></span> | <span data-ttu-id="df3b1-238">IP adresa</span><span class="sxs-lookup"><span data-stu-id="df3b1-238">IP address</span></span> | <span data-ttu-id="df3b1-239">Protokol</span><span class="sxs-lookup"><span data-stu-id="df3b1-239">protocol</span></span> | <span data-ttu-id="df3b1-240">port</span><span class="sxs-lookup"><span data-stu-id="df3b1-240">port</span></span> |
| --- | --- | --- | --- |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) <span data-ttu-id="df3b1-242">1</span><span class="sxs-lookup"><span data-stu-id="df3b1-242">1</span></span> |<span data-ttu-id="df3b1-243">65.52.0.1</span><span class="sxs-lookup"><span data-stu-id="df3b1-243">65.52.0.1</span></span> |<span data-ttu-id="df3b1-244">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-244">TCP</span></span> |<span data-ttu-id="df3b1-245">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-245">80</span></span> |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) <span data-ttu-id="df3b1-247">2</span><span class="sxs-lookup"><span data-stu-id="df3b1-247">2</span></span> |<span data-ttu-id="df3b1-248">*65.52.0.2*</span><span class="sxs-lookup"><span data-stu-id="df3b1-248">*65.52.0.2*</span></span> |<span data-ttu-id="df3b1-249">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-249">TCP</span></span> |<span data-ttu-id="df3b1-250">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-250">80</span></span> |

<span data-ttu-id="df3b1-251">Jsme definovali dvě pravidla:</span><span class="sxs-lookup"><span data-stu-id="df3b1-251">We define two rules:</span></span>

| <span data-ttu-id="df3b1-252">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="df3b1-252">Rule</span></span> | <span data-ttu-id="df3b1-253">Mapování front-endu</span><span class="sxs-lookup"><span data-stu-id="df3b1-253">Map frontend</span></span> | <span data-ttu-id="df3b1-254">Do fondu back-end</span><span class="sxs-lookup"><span data-stu-id="df3b1-254">To backend pool</span></span> |
| --- | --- | --- |
| <span data-ttu-id="df3b1-255">1</span><span class="sxs-lookup"><span data-stu-id="df3b1-255">1</span></span> |![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) <span data-ttu-id="df3b1-257">VIP1:80</span><span class="sxs-lookup"><span data-stu-id="df3b1-257">VIP1:80</span></span> |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) <span data-ttu-id="df3b1-259">VIP1:80 (v VM1 a virtuálního počítače 2)</span><span class="sxs-lookup"><span data-stu-id="df3b1-259">VIP1:80 (in VM1 and VM2)</span></span> |
| <span data-ttu-id="df3b1-260">2</span><span class="sxs-lookup"><span data-stu-id="df3b1-260">2</span></span> |![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) <span data-ttu-id="df3b1-262">VIP2:80</span><span class="sxs-lookup"><span data-stu-id="df3b1-262">VIP2:80</span></span> |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) <span data-ttu-id="df3b1-264">VIP2:80 (v VM1 a virtuálního počítače 2)</span><span class="sxs-lookup"><span data-stu-id="df3b1-264">VIP2:80 (in VM1 and VM2)</span></span> |

<span data-ttu-id="df3b1-265">V následující tabulce jsou uvedeny dokončení mapování nástroji pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="df3b1-265">The following table shows the complete mapping in the load balancer:</span></span>

| <span data-ttu-id="df3b1-266">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="df3b1-266">Rule</span></span> | <span data-ttu-id="df3b1-267">Virtuální IP adresy IP adresu</span><span class="sxs-lookup"><span data-stu-id="df3b1-267">VIP IP address</span></span> | <span data-ttu-id="df3b1-268">Protokol</span><span class="sxs-lookup"><span data-stu-id="df3b1-268">protocol</span></span> | <span data-ttu-id="df3b1-269">port</span><span class="sxs-lookup"><span data-stu-id="df3b1-269">port</span></span> | <span data-ttu-id="df3b1-270">Cíl</span><span class="sxs-lookup"><span data-stu-id="df3b1-270">Destination</span></span> | <span data-ttu-id="df3b1-271">port</span><span class="sxs-lookup"><span data-stu-id="df3b1-271">port</span></span> |
| --- | --- | --- | --- | --- | --- |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) <span data-ttu-id="df3b1-273">1</span><span class="sxs-lookup"><span data-stu-id="df3b1-273">1</span></span> |<span data-ttu-id="df3b1-274">65.52.0.1</span><span class="sxs-lookup"><span data-stu-id="df3b1-274">65.52.0.1</span></span> |<span data-ttu-id="df3b1-275">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-275">TCP</span></span> |<span data-ttu-id="df3b1-276">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-276">80</span></span> |<span data-ttu-id="df3b1-277">stejné jako VIP (65.52.0.1)</span><span class="sxs-lookup"><span data-stu-id="df3b1-277">same as VIP (65.52.0.1)</span></span> |<span data-ttu-id="df3b1-278">stejné jako VIP (80)</span><span class="sxs-lookup"><span data-stu-id="df3b1-278">same as VIP (80)</span></span> |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) <span data-ttu-id="df3b1-280">2</span><span class="sxs-lookup"><span data-stu-id="df3b1-280">2</span></span> |<span data-ttu-id="df3b1-281">65.52.0.2</span><span class="sxs-lookup"><span data-stu-id="df3b1-281">65.52.0.2</span></span> |<span data-ttu-id="df3b1-282">TCP</span><span class="sxs-lookup"><span data-stu-id="df3b1-282">TCP</span></span> |<span data-ttu-id="df3b1-283">80</span><span class="sxs-lookup"><span data-stu-id="df3b1-283">80</span></span> |<span data-ttu-id="df3b1-284">stejné jako VIP (65.52.0.2)</span><span class="sxs-lookup"><span data-stu-id="df3b1-284">same as VIP (65.52.0.2)</span></span> |<span data-ttu-id="df3b1-285">stejné jako VIP (80)</span><span class="sxs-lookup"><span data-stu-id="df3b1-285">same as VIP (80)</span></span> |

<span data-ttu-id="df3b1-286">Cílovým serverem příchozí tok je adresa VIP na rozhraní zpětné smyčky ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="df3b1-286">The destination of the inbound flow is the VIP address on the loopback interface in the VM.</span></span> <span data-ttu-id="df3b1-287">Každé pravidlo musí vytvořit toku s jedinečnou kombinaci cílové IP adresy a cílového portu.</span><span class="sxs-lookup"><span data-stu-id="df3b1-287">Each rule must produce a flow with a unique combination of destination IP address and destination port.</span></span> <span data-ttu-id="df3b1-288">Pomocí různých cílovou IP adresu toku, opakované použití portu je možné na stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="df3b1-288">By varying the destination IP address of the flow, port reuse is possible on the same VM.</span></span> <span data-ttu-id="df3b1-289">Služby je vystaven pro vyrovnávání zatížení pomocí vytvoření vazby virtuální IP adresy IP adresu a port rozhraní příslušných zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="df3b1-289">Your service is exposed to the load balancer by binding it to the VIP’s IP address and port of the respective loopback interface.</span></span>

<span data-ttu-id="df3b1-290">Všimněte si, že v tomto příkladu nezmění cílový port.</span><span class="sxs-lookup"><span data-stu-id="df3b1-290">Notice that this example does not change the destination port.</span></span> <span data-ttu-id="df3b1-291">I když se jedná o scénář plovoucí IP adresa, podporuje nástroj pro vyrovnávání zatížení Azure také definice pravidla přepsání cílový port back-end a aby se liší od cílový port front-endu.</span><span class="sxs-lookup"><span data-stu-id="df3b1-291">Even though this is a Floating IP scenario, Azure Load Balancer also supports defining a rule to rewrite the backend destination port and to make it different from the frontend destination port.</span></span>

<span data-ttu-id="df3b1-292">Typ pravidla plovoucí IP adresa je základ pro několik vzorů konfigurace služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="df3b1-292">The Floating IP rule type is the foundation of several load balancer configuration patterns.</span></span> <span data-ttu-id="df3b1-293">Příkladem, který je aktuálně k dispozici je [technologie AlwaysOn serveru SQL s více naslouchací procesy](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="df3b1-293">One example that is currently available is the [SQL AlwaysOn with Multiple Listeners](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) configuration.</span></span> <span data-ttu-id="df3b1-294">V průběhu času jsme se dokumentů z uvedených scénářů.</span><span class="sxs-lookup"><span data-stu-id="df3b1-294">Over time, we will document more of these scenarios.</span></span>

## <a name="limitations"></a><span data-ttu-id="df3b1-295">Omezení</span><span class="sxs-lookup"><span data-stu-id="df3b1-295">Limitations</span></span>

* <span data-ttu-id="df3b1-296">Více konfigurací virtuálních IP adres jsou podporovány pouze s virtuální počítače IaaS.</span><span class="sxs-lookup"><span data-stu-id="df3b1-296">Multiple VIP configurations are only supported with IaaS VMs.</span></span>
* <span data-ttu-id="df3b1-297">S tímto pravidlem plovoucí IP adresa musí vaše aplikace používat DIP pro odchozí toky.</span><span class="sxs-lookup"><span data-stu-id="df3b1-297">With the Floating IP rule, your application must use the DIP for outbound flows.</span></span> <span data-ttu-id="df3b1-298">Pokud vaše aplikace se váže k adresa VIP konfigurována na rozhraní zpětné smyčky v hostovaný operační systém, pak není k dispozici přepsání odchozího toku překládat pomocí SNAT a toku nezdaří.</span><span class="sxs-lookup"><span data-stu-id="df3b1-298">If your application binds to the VIP address configured on the loopback interface in the guest OS, then SNAT is not available to rewrite the outbound flow and the flow fails.</span></span>
* <span data-ttu-id="df3b1-299">Veřejné IP adresy mají vliv na fakturace.</span><span class="sxs-lookup"><span data-stu-id="df3b1-299">Public IP addresses have an effect on billing.</span></span> <span data-ttu-id="df3b1-300">Další informace najdete v tématu [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses/)</span><span class="sxs-lookup"><span data-stu-id="df3b1-300">For more information, see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/)</span></span>
* <span data-ttu-id="df3b1-301">Limity předplatného použít.</span><span class="sxs-lookup"><span data-stu-id="df3b1-301">Subscription limits apply.</span></span> <span data-ttu-id="df3b1-302">Další informace najdete v tématu [omezení služby](../azure-subscription-service-limits.md#networking-limits) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="df3b1-302">For more information, see [Service limits](../azure-subscription-service-limits.md#networking-limits) for details.</span></span>