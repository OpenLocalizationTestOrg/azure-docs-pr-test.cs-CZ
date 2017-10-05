---
title: "Povolit přístup k aplikaci kontejner Azure DC/OS | Microsoft Docs"
description: "Postup povolení veřejný přístup k DC/OS kontejnerů v Azure Container Service."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: c9ef5913859cf3a55a2de2107a9304f1d28a4829
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="enable-public-access-to-an-azure-container-service-application"></a><span data-ttu-id="11566-104">Povolit veřejný přístup k aplikaci Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="11566-104">Enable public access to an Azure Container Service application</span></span>
<span data-ttu-id="11566-105">Všechny DC/OS kontejneru služby ACS [veřejného agenta fondu](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) je automaticky přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="11566-105">Any DC/OS container in the ACS [public agent pool](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) is automatically exposed to the internet.</span></span> <span data-ttu-id="11566-106">Ve výchozím nastavení, porty **80**, **443**, **8080** jsou otevřené, a jsou dostupné všechny (veřejné) kontejneru naslouchá na těchto portech.</span><span class="sxs-lookup"><span data-stu-id="11566-106">By default, ports **80**, **443**, **8080** are opened, and any (public) container listening on those ports are accessible.</span></span> <span data-ttu-id="11566-107">Tento článek ukazuje, jak otevřít další porty pro vaše aplikace v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="11566-107">This article shows you how to open more ports for your applications in Azure Container Service.</span></span>

## <a name="open-a-port-portal"></a><span data-ttu-id="11566-108">Otevřete port (portál)</span><span class="sxs-lookup"><span data-stu-id="11566-108">Open a port (portal)</span></span>
<span data-ttu-id="11566-109">Nejprve musíme otevřít port, který má být.</span><span class="sxs-lookup"><span data-stu-id="11566-109">First, we need to open the port we want.</span></span>

1. <span data-ttu-id="11566-110">Přihlaste se k portálu.</span><span class="sxs-lookup"><span data-stu-id="11566-110">Log in to the portal.</span></span>
2. <span data-ttu-id="11566-111">Najít skupinu prostředků, které jste nasadili Azure Container Service na.</span><span class="sxs-lookup"><span data-stu-id="11566-111">Find the resource group that you deployed the Azure Container Service to.</span></span>
3. <span data-ttu-id="11566-112">Vyberte pro vyrovnávání zatížení agenta (která je s názvem podobná **XXXX--agent-lb XXXX**).</span><span class="sxs-lookup"><span data-stu-id="11566-112">Select the agent load balancer (which is named similar to **XXXX-agent-lb-XXXX**).</span></span>
   
    ![Nástroj pro vyrovnávání zatížení Azure container service](./media/container-service-enable-public-access/agent-load-balancer.png)
4. <span data-ttu-id="11566-114">Klikněte na tlačítko **sondy** a potom **přidat**.</span><span class="sxs-lookup"><span data-stu-id="11566-114">Click **Probes** and then **Add**.</span></span>
   
    ![Nástroj pro vyrovnávání zatížení Azure container service sondy](./media/container-service-enable-public-access/add-probe.png)
