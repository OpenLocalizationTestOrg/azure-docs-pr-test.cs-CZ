---
title: "hello aaaPublish pružiny spouštěcí aplikace jako kontejner Docker pomocí nástrojů Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak hello toopublish tooMicrosoft webové aplikace Azure jako kontejner Docker pomocí nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 29390c49c339a1ebb87cb3951b21cea01c0da15f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="2e183-103">Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí hello Azure Toolkit pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="2e183-103">Publish a Spring Boot app as a Docker container by using hello Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="2e183-104">Hello [pružiny Framework] open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="2e183-104">hello [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="2e183-105">Jeden z hello oblíbených další projekty, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="2e183-105">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="2e183-106">[Docker] je do řešení open source, který pomáhá vývojářům automatizovat nasazení hello, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="2e183-106">[Docker] is an open-source solution that helps developers automate hello deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="2e183-107">Tento kurz vás provede kroky toodeploy hello pružiny spouštěcí aplikace jako tooMicrosoft kontejner Docker Azure pomocí hello Azure Toolkit pro Eclipse.</span><span class="sxs-lookup"><span data-stu-id="2e183-107">This tutorial walks you through hello steps toodeploy a Spring Boot application as a Docker container tooMicrosoft Azure by using hello Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repository"></a><span data-ttu-id="2e183-108">Klonování úložiště Docker spouštěcí pružiny výchozí hello</span><span class="sxs-lookup"><span data-stu-id="2e183-108">Clone hello default Spring Boot Docker repository</span></span>

### <a name="import-hello-public-repository"></a><span data-ttu-id="2e183-109">Import hello veřejného úložiště</span><span class="sxs-lookup"><span data-stu-id="2e183-109">Import hello public repository</span></span>

