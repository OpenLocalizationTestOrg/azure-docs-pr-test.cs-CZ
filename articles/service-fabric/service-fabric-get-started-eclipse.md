---
title: aaaAzure Service Fabric modulu plug-in pro Eclipse | Microsoft Docs
description: "Začínáme s hello Service Fabric modulu plug-in pro Eclipse."
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
ms.openlocfilehash: 4ba5a28a6282387249a2bd4e62314e891ff04162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a><span data-ttu-id="9ae90-103">Modul plug-in Service Fabric pro vývoj aplikací v Eclipse Javě</span><span class="sxs-lookup"><span data-stu-id="9ae90-103">Service Fabric plug-in for Eclipse Java application development</span></span>
<span data-ttu-id="9ae90-104">Eclipse je jednou z nejčastěji používá hello integrované vývojové prostředí (integrovaného vývojového prostředí) pro vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="9ae90-104">Eclipse is one of hello most widely used integrated development environments (IDEs) for Java developers.</span></span> <span data-ttu-id="9ae90-105">V tomto článku jsme popisují, jak tooset do vašeho prostředí Eclipse vývoj prostředí toowork s Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="9ae90-105">In this article, we describe how tooset up your Eclipse development environment toowork with Azure Service Fabric.</span></span> <span data-ttu-id="9ae90-106">Zjistěte, jak vytvořit aplikace Service Fabric tooinstall hello Service Fabric modul plug-in a nasazení vaší Service Fabric aplikace tooa místního nebo vzdáleného clusteru Service Fabric v prostředí Eclipse Neónová.</span><span class="sxs-lookup"><span data-stu-id="9ae90-106">Learn how tooinstall hello Service Fabric plug-in, create a Service Fabric application, and deploy your Service Fabric application tooa local or remote Service Fabric cluster in Eclipse Neon.</span></span>

## <a name="install-or-update-hello-service-fabric-plug-in-in-eclipse-neon"></a><span data-ttu-id="9ae90-107">Nainstalujte nebo aktualizujte hello Service Fabric v prostředí Eclipse Neónová modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="9ae90-107">Install or update hello Service Fabric plug-in in Eclipse Neon</span></span>
<span data-ttu-id="9ae90-108">Modul plug-in Service Fabric můžete nainstalovat do Eclipse.</span><span class="sxs-lookup"><span data-stu-id="9ae90-108">You can install a Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="9ae90-109">Hello modul plug-in může zjednodušit proces vytváření a nasazení služeb Java hello.</span><span class="sxs-lookup"><span data-stu-id="9ae90-109">hello plug-in can help simplify hello process of building and deploying Java services.</span></span>

1.  <span data-ttu-id="9ae90-110">Ujistěte se, že máte nejnovější verzi Eclipse Neónová hello a hello nejnovější verzi Buildship nainstalován (1.0.17 nebo novější):</span><span class="sxs-lookup"><span data-stu-id="9ae90-110">Ensure that you have hello latest version of Eclipse Neon and hello latest version of Buildship (1.0.17 or a later version) installed:</span></span>
    -   <span data-ttu-id="9ae90-111">toocheck hello verze nainstalované součásti v prostředí Eclipse Neónová přejděte příliš**pomoci** > **podrobné informace o instalaci**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-111">toocheck hello versions of installed components, in Eclipse Neon, go too**Help** > **Installation Details**.</span></span>
    -   <span data-ttu-id="9ae90-112">tooupdate Buildship, najdete v části [Eclipse Buildship: Eclipse moduly plug-in pro Gradle][buildship-update].</span><span class="sxs-lookup"><span data-stu-id="9ae90-112">tooupdate Buildship, see [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].</span></span>
    -   <span data-ttu-id="9ae90-113">toocheck pro a nainstalovat aktualizace pro Eclipse Neónová přejděte příliš**pomoci** > **vyhledat aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-113">toocheck for and install updates for Eclipse Neon, go too**Help** > **Check for Updates**.</span></span>