5. <span data-ttu-id="11566-116">Vyplňte formulář kontroly a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="11566-116">Fill out the probe form and click **OK**.</span></span>
   
   | <span data-ttu-id="11566-117">Pole</span><span class="sxs-lookup"><span data-stu-id="11566-117">Field</span></span> | <span data-ttu-id="11566-118">Popis</span><span class="sxs-lookup"><span data-stu-id="11566-118">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="11566-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="11566-119">Name</span></span> |<span data-ttu-id="11566-120">Popisný název kontroly.</span><span class="sxs-lookup"><span data-stu-id="11566-120">A descriptive name of the probe.</span></span> |
   | <span data-ttu-id="11566-121">Port</span><span class="sxs-lookup"><span data-stu-id="11566-121">Port</span></span> |<span data-ttu-id="11566-122">Port kontejneru pro testování.</span><span class="sxs-lookup"><span data-stu-id="11566-122">The port of the container to test.</span></span> |
   | <span data-ttu-id="11566-123">Cesta</span><span class="sxs-lookup"><span data-stu-id="11566-123">Path</span></span> |<span data-ttu-id="11566-124">(Pokud v režimu HTTP) Web relativní cesta k testu.</span><span class="sxs-lookup"><span data-stu-id="11566-124">(When in HTTP mode) The relative website path to probe.</span></span> <span data-ttu-id="11566-125">Protokol HTTPS není podporována.</span><span class="sxs-lookup"><span data-stu-id="11566-125">HTTPS not supported.</span></span> |
   | <span data-ttu-id="11566-126">Interval</span><span class="sxs-lookup"><span data-stu-id="11566-126">Interval</span></span> |<span data-ttu-id="11566-127">Množství času mezi testu pokusí v sekundách.</span><span class="sxs-lookup"><span data-stu-id="11566-127">The amount of time between probe attempts, in seconds.</span></span> |
   | <span data-ttu-id="11566-128">Prahová hodnota špatného stavu</span><span class="sxs-lookup"><span data-stu-id="11566-128">Unhealthy threshold</span></span> |<span data-ttu-id="11566-129">Počet po sobě jdoucích testu pokusy o zadání před zvažování kontejneru není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="11566-129">Number of consecutive probe attempts before considering the container unhealthy.</span></span> |
6. <span data-ttu-id="11566-130">Zpět na vlastnosti služby Vyrovnávání zatížení agenta, klikněte na tlačítko **pravidla Vyrovnávání zatížení** a potom **přidat**.</span><span class="sxs-lookup"><span data-stu-id="11566-130">Back at the properties of the agent load balancer, click **Load balancing rules** and then **Add**.</span></span>
   
    ![Pravidla nástroje pro vyrovnávání zatížení Azure container service](./media/container-service-enable-public-access/add-balancer-rule.png)
7. <span data-ttu-id="11566-132">Vyplňte formulář, nástroje pro vyrovnávání zatížení a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="11566-132">Fill out the load balancer form and click **OK**.</span></span>
   
   | <span data-ttu-id="11566-133">Pole</span><span class="sxs-lookup"><span data-stu-id="11566-133">Field</span></span> | <span data-ttu-id="11566-134">Popis</span><span class="sxs-lookup"><span data-stu-id="11566-134">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="11566-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="11566-135">Name</span></span> |<span data-ttu-id="11566-136">Popisný název služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="11566-136">A descriptive name of the load balancer.</span></span> |
   | <span data-ttu-id="11566-137">Port</span><span class="sxs-lookup"><span data-stu-id="11566-137">Port</span></span> |<span data-ttu-id="11566-138">Veřejný příchozí port.</span><span class="sxs-lookup"><span data-stu-id="11566-138">The public incoming port.</span></span> |
   | <span data-ttu-id="11566-139">Back-endový port</span><span class="sxs-lookup"><span data-stu-id="11566-139">Backend port</span></span> |<span data-ttu-id="11566-140">Interní veřejný port kontejneru směrovat provoz.</span><span class="sxs-lookup"><span data-stu-id="11566-140">The internal-public port of the container to route traffic to.</span></span> |
   | <span data-ttu-id="11566-141">Fond back-end</span><span class="sxs-lookup"><span data-stu-id="11566-141">Backend pool</span></span> |<span data-ttu-id="11566-142">Kontejnery v tomto fondu budou cílem pro tento nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="11566-142">The containers in this pool will be the target for this load balancer.</span></span> |
   | <span data-ttu-id="11566-143">Test</span><span class="sxs-lookup"><span data-stu-id="11566-143">Probe</span></span> |<span data-ttu-id="11566-144">Kontroly používané k určení, zda cíl v **fond back-end** je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="11566-144">The probe used to determine if a target in the **Backend pool** is healthy.</span></span> |
   | <span data-ttu-id="11566-145">Trvalost relace</span><span class="sxs-lookup"><span data-stu-id="11566-145">Session persistence</span></span> |<span data-ttu-id="11566-146">Určuje způsob zpracování provoz z klienta po dobu trvání relace.</span><span class="sxs-lookup"><span data-stu-id="11566-146">Determines how traffic from a client should be handled for the duration of the session.</span></span><br><br><span data-ttu-id="11566-147">**Žádný**: po sobě jdoucí požadavky ze stejného klienta může zpracovávat všechny kontejneru.</span><span class="sxs-lookup"><span data-stu-id="11566-147">**None**: Successive requests from the same client can be handled by any container.</span></span><br><span data-ttu-id="11566-148">**Klient IP**: po sobě jdoucí požadavky ze stejné IP adresa klienta jsou zpracovávány ve stejném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="11566-148">**Client IP**: Successive requests from the same client IP are handled by the same container.</span></span><br><span data-ttu-id="11566-149">**Klient IP a protokol**: po sobě jdoucí požadavky ze stejné kombinaci IP adresy a protokol klienta jsou zpracovávány ve stejném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="11566-149">**Client IP and protocol**: Successive requests from the same client IP and protocol combination are handled by the same container.</span></span> |
   | <span data-ttu-id="11566-150">Časový limit nečinnosti</span><span class="sxs-lookup"><span data-stu-id="11566-150">Idle timeout</span></span> |<span data-ttu-id="11566-151">(Pouze TCP) V minutách, otevřete doba uchování TCP nebo HTTP klienta bez spoléhání na *udržování* zprávy.</span><span class="sxs-lookup"><span data-stu-id="11566-151">(TCP only) In minutes, the time to keep a TCP/HTTP client open without relying on *keep-alive* messages.</span></span> |

