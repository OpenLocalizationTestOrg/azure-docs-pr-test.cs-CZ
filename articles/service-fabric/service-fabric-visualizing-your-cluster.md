---
title: "aaaVisualizing do clusteru pomocí Service Fabric Explorer | Microsoft Docs"
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
ms.openlocfilehash: 73adc4fc254cf6b949b4419b02a046cee3f6a83d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a><span data-ttu-id="32367-103">Vizualizujte cluster pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="32367-103">Visualize your cluster with Service Fabric Explorer</span></span>
<span data-ttu-id="32367-104">Service Fabric Explorer je webový nástroj pro kontrolu a Správa aplikací a uzly v clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="32367-104">Service Fabric Explorer is a web-based tool for inspecting and managing applications and nodes in an Azure Service Fabric cluster.</span></span> <span data-ttu-id="32367-105">Service Fabric Explorer je hostován přímo v rámci clusteru hello, tak, aby byl vždy k dispozici, bez ohledu na to, kde běží vaše clusteru.</span><span class="sxs-lookup"><span data-stu-id="32367-105">Service Fabric Explorer is hosted directly within hello cluster, so it is always available, regardless of where your cluster is running.</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="32367-106">Videokurz</span><span class="sxs-lookup"><span data-stu-id="32367-106">Video tutorial</span></span>

<span data-ttu-id="32367-107">toolearn, jak sledovat toouse Service Fabric Explorer hello následující video Microsoft Virtual Academy:</span><span class="sxs-lookup"><span data-stu-id="32367-107">toolearn how toouse Service Fabric Explorer, watch hello following Microsoft Virtual Academy video:</span></span>

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-tooservice-fabric-explorer"></a><span data-ttu-id="32367-108">Připojit tooService Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="32367-108">Connect tooService Fabric Explorer</span></span>
<span data-ttu-id="32367-109">Pokud jste postupovali podle pokynů hello příliš[Příprava vývojového prostředí](service-fabric-get-started.md), Service Fabric Explorer můžete spustit v místním clusteru tak, že přejdete toohttp://localhost:19080 / Explorer.</span><span class="sxs-lookup"><span data-stu-id="32367-109">If you have followed hello instructions too[prepare your development environment](service-fabric-get-started.md), you can launch Service Fabric Explorer on your local cluster by navigating toohttp://localhost:19080/Explorer.</span></span>

## <a name="understand-hello-service-fabric-explorer-layout"></a><span data-ttu-id="32367-110">Pochopení hello Service Fabric Explorer rozložení</span><span class="sxs-lookup"><span data-stu-id="32367-110">Understand hello Service Fabric Explorer layout</span></span>
<span data-ttu-id="32367-111">Pomocí stromové struktury hello na levé straně hello můžete přejít pomocí Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="32367-111">You can navigate through Service Fabric Explorer by using hello tree on hello left.</span></span> <span data-ttu-id="32367-112">V kořenovém hello hello stromu řídicí panel clusteru hello obsahuje přehled clusteru, včetně shrnutí stavu uzlu a aplikace.</span><span class="sxs-lookup"><span data-stu-id="32367-112">At hello root of hello tree, hello cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span>

![Řídicí panel clusteru Service Fabric Exploreru][sfx-cluster-dashboard]

### <a name="view-hello-clusters-layout"></a><span data-ttu-id="32367-114">Zobrazit rozložení hello clusteru</span><span class="sxs-lookup"><span data-stu-id="32367-114">View hello cluster's layout</span></span>
<span data-ttu-id="32367-115">Uzly v clusteru Service Fabric se umístí napříč dvourozměrná mřížku domén selhání a upgradu domény.</span><span class="sxs-lookup"><span data-stu-id="32367-115">Nodes in a Service Fabric cluster are placed across a two-dimensional grid of fault domains and upgrade domains.</span></span> <span data-ttu-id="32367-116">Toto umístění zajistí, že vaše aplikace zůstanou dostupné i v hello přítomnost selhání hardwaru a upgrady aplikací.</span><span class="sxs-lookup"><span data-stu-id="32367-116">This placement ensures that your applications remain available in hello presence of hardware failures and application upgrades.</span></span> <span data-ttu-id="32367-117">Můžete zobrazit, jak aktuální cluster hello rozložená pomocí hello clusteru mapy.</span><span class="sxs-lookup"><span data-stu-id="32367-117">You can view how hello current cluster is laid out by using hello cluster map.</span></span>

