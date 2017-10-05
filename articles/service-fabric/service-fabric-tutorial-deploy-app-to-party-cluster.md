---
title: "Nasazení aplikace Azure Service Fabric do clusteru s podporou strany | Microsoft Docs"
description: "Zjistěte, jak nasadit aplikaci do clusteru s podporou strany."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.openlocfilehash: 6624d683edb548a65d07ab4012c599faaf940ed0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-an-application-to-a-party-cluster-in-azure"></a><span data-ttu-id="8235f-103">Nasazení aplikace do clusteru s podporou strany v Azure</span><span class="sxs-lookup"><span data-stu-id="8235f-103">Deploy an application to a Party Cluster in Azure</span></span>
<span data-ttu-id="8235f-104">V tomto kurzu je součástí dvě řady a ukazuje, jak nasadit aplikaci Azure Service Fabric do clusteru s podporou strany v Azure.</span><span class="sxs-lookup"><span data-stu-id="8235f-104">This tutorial is part two of a series and shows you how to deploy an Azure Service Fabric application to a Party Cluster in Azure.</span></span>

<span data-ttu-id="8235f-105">V druhé části kurzu řady zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="8235f-105">In part two of the tutorial series, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="8235f-106">Nasazení aplikace do vzdáleného clusteru pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8235f-106">Deploy an application to a remote cluster using Visual Studio</span></span>
> * <span data-ttu-id="8235f-107">Odebrání aplikace z clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="8235f-107">Remove an application from a cluster using Service Fabric Explorer</span></span>

