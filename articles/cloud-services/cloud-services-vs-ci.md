---
title: "aaaContinuous doručení pro cloud services pomocí sady Visual Studio Online | Microsoft Docs"
description: "Zjistěte, jak tooset až nastavené průběžné doručování pro Azure cloudových aplikací bez uložení diagnostiky úložiště klíčů toohello služby konfigurační soubory"
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
ms.openlocfilehash: dc87d049e46daf8b8a26ee4450ebd9b7910f287b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-tooazure-using-visual-studio-online"></a><span data-ttu-id="4737a-103">Securely uložit úložiště diagnostiky klíč cloudové služby a instalační program průběžnou integraci a nasazení tooAzure pomocí sady Visual Studio Online</span><span class="sxs-lookup"><span data-stu-id="4737a-103">Securely Save Cloud Services Diagnostics Storage Key and Setup Continuous Integration and Deployment tooAzure using Visual Studio Online</span></span>
 <span data-ttu-id="4737a-104">V současné době je běžné projekty zdrojů tooopen postupem.</span><span class="sxs-lookup"><span data-stu-id="4737a-104">It is a common practice tooopen source projects nowadays.</span></span> <span data-ttu-id="4737a-105">Ukládání tajné klíče aplikace v konfiguračních souborech již není bezpečné postupem jako ohrožení zabezpečení jsou viditelné z úniku z ovládacích prvků veřejné zdrojů tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="4737a-105">Saving application secrets in configuration files is no longer safe practice as security vulnerabilities are exposed from secrets being leaked from public source controls.</span></span> <span data-ttu-id="4737a-106">Ukládání tajný klíč, protože není ve formátu prostého textu v souboru v kanálu průběžnou integraci zabezpečené buď vzhledem k tomu, že servery sestavení může být sdílené prostředky na hello cloudového prostředí.</span><span class="sxs-lookup"><span data-stu-id="4737a-106">Storing secret as plaintext in a file in a Continuous Integration pipeline is not secure either since build servers could be shared resources on hello Cloud environment.</span></span> <span data-ttu-id="4737a-107">Tento článek vysvětluje, jak Visual Studio a Visual Studio Online snižuje hello zabezpečení se během vývoje a průběžnou integraci procesu.</span><span class="sxs-lookup"><span data-stu-id="4737a-107">This article explains how Visual Studio and Visual Studio Online mitigates hello security concerns during development and Continuous Integration process.</span></span>

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a><span data-ttu-id="4737a-108">Odebrat tajné klíče úložiště diagnostiky v konfiguračním souboru projektu</span><span class="sxs-lookup"><span data-stu-id="4737a-108">Remove Diagnostics Storage Key Secret in Project Configuration File</span></span>
<span data-ttu-id="4737a-109">Rozšíření diagnostiky cloud Services vyžaduje úložiště Azure pro ukládání diagnostiky výsledků.</span><span class="sxs-lookup"><span data-stu-id="4737a-109">Cloud Services diagnostics extension requires Azure storage for saving diagnostics results.</span></span> <span data-ttu-id="4737a-110">Připojovací řetězec úložiště hello dříve je uvedená v souborech konfigurace (.cscfg) hello cloudové služby a mohou být vráceny se změnami toosource řízení.</span><span class="sxs-lookup"><span data-stu-id="4737a-110">Formerly hello storage connection string is specified in hello Cloud Services configuration (.cscfg) files and could be checked in toosource control.</span></span> <span data-ttu-id="4737a-111">V nejnovější verzi sady Azure SDK hello jsme změnili hello chování tooonly úložiště částečné připojovací řetězec s klíčem hello nahrazuje token.</span><span class="sxs-lookup"><span data-stu-id="4737a-111">In hello latest Azure SDK release we changed hello behavior tooonly store a partial connection string with hello key replaced by a token.</span></span> <span data-ttu-id="4737a-112">Hello následující kroky popisují, jak funguje hello nové cloudové služby nástrojů:</span><span class="sxs-lookup"><span data-stu-id="4737a-112">hello following steps describe how hello new Cloud Services tooling works:</span></span>

