---
title: "aaaQuickly nasazení stávajícího clusteru Azure Service Fabric tooan aplikace"
description: "Pomocí Azure Service Fabric clusteru toohost stávající aplikace Node.js pomocí sady Visual Studio."
services: service-fabric
documentationcenter: nodejs
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: adegeo
ms.openlocfilehash: 20a3eb4a9206ba465acf96d0976ba241b07158bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="host-a-nodejs-application-on-azure-service-fabric"></a><span data-ttu-id="bf7b9-103">Hostování aplikace Node.js na platformě Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="bf7b9-103">Host a Node.js application on Azure Service Fabric</span></span>

<span data-ttu-id="bf7b9-104">Tento rychlý start vám pomůže nasadit existující aplikace (Node.js v tomto příkladu) tooa Service Fabric cluster spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-104">This quickstart helps you deploy an existing application (Node.js in this example) tooa Service Fabric cluster running on Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf7b9-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bf7b9-105">Prerequisites</span></span>

<span data-ttu-id="bf7b9-106">Než začnete, ujistěte se, že máte [nastavené vývojové prostředí](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bf7b9-106">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="bf7b9-107">Které zahrnuje instalaci hello Service Fabric SDK a Visual Studio 2017 nebo 2015.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-107">Which includes installing hello Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

<span data-ttu-id="bf7b9-108">Budete také potřebovat toohave existující aplikaci Node.js pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-108">You also need toohave an existing Node.js application for deployment.</span></span> <span data-ttu-id="bf7b9-109">V tomto rychlém startu se používá jednoduchý web v Node.js, který je ke stažení [zde][download-sample].</span><span class="sxs-lookup"><span data-stu-id="bf7b9-109">This quickstart uses a simple Node.js website that can be downloaded [here][download-sample].</span></span> <span data-ttu-id="bf7b9-110">Extrahování tento soubor tooyour `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` složky Po vytvoření projektu hello v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-110">Extract this file tooyour `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` folder after you create hello project in hello next step.</span></span>

<span data-ttu-id="bf7b9-111">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet][create-account].</span><span class="sxs-lookup"><span data-stu-id="bf7b9-111">If you don't have an Azure subscription, create a [free account][create-account].</span></span>

## <a name="create-hello-service"></a><span data-ttu-id="bf7b9-112">Vytvoření služby hello</span><span class="sxs-lookup"><span data-stu-id="bf7b9-112">Create hello service</span></span>

<span data-ttu-id="bf7b9-113">Spusťte sadu Visual Studio jako **správce**.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-113">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="bf7b9-114">Vytvořte projekt pomocí klávesové zkratky `CTRL`+`SHIFT`+`N`.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-114">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="bf7b9-115">V hello **nový projekt** dialogovém okně, vyberte **Cloud > aplikace Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-115">In hello **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="bf7b9-116">Název aplikace hello **MyGuestApp** a stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-116">Name hello application **MyGuestApp** and press **OK**.</span></span>

>[!IMPORTANT]
><span data-ttu-id="bf7b9-117">Node.js můžete snadno rozdělit hello 260 znaků limit pro cesty, které má windows.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-117">Node.js can easily break hello 260 character limit for paths that windows has.</span></span> <span data-ttu-id="bf7b9-118">Například použijte krátký cestu samotném projektu hello **c:\code\svc1**.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-118">Use a short path for hello project itself such as **c:\code\svc1**.</span></span>
   
![Dialogové okno Nový projekt ve Visual Studiu][new-project]

<span data-ttu-id="bf7b9-120">Můžete vytvořit jakýkoli typ služby Service Fabric z dialogového okna Další hello.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-120">You can create any type of Service Fabric service from hello next dialog.</span></span> <span data-ttu-id="bf7b9-121">Pro účely tohoto Rychlého startu zvolte **Spustitelný soubor typu Host**.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-121">For this quickstart, choose **Guest Executable**.</span></span>

<span data-ttu-id="bf7b9-122">Název služby hello **MyGuestService** a možnosti pro sadu hello na pravém toohello hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="bf7b9-122">Name hello service **MyGuestService** and set hello options on hello right toohello following values:</span></span>

