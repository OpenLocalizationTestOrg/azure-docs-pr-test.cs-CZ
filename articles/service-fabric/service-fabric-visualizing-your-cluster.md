---
title: "Vizualizace vašeho clusteru pomocí Service Fabric Explorer | Microsoft Docs"
description: "Service Fabric Explorer je webový nástroj pro kontrolu a správa cloudových aplikací a uzly v clusteru s podporou Microsoft Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: ryanwi
ms.openlocfilehash: 789793a7f50170188d688881a9178546c3074018
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a><span data-ttu-id="136eb-103">Vizualizujte cluster pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="136eb-103">Visualize your cluster with Service Fabric Explorer</span></span>
<span data-ttu-id="136eb-104">Service Fabric Explorer je webový nástroj pro kontrolu a Správa aplikací a uzly v clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="136eb-104">Service Fabric Explorer is a web-based tool for inspecting and managing applications and nodes in an Azure Service Fabric cluster.</span></span> <span data-ttu-id="136eb-105">Service Fabric Explorer je hostován přímo v rámci clusteru, tak, aby byl vždy k dispozici, bez ohledu na to, kde běží vaše clusteru.</span><span class="sxs-lookup"><span data-stu-id="136eb-105">Service Fabric Explorer is hosted directly within the cluster, so it is always available, regardless of where your cluster is running.</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="136eb-106">Videokurz</span><span class="sxs-lookup"><span data-stu-id="136eb-106">Video tutorial</span></span>

<span data-ttu-id="136eb-107">Informace o použití Service Fabric Explorer najdete v následujícím videu Microsoft Virtual Academy:</span><span class="sxs-lookup"><span data-stu-id="136eb-107">To learn how to use Service Fabric Explorer, watch the following Microsoft Virtual Academy video:</span></span>

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-to-service-fabric-explorer"></a><span data-ttu-id="136eb-108">Připojení k Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="136eb-108">Connect to Service Fabric Explorer</span></span>
<span data-ttu-id="136eb-109">Pokud jste postupovali podle pokynů [Příprava vývojového prostředí](service-fabric-get-started.md), Service Fabric Explorer můžete spustit v místním clusteru přechodem na http://localhost: 19080/Explorer.</span><span class="sxs-lookup"><span data-stu-id="136eb-109">If you have followed the instructions to [prepare your development environment](service-fabric-get-started.md), you can launch Service Fabric Explorer on your local cluster by navigating to http://localhost:19080/Explorer.</span></span>

## <a name="understand-the-service-fabric-explorer-layout"></a><span data-ttu-id="136eb-110">Pochopení rozložení Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="136eb-110">Understand the Service Fabric Explorer layout</span></span>
<span data-ttu-id="136eb-111">Pomocí stromové struktury nalevo můžete přejít pomocí Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="136eb-111">You can navigate through Service Fabric Explorer by using the tree on the left.</span></span> <span data-ttu-id="136eb-112">Řídicí panel clusteru v kořenu stromu, obsahuje přehled clusteru, včetně shrnutí stavu uzlu a aplikace.</span><span class="sxs-lookup"><span data-stu-id="136eb-112">At the root of the tree, the cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span>

