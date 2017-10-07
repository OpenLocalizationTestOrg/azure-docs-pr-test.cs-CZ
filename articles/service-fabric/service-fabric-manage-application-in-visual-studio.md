---
title: "aaaManage aplikace v sadě Visual Studio | Microsoft Docs"
description: "Pomocí sady Visual Studio toocreate, vývoj, balíčku, nasazení a ladění aplikací Service Fabric a služeb."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: b2d5803d85e4f9645dcbece33a2208bc0955498d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-visual-studio-toosimplify-writing-and-managing-your-service-fabric-applications"></a><span data-ttu-id="14f67-103">Pomocí sady Visual Studio toosimplify zápis a správu aplikací Service Fabric</span><span class="sxs-lookup"><span data-stu-id="14f67-103">Use Visual Studio toosimplify writing and managing your Service Fabric applications</span></span>
<span data-ttu-id="14f67-104">Můžete spravovat vaše aplikace Azure Service Fabric a služby pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14f67-104">You can manage your Azure Service Fabric applications and services through Visual Studio.</span></span> <span data-ttu-id="14f67-105">Jakmile jste [nastavení vývojového prostředí](service-fabric-get-started.md), můžete používat aplikace Service Fabric toocreate Visual Studio, přidejte služby nebo balíček, registrace a nasazení aplikací v místní vývojový cluster.</span><span class="sxs-lookup"><span data-stu-id="14f67-105">Once you've [set up your development environment](service-fabric-get-started.md), you can use Visual Studio toocreate Service Fabric applications, add services, or package, register, and deploy applications in your local development cluster.</span></span>

## <a name="deploy-your-service-fabric-application"></a><span data-ttu-id="14f67-106">Nasazení aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="14f67-106">Deploy your Service Fabric application</span></span>
<span data-ttu-id="14f67-107">Ve výchozím nastavení nasazení aplikace kombinuje hello následující kroky do jednoho jednoduché operace:</span><span class="sxs-lookup"><span data-stu-id="14f67-107">By default, deploying an application combines hello following steps into one simple operation:</span></span>

1. <span data-ttu-id="14f67-108">Vytvoření balíčku aplikace hello</span><span class="sxs-lookup"><span data-stu-id="14f67-108">Creating hello application package</span></span>
2. <span data-ttu-id="14f67-109">Odesílání úložiště bitových kopií toohello balíčku aplikace hello</span><span class="sxs-lookup"><span data-stu-id="14f67-109">Uploading hello application package toohello image store</span></span>
3. <span data-ttu-id="14f67-110">Registrace typu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="14f67-110">Registering hello application type</span></span>
4. <span data-ttu-id="14f67-111">Odebrat všechny spuštěné instance aplikace</span><span class="sxs-lookup"><span data-stu-id="14f67-111">Removing any running application instances</span></span>
5. <span data-ttu-id="14f67-112">Vytvoření instance aplikace</span><span class="sxs-lookup"><span data-stu-id="14f67-112">Creating an application instance</span></span>