<span data-ttu-id="2e183-110">Hello následující postup vás provede procesem klonování hello pružiny spouštěcí Docker úložiště tooyour místního počítače pomocí IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="2e183-110">hello following steps walk you through cloning hello Spring Boot Docker repository tooyour local computer by using IntelliJ.</span></span> <span data-ttu-id="2e183-111">Pokud chcete toouse příkazového řádku, najdete v části [nasazení pružiny spuštění aplikace v systému Linux v Azure Container Service][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="2e183-111">If you want toouse a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="2e183-112">Otevřete prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="2e183-112">Open Eclipse.</span></span>

1. <span data-ttu-id="2e183-113">Klikněte na tlačítko **soubor** > **Import**.</span><span class="sxs-lookup"><span data-stu-id="2e183-113">Click **File** > **Import**.</span></span>

   ![Nabídka Soubor importu][CL01]

1. <span data-ttu-id="2e183-115">Když hello **Import** otevře se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="2e183-115">When hello **Import** dialog box opens:</span></span>

   <span data-ttu-id="2e183-116">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-116">a.</span></span> <span data-ttu-id="2e183-117">Rozbalte položku **Git**.</span><span class="sxs-lookup"><span data-stu-id="2e183-117">Expand **Git**.</span></span>

   <span data-ttu-id="2e183-118">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-118">b.</span></span> <span data-ttu-id="2e183-119">Vyberte **projekty z Gitu**.</span><span class="sxs-lookup"><span data-stu-id="2e183-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="2e183-120">c.</span><span class="sxs-lookup"><span data-stu-id="2e183-120">c.</span></span> <span data-ttu-id="2e183-121">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2e183-121">Click **Next**.</span></span>

   ![Dialog importovat][CL02]

1. <span data-ttu-id="2e183-123">Na hello **vybrat zdroj úložiště** stránky:</span><span class="sxs-lookup"><span data-stu-id="2e183-123">On hello **Select Repository Source** page:</span></span>

   <span data-ttu-id="2e183-124">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-124">a.</span></span> <span data-ttu-id="2e183-125">Vyberte **klonovat URI**.</span><span class="sxs-lookup"><span data-stu-id="2e183-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="2e183-126">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-126">b.</span></span> <span data-ttu-id="2e183-127">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2e183-127">Click **Next**.</span></span>

   ![Vyberte zdrojové úložiště stránky][CL03]

1. <span data-ttu-id="2e183-129">Na hello **zdrojové úložiště Git** stránky:</span><span class="sxs-lookup"><span data-stu-id="2e183-129">On hello **Source Git Repository** page:</span></span>

   <span data-ttu-id="2e183-130">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-130">a.</span></span> <span data-ttu-id="2e183-131">Pro **URI**, zadejte `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span><span class="sxs-lookup"><span data-stu-id="2e183-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="2e183-132">Tento krok by měl automaticky vyplnit hello **hostitele** a **úložiště cesta** pole s hello opravte hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2e183-132">This step should automatically populate hello **Host** and **Repository path** fields with hello correct values.</span></span>
   
   <span data-ttu-id="2e183-133">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-133">b.</span></span> <span data-ttu-id="2e183-134">Hello pružiny spouštěcí úložiště je veřejný, takže by neměla mít tooenter Git uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="2e183-134">hello Spring Boot repository is public, so you should not have tooenter your Git username and password.</span></span>
   
   <span data-ttu-id="2e183-135">c.</span><span class="sxs-lookup"><span data-stu-id="2e183-135">c.</span></span> <span data-ttu-id="2e183-136">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2e183-136">Click **Next**.</span></span>

   ![Zdrojové úložiště Git stránky][CL04]

1. <span data-ttu-id="2e183-138">Na hello **výběr větve** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2e183-138">On hello **Branch Selection** page, click **Next**.</span></span>

   ![Stránka Výběr větve][CL05]

1. <span data-ttu-id="2e183-140">Na hello **místní cílovou** stránky:</span><span class="sxs-lookup"><span data-stu-id="2e183-140">On hello **Local Destination** page:</span></span>

   <span data-ttu-id="2e183-141">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-141">a.</span></span> <span data-ttu-id="2e183-142">Zadejte hello místní složku, kam chcete vaše místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e183-142">Specify hello local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="2e183-143">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-143">b.</span></span> <span data-ttu-id="2e183-144">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2e183-144">Click **Next**.</span></span>

   ![Místní cílová stránka][CL06]

1. <span data-ttu-id="2e183-146">Na hello **vyberte toouse Průvodce pro import projektů** stránky:</span><span class="sxs-lookup"><span data-stu-id="2e183-146">On hello **Select a wizard toouse for importing projects** page:</span></span>

   <span data-ttu-id="2e183-147">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-147">a.</span></span> <span data-ttu-id="2e183-148">Vyberte **Import jako obecné projekt**.</span><span class="sxs-lookup"><span data-stu-id="2e183-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="2e183-149">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-149">b.</span></span> <span data-ttu-id="2e183-150">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2e183-150">Click **Next**.</span></span>

   !["Vyberte toouse Průvodce pro import projektů" stránky][CL07]

1. <span data-ttu-id="2e183-152">Na hello **Import projektů** stránky:</span><span class="sxs-lookup"><span data-stu-id="2e183-152">On hello **Import Projects** page:</span></span>

   <span data-ttu-id="2e183-153">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-153">a.</span></span> <span data-ttu-id="2e183-154">Zadejte název projektu.</span><span class="sxs-lookup"><span data-stu-id="2e183-154">Specify your project name.</span></span>
   
   <span data-ttu-id="2e183-155">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-155">b.</span></span> <span data-ttu-id="2e183-156">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="2e183-156">Click **Finish**.</span></span>

   ![Import projektů stránky][CL08]

1. <span data-ttu-id="2e183-158">Pokud hello úložiště se úspěšně klonovat, uvidíte všechny soubory hello uvedené v Eclipse.</span><span class="sxs-lookup"><span data-stu-id="2e183-158">When hello repository is cloned successfully, you see all hello files listed in Eclipse.</span></span>

   ![Místní úložiště][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="2e183-160">Vytvořte projekt Maven z místního úložiště</span><span class="sxs-lookup"><span data-stu-id="2e183-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="2e183-161">úložiště Docker spouštěcí pružiny Hello obsahuje dokončený projekt Maven, které budete používat pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2e183-161">hello Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="2e183-162">Klikněte na tlačítko **soubor** > **Import**.</span><span class="sxs-lookup"><span data-stu-id="2e183-162">Click **File** > **Import**.</span></span>

   ![Příkaz v nabídce Soubor hello import][CL01]

1. <span data-ttu-id="2e183-164">Když hello **Import** otevře se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="2e183-164">When hello **Import** dialog box opens:</span></span>

   <span data-ttu-id="2e183-165">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-165">a.</span></span> <span data-ttu-id="2e183-166">Rozbalte položku **Maven**.</span><span class="sxs-lookup"><span data-stu-id="2e183-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="2e183-167">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-167">b.</span></span> <span data-ttu-id="2e183-168">Vyberte **existující projekty Maven**.</span><span class="sxs-lookup"><span data-stu-id="2e183-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="2e183-169">c.</span><span class="sxs-lookup"><span data-stu-id="2e183-169">c.</span></span> <span data-ttu-id="2e183-170">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2e183-170">Click **Next**.</span></span>

   ![Dialog importovat][MV01]

1. <span data-ttu-id="2e183-172">Na hello **projekty Maven** stránky:</span><span class="sxs-lookup"><span data-stu-id="2e183-172">On hello **Maven Projects** page:</span></span>

   <span data-ttu-id="2e183-173">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-173">a.</span></span> <span data-ttu-id="2e183-174">Pro **kořenový adresář**, zadejte hello **dokončení** složky v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="2e183-174">For **Root Directory**, specify hello **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="2e183-175">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-175">b.</span></span> <span data-ttu-id="2e183-176">Rozbalte hello **Upřesnit** a zadejte vlastní název pro **šablony názvu**.</span><span class="sxs-lookup"><span data-stu-id="2e183-176">Expand hello **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="2e183-177">c.</span><span class="sxs-lookup"><span data-stu-id="2e183-177">c.</span></span> <span data-ttu-id="2e183-178">Vyberte hello pole hello **pom.xml** soubor v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="2e183-178">Select hello box for hello **pom.xml** file in hello project.</span></span>
   
   <span data-ttu-id="2e183-179">d.</span><span class="sxs-lookup"><span data-stu-id="2e183-179">d.</span></span> <span data-ttu-id="2e183-180">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="2e183-180">Click **Finish**.</span></span>

   ![Stránka projekty maven][MV02]

1. <span data-ttu-id="2e183-182">Pokud projekt Maven hello byl úspěšně otevřen, uvidíte druhý projekt uvedené v Eclipse.</span><span class="sxs-lookup"><span data-stu-id="2e183-182">When hello Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![Místní projekt Maven][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="2e183-184">Sestavení aplikace pružiny spouštěcí pomocí Maven</span><span class="sxs-lookup"><span data-stu-id="2e183-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="2e183-185">V prohlížeči projektu Eclipse hello vyberte projekt Maven hello.</span><span class="sxs-lookup"><span data-stu-id="2e183-185">In hello Eclipse Project Explorer, select hello Maven project.</span></span>

1. <span data-ttu-id="2e183-186">Klikněte na tlačítko **spustit** > **spustit jako** > **sestavení Maven**.</span><span class="sxs-lookup"><span data-stu-id="2e183-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![Příkazy toorun jako sestavení Maven][BU01]

1. <span data-ttu-id="2e183-188">Pokud vaše aplikace je úspěšně vytvořená, se hello okna konzoly zobrazuje stav hello.</span><span class="sxs-lookup"><span data-stu-id="2e183-188">When your application is successfully built, hello console window shows hello status.</span></span>

   ![Úspěšný build Maven][BU02]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="2e183-190">Publikovat tooAzure vaší webové aplikace pomocí kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="2e183-190">Publish your web app tooAzure by using a Docker container</span></span>

1. <span data-ttu-id="2e183-191">V prohlížeči projektu Eclipse hello vyberte projekt Maven hello.</span><span class="sxs-lookup"><span data-stu-id="2e183-191">In hello Eclipse Project Explorer, select hello Maven project.</span></span>

1. <span data-ttu-id="2e183-192">Klikněte na tlačítko hello Azure **publikovat** nabídce a pak klikněte na tlačítko **publikovat jako kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="2e183-192">Click hello Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![Publikovat jako příkaz kontejner Docker][PU01]

1. <span data-ttu-id="2e183-194">Když hello **nasazení kontejner Docker v Azure** zobrazí se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="2e183-194">When hello **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="2e183-195">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-195">a.</span></span> <span data-ttu-id="2e183-196">Zadejte název vlastního Docker bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="2e183-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="2e183-197">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-197">b.</span></span> <span data-ttu-id="2e183-198">Pro **artefaktů toodeploy**, zadejte cestu toohello hello **gs pružiny spouštěcí docker-0.1.0.jar** souborů, které jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2e183-198">For **Artifact toodeploy**, specify hello path toohello **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![Zadejte možnosti Docker][PU02]

   <span data-ttu-id="2e183-200">Zobrazí se všechny existující hostitelů Docker.</span><span class="sxs-lookup"><span data-stu-id="2e183-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="2e183-201">Pokud si zvolíte toodeploy tooan existující hostitele, můžete přeskočit toostep 5.</span><span class="sxs-lookup"><span data-stu-id="2e183-201">If you choose toodeploy tooan existing host, you can skip toostep 5.</span></span> <span data-ttu-id="2e183-202">Jinak použijte následující kroky toocreate hostitele hello:</span><span class="sxs-lookup"><span data-stu-id="2e183-202">Otherwise, use hello following steps toocreate a host:</span></span>

   <span data-ttu-id="2e183-203">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-203">a.</span></span> <span data-ttu-id="2e183-204">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="2e183-204">Click **Add**.</span></span>

      ![Přidat nový hostitel Docker][PU03]

   <span data-ttu-id="2e183-206">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-206">b.</span></span> <span data-ttu-id="2e183-207">Když hello **vytvořit hostitelů Docker** zobrazí se dialogové okno, můžete zvolit výchozí hello tooaccept nebo můžete zadat vlastní nastavení pro nové Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="2e183-207">When hello **Create Docker Host** dialog box appears, you can choose tooaccept hello defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="2e183-208">(Podrobný popis hello různá nastavení, najdete v části [publikovat webovou aplikaci jako kontejner Docker pomocí hello Azure Toolkit pro IntelliJ][Publish Container with Azure Toolkit].) Klikněte na tlačítko **Další** jste zadali při které toouse nastavení.</span><span class="sxs-lookup"><span data-stu-id="2e183-208">(For detailed descriptions of hello various settings, see [Publish a web app as a Docker container by using hello Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings toouse.</span></span>

      ![Zadejte možnosti hostitelů Docker][PU04]

   <span data-ttu-id="2e183-210">c.</span><span class="sxs-lookup"><span data-stu-id="2e183-210">c.</span></span> <span data-ttu-id="2e183-211">Můžete vybrat toouse existující přihlašovací údaje z trezoru služby Azure klíče nebo můžete tooenter nové Docker přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="2e183-211">You can choose toouse existing login credentials from an Azure key vault, or you can choose tooenter new Docker login credentials.</span></span> <span data-ttu-id="2e183-212">Klikněte na tlačítko **Dokončit** když nastavíte možnosti.</span><span class="sxs-lookup"><span data-stu-id="2e183-212">Click **Finish** when you have specified your options.</span></span>

      ![Zadejte přihlašovací údaje hostitelů Docker][PU05]

1. <span data-ttu-id="2e183-214">Vyberte Docker hostiteli a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2e183-214">Select your Docker host, and then click **Next**.</span></span>

   ![Vyberte hostitele toouse Docker][PU06]

1. <span data-ttu-id="2e183-216">Na poslední stránce hello hello **nasazení kontejner Docker v Azure** dialogovém okně zadejte hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="2e183-216">On hello last page of hello **Deploying Docker Container on Azure** dialog box, specify hello following options:</span></span>

   <span data-ttu-id="2e183-217">a.</span><span class="sxs-lookup"><span data-stu-id="2e183-217">a.</span></span> <span data-ttu-id="2e183-218">Můžete toospecify vlastní název pro hello kontejneru, který bude hostovat vaše kontejner Docker nebo můžete přijmout výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="2e183-218">You can choose toospecify a custom name for hello container that will host your Docker container, or you can accept hello default.</span></span>

   <span data-ttu-id="2e183-219">b.</span><span class="sxs-lookup"><span data-stu-id="2e183-219">b.</span></span> <span data-ttu-id="2e183-220">Zadejte porty TCP hello pro svého hostitele docker pomocí následující syntaxe hello: *[externí port]*:*[interní port]*.</span><span class="sxs-lookup"><span data-stu-id="2e183-220">Enter hello TCP ports for your docker host by using hello following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="2e183-221">Například **80:8080** určuje externí port 80 a hello výchozí vnitřní pružiny spouštěcí port 8080.</span><span class="sxs-lookup"><span data-stu-id="2e183-221">For example, **80:8080** specifies an external port of 80 and hello default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="2e183-222">Pokud jste upravili interní port (například úpravou souboru application.yml hello), je třeba číslo portu hello toospecify pro správné směrování toooccur hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="2e183-222">If you have customized your internal port (for example, by editing hello application.yml file), you need toospecify hello port number for hello correct routing toooccur in Azure.</span></span>

   <span data-ttu-id="2e183-223">c.</span><span class="sxs-lookup"><span data-stu-id="2e183-223">c.</span></span> <span data-ttu-id="2e183-224">Když nakonfigurujete tyto možnosti, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="2e183-224">After you configure these options, click **Finish**.</span></span>

   ![Nasadit kontejner Docker v Azure][PU07]

1. <span data-ttu-id="2e183-226">Po dokončení publikování hello Azure Toolkit hello protokol činnosti Azure zobrazí **publikováno** hello stavu.</span><span class="sxs-lookup"><span data-stu-id="2e183-226">When hello Azure Toolkit has finished publishing, hello Azure Activity Log displays **Published** for hello status.</span></span>

   ![Byla úspěšně nasazena hostitelů Docker][PU08]

## <a name="next-steps"></a><span data-ttu-id="2e183-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e183-228">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny Framework]: https://spring.io/

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
