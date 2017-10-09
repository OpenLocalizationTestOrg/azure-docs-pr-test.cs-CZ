---
title: "hello aaaPublish pružiny spouštěcí aplikace jako kontejner Docker pomocí nástrojů Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak hello toopublish tooMicrosoft webové aplikace Azure jako kontejner Docker pomocí nástrojů Azure pro IntelliJ."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/21/2017
ms.author: robmcm
ms.openlocfilehash: 8964cb33fd8f61a39f091633ae9074d9658232fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a><span data-ttu-id="a673b-103">Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí hello Azure Toolkit pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a673b-103">Publish a Spring Boot app as a Docker container by using hello Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="a673b-104">Hello [pružiny Framework] open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="a673b-104">hello [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="a673b-105">Jeden z hello oblíbených další projekty, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="a673b-105">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="a673b-106">[Docker] je do řešení open source, který pomáhá vývojářům automatizovat nasazení hello, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="a673b-106">[Docker] is an open-source solution that helps developers automate hello deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="a673b-107">Tento kurz vás provede kroky toodeploy hello pružiny spouštěcí aplikace jako tooMicrosoft kontejner Docker Azure pomocí hello Azure Toolkit pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="a673b-107">This tutorial walks you through hello steps toodeploy a Spring Boot application as a Docker container tooMicrosoft Azure by using hello Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repo"></a><span data-ttu-id="a673b-108">Klonování úložiště Docker spouštěcí pružiny výchozí hello</span><span class="sxs-lookup"><span data-stu-id="a673b-108">Clone hello default Spring Boot Docker repo</span></span>

