---
title: aplikace kontejneru aaaEnable access tooAzure DC/OS | Microsoft Docs
description: "Jak tooenable veřejný přístup ke kontejnerům tooDC nebo operačního systému v Azure Container Service."
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
ms.openlocfilehash: 1ba251ba5a176a6a5e1c7831655164e380a62b27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-public-access-tooan-azure-container-service-application"></a><span data-ttu-id="8be98-104">Povolit aplikaci Azure Container Service tooan veřejný přístup</span><span class="sxs-lookup"><span data-stu-id="8be98-104">Enable public access tooan Azure Container Service application</span></span>
<span data-ttu-id="8be98-105">Každý kontejner DC/OS v hello ACS [veřejného agenta fondu](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) je automaticky zveřejněné toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="8be98-105">Any DC/OS container in hello ACS [public agent pool](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) is automatically exposed toohello internet.</span></span> <span data-ttu-id="8be98-106">Ve výchozím nastavení, porty **80**, **443**, **8080** jsou otevřené, a jsou dostupné všechny (veřejné) kontejneru naslouchá na těchto portech.</span><span class="sxs-lookup"><span data-stu-id="8be98-106">By default, ports **80**, **443**, **8080** are opened, and any (public) container listening on those ports are accessible.</span></span> <span data-ttu-id="8be98-107">Tento článek ukazuje, jak tooopen více porty pro vaše aplikace v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="8be98-107">This article shows you how tooopen more ports for your applications in Azure Container Service.</span></span>

## <a name="open-a-port-portal"></a><span data-ttu-id="8be98-108">Otevřete port (portál)</span><span class="sxs-lookup"><span data-stu-id="8be98-108">Open a port (portal)</span></span>
<span data-ttu-id="8be98-109">Nejdřív potřebujeme tooopen hello port, který má být.</span><span class="sxs-lookup"><span data-stu-id="8be98-109">First, we need tooopen hello port we want.</span></span>

1. <span data-ttu-id="8be98-110">Přihlaste se toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="8be98-110">Log in toohello portal.</span></span>
2. <span data-ttu-id="8be98-111">Skupina prostředků hello najít, který jste nasadili hello Azure Container Service na.</span><span class="sxs-lookup"><span data-stu-id="8be98-111">Find hello resource group that you deployed hello Azure Container Service to.</span></span>
3. <span data-ttu-id="8be98-112">Vyberte pro vyrovnávání zatížení agenta hello (která má název podobné příliš**XXXX--agent-lb XXXX**).</span><span class="sxs-lookup"><span data-stu-id="8be98-112">Select hello agent load balancer (which is named similar too**XXXX-agent-lb-XXXX**).</span></span>
   
    ![Nástroj pro vyrovnávání zatížení Azure container service](./media/container-service-enable-public-access/agent-load-balancer.png)
4. <span data-ttu-id="8be98-114">Klikněte na tlačítko **sondy** a potom **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8be98-114">Click **Probes** and then **Add**.</span></span>
   
    ![Nástroj pro vyrovnávání zatížení Azure container service sondy](./media/container-service-enable-public-access/add-probe.png)
