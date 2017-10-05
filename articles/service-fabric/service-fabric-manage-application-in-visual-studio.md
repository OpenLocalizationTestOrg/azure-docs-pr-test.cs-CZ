---
title: "Správa aplikací v sadě Visual Studio | Microsoft Docs"
description: "Pomocí sady Visual Studio k vytvoření, vývoji, balíčku, nasazení a ladění aplikací Service Fabric a služeb."
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
ms.openlocfilehash: 3f6a47a15b74a7ceb6504b2834be62e76ab70bcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a><span data-ttu-id="f3c14-103">Pomocí sady Visual Studio zjednodušují zápis a správu aplikací Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f3c14-103">Use Visual Studio to simplify writing and managing your Service Fabric applications</span></span>
<span data-ttu-id="f3c14-104">Můžete spravovat vaše aplikace Azure Service Fabric a služby pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f3c14-104">You can manage your Azure Service Fabric applications and services through Visual Studio.</span></span> <span data-ttu-id="f3c14-105">Jakmile jste [nastavení vývojového prostředí](service-fabric-get-started.md), Visual Studio můžete použít k vytvoření aplikací Service Fabric, přidání služby nebo balíček, registrace a nasazení aplikací v místní vývojový cluster.</span><span class="sxs-lookup"><span data-stu-id="f3c14-105">Once you've [set up your development environment](service-fabric-get-started.md), you can use Visual Studio to create Service Fabric applications, add services, or package, register, and deploy applications in your local development cluster.</span></span>

## <a name="deploy-your-service-fabric-application"></a><span data-ttu-id="f3c14-106">Nasazení aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f3c14-106">Deploy your Service Fabric application</span></span>
<span data-ttu-id="f3c14-107">Ve výchozím nastavení nasazení aplikace kombinuje následující kroky do jednoho jednoduché operace:</span><span class="sxs-lookup"><span data-stu-id="f3c14-107">By default, deploying an application combines the following steps into one simple operation:</span></span>

1. <span data-ttu-id="f3c14-108">Vytvoření balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="f3c14-108">Creating the application package</span></span>
2. <span data-ttu-id="f3c14-109">Nahrávání balíčku aplikace do úložiště bitové kopie</span><span class="sxs-lookup"><span data-stu-id="f3c14-109">Uploading the application package to the image store</span></span>
3. <span data-ttu-id="f3c14-110">Registrace typu aplikace</span><span class="sxs-lookup"><span data-stu-id="f3c14-110">Registering the application type</span></span>
4. <span data-ttu-id="f3c14-111">Odebrat všechny spuštěné instance aplikace</span><span class="sxs-lookup"><span data-stu-id="f3c14-111">Removing any running application instances</span></span>
5. <span data-ttu-id="f3c14-112">Vytvoření instance aplikace</span><span class="sxs-lookup"><span data-stu-id="f3c14-112">Creating an application instance</span></span>

<span data-ttu-id="f3c14-113">Ve Visual Studiu stisknutím **F5** nasadí aplikaci a připojit ladicí program na všechny instance aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3c14-113">In Visual Studio, pressing **F5** deploys your application and attach the debugger to all application instances.</span></span> <span data-ttu-id="f3c14-114">Můžete použít **Ctrl + F5** k nasazení aplikace bez ladění, nebo můžete publikovat do místního nebo vzdáleného clusteru pomocí profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="f3c14-114">You can use **Ctrl+F5** to deploy an application without debugging, or you can publish to a local or remote cluster by using the publish profile.</span></span> <span data-ttu-id="f3c14-115">Další informace najdete v tématu [publikování aplikace do vzdáleného clusteru pomocí sady Visual Studio](service-fabric-publish-app-remote-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="f3c14-115">For more information, see [Publish an application to a remote cluster by using Visual Studio](service-fabric-publish-app-remote-cluster.md).</span></span>

### <a name="application-debug-mode"></a><span data-ttu-id="f3c14-116">Režim ladění aplikací</span><span class="sxs-lookup"><span data-stu-id="f3c14-116">Application Debug Mode</span></span>
<span data-ttu-id="f3c14-117">Visual Studio poskytují vlastnost s názvem **režim ladění aplikací**, který určuje, jakým způsobem chcete Visual Studia pro zpracování nasazení aplikace v rámci ladění.</span><span class="sxs-lookup"><span data-stu-id="f3c14-117">Visual Studio provide a property called **Application Debug Mode**, which controls how you want Visual Studios to handle Application deployment as part of debugging.</span></span>