![Řídicí panel clusteru Service Fabric Exploreru][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a><span data-ttu-id="136eb-114">Zobrazit rozložení clusteru</span><span class="sxs-lookup"><span data-stu-id="136eb-114">View the cluster's layout</span></span>
<span data-ttu-id="136eb-115">Uzly v clusteru Service Fabric se umístí napříč dvourozměrná mřížku domén selhání a upgradu domény.</span><span class="sxs-lookup"><span data-stu-id="136eb-115">Nodes in a Service Fabric cluster are placed across a two-dimensional grid of fault domains and upgrade domains.</span></span> <span data-ttu-id="136eb-116">Toto umístění zaručuje, že vaše aplikace zachovány k dispozici v případě selhání hardwaru a upgrady aplikací.</span><span class="sxs-lookup"><span data-stu-id="136eb-116">This placement ensures that your applications remain available in the presence of hardware failures and application upgrades.</span></span> <span data-ttu-id="136eb-117">Můžete zobrazit, jak aktuální cluster rozložená pomocí mapy clusteru.</span><span class="sxs-lookup"><span data-stu-id="136eb-117">You can view how the current cluster is laid out by using the cluster map.</span></span>

![Mapa clusteru Service Fabric Exploreru][sfx-cluster-map]

### <a name="view-applications-and-services"></a><span data-ttu-id="136eb-119">Zobrazení aplikace a služby</span><span class="sxs-lookup"><span data-stu-id="136eb-119">View applications and services</span></span>
<span data-ttu-id="136eb-120">Cluster obsahuje dva podstromy: jeden pro aplikace a druhý pro uzly.</span><span class="sxs-lookup"><span data-stu-id="136eb-120">The cluster contains two subtrees: one for applications and another for nodes.</span></span>

<span data-ttu-id="136eb-121">Zobrazení aplikací můžete procházet logické hierarchie Service Fabric: aplikace, služby, oddíly a repliky.</span><span class="sxs-lookup"><span data-stu-id="136eb-121">You can use the application view to navigate through Service Fabric's logical hierarchy: applications, services, partitions, and replicas.</span></span>

<span data-ttu-id="136eb-122">V příkladu níže aplikace **Moje aplikace** se skládá ze dvou služeb **MyStatefulService** a **WebService**.</span><span class="sxs-lookup"><span data-stu-id="136eb-122">In the example below, the application **MyApp** consists of two services, **MyStatefulService** and **WebService**.</span></span> <span data-ttu-id="136eb-123">Vzhledem k tomu **MyStatefulService** je stavová, obsahuje oddíl s jeden primární a dva sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="136eb-123">Since **MyStatefulService** is stateful, it includes a partition with one primary and two secondary replicas.</span></span> <span data-ttu-id="136eb-124">Naopak WebSvcService je bezstavové a obsahuje jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="136eb-124">By contrast, WebSvcService is stateless and contains a single instance.</span></span>

![Zobrazení aplikace Service Fabric Exploreru][sfx-application-tree]

<span data-ttu-id="136eb-126">Na každé úrovni stromu hlavním podokně zobrazí příslušné informace o položce.</span><span class="sxs-lookup"><span data-stu-id="136eb-126">At each level of the tree, the main pane shows pertinent information about the item.</span></span> <span data-ttu-id="136eb-127">Například se zobrazí stav a verze pro konkrétní službu.</span><span class="sxs-lookup"><span data-stu-id="136eb-127">For example, you can see the health status and version for a particular service.</span></span>

![Podokno essentials Service Fabric Exploreru][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a><span data-ttu-id="136eb-129">Zobrazení uzlů clusteru</span><span class="sxs-lookup"><span data-stu-id="136eb-129">View the cluster's nodes</span></span>
<span data-ttu-id="136eb-130">Zobrazení uzlu obsahuje fyzické rozložení clusteru.</span><span class="sxs-lookup"><span data-stu-id="136eb-130">The node view shows the physical layout of the cluster.</span></span> <span data-ttu-id="136eb-131">Pro daný uzel můžete zjistit, které aplikace mají v uzlu nasazený kód.</span><span class="sxs-lookup"><span data-stu-id="136eb-131">For a given node, you can inspect which applications have code deployed on that node.</span></span> <span data-ttu-id="136eb-132">Přesněji řečeno uvidíte, které repliky jsou aktuálně spuštěné existuje.</span><span class="sxs-lookup"><span data-stu-id="136eb-132">More specifically, you can see which replicas are currently running there.</span></span>

## <a name="actions"></a><span data-ttu-id="136eb-133">Akce</span><span class="sxs-lookup"><span data-stu-id="136eb-133">Actions</span></span>
<span data-ttu-id="136eb-134">Service Fabric Explorer nabízí rychlý způsob, jak vyvolání akce na uzly, aplikace a služby v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="136eb-134">Service Fabric Explorer offers a quick way to invoke actions on nodes, applications, and services within your cluster.</span></span>

<span data-ttu-id="136eb-135">Chcete-li odstranit instanci aplikace, například ze stromu na levé straně zvolte aplikaci a poté zvolte **akce** > **odstranit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="136eb-135">For example, to delete an application instance, choose the application from the tree on the left, and then choose **Actions** > **Delete Application**.</span></span>

![Odstranění aplikace v Service Fabric Exploreru][sfx-delete-application]

> [!TIP]
> <span data-ttu-id="136eb-137">Kliknutím na tlačítko se třemi tečkami vedle jednotlivých prvků můžete provádět stejné akce.</span><span class="sxs-lookup"><span data-stu-id="136eb-137">You can perform the same actions by clicking the ellipsis next to each element.</span></span>
>
>

<span data-ttu-id="136eb-138">Následující tabulka uvádí akce, které jsou dostupné u jednotlivých entit:</span><span class="sxs-lookup"><span data-stu-id="136eb-138">The following table lists the actions available for each entity:</span></span>

| <span data-ttu-id="136eb-139">**Entity**</span><span class="sxs-lookup"><span data-stu-id="136eb-139">**Entity**</span></span> | <span data-ttu-id="136eb-140">**Akce**</span><span class="sxs-lookup"><span data-stu-id="136eb-140">**Action**</span></span> | <span data-ttu-id="136eb-141">**Popis**</span><span class="sxs-lookup"><span data-stu-id="136eb-141">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="136eb-142">Typ aplikace</span><span class="sxs-lookup"><span data-stu-id="136eb-142">Application type</span></span> |<span data-ttu-id="136eb-143">Zrušit zřízení typu</span><span class="sxs-lookup"><span data-stu-id="136eb-143">Unprovision type</span></span> |<span data-ttu-id="136eb-144">Odebere balíček aplikace z úložiště bitových kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="136eb-144">Removes the application package from the cluster's image store.</span></span> <span data-ttu-id="136eb-145">Vyžaduje nejdřív odstranit všechny aplikacím daného typu.</span><span class="sxs-lookup"><span data-stu-id="136eb-145">Requires all applications of that type to be removed first.</span></span> |
| <span data-ttu-id="136eb-146">Aplikace</span><span class="sxs-lookup"><span data-stu-id="136eb-146">Application</span></span> |<span data-ttu-id="136eb-147">Odstranění aplikace</span><span class="sxs-lookup"><span data-stu-id="136eb-147">Delete Application</span></span> |<span data-ttu-id="136eb-148">Odstraňte aplikaci, včetně všech jejích služeb a jejich stavu (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="136eb-148">Delete the application, including all its services and their state (if any).</span></span> |
| <span data-ttu-id="136eb-149">Služba</span><span class="sxs-lookup"><span data-stu-id="136eb-149">Service</span></span> |<span data-ttu-id="136eb-150">Odstranění služby</span><span class="sxs-lookup"><span data-stu-id="136eb-150">Delete Service</span></span> |<span data-ttu-id="136eb-151">Odstraňte službu a její stav (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="136eb-151">Delete the service and its state (if any).</span></span> |
| <span data-ttu-id="136eb-152">Node</span><span class="sxs-lookup"><span data-stu-id="136eb-152">Node</span></span> |<span data-ttu-id="136eb-153">Aktivovat</span><span class="sxs-lookup"><span data-stu-id="136eb-153">Activate</span></span> |<span data-ttu-id="136eb-154">Aktivujte uzlu.</span><span class="sxs-lookup"><span data-stu-id="136eb-154">Activate the node.</span></span> |
| <span data-ttu-id="136eb-155">Node</span><span class="sxs-lookup"><span data-stu-id="136eb-155">Node</span></span> | <span data-ttu-id="136eb-156">Deaktivovat (Pozastavit)</span><span class="sxs-lookup"><span data-stu-id="136eb-156">Deactivate (pause)</span></span> | <span data-ttu-id="136eb-157">Pozastavení uzlu v jejím aktuálním stavu.</span><span class="sxs-lookup"><span data-stu-id="136eb-157">Pause the node in its current state.</span></span> <span data-ttu-id="136eb-158">Služby pokračovat ke spuštění, ale Service Fabric nepřesouvá proaktivně nic na nebo vypněte ho Pokud je třeba, aby se zabránilo nekonzistenci výpadku nebo data.</span><span class="sxs-lookup"><span data-stu-id="136eb-158">Services continue to run but Service Fabric does not proactively move anything onto or off it unless it is required to prevent an outage or data inconsistency.</span></span> <span data-ttu-id="136eb-159">Tato akce se obvykle používá k povolení ladění služeb v konkrétním uzlu zajistit, že není přesunout během kontroly.</span><span class="sxs-lookup"><span data-stu-id="136eb-159">This action is typically used to enable debugging services on a specific node to ensure that they do not move during inspection.</span></span> | |
| <span data-ttu-id="136eb-160">Node</span><span class="sxs-lookup"><span data-stu-id="136eb-160">Node</span></span> | <span data-ttu-id="136eb-161">Deaktivovat (restartovat)</span><span class="sxs-lookup"><span data-stu-id="136eb-161">Deactivate (restart)</span></span> | <span data-ttu-id="136eb-162">Bezpečně přesunete všechny služby v paměti vypnout uzlu a trvalé services zavřete.</span><span class="sxs-lookup"><span data-stu-id="136eb-162">Safely move all in-memory services off a node and close persistent services.</span></span> <span data-ttu-id="136eb-163">Obvykle nepoužívá, pokud hostitelské procesy nebo počítač musí restartovat.</span><span class="sxs-lookup"><span data-stu-id="136eb-163">Typically used when the host processes or machine need to be restarted.</span></span> | |
| <span data-ttu-id="136eb-164">Node</span><span class="sxs-lookup"><span data-stu-id="136eb-164">Node</span></span> | <span data-ttu-id="136eb-165">Deaktivovat (odebrat data)</span><span class="sxs-lookup"><span data-stu-id="136eb-165">Deactivate (remove data)</span></span> | <span data-ttu-id="136eb-166">Bezpečně zavřete všechny služby spuštěné na uzlu po sestavení dostatečná k výměně za chodu repliky.</span><span class="sxs-lookup"><span data-stu-id="136eb-166">Safely close all services running on the node after building sufficient spare replicas.</span></span> <span data-ttu-id="136eb-167">Typicky používá při uzlu (nebo aspoň jeho úložiště) jsou mimo Komise trvale přijata.</span><span class="sxs-lookup"><span data-stu-id="136eb-167">Typically used when a node (or at least its storage) is being permanently taken out of commission.</span></span> | |
| <span data-ttu-id="136eb-168">Node</span><span class="sxs-lookup"><span data-stu-id="136eb-168">Node</span></span> | <span data-ttu-id="136eb-169">Odeberte stav uzlu</span><span class="sxs-lookup"><span data-stu-id="136eb-169">Remove node state</span></span> | <span data-ttu-id="136eb-170">Odebrání znalosti repliky uzlu z clusteru.</span><span class="sxs-lookup"><span data-stu-id="136eb-170">Remove knowledge of a node's replicas from the cluster.</span></span> <span data-ttu-id="136eb-171">Obvykle používá, když už selhání uzlu se považuje neopravitelné.</span><span class="sxs-lookup"><span data-stu-id="136eb-171">Typically used when an already failed node is deemed unrecoverable.</span></span> | |
| <span data-ttu-id="136eb-172">Node</span><span class="sxs-lookup"><span data-stu-id="136eb-172">Node</span></span> | <span data-ttu-id="136eb-173">Restartování</span><span class="sxs-lookup"><span data-stu-id="136eb-173">Restart</span></span> | <span data-ttu-id="136eb-174">Simulace selhání uzlu restartováním uzlu.</span><span class="sxs-lookup"><span data-stu-id="136eb-174">Simulate a node failure by restarting the node.</span></span> <span data-ttu-id="136eb-175">Další informace [sem](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="136eb-175">More information [here](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span></span> | |

<span data-ttu-id="136eb-176">Vzhledem k tomu, že mnoho akce jsou destruktivní, můžete být vyzváni k potvrzení vašich představ, před dokončením akce.</span><span class="sxs-lookup"><span data-stu-id="136eb-176">Since many actions are destructive, you may be asked to confirm your intent before the action is completed.</span></span>

> [!TIP]
> <span data-ttu-id="136eb-177">Každou akci, která lze provádět pomocí Service Fabric Explorer můžete provést i pomocí prostředí PowerShell nebo rozhraní REST API, jak povolit automatizaci.</span><span class="sxs-lookup"><span data-stu-id="136eb-177">Every action that can be performed through Service Fabric Explorer can also be performed through PowerShell or a REST API, to enable automation.</span></span>
>
>

<span data-ttu-id="136eb-178">Můžete také Service Fabric Explorer pro vytvoření instancí aplikace daný typ a verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="136eb-178">You can also use Service Fabric Explorer to create application instances for a given application type and version.</span></span> <span data-ttu-id="136eb-179">Vyberte typ aplikace, v zobrazení stromu a pak klikněte na **vytvořit instanci aplikace** odkaz vedle verze byste chtěli v pravém podokně.</span><span class="sxs-lookup"><span data-stu-id="136eb-179">Choose the application type in the tree view, then click the **Create app instance** link next to the version you'd like in the right pane.</span></span>

![Vytvoření instance aplikace v Service Fabric Exploreru][sfx-create-app-instance]

> [!NOTE]
> <span data-ttu-id="136eb-181">Instance aplikace vytvořené pomocí Service Fabric Explorer nemůže být aktuálně parametry.</span><span class="sxs-lookup"><span data-stu-id="136eb-181">Application instances created through Service Fabric Explorer cannot currently be parameterized.</span></span> <span data-ttu-id="136eb-182">Jsou vytvořeny pomocí výchozí hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="136eb-182">They are created using default parameter values.</span></span>
>
>

## <a name="connect-to-a-remote-service-fabric-cluster"></a><span data-ttu-id="136eb-183">Připojení ke vzdálené clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="136eb-183">Connect to a remote Service Fabric cluster</span></span>
<span data-ttu-id="136eb-184">Pokud znáte koncový bod clusteru a že máte dostatečná oprávnění k Service Fabric Explorer máte přístup z libovolného prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="136eb-184">If you know the cluster's endpoint and have sufficient permissions you can access Service Fabric Explorer from any browser.</span></span> <span data-ttu-id="136eb-185">Je to proto, že je právě jiné službě, která běží v clusteru Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="136eb-185">This is because Service Fabric Explorer is just another service that runs in the cluster.</span></span>

### <a name="discover-the-service-fabric-explorer-endpoint-for-a-remote-cluster"></a><span data-ttu-id="136eb-186">Zjistit koncový bod Service Fabric Explorer pro vzdálený cluster</span><span class="sxs-lookup"><span data-stu-id="136eb-186">Discover the Service Fabric Explorer endpoint for a remote cluster</span></span>
<span data-ttu-id="136eb-187">K dosažení pro daný cluster Service Fabric Exploreru, přejděte v prohlížeči na:</span><span class="sxs-lookup"><span data-stu-id="136eb-187">To reach Service Fabric Explorer for a given cluster, point your browser to:</span></span>

<span data-ttu-id="136eb-188">http://&lt;vašeho clusteru endpoint&gt;: 19080/Explorer</span><span class="sxs-lookup"><span data-stu-id="136eb-188">http://&lt;your-cluster-endpoint&gt;:19080/Explorer</span></span>

<span data-ttu-id="136eb-189">Azure v rámci clusterů úplnou adresu URL je také k dispozici v podokně clusteru essentials na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="136eb-189">For Azure clusters, the full URL is also available in the cluster essentials pane of the Azure portal.</span></span>

### <a name="connect-to-a-secure-cluster"></a><span data-ttu-id="136eb-190">Připojení k zabezpečenému clusteru</span><span class="sxs-lookup"><span data-stu-id="136eb-190">Connect to a secure cluster</span></span>
<span data-ttu-id="136eb-191">Ke svému clusteru Service Fabric pomocí certifikátů nebo pomocí Azure Active Directory (AAD) můžete řídit přístup klienta.</span><span class="sxs-lookup"><span data-stu-id="136eb-191">You can control client access to your Service Fabric cluster either with certificates or using Azure Active Directory (AAD).</span></span>

<span data-ttu-id="136eb-192">Pokud se pokusíte připojit k Service Fabric Explorer na clusteru s podporou zabezpečení, pak v závislosti na konfiguraci clusteru budete se budete muset certifikát klienta k dispozici nebo se přihlaste pomocí AAD.</span><span class="sxs-lookup"><span data-stu-id="136eb-192">If you attempt to connect to Service Fabric Explorer on a secure cluster, then depending on the cluster's configuration you'll be required to present a client certificate or log in using AAD.</span></span>

## <a name="next-steps"></a><span data-ttu-id="136eb-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="136eb-193">Next steps</span></span>
* [<span data-ttu-id="136eb-194">Přehled testovatelnosti</span><span class="sxs-lookup"><span data-stu-id="136eb-194">Testability overview</span></span>](service-fabric-testability-overview.md)
* [<span data-ttu-id="136eb-195">Správu aplikací Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="136eb-195">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="136eb-196">Nasazení aplikace Service Fabric pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="136eb-196">Service Fabric application deployment using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
