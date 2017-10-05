---
title: Modul plug-in Azure Service Fabric pro Eclipse | Microsoft Docs
description: "Začínáme s modulem plug-in Service Fabric pro Eclipse."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2016
ms.author: saysa
ms.openlocfilehash: 98c1b99972b9ad7a396d72b98e727286f6822e42
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a><span data-ttu-id="d3373-103">Modul plug-in Service Fabric pro vývoj aplikací v Eclipse Javě</span><span class="sxs-lookup"><span data-stu-id="d3373-103">Service Fabric plug-in for Eclipse Java application development</span></span>
<span data-ttu-id="d3373-104">Eclipse je jedním z nejčastěji používaných integrovaných vývojových prostředí (IDE) pro vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="d3373-104">Eclipse is one of the most widely used integrated development environments (IDEs) for Java developers.</span></span> <span data-ttu-id="d3373-105">V tomto článku probereme možnosti nastavení vývojového prostředí Eclipse pro práci s Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d3373-105">In this article, we describe how to set up your Eclipse development environment to work with Azure Service Fabric.</span></span> <span data-ttu-id="d3373-106">Naučíte se nainstalovat modul plug-in Service Fabric, vytvořit aplikaci Service Fabric a nasadit ji na místní nebo vzdálený cluster Service Fabric v Eclipse Neonu.</span><span class="sxs-lookup"><span data-stu-id="d3373-106">Learn how to install the Service Fabric plug-in, create a Service Fabric application, and deploy your Service Fabric application to a local or remote Service Fabric cluster in Eclipse Neon.</span></span>

## <a name="install-or-update-the-service-fabric-plug-in-in-eclipse-neon"></a><span data-ttu-id="d3373-107">Instalace a aktualizace modulu plug-in Service Fabric v prostředí Eclipse Neon</span><span class="sxs-lookup"><span data-stu-id="d3373-107">Install or update the Service Fabric plug-in in Eclipse Neon</span></span>
<span data-ttu-id="d3373-108">Modul plug-in Service Fabric můžete nainstalovat do Eclipse.</span><span class="sxs-lookup"><span data-stu-id="d3373-108">You can install a Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="d3373-109">Tento modul plug-in může zjednodušit proces vytváření a nasazování služeb v Javě.</span><span class="sxs-lookup"><span data-stu-id="d3373-109">The plug-in can help simplify the process of building and deploying Java services.</span></span>

1.  <span data-ttu-id="d3373-110">Zkontrolujte, že máte nainstalovanou nejnovější verzi Eclipse Neonu a nejnovější verzi Buildshipu (1.0.17 nebo novější):</span><span class="sxs-lookup"><span data-stu-id="d3373-110">Ensure that you have the latest version of Eclipse Neon and the latest version of Buildship (1.0.17 or a later version) installed:</span></span>
    -   <span data-ttu-id="d3373-111">Verze nainstalovaných komponent můžete v Eclipse Neonu zkontrolovat tak, že zvolíte **Help** > **Installation Details** (Nápověda => Podrobnosti o instalaci).</span><span class="sxs-lookup"><span data-stu-id="d3373-111">To check the versions of installed components, in Eclipse Neon, go to **Help** > **Installation Details**.</span></span>
    -   <span data-ttu-id="d3373-112">Pokud chcete aktualizovat Buildship, přečtěte si téma [Eclipse Buildship: Moduly plug-in Eclipse pro Gradle][buildship-update].</span><span class="sxs-lookup"><span data-stu-id="d3373-112">To update Buildship, see [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].</span></span>
    -   <span data-ttu-id="d3373-113">Pokud chcete zkontrolovat a nainstalovat aktualizace pro Eclipse Neon, přejděte k části **Help** > **Check for Updates** (Nápověda => Vyhledat aktualizace).</span><span class="sxs-lookup"><span data-stu-id="d3373-113">To check for and install updates for Eclipse Neon, go to **Help** > **Check for Updates**.</span></span>