| <span data-ttu-id="bf7b9-123">Nastavení</span><span class="sxs-lookup"><span data-stu-id="bf7b9-123">Setting</span></span>                   | <span data-ttu-id="bf7b9-124">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bf7b9-124">Value</span></span> |
| ------------------------- | ------ |
| <span data-ttu-id="bf7b9-125">Složka balíčku kódu</span><span class="sxs-lookup"><span data-stu-id="bf7b9-125">Code Package Folder</span></span>       | <span data-ttu-id="bf7b9-126">_&lt;Složka Hello se aplikace Node.js&gt;_</span><span class="sxs-lookup"><span data-stu-id="bf7b9-126">_&lt;hello folder with your Node.js app&gt;_</span></span> |
| <span data-ttu-id="bf7b9-127">Chování balíčku kódu</span><span class="sxs-lookup"><span data-stu-id="bf7b9-127">Code Package Behavior</span></span>     | <span data-ttu-id="bf7b9-128">Zkopírujte složku obsah tooproject</span><span class="sxs-lookup"><span data-stu-id="bf7b9-128">Copy folder contents tooproject</span></span> |
| <span data-ttu-id="bf7b9-129">Program</span><span class="sxs-lookup"><span data-stu-id="bf7b9-129">Program</span></span>                   | <span data-ttu-id="bf7b9-130">node.exe</span><span class="sxs-lookup"><span data-stu-id="bf7b9-130">node.exe</span></span> |
| <span data-ttu-id="bf7b9-131">Argumenty</span><span class="sxs-lookup"><span data-stu-id="bf7b9-131">Arguments</span></span>                 | <span data-ttu-id="bf7b9-132">server.js</span><span class="sxs-lookup"><span data-stu-id="bf7b9-132">server.js</span></span> |
| <span data-ttu-id="bf7b9-133">Pracovní složka</span><span class="sxs-lookup"><span data-stu-id="bf7b9-133">Working Folder</span></span>            | <span data-ttu-id="bf7b9-134">CodePackage</span><span class="sxs-lookup"><span data-stu-id="bf7b9-134">CodePackage</span></span> |

<span data-ttu-id="bf7b9-135">Stiskněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-135">Press **OK**.</span></span>

![Dialogové okno Nová služba ve Visual Studiu][new-service]

<span data-ttu-id="bf7b9-137">Visual Studio vytvoří projekt aplikace hello a projekt služby objektu actor hello a zobrazí je v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-137">Visual Studio creates hello application project and hello actor service project and displays them in Solution Explorer.</span></span>

<span data-ttu-id="bf7b9-138">projekt aplikace Hello (**MyGuestApp**) přímo neobsahuje žádný kód.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-138">hello application project (**MyGuestApp**) does not contain any code directly.</span></span> <span data-ttu-id="bf7b9-139">Odkazuje ale na sadu projektů služeb.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-139">Instead, it references a set of service projects.</span></span> <span data-ttu-id="bf7b9-140">Kromě toho obsahuje další tři typy obsahu:</span><span class="sxs-lookup"><span data-stu-id="bf7b9-140">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="bf7b9-141">**Profily publikování**</span><span class="sxs-lookup"><span data-stu-id="bf7b9-141">**Publish profiles**</span></span>  
<span data-ttu-id="bf7b9-142">Předvolby nástrojů pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-142">Tooling preferences for different environments.</span></span>

* <span data-ttu-id="bf7b9-143">**Skripty**</span><span class="sxs-lookup"><span data-stu-id="bf7b9-143">**Scripts**</span></span>  
<span data-ttu-id="bf7b9-144">Skript PowerShellu pro nasazení/upgrade aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-144">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="bf7b9-145">**Definice aplikace**</span><span class="sxs-lookup"><span data-stu-id="bf7b9-145">**Application definition**</span></span>  
<span data-ttu-id="bf7b9-146">Obsahuje manifest aplikace hello *ApplicationPackageRoot*.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-146">Includes hello application manifest under *ApplicationPackageRoot*.</span></span> <span data-ttu-id="bf7b9-147">Soubory parametrů přidružené aplikace jsou v části *ApplicationParameters*, které definují hello aplikace a umožní vám tooconfigure ji speciálně pro dané prostředí.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-147">Associated application parameter files are under *ApplicationParameters*, which define hello application and allow you tooconfigure it specifically for a given environment.</span></span>
    