5. <span data-ttu-id="8be98-116">Vyplňte formulář hello kontroly a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8be98-116">Fill out hello probe form and click **OK**.</span></span>
   
   | <span data-ttu-id="8be98-117">Pole</span><span class="sxs-lookup"><span data-stu-id="8be98-117">Field</span></span> | <span data-ttu-id="8be98-118">Popis</span><span class="sxs-lookup"><span data-stu-id="8be98-118">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="8be98-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8be98-119">Name</span></span> |<span data-ttu-id="8be98-120">Popisný název hello kontroly.</span><span class="sxs-lookup"><span data-stu-id="8be98-120">A descriptive name of hello probe.</span></span> |
   | <span data-ttu-id="8be98-121">Port</span><span class="sxs-lookup"><span data-stu-id="8be98-121">Port</span></span> |<span data-ttu-id="8be98-122">port Hello tootest kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="8be98-122">hello port of hello container tootest.</span></span> |
   | <span data-ttu-id="8be98-123">Cesta</span><span class="sxs-lookup"><span data-stu-id="8be98-123">Path</span></span> |<span data-ttu-id="8be98-124">(Pokud v režimu HTTP) hello tooprobe cesta relativní webu.</span><span class="sxs-lookup"><span data-stu-id="8be98-124">(When in HTTP mode) hello relative website path tooprobe.</span></span> <span data-ttu-id="8be98-125">Protokol HTTPS není podporována.</span><span class="sxs-lookup"><span data-stu-id="8be98-125">HTTPS not supported.</span></span> |
   | <span data-ttu-id="8be98-126">Interval</span><span class="sxs-lookup"><span data-stu-id="8be98-126">Interval</span></span> |<span data-ttu-id="8be98-127">Hello mezi testu pokusů, v sekundách.</span><span class="sxs-lookup"><span data-stu-id="8be98-127">hello amount of time between probe attempts, in seconds.</span></span> |
   | <span data-ttu-id="8be98-128">Prahová hodnota špatného stavu</span><span class="sxs-lookup"><span data-stu-id="8be98-128">Unhealthy threshold</span></span> |<span data-ttu-id="8be98-129">Počet po sobě jdoucích testu pokusy o zadání před zvažování hello kontejneru není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="8be98-129">Number of consecutive probe attempts before considering hello container unhealthy.</span></span> |
6. <span data-ttu-id="8be98-130">Zpět na hello vlastnosti služby vyrovnání zatížení hello agenta, klikněte na tlačítko **pravidla Vyrovnávání zatížení** a potom **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8be98-130">Back at hello properties of hello agent load balancer, click **Load balancing rules** and then **Add**.</span></span>
   
    ![Pravidla nástroje pro vyrovnávání zatížení Azure container service](./media/container-service-enable-public-access/add-balancer-rule.png)
7. <span data-ttu-id="8be98-132">Vyplňte formulář pro vyrovnávání zatížení hello a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8be98-132">Fill out hello load balancer form and click **OK**.</span></span>
   
   | <span data-ttu-id="8be98-133">Pole</span><span class="sxs-lookup"><span data-stu-id="8be98-133">Field</span></span> | <span data-ttu-id="8be98-134">Popis</span><span class="sxs-lookup"><span data-stu-id="8be98-134">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="8be98-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8be98-135">Name</span></span> |<span data-ttu-id="8be98-136">Popisný název služby Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="8be98-136">A descriptive name of hello load balancer.</span></span> |
   | <span data-ttu-id="8be98-137">Port</span><span class="sxs-lookup"><span data-stu-id="8be98-137">Port</span></span> |<span data-ttu-id="8be98-138">veřejný port příchozí Hello.</span><span class="sxs-lookup"><span data-stu-id="8be98-138">hello public incoming port.</span></span> |
   | <span data-ttu-id="8be98-139">Back-endový port</span><span class="sxs-lookup"><span data-stu-id="8be98-139">Backend port</span></span> |<span data-ttu-id="8be98-140">Hello interní veřejný port provozu tooroute kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="8be98-140">hello internal-public port of hello container tooroute traffic to.</span></span> |
   | <span data-ttu-id="8be98-141">Fond back-end</span><span class="sxs-lookup"><span data-stu-id="8be98-141">Backend pool</span></span> |<span data-ttu-id="8be98-142">Hello kontejnery v tomto fondu budou hello cíl pro tuto službu Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="8be98-142">hello containers in this pool will be hello target for this load balancer.</span></span> |
   | <span data-ttu-id="8be98-143">Test</span><span class="sxs-lookup"><span data-stu-id="8be98-143">Probe</span></span> |<span data-ttu-id="8be98-144">Pokud cíl v hello Hello kontroly používané toodetermine **fond back-end** je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="8be98-144">hello probe used toodetermine if a target in hello **Backend pool** is healthy.</span></span> |
   | <span data-ttu-id="8be98-145">Trvalost relace</span><span class="sxs-lookup"><span data-stu-id="8be98-145">Session persistence</span></span> |<span data-ttu-id="8be98-146">Určuje způsob zpracování přenosů od klienta pro hello trvání relace hello.</span><span class="sxs-lookup"><span data-stu-id="8be98-146">Determines how traffic from a client should be handled for hello duration of hello session.</span></span><br><br><span data-ttu-id="8be98-147">**Žádný**: po sobě jdoucí požadavky ze stejného klienta může zpracovávat všechny kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="8be98-147">**None**: Successive requests from hello same client can be handled by any container.</span></span><br><span data-ttu-id="8be98-148">**Klient IP**: po sobě jdoucí požadavky ze hello stejnou IP adresu klienta, jsou zpracovávány hello stejnému kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8be98-148">**Client IP**: Successive requests from hello same client IP are handled by hello same container.</span></span><br><span data-ttu-id="8be98-149">**Klient IP a protokol**: po sobě jdoucí požadavky ze stejné kombinaci IP adresy a protokol klienta jsou zpracovávány hello hello stejnému kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8be98-149">**Client IP and protocol**: Successive requests from hello same client IP and protocol combination are handled by hello same container.</span></span> |
   | <span data-ttu-id="8be98-150">Časový limit nečinnosti</span><span class="sxs-lookup"><span data-stu-id="8be98-150">Idle timeout</span></span> |<span data-ttu-id="8be98-151">(Pouze TCP) V minutách, hello tookeep čas klienta TCP nebo HTTP otevřete bez spoléhání na *udržování* zprávy.</span><span class="sxs-lookup"><span data-stu-id="8be98-151">(TCP only) In minutes, hello time tookeep a TCP/HTTP client open without relying on *keep-alive* messages.</span></span> |