<span data-ttu-id="8235f-108">V této série kurzu zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="8235f-108">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * [<span data-ttu-id="8235f-109">Sestavení aplikace .NET Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8235f-109">Build a .NET Service Fabric application</span></span>](service-fabric-tutorial-create-dotnet-app.md)
> * <span data-ttu-id="8235f-110">Nasazení aplikace na vzdálený cluster</span><span class="sxs-lookup"><span data-stu-id="8235f-110">Deploy the application to a remote cluster</span></span>
> * [<span data-ttu-id="8235f-111">Konfigurace CI/CD pomocí Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="8235f-111">Configure CI/CD using Visual Studio Team Services</span></span>](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a><span data-ttu-id="8235f-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8235f-112">Prerequisites</span></span>
<span data-ttu-id="8235f-113">Před zahájením tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="8235f-113">Before you begin this tutorial:</span></span>
- <span data-ttu-id="8235f-114">Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="8235f-114">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="8235f-115">[Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte **Azure development** a **ASP.NET a webové vývoj** úlohy.</span><span class="sxs-lookup"><span data-stu-id="8235f-115">[Install Visual Studio 2017](https://www.visualstudio.com/) and install the **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="8235f-116">Instalace Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="8235f-116">Install the Service Fabric SDK</span></span>](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a><span data-ttu-id="8235f-117">Stažení ukázkové aplikace Voting</span><span class="sxs-lookup"><span data-stu-id="8235f-117">Download the Voting sample application</span></span>
<span data-ttu-id="8235f-118">Pokud není sestavení Voting ukázkovou aplikaci [součástí jeden z této série kurz](service-fabric-tutorial-create-dotnet-app.md), můžete ho stáhnout.</span><span class="sxs-lookup"><span data-stu-id="8235f-118">If you did not build the Voting sample application in [part one of this tutorial series](service-fabric-tutorial-create-dotnet-app.md), you can download it.</span></span> <span data-ttu-id="8235f-119">V příkazovém okně spusťte následující příkaz, který klonovat úložiště ukázkové aplikace do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="8235f-119">In a command window, run the following command to clone the sample app repository to your local machine.</span></span>

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-a-party-cluster"></a><span data-ttu-id="8235f-120">Nastavení clusteru strany</span><span class="sxs-lookup"><span data-stu-id="8235f-120">Set up a Party Cluster</span></span>
<span data-ttu-id="8235f-121">Party clustery jsou bezplatné, časově omezené clustery Service Fabric hostované v Azure a provozované týmem Service Fabric, na kterých může kdokoli nasazovat aplikace a seznamovat se s platformou.</span><span class="sxs-lookup"><span data-stu-id="8235f-121">Party clusters are free, limited-time Service Fabric clusters hosted on Azure and run by the Service Fabric team where anyone can deploy applications and learn about the platform.</span></span> <span data-ttu-id="8235f-122">Zdarma!</span><span class="sxs-lookup"><span data-stu-id="8235f-122">For free!</span></span>

<span data-ttu-id="8235f-123">Chcete-li získat přístup ke clusteru s podporou strany, přejděte do této lokality: http://aka.ms/tryservicefabric a postupujte podle pokynů a získat přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="8235f-123">To get access to a Party Cluster, browse to this site: http://aka.ms/tryservicefabric and follow the instructions to get access to a cluster.</span></span> <span data-ttu-id="8235f-124">Potřebujete účet Facebook nebo Githubu a získat přístup ke clusteru s podporou strany.</span><span class="sxs-lookup"><span data-stu-id="8235f-124">You need a Facebook or GitHub account to get access to a Party Cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="8235f-125">Strany clustery není zabezpečená, aby vaše aplikace a všechna data, která jste uložili v nich může být vidět další uživatelé.</span><span class="sxs-lookup"><span data-stu-id="8235f-125">Party clusters are not secured, so your applications and any data you put in them may be visible to others.</span></span> <span data-ttu-id="8235f-126">Nenasazujte nic nechcete vidět.</span><span class="sxs-lookup"><span data-stu-id="8235f-126">Don't deploy anything you don't want others to see.</span></span> <span data-ttu-id="8235f-127">Nezapomeňte si přečíst přes naše podmínky použití pro všechny podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8235f-127">Be sure to read over our Terms of Use for all the details.</span></span>

## <a name="configure-the-listening-port"></a><span data-ttu-id="8235f-128">Nakonfigurujte port pro naslouchání</span><span class="sxs-lookup"><span data-stu-id="8235f-128">Configure the listening port</span></span>
<span data-ttu-id="8235f-129">Při vytváření front-endová služba VotingWeb Visual Studio náhodně vybere port pro službu pro naslouchání.</span><span class="sxs-lookup"><span data-stu-id="8235f-129">When the VotingWeb front-end service is created, Visual Studio randomly selects a port for the service to listen on.</span></span>  <span data-ttu-id="8235f-130">Služba VotingWeb funguje jako front-end pro tuto aplikaci a přijímá externích přenosů, můžeme vytvořit vazbu pevná, služby a také znát port.</span><span class="sxs-lookup"><span data-stu-id="8235f-130">The VotingWeb service acts as the front-end for this application and accepts external traffic, so let's bind that service to a fixed and well-know port.</span></span> <span data-ttu-id="8235f-131">V Průzkumníku řešení otevřete *VotingWeb/PackageRoot/ServiceManifest.xml*.</span><span class="sxs-lookup"><span data-stu-id="8235f-131">In Solution Explorer, open  *VotingWeb/PackageRoot/ServiceManifest.xml*.</span></span>  <span data-ttu-id="8235f-132">Najít **koncový bod** prostředku v **prostředky** části a změňte **Port** hodnotu 80.</span><span class="sxs-lookup"><span data-stu-id="8235f-132">Find the **Endpoint** resource in the **Resources** section and change the **Port** value to 80.</span></span>

```xml
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="80" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="8235f-133">Také aktualizujte hodnotu vlastnosti Adresa URL aplikace v projektu Voting, otevře se webový prohlížeč na správný port při ladění pomocí 'F5'.</span><span class="sxs-lookup"><span data-stu-id="8235f-133">Also update the Application URL property value in the Voting project so a web browser opens to the correct port when you debug using 'F5'.</span></span>  <span data-ttu-id="8235f-134">V Průzkumníku řešení, vyberte **Voting** projekt a aktualizace **adresa URL aplikace** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8235f-134">In Solution Explorer, select the **Voting** project and update the **Application URL** property.</span></span>

![Adresa URL aplikace](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-url.png)

## <a name="deploy-the-app-to-the-azure"></a><span data-ttu-id="8235f-136">Nasazení aplikace na Azure</span><span class="sxs-lookup"><span data-stu-id="8235f-136">Deploy the app to the Azure</span></span>
<span data-ttu-id="8235f-137">Teď, když je aplikace připravená, můžete ho nasadit do clusteru strany přímé ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8235f-137">Now that the application is ready, you can deploy it to the Party Cluster direct from Visual Studio.</span></span>

1. <span data-ttu-id="8235f-138">Klikněte pravým tlačítkem na **Voting** v Průzkumníku řešení a zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8235f-138">Right-click **Voting** in the Solution Explorer and choose **Publish**.</span></span>

    ![Dialogové okno Publikovat](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

2. <span data-ttu-id="8235f-140">Typ v koncovém bodě připojení clusteru, strana **koncového bodu připojení** pole a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8235f-140">Type in the Connection Endpoint of the Party Cluster in the **Connection Endpoint** field and click **Publish**.</span></span>

    <span data-ttu-id="8235f-141">Po dokončení publikování, byste měli mít k odeslání požadavku na aplikaci prostřednictvím prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="8235f-141">Once the publish has finished, you should be able to send a request to the application via a browser.</span></span>

3. <span data-ttu-id="8235f-142">Otevřít vaše preferované prohlížeč a zadejte adresu clusteru (koncového bodu připojení bez informace o portu – například win1kw5649s.westus.cloudapp.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8235f-142">Open you preferred browser and type in the cluster address (the connection endpoint without the port information - for example, win1kw5649s.westus.cloudapp.azure.com).</span></span>

    <span data-ttu-id="8235f-143">Teď byste měli vidět stejného výsledku, jako jste viděli při spuštění aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="8235f-143">You should now see the same result as you saw when running the application locally.</span></span>

    ![Rozhraní API odpověď z clusteru](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-the-application-from-a-cluster-using-service-fabric-explorer"></a><span data-ttu-id="8235f-145">Odebrání aplikace z clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="8235f-145">Remove the application from a cluster using Service Fabric Explorer</span></span>
<span data-ttu-id="8235f-146">Service Fabric Explorer je grafické uživatelské rozhraní umožní zkoumat a správě aplikací v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="8235f-146">Service Fabric Explorer is a graphical user interface to explore and manage applications in a Service Fabric cluster.</span></span>

<span data-ttu-id="8235f-147">Lze aplikaci odebrat z clusteru strany:</span><span class="sxs-lookup"><span data-stu-id="8235f-147">To remove the application from the Party Cluster:</span></span>

1. <span data-ttu-id="8235f-148">Přejděte do Service Fabric Explorer pomocí odkazu poskytované stránku pro přihlášení strany clusteru.</span><span class="sxs-lookup"><span data-stu-id="8235f-148">Browse to the Service Fabric Explorer, using the link provided by the Party Cluster sign-up page.</span></span> <span data-ttu-id="8235f-149">Například http://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.</span><span class="sxs-lookup"><span data-stu-id="8235f-149">For example, http://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.</span></span>

2. <span data-ttu-id="8235f-150">V Service Fabric Exploreru, přejděte na **fabric://Voting** uzlu ve stromovém zobrazení na levé straně.</span><span class="sxs-lookup"><span data-stu-id="8235f-150">In Service Fabric Explorer, navigate to the **fabric://Voting** node in the treeview on the left-hand side.</span></span>

3. <span data-ttu-id="8235f-151">Klikněte **akce** tlačítko v pravém **Essentials** podokně a zvolte **odstranit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="8235f-151">Click the **Action** button in the right-hand **Essentials** pane, and choose **Delete Application**.</span></span> <span data-ttu-id="8235f-152">Potvrďte odstranění instanci aplikace, která odebere instanci naše aplikace běžící v clusteru.</span><span class="sxs-lookup"><span data-stu-id="8235f-152">Confirm deleting the application instance, which removes the instance of our application running in the cluster.</span></span>

![Odstranění aplikace v Service Fabric Exploreru](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

## <a name="remove-the-application-type-from-a-cluster-using-service-fabric-explorer"></a><span data-ttu-id="8235f-154">Typ aplikace odebrat z clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="8235f-154">Remove the application type from a cluster using Service Fabric Explorer</span></span>
<span data-ttu-id="8235f-155">Aplikace jsou nasazeny jako typy aplikací v clusteru Service Fabric, která umožňuje mít víc instancí a verze aplikace běžící v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="8235f-155">Applications are deployed as application types in a Service Fabric cluster, which enables you to have multiple instances and versions of the application running within the cluster.</span></span> <span data-ttu-id="8235f-156">Po odebrání spuštěnou instanci aplikace, jsme rovněž můžete odebrat typu, k dokončení vyčištění nasazení.</span><span class="sxs-lookup"><span data-stu-id="8235f-156">After having removed the running instance of our application, we can also remove the type, to complete the cleanup of the deployment.</span></span>

<span data-ttu-id="8235f-157">Další informace o modelu aplikace v Service Fabric najdete v tématu [Model aplikace v Service Fabric](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="8235f-157">For more information about the application model in Service Fabric, see [Model an application in Service Fabric](service-fabric-application-model.md).</span></span>

1. <span data-ttu-id="8235f-158">Přejděte na **VotingType** uzlu ve stromovém zobrazení.</span><span class="sxs-lookup"><span data-stu-id="8235f-158">Navigate to the **VotingType** node in the treeview.</span></span>

2. <span data-ttu-id="8235f-159">Klikněte na tlačítko **akce** tlačítko v pravém **Essentials** podokně a zvolte **Unprovision typu**.</span><span class="sxs-lookup"><span data-stu-id="8235f-159">Click the **Action** button in the right-hand **Essentials** pane, and choose **Unprovision Type**.</span></span> <span data-ttu-id="8235f-160">Potvrďte rušení zajišťování typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="8235f-160">Confirm unprovisioning the application type.</span></span>

![Zrušit zřízení typu aplikace v Service Fabric Exploreru](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

<span data-ttu-id="8235f-162">Tím je kurz ukončen.</span><span class="sxs-lookup"><span data-stu-id="8235f-162">This concludes the tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8235f-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8235f-163">Next steps</span></span>
<span data-ttu-id="8235f-164">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="8235f-164">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8235f-165">Nasazení aplikace do vzdáleného clusteru pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8235f-165">Deploy an application to a remote cluster using Visual Studio</span></span>
> * <span data-ttu-id="8235f-166">Odebrání aplikace z clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="8235f-166">Remove an application from a cluster using Service Fabric Explorer</span></span>

<span data-ttu-id="8235f-167">Přechodu na další kurz:</span><span class="sxs-lookup"><span data-stu-id="8235f-167">Advance to the next tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="8235f-168">Nastavte průběžnou integraci pomocí Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="8235f-168">Set up continuous integration using Visual Studio Team Services</span></span>](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)