2.  <span data-ttu-id="9ae90-114">tooinstall hello Service Fabric modul plug-in, v prostředí Eclipse Neónová přejděte příliš**pomoci** > **instalace nového softwaru**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-114">tooinstall hello Service Fabric plug-in, in Eclipse Neon, go too**Help** > **Install New Software**.</span></span>
  1.    <span data-ttu-id="9ae90-115">V hello **pracovat s** zadejte **http://dl.microsoft.com/eclipse**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-115">In hello **Work with** box, enter **http://dl.microsoft.com/eclipse**.</span></span>
  2.    <span data-ttu-id="9ae90-116">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-116">Click **Add**.</span></span>

         ![Modul plug-in Service Fabric pro Eclipse Neon][sf-eclipse-plugin-install]
  3.    <span data-ttu-id="9ae90-118">Vyberte hello Service Fabric modul plug-in a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-118">Select hello Service Fabric plug-in, and then click **Next**.</span></span>
  4.    <span data-ttu-id="9ae90-119">Dokončete kroky instalace hello a přijměte licenční podmínky softwaru společnosti Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="9ae90-119">Complete hello installation steps, and then accept hello Microsoft Software License Terms.</span></span>

<span data-ttu-id="9ae90-120">Pokud již máte hello Service Fabric modulu plug-in nainstalována, ujistěte se, že máte nejnovější verzi hello.</span><span class="sxs-lookup"><span data-stu-id="9ae90-120">If you already have hello Service Fabric plug-in installed, make sure that you have hello latest version.</span></span> <span data-ttu-id="9ae90-121">toocheck dostupných aktualizací, přejděte příliš**pomoci** > **podrobné informace o instalaci**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-121">toocheck for available updates, go too**Help** > **Installation Details**.</span></span> <span data-ttu-id="9ae90-122">V seznamu nainstalovaných modulů plug-in hello vyberte Service Fabric a pak klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-122">In hello list of installed plug-ins, select Service Fabric, and then click **Update**.</span></span> <span data-ttu-id="9ae90-123">Nainstalují se dostupné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9ae90-123">Available updates will be installed.</span></span>

> [!NOTE]
> <span data-ttu-id="9ae90-124">Pokud instalace nebo aktualizace hello Service Fabric modul plug-in je pomalé, může to být kvůli tooan Eclipse nastavení.</span><span class="sxs-lookup"><span data-stu-id="9ae90-124">If installing or updating hello Service Fabric plug-in is slow, it might be due tooan Eclipse setting.</span></span> <span data-ttu-id="9ae90-125">Eclipse shromažďuje metadata na všechny lokality tooupdate změny, které jsou registrovány s vaší instancí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="9ae90-125">Eclipse collects metadata on all changes tooupdate sites that are registered with your Eclipse instance.</span></span> <span data-ttu-id="9ae90-126">toospeed až hello proces vyhledávání a instalace modulu plug-in aktualizace Service Fabric, přejděte příliš**dostupných stránek softwaru**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-126">toospeed up hello process of checking for and installing a Service Fabric plug-in update, go too**Available Software Sites**.</span></span> <span data-ttu-id="9ae90-127">Zrušte zaškrtnutí políček hello pro všechny weby s výjimkou hello ten, který ukazuje toohello Service Fabric modulu plug-in umístění (http://dl.microsoft.com/eclipse/azure/servicefabric).</span><span class="sxs-lookup"><span data-stu-id="9ae90-127">Clear hello check boxes for all sites except for hello one that points toohello Service Fabric plug-in location (http://dl.microsoft.com/eclipse/azure/servicefabric).</span></span>

## <a name="create-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="9ae90-128">Vytvoření aplikace Service Fabric pomocí Eclipse</span><span class="sxs-lookup"><span data-stu-id="9ae90-128">Create a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="9ae90-129">V Eclipse Neónová přejděte příliš**soubor** > **nový** > **jiných**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-129">In Eclipse Neon, go too**File** > **New** > **Other**.</span></span> <span data-ttu-id="9ae90-130">Vyberte **Service Fabric Project** (Projekt Service Fabric) a potom klikněte na **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="9ae90-130">Select  **Service Fabric Project**, and then click **Next**.</span></span>

    ![Nový projekt Service Fabric – stránka 1][create-application/p1]

2.  <span data-ttu-id="9ae90-132">Zadejte název pro svůj projekt a potom klikněte na **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="9ae90-132">Enter a name for your project, and then click **Next**.</span></span>

    ![Nový projekt Service Fabric – stránka 2][create-application/p2]

3.  <span data-ttu-id="9ae90-134">V seznamu hello šablon vyberte **šablony služby**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-134">In hello list of templates, select **Service Template**.</span></span> <span data-ttu-id="9ae90-135">Vyberte typ šablony služby (Actor, Stateless, Container nebo Guest Binary) a potom klikněte na **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="9ae90-135">Select your service template type (Actor, Stateless, Container, or Guest Binary), and then click **Next**.</span></span>

    ![Nový projekt Service Fabric – stránka 3][create-application/p3]

