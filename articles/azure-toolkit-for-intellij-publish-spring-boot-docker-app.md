---
title: "Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí nástrojů Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak publikovat webovou aplikaci do služby Microsoft Azure jako kontejner Docker pomocí nástrojů Azure pro IntelliJ."
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
ms.openlocfilehash: b771238934183c953615ac33c42a275d80657556
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="f9d61-103">Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí nástrojů Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="f9d61-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="f9d61-104">[Pružiny Framework] open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="f9d61-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="f9d61-105">Jedním z dalších oblíbených projektů, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="f9d61-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="f9d61-106">[Docker] je do řešení open source, který pomáhá vývojářům automatizovat nasazení, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="f9d61-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="f9d61-107">Tento kurz vás provede kroky nasazení spouštěcí pružiny aplikace jako kontejner Docker do služby Microsoft Azure pomocí sady nástrojů Azure pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="f9d61-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repo"></a><span data-ttu-id="f9d61-108">Naklonujte úložiště Docker spouštěcí pružiny výchozí</span><span class="sxs-lookup"><span data-stu-id="f9d61-108">Clone the default Spring Boot Docker repo</span></span>

<span data-ttu-id="f9d61-109">Následující postup vás provede procesem klonování úložiště Docker spouštěcí pružiny pomocí IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="f9d61-109">The following steps walk you through cloning the Spring Boot Docker repo by using IntelliJ.</span></span> <span data-ttu-id="f9d61-110">Pokud chcete použít příkazový řádek, přečtěte si téma [nasazení pružiny spuštění aplikace v systému Linux v Azure Container Service][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="f9d61-110">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="f9d61-111">Otevřete IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="f9d61-111">Open IntelliJ.</span></span>

1. <span data-ttu-id="f9d61-112">Na úvodní obrazovce, vyberte **Githubu** možnost **rezervovat z verzí** seznamu.</span><span class="sxs-lookup"><span data-stu-id="f9d61-112">On the welcome screen, select the **GitHub** option in the **Check out from Version Control** list.</span></span>

   ![Možnost GitHub pro správu verzí][CL01]

1. <span data-ttu-id="f9d61-114">Zadejte přihlašovací údaje, pokud se zobrazí výzva k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f9d61-114">Enter your credentials if you are prompted to log in.</span></span>

   * <span data-ttu-id="f9d61-115">Pokud používáte uživatelské jméno a heslo pro přihlášení ke Githubu:</span><span class="sxs-lookup"><span data-stu-id="f9d61-115">If you are using a username/password to log in to GitHub:</span></span>

      ![Dialogové okno pro zadání Githubu uživatelské jméno a heslo][CL02a]

   * <span data-ttu-id="f9d61-117">Pokud používáte token do protokolu Githubu:</span><span class="sxs-lookup"><span data-stu-id="f9d61-117">If you are using a token to log in to GitHub:</span></span>

      ![Dialogové okno pro zadání token Githubu][CL02b]

1. <span data-ttu-id="f9d61-119">Zadejte **https://github.com/spring-guides/gs-spring-boot-docker.git** úložišti adresy URL, zadejte místní cestu a informace o složce a pak klikněte na tlačítko **klon**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-119">Enter **https://github.com/spring-guides/gs-spring-boot-docker.git** for the repo URL, specify your local path and folder information, and then click **Clone**.</span></span>

   ![Dialogové okno klonování úložiště][CL03]

1. <span data-ttu-id="f9d61-121">Když se zobrazí výzva k vytvoření projektu IntelliJ, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-121">When you're prompted to create an IntelliJ project, select **No**.</span></span>

   ![Odmítnutí k vytvoření projektu IntelliJ][CL04]

1. <span data-ttu-id="f9d61-123">Na úvodní stránce klikněte na tlačítko **Importovat projekt**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-123">On the welcome page, click **Import Project**.</span></span>

   ![Import výběru projektů][CL05]

1. <span data-ttu-id="f9d61-125">Vyhledejte cestu, do které jste naklonovali úložiště pružiny spouštěcí, vyberte **dokončení** složku kořenové a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-125">Locate the path where you cloned the Spring Boot repo, select the **complete** folder under the root, and then click **OK**.</span></span>

   ![Vyberte složku pro import][CL06]

1. <span data-ttu-id="f9d61-127">Když se zobrazí výzva, vyberte **vytvoření projektu z existujících zdrojů**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-127">When you're prompted, select **Create project from existing sources**.</span></span>

   ![Možnost vytváření projektů z existujícího zdroje][CL07]

1. <span data-ttu-id="f9d61-129">Zadejte název projektu nebo přijměte výchozí nastavení, ověřte správnou cestu **dokončení** složku a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-129">Specify your project name or accept the default, verify the correct path to the **complete** folder, and then click **Next**.</span></span>

   ![Zadejte název projektu][CL08]

1. <span data-ttu-id="f9d61-131">Přizpůsobit všechny adresáře pro import a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-131">Customize any directories for importing, and then click **Next**.</span></span>

   ![Zvolte adresáře][CL09]

1. <span data-ttu-id="f9d61-133">Zkontrolujte knihoven importovat, a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-133">Review the libraries to import, and then click **Next**.</span></span>

   ![Zkontrolujte projektu knihovny][CL10]

1. <span data-ttu-id="f9d61-135">Zkontrolujte struktura modulu a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-135">Review the module structure, and then click **Next**.</span></span>

   ![Zkontrolujte struktura modulu][CL11]

1. <span data-ttu-id="f9d61-137">Zadejte vaše JDK a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-137">Specify your JDK, and then click **Next**.</span></span>

   ![Zadejte JDK][CL12]

1. <span data-ttu-id="f9d61-139">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-139">Click **Finish**.</span></span>

   ![Tlačítko Dokončit][CL13]

<span data-ttu-id="f9d61-141">IntelliJ importuje pružiny spouštěcí aplikace jako projekt a zobrazí strukturu po dokončení importu.</span><span class="sxs-lookup"><span data-stu-id="f9d61-141">IntelliJ imports the Spring Boot app as a project and displays the structure when the import has finished.</span></span>

![Spuštění aplikace Spring v IntelliJ][CL14]

## <a name="build-your-spring-boot-app"></a><span data-ttu-id="f9d61-143">Sestavení vaší pružiny spouštěcí aplikace</span><span class="sxs-lookup"><span data-stu-id="f9d61-143">Build your Spring Boot app</span></span>

### <a name="build-the-app-by-using-the-maven-pom"></a><span data-ttu-id="f9d61-144">Sestavení aplikace pomocí Maven POM</span><span class="sxs-lookup"><span data-stu-id="f9d61-144">Build the app by using the Maven POM</span></span>

1. <span data-ttu-id="f9d61-145">Pokud už není otevřený, otevřete okno nástroje Maven.</span><span class="sxs-lookup"><span data-stu-id="f9d61-145">Open the Maven tool window if it is not already opened.</span></span> <span data-ttu-id="f9d61-146">Klikněte na tlačítko **zobrazení** > **nástroj Windows** > **projekty Maven**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-146">Click **View** > **Tool Windows** > **Maven Projects**.</span></span>

   ![Nástroje systému Windows a projekty Maven příkazy][BU01]

1. <span data-ttu-id="f9d61-148">V okně nástroje Maven, klikněte pravým tlačítkem na **balíček** a vyberte **spustit Build Maven**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-148">In the Maven tool window, right-click **package** and select **Run Maven Build**.</span></span> <span data-ttu-id="f9d61-149">(Pokud projekt Maven nezobrazí automaticky, klikněte **znovu naimportovat** ikonu na panelu nástrojů Maven.)</span><span class="sxs-lookup"><span data-stu-id="f9d61-149">(If your Maven project does not show up automatically, click the **Reimport** icon on the Maven toolbar.)</span></span>

   ![Spusťte příkaz Maven Build][BU02]

1. <span data-ttu-id="f9d61-151">IntelliJ by se měl zobrazit **ÚSPĚŠNOSTI sestavení** zprávy při pružiny spouštěcí aplikace je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="f9d61-151">IntelliJ should display a **BUILD SUCCESS** message when your Spring Boot app is successfully created.</span></span>

   ![Zpráva ÚSPĚŠNOSTI sestavení][BU03]

### <a name="create-a-deployment-ready-artifact"></a><span data-ttu-id="f9d61-153">Vytvoření artefaktu připravené pro nasazení</span><span class="sxs-lookup"><span data-stu-id="f9d61-153">Create a deployment-ready artifact</span></span>

<span data-ttu-id="f9d61-154">K publikování aplikace pružiny spouštěcí, musíte vytvořit artefakt připravené pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="f9d61-154">To publish your Spring Boot app, you need to create a deployment-ready artifact.</span></span> <span data-ttu-id="f9d61-155">Použijte k tomu následující postup:</span><span class="sxs-lookup"><span data-stu-id="f9d61-155">Use the following steps:</span></span>

1. <span data-ttu-id="f9d61-156">Otevřete projekt webové aplikace v IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="f9d61-156">Open your web app project in IntelliJ.</span></span>

1. <span data-ttu-id="f9d61-157">Klikněte na tlačítko **soubor**a potom klikněte na **strukturu projektu**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-157">Click **File**, and then click **Project Structure**.</span></span>

   ![Struktura projekt – příkaz][ART01]

1. <span data-ttu-id="f9d61-159">Klikněte na zelenou plus (**+**) symbol, který chcete přidat artefakt, klikněte na tlačítko **JAR**a potom klikněte na **prázdný**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-159">Click the green plus (**+**) symbol to add an artifact, click **JAR**, and then click **Empty**.</span></span>

   ![Přidat artefakt][ART02]

1. <span data-ttu-id="f9d61-161">Zadejte název vaší artefaktů při vytváření nezapomeňte přidat příponu ".jar" a pak zadejte cílovou složku pro výstupní Maven.</span><span class="sxs-lookup"><span data-stu-id="f9d61-161">Name your artifact while making sure not to add the ".jar" extension, and then specify the target folder for the Maven output.</span></span>

   ![Zadejte vlastnosti artefaktů][ART03]

1. <span data-ttu-id="f9d61-163">Vytvoření manifestu pro vaše artefaktů (volitelné):</span><span class="sxs-lookup"><span data-stu-id="f9d61-163">Create a manifest for your artifact (optional):</span></span>

   <span data-ttu-id="f9d61-164">a.</span><span class="sxs-lookup"><span data-stu-id="f9d61-164">a.</span></span> <span data-ttu-id="f9d61-165">Klikněte na tlačítko **vytvoření manifestu**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-165">Click **Create Manifest**.</span></span>

      ![Klikněte na tlačítko Vytvořit Manifest][ART04a]

   <span data-ttu-id="f9d61-167">b.</span><span class="sxs-lookup"><span data-stu-id="f9d61-167">b.</span></span> <span data-ttu-id="f9d61-168">Výchozí cesta pro artefaktu a potom na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-168">Choose the default path for the artifact, and then click **OK**.</span></span>

      ![Zadejte cestu artefaktů][ART04b]

   <span data-ttu-id="f9d61-170">c.</span><span class="sxs-lookup"><span data-stu-id="f9d61-170">c.</span></span> <span data-ttu-id="f9d61-171">Klikněte na tlačítko se třemi tečkami (**...** ) najít hlavní třídy.</span><span class="sxs-lookup"><span data-stu-id="f9d61-171">Click the ellipsis (**...**) to locate the main class.</span></span>

      ![Vyhledejte hlavní – třída][ART04c]

   <span data-ttu-id="f9d61-173">d.</span><span class="sxs-lookup"><span data-stu-id="f9d61-173">d.</span></span> <span data-ttu-id="f9d61-174">Vyberte hlavní třídy a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-174">Choose your main class, and then click **OK**.</span></span>

      ![Zadejte hlavní – třída][ART04d]

1. <span data-ttu-id="f9d61-176">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-176">Click **OK**.</span></span>

   ![Zavřete dialogové okno struktura projektu][ART05]

> [!NOTE]
> <span data-ttu-id="f9d61-178">Další informace o vytváření artefakty v IntelliJ najdete v tématu [konfigurace artefakty] na webu JetBrains.</span><span class="sxs-lookup"><span data-stu-id="f9d61-178">For more information about creating artifacts in IntelliJ, see [Configuring Artifacts] on the JetBrains website.</span></span>
>

### <a name="build-the-artifact-for-deployment"></a><span data-ttu-id="f9d61-179">Sestavení artefaktů pro nasazení</span><span class="sxs-lookup"><span data-stu-id="f9d61-179">Build the artifact for deployment</span></span>

1. <span data-ttu-id="f9d61-180">Klikněte na tlačítko **sestavení**a potom klikněte na **artefakty**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-180">Click **Build**, and then click **Artifacts**.</span></span>

   ![Příkaz artefaktů sestavení][BU04]

1. <span data-ttu-id="f9d61-182">Když **sestavení artefaktů** místní nabídce klikněte na tlačítko **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-182">When the **Build Artifact** context menu appears, click **Build**.</span></span>

   ![Sestavení artefaktů kontextové nabídky][BU05]

<span data-ttu-id="f9d61-184">IntelliJ by měl zobrazit dokončené artefakt Spring spuštění aplikace v okně nástroje projektu.</span><span class="sxs-lookup"><span data-stu-id="f9d61-184">IntelliJ should display the completed artifact for your Spring Boot app in the project tool window.</span></span>

   ![Vytvořený artefaktů][BU06]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="f9d61-186">Publikování webové aplikace do Azure pomocí kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="f9d61-186">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="f9d61-187">Pokud nejste přihlášeni k účtu Azure, postupujte podle kroků v [přihlášení pokyny pro Azure Toolkit IntelliJ][Azure Sign In for IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="f9d61-187">If you have not signed in to your Azure account, follow the steps in [Sign-in instructions for the Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span></span>

1. <span data-ttu-id="f9d61-188">V okně nástroje prohlížeči projektu klikněte pravým tlačítkem na projekt a potom vyberte **Azure** > **publikovat jako kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-188">In the Project Explorer tool window, right-click the project, and then select **Azure** > **Publish as Docker Container**.</span></span>

   ![Publikovat jako příkaz kontejner Docker][PU01]

1. <span data-ttu-id="f9d61-190">Když **nasadit kontejner Docker v Azure** se zobrazí dialogové okno, jsou zobrazeny všechny existující hostitelů Docker.</span><span class="sxs-lookup"><span data-stu-id="f9d61-190">When the **Deploy Docker Container on Azure** dialog box appears, any existing Docker hosts are displayed.</span></span> <span data-ttu-id="f9d61-191">Pokud se rozhodnete nasadit do existující hostitele, můžete přeskočit ke kroku 4.</span><span class="sxs-lookup"><span data-stu-id="f9d61-191">If you choose to deploy to an existing host, you can skip to step 4.</span></span> <span data-ttu-id="f9d61-192">Jinak použijte následující kroky pro vytvoření hostitele:</span><span class="sxs-lookup"><span data-stu-id="f9d61-192">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="f9d61-193">a.</span><span class="sxs-lookup"><span data-stu-id="f9d61-193">a.</span></span> <span data-ttu-id="f9d61-194">Klikněte na zelenou plus (**+**) symbol.</span><span class="sxs-lookup"><span data-stu-id="f9d61-194">Click the green plus (**+**) symbol.</span></span>

      ![Přidat nový hostitel Docker][PU02]

   <span data-ttu-id="f9d61-196">b.</span><span class="sxs-lookup"><span data-stu-id="f9d61-196">b.</span></span> <span data-ttu-id="f9d61-197">Když **vytvořit hostitelů Docker** zobrazí se dialogové okno, můžete přijmout výchozí hodnoty nebo můžete zadat vlastní nastavení pro nové Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="f9d61-197">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="f9d61-198">(Podrobný popis různá nastavení, najdete v části [publikovat webovou aplikaci jako kontejner Docker pomocí nástrojů Azure pro IntelliJ][Publish Container with Azure Toolkit].) Klikněte na tlačítko **Další** jste zadali při nastavení, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="f9d61-198">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Zadejte možnosti hostitelů Docker][PU03a]

   <span data-ttu-id="f9d61-200">c.</span><span class="sxs-lookup"><span data-stu-id="f9d61-200">c.</span></span> <span data-ttu-id="f9d61-201">Můžete použít existující přihlašovací údaje z trezoru služby Azure klíče, nebo můžete zadat nový Docker přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f9d61-201">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="f9d61-202">Klikněte na tlačítko **Dokončit** když nastavíte možnosti.</span><span class="sxs-lookup"><span data-stu-id="f9d61-202">Click **Finish** when you have specified your options.</span></span>

      ![Zadejte přihlašovací údaje hostitelů Docker][PU03b]

1. <span data-ttu-id="f9d61-204">Vyberte Docker hostiteli a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-204">Select your Docker host, and then click **Next**.</span></span>

   ![Vyberte hostitel Docker používat][PU04]

1. <span data-ttu-id="f9d61-206">Na poslední stránce **nasadit kontejner Docker v Azure** dialogové okno pole, určete následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="f9d61-206">On the last page of the **Deploy Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="f9d61-207">a.</span><span class="sxs-lookup"><span data-stu-id="f9d61-207">a.</span></span> <span data-ttu-id="f9d61-208">Můžete zadat vlastní název kontejneru, který bude hostovat vaše kontejner Docker nebo můžete přijmout výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="f9d61-208">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="f9d61-209">b.</span><span class="sxs-lookup"><span data-stu-id="f9d61-209">b.</span></span> <span data-ttu-id="f9d61-210">Zadejte porty protokolu TCP pro svého hostitele docker pomocí následující syntaxe: *[externí port]*:*[interní port]*.</span><span class="sxs-lookup"><span data-stu-id="f9d61-210">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="f9d61-211">Například **80:8080** určuje externí port 80 a výchozí vnitřní pružiny spouštěcí port 8080.</span><span class="sxs-lookup"><span data-stu-id="f9d61-211">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="f9d61-212">Pokud jste upravili interní port (například úpravou souboru application.yml), je třeba zadat číslo portu pro správné směrování v Azure.</span><span class="sxs-lookup"><span data-stu-id="f9d61-212">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="f9d61-213">c.</span><span class="sxs-lookup"><span data-stu-id="f9d61-213">c.</span></span> <span data-ttu-id="f9d61-214">Po nakonfigurování těchto možností, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f9d61-214">After you have configured these options, click **Finish**.</span></span>

   ![Nasadit kontejner Docker v Azure][PU05]

1. <span data-ttu-id="f9d61-216">Po dokončení publikování sady nástrojů Azure protokol činnosti Azure zobrazí **publikováno** stavu.</span><span class="sxs-lookup"><span data-stu-id="f9d61-216">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![Byla úspěšně nasazena hostitelů Docker][PU06]

## <a name="next-steps"></a><span data-ttu-id="f9d61-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f9d61-218">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="f9d61-219">Další informace o další metody pro vytváření aplikací pružiny spouštěcí pomocí IntelliJ najdete v tématu [vytváření projektů spouštěcí pružiny](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) na webu JetBrains.</span><span class="sxs-lookup"><span data-stu-id="f9d61-219">To learn about additional methods for creating Spring Boot apps by using IntelliJ, see [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) on the JetBrains website.</span></span>

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
<span data-ttu-id="f9d61-220">[konfigurace artefakty]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span><span class="sxs-lookup"><span data-stu-id="f9d61-220">[Configuring Artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span></span>
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
<span data-ttu-id="f9d61-221">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="f9d61-221">[Docker]: https://www.docker.com/</span></span>
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
<span data-ttu-id="f9d61-222">[pružiny spouštěcí]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="f9d61-222">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="f9d61-223">[Pružiny Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="f9d61-223">[Spring Framework]: https://spring.io/</span></span>

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
