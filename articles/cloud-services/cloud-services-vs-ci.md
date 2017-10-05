---
title: "Nastavené průběžné doručování pro cloud services pomocí sady Visual Studio Online | Microsoft Docs"
description: "Naučte se nastavit průběžné doručování Azure cloudových aplikací bez uložení klíč úložiště diagnostiky konfiguračních souborů služby"
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 148b2959-c5db-4e4a-a7e9-fccb252e7e8a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
ms.openlocfilehash: 7e70f92d4d1333ca6cbac5876e5ccbc763bd915c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-to-azure-using-visual-studio-online"></a><span data-ttu-id="31901-103">Securely uložit úložiště diagnostiky klíč cloudové služby a instalační program průběžnou integraci a nasazení do Azure pomocí sady Visual Studio Online</span><span class="sxs-lookup"><span data-stu-id="31901-103">Securely Save Cloud Services Diagnostics Storage Key and Setup Continuous Integration and Deployment to Azure using Visual Studio Online</span></span>
 <span data-ttu-id="31901-104">Je běžnou praxí zdroj projekty otevřít v současné době.</span><span class="sxs-lookup"><span data-stu-id="31901-104">It is a common practice to open source projects nowadays.</span></span> <span data-ttu-id="31901-105">Ukládání tajné klíče aplikace v konfiguračních souborech již není bezpečné postupem jako ohrožení zabezpečení jsou viditelné z úniku z ovládacích prvků veřejné zdrojů tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="31901-105">Saving application secrets in configuration files is no longer safe practice as security vulnerabilities are exposed from secrets being leaked from public source controls.</span></span> <span data-ttu-id="31901-106">Ukládání tajný klíč, protože není ve formátu prostého textu v souboru v kanálu průběžnou integraci zabezpečené buď vzhledem k tomu, že servery sestavení může být sdílené prostředky v Cloudovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="31901-106">Storing secret as plaintext in a file in a Continuous Integration pipeline is not secure either since build servers could be shared resources on the Cloud environment.</span></span> <span data-ttu-id="31901-107">Tento článek vysvětluje, jak Visual Studio a Visual Studio Online snižuje riziko z hlediska zabezpečení během vývoje a průběžnou integraci procesu.</span><span class="sxs-lookup"><span data-stu-id="31901-107">This article explains how Visual Studio and Visual Studio Online mitigates the security concerns during development and Continuous Integration process.</span></span>

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a><span data-ttu-id="31901-108">Odebrat tajné klíče úložiště diagnostiky v konfiguračním souboru projektu</span><span class="sxs-lookup"><span data-stu-id="31901-108">Remove Diagnostics Storage Key Secret in Project Configuration File</span></span>
<span data-ttu-id="31901-109">Rozšíření diagnostiky cloud Services vyžaduje úložiště Azure pro ukládání diagnostiky výsledků.</span><span class="sxs-lookup"><span data-stu-id="31901-109">Cloud Services diagnostics extension requires Azure storage for saving diagnostics results.</span></span> <span data-ttu-id="31901-110">Dříve připojovací řetězec úložiště je zadána v souborech cloudové služby konfigurace (.cscfg) a může být vráceny se změnami do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="31901-110">Formerly the storage connection string is specified in the Cloud Services configuration (.cscfg) files and could be checked in to source control.</span></span> <span data-ttu-id="31901-111">V nejnovější verzi sady Azure SDK jsme změnili chování pouze ukládat částečné připojovací řetězec s klíčem nahrazuje token.</span><span class="sxs-lookup"><span data-stu-id="31901-111">In the latest Azure SDK release we changed the behavior to only store a partial connection string with the key replaced by a token.</span></span> <span data-ttu-id="31901-112">Následující kroky popisují, jak funguje nástrojů nové cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="31901-112">The following steps describe how the new Cloud Services tooling works:</span></span>

### <a name="1-open-the-role-designer"></a><span data-ttu-id="31901-113">1. Otevřete roli návrháře</span><span class="sxs-lookup"><span data-stu-id="31901-113">1. Open the Role designer</span></span>
* <span data-ttu-id="31901-114">Dvakrát klikněte na tlačítko nebo klikněte pravým tlačítkem na roli cloudové služby otevřete návrhář Role</span><span class="sxs-lookup"><span data-stu-id="31901-114">Double click or right click on a Cloud Services role to open Role designer</span></span>

