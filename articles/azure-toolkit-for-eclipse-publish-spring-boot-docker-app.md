---
title: "Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí nástrojů Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak publikovat webovou aplikaci do služby Microsoft Azure jako kontejner Docker pomocí nástrojů Azure pro Eclipse."
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
ms.openlocfilehash: fcb60fcfbda26f5f37bfb0edcb01f8737188b6bc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="254bc-103">Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí nástrojů Azure pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="254bc-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="254bc-104">[Pružiny Framework] open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="254bc-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="254bc-105">Jedním z dalších oblíbených projektů, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="254bc-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="254bc-106">[Docker] je do řešení open source, který pomáhá vývojářům automatizovat nasazení, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="254bc-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="254bc-107">Tento kurz vás provede kroky nasazení spouštěcí pružiny aplikace jako kontejner Docker do služby Microsoft Azure pomocí sady nástrojů Azure pro Eclipse.</span><span class="sxs-lookup"><span data-stu-id="254bc-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a><span data-ttu-id="254bc-108">Naklonujte úložiště Docker spouštěcí pružiny výchozí</span><span class="sxs-lookup"><span data-stu-id="254bc-108">Clone the default Spring Boot Docker repository</span></span>

### <a name="import-the-public-repository"></a><span data-ttu-id="254bc-109">Import veřejného úložiště</span><span class="sxs-lookup"><span data-stu-id="254bc-109">Import the public repository</span></span>