## <a name="add-a-security-rule-portal"></a><span data-ttu-id="11566-152">Přidat pravidlo zabezpečení (portál)</span><span class="sxs-lookup"><span data-stu-id="11566-152">Add a security rule (portal)</span></span>
<span data-ttu-id="11566-153">Dále je potřeba přidat pravidlo zabezpečení, který směruje provoz z našich otevřen port přes bránu firewall.</span><span class="sxs-lookup"><span data-stu-id="11566-153">Next, we need to add a security rule that routes traffic from our opened port through the firewall.</span></span>

1. <span data-ttu-id="11566-154">Přihlaste se k portálu.</span><span class="sxs-lookup"><span data-stu-id="11566-154">Log in to the portal.</span></span>
2. <span data-ttu-id="11566-155">Najít skupinu prostředků, které jste nasadili Azure Container Service na.</span><span class="sxs-lookup"><span data-stu-id="11566-155">Find the resource group that you deployed the Azure Container Service to.</span></span>
3. <span data-ttu-id="11566-156">Vyberte **veřejné** agenta skupinu zabezpečení sítě (která je s názvem podobná **XXXX-agent veřejný nsg-XXXX**).</span><span class="sxs-lookup"><span data-stu-id="11566-156">Select the **public** agent network security group (which is named similar to **XXXX-agent-public-nsg-XXXX**).</span></span>
   
    ![Skupina zabezpečení sítě Azure container service](./media/container-service-enable-public-access/agent-nsg.png)
4. <span data-ttu-id="11566-158">Vyberte **příchozí pravidla zabezpečení** a potom **přidat**.</span><span class="sxs-lookup"><span data-stu-id="11566-158">Select **Inbound security rules** and then **Add**.</span></span>
   
    ![Pravidla skupiny zabezpečení sítě Azure container service](./media/container-service-enable-public-access/add-firewall-rule.png)