![Otevřete roli návrháře][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a><span data-ttu-id="31901-116">2. V části Diagnostika nové zaškrtávací políčko "nemáte odebrat z projektu tajný klíč úložiště" je přidána</span><span class="sxs-lookup"><span data-stu-id="31901-116">2. Under diagnostics section, a new check box “Don’t remove storage key secret from project” is added</span></span>
* <span data-ttu-id="31901-117">Pokud používáte emulátor místního úložiště, toto políčko je zakázána, protože neexistuje tajný klíč pro správu pro místní připojovací řetězec, který je UseDevelopmentStorage = true.</span><span class="sxs-lookup"><span data-stu-id="31901-117">If you are using the local storage emulator, this checkbox is disabled because there is no secret to manage for the local connection string, which is UseDevelopmentStorage=true.</span></span>

![Připojovací řetězec pro emulátor místního úložiště není tajné][1]

* <span data-ttu-id="31901-119">Pokud vytváříte nový projekt, ve výchozím nastavení toto políčko je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="31901-119">If you are creating a new project, by default this checkbox is unchecked.</span></span> <span data-ttu-id="31901-120">Výsledkem části klíče úložiště připojovacího řetězce úložiště vybrané nahrazován token.</span><span class="sxs-lookup"><span data-stu-id="31901-120">This results in the storage key section of the selected storage connection string being replaced with a token.</span></span> <span data-ttu-id="31901-121">Hodnota tokenu budou nacházet ve složce data aplikací Roaming aktuálního uživatele, například: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService</span><span class="sxs-lookup"><span data-stu-id="31901-121">The value of the token will be found under the current user’s AppData Roaming folder, for example: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService</span></span>

> <span data-ttu-id="31901-122">Upozorňujeme, že složce user\AppData je řídí přístup k přihlášení uživatele a je považován za na bezpečné místo pro uložení vývoj tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="31901-122">Note that the user\AppData folder is Access Controlled by user sign-in and is considered a secure place to store development secrets.</span></span>
> 
> 

![Úložiště klíč je uložen ve složce profilu uživatele][2]

### <a name="3-select-a-diagnostics-storage-account"></a><span data-ttu-id="31901-124">3. Vyberte účet úložiště diagnostiky</span><span class="sxs-lookup"><span data-stu-id="31901-124">3. Select a diagnostics storage account</span></span>
* <span data-ttu-id="31901-125">Vyberte účet úložiště z tohoto dialogového okna spustit kliknutím "..."</span><span class="sxs-lookup"><span data-stu-id="31901-125">Select a storage account from the dialog launched by clicking the “…”</span></span> <span data-ttu-id="31901-126">.</span><span class="sxs-lookup"><span data-stu-id="31901-126">button.</span></span> <span data-ttu-id="31901-127">Všimněte si, jak vygenerovat připojovacího řetězce úložiště nebude mít klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="31901-127">Notice how the storage connection string generated will not have the storage account key.</span></span>
* <span data-ttu-id="31901-128">Příklad: DefaultEndpointsProtocol = https; AccountName = contosostorage; AccountKey = $(*clouddiagstrg.key*)</span><span class="sxs-lookup"><span data-stu-id="31901-128">For example: DefaultEndpointsProtocol=https;AccountName=contosostorage;AccountKey=$(*clouddiagstrg.key*)</span></span>

### <a name="4----debugging-the-project"></a><span data-ttu-id="31901-129">4.    Ladění projektu</span><span class="sxs-lookup"><span data-stu-id="31901-129">4.    Debugging the project</span></span>
* <span data-ttu-id="31901-130">F5 spusťte ladění v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31901-130">F5 to start debugging in Visual Studio.</span></span> <span data-ttu-id="31901-131">Všechno, co by měly fungovat stejným způsobem jako před.</span><span class="sxs-lookup"><span data-stu-id="31901-131">Everything should work in the same way as before.</span></span>
  <span data-ttu-id="31901-132">![Spuštění ladění místně][3]</span><span class="sxs-lookup"><span data-stu-id="31901-132">![Start debugging locally][3]</span></span>

### <a name="5-publish-project-from-visual-studio"></a><span data-ttu-id="31901-133">5. Publikování projektu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31901-133">5. Publish project from Visual Studio</span></span>
* <span data-ttu-id="31901-134">Spustit dialogové okno publikování a pokračujte v přihlášení pokyny k publikování applicaion do Azure.</span><span class="sxs-lookup"><span data-stu-id="31901-134">Launch the publish dialog and proceed with sign-in instructions to publish the applicaion to Azure.</span></span>

### <a name="6-additional-information"></a><span data-ttu-id="31901-135">6. Další informace</span><span class="sxs-lookup"><span data-stu-id="31901-135">6. Additional information</span></span>
> <span data-ttu-id="31901-136">Poznámka: Panelu nastavení v Návrháři role zůstanou, protože je teď.</span><span class="sxs-lookup"><span data-stu-id="31901-136">Note: The Settings panel in the role designer will stay as it is for now.</span></span> <span data-ttu-id="31901-137">Pokud chcete používat funkci tajný správy pro diagnostiku, přejděte na kartu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="31901-137">If you want to use the secret management feature for diagnostics, go to the Configurations tab.</span></span>
> 
> 

![Přidejte nastavení][4]

> <span data-ttu-id="31901-139">Poznámka: Pokud je povoleno, Application Insights klíč bude uložen jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="31901-139">Note: If enabled, the Application Insights key will be stored as plain text.</span></span> <span data-ttu-id="31901-140">Klíč slouží pouze pro odeslání obsahu, žádná citlivá data budou v riziko ohrožení.</span><span class="sxs-lookup"><span data-stu-id="31901-140">The key is only used for upload contents so no sensitive data will be at risk being compromised.</span></span>
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a><span data-ttu-id="31901-141">Sestavení a publikování projekt cloudových služeb, pomocí šablony sady Visual Studio online úloh</span><span class="sxs-lookup"><span data-stu-id="31901-141">Build and Publish a Cloud Services Project using Visual Studio online Task Templates</span></span>
* <span data-ttu-id="31901-142">Následující postup ukazuje, jak nastavit průběžnou integraci pro projekt cloudové služby pomocí sady Visual Studio online úlohy:</span><span class="sxs-lookup"><span data-stu-id="31901-142">The following steps illustrates how to setup Continuous Integration for Cloud Services project using Visual Studio online tasks:</span></span>
  ### <a name="1----obtain-a-vso-account"></a><span data-ttu-id="31901-143">1.    Získat účet VSO</span><span class="sxs-lookup"><span data-stu-id="31901-143">1.    Obtain a VSO account</span></span>
* <span data-ttu-id="31901-144">[Vytvoření sady Visual Studio Online účtu] [ Create Visual Studio Online account] Pokud již nemáte</span><span class="sxs-lookup"><span data-stu-id="31901-144">[Create Visual Studio Online account][Create Visual Studio Online account] if you don’t have one already</span></span>
* <span data-ttu-id="31901-145">[Vytvoření týmového projektu] [ Create team project] ve vašem účtu sady Visual Studio online</span><span class="sxs-lookup"><span data-stu-id="31901-145">[Create team project][Create team project] in your Visual Studio online account</span></span>

### <a name="2----setup-source-control-in-visual-studio"></a><span data-ttu-id="31901-146">2.    Instalační program zdrojového kódu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31901-146">2.    Setup source control in Visual Studio</span></span>
* <span data-ttu-id="31901-147">Připojení k týmovému projektu</span><span class="sxs-lookup"><span data-stu-id="31901-147">Connect to a team project</span></span>

![Připojení k týmovému projektu][5]

![Vyberte pro připojení k týmovému projektu][6]

* <span data-ttu-id="31901-150">Přidání projektu do správy zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="31901-150">Add your project to source control</span></span>

![Přidání projektu do správy zdrojového kódu][7]

![Mapování projektu do zdrojové složky ovládací prvek][8]

* <span data-ttu-id="31901-153">Zkontrolujte ve vašem projektu z Team Explorer</span><span class="sxs-lookup"><span data-stu-id="31901-153">Check in your project from Team Explorer</span></span>

![Projekt do správy zdrojového kódu se změnami][9]

### <a name="3----configure-build-process"></a><span data-ttu-id="31901-155">3.    Konfigurace procesu sestavení</span><span class="sxs-lookup"><span data-stu-id="31901-155">3.    Configure Build process</span></span>
* <span data-ttu-id="31901-156">Přejděte k vašemu týmovému projektu a přidat nové šablony procesu sestavení</span><span class="sxs-lookup"><span data-stu-id="31901-156">Browse to your team project and add a new build process Templates</span></span>

![Přidání nového sestavení][10]

* <span data-ttu-id="31901-158">Úlohu vyberte sestavení</span><span class="sxs-lookup"><span data-stu-id="31901-158">Select build task</span></span>

![Přidat úlohu sestavení][11]

![Vyberte šablonu úloh sestavení Visual Studio][12]

* <span data-ttu-id="31901-161">Upravte vstup úloh sestavení.</span><span class="sxs-lookup"><span data-stu-id="31901-161">Edit build task input.</span></span> <span data-ttu-id="31901-162">Prosím přizpůsobit podle potřeby vašeho parametry sestavení</span><span class="sxs-lookup"><span data-stu-id="31901-162">Please customize the build parameters according to your need</span></span>

![Nakonfigurujte úlohu sestavení][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* <span data-ttu-id="31901-164">Konfigurace sestavení proměnné</span><span class="sxs-lookup"><span data-stu-id="31901-164">Configure build variables</span></span>

![Konfigurace sestavení proměnné][14]

* <span data-ttu-id="31901-166">Přidat úloha nahrát rozevírací sestavení</span><span class="sxs-lookup"><span data-stu-id="31901-166">Add a task to upload build drop</span></span>

![Zvolte publikování úlohu rozevírací sestavení][15]

![Konfigurace publikování úlohu rozevírací sestavení][16]

* <span data-ttu-id="31901-169">Spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="31901-169">Run the build</span></span>

![Nové sestavení fronty][17]

![Zobrazení souhrnu sestavení][18]

* <span data-ttu-id="31901-172">Pokud sestavení úspěšné se zobrazí podobná níže výsledek</span><span class="sxs-lookup"><span data-stu-id="31901-172">if the build is successful you will see a result similar to below</span></span>

![Sestavení výsledek][19]

### <a name="4----configure-release-process"></a><span data-ttu-id="31901-174">4.    Konfigurace verze procesu</span><span class="sxs-lookup"><span data-stu-id="31901-174">4.    Configure Release process</span></span>
* <span data-ttu-id="31901-175">Vytvořit novou verzi</span><span class="sxs-lookup"><span data-stu-id="31901-175">Create a new release</span></span>

![Vytvořit novou verzi][20]

* <span data-ttu-id="31901-177">Vyberte úkol nasazení služby Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="31901-177">Select the Azure Cloud Services Deployment task</span></span>

![Vyberte úlohu nasazení Azure Cloud Services][21]

* <span data-ttu-id="31901-179">Jako klíč účtu úložiště není změnami do správy zdrojového kódu, je potřeba zadat tajný klíč pro nastavení rozšíření diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="31901-179">As the storage account key is not checked in to source control, we need to specify the secret key for setting diagnostics extensions.</span></span> <span data-ttu-id="31901-180">Rozbalte **rozšířené možnosti pro vytvoření nové služby** část a upravte **klíče účtu úložiště diagnostiky** vstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="31901-180">Expand the **Advanced Options for Creating New Service** section and edit the **Diagnostics Storage Account Keys** parameter input.</span></span> <span data-ttu-id="31901-181">Tento vstup přebírá více řádků pár klíčových hodnot ve formátu **[RoleName]:$(StorageAccountKey)**</span><span class="sxs-lookup"><span data-stu-id="31901-181">This input takes in multiple lines of key value pair in the format of **[RoleName]:$(StorageAccountKey)**</span></span>

> <span data-ttu-id="31901-182">Poznámka: Pokud váš účet úložiště diagnostiky v rámci stejného předplatného jako ve které budete publikovat aplikace cloudové služby, nemusíte zadejte klíč ve vstupu úlohy nasazení; nasazení prostřednictvím kódu programu získat informace o úložiště ze svého předplatného</span><span class="sxs-lookup"><span data-stu-id="31901-182">Note: if your diagnostics storage account is under the same subscription as where you will publish the Cloud Services application, you don't have to enter the key in the deployment task input; the deployment will programmatically obtain the storage information from your subscription</span></span>
> 
> 

![Konfigurace úlohy nasazení cloudové služby][22]

* <span data-ttu-id="31901-184">Tajný klíč použití vytvářet proměnné, které chcete uložit klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="31901-184">Use secret build variables to save storage Keys.</span></span> <span data-ttu-id="31901-185">K maskování proměnnou jako tajný klíč. klikněte na ikonu zámku na pravé straně vstupní proměnné</span><span class="sxs-lookup"><span data-stu-id="31901-185">To mask a variable as secret click on the lock icon on the right side of the Variables input</span></span>

![Uložení klíčů k úložišti v proměnné tajný sestavení][23]

* <span data-ttu-id="31901-187">Vytvoření verze a nasazení projektu do Azure</span><span class="sxs-lookup"><span data-stu-id="31901-187">Create a release and deploy your project to Azure</span></span>

![Vytvořit novou verzi][24]

## <a name="next-steps"></a><span data-ttu-id="31901-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31901-189">Next steps</span></span>
<span data-ttu-id="31901-190">Další informace o nastavení rozšíření diagnostiky pro Azure Cloud Services, najdete v tématu [zapněte diagnostiku ve službě Azure Cloud Services pomocí prostředí PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]</span><span class="sxs-lookup"><span data-stu-id="31901-190">To learn more about setting diagnostics extensions for Azure Cloud Services, please see [Enable diagnostics in Azure Cloud Services using PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]</span></span>

[Create Visual Studio Online account]:https://www.visualstudio.com/team-services/
[Create team project]: https://www.visualstudio.com/it-it/docs/setup-admin/team-services/connect-to-visual-studio-team-services
[Enable diagnostics in Azure Cloud Services using PowerShell]:https://azure.microsoft.com/en-us/documentation/articles/cloud-services-diagnostics-powershell/

[0]: ./media/cloud-services-vs-ci/vs-01.png
[1]: ./media/cloud-services-vs-ci/vs-02.png
[2]: ./media/cloud-services-vs-ci/file-01.png
[3]: ./media/cloud-services-vs-ci/vs-03.png
[4]: ./media/cloud-services-vs-ci/vs-04.png
[5]: ./media/cloud-services-vs-ci/vs-05.png
[6]: ./media/cloud-services-vs-ci/vs-06.png
[7]: ./media/cloud-services-vs-ci/vs-07.png
[8]: ./media/cloud-services-vs-ci/vs-08.png
[9]: ./media/cloud-services-vs-ci/vs-09.png
[10]: ./media/cloud-services-vs-ci/vso-01.png
[11]: ./media/cloud-services-vs-ci/vso-02.png
[12]: ./media/cloud-services-vs-ci/vso-03.png
[13]: ./media/cloud-services-vs-ci/vso-04.png
[14]: ./media/cloud-services-vs-ci/vso-05.png
[15]: ./media/cloud-services-vs-ci/vso-06.png
[16]: ./media/cloud-services-vs-ci/vso-07.png
[17]: ./media/cloud-services-vs-ci/vso-08.png
[18]: ./media/cloud-services-vs-ci/vso-09.png
[19]: ./media/cloud-services-vs-ci/vso-10.png
[20]: ./media/cloud-services-vs-ci/vso-11.png
[21]: ./media/cloud-services-vs-ci/vso-12.png
[22]: ./media/cloud-services-vs-ci/vso-13.png
[23]: ./media/cloud-services-vs-ci/vso-14.png
[24]: ./media/cloud-services-vs-ci/vso-15.png