#### <a name="to-set-the-application-debug-mode-property"></a><span data-ttu-id="f3c14-118">Chcete-li nastavit vlastnost režim ladění aplikací</span><span class="sxs-lookup"><span data-stu-id="f3c14-118">To set the Application Debug Mode property</span></span>
1. <span data-ttu-id="f3c14-119">V Service Fabric aplikace projektu (*.sfproj) místní nabídky zvolte **vlastnosti** (nebo stiskněte klávesu **F4** klíč).</span><span class="sxs-lookup"><span data-stu-id="f3c14-119">On the Service Fabric application project's (*.sfproj) shortcut menu, choose **Properties** (or press the **F4** key).</span></span>
2. <span data-ttu-id="f3c14-120">V **vlastnosti** nastavte **režim ladění aplikací** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f3c14-120">In the **Properties** window, set the **Application Debug Mode** property.</span></span>

![Nastavte vlastnost režimu ladění aplikace][debugmodeproperty]

#### <a name="application-debug-modes"></a><span data-ttu-id="f3c14-122">Režim ladění aplikace</span><span class="sxs-lookup"><span data-stu-id="f3c14-122">Application Debug Modes</span></span>

1. <span data-ttu-id="f3c14-123">**Aktualizovat aplikaci** tento režim umožňuje rychle změnit a ladění kódu a podporuje úpravy statických webových souborů při ladění.</span><span class="sxs-lookup"><span data-stu-id="f3c14-123">**Refresh Application** This mode enables you to quickly change and debug your code and supports editing static web files while debugging.</span></span> <span data-ttu-id="f3c14-124">Tento režim funguje jenom v případě místního vývojového clusteru je v [režim 1 uzel](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span><span class="sxs-lookup"><span data-stu-id="f3c14-124">This mode only works if your local development cluster is in [1-Node mode](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span></span>
2. <span data-ttu-id="f3c14-125">**Odebrání aplikace** způsobí, že aplikace má být odebrán při ukončení relace ladění.</span><span class="sxs-lookup"><span data-stu-id="f3c14-125">**Remove Application** causes the application to be removed when the debug session ends.</span></span>
3. <span data-ttu-id="f3c14-126">**Automaticky Upgrade** aplikace stále běží při ukončení relace ladění.</span><span class="sxs-lookup"><span data-stu-id="f3c14-126">**Auto Upgrade** The application continues to run when the debug session ends.</span></span> <span data-ttu-id="f3c14-127">Další relaci ladění bude považovat za nasazení upgradu.</span><span class="sxs-lookup"><span data-stu-id="f3c14-127">The next debug session will treat the deployment as an upgrade.</span></span> <span data-ttu-id="f3c14-128">Proces upgradu zachová všechna data, která jste zadali v předchozí relaci ladění.</span><span class="sxs-lookup"><span data-stu-id="f3c14-128">The upgrade process preserves any data that you entered in a previous debug session.</span></span>
4. <span data-ttu-id="f3c14-129">**Zachovat aplikace** aplikace neustále běží v clusteru při ukončení relace ladění.</span><span class="sxs-lookup"><span data-stu-id="f3c14-129">**Keep Application** The application keeps running in the cluster when the debug session ends.</span></span> <span data-ttu-id="f3c14-130">Na začátku další relaci ladění, se odeberou aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3c14-130">At the beginning of the next debug session, the application will be removed.</span></span>

<span data-ttu-id="f3c14-131">Pro **automatického upgradu** uchování dat s použitím možnosti upgradu aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="f3c14-131">For **Auto Upgrade** data is preserved by applying the application upgrade capabilities of Service Fabric.</span></span> <span data-ttu-id="f3c14-132">Další informace o upgrade aplikace a jak můžete provést upgrade v prostředí skutečné najdete v tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="f3c14-132">For more information about upgrading applications and how you might perform an upgrade in a real environment, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

## <a name="add-a-service-to-your-service-fabric-application"></a><span data-ttu-id="f3c14-133">Přidání služby do aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f3c14-133">Add a service to your Service Fabric application</span></span>
<span data-ttu-id="f3c14-134">Nové služby je možné přidat do vaší aplikace pro rozšíření její funkce.</span><span class="sxs-lookup"><span data-stu-id="f3c14-134">You can add new services to your application to extend its functionality.</span></span>  <span data-ttu-id="f3c14-135">Aby se zajistilo, že služba je součástí balíčku aplikace, přidejte služby pomocí **nové služby technologie Fabric...**  položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="f3c14-135">To ensure that the service is included in your application package, add the service through the **New Fabric Service...** menu item.</span></span>

![Přidejte novou službu Service Fabric][newservice]

<span data-ttu-id="f3c14-137">Vyberte typ projektu Service Fabric přidat do vaší aplikace a zadejte název pro službu.</span><span class="sxs-lookup"><span data-stu-id="f3c14-137">Select a Service Fabric project type to add to your application, and specify a name for the service.</span></span>  <span data-ttu-id="f3c14-138">V tématu [výběr rozhraní služby](service-fabric-choose-framework.md) vám pomohou rozhodnout typů služby, které se mají použít.</span><span class="sxs-lookup"><span data-stu-id="f3c14-138">See [Choosing a framework for your service](service-fabric-choose-framework.md) to help you decide which service type to use.</span></span>

![Vyberte typ projektu služby Service Fabric k přidání do aplikace][addserviceproject]

<span data-ttu-id="f3c14-140">Nové služby se přidá do vašeho řešení a stávající balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3c14-140">The new service is added to your solution and existing application package.</span></span> <span data-ttu-id="f3c14-141">Odkazy na službu a výchozí instance služby se zařadí do manifestu aplikace, která způsobila služba vytvořena a spuštěna při příštím nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3c14-141">The service references and a default service instance will be added to the application manifest, causing the service to be created and started the next time you deploy the application.</span></span>

![Nové služby se přidá do manifestu aplikace][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a><span data-ttu-id="f3c14-143">Balíček aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f3c14-143">Package your Service Fabric application</span></span>
<span data-ttu-id="f3c14-144">Pokud chcete nasadit aplikace a služby do clusteru, musíte vytvořit balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3c14-144">To deploy the application and its services to a cluster, you need to create an application package.</span></span>  <span data-ttu-id="f3c14-145">Balíček organizuje manifest aplikace, manifesty služby a jiné potřebné soubory v konkrétní rozložení.</span><span class="sxs-lookup"><span data-stu-id="f3c14-145">The package organizes the application manifest, service manifests, and other necessary files in a specific layout.</span></span>  <span data-ttu-id="f3c14-146">Visual Studio nastavuje a spravuje balíček v projektu aplikace složky v adresáři 'pkg'.</span><span class="sxs-lookup"><span data-stu-id="f3c14-146">Visual Studio sets up and manages the package in the application project's folder, in the 'pkg' directory.</span></span>  <span data-ttu-id="f3c14-147">Kliknutím na tlačítko **balíček** z **aplikace** kontextovou nabídku vytvoří nebo aktualizuje balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3c14-147">Clicking **Package** from the **Application** context menu creates or updates the application package.</span></span>

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a><span data-ttu-id="f3c14-148">Odebrat aplikace a typy aplikací pomocí Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="f3c14-148">Remove applications and application types using Cloud Explorer</span></span>
<span data-ttu-id="f3c14-149">Můžete provádět operace správy základní cluster z v sadě Visual Studio pomocí Průzkumníku cloudu, který můžete spustit z **zobrazení** nabídky.</span><span class="sxs-lookup"><span data-stu-id="f3c14-149">You can perform basic cluster management operations from within Visual Studio using Cloud Explorer, which you can launch from the **View** menu.</span></span> <span data-ttu-id="f3c14-150">Můžete například odstranit aplikace a zrušit zřízení typy aplikací v clusterech místního nebo vzdáleného.</span><span class="sxs-lookup"><span data-stu-id="f3c14-150">For instance, you can delete applications and unprovision application types on local or remote clusters.</span></span>

![Odebrání aplikace][removeapplication]

> [!TIP]
> <span data-ttu-id="f3c14-152">Funkce správy bohatší clusteru, najdete v části [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="f3c14-152">For richer cluster management functionality, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
>
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="f3c14-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3c14-153">Next steps</span></span>
* [<span data-ttu-id="f3c14-154">Model aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f3c14-154">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="f3c14-155">Nasazení aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f3c14-155">Service Fabric application deployment</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="f3c14-156">Správa aplikací parametry pro prostředí s více</span><span class="sxs-lookup"><span data-stu-id="f3c14-156">Managing application parameters for multiple environments</span></span>](service-fabric-manage-multiple-environment-app-configuration.md)
* [<span data-ttu-id="f3c14-157">Ladění aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f3c14-157">Debugging your Service Fabric application</span></span>](service-fabric-debugging-your-application.md)
* [<span data-ttu-id="f3c14-158">Vizualizace vašeho clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="f3c14-158">Visualizing your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png