<span data-ttu-id="a673b-109">Hello následující postup vás provede procesem klonování úložiště Docker spouštěcí pružiny hello pomocí IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="a673b-109">hello following steps walk you through cloning hello Spring Boot Docker repo by using IntelliJ.</span></span> <span data-ttu-id="a673b-110">Pokud chcete toouse příkazového řádku, najdete v části [nasazení pružiny spuštění aplikace v systému Linux v Azure Container Service][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="a673b-110">If you want toouse a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="a673b-111">Otevřete IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="a673b-111">Open IntelliJ.</span></span>

1. <span data-ttu-id="a673b-112">Na úvodní obrazovce hello vyberte hello **Githubu** možnost v hello **rezervovat z verzí** seznamu.</span><span class="sxs-lookup"><span data-stu-id="a673b-112">On hello welcome screen, select hello **GitHub** option in hello **Check out from Version Control** list.</span></span>

   ![Možnost GitHub pro správu verzí][CL01]

1. <span data-ttu-id="a673b-114">Zadejte přihlašovací údaje, pokud jste výzvami toolog v.</span><span class="sxs-lookup"><span data-stu-id="a673b-114">Enter your credentials if you are prompted toolog in.</span></span>

   * <span data-ttu-id="a673b-115">Pokud používáte toolog uživatelského jména a hesla v tooGitHub:</span><span class="sxs-lookup"><span data-stu-id="a673b-115">If you are using a username/password toolog in tooGitHub:</span></span>

      ![Dialogové okno pro zadání Githubu uživatelské jméno a heslo][CL02a]

   * <span data-ttu-id="a673b-117">Pokud používáte tokenu toolog v tooGitHub:</span><span class="sxs-lookup"><span data-stu-id="a673b-117">If you are using a token toolog in tooGitHub:</span></span>

      ![Dialogové okno pro zadání token Githubu][CL02b]

1. <span data-ttu-id="a673b-119">Zadejte **https://github.com/spring-guides/gs-spring-boot-docker.git** hello úložišti adresy URL, zadejte místní cestu a informace o složce a pak klikněte na tlačítko **klon**.</span><span class="sxs-lookup"><span data-stu-id="a673b-119">Enter **https://github.com/spring-guides/gs-spring-boot-docker.git** for hello repo URL, specify your local path and folder information, and then click **Clone**.</span></span>

   ![Dialogové okno klonování úložiště][CL03]

1. <span data-ttu-id="a673b-121">Když se zobrazí výzva toocreate IntelliJ projekt, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="a673b-121">When you're prompted toocreate an IntelliJ project, select **No**.</span></span>

   ![Odmítnutí toocreate projektu IntelliJ][CL04]

1. <span data-ttu-id="a673b-123">Na úvodní stránku hello, klikněte na tlačítko **Importovat projekt**.</span><span class="sxs-lookup"><span data-stu-id="a673b-123">On hello welcome page, click **Import Project**.</span></span>

   ![Import výběru projektů][CL05]

1. <span data-ttu-id="a673b-125">Vyhledejte cestu hello, které jste naklonovali úložiště pružiny spouštěcí hello, vyberte hello **dokončení** ve složce hello kořenové a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a673b-125">Locate hello path where you cloned hello Spring Boot repo, select hello **complete** folder under hello root, and then click **OK**.</span></span>

   ![Vyberte složku pro import][CL06]

1. <span data-ttu-id="a673b-127">Když se zobrazí výzva, vyberte **vytvoření projektu z existujících zdrojů**.</span><span class="sxs-lookup"><span data-stu-id="a673b-127">When you're prompted, select **Create project from existing sources**.</span></span>

   ![Možnost toocreate projektu z existujících zdrojů][CL07]

1. <span data-ttu-id="a673b-129">Zadejte název projektu nebo přijměte výchozí hello, ověřte správnou cestu toohello hello **dokončení** složku a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a673b-129">Specify your project name or accept hello default, verify hello correct path toohello **complete** folder, and then click **Next**.</span></span>

   ![Zadejte název projektu hello][CL08]

1. <span data-ttu-id="a673b-131">Přizpůsobit všechny adresáře pro import a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a673b-131">Customize any directories for importing, and then click **Next**.</span></span>

   ![Zvolte adresáře][CL09]

1. <span data-ttu-id="a673b-133">Zkontrolujte tooimport hello knihovny a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a673b-133">Review hello libraries tooimport, and then click **Next**.</span></span>

   ![Zkontrolujte projektu knihovny][CL10]

1. <span data-ttu-id="a673b-135">Zkontrolujte struktura hello modulu a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a673b-135">Review hello module structure, and then click **Next**.</span></span>

   ![Zkontrolujte struktura modulu][CL11]

1. <span data-ttu-id="a673b-137">Zadejte vaše JDK a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a673b-137">Specify your JDK, and then click **Next**.</span></span>

   ![Zadejte JDK][CL12]

1. <span data-ttu-id="a673b-139">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="a673b-139">Click **Finish**.</span></span>

   ![Tlačítko Dokončit][CL13]

<span data-ttu-id="a673b-141">IntelliJ importuje hello pružiny spouštěcí aplikace jako projekt a zobrazí hello struktura po dokončení importu hello.</span><span class="sxs-lookup"><span data-stu-id="a673b-141">IntelliJ imports hello Spring Boot app as a project and displays hello structure when hello import has finished.</span></span>

![Spuštění aplikace Spring v IntelliJ][CL14]

## <a name="build-your-spring-boot-app"></a><span data-ttu-id="a673b-143">Sestavení vaší pružiny spouštěcí aplikace</span><span class="sxs-lookup"><span data-stu-id="a673b-143">Build your Spring Boot app</span></span>

### <a name="build-hello-app-by-using-hello-maven-pom"></a><span data-ttu-id="a673b-144">Sestavení aplikace hello pomocí hello Maven POM</span><span class="sxs-lookup"><span data-stu-id="a673b-144">Build hello app by using hello Maven POM</span></span>

1. <span data-ttu-id="a673b-145">Pokud už není otevřený, otevřete okno nástroje Maven hello.</span><span class="sxs-lookup"><span data-stu-id="a673b-145">Open hello Maven tool window if it is not already opened.</span></span> <span data-ttu-id="a673b-146">Klikněte na tlačítko **zobrazení** > **nástroj Windows** > **projekty Maven**.</span><span class="sxs-lookup"><span data-stu-id="a673b-146">Click **View** > **Tool Windows** > **Maven Projects**.</span></span>

   ![Nástroje systému Windows a projekty Maven příkazy][BU01]

1. <span data-ttu-id="a673b-148">V okně nástroje Maven hello, klikněte pravým tlačítkem na **balíček** a vyberte **spustit Build Maven**.</span><span class="sxs-lookup"><span data-stu-id="a673b-148">In hello Maven tool window, right-click **package** and select **Run Maven Build**.</span></span> <span data-ttu-id="a673b-149">(Pokud projekt Maven nezobrazí automaticky, klikněte na tlačítko hello **znovu naimportovat** ikonu na panelu nástrojů hello Maven.)</span><span class="sxs-lookup"><span data-stu-id="a673b-149">(If your Maven project does not show up automatically, click hello **Reimport** icon on hello Maven toolbar.)</span></span>

   ![Spusťte příkaz Maven Build][BU02]

1. <span data-ttu-id="a673b-151">IntelliJ by se měl zobrazit **ÚSPĚŠNOSTI sestavení** zprávy při pružiny spouštěcí aplikace je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="a673b-151">IntelliJ should display a **BUILD SUCCESS** message when your Spring Boot app is successfully created.</span></span>

   ![Zpráva ÚSPĚŠNOSTI sestavení][BU03]

### <a name="create-a-deployment-ready-artifact"></a><span data-ttu-id="a673b-153">Vytvoření artefaktu připravené pro nasazení</span><span class="sxs-lookup"><span data-stu-id="a673b-153">Create a deployment-ready artifact</span></span>

<span data-ttu-id="a673b-154">toopublish pružiny spouštěcí aplikace, budete potřebovat toocreate artefaktů připravené pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="a673b-154">toopublish your Spring Boot app, you need toocreate a deployment-ready artifact.</span></span> <span data-ttu-id="a673b-155">Použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a673b-155">Use hello following steps:</span></span>

1. <span data-ttu-id="a673b-156">Otevřete projekt webové aplikace v IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="a673b-156">Open your web app project in IntelliJ.</span></span>

1. <span data-ttu-id="a673b-157">Klikněte na tlačítko **soubor**a potom klikněte na **strukturu projektu**.</span><span class="sxs-lookup"><span data-stu-id="a673b-157">Click **File**, and then click **Project Structure**.</span></span>

   ![Struktura projekt – příkaz][ART01]

1. <span data-ttu-id="a673b-159">Klikněte na zelenou plus hello (**+**) symbolů tooadd artefakt, klikněte na tlačítko **JAR**a potom klikněte na **prázdný**.</span><span class="sxs-lookup"><span data-stu-id="a673b-159">Click hello green plus (**+**) symbol tooadd an artifact, click **JAR**, and then click **Empty**.</span></span>

   ![Přidat artefakt][ART02]

1. <span data-ttu-id="a673b-161">Název vašeho artefaktů přičemž zajistěte, aby není tooadd hello rozšíření ".jar" a pak zadejte hello cílovou složku pro výstupní Maven hello.</span><span class="sxs-lookup"><span data-stu-id="a673b-161">Name your artifact while making sure not tooadd hello ".jar" extension, and then specify hello target folder for hello Maven output.</span></span>

   ![Zadejte vlastnosti artefaktů][ART03]

1. <span data-ttu-id="a673b-163">Vytvoření manifestu pro vaše artefaktů (volitelné):</span><span class="sxs-lookup"><span data-stu-id="a673b-163">Create a manifest for your artifact (optional):</span></span>

   <span data-ttu-id="a673b-164">a.</span><span class="sxs-lookup"><span data-stu-id="a673b-164">a.</span></span> <span data-ttu-id="a673b-165">Klikněte na tlačítko **vytvoření manifestu**.</span><span class="sxs-lookup"><span data-stu-id="a673b-165">Click **Create Manifest**.</span></span>

      ![Klikněte na tlačítko Vytvořit Manifest hello][ART04a]

   <span data-ttu-id="a673b-167">b.</span><span class="sxs-lookup"><span data-stu-id="a673b-167">b.</span></span> <span data-ttu-id="a673b-168">Zvolte hello výchozí cesta pro hello artefaktů a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a673b-168">Choose hello default path for hello artifact, and then click **OK**.</span></span>

      ![Zadejte cestu artefaktů][ART04b]

   <span data-ttu-id="a673b-170">c.</span><span class="sxs-lookup"><span data-stu-id="a673b-170">c.</span></span> <span data-ttu-id="a673b-171">Klikněte na tlačítko se třemi tečkami hello (**...** ) toolocate hello hlavní třídy.</span><span class="sxs-lookup"><span data-stu-id="a673b-171">Click hello ellipsis (**...**) toolocate hello main class.</span></span>

      ![Vyhledejte hlavní – třída][ART04c]

   <span data-ttu-id="a673b-173">d.</span><span class="sxs-lookup"><span data-stu-id="a673b-173">d.</span></span> <span data-ttu-id="a673b-174">Vyberte hlavní třídy a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a673b-174">Choose your main class, and then click **OK**.</span></span>

      ![Zadejte hlavní – třída][ART04d]

1. <span data-ttu-id="a673b-176">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a673b-176">Click **OK**.</span></span>

   ![Zavřete dialogové okno projektu struktura hello][ART05]

> [!NOTE]
> <span data-ttu-id="a673b-178">Další informace o vytváření artefakty v IntelliJ najdete v tématu [konfigurace artefakty] na webu JetBrains hello.</span><span class="sxs-lookup"><span data-stu-id="a673b-178">For more information about creating artifacts in IntelliJ, see [Configuring Artifacts] on hello JetBrains website.</span></span>
>

### <a name="build-hello-artifact-for-deployment"></a><span data-ttu-id="a673b-179">Sestavení hello artefaktů pro nasazení</span><span class="sxs-lookup"><span data-stu-id="a673b-179">Build hello artifact for deployment</span></span>

1. <span data-ttu-id="a673b-180">Klikněte na tlačítko **sestavení**a potom klikněte na **artefakty**.</span><span class="sxs-lookup"><span data-stu-id="a673b-180">Click **Build**, and then click **Artifacts**.</span></span>

   ![Příkaz artefaktů sestavení][BU04]

1. <span data-ttu-id="a673b-182">Když hello **sestavení artefaktů** místní nabídce klikněte na tlačítko **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="a673b-182">When hello **Build Artifact** context menu appears, click **Build**.</span></span>

   ![Sestavení artefaktů kontextové nabídky][BU05]

<span data-ttu-id="a673b-184">IntelliJ by měl zobrazovat hello dokončit artefaktů pro vaši aplikaci pružiny spouštěcí v okně nástroje projektu hello.</span><span class="sxs-lookup"><span data-stu-id="a673b-184">IntelliJ should display hello completed artifact for your Spring Boot app in hello project tool window.</span></span>

   ![Vytvořený artefaktů][BU06]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="a673b-186">Publikovat tooAzure vaší webové aplikace pomocí kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="a673b-186">Publish your web app tooAzure by using a Docker container</span></span>

1. <span data-ttu-id="a673b-187">Pokud nejste přihlášeni tooyour účet Azure, postupujte podle kroků hello v [přihlášení pokyny pro hello Azure Toolkit IntelliJ][Azure Sign In for IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="a673b-187">If you have not signed in tooyour Azure account, follow hello steps in [Sign-in instructions for hello Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span></span>

1. <span data-ttu-id="a673b-188">V okně nástroje hello prohlížeči projektu klikněte pravým tlačítkem na projekt hello a vyberte **Azure** > **publikovat jako kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="a673b-188">In hello Project Explorer tool window, right-click hello project, and then select **Azure** > **Publish as Docker Container**.</span></span>

   ![Publikovat jako příkaz kontejner Docker][PU01]

1. <span data-ttu-id="a673b-190">Když hello **nasadit kontejner Docker v Azure** se zobrazí dialogové okno, jsou zobrazeny všechny existující hostitelů Docker.</span><span class="sxs-lookup"><span data-stu-id="a673b-190">When hello **Deploy Docker Container on Azure** dialog box appears, any existing Docker hosts are displayed.</span></span> <span data-ttu-id="a673b-191">Pokud si zvolíte toodeploy tooan existující hostitele, můžete přeskočit toostep 4.</span><span class="sxs-lookup"><span data-stu-id="a673b-191">If you choose toodeploy tooan existing host, you can skip toostep 4.</span></span> <span data-ttu-id="a673b-192">Jinak použijte následující kroky toocreate hostitele hello:</span><span class="sxs-lookup"><span data-stu-id="a673b-192">Otherwise, use hello following steps toocreate a host:</span></span>

   <span data-ttu-id="a673b-193">a.</span><span class="sxs-lookup"><span data-stu-id="a673b-193">a.</span></span> <span data-ttu-id="a673b-194">Klikněte na zelenou plus hello (**+**) symbol.</span><span class="sxs-lookup"><span data-stu-id="a673b-194">Click hello green plus (**+**) symbol.</span></span>

      ![Přidat nový hostitel Docker][PU02]

   <span data-ttu-id="a673b-196">b.</span><span class="sxs-lookup"><span data-stu-id="a673b-196">b.</span></span> <span data-ttu-id="a673b-197">Když hello **vytvořit hostitelů Docker** zobrazí se dialogové okno, můžete zvolit výchozí hello tooaccept nebo můžete zadat vlastní nastavení pro nové Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="a673b-197">When hello **Create Docker Host** dialog box appears, you can choose tooaccept hello defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="a673b-198">(Podrobný popis hello různá nastavení, najdete v části [publikovat webovou aplikaci jako kontejner Docker pomocí hello Azure Toolkit pro IntelliJ][Publish Container with Azure Toolkit].) Klikněte na tlačítko **Další** jste zadali při které toouse nastavení.</span><span class="sxs-lookup"><span data-stu-id="a673b-198">(For detailed descriptions of hello various settings, see [Publish a web app as a Docker container by using hello Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings toouse.</span></span>

      ![Zadejte možnosti hostitelů Docker][PU03a]

   <span data-ttu-id="a673b-200">c.</span><span class="sxs-lookup"><span data-stu-id="a673b-200">c.</span></span> <span data-ttu-id="a673b-201">Můžete vybrat toouse existující přihlašovací údaje z trezoru služby Azure klíče nebo můžete tooenter nové Docker přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="a673b-201">You can choose toouse existing login credentials from an Azure key vault, or you can choose tooenter new Docker login credentials.</span></span> <span data-ttu-id="a673b-202">Klikněte na tlačítko **Dokončit** když nastavíte možnosti.</span><span class="sxs-lookup"><span data-stu-id="a673b-202">Click **Finish** when you have specified your options.</span></span>

      ![Zadejte přihlašovací údaje hostitelů Docker][PU03b]

1. <span data-ttu-id="a673b-204">Vyberte Docker hostiteli a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a673b-204">Select your Docker host, and then click **Next**.</span></span>

   ![Vyberte toouse hostitelů Docker hello][PU04]

1. <span data-ttu-id="a673b-206">Na poslední stránce hello hello **nasadit kontejner Docker v Azure** dialogovém okně zadejte hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="a673b-206">On hello last page of hello **Deploy Docker Container on Azure** dialog box, specify hello following options:</span></span>

   <span data-ttu-id="a673b-207">a.</span><span class="sxs-lookup"><span data-stu-id="a673b-207">a.</span></span> <span data-ttu-id="a673b-208">Můžete toospecify vlastní název pro hello kontejneru, který bude hostovat vaše kontejner Docker nebo můžete přijmout výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="a673b-208">You can choose toospecify a custom name for hello container that will host your Docker container, or you can accept hello default.</span></span>

   <span data-ttu-id="a673b-209">b.</span><span class="sxs-lookup"><span data-stu-id="a673b-209">b.</span></span> <span data-ttu-id="a673b-210">Zadejte porty TCP hello pro svého hostitele docker pomocí následující syntaxe hello: *[externí port]*:*[interní port]*.</span><span class="sxs-lookup"><span data-stu-id="a673b-210">Enter hello TCP ports for your docker host by using hello following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="a673b-211">Například **80:8080** určuje externí port 80 a hello výchozí vnitřní pružiny spouštěcí port 8080.</span><span class="sxs-lookup"><span data-stu-id="a673b-211">For example, **80:8080** specifies an external port of 80 and hello default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="a673b-212">Pokud jste upravili interní port (například úpravou souboru application.yml hello), je třeba číslo portu hello toospecify pro správné směrování toooccur hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="a673b-212">If you have customized your internal port (for example, by editing hello application.yml file), you need toospecify hello port number for hello correct routing toooccur in Azure.</span></span>

   <span data-ttu-id="a673b-213">c.</span><span class="sxs-lookup"><span data-stu-id="a673b-213">c.</span></span> <span data-ttu-id="a673b-214">Po nakonfigurování těchto možností, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="a673b-214">After you have configured these options, click **Finish**.</span></span>

   ![Nasadit kontejner Docker v Azure][PU05]

1. <span data-ttu-id="a673b-216">Po dokončení publikování hello Azure Toolkit hello protokol činnosti Azure zobrazí **publikováno** hello stavu.</span><span class="sxs-lookup"><span data-stu-id="a673b-216">When hello Azure Toolkit has finished publishing, hello Azure Activity Log displays **Published** for hello status.</span></span>

   ![Byla úspěšně nasazena hostitelů Docker][PU06]

## <a name="next-steps"></a><span data-ttu-id="a673b-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a673b-218">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="a673b-219">toolearn o další metody pro vytváření aplikací pružiny spouštěcí pomocí IntelliJ, najdete v části [vytváření projektů spouštěcí pružiny](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) na webu JetBrains hello.</span><span class="sxs-lookup"><span data-stu-id="a673b-219">toolearn about additional methods for creating Spring Boot apps by using IntelliJ, see [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) on hello JetBrains website.</span></span>

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[konfigurace artefakty]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png
