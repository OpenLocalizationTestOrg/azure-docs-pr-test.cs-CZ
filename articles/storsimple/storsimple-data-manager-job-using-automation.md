---
title: "Azure Automation tootrigger aaaUse úlohu | Microsoft Docs"
description: "Zjistěte, jak toouse Azure Automation pro spuštění úlohy StorSimple Manager dat (soukromém náhledu)."
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
ms.openlocfilehash: 0d9d6e5e6d52ed27ca759ed7e949b5f5dfd319c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-automation-tootrigger-a-job-private-preview"></a><span data-ttu-id="15d46-103">Použití Azure Automation tootrigger úlohy (soukromém náhledu).</span><span class="sxs-lookup"><span data-stu-id="15d46-103">Use Azure Automation tootrigger a job (Private Preview)</span></span>

<span data-ttu-id="15d46-104">Tento článek popisuje, jak toouse Azure Automation tootrigger StorSimple Data Manager úlohy.</span><span class="sxs-lookup"><span data-stu-id="15d46-104">This articles describes how toouse Azure Automation tootrigger a StorSimple Data Manager job.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15d46-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="15d46-105">Prerequisites</span></span>

<span data-ttu-id="15d46-106">Než začnete, ujistěte se, zda máte:</span><span class="sxs-lookup"><span data-stu-id="15d46-106">Before you begin, ensure that you have:</span></span>

*   <span data-ttu-id="15d46-107">Nainstalovat Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="15d46-107">Azure Powershell installed.</span></span> <span data-ttu-id="15d46-108">[Stáhnout prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="15d46-108">[Download Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
*   <span data-ttu-id="15d46-109">Konfigurace nastavení tooinitialize hello transformaci dat úlohy (pokyny tooobtain tato nastavení jsou zahrnuty v tomto poli).</span><span class="sxs-lookup"><span data-stu-id="15d46-109">Configuration settings tooinitialize hello Data Transformation job (instructions tooobtain these settings are included here).</span></span>
*   <span data-ttu-id="15d46-110">Definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="15d46-110">A job definition that has been correctly configured in a Hybrid Data Resource within a resource group.</span></span>
*   <span data-ttu-id="15d46-111">Stáhněte si `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) soubor z úložiště github hello.</span><span class="sxs-lookup"><span data-stu-id="15d46-111">Download `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) file from hello github repository.</span></span>
*   <span data-ttu-id="15d46-112">Stáhněte si `Get-ConfigurationParams.ps1` [skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) z úložiště githubu hello.</span><span class="sxs-lookup"><span data-stu-id="15d46-112">Download `Get-ConfigurationParams.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) from hello github repository.</span></span>
*   <span data-ttu-id="15d46-113">Stáhněte si `Trigger-DataTransformation-Job.ps1` [skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) z úložiště githubu hello.</span><span class="sxs-lookup"><span data-stu-id="15d46-113">Download `Trigger-DataTransformation-Job.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) from hello github repository.</span></span>

## <a name="step-by-step"></a><span data-ttu-id="15d46-114">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="15d46-114">Step-by-step</span></span>

### <a name="get-azure-active-directory-permissions-for-hello-automation-job-toorun-hello-job-definition"></a><span data-ttu-id="15d46-115">Získat oprávnění služby Azure Active Directory pro definici úlohy hello automatizace úloh toorun hello</span><span class="sxs-lookup"><span data-stu-id="15d46-115">Get Azure Active Directory permissions for hello automation job toorun hello job definition</span></span>