![Mapa clusteru Service Fabric Exploreru][sfx-cluster-map]

### <a name="view-applications-and-services"></a><span data-ttu-id="32367-119">Zobrazení aplikace a služby</span><span class="sxs-lookup"><span data-stu-id="32367-119">View applications and services</span></span>
<span data-ttu-id="32367-120">Hello cluster obsahuje dva podstromy: jeden pro aplikace a druhý pro uzly.</span><span class="sxs-lookup"><span data-stu-id="32367-120">hello cluster contains two subtrees: one for applications and another for nodes.</span></span>

<span data-ttu-id="32367-121">Můžete použít toonavigate zobrazení aplikace hello prostřednictvím hierarchie logické Service Fabric: aplikace, služby, oddíly a repliky.</span><span class="sxs-lookup"><span data-stu-id="32367-121">You can use hello application view toonavigate through Service Fabric's logical hierarchy: applications, services, partitions, and replicas.</span></span>

<span data-ttu-id="32367-122">V příkladu hello níže hello aplikace **Moje aplikace** se skládá ze dvou služeb **MyStatefulService** a **WebService**.</span><span class="sxs-lookup"><span data-stu-id="32367-122">In hello example below, hello application **MyApp** consists of two services, **MyStatefulService** and **WebService**.</span></span> <span data-ttu-id="32367-123">Vzhledem k tomu **MyStatefulService** je stavová, obsahuje oddíl s jeden primární a dva sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="32367-123">Since **MyStatefulService** is stateful, it includes a partition with one primary and two secondary replicas.</span></span> <span data-ttu-id="32367-124">Naopak WebSvcService je bezstavové a obsahuje jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="32367-124">By contrast, WebSvcService is stateless and contains a single instance.</span></span>

![Zobrazení aplikace Service Fabric Exploreru][sfx-application-tree]

<span data-ttu-id="32367-126">Na každé úrovni stromu hello hello hlavním podokně zobrazí příslušné informace o položce hello.</span><span class="sxs-lookup"><span data-stu-id="32367-126">At each level of hello tree, hello main pane shows pertinent information about hello item.</span></span> <span data-ttu-id="32367-127">Například můžete zobrazit stav hello a verze pro konkrétní službu.</span><span class="sxs-lookup"><span data-stu-id="32367-127">For example, you can see hello health status and version for a particular service.</span></span>

![Podokno essentials Service Fabric Exploreru][sfx-service-essentials]

### <a name="view-hello-clusters-nodes"></a><span data-ttu-id="32367-129">Zobrazení uzlů clusteru hello</span><span class="sxs-lookup"><span data-stu-id="32367-129">View hello cluster's nodes</span></span>
<span data-ttu-id="32367-130">zobrazení uzlu Hello zobrazuje fyzické rozložení hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="32367-130">hello node view shows hello physical layout of hello cluster.</span></span> <span data-ttu-id="32367-131">Pro daný uzel můžete zjistit, které aplikace mají v uzlu nasazený kód.</span><span class="sxs-lookup"><span data-stu-id="32367-131">For a given node, you can inspect which applications have code deployed on that node.</span></span> <span data-ttu-id="32367-132">Přesněji řečeno uvidíte, které repliky jsou aktuálně spuštěné existuje.</span><span class="sxs-lookup"><span data-stu-id="32367-132">More specifically, you can see which replicas are currently running there.</span></span>

## <a name="actions"></a><span data-ttu-id="32367-133">Akce</span><span class="sxs-lookup"><span data-stu-id="32367-133">Actions</span></span>
<span data-ttu-id="32367-134">Service Fabric Explorer nabízí rychlý způsob tooinvoke akce na uzly, aplikace a služby v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="32367-134">Service Fabric Explorer offers a quick way tooinvoke actions on nodes, applications, and services within your cluster.</span></span>

<span data-ttu-id="32367-135">Například toodelete instance aplikace zvolte aplikace hello ze stromu hello na levé straně hello a potom zvolte **akce** > **odstranit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="32367-135">For example, toodelete an application instance, choose hello application from hello tree on hello left, and then choose **Actions** > **Delete Application**.</span></span>

