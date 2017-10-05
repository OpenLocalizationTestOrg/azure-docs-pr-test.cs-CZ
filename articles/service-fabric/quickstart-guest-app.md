---
title: "Rychlé nasazení existující aplikace do clusteru Azure Service Fabric"
description: "Použijte cluster Azure Service Fabric k hostování existující aplikace Node.js pomocí sady Visual Studio."
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
ms.openlocfilehash: 3601b73872bbea4b4e5324382eb97b7384ca6e13
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="host-a-nodejs-application-on-azure-service-fabric"></a><span data-ttu-id="a6afd-103">Hostování aplikace Node.js na platformě Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a6afd-103">Host a Node.js application on Azure Service Fabric</span></span>

<span data-ttu-id="a6afd-104">Tento rychlý start vám pomůže s nasazením existující aplikace (v tomto příkladu Node.js) do clusteru Service Fabric spuštěného v Azure.</span><span class="sxs-lookup"><span data-stu-id="a6afd-104">This quickstart helps you deploy an existing application (Node.js in this example) to a Service Fabric cluster running on Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6afd-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a6afd-105">Prerequisites</span></span>

<span data-ttu-id="a6afd-106">Než začnete, ujistěte se, že máte [nastavené vývojové prostředí](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a6afd-106">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="a6afd-107">To zahrnuje instalaci sady Service Fabric SDK a sady Visual Studio 2017 nebo 2015.</span><span class="sxs-lookup"><span data-stu-id="a6afd-107">Which includes installing the Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

<span data-ttu-id="a6afd-108">Také musíte mít existující aplikaci Node.js k nasazení.</span><span class="sxs-lookup"><span data-stu-id="a6afd-108">You also need to have an existing Node.js application for deployment.</span></span> <span data-ttu-id="a6afd-109">V tomto rychlém startu se používá jednoduchý web v Node.js, který je ke stažení [zde][download-sample].</span><span class="sxs-lookup"><span data-stu-id="a6afd-109">This quickstart uses a simple Node.js website that can be downloaded [here][download-sample].</span></span> <span data-ttu-id="a6afd-110">Po vytvoření projektu v dalším kroku extrahujte tento soubor do složky `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\`.</span><span class="sxs-lookup"><span data-stu-id="a6afd-110">Extract this file to your `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` folder after you create the project in the next step.</span></span>

<span data-ttu-id="a6afd-111">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet][create-account].</span><span class="sxs-lookup"><span data-stu-id="a6afd-111">If you don't have an Azure subscription, create a [free account][create-account].</span></span>

## <a name="create-the-service"></a><span data-ttu-id="a6afd-112">Vytvoření služby</span><span class="sxs-lookup"><span data-stu-id="a6afd-112">Create the service</span></span>

<span data-ttu-id="a6afd-113">Spusťte sadu Visual Studio jako **správce**.</span><span class="sxs-lookup"><span data-stu-id="a6afd-113">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="a6afd-114">Vytvořte projekt pomocí klávesové zkratky `CTRL`+`SHIFT`+`N`.</span><span class="sxs-lookup"><span data-stu-id="a6afd-114">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="a6afd-115">V dialogovém okně **Nový projekt** zvolte **Cloud > Aplikace Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="a6afd-115">In the **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="a6afd-116">Pojmenujte aplikaci **MyGuestApp** a stiskněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6afd-116">Name the application **MyGuestApp** and press **OK**.</span></span>

>[!IMPORTANT]
><span data-ttu-id="a6afd-117">Node.js může snadno překročit omezení 260 znaků pro cesty v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a6afd-117">Node.js can easily break the 260 character limit for paths that windows has.</span></span> <span data-ttu-id="a6afd-118">Pro samotný projekt použijte krátkou cestu, například **c:\code\svc1**.</span><span class="sxs-lookup"><span data-stu-id="a6afd-118">Use a short path for the project itself such as **c:\code\svc1**.</span></span>
   
![Dialogové okno Nový projekt ve Visual Studiu][new-project]

<span data-ttu-id="a6afd-120">V dalším dialogovém okně můžete vytvořit jakýkoli typ služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a6afd-120">You can create any type of Service Fabric service from the next dialog.</span></span> <span data-ttu-id="a6afd-121">Pro účely tohoto Rychlého startu zvolte **Spustitelný soubor typu Host**.</span><span class="sxs-lookup"><span data-stu-id="a6afd-121">For this quickstart, choose **Guest Executable**.</span></span>

<span data-ttu-id="a6afd-122">Pojmenujte službu **MyGuestService** a nastavte možnosti na pravé straně na následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a6afd-122">Name the service **MyGuestService** and set the options on the right to the following values:</span></span>

| <span data-ttu-id="a6afd-123">Nastavení</span><span class="sxs-lookup"><span data-stu-id="a6afd-123">Setting</span></span>                   | <span data-ttu-id="a6afd-124">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a6afd-124">Value</span></span> |
| ------------------------- | ------ |
| <span data-ttu-id="a6afd-125">Složka balíčku kódu</span><span class="sxs-lookup"><span data-stu-id="a6afd-125">Code Package Folder</span></span>       | <span data-ttu-id="a6afd-126">_&lt;složka s vaší aplikací Node.js&gt;_</span><span class="sxs-lookup"><span data-stu-id="a6afd-126">_&lt;the folder with your Node.js app&gt;_</span></span> |
| <span data-ttu-id="a6afd-127">Chování balíčku kódu</span><span class="sxs-lookup"><span data-stu-id="a6afd-127">Code Package Behavior</span></span>     | <span data-ttu-id="a6afd-128">Zkopírujte obsah složky do projektu</span><span class="sxs-lookup"><span data-stu-id="a6afd-128">Copy folder contents to project</span></span> |
| <span data-ttu-id="a6afd-129">Program</span><span class="sxs-lookup"><span data-stu-id="a6afd-129">Program</span></span>                   | <span data-ttu-id="a6afd-130">node.exe</span><span class="sxs-lookup"><span data-stu-id="a6afd-130">node.exe</span></span> |
| <span data-ttu-id="a6afd-131">Argumenty</span><span class="sxs-lookup"><span data-stu-id="a6afd-131">Arguments</span></span>                 | <span data-ttu-id="a6afd-132">server.js</span><span class="sxs-lookup"><span data-stu-id="a6afd-132">server.js</span></span> |
| <span data-ttu-id="a6afd-133">Pracovní složka</span><span class="sxs-lookup"><span data-stu-id="a6afd-133">Working Folder</span></span>            | <span data-ttu-id="a6afd-134">CodePackage</span><span class="sxs-lookup"><span data-stu-id="a6afd-134">CodePackage</span></span> |

<span data-ttu-id="a6afd-135">Stiskněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6afd-135">Press **OK**.</span></span>

![Dialogové okno Nová služba ve Visual Studiu][new-service]

<span data-ttu-id="a6afd-137">Sada Visual Studio vytvoří projekt aplikace a projekt služby objektu actor a zobrazí je v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="a6afd-137">Visual Studio creates the application project and the actor service project and displays them in Solution Explorer.</span></span>

<span data-ttu-id="a6afd-138">Projekt aplikace (**MyGuestApp**) jako takový neobsahuje žádný kód.</span><span class="sxs-lookup"><span data-stu-id="a6afd-138">The application project (**MyGuestApp**) does not contain any code directly.</span></span> <span data-ttu-id="a6afd-139">Odkazuje ale na sadu projektů služeb.</span><span class="sxs-lookup"><span data-stu-id="a6afd-139">Instead, it references a set of service projects.</span></span> <span data-ttu-id="a6afd-140">Kromě toho obsahuje další tři typy obsahu:</span><span class="sxs-lookup"><span data-stu-id="a6afd-140">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="a6afd-141">**Profily publikování**</span><span class="sxs-lookup"><span data-stu-id="a6afd-141">**Publish profiles**</span></span>  
<span data-ttu-id="a6afd-142">Předvolby nástrojů pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="a6afd-142">Tooling preferences for different environments.</span></span>

* <span data-ttu-id="a6afd-143">**Skripty**</span><span class="sxs-lookup"><span data-stu-id="a6afd-143">**Scripts**</span></span>  
<span data-ttu-id="a6afd-144">Skript PowerShellu pro nasazení/upgrade aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6afd-144">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="a6afd-145">**Definice aplikace**</span><span class="sxs-lookup"><span data-stu-id="a6afd-145">**Application definition**</span></span>  
<span data-ttu-id="a6afd-146">Obsahuje manifest aplikace ve složce *ApplicationPackageRoot*.</span><span class="sxs-lookup"><span data-stu-id="a6afd-146">Includes the application manifest under *ApplicationPackageRoot*.</span></span> <span data-ttu-id="a6afd-147">Přidružené soubory parametrů aplikace, které se nachází ve složce *ApplicationParameters*, definují aplikaci a umožňují vám ji nakonfigurovat speciálně pro určité prostředí.</span><span class="sxs-lookup"><span data-stu-id="a6afd-147">Associated application parameter files are under *ApplicationParameters*, which define the application and allow you to configure it specifically for a given environment.</span></span>
    
<span data-ttu-id="a6afd-148">Přehled obsahu projektu služby najdete v tématu [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="a6afd-148">For an overview of the contents of the service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="set-up-networking"></a><span data-ttu-id="a6afd-149">Nastavení síťových služeb</span><span class="sxs-lookup"><span data-stu-id="a6afd-149">Set up networking</span></span>

<span data-ttu-id="a6afd-150">Příklad aplikace Node.js, který nasazujeme, používá port **80** a platformě Service Fabric potřebujeme říct, že tento port potřebujeme zpřístupnit.</span><span class="sxs-lookup"><span data-stu-id="a6afd-150">The example Node.js app we're deploying uses port **80** and we need to tell Service Fabric that we need that port exposed.</span></span>

<span data-ttu-id="a6afd-151">Otevřete v projektu soubor **ServiceManifest.xml**.</span><span class="sxs-lookup"><span data-stu-id="a6afd-151">Open the **ServiceManifest.xml** file in the project.</span></span> <span data-ttu-id="a6afd-152">V dolní části manifestu je část `<Resources> \ <Endpoints>` s již definovanou položkou.</span><span class="sxs-lookup"><span data-stu-id="a6afd-152">At the bottom of the manifest, there is a `<Resources> \ <Endpoints>` with an entry already defined.</span></span> <span data-ttu-id="a6afd-153">Upravte tuto položku a přidejte `Port`, `Protocol` a `Type`.</span><span class="sxs-lookup"><span data-stu-id="a6afd-153">Modify that entry to add `Port`, `Protocol`, and `Type`.</span></span> 

```xml
  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyGuestAppServiceTypeEndpoint" Port="80" Protocol="http" Type="Input" />
    </Endpoints>
  </Resources>
```

## <a name="deploy-to-azure"></a><span data-ttu-id="a6afd-154">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="a6afd-154">Deploy to Azure</span></span>

<span data-ttu-id="a6afd-155">Pokud stisknete klávesu **F5** a spustíte projekt, nasadí se do místního clusteru.</span><span class="sxs-lookup"><span data-stu-id="a6afd-155">If you press **F5** and run the project, it is deployed to the local cluster.</span></span> <span data-ttu-id="a6afd-156">My jej ale místo toho nasadíme do Azure.</span><span class="sxs-lookup"><span data-stu-id="a6afd-156">However, let's deploy to Azure instead.</span></span>

<span data-ttu-id="a6afd-157">Klikněte na projekt pravým tlačítkem a zvolte **Publikovat...**, tím se otevře dialogové okno pro publikování do Azure.</span><span class="sxs-lookup"><span data-stu-id="a6afd-157">Right-click on the project and choose **Publish...** which opens a dialog to publish to Azure.</span></span>

![Dialogové okno pro publikování služby Service Fabric do Azure][publish]

<span data-ttu-id="a6afd-159">Vyberte cílový profil **PublishProfiles\Cloud.xml**.</span><span class="sxs-lookup"><span data-stu-id="a6afd-159">Select the **PublishProfiles\Cloud.xml** target profile.</span></span>

<span data-ttu-id="a6afd-160">Pokud jste to neudělali dříve, zvolte účet Azure, do kterého se má nasazení provést.</span><span class="sxs-lookup"><span data-stu-id="a6afd-160">If you haven't previously, choose an Azure account to deploy to.</span></span> <span data-ttu-id="a6afd-161">Pokud ještě žádný nemáte, [zaregistrujte si bezplatný účet][create-account].</span><span class="sxs-lookup"><span data-stu-id="a6afd-161">If you don't have one yet, [sign-up for one][create-account].</span></span>

<span data-ttu-id="a6afd-162">V části **Koncový bod připojení** vyberte cluster Service Fabric, do kterého se má nasazení provést.</span><span class="sxs-lookup"><span data-stu-id="a6afd-162">Under **Connection Endpoint**, select the Service Fabric cluster to deploy to.</span></span> <span data-ttu-id="a6afd-163">Pokud žádný nemáte, vyberte **&lt;Vytvořit nový cluster...&gt;**, tím se otevře okno webového prohlížeče s webem Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="a6afd-163">If you do not have one, select **&lt;Create New Cluster...&gt;** which opens up web browser window to the Azure portal.</span></span> <span data-ttu-id="a6afd-164">Další informace najdete v tématu popisujícím [vytvoření clusteru na portálu](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="a6afd-164">For more information, see [create a cluster in the portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal).</span></span> 

<span data-ttu-id="a6afd-165">Při vytváření clusteru Service Fabric nezapomeňte nastavit nastavení **Vlastní koncové body** na hodnotu **80**.</span><span class="sxs-lookup"><span data-stu-id="a6afd-165">When you create the Service Fabric cluster, make sure to set the **Custom endpoints** setting to **80**.</span></span>

![Konfigurace typu uzlu Service Fabric s vlastním koncovým bodem][custom-endpoint]

<span data-ttu-id="a6afd-167">Dokončení vytvoření nového clusteru Service Fabric nějakou dobu trvá.</span><span class="sxs-lookup"><span data-stu-id="a6afd-167">Creating a new Service Fabric cluster takes some time to complete.</span></span> <span data-ttu-id="a6afd-168">Jakmile bude vytvořený, vraťte se do dialogového okna pro publikování a vyberte **&lt;Aktualizovat&gt;**.</span><span class="sxs-lookup"><span data-stu-id="a6afd-168">Once it has been created, go back to the publish dialog and select **&lt;Refresh&gt;**.</span></span> <span data-ttu-id="a6afd-169">Nový cluster bude uveden v rozevíracím seznamu, vyberte ho.</span><span class="sxs-lookup"><span data-stu-id="a6afd-169">The new cluster is listed in the drop-down box; select it.</span></span>

<span data-ttu-id="a6afd-170">Stiskněte **Publikovat** a počkejte na dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="a6afd-170">Press **Publish** and wait for the deployment to finish.</span></span>

<span data-ttu-id="a6afd-171">Může to trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="a6afd-171">This may take a few minutes.</span></span> <span data-ttu-id="a6afd-172">Po dokončení nasazení může ještě několik minut trvat, než bude aplikace plně dostupná.</span><span class="sxs-lookup"><span data-stu-id="a6afd-172">After it completes, it may take a few more minutes for the application to be fully available.</span></span>

## <a name="test-the-website"></a><span data-ttu-id="a6afd-173">Testování webu</span><span class="sxs-lookup"><span data-stu-id="a6afd-173">Test the website</span></span>

<span data-ttu-id="a6afd-174">Jakmile bude vaše služba publikována, otestujte ji ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a6afd-174">After your service has been published, test it in a web browser.</span></span> 

<span data-ttu-id="a6afd-175">Nejprve otevřete web Azure Portal a vyhledejte vaši službu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a6afd-175">First, open the Azure portal and find your Service Fabric service.</span></span>

<span data-ttu-id="a6afd-176">Zkontrolujte okno přehledu adresy služby.</span><span class="sxs-lookup"><span data-stu-id="a6afd-176">Check the overview blade of the service address.</span></span> <span data-ttu-id="a6afd-177">Použijte název domény z vlastnosti _Koncový bod pro připojení klienta_.</span><span class="sxs-lookup"><span data-stu-id="a6afd-177">Use the domain name from the _Client connection endpoint_ property.</span></span> <span data-ttu-id="a6afd-178">Například, `http://mysvcfab1.westus2.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="a6afd-178">For example, `http://mysvcfab1.westus2.cloudapp.azure.com`.</span></span>

![Okno přehledu Service Fabric na webu Azure Portal][overview]

<span data-ttu-id="a6afd-180">Přejděte na tuto adresu, kde se zobrazí odezva `HELLO WORLD`.</span><span class="sxs-lookup"><span data-stu-id="a6afd-180">Navigate to this address where you will see the `HELLO WORLD` response.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="a6afd-181">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="a6afd-181">Delete the cluster</span></span>

<span data-ttu-id="a6afd-182">Nezapomeňte odstranit všechny prostředky, které jste vytvořili pro účely tohoto rychlého startu – tyto prostředky se vám účtují.</span><span class="sxs-lookup"><span data-stu-id="a6afd-182">Do not forget to delete all of the resources you've created for this quickstart, as you are charged for those resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6afd-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6afd-183">Next steps</span></span>
<span data-ttu-id="a6afd-184">Další informace o [spustitelných souborech typu Host](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="a6afd-184">Read more about [guest executables](service-fabric-deploy-existing-app.md).</span></span>

<!-- Image References -->

[new-project]: ./media/quickstart-guest-app/new-project.png
[new-service]: ./media/quickstart-guest-app/template.png
[solution-exp]: ./media/quickstart-guest-app/solution-explorer.png
[publish]: ./media/quickstart-guest-app/publish.png
[overview]: ./media/quickstart-guest-app/overview.png
[custom-endpoint]: ./media/quickstart-guest-app/custom-endpoint.png

[download-sample]: https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/service-fabric-node-website.zip
[create-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F