2.  <span data-ttu-id="d3373-114">Pokud chcete nainstalovat modul plug-in Service Fabric v Eclipse Neonu, přejděte k části **Help** > **Install New Software** (Nápověda => Instalovat nový software).</span><span class="sxs-lookup"><span data-stu-id="d3373-114">To install the Service Fabric plug-in, in Eclipse Neon, go to **Help** > **Install New Software**.</span></span>
  1.    <span data-ttu-id="d3373-115">V poli **Work with** (Pracovat s) zadejte **http://dl.microsoft.com/eclipse**.</span><span class="sxs-lookup"><span data-stu-id="d3373-115">In the **Work with** box, enter **http://dl.microsoft.com/eclipse**.</span></span>
  2.    <span data-ttu-id="d3373-116">Klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="d3373-116">Click **Add**.</span></span>

         ![Modul plug-in Service Fabric pro Eclipse Neon][sf-eclipse-plugin-install]
  3.    <span data-ttu-id="d3373-118">Vyberte modul plug-in Service Fabric a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3373-118">Select the Service Fabric plug-in, and then click **Next**.</span></span>
  4.    <span data-ttu-id="d3373-119">Dokončete instalaci a přijměte licenční podmínky pro software společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d3373-119">Complete the installation steps, and then accept the Microsoft Software License Terms.</span></span>

<span data-ttu-id="d3373-120">Pokud už máte modul plug-in Service Fabric nainstalovaný, ověřte, že používáte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="d3373-120">If you already have the Service Fabric plug-in installed, make sure that you have the latest version.</span></span> <span data-ttu-id="d3373-121">Pokud chcete vyhledat dostupné aktualizace, přejděte k části **Help** > **Installation Details** (Nápověda => Podrobnosti o instalaci).</span><span class="sxs-lookup"><span data-stu-id="d3373-121">To check for available updates, go to **Help** > **Installation Details**.</span></span> <span data-ttu-id="d3373-122">V seznamu nainstalovaných modulů plug-in vyberte Service Fabric a potom klikněte na **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="d3373-122">In the list of installed plug-ins, select Service Fabric, and then click **Update**.</span></span> <span data-ttu-id="d3373-123">Nainstalují se dostupné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="d3373-123">Available updates will be installed.</span></span>