5. <span data-ttu-id="11566-160">Vyplňte pravidlo brány firewall povolit veřejný port a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="11566-160">Fill out the firewall rule to allow your public port and click **OK**.</span></span>
   
   | <span data-ttu-id="11566-161">Pole</span><span class="sxs-lookup"><span data-stu-id="11566-161">Field</span></span> | <span data-ttu-id="11566-162">Popis</span><span class="sxs-lookup"><span data-stu-id="11566-162">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="11566-163">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="11566-163">Name</span></span> |<span data-ttu-id="11566-164">Popisný název pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="11566-164">A descriptive name of the firewall rule.</span></span> |
   | <span data-ttu-id="11566-165">Priorita</span><span class="sxs-lookup"><span data-stu-id="11566-165">Priority</span></span> |<span data-ttu-id="11566-166">Pořadí priority pro pravidlo.</span><span class="sxs-lookup"><span data-stu-id="11566-166">Priority rank for the rule.</span></span> <span data-ttu-id="11566-167">Nižší počet tím vyšší je priorita.</span><span class="sxs-lookup"><span data-stu-id="11566-167">The lower the number the higher the priority.</span></span> |
   | <span data-ttu-id="11566-168">Zdroj</span><span class="sxs-lookup"><span data-stu-id="11566-168">Source</span></span> |<span data-ttu-id="11566-169">Omezte příchozí rozsah IP adres má být povolené nebo zakázané tímto pravidlem.</span><span class="sxs-lookup"><span data-stu-id="11566-169">Restrict the incoming IP address range to be allowed or denied by this rule.</span></span> <span data-ttu-id="11566-170">Použití **žádné** nezadat omezení.</span><span class="sxs-lookup"><span data-stu-id="11566-170">Use **Any** to not specify a restriction.</span></span> |
   | <span data-ttu-id="11566-171">Služba</span><span class="sxs-lookup"><span data-stu-id="11566-171">Service</span></span> |<span data-ttu-id="11566-172">Vyberte sadu předdefinovaných služeb, ke kterému se toto pravidlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="11566-172">Select a set of predefined services this security rule is for.</span></span> <span data-ttu-id="11566-173">Jinak použijte **vlastní** a vytvořte vlastní.</span><span class="sxs-lookup"><span data-stu-id="11566-173">Otherwise use **Custom** to create your own.</span></span> |
   | <span data-ttu-id="11566-174">Protocol (Protokol)</span><span class="sxs-lookup"><span data-stu-id="11566-174">Protocol</span></span> |<span data-ttu-id="11566-175">Omezit přenos na základě **TCP** nebo **UDP**.</span><span class="sxs-lookup"><span data-stu-id="11566-175">Restrict traffic based on **TCP** or **UDP**.</span></span> <span data-ttu-id="11566-176">Použití **žádné** nezadat omezení.</span><span class="sxs-lookup"><span data-stu-id="11566-176">Use **Any** to not specify a restriction.</span></span> |
   | <span data-ttu-id="11566-177">Rozsah portů</span><span class="sxs-lookup"><span data-stu-id="11566-177">Port range</span></span> |<span data-ttu-id="11566-178">Když **služby** je **vlastní**, určuje rozsah portů, která toto pravidlo vztahuje.</span><span class="sxs-lookup"><span data-stu-id="11566-178">When **Service** is **Custom**, specifies the range of ports that this rule affects.</span></span> <span data-ttu-id="11566-179">Můžete používat jediný port, jako třeba **80**, nebo jako rozsah **1024-1 500**.</span><span class="sxs-lookup"><span data-stu-id="11566-179">You can use a single port, such as **80**, or a range like **1024-1500**.</span></span> |
   | <span data-ttu-id="11566-180">Akce</span><span class="sxs-lookup"><span data-stu-id="11566-180">Action</span></span> |<span data-ttu-id="11566-181">Povolí nebo zakážou provoz, která splňuje kritéria.</span><span class="sxs-lookup"><span data-stu-id="11566-181">Allow or deny traffic that meets the criteria.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="11566-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11566-182">Next steps</span></span>
<span data-ttu-id="11566-183">Další informace o rozdílu mezi [veřejné a privátní agentů DC/OS](container-service-dcos-agents.md).</span><span class="sxs-lookup"><span data-stu-id="11566-183">Learn about the difference between [public and private DC/OS agents](container-service-dcos-agents.md).</span></span>

<span data-ttu-id="11566-184">Přečtěte si další informace o [Správa kontejnerů Váš DC/OS](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="11566-184">Read more information about [managing your DC/OS containers](container-service-mesos-marathon-ui.md).</span></span>

