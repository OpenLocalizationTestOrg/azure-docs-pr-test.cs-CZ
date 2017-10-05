---
title: "Použít k aktivaci úlohy Azure Automation | Microsoft Docs"
description: "Naučte se používat Azure Automation pro spuštění úlohy StorSimple Manager dat (soukromém náhledu)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 9691408bcd80afb6eba534f26749b76dd3bfe315
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-automation-to-trigger-a-job-private-preview"></a><span data-ttu-id="7fa77-103">Použití Azure Automation k aktivaci úlohy (soukromém náhledu).</span><span class="sxs-lookup"><span data-stu-id="7fa77-103">Use Azure Automation to trigger a job (Private Preview)</span></span>

<span data-ttu-id="7fa77-104">Tento článek popisuje, jak používat Azure Automation k aktivaci úlohy StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="7fa77-104">This articles describes how to use Azure Automation to trigger a StorSimple Data Manager job.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7fa77-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7fa77-105">Prerequisites</span></span>

<span data-ttu-id="7fa77-106">Než začnete, ujistěte se, zda máte:</span><span class="sxs-lookup"><span data-stu-id="7fa77-106">Before you begin, ensure that you have:</span></span>

*   <span data-ttu-id="7fa77-107">Nainstalovat Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="7fa77-107">Azure Powershell installed.</span></span> <span data-ttu-id="7fa77-108">[Stáhnout prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="7fa77-108">[Download Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
*   <span data-ttu-id="7fa77-109">Nastavení konfigurace k chybě při inicializaci úlohu transformace dat (pokyny k získání těchto nastavení jsou zde uvedena).</span><span class="sxs-lookup"><span data-stu-id="7fa77-109">Configuration settings to initialize the Data Transformation job (instructions to obtain these settings are included here).</span></span>
*   <span data-ttu-id="7fa77-110">Definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="7fa77-110">A job definition that has been correctly configured in a Hybrid Data Resource within a resource group.</span></span>
*   <span data-ttu-id="7fa77-111">Stáhněte si `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) soubor z úložiště githubu.</span><span class="sxs-lookup"><span data-stu-id="7fa77-111">Download `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) file from the github repository.</span></span>
*   <span data-ttu-id="7fa77-112">Stáhněte si `Get-ConfigurationParams.ps1` [skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) z úložiště githubu.</span><span class="sxs-lookup"><span data-stu-id="7fa77-112">Download `Get-ConfigurationParams.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) from the github repository.</span></span>
*   <span data-ttu-id="7fa77-113">Stáhněte si `Trigger-DataTransformation-Job.ps1` [skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) z úložiště githubu.</span><span class="sxs-lookup"><span data-stu-id="7fa77-113">Download `Trigger-DataTransformation-Job.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) from the github repository.</span></span>

## <a name="step-by-step"></a><span data-ttu-id="7fa77-114">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="7fa77-114">Step-by-step</span></span>

### <a name="get-azure-active-directory-permissions-for-the-automation-job-to-run-the-job-definition"></a><span data-ttu-id="7fa77-115">Získat oprávnění služby Azure Active Directory pro danou definici úlohy úlohu automatizace</span><span class="sxs-lookup"><span data-stu-id="7fa77-115">Get Azure Active Directory permissions for the automation job to run the job definition</span></span>