<span data-ttu-id="254bc-110">Následující postup vás provede procesem klonování úložiště Docker spouštěcí pružiny do místního počítače pomocí IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="254bc-110">The following steps walk you through cloning the Spring Boot Docker repository to your local computer by using IntelliJ.</span></span> <span data-ttu-id="254bc-111">Pokud chcete použít příkazový řádek, přečtěte si téma [nasazení pružiny spuštění aplikace v systému Linux v Azure Container Service][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="254bc-111">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="254bc-112">Otevřete prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="254bc-112">Open Eclipse.</span></span>

1. <span data-ttu-id="254bc-113">Klikněte na tlačítko **soubor** > **Import**.</span><span class="sxs-lookup"><span data-stu-id="254bc-113">Click **File** > **Import**.</span></span>

   ![Nabídka Soubor importu][CL01]

1. <span data-ttu-id="254bc-115">Když **Import** otevře se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="254bc-115">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="254bc-116">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-116">a.</span></span> <span data-ttu-id="254bc-117">Rozbalte položku **Git**.</span><span class="sxs-lookup"><span data-stu-id="254bc-117">Expand **Git**.</span></span>

   <span data-ttu-id="254bc-118">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-118">b.</span></span> <span data-ttu-id="254bc-119">Vyberte **projekty z Gitu**.</span><span class="sxs-lookup"><span data-stu-id="254bc-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="254bc-120">c.</span><span class="sxs-lookup"><span data-stu-id="254bc-120">c.</span></span> <span data-ttu-id="254bc-121">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="254bc-121">Click **Next**.</span></span>

   ![Dialog importovat][CL02]

1. <span data-ttu-id="254bc-123">Na **vybrat zdroj úložiště** stránky:</span><span class="sxs-lookup"><span data-stu-id="254bc-123">On the **Select Repository Source** page:</span></span>

   <span data-ttu-id="254bc-124">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-124">a.</span></span> <span data-ttu-id="254bc-125">Vyberte **klonovat URI**.</span><span class="sxs-lookup"><span data-stu-id="254bc-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="254bc-126">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-126">b.</span></span> <span data-ttu-id="254bc-127">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="254bc-127">Click **Next**.</span></span>

   ![Vyberte zdrojové úložiště stránky][CL03]

1. <span data-ttu-id="254bc-129">Na **zdrojové úložiště Git** stránky:</span><span class="sxs-lookup"><span data-stu-id="254bc-129">On the **Source Git Repository** page:</span></span>

   <span data-ttu-id="254bc-130">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-130">a.</span></span> <span data-ttu-id="254bc-131">Pro **URI**, zadejte `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span><span class="sxs-lookup"><span data-stu-id="254bc-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="254bc-132">Tento krok by měl automaticky vyplnit **hostitele** a **úložiště cesta** pole s správné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="254bc-132">This step should automatically populate the **Host** and **Repository path** fields with the correct values.</span></span>
   
   <span data-ttu-id="254bc-133">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-133">b.</span></span> <span data-ttu-id="254bc-134">Spouštěcí pružiny úložiště je veřejný, proto by nemělo být zadejte Git uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="254bc-134">The Spring Boot repository is public, so you should not have to enter your Git username and password.</span></span>
   
   <span data-ttu-id="254bc-135">c.</span><span class="sxs-lookup"><span data-stu-id="254bc-135">c.</span></span> <span data-ttu-id="254bc-136">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="254bc-136">Click **Next**.</span></span>

   ![Zdrojové úložiště Git stránky][CL04]

1. <span data-ttu-id="254bc-138">Na **výběr větve** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="254bc-138">On the **Branch Selection** page, click **Next**.</span></span>

   ![Stránka Výběr větve][CL05]

1. <span data-ttu-id="254bc-140">Na **místní cílovou** stránky:</span><span class="sxs-lookup"><span data-stu-id="254bc-140">On the **Local Destination** page:</span></span>

   <span data-ttu-id="254bc-141">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-141">a.</span></span> <span data-ttu-id="254bc-142">Zadejte místní složku, kam chcete vaše místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="254bc-142">Specify the local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="254bc-143">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-143">b.</span></span> <span data-ttu-id="254bc-144">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="254bc-144">Click **Next**.</span></span>

   ![Místní cílová stránka][CL06]

1. <span data-ttu-id="254bc-146">Na **vyberte Průvodce pro import projektů** stránky:</span><span class="sxs-lookup"><span data-stu-id="254bc-146">On the **Select a wizard to use for importing projects** page:</span></span>

   <span data-ttu-id="254bc-147">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-147">a.</span></span> <span data-ttu-id="254bc-148">Vyberte **Import jako obecné projekt**.</span><span class="sxs-lookup"><span data-stu-id="254bc-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="254bc-149">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-149">b.</span></span> <span data-ttu-id="254bc-150">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="254bc-150">Click **Next**.</span></span>

   !["Vyberte průvodce pro import projektů" stránky][CL07]

1. <span data-ttu-id="254bc-152">Na **Import projektů** stránky:</span><span class="sxs-lookup"><span data-stu-id="254bc-152">On the **Import Projects** page:</span></span>

   <span data-ttu-id="254bc-153">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-153">a.</span></span> <span data-ttu-id="254bc-154">Zadejte název projektu.</span><span class="sxs-lookup"><span data-stu-id="254bc-154">Specify your project name.</span></span>
   
   <span data-ttu-id="254bc-155">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-155">b.</span></span> <span data-ttu-id="254bc-156">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="254bc-156">Click **Finish**.</span></span>

   ![Import projektů stránky][CL08]

1. <span data-ttu-id="254bc-158">Pokud úložiště je úspěšně klonovat, zobrazí se všechny soubory, které jsou uvedené v Eclipse.</span><span class="sxs-lookup"><span data-stu-id="254bc-158">When the repository is cloned successfully, you see all the files listed in Eclipse.</span></span>

   ![Místní úložiště][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="254bc-160">Vytvořte projekt Maven z místního úložiště</span><span class="sxs-lookup"><span data-stu-id="254bc-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="254bc-161">Úložiště Docker spouštěcí pružiny obsahuje dokončený projekt Maven, které budete používat pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="254bc-161">The Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="254bc-162">Klikněte na tlačítko **soubor** > **Import**.</span><span class="sxs-lookup"><span data-stu-id="254bc-162">Click **File** > **Import**.</span></span>

   ![Příkaz Import v nabídce Soubor][CL01]

1. <span data-ttu-id="254bc-164">Když **Import** otevře se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="254bc-164">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="254bc-165">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-165">a.</span></span> <span data-ttu-id="254bc-166">Rozbalte položku **Maven**.</span><span class="sxs-lookup"><span data-stu-id="254bc-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="254bc-167">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-167">b.</span></span> <span data-ttu-id="254bc-168">Vyberte **existující projekty Maven**.</span><span class="sxs-lookup"><span data-stu-id="254bc-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="254bc-169">c.</span><span class="sxs-lookup"><span data-stu-id="254bc-169">c.</span></span> <span data-ttu-id="254bc-170">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="254bc-170">Click **Next**.</span></span>

   ![Dialog importovat][MV01]

1. <span data-ttu-id="254bc-172">Na **projekty Maven** stránky:</span><span class="sxs-lookup"><span data-stu-id="254bc-172">On the **Maven Projects** page:</span></span>

   <span data-ttu-id="254bc-173">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-173">a.</span></span> <span data-ttu-id="254bc-174">Pro **kořenový adresář**, zadejte **dokončení** složky v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="254bc-174">For **Root Directory**, specify the **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="254bc-175">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-175">b.</span></span> <span data-ttu-id="254bc-176">Rozbalte **Upřesnit** a zadejte vlastní název pro **šablony názvu**.</span><span class="sxs-lookup"><span data-stu-id="254bc-176">Expand the **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="254bc-177">c.</span><span class="sxs-lookup"><span data-stu-id="254bc-177">c.</span></span> <span data-ttu-id="254bc-178">Vyberte pole **pom.xml** v projektu.</span><span class="sxs-lookup"><span data-stu-id="254bc-178">Select the box for the **pom.xml** file in the project.</span></span>
   
   <span data-ttu-id="254bc-179">d.</span><span class="sxs-lookup"><span data-stu-id="254bc-179">d.</span></span> <span data-ttu-id="254bc-180">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="254bc-180">Click **Finish**.</span></span>

   ![Stránka projekty maven][MV02]

1. <span data-ttu-id="254bc-182">Pokud projekt Maven byl úspěšně otevřen, uvidíte druhý projekt uvedené v Eclipse.</span><span class="sxs-lookup"><span data-stu-id="254bc-182">When the Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![Místní projekt Maven][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="254bc-184">Sestavení aplikace pružiny spouštěcí pomocí Maven</span><span class="sxs-lookup"><span data-stu-id="254bc-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="254bc-185">V prohlížeči projektu Eclipse vyberte projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="254bc-185">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="254bc-186">Klikněte na tlačítko **spustit** > **spustit jako** > **sestavení Maven**.</span><span class="sxs-lookup"><span data-stu-id="254bc-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![Příkazy ke spuštění jako Maven sestavení][BU01]

1. <span data-ttu-id="254bc-188">Pokud vaše aplikace je úspěšně vytvořen, v okně konzoly zobrazuje stav.</span><span class="sxs-lookup"><span data-stu-id="254bc-188">When your application is successfully built, the console window shows the status.</span></span>

   ![Úspěšný build Maven][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="254bc-190">Publikování webové aplikace do Azure pomocí kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="254bc-190">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="254bc-191">V prohlížeči projektu Eclipse vyberte projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="254bc-191">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="254bc-192">Klikněte na tlačítko Azure **publikovat** nabídce a pak klikněte na tlačítko **publikovat jako kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="254bc-192">Click the Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![Publikovat jako příkaz kontejner Docker][PU01]

1. <span data-ttu-id="254bc-194">Když **nasazení kontejner Docker v Azure** zobrazí se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="254bc-194">When the **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="254bc-195">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-195">a.</span></span> <span data-ttu-id="254bc-196">Zadejte název vlastního Docker bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="254bc-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="254bc-197">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-197">b.</span></span> <span data-ttu-id="254bc-198">Pro **artefaktů nasazení**, zadejte cestu k **gs pružiny spouštěcí docker-0.1.0.jar** souborů, které jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="254bc-198">For **Artifact to deploy**, specify the path to the **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![Zadejte možnosti Docker][PU02]

   <span data-ttu-id="254bc-200">Zobrazí se všechny existující hostitelů Docker.</span><span class="sxs-lookup"><span data-stu-id="254bc-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="254bc-201">Pokud se rozhodnete nasadit do existující hostitele, můžete přeskočit ke kroku 5.</span><span class="sxs-lookup"><span data-stu-id="254bc-201">If you choose to deploy to an existing host, you can skip to step 5.</span></span> <span data-ttu-id="254bc-202">Jinak použijte následující kroky pro vytvoření hostitele:</span><span class="sxs-lookup"><span data-stu-id="254bc-202">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="254bc-203">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-203">a.</span></span> <span data-ttu-id="254bc-204">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="254bc-204">Click **Add**.</span></span>

      ![Přidat nový hostitel Docker][PU03]

   <span data-ttu-id="254bc-206">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-206">b.</span></span> <span data-ttu-id="254bc-207">Když **vytvořit hostitelů Docker** zobrazí se dialogové okno, můžete přijmout výchozí hodnoty nebo můžete zadat vlastní nastavení pro nové Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="254bc-207">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="254bc-208">(Podrobný popis různá nastavení, najdete v části [publikovat webovou aplikaci jako kontejner Docker pomocí nástrojů Azure pro IntelliJ][Publish Container with Azure Toolkit].) Klikněte na tlačítko **Další** jste zadali při nastavení, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="254bc-208">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Zadejte možnosti hostitelů Docker][PU04]

   <span data-ttu-id="254bc-210">c.</span><span class="sxs-lookup"><span data-stu-id="254bc-210">c.</span></span> <span data-ttu-id="254bc-211">Můžete použít existující přihlašovací údaje z trezoru služby Azure klíče, nebo můžete zadat nový Docker přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="254bc-211">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="254bc-212">Klikněte na tlačítko **Dokončit** když nastavíte možnosti.</span><span class="sxs-lookup"><span data-stu-id="254bc-212">Click **Finish** when you have specified your options.</span></span>

      ![Zadejte přihlašovací údaje hostitelů Docker][PU05]

1. <span data-ttu-id="254bc-214">Vyberte Docker hostiteli a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="254bc-214">Select your Docker host, and then click **Next**.</span></span>

   ![Vyberte hostitele Docker používat][PU06]

1. <span data-ttu-id="254bc-216">Na poslední stránce **nasazení kontejner Docker v Azure** dialogové okno pole, určete následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="254bc-216">On the last page of the **Deploying Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="254bc-217">a.</span><span class="sxs-lookup"><span data-stu-id="254bc-217">a.</span></span> <span data-ttu-id="254bc-218">Můžete zadat vlastní název kontejneru, který bude hostovat vaše kontejner Docker nebo můžete přijmout výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="254bc-218">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="254bc-219">b.</span><span class="sxs-lookup"><span data-stu-id="254bc-219">b.</span></span> <span data-ttu-id="254bc-220">Zadejte porty protokolu TCP pro svého hostitele docker pomocí následující syntaxe: *[externí port]*:*[interní port]*.</span><span class="sxs-lookup"><span data-stu-id="254bc-220">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="254bc-221">Například **80:8080** určuje externí port 80 a výchozí vnitřní pružiny spouštěcí port 8080.</span><span class="sxs-lookup"><span data-stu-id="254bc-221">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="254bc-222">Pokud jste upravili interní port (například úpravou souboru application.yml), je třeba zadat číslo portu pro správné směrování v Azure.</span><span class="sxs-lookup"><span data-stu-id="254bc-222">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="254bc-223">c.</span><span class="sxs-lookup"><span data-stu-id="254bc-223">c.</span></span> <span data-ttu-id="254bc-224">Když nakonfigurujete tyto možnosti, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="254bc-224">After you configure these options, click **Finish**.</span></span>

   ![Nasadit kontejner Docker v Azure][PU07]

1. <span data-ttu-id="254bc-226">Po dokončení publikování sady nástrojů Azure protokol činnosti Azure zobrazí **publikováno** stavu.</span><span class="sxs-lookup"><span data-stu-id="254bc-226">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![Byla úspěšně nasazena hostitelů Docker][PU08]

## <a name="next-steps"></a><span data-ttu-id="254bc-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="254bc-228">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
<span data-ttu-id="254bc-229">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="254bc-229">[Docker]: https://www.docker.com/</span></span>
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
<span data-ttu-id="254bc-230">[pružiny spouštěcí]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="254bc-230">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="254bc-231">[Pružiny Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="254bc-231">[Spring Framework]: https://spring.io/</span></span>

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png