4.  <span data-ttu-id="9ae90-137">Zadejte název služby hello podrobnosti služby a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-137">Enter hello service name and service details, and then click **Finish**.</span></span>

    ![Nový projekt Service Fabric – stránka 4][create-application/p4]

5. <span data-ttu-id="9ae90-139">Když vytvoříte svůj první projekt Service Fabric v hello **otevřete přidružené hlediska** dialogové okno, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-139">When you create your first Service Fabric project, in hello **Open Associated Perspective** dialog box, click **Yes**.</span></span>

    ![Nový projekt Service Fabric – stránka 5][create-application/p5]

6.  <span data-ttu-id="9ae90-141">Nový projekt vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9ae90-141">Your new project looks like this:</span></span>

    ![Nový projekt Service Fabric – stránka 6][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="9ae90-143">Vytvoření a nasazení aplikace Service Fabric v Eclipse</span><span class="sxs-lookup"><span data-stu-id="9ae90-143">Build and deploy a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="9ae90-144">Klikněte na novou aplikaci Service Fabric pravým tlačítkem a potom vyberte **Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-144">Right-click your new Service Fabric application, and then select **Service Fabric**.</span></span>

    ![Nabídka Service Fabric zobrazená po kliknutí pravým tlačítkem][publish/RightClick]

2. <span data-ttu-id="9ae90-146">V podnabídce hello vyberte požadovanou možnost hello:</span><span class="sxs-lookup"><span data-stu-id="9ae90-146">In hello submenu, select hello option you want:</span></span>
    -   <span data-ttu-id="9ae90-147">Klikněte na tlačítko aplikace hello toobuild bez čištění, **sestavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-147">toobuild hello application without cleaning, click **Build Application**.</span></span>
    -   <span data-ttu-id="9ae90-148">Klikněte na tlačítko toodo čistá sestavení aplikace hello **znovu sestavte aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-148">toodo a clean build of hello application, click **Rebuild Application**.</span></span>
    -   <span data-ttu-id="9ae90-149">Klikněte na tlačítko aplikace hello tooclean integrovaný artefaktů, **vyčištění aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-149">tooclean hello application of built artifacts, click **Clean Application**.</span></span>

3.  <span data-ttu-id="9ae90-150">V této nabídce můžete zvolit také nasazení, zrušení nasazení nebo publikování aplikace:</span><span class="sxs-lookup"><span data-stu-id="9ae90-150">From this menu, you also can deploy, undeploy, and publish your application:</span></span>
    -   <span data-ttu-id="9ae90-151">toodeploy tooyour místní cluster, klikněte na tlačítko **nasazení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-151">toodeploy tooyour local cluster, click **Deploy Application**.</span></span>
    -   <span data-ttu-id="9ae90-152">V hello **publikovat aplikace** dialogové okno, vyberte profil publikování:</span><span class="sxs-lookup"><span data-stu-id="9ae90-152">In hello **Publish Application** dialog box, select a publish profile:</span></span>
        -  <span data-ttu-id="9ae90-153">**Local.json**</span><span class="sxs-lookup"><span data-stu-id="9ae90-153">**Local.json**</span></span>
        -  <span data-ttu-id="9ae90-154">**Cloud.json**</span><span class="sxs-lookup"><span data-stu-id="9ae90-154">**Cloud.json**</span></span>

     <span data-ttu-id="9ae90-155">Tyto soubory JavaScript Object Notation (JSON) ukládat informace (například koncové body připojení a informace o zabezpečení), které jsou požadované tooconnect tooyour místní nebo v cloudu (Azure) clusteru.</span><span class="sxs-lookup"><span data-stu-id="9ae90-155">These JavaScript Object Notation (JSON) files store information (such as connection endpoints and security information) that is required tooconnect tooyour local or cloud (Azure) cluster.</span></span>

  ![Nabídka Publish (Publikovat) v Service Fabric][publish/Publish]

<span data-ttu-id="9ae90-157">Toodeploy jiný způsob, jak vaše aplikace Service Fabric pomocí Eclipse spuštění konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9ae90-157">An alternate way toodeploy your Service Fabric application is by using Eclipse run configurations.</span></span>

  1.    <span data-ttu-id="9ae90-158">Přejděte příliš**spustit** > **konfigurace spuštění**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-158">Go too**Run** > **Run Configurations**.</span></span>
  2.    <span data-ttu-id="9ae90-159">V části **Gradle projektu**, vyberte hello **ServiceFabricDeployer** spustit konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9ae90-159">Under **Gradle Project**, select hello **ServiceFabricDeployer** run configuration.</span></span>
  3.    <span data-ttu-id="9ae90-160">V pravém podokně hello na hello **argumenty** kartě pro **publishProfile**, vyberte **místní** nebo **cloudu**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-160">In hello right pane, on hello **Arguments** tab, for **publishProfile**, select **local** or **cloud**.</span></span>  <span data-ttu-id="9ae90-161">Výchozí hodnota Hello je **místní**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-161">hello default is **local**.</span></span> <span data-ttu-id="9ae90-162">toodeploy tooa vzdálené nebo cloudu cluster, vyberte **cloudu**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-162">toodeploy tooa remote or cloud cluster, select **cloud**.</span></span>
  4.    <span data-ttu-id="9ae90-163">tooensure, správné informace hello se importují v hello publikační profily, upravit **Local.json** nebo **Cloud.json** podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="9ae90-163">tooensure that hello proper information is populated in hello publish profiles, edit **Local.json** or **Cloud.json** as needed.</span></span> <span data-ttu-id="9ae90-164">Můžete přidat nebo aktualizovat podrobnosti o koncových bodech a zabezpečovací přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9ae90-164">You can add or update endpoint details and security credentials.</span></span>
  5.    <span data-ttu-id="9ae90-165">Ujistěte se, že **pracovní adresář** body toohello aplikace, které chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="9ae90-165">Ensure that **Working Directory** points toohello application you want toodeploy.</span></span> <span data-ttu-id="9ae90-166">toochange hello aplikace, klikněte na tlačítko hello **prostoru** tlačítko a pak vyberte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="9ae90-166">toochange hello application, click hello **Workspace** button, and then select hello application you want.</span></span>
  6.    <span data-ttu-id="9ae90-167">Klikněte na **Apply** (Použít) a potom na **Run** (Spustit).</span><span class="sxs-lookup"><span data-stu-id="9ae90-167">Click **Apply**, and then click **Run**.</span></span>

<span data-ttu-id="9ae90-168">Vaše aplikace se během chvilky sestaví a nasadí.</span><span class="sxs-lookup"><span data-stu-id="9ae90-168">Your application builds and deploys within a few moments.</span></span> <span data-ttu-id="9ae90-169">Můžete sledovat stav nasazení hello v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="9ae90-169">You can monitor hello deployment status in Service Fabric Explorer.</span></span>  

## <a name="add-a-service-fabric-service-tooyour-service-fabric-application"></a><span data-ttu-id="9ae90-170">Přidat Service Fabric service tooyour aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="9ae90-170">Add a Service Fabric service tooyour Service Fabric application</span></span>

<span data-ttu-id="9ae90-171">tooadd Service Fabric service tooan existující aplikace Service Fabric, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9ae90-171">tooadd a Service Fabric service tooan existing Service Fabric application, do hello following steps:</span></span>

1.  <span data-ttu-id="9ae90-172">Klikněte pravým tlačítkem na hello projektu tooadd služby a pak klikněte na **Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-172">Right-click hello project you want tooadd a service to, and then click **Service Fabric**.</span></span>

    ![Přidat službu Service Fabric – stránka 1][add-service/p1]

2.  <span data-ttu-id="9ae90-174">Klikněte na tlačítko **přidat Service Fabric Service**, a dokončení hello sada kroků tooadd toohello projektu služby.</span><span class="sxs-lookup"><span data-stu-id="9ae90-174">Click **Add Service Fabric Service**, and complete hello set of steps tooadd a service toohello project.</span></span>
3.  <span data-ttu-id="9ae90-175">Šablona služby vyberte hello tooadd tooyour projekt a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-175">Select hello service template you want tooadd tooyour project, and then click **Next**.</span></span>

    ![Přidat službu Service Fabric – stránka 2][add-service/p2]

4.  <span data-ttu-id="9ae90-177">Zadejte název služby hello (a další podrobnosti, podle potřeby) a pak klikněte na tlačítko hello **přidat službu** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ae90-177">Enter hello service name (and other details, as needed), and then click hello **Add Service** button.</span></span>  

    ![Přidat službu Service Fabric – stránka 3][add-service/p3]

5.  <span data-ttu-id="9ae90-179">Po přidání služby hello strukturu celkové projektu vypadá podobně jako toohello následující projektu:</span><span class="sxs-lookup"><span data-stu-id="9ae90-179">After hello service is added, your overall project structure looks similar toohello following project:</span></span>

    ![Přidat službu Service Fabric – stránka 4][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a><span data-ttu-id="9ae90-181">Úprava verzí manifestu aplikace Service Fabric v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="9ae90-181">Edit Manifest versions of your Service Fabric Java application</span></span>

<span data-ttu-id="9ae90-182">verze manifestu tooedit, klikněte pravým tlačítkem na projekt hello, přejděte příliš**Service Fabric** a vyberte **upravit verze manifestu...**  z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="9ae90-182">tooedit manifest versions, right click on hello project, go too**Service Fabric** and select **Edit Manifest Versions...** from hello menu dropdown.</span></span> <span data-ttu-id="9ae90-183">V Průvodci hello, můžete aktualizovat hello manifestu verze pro service manifest manifestu, aplikace a hello verze pro **kód**, **konfigurace** a **Data** balíčky.</span><span class="sxs-lookup"><span data-stu-id="9ae90-183">In hello wizard, you can update hello manifest versions for application manifest, service manifest and hello versions for **Code**, **Config** and **Data** packages.</span></span>

<span data-ttu-id="9ae90-184">Pokud zaškrtnete možnost hello **automatické aktualizaci aplikace a verze aktualizace service** a aktualizujte na verzi, pak se automaticky aktualizuje verze manifestu hello.</span><span class="sxs-lookup"><span data-stu-id="9ae90-184">If you check hello option **Automatically update application and service versions** and then update a version, then hello manifest versions will be automatically updated.</span></span> <span data-ttu-id="9ae90-185">toogive příklad, je nejprve vybrat hello políčko a potom aktualizaci hello verze **kód** verzi z 0.0.0 too0.0.1 a klikněte na **Dokončit**, pak službu verze manifestu a manifest aplikace verze bude automaticky aktualizovány too0.0.1.</span><span class="sxs-lookup"><span data-stu-id="9ae90-185">toogive an example, you first select hello check-box, then update hello version of **Code** version from 0.0.0 too0.0.1 and click on **Finish**, then service manifest version and application manifest version will be automatically updated too0.0.1.</span></span>

## <a name="upgrade-your-service-fabric-java-application"></a><span data-ttu-id="9ae90-186">Upgrade aplikace Service Fabric v Javě</span><span class="sxs-lookup"><span data-stu-id="9ae90-186">Upgrade your Service Fabric Java application</span></span>

<span data-ttu-id="9ae90-187">Scénář upgradu, můžete vytvořit hello **App1** projektu pomocí hello Service Fabric v prostředí Eclipse modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="9ae90-187">For an upgrade scenario, say you created hello **App1** project by using hello Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="9ae90-188">Provedli jste nasazení pomocí modulu plug-in toocreate hello aplikaci s názvem **fabric: / App1Application**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-188">You deployed it by using hello plug-in toocreate an application named **fabric:/App1Application**.</span></span> <span data-ttu-id="9ae90-189">je typem aplikace Hello **App1AppicationType**, a verze aplikace hello je 1.0.</span><span class="sxs-lookup"><span data-stu-id="9ae90-189">hello application type is **App1AppicationType**, and hello application version is 1.0.</span></span> <span data-ttu-id="9ae90-190">Teď chcete tooupgrade aplikace bez přerušení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="9ae90-190">Now, you want tooupgrade your application without interrupting availability.</span></span>

<span data-ttu-id="9ae90-191">Nejprve proveďte požadované změny tooyour aplikace a pak znovu vytvořte hello upravit služby.</span><span class="sxs-lookup"><span data-stu-id="9ae90-191">First, make any changes tooyour application, and then rebuild hello modified service.</span></span> <span data-ttu-id="9ae90-192">Aktualizace hello změnit služby soubor manifestu (ServiceManifest.xml) hello aktualizované verze pro hello služby (a kód, konfigurace nebo Data, jako je relevantní).</span><span class="sxs-lookup"><span data-stu-id="9ae90-192">Update hello modified service’s manifest file (ServiceManifest.xml) with hello updated versions for hello service (and Code, Config, or Data, as relevant).</span></span> <span data-ttu-id="9ae90-193">Navíc upravte manifest aplikace hello (ApplicationManifest.xml) s hello aktualizovat číslo verze aplikace hello a hello upravené služby.</span><span class="sxs-lookup"><span data-stu-id="9ae90-193">Also, modify hello application’s manifest (ApplicationManifest.xml) with hello updated version number for hello application and hello modified service.</span></span>  

<span data-ttu-id="9ae90-194">tooupgrade vaší aplikace pomocí Neónová Eclipse, můžete vytvořit duplicitní spuštění konfiguračního profilu.</span><span class="sxs-lookup"><span data-stu-id="9ae90-194">tooupgrade your application by using Eclipse Neon, you can create a duplicate run configuration profile.</span></span> <span data-ttu-id="9ae90-195">Potom ho použít tooupgrade aplikace podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="9ae90-195">Then, use it tooupgrade your application as needed.</span></span>

1.  <span data-ttu-id="9ae90-196">Přejděte příliš**spustit** > **konfigurace spuštění**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-196">Go too**Run** > **Run Configurations**.</span></span> <span data-ttu-id="9ae90-197">V levém podokně hello, klikněte na tlačítko hello malou šipku toohello nalevo od **Gradle projektu**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-197">In hello left pane, click hello small arrow toohello left of **Gradle Project**.</span></span>
2.  <span data-ttu-id="9ae90-198">Klikněte pravým tlačítkem na **ServiceFabricDeployer** a potom vyberte **Duplicate** (Duplikovat).</span><span class="sxs-lookup"><span data-stu-id="9ae90-198">Right-click **ServiceFabricDeployer**, and then select **Duplicate**.</span></span> <span data-ttu-id="9ae90-199">Zadejte nový název pro tuto konfiguraci, třeba **ServiceFabricUpgrader**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-199">Enter a new name for this configuration, for example, **ServiceFabricUpgrader**.</span></span>
3.  <span data-ttu-id="9ae90-200">V pravém panelu hello na hello **argumenty** změňte **- Pconfig = 'nasazení'** příliš**- Pconfig = 'upgradu,**a potom klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="9ae90-200">In hello right panel, on hello **Arguments** tab, change **-Pconfig='deploy'** too**-Pconfig='upgrade'**, and then click **Apply**.</span></span>

<span data-ttu-id="9ae90-201">Tento proces vytvoří a uloží spuštění konfigurační profil můžete použít na všechny čas tooupgrade vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ae90-201">This process creates and saves a run configuration profile you can use at any time tooupgrade your application.</span></span> <span data-ttu-id="9ae90-202">Získá také hello nejnovější verze typu aktualizovanou aplikaci ze souboru manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9ae90-202">It also gets hello latest updated application type version from hello application manifest file.</span></span>

<span data-ttu-id="9ae90-203">upgrade aplikace Hello trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="9ae90-203">hello application upgrade takes a few minutes.</span></span> <span data-ttu-id="9ae90-204">Můžete monitorovat hello upgradu aplikace v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="9ae90-204">You can monitor hello application upgrade in Service Fabric Explorer.</span></span>

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a><span data-ttu-id="9ae90-205">Migrace starého toobe aplikace Service Fabric Java použít s Maven</span><span class="sxs-lookup"><span data-stu-id="9ae90-205">Migrating old Service Fabric Java applications toobe used with Maven</span></span>
<span data-ttu-id="9ae90-206">Nedávno jsme přesunuli knihovny Service Fabric Java z úložiště tooMaven Service Fabric Java SDK.</span><span class="sxs-lookup"><span data-stu-id="9ae90-206">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK tooMaven repository.</span></span> <span data-ttu-id="9ae90-207">Pokud je hello nové aplikace, který generovat pomocí prostředí Eclipse, vygeneruje nejnovější aktualizované projekty (které bude možné toowork s Maven), můžete aktualizovat existující Service Fabric bezstavové nebo objektu actor aplikací Java, které byly pomocí hello Service Fabric Java SDK starší, toouse hello Service Fabric Java závislostí z Maven.</span><span class="sxs-lookup"><span data-stu-id="9ae90-207">While hello new applications you generate using Eclipse, will generate latest updated projects (which will be able toowork with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using hello Service Fabric Java SDK earlier, toouse hello Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="9ae90-208">Postupujte podle kroků hello [sem](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure starší aplikace funguje s Maven.</span><span class="sxs-lookup"><span data-stu-id="9ae90-208">Please follow hello steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure your older application works with Maven.</span></span>

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