## <a name="add-a-security-rule-portal"></a><span data-ttu-id="8be98-152">Přidat pravidlo zabezpečení (portál)</span><span class="sxs-lookup"><span data-stu-id="8be98-152">Add a security rule (portal)</span></span>
<span data-ttu-id="8be98-153">V dalším kroku potřebujeme tooadd pravidlo zabezpečení, který směruje provoz z našich otevřen port přes bránu firewall hello.</span><span class="sxs-lookup"><span data-stu-id="8be98-153">Next, we need tooadd a security rule that routes traffic from our opened port through hello firewall.</span></span>

1. <span data-ttu-id="8be98-154">Přihlaste se toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="8be98-154">Log in toohello portal.</span></span>
2. <span data-ttu-id="8be98-155">Skupina prostředků hello najít, který jste nasadili hello Azure Container Service na.</span><span class="sxs-lookup"><span data-stu-id="8be98-155">Find hello resource group that you deployed hello Azure Container Service to.</span></span>
3. <span data-ttu-id="8be98-156">Vyberte hello **veřejné** agenta skupinu zabezpečení sítě (která má název podobné příliš**XXXX-agent veřejný nsg-XXXX**).</span><span class="sxs-lookup"><span data-stu-id="8be98-156">Select hello **public** agent network security group (which is named similar too**XXXX-agent-public-nsg-XXXX**).</span></span>
   
    ![Skupina zabezpečení sítě Azure container service](./media/container-service-enable-public-access/agent-nsg.png)
4. <span data-ttu-id="8be98-158">Vyberte **příchozí pravidla zabezpečení** a potom **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8be98-158">Select **Inbound security rules** and then **Add**.</span></span>
   
    ![Pravidla skupiny zabezpečení sítě Azure container service](./media/container-service-enable-public-access/add-firewall-rule.png)