<span data-ttu-id="14f67-113">Ve Visual Studiu stisknutím **F5** nasadí aplikaci a připojte instancemi aplikace tooall hello ladicí program.</span><span class="sxs-lookup"><span data-stu-id="14f67-113">In Visual Studio, pressing **F5** deploys your application and attach hello debugger tooall application instances.</span></span> <span data-ttu-id="14f67-114">Můžete použít **Ctrl + F5** toodeploy aplikace bez ladění, nebo můžete publikovat tooa místního nebo vzdáleného clusteru pomocí hello profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="14f67-114">You can use **Ctrl+F5** toodeploy an application without debugging, or you can publish tooa local or remote cluster by using hello publish profile.</span></span> <span data-ttu-id="14f67-115">Další informace najdete v tématu [publikování vzdáleného clusteru služby aplikace tooa pomocí sady Visual Studio](service-fabric-publish-app-remote-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="14f67-115">For more information, see [Publish an application tooa remote cluster by using Visual Studio](service-fabric-publish-app-remote-cluster.md).</span></span>

### <a name="application-debug-mode"></a><span data-ttu-id="14f67-116">Režim ladění aplikací</span><span class="sxs-lookup"><span data-stu-id="14f67-116">Application Debug Mode</span></span>
<span data-ttu-id="14f67-117">Visual Studio poskytují vlastnost s názvem **režim ladění aplikací**, který určuje, jakým způsobem chcete nasazení aplikace Visual Studia toohandle jako součást ladění.</span><span class="sxs-lookup"><span data-stu-id="14f67-117">Visual Studio provide a property called **Application Debug Mode**, which controls how you want Visual Studios toohandle Application deployment as part of debugging.</span></span>

#### <a name="tooset-hello-application-debug-mode-property"></a><span data-ttu-id="14f67-118">hello tooset vlastnost režim ladění aplikací</span><span class="sxs-lookup"><span data-stu-id="14f67-118">tooset hello Application Debug Mode property</span></span>
1. <span data-ttu-id="14f67-119">Na hello Service Fabric aplikace projektu (*.sfproj) místní nabídky zvolte **vlastnosti** (nebo stiskněte klávesu hello **F4** klíč).</span><span class="sxs-lookup"><span data-stu-id="14f67-119">On hello Service Fabric application project's (*.sfproj) shortcut menu, choose **Properties** (or press hello **F4** key).</span></span>
2. <span data-ttu-id="14f67-120">V hello **vlastnosti** okno, sada hello **režim ladění aplikací** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="14f67-120">In hello **Properties** window, set hello **Application Debug Mode** property.</span></span>

![Nastavte vlastnost režimu ladění aplikace][debugmodeproperty]

#### <a name="application-debug-modes"></a><span data-ttu-id="14f67-122">Režim ladění aplikace</span><span class="sxs-lookup"><span data-stu-id="14f67-122">Application Debug Modes</span></span>

1. <span data-ttu-id="14f67-123">**Aktualizovat aplikaci** tento režim můžete změnit tooquickly a ladění kódu a podporuje úpravy statických webových souborů při ladění.</span><span class="sxs-lookup"><span data-stu-id="14f67-123">**Refresh Application** This mode enables you tooquickly change and debug your code and supports editing static web files while debugging.</span></span> <span data-ttu-id="14f67-124">Tento režim funguje jenom v případě místního vývojového clusteru je v [režim 1 uzel](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span><span class="sxs-lookup"><span data-stu-id="14f67-124">This mode only works if your local development cluster is in [1-Node mode](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span></span>
2. <span data-ttu-id="14f67-125">**Odebrání aplikace** příčiny hello toobe aplikace odebrat, pokud ukončení relace ladění hello.</span><span class="sxs-lookup"><span data-stu-id="14f67-125">**Remove Application** causes hello application toobe removed when hello debug session ends.</span></span>
3. <span data-ttu-id="14f67-126">**Automaticky Upgrade** aplikace hello stále toorun při ukončení relace ladění hello.</span><span class="sxs-lookup"><span data-stu-id="14f67-126">**Auto Upgrade** hello application continues toorun when hello debug session ends.</span></span> <span data-ttu-id="14f67-127">Hello další relaci ladění bude považovat za hello nasazení upgradu.</span><span class="sxs-lookup"><span data-stu-id="14f67-127">hello next debug session will treat hello deployment as an upgrade.</span></span> <span data-ttu-id="14f67-128">proces upgradu Hello se zachovají všechna data, která jste zadali v předchozí relaci ladění.</span><span class="sxs-lookup"><span data-stu-id="14f67-128">hello upgrade process preserves any data that you entered in a previous debug session.</span></span>
4. <span data-ttu-id="14f67-129">**Zachovat aplikace** udržuje hello aplikace spuštěné v clusteru hello při hello ladění neukončí relace.</span><span class="sxs-lookup"><span data-stu-id="14f67-129">**Keep Application** hello application keeps running in hello cluster when hello debug session ends.</span></span> <span data-ttu-id="14f67-130">Na hello začátku hello další relaci ladění, se odeberou aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="14f67-130">At hello beginning of hello next debug session, hello application will be removed.</span></span>

<span data-ttu-id="14f67-131">Pro **automatického upgradu** uchování dat s použitím možnosti upgradu hello aplikací Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="14f67-131">For **Auto Upgrade** data is preserved by applying hello application upgrade capabilities of Service Fabric.</span></span> <span data-ttu-id="14f67-132">Další informace o upgrade aplikace a jak můžete provést upgrade v prostředí skutečné najdete v tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="14f67-132">For more information about upgrading applications and how you might perform an upgrade in a real environment, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

## <a name="add-a-service-tooyour-service-fabric-application"></a><span data-ttu-id="14f67-133">Přidání služby tooyour aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="14f67-133">Add a service tooyour Service Fabric application</span></span>
<span data-ttu-id="14f67-134">Můžete přidat nové služby tooyour aplikace tooextend její funkce.</span><span class="sxs-lookup"><span data-stu-id="14f67-134">You can add new services tooyour application tooextend its functionality.</span></span>  <span data-ttu-id="14f67-135">tooensure hello služby je součástí balíčku aplikace, přidejte hello služby prostřednictvím hello **nové infrastruktury služby...**  položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="14f67-135">tooensure that hello service is included in your application package, add hello service through hello **New Fabric Service...** menu item.</span></span>

![Přidejte novou službu Service Fabric][newservice]

<span data-ttu-id="14f67-137">Vyberte aplikace Service Fabric projektu typu tooadd tooyour a zadejte název pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="14f67-137">Select a Service Fabric project type tooadd tooyour application, and specify a name for hello service.</span></span>  <span data-ttu-id="14f67-138">V tématu [výběr rozhraní služby](service-fabric-choose-framework.md) toohelp rozhodnete služby, která zadejte toouse.</span><span class="sxs-lookup"><span data-stu-id="14f67-138">See [Choosing a framework for your service](service-fabric-choose-framework.md) toohelp you decide which service type toouse.</span></span>

![Vyberte aplikace Service Fabric služby projektu typu tooadd tooyour][addserviceproject]

<span data-ttu-id="14f67-140">Přidání nové služby Hello tooyour řešení a stávající balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="14f67-140">hello new service is added tooyour solution and existing application package.</span></span> <span data-ttu-id="14f67-141">odkazy na službu Hello a výchozí instance služby se přidané toohello manifest aplikace, což způsobilo toobe hello služby vytvořit a spustit hello příštím nasazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="14f67-141">hello service references and a default service instance will be added toohello application manifest, causing hello service toobe created and started hello next time you deploy hello application.</span></span>

![Přidání nové služby Hello tooyour manifest aplikace][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a><span data-ttu-id="14f67-143">Balíček aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="14f67-143">Package your Service Fabric application</span></span>
<span data-ttu-id="14f67-144">aplikace hello toodeploy a jeho služeb tooa cluster, je nutné toocreate balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="14f67-144">toodeploy hello application and its services tooa cluster, you need toocreate an application package.</span></span>  <span data-ttu-id="14f67-145">balíček Hello organizuje manifest aplikace hello manifesty služby a jiné potřebné soubory v konkrétní rozložení.</span><span class="sxs-lookup"><span data-stu-id="14f67-145">hello package organizes hello application manifest, service manifests, and other necessary files in a specific layout.</span></span>  <span data-ttu-id="14f67-146">Visual Studio nastavuje a spravuje hello balíček v projektu aplikace hello složky v adresáři 'pkg' hello.</span><span class="sxs-lookup"><span data-stu-id="14f67-146">Visual Studio sets up and manages hello package in hello application project's folder, in hello 'pkg' directory.</span></span>  <span data-ttu-id="14f67-147">Kliknutím na tlačítko **balíček** z hello **aplikace** vytvoří místní nabídky nebo aktualizace hello balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="14f67-147">Clicking **Package** from hello **Application** context menu creates or updates hello application package.</span></span>

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a><span data-ttu-id="14f67-148">Odebrat aplikace a typy aplikací pomocí Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="14f67-148">Remove applications and application types using Cloud Explorer</span></span>
<span data-ttu-id="14f67-149">Můžete provádět operace správy základní cluster z v sadě Visual Studio pomocí Průzkumníku cloudu, který můžete spustit z hello **zobrazení** nabídky.</span><span class="sxs-lookup"><span data-stu-id="14f67-149">You can perform basic cluster management operations from within Visual Studio using Cloud Explorer, which you can launch from hello **View** menu.</span></span> <span data-ttu-id="14f67-150">Můžete například odstranit aplikace a zrušit zřízení typy aplikací v clusterech místního nebo vzdáleného.</span><span class="sxs-lookup"><span data-stu-id="14f67-150">For instance, you can delete applications and unprovision application types on local or remote clusters.</span></span>

![Odebrání aplikace][removeapplication]

> [!TIP]
> <span data-ttu-id="14f67-152">Funkce správy bohatší clusteru, najdete v části [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="14f67-152">For richer cluster management functionality, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
>
>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="14f67-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14f67-153">Next steps</span></span>
* [<span data-ttu-id="14f67-154">Model aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="14f67-154">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="14f67-155">Nasazení aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="14f67-155">Service Fabric application deployment</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="14f67-156">Správa aplikací parametry pro prostředí s více</span><span class="sxs-lookup"><span data-stu-id="14f67-156">Managing application parameters for multiple environments</span></span>](service-fabric-manage-multiple-environment-app-configuration.md)
* [<span data-ttu-id="14f67-157">Ladění aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="14f67-157">Debugging your Service Fabric application</span></span>](service-fabric-debugging-your-application.md)
* [<span data-ttu-id="14f67-158">Vizualizace vašeho clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="14f67-158">Visualizing your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png