> [!NOTE]
> <span data-ttu-id="d3373-124">Pokud je instalace nebo aktualizace modulu plug-in Service Fabric pomalá, může být důvodem nastavení Eclipse.</span><span class="sxs-lookup"><span data-stu-id="d3373-124">If installing or updating the Service Fabric plug-in is slow, it might be due to an Eclipse setting.</span></span> <span data-ttu-id="d3373-125">Eclipse shromažďuje metadata o všech změnách, aby aktualizoval weby, které jsou registrované pro vaši instanci Eclipse.</span><span class="sxs-lookup"><span data-stu-id="d3373-125">Eclipse collects metadata on all changes to update sites that are registered with your Eclipse instance.</span></span> <span data-ttu-id="d3373-126">Pokud chcete proces vyhledávání a instalace aktualizace modulu plug-in Service Fabric urychlit, přejděte k části **Available Software Sites** (Dostupné softwarové servery).</span><span class="sxs-lookup"><span data-stu-id="d3373-126">To speed up the process of checking for and installing a Service Fabric plug-in update, go to **Available Software Sites**.</span></span> <span data-ttu-id="d3373-127">Zrušte zaškrtnutí políček pro všechny weby kromě políčka odkazujícího na umístění modulu plug-in Service Fabric (http://dl.microsoft.com/eclipse/azure/servicefabric).</span><span class="sxs-lookup"><span data-stu-id="d3373-127">Clear the check boxes for all sites except for the one that points to the Service Fabric plug-in location (http://dl.microsoft.com/eclipse/azure/servicefabric).</span></span>

## <a name="create-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="d3373-128">Vytvoření aplikace Service Fabric pomocí Eclipse</span><span class="sxs-lookup"><span data-stu-id="d3373-128">Create a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="d3373-129">V Eclipse Neonu přejděte na **File** > **New** > **Other** (Soubor => Nový => Ostatní).</span><span class="sxs-lookup"><span data-stu-id="d3373-129">In Eclipse Neon, go to **File** > **New** > **Other**.</span></span> <span data-ttu-id="d3373-130">Vyberte **Service Fabric Project** (Projekt Service Fabric) a potom klikněte na **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="d3373-130">Select  **Service Fabric Project**, and then click **Next**.</span></span>

    ![Nový projekt Service Fabric – stránka 1][create-application/p1]

2.  <span data-ttu-id="d3373-132">Zadejte název pro svůj projekt a potom klikněte na **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="d3373-132">Enter a name for your project, and then click **Next**.</span></span>

    ![Nový projekt Service Fabric – stránka 2][create-application/p2]

3.  <span data-ttu-id="d3373-134">V seznamu šablon vyberte **Service Template** (Šablona služby).</span><span class="sxs-lookup"><span data-stu-id="d3373-134">In the list of templates, select **Service Template**.</span></span> <span data-ttu-id="d3373-135">Vyberte typ šablony služby (Actor, Stateless, Container nebo Guest Binary) a potom klikněte na **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="d3373-135">Select your service template type (Actor, Stateless, Container, or Guest Binary), and then click **Next**.</span></span>

    ![Nový projekt Service Fabric – stránka 3][create-application/p3]

4.  <span data-ttu-id="d3373-137">Zadejte název a podrobnosti služby a potom klikněte na **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="d3373-137">Enter the service name and service details, and then click **Finish**.</span></span>

    ![Nový projekt Service Fabric – stránka 4][create-application/p4]

5. <span data-ttu-id="d3373-139">Když vytvoříte svůj první projekt Service Fabric, v dialogovém okně **Open Associated Perspective** Otevřít přidruženou perspektivu) klikněte na **Yes** (Ano).</span><span class="sxs-lookup"><span data-stu-id="d3373-139">When you create your first Service Fabric project, in the **Open Associated Perspective** dialog box, click **Yes**.</span></span>

    ![Nový projekt Service Fabric – stránka 5][create-application/p5]