1. <span data-ttu-id="15d46-116">tooretrieve hello parametry konfigurace pro službu Active Directory, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="15d46-116">tooretrieve hello configuration parameters for Active Directory, do hello following steps:</span></span>

    1. <span data-ttu-id="15d46-117">Otevřete prostředí Windows PowerShell v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="15d46-117">Open Windows PowerShell in your local machine.</span></span> <span data-ttu-id="15d46-118">Ujistěte se, že [prostředí Azure PowerShell](https://azure.microsoft.com/downloads/) je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="15d46-118">Ensure that [Azure PowerShell](https://azure.microsoft.com/downloads/) is installed.</span></span>
    1. <span data-ttu-id="15d46-119">Spustit hello `Get-ConfigurationParams.ps1` skriptu (ve složce hello jste stáhli výše).</span><span class="sxs-lookup"><span data-stu-id="15d46-119">Run hello `Get-ConfigurationParams.ps1` script (in hello folder you downloaded above).</span></span> <span data-ttu-id="15d46-120">Zadejte následující příkaz v okně PowerShell hello hello:</span><span class="sxs-lookup"><span data-stu-id="15d46-120">Type hello following command in hello PowerShell window:</span></span>

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        <span data-ttu-id="15d46-121">Hello ActiveDirectoryKey je heslo, které použijete později.</span><span class="sxs-lookup"><span data-stu-id="15d46-121">hello ActiveDirectoryKey is a password that you use later.</span></span> <span data-ttu-id="15d46-122">Zadejte heslo podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="15d46-122">Enter a password of your choice.</span></span> <span data-ttu-id="15d46-123">AppName může být libovolný řetězec.</span><span class="sxs-lookup"><span data-stu-id="15d46-123">AppName can be any string.</span></span>

2. <span data-ttu-id="15d46-124">Skript vypíše hello následující hodnoty, které se mají použít při spuštění hello automation runbook.</span><span class="sxs-lookup"><span data-stu-id="15d46-124">This script outputs hello following values that should be used while triggering hello automation runbook.</span></span> <span data-ttu-id="15d46-125">Tyto hodnoty si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="15d46-125">Make a note of these values.</span></span>

    - <span data-ttu-id="15d46-126">ID klienta</span><span class="sxs-lookup"><span data-stu-id="15d46-126">Client ID</span></span>
    - <span data-ttu-id="15d46-127">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="15d46-127">Tenant ID</span></span>
    - <span data-ttu-id="15d46-128">Klíčů služby Active Directory (stejný jako jeden výše uvedených hello)</span><span class="sxs-lookup"><span data-stu-id="15d46-128">Active Directory key (same as hello one entered above)</span></span>
    - <span data-ttu-id="15d46-129">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="15d46-129">Subscription ID</span></span>

### <a name="set-up-hello-automation-account"></a><span data-ttu-id="15d46-130">Nastavit hello účet Automation.</span><span class="sxs-lookup"><span data-stu-id="15d46-130">Set up hello Automation Account</span></span>

1. <span data-ttu-id="15d46-131">Přihlaste se na tooAzure a otevřete svůj účet Automation.</span><span class="sxs-lookup"><span data-stu-id="15d46-131">Log on tooAzure and open your Automation account.</span></span>
2. <span data-ttu-id="15d46-132">Klikněte na tlačítko **prostředky** dlaždice tooopen hello seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="15d46-132">Click **Assets** tile tooopen hello list of assets.</span></span>
3. <span data-ttu-id="15d46-133">Klikněte na tlačítko **moduly** dlaždice tooopen hello seznamu modulů.</span><span class="sxs-lookup"><span data-stu-id="15d46-133">Click **Modules** tile tooopen hello list of modules.</span></span>
4. <span data-ttu-id="15d46-134">Klikněte na tlačítko **+ přidat modul** spuštění tlačítko a hello okno Přidat modul.</span><span class="sxs-lookup"><span data-stu-id="15d46-134">Click **+ Add a module** button and hello Add module blade is launched.</span></span>

    ![Nastavení účtu Automation.](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. <span data-ttu-id="15d46-136">Po výběru hello `DataTransformationApp.zip` souboru z místního počítače, klikněte na tlačítko **OK** tooimport hello modulu.</span><span class="sxs-lookup"><span data-stu-id="15d46-136">After you have selected hello `DataTransformationApp.zip` file from your local computer, click **OK** tooimport hello module.</span></span>

   <span data-ttu-id="15d46-137">Jakmile Azure Automation importuje účet tooyour modulu, extrahuje metadata o hello modulu.</span><span class="sxs-lookup"><span data-stu-id="15d46-137">When Azure Automation imports a module tooyour account, it extracts metadata about hello module.</span></span> <span data-ttu-id="15d46-138">Tato operace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="15d46-138">This operation may take a couple of minutes.</span></span>

   ![Nastavení účtu Automation.](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. <span data-ttu-id="15d46-140">Přijímat oznámení Tenhle modul hello se nasazuje a jiné oznámení po dokončení procesu hello.</span><span class="sxs-lookup"><span data-stu-id="15d46-140">You receive a notification that hello module is being deployed and another notification when hello process is complete.</span></span>  <span data-ttu-id="15d46-141">Můžete také zkontrolovat stav hello ve **moduly** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="15d46-141">You can also check hello status in **Modules** tile.</span></span>

### <a name="tooimport-hello-runbook-that-triggers-hello-job-definition"></a><span data-ttu-id="15d46-142">tooimport hello runbook, která aktivuje definice úlohy hello</span><span class="sxs-lookup"><span data-stu-id="15d46-142">tooimport hello runbook that triggers hello job definition</span></span>

1. <span data-ttu-id="15d46-143">V hello portálu Azure otevřete účet Automation.</span><span class="sxs-lookup"><span data-stu-id="15d46-143">In hello Azure portal, open your Automation account.</span></span>
2. <span data-ttu-id="15d46-144">Klikněte na tlačítko **Runbooky** dlaždice tooopen hello seznamu sad runbook.</span><span class="sxs-lookup"><span data-stu-id="15d46-144">Click **Runbooks** tile tooopen hello list of runbooks.</span></span>
3. <span data-ttu-id="15d46-145">Klikněte na tlačítko **+ přidat runbook** a potom **importovat stávající runbook**.</span><span class="sxs-lookup"><span data-stu-id="15d46-145">Click **+ Add a runbook** and then **Import an existing runbook**.</span></span>

   ![Importovat stávající runbook](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. <span data-ttu-id="15d46-147">Klikněte na tlačítko **soubor sady Runbook** a vyberte hello souboru tooimport `Trigger-DataTransformation-Job.ps1`.</span><span class="sxs-lookup"><span data-stu-id="15d46-147">Click **Runbook file** and select hello file tooimport `Trigger-DataTransformation-Job.ps1`.</span></span>
5. <span data-ttu-id="15d46-148">Klikněte na tlačítko **vytvořit** tooimport hello runbook.</span><span class="sxs-lookup"><span data-stu-id="15d46-148">Click **Create** tooimport hello runbook.</span></span> <span data-ttu-id="15d46-149">Nová sada runbook Hello se zobrazí v seznamu hello sad runbook pro hello účet Automation.</span><span class="sxs-lookup"><span data-stu-id="15d46-149">hello new runbook appears in hello list of runbooks for hello Automation account.</span></span>
7. <span data-ttu-id="15d46-150">Klikněte na tlačítko **aktivační událost-DataTransformation-Job** runbook a potom klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="15d46-150">Click **Trigger-DataTransformation-Job** runbook and then click **Edit**.</span></span>
8. <span data-ttu-id="15d46-151">Klikněte na tlačítko **publikovat** a potom **Ano** po zobrazení výzvy k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="15d46-151">Click **Publish** and then **Yes** when prompted for confirmation.</span></span>


### <a name="toorun-hello-runbook"></a><span data-ttu-id="15d46-152">toorun hello runbook:</span><span class="sxs-lookup"><span data-stu-id="15d46-152">toorun hello runbook:</span></span>
1. <span data-ttu-id="15d46-153">V hello portálu Azure otevřete účet Automation.</span><span class="sxs-lookup"><span data-stu-id="15d46-153">In hello Azure portal, open your Automation account.</span></span>
2. <span data-ttu-id="15d46-154">Klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.</span><span class="sxs-lookup"><span data-stu-id="15d46-154">Click hello **Runbooks** tile tooopen hello list of runbooks.</span></span>
3. <span data-ttu-id="15d46-155">Klikněte na tlačítko **aktivační událost DataTransformation-Job**.</span><span class="sxs-lookup"><span data-stu-id="15d46-155">Click **Trigger-DataTransformation-Job**.</span></span>
4. <span data-ttu-id="15d46-156">Klikněte na tlačítko **spustit** toostart hello runbook.</span><span class="sxs-lookup"><span data-stu-id="15d46-156">Click **Start** toostart hello runbook.</span></span>

   ![Spuštění runbooku](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. <span data-ttu-id="15d46-158">V hello **spuštění sady runbook** okno, zadejte všechny parametry hello.</span><span class="sxs-lookup"><span data-stu-id="15d46-158">In hello **Start runbook** blade, enter all hello parameters.</span></span> <span data-ttu-id="15d46-159">Klikněte na tlačítko **OK** toosubmit hello transformaci dat úlohy.</span><span class="sxs-lookup"><span data-stu-id="15d46-159">Click **OK** toosubmit hello Data Transformation job.</span></span>

   ![Spuštění runbooku](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a><span data-ttu-id="15d46-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15d46-161">Next steps</span></span>

<span data-ttu-id="15d46-162">[Pomocí uživatelského rozhraní Správce dat StorSimple tootransform dat](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="15d46-162">[Use StorSimple Data Manager UI tootransform your data](storsimple-data-manager-ui.md).</span></span>