5. <span data-ttu-id="8be98-160">Vyplňte tooallow pravidlo brány firewall hello veřejný port a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8be98-160">Fill out hello firewall rule tooallow your public port and click **OK**.</span></span>
   
   | <span data-ttu-id="8be98-161">Pole</span><span class="sxs-lookup"><span data-stu-id="8be98-161">Field</span></span> | <span data-ttu-id="8be98-162">Popis</span><span class="sxs-lookup"><span data-stu-id="8be98-162">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="8be98-163">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8be98-163">Name</span></span> |<span data-ttu-id="8be98-164">Popisný název pravidla brány firewall hello.</span><span class="sxs-lookup"><span data-stu-id="8be98-164">A descriptive name of hello firewall rule.</span></span> |
   | <span data-ttu-id="8be98-165">Priorita</span><span class="sxs-lookup"><span data-stu-id="8be98-165">Priority</span></span> |<span data-ttu-id="8be98-166">Pořadí priority pro pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="8be98-166">Priority rank for hello rule.</span></span> <span data-ttu-id="8be98-167">Hello nižší hello číslo hello vyšší hello prioritou.</span><span class="sxs-lookup"><span data-stu-id="8be98-167">hello lower hello number hello higher hello priority.</span></span> |
   | <span data-ttu-id="8be98-168">Zdroj</span><span class="sxs-lookup"><span data-stu-id="8be98-168">Source</span></span> |<span data-ttu-id="8be98-169">Omezte hello příchozí IP adresa rozsahu toobe povolené nebo zakázané tímto pravidlem.</span><span class="sxs-lookup"><span data-stu-id="8be98-169">Restrict hello incoming IP address range toobe allowed or denied by this rule.</span></span> <span data-ttu-id="8be98-170">Použití **žádné** toonot zadejte omezení.</span><span class="sxs-lookup"><span data-stu-id="8be98-170">Use **Any** toonot specify a restriction.</span></span> |
   | <span data-ttu-id="8be98-171">Služba</span><span class="sxs-lookup"><span data-stu-id="8be98-171">Service</span></span> |<span data-ttu-id="8be98-172">Vyberte sadu předdefinovaných služeb, ke kterému se toto pravidlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="8be98-172">Select a set of predefined services this security rule is for.</span></span> <span data-ttu-id="8be98-173">Jinak použijte **vlastní** toocreate vlastní.</span><span class="sxs-lookup"><span data-stu-id="8be98-173">Otherwise use **Custom** toocreate your own.</span></span> |
   | <span data-ttu-id="8be98-174">Protocol (Protokol)</span><span class="sxs-lookup"><span data-stu-id="8be98-174">Protocol</span></span> |<span data-ttu-id="8be98-175">Omezit přenos na základě **TCP** nebo **UDP**.</span><span class="sxs-lookup"><span data-stu-id="8be98-175">Restrict traffic based on **TCP** or **UDP**.</span></span> <span data-ttu-id="8be98-176">Použití **žádné** toonot zadejte omezení.</span><span class="sxs-lookup"><span data-stu-id="8be98-176">Use **Any** toonot specify a restriction.</span></span> |
   | <span data-ttu-id="8be98-177">Rozsah portů</span><span class="sxs-lookup"><span data-stu-id="8be98-177">Port range</span></span> |<span data-ttu-id="8be98-178">Když **služby** je **vlastní**, určuje hello rozsah portů, která toto pravidlo vztahuje.</span><span class="sxs-lookup"><span data-stu-id="8be98-178">When **Service** is **Custom**, specifies hello range of ports that this rule affects.</span></span> <span data-ttu-id="8be98-179">Můžete používat jediný port, jako třeba **80**, nebo jako rozsah **1024-1 500**.</span><span class="sxs-lookup"><span data-stu-id="8be98-179">You can use a single port, such as **80**, or a range like **1024-1500**.</span></span> |
   | <span data-ttu-id="8be98-180">Akce</span><span class="sxs-lookup"><span data-stu-id="8be98-180">Action</span></span> |<span data-ttu-id="8be98-181">Povolí nebo zakážou provoz, která splňuje kritéria hello.</span><span class="sxs-lookup"><span data-stu-id="8be98-181">Allow or deny traffic that meets hello criteria.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8be98-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8be98-182">Next steps</span></span>
<span data-ttu-id="8be98-183">Další informace o hello rozdíl mezi [veřejné a privátní agentů DC/OS](container-service-dcos-agents.md).</span><span class="sxs-lookup"><span data-stu-id="8be98-183">Learn about hello difference between [public and private DC/OS agents](container-service-dcos-agents.md).</span></span>

<span data-ttu-id="8be98-184">Přečtěte si další informace o [Správa kontejnerů Váš DC/OS](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="8be98-184">Read more information about [managing your DC/OS containers](container-service-mesos-marathon-ui.md).</span></span>