<span data-ttu-id="bf7b9-148">Přehled hello obsah hello projektu služby najdete v tématu [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="bf7b9-148">For an overview of hello contents of hello service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="set-up-networking"></a><span data-ttu-id="bf7b9-149">Nastavení síťových služeb</span><span class="sxs-lookup"><span data-stu-id="bf7b9-149">Set up networking</span></span>

<span data-ttu-id="bf7b9-150">Příklad Hello aplikace Node.js jsme nasazovali používá port **80** a potřebujeme tootell Service Fabric, která je třeba, že port zpřístupněný.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-150">hello example Node.js app we're deploying uses port **80** and we need tootell Service Fabric that we need that port exposed.</span></span>

<span data-ttu-id="bf7b9-151">Otevřete hello **ServiceManifest.xml** soubor v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-151">Open hello **ServiceManifest.xml** file in hello project.</span></span> <span data-ttu-id="bf7b9-152">V dolní části hello hello manifestu, je `<Resources> \ <Endpoints>` se záznamem již definována.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-152">At hello bottom of hello manifest, there is a `<Resources> \ <Endpoints>` with an entry already defined.</span></span> <span data-ttu-id="bf7b9-153">Upravit tuto položku tooadd `Port`, `Protocol`, a `Type`.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-153">Modify that entry tooadd `Port`, `Protocol`, and `Type`.</span></span> 

```xml
  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyGuestAppServiceTypeEndpoint" Port="80" Protocol="http" Type="Input" />
    </Endpoints>
  </Resources>
```

## <a name="deploy-tooazure"></a><span data-ttu-id="bf7b9-154">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="bf7b9-154">Deploy tooAzure</span></span>

<span data-ttu-id="bf7b9-155">Pokud vyberete **F5** a spusťte projekt hello, je nasazené toohello místní cluster.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-155">If you press **F5** and run hello project, it is deployed toohello local cluster.</span></span> <span data-ttu-id="bf7b9-156">Ale nyní nasadíme tooAzure místo.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-156">However, let's deploy tooAzure instead.</span></span>

<span data-ttu-id="bf7b9-157">Klikněte pravým tlačítkem na projekt hello a zvolte **publikování...**  který otevře tooAzure toopublish dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-157">Right-click on hello project and choose **Publish...** which opens a dialog toopublish tooAzure.</span></span>

![Publikování tooazure dialogové okno pro službu service fabric][publish]

<span data-ttu-id="bf7b9-159">Vyberte hello **PublishProfiles\Cloud.xml** cíle profilu.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-159">Select hello **PublishProfiles\Cloud.xml** target profile.</span></span>

<span data-ttu-id="bf7b9-160">Pokud jste to ještě dříve, zvolte toodeploy účtu Azure k.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-160">If you haven't previously, choose an Azure account toodeploy to.</span></span> <span data-ttu-id="bf7b9-161">Pokud ještě žádný nemáte, [zaregistrujte si bezplatný účet][create-account].</span><span class="sxs-lookup"><span data-stu-id="bf7b9-161">If you don't have one yet, [sign-up for one][create-account].</span></span>

<span data-ttu-id="bf7b9-162">V části **koncového bodu připojení**, vyberte hello toodeploy clusteru Service Fabric na.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-162">Under **Connection Endpoint**, select hello Service Fabric cluster toodeploy to.</span></span> <span data-ttu-id="bf7b9-163">Pokud nemáte jeden, vyberte  **&lt;vytvoření nového clusteru... &gt;**  který otevře webový prohlížeč okno toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-163">If you do not have one, select **&lt;Create New Cluster...&gt;** which opens up web browser window toohello Azure portal.</span></span> <span data-ttu-id="bf7b9-164">Další informace najdete v tématu [vytvoření clusteru s podporou portálu hello](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="bf7b9-164">For more information, see [create a cluster in hello portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal).</span></span> 

<span data-ttu-id="bf7b9-165">Když vytvoříte cluster Service Fabric hello, ujistěte se, zda text hello tooset **vlastní koncové body** nastavení příliš**80**.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-165">When you create hello Service Fabric cluster, make sure tooset hello **Custom endpoints** setting too**80**.</span></span>

![Konfigurace typu uzlu Service Fabric s vlastním koncovým bodem][custom-endpoint]

<span data-ttu-id="bf7b9-167">Vytvoření nového clusteru Service Fabric trvá některé toocomplete čas.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-167">Creating a new Service Fabric cluster takes some time toocomplete.</span></span> <span data-ttu-id="bf7b9-168">Poté, co byl vytvořený, přejděte zpět toohello dialogového okna publikování a vyberte  **&lt;aktualizovat&gt;**.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-168">Once it has been created, go back toohello publish dialog and select **&lt;Refresh&gt;**.</span></span> <span data-ttu-id="bf7b9-169">nový cluster Hello je uvedena v rozevíracím seznamu hello; vyberte ho.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-169">hello new cluster is listed in hello drop-down box; select it.</span></span>

<span data-ttu-id="bf7b9-170">Stiskněte klávesu **publikovat** a počkejte toofinish nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-170">Press **Publish** and wait for hello deployment toofinish.</span></span>

<span data-ttu-id="bf7b9-171">Může to trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-171">This may take a few minutes.</span></span> <span data-ttu-id="bf7b9-172">Po jeho dokončení může trvat několik minut pro toobe aplikace hello plně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-172">After it completes, it may take a few more minutes for hello application toobe fully available.</span></span>

## <a name="test-hello-website"></a><span data-ttu-id="bf7b9-173">Test hello webu</span><span class="sxs-lookup"><span data-stu-id="bf7b9-173">Test hello website</span></span>

<span data-ttu-id="bf7b9-174">Jakmile bude vaše služba publikována, otestujte ji ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-174">After your service has been published, test it in a web browser.</span></span> 

<span data-ttu-id="bf7b9-175">První otevřete hello portál Azure a vyhledání služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-175">First, open hello Azure portal and find your Service Fabric service.</span></span>

<span data-ttu-id="bf7b9-176">Zkontrolujte okno Přehled hello adresu služby hello.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-176">Check hello overview blade of hello service address.</span></span> <span data-ttu-id="bf7b9-177">Použijte název domény hello z hello _koncového bodu připojení klienta_ vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-177">Use hello domain name from hello _Client connection endpoint_ property.</span></span> <span data-ttu-id="bf7b9-178">Například, `http://mysvcfab1.westus2.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-178">For example, `http://mysvcfab1.westus2.cloudapp.azure.com`.</span></span>

![Okno Přehled Service fabric na hello portálu Azure][overview]

<span data-ttu-id="bf7b9-180">Přejděte na adresu toothis, kde uvidíte hello `HELLO WORLD` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-180">Navigate toothis address where you will see hello `HELLO WORLD` response.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="bf7b9-181">Odstranění clusteru hello</span><span class="sxs-lookup"><span data-stu-id="bf7b9-181">Delete hello cluster</span></span>

<span data-ttu-id="bf7b9-182">Nevynechali toodelete všechny hello prostředky, které jste vytvořili pro tento rychlý start, jako je budou účtovat tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="bf7b9-182">Do not forget toodelete all of hello resources you've created for this quickstart, as you are charged for those resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf7b9-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf7b9-183">Next steps</span></span>
<span data-ttu-id="bf7b9-184">Další informace o [spustitelných souborech typu Host](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="bf7b9-184">Read more about [guest executables](service-fabric-deploy-existing-app.md).</span></span>

<!-- Image References -->

[new-project]: ./media/quickstart-guest-app/new-project.png
[new-service]: ./media/quickstart-guest-app/template.png
[solution-exp]: ./media/quickstart-guest-app/solution-explorer.png
[publish]: ./media/quickstart-guest-app/publish.png
[overview]: ./media/quickstart-guest-app/overview.png
[custom-endpoint]: ./media/quickstart-guest-app/custom-endpoint.png

[download-sample]: https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/service-fabric-node-website.zip
[create-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F