![Odstranění aplikace v Service Fabric Exploreru][sfx-delete-application]

> [!TIP]
> <span data-ttu-id="32367-137">Můžete provést klepnutím na další prvek tooeach hello třemi tečkami hello stejné akce.</span><span class="sxs-lookup"><span data-stu-id="32367-137">You can perform hello same actions by clicking hello ellipsis next tooeach element.</span></span>
>
>

<span data-ttu-id="32367-138">Hello následující tabulka uvádí hello akce dostupné u jednotlivých entit:</span><span class="sxs-lookup"><span data-stu-id="32367-138">hello following table lists hello actions available for each entity:</span></span>

| <span data-ttu-id="32367-139">**Entity**</span><span class="sxs-lookup"><span data-stu-id="32367-139">**Entity**</span></span> | <span data-ttu-id="32367-140">**Akce**</span><span class="sxs-lookup"><span data-stu-id="32367-140">**Action**</span></span> | <span data-ttu-id="32367-141">**Popis**</span><span class="sxs-lookup"><span data-stu-id="32367-141">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="32367-142">Typ aplikace</span><span class="sxs-lookup"><span data-stu-id="32367-142">Application type</span></span> |<span data-ttu-id="32367-143">Zrušit zřízení typu</span><span class="sxs-lookup"><span data-stu-id="32367-143">Unprovision type</span></span> |<span data-ttu-id="32367-144">Odebere balíček aplikace hello z úložiště bitových kopií hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="32367-144">Removes hello application package from hello cluster's image store.</span></span> <span data-ttu-id="32367-145">Vyžaduje všechny aplikace toobe tento typ nejdřív odstranit.</span><span class="sxs-lookup"><span data-stu-id="32367-145">Requires all applications of that type toobe removed first.</span></span> |
| <span data-ttu-id="32367-146">Aplikace</span><span class="sxs-lookup"><span data-stu-id="32367-146">Application</span></span> |<span data-ttu-id="32367-147">Odstranění aplikace</span><span class="sxs-lookup"><span data-stu-id="32367-147">Delete Application</span></span> |<span data-ttu-id="32367-148">Odstraňte hello aplikace, včetně všech jejích služeb a jejich stavu (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="32367-148">Delete hello application, including all its services and their state (if any).</span></span> |
| <span data-ttu-id="32367-149">Služba</span><span class="sxs-lookup"><span data-stu-id="32367-149">Service</span></span> |<span data-ttu-id="32367-150">Odstranění služby</span><span class="sxs-lookup"><span data-stu-id="32367-150">Delete Service</span></span> |<span data-ttu-id="32367-151">Odstraňte hello service a její stav (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="32367-151">Delete hello service and its state (if any).</span></span> |
| <span data-ttu-id="32367-152">Node</span><span class="sxs-lookup"><span data-stu-id="32367-152">Node</span></span> |<span data-ttu-id="32367-153">Aktivovat</span><span class="sxs-lookup"><span data-stu-id="32367-153">Activate</span></span> |<span data-ttu-id="32367-154">Aktivujte hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="32367-154">Activate hello node.</span></span> |
| <span data-ttu-id="32367-155">Node</span><span class="sxs-lookup"><span data-stu-id="32367-155">Node</span></span> | <span data-ttu-id="32367-156">Deaktivovat (Pozastavit)</span><span class="sxs-lookup"><span data-stu-id="32367-156">Deactivate (pause)</span></span> | <span data-ttu-id="32367-157">Pozastavení hello uzel v jejím aktuálním stavu.</span><span class="sxs-lookup"><span data-stu-id="32367-157">Pause hello node in its current state.</span></span> <span data-ttu-id="32367-158">Služby pokračovat toorun, ale Service Fabric nepřesouvá proaktivně nic na nebo vypněte ho Pokud je požadovaná tooprevent výpadku nebo data můžou být nekonzistentní.</span><span class="sxs-lookup"><span data-stu-id="32367-158">Services continue toorun but Service Fabric does not proactively move anything onto or off it unless it is required tooprevent an outage or data inconsistency.</span></span> <span data-ttu-id="32367-159">Tato akce je obvykle používanými tooenable ladění služeb na tooensure konkrétním uzlu, který není přesunout během kontroly.</span><span class="sxs-lookup"><span data-stu-id="32367-159">This action is typically used tooenable debugging services on a specific node tooensure that they do not move during inspection.</span></span> | |
| <span data-ttu-id="32367-160">Node</span><span class="sxs-lookup"><span data-stu-id="32367-160">Node</span></span> | <span data-ttu-id="32367-161">Deaktivovat (restartovat)</span><span class="sxs-lookup"><span data-stu-id="32367-161">Deactivate (restart)</span></span> | <span data-ttu-id="32367-162">Bezpečně přesunete všechny služby v paměti vypnout uzlu a trvalé services zavřete.</span><span class="sxs-lookup"><span data-stu-id="32367-162">Safely move all in-memory services off a node and close persistent services.</span></span> <span data-ttu-id="32367-163">Obvykle se používá při hello hostitelské procesy nebo toobe nutné počítač restartovat.</span><span class="sxs-lookup"><span data-stu-id="32367-163">Typically used when hello host processes or machine need toobe restarted.</span></span> | |
| <span data-ttu-id="32367-164">Node</span><span class="sxs-lookup"><span data-stu-id="32367-164">Node</span></span> | <span data-ttu-id="32367-165">Deaktivovat (odebrat data)</span><span class="sxs-lookup"><span data-stu-id="32367-165">Deactivate (remove data)</span></span> | <span data-ttu-id="32367-166">Bezpečně zavřete všechny služby běží na uzlu hello po sestavení dostatečná k výměně za chodu repliky.</span><span class="sxs-lookup"><span data-stu-id="32367-166">Safely close all services running on hello node after building sufficient spare replicas.</span></span> <span data-ttu-id="32367-167">Typicky používá při uzlu (nebo aspoň jeho úložiště) jsou mimo Komise trvale přijata.</span><span class="sxs-lookup"><span data-stu-id="32367-167">Typically used when a node (or at least its storage) is being permanently taken out of commission.</span></span> | |
| <span data-ttu-id="32367-168">Node</span><span class="sxs-lookup"><span data-stu-id="32367-168">Node</span></span> | <span data-ttu-id="32367-169">Odeberte stav uzlu</span><span class="sxs-lookup"><span data-stu-id="32367-169">Remove node state</span></span> | <span data-ttu-id="32367-170">Odeberte znalosti repliky uzlu z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="32367-170">Remove knowledge of a node's replicas from hello cluster.</span></span> <span data-ttu-id="32367-171">Obvykle používá, když už selhání uzlu se považuje neopravitelné.</span><span class="sxs-lookup"><span data-stu-id="32367-171">Typically used when an already failed node is deemed unrecoverable.</span></span> | |
| <span data-ttu-id="32367-172">Node</span><span class="sxs-lookup"><span data-stu-id="32367-172">Node</span></span> | <span data-ttu-id="32367-173">Restartování</span><span class="sxs-lookup"><span data-stu-id="32367-173">Restart</span></span> | <span data-ttu-id="32367-174">Simulace selhání uzlu restartováním hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="32367-174">Simulate a node failure by restarting hello node.</span></span> <span data-ttu-id="32367-175">Další informace [sem](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="32367-175">More information [here](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span></span> | |

<span data-ttu-id="32367-176">Vzhledem k tomu, že mnoho akce jsou destruktivní, můžete být vyzváni tooconfirm vašich představ, před dokončením akce hello.</span><span class="sxs-lookup"><span data-stu-id="32367-176">Since many actions are destructive, you may be asked tooconfirm your intent before hello action is completed.</span></span>

> [!TIP]
> <span data-ttu-id="32367-177">Každou akci, která lze provádět pomocí Service Fabric Explorer můžete provést i pomocí prostředí PowerShell nebo rozhraní REST API, tooenable automatizace.</span><span class="sxs-lookup"><span data-stu-id="32367-177">Every action that can be performed through Service Fabric Explorer can also be performed through PowerShell or a REST API, tooenable automation.</span></span>
>
>

<span data-ttu-id="32367-178">Můžete taky instancí aplikace Service Fabric Explorer toocreate pro daný typ a verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="32367-178">You can also use Service Fabric Explorer toocreate application instances for a given application type and version.</span></span> <span data-ttu-id="32367-179">Zvolte typ aplikace hello hello stromové zobrazení, a klikněte na hello **vytvořit instanci aplikace** další verze toohello odkaz byste chtěli v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="32367-179">Choose hello application type in hello tree view, then click hello **Create app instance** link next toohello version you'd like in hello right pane.</span></span>

![Vytvoření instance aplikace v Service Fabric Exploreru][sfx-create-app-instance]

> [!NOTE]
> <span data-ttu-id="32367-181">Instance aplikace vytvořené pomocí Service Fabric Explorer nemůže být aktuálně parametry.</span><span class="sxs-lookup"><span data-stu-id="32367-181">Application instances created through Service Fabric Explorer cannot currently be parameterized.</span></span> <span data-ttu-id="32367-182">Jsou vytvořeny pomocí výchozí hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="32367-182">They are created using default parameter values.</span></span>
>
>

## <a name="connect-tooa-remote-service-fabric-cluster"></a><span data-ttu-id="32367-183">Připojte tooa vzdálený cluster Service Fabric</span><span class="sxs-lookup"><span data-stu-id="32367-183">Connect tooa remote Service Fabric cluster</span></span>
<span data-ttu-id="32367-184">Pokud znáte koncový bod hello clusteru a že máte dostatečná oprávnění k Service Fabric Explorer máte přístup z libovolného prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="32367-184">If you know hello cluster's endpoint and have sufficient permissions you can access Service Fabric Explorer from any browser.</span></span> <span data-ttu-id="32367-185">Je to proto Service Fabric Explorer je právě jiné službě, která běží v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="32367-185">This is because Service Fabric Explorer is just another service that runs in hello cluster.</span></span>

### <a name="discover-hello-service-fabric-explorer-endpoint-for-a-remote-cluster"></a><span data-ttu-id="32367-186">Zjistit koncový bod Service Fabric Explorer hello vzdáleného clusteru</span><span class="sxs-lookup"><span data-stu-id="32367-186">Discover hello Service Fabric Explorer endpoint for a remote cluster</span></span>
<span data-ttu-id="32367-187">tooreach Service Fabric Explorer pro daný cluster, přejděte v prohlížeči na:</span><span class="sxs-lookup"><span data-stu-id="32367-187">tooreach Service Fabric Explorer for a given cluster, point your browser to:</span></span>

<span data-ttu-id="32367-188">http://&lt;vašeho clusteru endpoint&gt;: 19080/Explorer</span><span class="sxs-lookup"><span data-stu-id="32367-188">http://&lt;your-cluster-endpoint&gt;:19080/Explorer</span></span>

<span data-ttu-id="32367-189">Azure v rámci clusterů hello úplnou adresu URL je také k dispozici v podokně essentials clusteru hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="32367-189">For Azure clusters, hello full URL is also available in hello cluster essentials pane of hello Azure portal.</span></span>

### <a name="connect-tooa-secure-cluster"></a><span data-ttu-id="32367-190">Připojte tooa zabezpečené cluster</span><span class="sxs-lookup"><span data-stu-id="32367-190">Connect tooa secure cluster</span></span>
<span data-ttu-id="32367-191">Můžete ovládat klienta přístup tooyour cluster Service Fabric pomocí certifikátů nebo pomocí Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="32367-191">You can control client access tooyour Service Fabric cluster either with certificates or using Azure Active Directory (AAD).</span></span>

<span data-ttu-id="32367-192">Když zkusíte tooconnect tooService Fabric Explorer na clusteru s podporou zabezpečení, pak v závislosti na konfiguraci clusteru hello máte být toopresent vyžaduje klientský certifikát nebo přihlášení pomocí AAD.</span><span class="sxs-lookup"><span data-stu-id="32367-192">If you attempt tooconnect tooService Fabric Explorer on a secure cluster, then depending on hello cluster's configuration you'll be required toopresent a client certificate or log in using AAD.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32367-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32367-193">Next steps</span></span>
* [<span data-ttu-id="32367-194">Přehled testovatelnosti</span><span class="sxs-lookup"><span data-stu-id="32367-194">Testability overview</span></span>](service-fabric-testability-overview.md)
* [<span data-ttu-id="32367-195">Správu aplikací Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32367-195">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="32367-196">Nasazení aplikace Service Fabric pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="32367-196">Service Fabric application deployment using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