### <a name="1-open-hello-role-designer"></a><span data-ttu-id="4737a-113">1. Otevřete roli designer hello</span><span class="sxs-lookup"><span data-stu-id="4737a-113">1. Open hello Role designer</span></span>
* <span data-ttu-id="4737a-114">Dvakrát klikněte na tlačítko nebo klikněte pravým tlačítkem na roli návrháře tooopen role cloudové služby</span><span class="sxs-lookup"><span data-stu-id="4737a-114">Double click or right click on a Cloud Services role tooopen Role designer</span></span>

![Otevřete roli návrháře][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a><span data-ttu-id="4737a-116">2. V části Diagnostika nové zaškrtávací políčko "nemáte odebrat z projektu tajný klíč úložiště" je přidána</span><span class="sxs-lookup"><span data-stu-id="4737a-116">2. Under diagnostics section, a new check box “Don’t remove storage key secret from project” is added</span></span>
* <span data-ttu-id="4737a-117">Pokud používáte emulátor místního úložiště hello, toto políčko je zakázána, protože neexistuje žádný tajný toomanage pro hello místní připojovací řetězec, který je UseDevelopmentStorage = true.</span><span class="sxs-lookup"><span data-stu-id="4737a-117">If you are using hello local storage emulator, this checkbox is disabled because there is no secret toomanage for hello local connection string, which is UseDevelopmentStorage=true.</span></span>

![Připojovací řetězec pro emulátor místního úložiště není tajné][1]

* <span data-ttu-id="4737a-119">Pokud vytváříte nový projekt, ve výchozím nastavení toto políčko je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="4737a-119">If you are creating a new project, by default this checkbox is unchecked.</span></span> <span data-ttu-id="4737a-120">Výsledkem hello úložiště klíče oddílu nahrazován token hello vybraný připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="4737a-120">This results in hello storage key section of hello selected storage connection string being replaced with a token.</span></span> <span data-ttu-id="4737a-121">Hello hodnota tokenu hello najdete ve složce data aplikací Roaming hello aktuálního uživatele, například: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService</span><span class="sxs-lookup"><span data-stu-id="4737a-121">hello value of hello token will be found under hello current user’s AppData Roaming folder, for example: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService</span></span>

> <span data-ttu-id="4737a-122">Poznámka: Tato složka user\AppData hello je řídí přístup k přihlášení uživatele a je považován za tajné klíče bezpečné místo toostore vývoj.</span><span class="sxs-lookup"><span data-stu-id="4737a-122">Note that hello user\AppData folder is Access Controlled by user sign-in and is considered a secure place toostore development secrets.</span></span>
> 
> 

![Úložiště klíč je uložen ve složce profilu uživatele][2]

### <a name="3-select-a-diagnostics-storage-account"></a><span data-ttu-id="4737a-124">3. Vyberte účet úložiště diagnostiky</span><span class="sxs-lookup"><span data-stu-id="4737a-124">3. Select a diagnostics storage account</span></span>
* <span data-ttu-id="4737a-125">Vyberte účet úložiště z hello dialogového okna spustit kliknutím na tlačítko "..." hello</span><span class="sxs-lookup"><span data-stu-id="4737a-125">Select a storage account from hello dialog launched by clicking hello “…”</span></span> <span data-ttu-id="4737a-126">.</span><span class="sxs-lookup"><span data-stu-id="4737a-126">button.</span></span> <span data-ttu-id="4737a-127">Všimněte si, jak vygenerovat hello připojovacího řetězce úložiště nebude mít hello klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4737a-127">Notice how hello storage connection string generated will not have hello storage account key.</span></span>
* <span data-ttu-id="4737a-128">Příklad: DefaultEndpointsProtocol = https; AccountName = contosostorage; AccountKey = $(*clouddiagstrg.key*)</span><span class="sxs-lookup"><span data-stu-id="4737a-128">For example: DefaultEndpointsProtocol=https;AccountName=contosostorage;AccountKey=$(*clouddiagstrg.key*)</span></span>

### <a name="4----debugging-hello-project"></a><span data-ttu-id="4737a-129">4.    Ladění projektu hello</span><span class="sxs-lookup"><span data-stu-id="4737a-129">4.    Debugging hello project</span></span>
* <span data-ttu-id="4737a-130">Ladění v sadě Visual Studio toostart F5.</span><span class="sxs-lookup"><span data-stu-id="4737a-130">F5 toostart debugging in Visual Studio.</span></span> <span data-ttu-id="4737a-131">Všechno, co by měla fungovat v hello stejný způsobem jako předtím.</span><span class="sxs-lookup"><span data-stu-id="4737a-131">Everything should work in hello same way as before.</span></span>
  <span data-ttu-id="4737a-132">![Spuštění ladění místně][3]</span><span class="sxs-lookup"><span data-stu-id="4737a-132">![Start debugging locally][3]</span></span>

### <a name="5-publish-project-from-visual-studio"></a><span data-ttu-id="4737a-133">5. Publikování projektu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4737a-133">5. Publish project from Visual Studio</span></span>
* <span data-ttu-id="4737a-134">Spuštění hello dialogového okna publikování a pokračujte v přihlášení pokyny toopublish hello applicaion tooAzure.</span><span class="sxs-lookup"><span data-stu-id="4737a-134">Launch hello publish dialog and proceed with sign-in instructions toopublish hello applicaion tooAzure.</span></span>

### <a name="6-additional-information"></a><span data-ttu-id="4737a-135">6. Další informace</span><span class="sxs-lookup"><span data-stu-id="4737a-135">6. Additional information</span></span>
> <span data-ttu-id="4737a-136">Poznámka: panel nastavení hello v Návrháři role hello zůstanou, protože je teď.</span><span class="sxs-lookup"><span data-stu-id="4737a-136">Note: hello Settings panel in hello role designer will stay as it is for now.</span></span> <span data-ttu-id="4737a-137">Pokud chcete funkci tajný správy hello toouse pro diagnostiku, přejděte toohello kartu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4737a-137">If you want toouse hello secret management feature for diagnostics, go toohello Configurations tab.</span></span>
> 
> 

![Přidejte nastavení][4]

> <span data-ttu-id="4737a-139">Poznámka: Pokud je povoleno, hello Application Insights se má uložit klíč jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="4737a-139">Note: If enabled, hello Application Insights key will be stored as plain text.</span></span> <span data-ttu-id="4737a-140">Hello klíč se používá pouze pro odeslání obsahu, tak žádné citlivá data budou v riziko ohrožení.</span><span class="sxs-lookup"><span data-stu-id="4737a-140">hello key is only used for upload contents so no sensitive data will be at risk being compromised.</span></span>
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a><span data-ttu-id="4737a-141">Sestavení a publikování projekt cloudových služeb, pomocí šablony sady Visual Studio online úloh</span><span class="sxs-lookup"><span data-stu-id="4737a-141">Build and Publish a Cloud Services Project using Visual Studio online Task Templates</span></span>
* <span data-ttu-id="4737a-142">Hello následující postup ukazuje, jak toosetup průběžnou integraci pro cloudové služby projektu pomocí sady Visual Studio online úloh:</span><span class="sxs-lookup"><span data-stu-id="4737a-142">hello following steps illustrates how toosetup Continuous Integration for Cloud Services project using Visual Studio online tasks:</span></span>
  ### <a name="1----obtain-a-vso-account"></a><span data-ttu-id="4737a-143">1.    Získat účet VSO</span><span class="sxs-lookup"><span data-stu-id="4737a-143">1.    Obtain a VSO account</span></span>
* <span data-ttu-id="4737a-144">[Vytvoření sady Visual Studio Online účtu] [ Create Visual Studio Online account] Pokud již nemáte</span><span class="sxs-lookup"><span data-stu-id="4737a-144">[Create Visual Studio Online account][Create Visual Studio Online account] if you don’t have one already</span></span>
* <span data-ttu-id="4737a-145">[Vytvoření týmového projektu] [ Create team project] ve vašem účtu sady Visual Studio online</span><span class="sxs-lookup"><span data-stu-id="4737a-145">[Create team project][Create team project] in your Visual Studio online account</span></span>

### <a name="2----setup-source-control-in-visual-studio"></a><span data-ttu-id="4737a-146">2.    Instalační program zdrojového kódu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4737a-146">2.    Setup source control in Visual Studio</span></span>
* <span data-ttu-id="4737a-147">Připojit tooa týmového projektu</span><span class="sxs-lookup"><span data-stu-id="4737a-147">Connect tooa team project</span></span>

![Připojení tooteam projektu][5]

![Vyberte tooconnect týmového projektu pro][6]

* <span data-ttu-id="4737a-150">Přidání ovládacího prvku vašeho projektu toosource</span><span class="sxs-lookup"><span data-stu-id="4737a-150">Add your project toosource control</span></span>

![Přidání ovládacího prvku toosource projektu][7]

![Mapování složky projektu tooa zdroj ovládacího prvku][8]

* <span data-ttu-id="4737a-153">Zkontrolujte ve vašem projektu z Team Explorer</span><span class="sxs-lookup"><span data-stu-id="4737a-153">Check in your project from Team Explorer</span></span>

![Zkontrolujte v toosource řízení projektu][9]

### <a name="3----configure-build-process"></a><span data-ttu-id="4737a-155">3.    Konfigurace procesu sestavení</span><span class="sxs-lookup"><span data-stu-id="4737a-155">3.    Configure Build process</span></span>
* <span data-ttu-id="4737a-156">Procházet tooyour týmový projekt a přidejte nové šablony procesu sestavení</span><span class="sxs-lookup"><span data-stu-id="4737a-156">Browse tooyour team project and add a new build process Templates</span></span>

![Přidání nového sestavení][10]

* <span data-ttu-id="4737a-158">Úlohu vyberte sestavení</span><span class="sxs-lookup"><span data-stu-id="4737a-158">Select build task</span></span>

![Přidat úlohu sestavení][11]

![Vyberte šablonu úloh sestavení Visual Studio][12]

* <span data-ttu-id="4737a-161">Upravte vstup úloh sestavení.</span><span class="sxs-lookup"><span data-stu-id="4737a-161">Edit build task input.</span></span> <span data-ttu-id="4737a-162">Prosím přizpůsobit hello sestavení, potřebné parametry podle tooyour</span><span class="sxs-lookup"><span data-stu-id="4737a-162">Please customize hello build parameters according tooyour need</span></span>

![Nakonfigurujte úlohu sestavení][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* <span data-ttu-id="4737a-164">Konfigurace sestavení proměnné</span><span class="sxs-lookup"><span data-stu-id="4737a-164">Configure build variables</span></span>

![Konfigurace sestavení proměnné][14]

* <span data-ttu-id="4737a-166">Přidat úloha tooupload sestavení pokles</span><span class="sxs-lookup"><span data-stu-id="4737a-166">Add a task tooupload build drop</span></span>

![Zvolte publikování úlohu rozevírací sestavení][15]

![Konfigurace publikování úlohu rozevírací sestavení][16]

* <span data-ttu-id="4737a-169">Spustit hello sestavení</span><span class="sxs-lookup"><span data-stu-id="4737a-169">Run hello build</span></span>

![Nové sestavení fronty][17]

![Zobrazení souhrnu sestavení][18]

* <span data-ttu-id="4737a-172">je-li hello sestavení úspěšně zobrazí se podobné toobelow výsledek</span><span class="sxs-lookup"><span data-stu-id="4737a-172">if hello build is successful you will see a result similar toobelow</span></span>

![Sestavení výsledek][19]

### <a name="4----configure-release-process"></a><span data-ttu-id="4737a-174">4.    Konfigurace verze procesu</span><span class="sxs-lookup"><span data-stu-id="4737a-174">4.    Configure Release process</span></span>
* <span data-ttu-id="4737a-175">Vytvořit novou verzi</span><span class="sxs-lookup"><span data-stu-id="4737a-175">Create a new release</span></span>

![Vytvořit novou verzi][20]

* <span data-ttu-id="4737a-177">Vyberte úlohu nasazení služby Azure Cloud Services hello</span><span class="sxs-lookup"><span data-stu-id="4737a-177">Select hello Azure Cloud Services Deployment task</span></span>

![Vyberte úlohu nasazení Azure Cloud Services][21]

* <span data-ttu-id="4737a-179">Jako hello klíč účtu úložiště není zaškrtnuté v ovládacím prvku toosource, potřebujeme toospecify hello tajný klíč pro nastavení rozšíření diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="4737a-179">As hello storage account key is not checked in toosource control, we need toospecify hello secret key for setting diagnostics extensions.</span></span> <span data-ttu-id="4737a-180">Rozbalte hello **rozšířené možnosti pro vytvoření nové služby** část a upravte hello **klíče účtu úložiště diagnostiky** vstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="4737a-180">Expand hello **Advanced Options for Creating New Service** section and edit hello **Diagnostics Storage Account Keys** parameter input.</span></span> <span data-ttu-id="4737a-181">Tento vstup přebírá více řádků pár klíčových hodnot ve formátu hello **[RoleName]:$(StorageAccountKey)**</span><span class="sxs-lookup"><span data-stu-id="4737a-181">This input takes in multiple lines of key value pair in hello format of **[RoleName]:$(StorageAccountKey)**</span></span>

> <span data-ttu-id="4737a-182">Poznámka: Pokud váš diagnostiky, které účet úložiště je pod hello stejnému předplatnému jako ve které budete publikovat aplikace hello cloudové služby, nemáte tooenter hello klíč ve vstupu úlohy nasazení hello; Hello nasazení prostřednictvím kódu programu získat informace o hello úložiště ze svého předplatného</span><span class="sxs-lookup"><span data-stu-id="4737a-182">Note: if your diagnostics storage account is under hello same subscription as where you will publish hello Cloud Services application, you don't have tooenter hello key in hello deployment task input; hello deployment will programmatically obtain hello storage information from your subscription</span></span>
> 
> 

![Konfigurace úlohy nasazení cloudové služby][22]

* <span data-ttu-id="4737a-184">Tajný klíč použití sestavení proměnné toosave klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="4737a-184">Use secret build variables toosave storage Keys.</span></span> <span data-ttu-id="4737a-185">vstupní proměnné toomask proměnnou jako tajný klíč. klikněte na ikonu zámku hello na pravé straně hello hello</span><span class="sxs-lookup"><span data-stu-id="4737a-185">toomask a variable as secret click on hello lock icon on hello right side of hello Variables input</span></span>

![Uložení klíčů k úložišti v proměnné tajný sestavení][23]

* <span data-ttu-id="4737a-187">Vytvořit verzi a nasadit vaši tooAzure projektu</span><span class="sxs-lookup"><span data-stu-id="4737a-187">Create a release and deploy your project tooAzure</span></span>

![Vytvořit novou verzi][24]

## <a name="next-steps"></a><span data-ttu-id="4737a-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4737a-189">Next steps</span></span>
<span data-ttu-id="4737a-190">toolearn Další informace o nastavení rozšíření diagnostiky pro Azure Cloud Services najdete v tématu [zapněte diagnostiku ve službě Azure Cloud Services pomocí prostředí PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]</span><span class="sxs-lookup"><span data-stu-id="4737a-190">toolearn more about setting diagnostics extensions for Azure Cloud Services, please see [Enable diagnostics in Azure Cloud Services using PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]</span></span>

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