1. <span data-ttu-id="7fa77-116">Chcete-li načíst parametry konfigurace pro službu Active Directory, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7fa77-116">To retrieve the configuration parameters for Active Directory, do the following steps:</span></span>

    1. <span data-ttu-id="7fa77-117">Otevřete prostředí Windows PowerShell v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="7fa77-117">Open Windows PowerShell in your local machine.</span></span> <span data-ttu-id="7fa77-118">Ujistěte se, že [prostředí Azure PowerShell](https://azure.microsoft.com/downloads/) je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="7fa77-118">Ensure that [Azure PowerShell](https://azure.microsoft.com/downloads/) is installed.</span></span>
    1. <span data-ttu-id="7fa77-119">Spustit `Get-ConfigurationParams.ps1` skriptu (ve složce stažené výše).</span><span class="sxs-lookup"><span data-stu-id="7fa77-119">Run the `Get-ConfigurationParams.ps1` script (in the folder you downloaded above).</span></span> <span data-ttu-id="7fa77-120">V okně prostředí PowerShell zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fa77-120">Type the following command in the PowerShell window:</span></span>

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        <span data-ttu-id="7fa77-121">ActiveDirectoryKey je heslo, které použijete později.</span><span class="sxs-lookup"><span data-stu-id="7fa77-121">The ActiveDirectoryKey is a password that you use later.</span></span> <span data-ttu-id="7fa77-122">Zadejte heslo podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="7fa77-122">Enter a password of your choice.</span></span> <span data-ttu-id="7fa77-123">AppName může být libovolný řetězec.</span><span class="sxs-lookup"><span data-stu-id="7fa77-123">AppName can be any string.</span></span>

2. <span data-ttu-id="7fa77-124">Skript vypíše následující hodnoty, které se mají použít při spuštění sady automation runbook.</span><span class="sxs-lookup"><span data-stu-id="7fa77-124">This script outputs the following values that should be used while triggering the automation runbook.</span></span> <span data-ttu-id="7fa77-125">Tyto hodnoty si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="7fa77-125">Make a note of these values.</span></span>

    - <span data-ttu-id="7fa77-126">ID klienta</span><span class="sxs-lookup"><span data-stu-id="7fa77-126">Client ID</span></span>
    - <span data-ttu-id="7fa77-127">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="7fa77-127">Tenant ID</span></span>
    - <span data-ttu-id="7fa77-128">Klíčů služby Active Directory (stejný jako ten, který je zadaný výše)</span><span class="sxs-lookup"><span data-stu-id="7fa77-128">Active Directory key (same as the one entered above)</span></span>
    - <span data-ttu-id="7fa77-129">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="7fa77-129">Subscription ID</span></span>

### <a name="set-up-the-automation-account"></a><span data-ttu-id="7fa77-130">Nastavit účet Automation</span><span class="sxs-lookup"><span data-stu-id="7fa77-130">Set up the Automation Account</span></span>

1. <span data-ttu-id="7fa77-131">Přihlaste se k Azure a otevřete svůj účet Automation.</span><span class="sxs-lookup"><span data-stu-id="7fa77-131">Log on to Azure and open your Automation account.</span></span>
2. <span data-ttu-id="7fa77-132">Klikněte na tlačítko **prostředky** dlaždici otevřete seznam prostředků.</span><span class="sxs-lookup"><span data-stu-id="7fa77-132">Click **Assets** tile to open the list of assets.</span></span>
3. <span data-ttu-id="7fa77-133">Klikněte na tlačítko **moduly** dlaždici otevřete seznamu modulů.</span><span class="sxs-lookup"><span data-stu-id="7fa77-133">Click **Modules** tile to open the list of modules.</span></span>
4. <span data-ttu-id="7fa77-134">Klikněte na tlačítko **+ přidat modul** spuštění tlačítko a okna Přidat modul.</span><span class="sxs-lookup"><span data-stu-id="7fa77-134">Click **+ Add a module** button and the Add module blade is launched.</span></span>

    ![Nastavení účtu Automation.](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. <span data-ttu-id="7fa77-136">Po výběru `DataTransformationApp.zip` souboru z místního počítače, klikněte na tlačítko **OK** Import modulu.</span><span class="sxs-lookup"><span data-stu-id="7fa77-136">After you have selected the `DataTransformationApp.zip` file from your local computer, click **OK** to import the module.</span></span>

   <span data-ttu-id="7fa77-137">Jakmile Azure Automation importuje modul ke svému účtu, extrahuje metadata o modulu.</span><span class="sxs-lookup"><span data-stu-id="7fa77-137">When Azure Automation imports a module to your account, it extracts metadata about the module.</span></span> <span data-ttu-id="7fa77-138">Tato operace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="7fa77-138">This operation may take a couple of minutes.</span></span>

   ![Nastavení účtu Automation.](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. <span data-ttu-id="7fa77-140">Po dokončení procesu obdržíte oznámení, který je nasazován modul a jiné oznámení.</span><span class="sxs-lookup"><span data-stu-id="7fa77-140">You receive a notification that the module is being deployed and another notification when the process is complete.</span></span>  <span data-ttu-id="7fa77-141">Stav můžete zkontrolovat **moduly** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7fa77-141">You can also check the status in **Modules** tile.</span></span>

### <a name="to-import-the-runbook-that-triggers-the-job-definition"></a><span data-ttu-id="7fa77-142">Chcete-li importovat sady runbook, která aktivuje definici úlohy</span><span class="sxs-lookup"><span data-stu-id="7fa77-142">To import the runbook that triggers the job definition</span></span>

1. <span data-ttu-id="7fa77-143">Na webu Azure Portal otevřete účet Automation.</span><span class="sxs-lookup"><span data-stu-id="7fa77-143">In the Azure portal, open your Automation account.</span></span>
2. <span data-ttu-id="7fa77-144">Klikněte na tlačítko **Runbooky** dlaždici otevřete seznam runbooků.</span><span class="sxs-lookup"><span data-stu-id="7fa77-144">Click **Runbooks** tile to open the list of runbooks.</span></span>
3. <span data-ttu-id="7fa77-145">Klikněte na tlačítko **+ přidat runbook** a potom **importovat stávající runbook**.</span><span class="sxs-lookup"><span data-stu-id="7fa77-145">Click **+ Add a runbook** and then **Import an existing runbook**.</span></span>

   ![Importovat stávající runbook](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. <span data-ttu-id="7fa77-147">Klikněte na tlačítko **soubor sady Runbook** a vyberte soubor k importu `Trigger-DataTransformation-Job.ps1`.</span><span class="sxs-lookup"><span data-stu-id="7fa77-147">Click **Runbook file** and select the file to import `Trigger-DataTransformation-Job.ps1`.</span></span>
5. <span data-ttu-id="7fa77-148">Klikněte na tlačítko **vytvořit** k importu sady runbook.</span><span class="sxs-lookup"><span data-stu-id="7fa77-148">Click **Create** to import the runbook.</span></span> <span data-ttu-id="7fa77-149">Nový runbook se zobrazí v seznamu sad runbook pro účet služby Automation.</span><span class="sxs-lookup"><span data-stu-id="7fa77-149">The new runbook appears in the list of runbooks for the Automation account.</span></span>
7. <span data-ttu-id="7fa77-150">Klikněte na tlačítko **aktivační událost-DataTransformation-Job** runbook a potom klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="7fa77-150">Click **Trigger-DataTransformation-Job** runbook and then click **Edit**.</span></span>
8. <span data-ttu-id="7fa77-151">Klikněte na tlačítko **publikovat** a potom **Ano** po zobrazení výzvy k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="7fa77-151">Click **Publish** and then **Yes** when prompted for confirmation.</span></span>


### <a name="to-run-the-runbook"></a><span data-ttu-id="7fa77-152">Spuštění sady runbook:</span><span class="sxs-lookup"><span data-stu-id="7fa77-152">To run the runbook:</span></span>
1. <span data-ttu-id="7fa77-153">Na webu Azure Portal otevřete účet Automation.</span><span class="sxs-lookup"><span data-stu-id="7fa77-153">In the Azure portal, open your Automation account.</span></span>
2. <span data-ttu-id="7fa77-154">Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.</span><span class="sxs-lookup"><span data-stu-id="7fa77-154">Click the **Runbooks** tile to open the list of runbooks.</span></span>
3. <span data-ttu-id="7fa77-155">Klikněte na tlačítko **aktivační událost DataTransformation-Job**.</span><span class="sxs-lookup"><span data-stu-id="7fa77-155">Click **Trigger-DataTransformation-Job**.</span></span>
4. <span data-ttu-id="7fa77-156">Kliknutím na **Spustit** spustíte runbook.</span><span class="sxs-lookup"><span data-stu-id="7fa77-156">Click **Start** to start the runbook.</span></span>

   ![Spuštění runbooku](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. <span data-ttu-id="7fa77-158">V **spuštění sady runbook** okno, zadejte všechny parametry.</span><span class="sxs-lookup"><span data-stu-id="7fa77-158">In the **Start runbook** blade, enter all the parameters.</span></span> <span data-ttu-id="7fa77-159">Klikněte na tlačítko **OK** se odeslat úlohu transformace Data.</span><span class="sxs-lookup"><span data-stu-id="7fa77-159">Click **OK** to submit the Data Transformation job.</span></span>

   ![Spuštění runbooku](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a><span data-ttu-id="7fa77-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7fa77-161">Next steps</span></span>

<span data-ttu-id="7fa77-162">[Data Manager zařízení StorSimple pomocí uživatelského rozhraní pro transformaci dat](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="7fa77-162">[Use StorSimple Data Manager UI to transform your data](storsimple-data-manager-ui.md).</span></span>