6.  <span data-ttu-id="d3373-141">Nový projekt vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d3373-141">Your new project looks like this:</span></span>

    ![Nový projekt Service Fabric – stránka 6][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="d3373-143">Vytvoření a nasazení aplikace Service Fabric v Eclipse</span><span class="sxs-lookup"><span data-stu-id="d3373-143">Build and deploy a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="d3373-144">Klikněte na novou aplikaci Service Fabric pravým tlačítkem a potom vyberte **Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="d3373-144">Right-click your new Service Fabric application, and then select **Service Fabric**.</span></span>

    ![Nabídka Service Fabric zobrazená po kliknutí pravým tlačítkem][publish/RightClick]

2. <span data-ttu-id="d3373-146">V podnabídce vyberte požadovanou možnost:</span><span class="sxs-lookup"><span data-stu-id="d3373-146">In the submenu, select the option you want:</span></span>
    -   <span data-ttu-id="d3373-147">**Build Application** (Sestavit aplikaci) sestaví aplikaci bez vyčištění.</span><span class="sxs-lookup"><span data-stu-id="d3373-147">To build the application without cleaning, click **Build Application**.</span></span>
    -   <span data-ttu-id="d3373-148">**Rebuild Application** (Znovu sestavit aplikaci) provede vyčištění a nové sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3373-148">To do a clean build of the application, click **Rebuild Application**.</span></span>
    -   <span data-ttu-id="d3373-149">**Clean Application** (Vyčistit aplikaci) vyčistí aplikaci od artefaktů sestavení.</span><span class="sxs-lookup"><span data-stu-id="d3373-149">To clean the application of built artifacts, click **Clean Application**.</span></span>

3.  <span data-ttu-id="d3373-150">V této nabídce můžete zvolit také nasazení, zrušení nasazení nebo publikování aplikace:</span><span class="sxs-lookup"><span data-stu-id="d3373-150">From this menu, you also can deploy, undeploy, and publish your application:</span></span>
    -   <span data-ttu-id="d3373-151">**Deploy Application** (Nasadit aplikaci) nasadí aplikaci na místní cluster.</span><span class="sxs-lookup"><span data-stu-id="d3373-151">To deploy to your local cluster, click **Deploy Application**.</span></span>
    -   <span data-ttu-id="d3373-152">V dialogovém okně **Publish Application** (Publikovat aplikaci) vyberte profil publikování:</span><span class="sxs-lookup"><span data-stu-id="d3373-152">In the **Publish Application** dialog box, select a publish profile:</span></span>
        -  <span data-ttu-id="d3373-153">**Local.json**</span><span class="sxs-lookup"><span data-stu-id="d3373-153">**Local.json**</span></span>
        -  <span data-ttu-id="d3373-154">**Cloud.json**</span><span class="sxs-lookup"><span data-stu-id="d3373-154">**Cloud.json**</span></span>

     <span data-ttu-id="d3373-155">Tyto soubory JSON (JavaScript Object Notation) se používají k uložení informací (jako jsou koncové body připojení a informace o zabezpečení) potřebných pro připojení k místnímu nebo cloudovému (Azure) clusteru.</span><span class="sxs-lookup"><span data-stu-id="d3373-155">These JavaScript Object Notation (JSON) files store information (such as connection endpoints and security information) that is required to connect to your local or cloud (Azure) cluster.</span></span>

  ![Nabídka Publish (Publikovat) v Service Fabric][publish/Publish]

<span data-ttu-id="d3373-157">Existuje alternativní způsob nasazení aplikace Service Fabric pomocí konfigurací spuštění Eclipse.</span><span class="sxs-lookup"><span data-stu-id="d3373-157">An alternate way to deploy your Service Fabric application is by using Eclipse run configurations.</span></span>

  1.    <span data-ttu-id="d3373-158">Přejděte k části **Run** > **Run Configurations** (Spustit > Konfigurace spuštění).</span><span class="sxs-lookup"><span data-stu-id="d3373-158">Go to **Run** > **Run Configurations**.</span></span>
  2.    <span data-ttu-id="d3373-159">Vyberte konfiguraci spuštění **ServiceFabricDeployer** v části **Gradle Project** (Projekt Gradle).</span><span class="sxs-lookup"><span data-stu-id="d3373-159">Under **Gradle Project**, select the **ServiceFabricDeployer** run configuration.</span></span>
  3.    <span data-ttu-id="d3373-160">Na kartě **Arguments** (Argumenty) v pravém podokně jako **publishProfile** (Profil publikování) zadejte **local** (místní) nebo **cloud** (cloudový).</span><span class="sxs-lookup"><span data-stu-id="d3373-160">In the right pane, on the **Arguments** tab, for **publishProfile**, select **local** or **cloud**.</span></span>  <span data-ttu-id="d3373-161">Výchozí hodnota je **local** (místní).</span><span class="sxs-lookup"><span data-stu-id="d3373-161">The default is **local**.</span></span> <span data-ttu-id="d3373-162">Pro nasazení na vzdálený nebo cloudový cluster vyberte **cloud** (cloudový).</span><span class="sxs-lookup"><span data-stu-id="d3373-162">To deploy to a remote or cloud cluster, select **cloud**.</span></span>
  4.    <span data-ttu-id="d3373-163">Aby se zajistilo, že v profilech publikování jsou zadané správné informace, upravte **Local.json** nebo **Cloud.json** podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="d3373-163">To ensure that the proper information is populated in the publish profiles, edit **Local.json** or **Cloud.json** as needed.</span></span> <span data-ttu-id="d3373-164">Můžete přidat nebo aktualizovat podrobnosti o koncových bodech a zabezpečovací přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="d3373-164">You can add or update endpoint details and security credentials.</span></span>
  5.    <span data-ttu-id="d3373-165">Ujistěte se, že **Working Directory** (Pracovní adresář) odkazuje na aplikaci, kterou chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="d3373-165">Ensure that **Working Directory** points to the application you want to deploy.</span></span> <span data-ttu-id="d3373-166">Pokud chcete aplikaci změnit, stačí kliknout na **Workspace** (Pracovní prostor) a vybrat požadovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3373-166">To change the application, click the **Workspace** button, and then select the application you want.</span></span>
  6.    <span data-ttu-id="d3373-167">Klikněte na **Apply** (Použít) a potom na **Run** (Spustit).</span><span class="sxs-lookup"><span data-stu-id="d3373-167">Click **Apply**, and then click **Run**.</span></span>

<span data-ttu-id="d3373-168">Vaše aplikace se během chvilky sestaví a nasadí.</span><span class="sxs-lookup"><span data-stu-id="d3373-168">Your application builds and deploys within a few moments.</span></span> <span data-ttu-id="d3373-169">Stav nasazení můžete sledovat v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="d3373-169">You can monitor the deployment status in Service Fabric Explorer.</span></span>  

## <a name="add-a-service-fabric-service-to-your-service-fabric-application"></a><span data-ttu-id="d3373-170">Přidání služby Service Fabric do aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="d3373-170">Add a Service Fabric service to your Service Fabric application</span></span>

<span data-ttu-id="d3373-171">Přidat službu Service Fabric do stávající aplikace Service Fabric je možné pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d3373-171">To add a Service Fabric service to an existing Service Fabric application, do the following steps:</span></span>

1.  <span data-ttu-id="d3373-172">Klikněte pravým tlačítkem na projekt, ke kterému chcete přidat službu, a potom klikněte na **Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="d3373-172">Right-click the project you want to add a service to, and then click **Service Fabric**.</span></span>

    ![Přidat službu Service Fabric – stránka 1][add-service/p1]

2.  <span data-ttu-id="d3373-174">Klikněte na **Add Service Fabric Service** (Přidat službu Service Fabric) a dokončete kroky pro přidání služby k tomuto projektu.</span><span class="sxs-lookup"><span data-stu-id="d3373-174">Click **Add Service Fabric Service**, and complete the set of steps to add a service to the project.</span></span>
3.  <span data-ttu-id="d3373-175">Vyberte šablonu služby, kterou chcete přidat do projektu, a potom klikněte na **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="d3373-175">Select the service template you want to add to your project, and then click **Next**.</span></span>

    ![Přidat službu Service Fabric – stránka 2][add-service/p2]

4.  <span data-ttu-id="d3373-177">Zadejte název služby (a další podrobnosti podle potřeby) a potom klikněte na **Add Service** (Přidat službu).</span><span class="sxs-lookup"><span data-stu-id="d3373-177">Enter the service name (and other details, as needed), and then click the **Add Service** button.</span></span>  

    ![Přidat službu Service Fabric – stránka 3][add-service/p3]

5.  <span data-ttu-id="d3373-179">Celková struktura projektu po přidání služby vypadá podobně jako u tohoto projektu:</span><span class="sxs-lookup"><span data-stu-id="d3373-179">After the service is added, your overall project structure looks similar to the following project:</span></span>

    ![Přidat službu Service Fabric – stránka 4][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a><span data-ttu-id="d3373-181">Úprava verzí manifestu aplikace Service Fabric v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="d3373-181">Edit Manifest versions of your Service Fabric Java application</span></span>

<span data-ttu-id="d3373-182">Pokud chcete upravit verze manifestu, klikněte pravým tlačítkem na projekt, přejděte na **Service Fabric** a z rozevírací nabídky vyberte **Upravit verze manifestu...**.</span><span class="sxs-lookup"><span data-stu-id="d3373-182">To edit manifest versions, right click on the project, go to **Service Fabric** and select **Edit Manifest Versions...** from the menu dropdown.</span></span> <span data-ttu-id="d3373-183">V průvodci můžete aktualizovat verze manifestu pro manifest aplikace, manifest služby a verze pro balíčky **Code**, **Config** a **Data**.</span><span class="sxs-lookup"><span data-stu-id="d3373-183">In the wizard, you can update the manifest versions for application manifest, service manifest and the versions for **Code**, **Config** and **Data** packages.</span></span>

<span data-ttu-id="d3373-184">Pokud zaškrtnete políčko **Automaticky aktualizovat verze aplikací a služeb** a pak aktualizujete verzi, verze manifestu se zaktualizují automaticky.</span><span class="sxs-lookup"><span data-stu-id="d3373-184">If you check the option **Automatically update application and service versions** and then update a version, then the manifest versions will be automatically updated.</span></span> <span data-ttu-id="d3373-185">Když například nejprve zaškrtnete toto políčko a pak zaktualizujete verzi pro verzi **Code** z 0.0.0 na 0.0.1 a klikněte na **Dokončit**, verze manifestu služby a verze manifestu aplikace se automaticky aktualizují na 0.0.1.</span><span class="sxs-lookup"><span data-stu-id="d3373-185">To give an example, you first select the check-box, then update the version of **Code** version from 0.0.0 to 0.0.1 and click on **Finish**, then service manifest version and application manifest version will be automatically updated to 0.0.1.</span></span>

## <a name="upgrade-your-service-fabric-java-application"></a><span data-ttu-id="d3373-186">Upgrade aplikace Service Fabric v Javě</span><span class="sxs-lookup"><span data-stu-id="d3373-186">Upgrade your Service Fabric Java application</span></span>

<span data-ttu-id="d3373-187">Pro scénář upgradu předpokládejme, že jste pomocí modulu plug-in Service Fabric v Eclipse vytvořili projekt **App1**.</span><span class="sxs-lookup"><span data-stu-id="d3373-187">For an upgrade scenario, say you created the **App1** project by using the Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="d3373-188">Pomocí modulu plug-in jste ho nasadili a vytvořili aplikaci s názvem **fabric: / App1Application**.</span><span class="sxs-lookup"><span data-stu-id="d3373-188">You deployed it by using the plug-in to create an application named **fabric:/App1Application**.</span></span> <span data-ttu-id="d3373-189">Tato aplikace je typu **App1AppicationType** a verze této aplikace je 1.0.</span><span class="sxs-lookup"><span data-stu-id="d3373-189">The application type is **App1AppicationType**, and the application version is 1.0.</span></span> <span data-ttu-id="d3373-190">Nyní chcete provést upgrade této aplikace bez přerušení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="d3373-190">Now, you want to upgrade your application without interrupting availability.</span></span>

<span data-ttu-id="d3373-191">Nejdřív proveďte úpravy aplikace a znovu sestavte upravenou službu.</span><span class="sxs-lookup"><span data-stu-id="d3373-191">First, make any changes to your application, and then rebuild the modified service.</span></span> <span data-ttu-id="d3373-192">Aktualizujte soubor manifestu upravené služby (ServiceManifest.xml) s použitím aktualizovaných verzí pro službu (a podle potřeby i verzí kódu, konfigurace nebo dat).</span><span class="sxs-lookup"><span data-stu-id="d3373-192">Update the modified service’s manifest file (ServiceManifest.xml) with the updated versions for the service (and Code, Config, or Data, as relevant).</span></span> <span data-ttu-id="d3373-193">Dál upravte manifest aplikace (ApplicationManifest.xml) s použitím čísla aktualizované verze pro aplikaci a upravenou službu.</span><span class="sxs-lookup"><span data-stu-id="d3373-193">Also, modify the application’s manifest (ApplicationManifest.xml) with the updated version number for the application and the modified service.</span></span>  

<span data-ttu-id="d3373-194">Pokud chcete k upgradu aplikace použít Eclipse Neon, můžete vytvořit duplicitní profil konfigurace spuštění.</span><span class="sxs-lookup"><span data-stu-id="d3373-194">To upgrade your application by using Eclipse Neon, you can create a duplicate run configuration profile.</span></span> <span data-ttu-id="d3373-195">Potom ho podle potřeby využijte k upgradu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3373-195">Then, use it to upgrade your application as needed.</span></span>

1.  <span data-ttu-id="d3373-196">Přejděte k části **Run** > **Run Configurations** (Spustit > Konfigurace spuštění).</span><span class="sxs-lookup"><span data-stu-id="d3373-196">Go to **Run** > **Run Configurations**.</span></span> <span data-ttu-id="d3373-197">v levém podokně klikněte na malou šipku nalevo od možnosti **Gradle Project** (Projekt Gradle).</span><span class="sxs-lookup"><span data-stu-id="d3373-197">In the left pane, click the small arrow to the left of **Gradle Project**.</span></span>
2.  <span data-ttu-id="d3373-198">Klikněte pravým tlačítkem na **ServiceFabricDeployer** a potom vyberte **Duplicate** (Duplikovat).</span><span class="sxs-lookup"><span data-stu-id="d3373-198">Right-click **ServiceFabricDeployer**, and then select **Duplicate**.</span></span> <span data-ttu-id="d3373-199">Zadejte nový název pro tuto konfiguraci, třeba **ServiceFabricUpgrader**.</span><span class="sxs-lookup"><span data-stu-id="d3373-199">Enter a new name for this configuration, for example, **ServiceFabricUpgrader**.</span></span>
3.  <span data-ttu-id="d3373-200">Na pravém panelu na kartě **Arguments** (Argumenty) změňte **-Pconfig='deploy'** na **-Pconfig=upgrade** a potom klikněte na **Apply** (Použít).</span><span class="sxs-lookup"><span data-stu-id="d3373-200">In the right panel, on the **Arguments** tab, change **-Pconfig='deploy'** to **-Pconfig='upgrade'**, and then click **Apply**.</span></span>

<span data-ttu-id="d3373-201">Tento proces vytvoří a uloží profil konfigurace spuštění, který můžete kdykoliv použít k upgradu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3373-201">This process creates and saves a run configuration profile you can use at any time to upgrade your application.</span></span> <span data-ttu-id="d3373-202">Postará se také o získání nejnovější aktualizované verze typu aplikace ze souboru manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3373-202">It also gets the latest updated application type version from the application manifest file.</span></span>

<span data-ttu-id="d3373-203">Upgrade aplikace trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="d3373-203">The application upgrade takes a few minutes.</span></span> <span data-ttu-id="d3373-204">Průběh upgradu aplikace můžete monitorovat pomocí Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="d3373-204">You can monitor the application upgrade in Service Fabric Explorer.</span></span>

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a><span data-ttu-id="d3373-205">Migrace starých aplikací Service Fabric Java pro použití s Mavenem</span><span class="sxs-lookup"><span data-stu-id="d3373-205">Migrating old Service Fabric Java applications to be used with Maven</span></span>
<span data-ttu-id="d3373-206">Nedávno jsme přesunuli knihovny Service Fabric Java ze sady Service Fabric Java SDK do úložiště Maven.</span><span class="sxs-lookup"><span data-stu-id="d3373-206">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK to Maven repository.</span></span> <span data-ttu-id="d3373-207">Zatímco nové aplikace, které vygenerujete pomocí Eclipse, vygenerují nejnovější aktualizované projekty (které budou moct pracovat s Mavenem), můžete vaše stávající bezstavové aplikace nebo aplikace objektu actor v Javě pro Service Fabric, které dřív používali sadu Service Fabric Java SDK, aktualizovat tak, aby používaly závislosti Service Fabric Java z Mavenu.</span><span class="sxs-lookup"><span data-stu-id="d3373-207">While the new applications you generate using Eclipse, will generate latest updated projects (which will be able to work with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using the Service Fabric Java SDK earlier, to use the Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="d3373-208">Abyste zajistili fungování starších aplikací s Mavenem, postupujte podle kroků uvedených [tady](service-fabric-migrate-old-javaapp-to-use-maven.md).</span><span class="sxs-lookup"><span data-stu-id="d3373-208">Please follow the steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) to ensure your older application works with Maven.</span></span>

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
