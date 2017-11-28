---
title: "pro úlohy Microsoft Azure StorSimple Data Manager aaaUse .NET SDK | Microsoft Docs"
description: "Zjistěte, jak .NET SDK toolaunch toouse StorSimple Manager dat úlohy (soukromém náhledu)."
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
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: b07fe64369574c994fd28d42786aa02dca435ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-net-sdk-tooinitiate-data-transformation-private-preview"></a><span data-ttu-id="88bc7-103">Použití hello .net SDK transformaci dat tooinitiate (soukromém náhledu).</span><span class="sxs-lookup"><span data-stu-id="88bc7-103">Use hello .Net SDK tooinitiate data transformation (Private Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="88bc7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="88bc7-104">Overview</span></span>

<span data-ttu-id="88bc7-105">Tento článek vysvětluje, jak můžete použít funkce transformace hello dat v rámci tootransform služby StorSimple Manager dat hello data zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="88bc7-105">This article explains how you can use hello data transformation feature within hello StorSimple Data Manager service tootransform StorSimple device data.</span></span> <span data-ttu-id="88bc7-106">Hello Transformovaná data se pak spotřebované jinými službami Azure v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="88bc7-106">hello transformed data is then consumed by other Azure services in hello cloud.</span></span> <span data-ttu-id="88bc7-107">Hello článek má také návod toohelp vytvoření tooinitiate aplikace konzoly .NET ukázkové úlohy transformace dat a pak ho sledování pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="88bc7-107">hello article also has a walkthrough toohelp create a sample .NET console application tooinitiate a data transformation job and then track it for completion.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88bc7-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="88bc7-108">Prerequisites</span></span>

<span data-ttu-id="88bc7-109">Než začnete, ujistěte se, zda máte:</span><span class="sxs-lookup"><span data-stu-id="88bc7-109">Before you begin, ensure that you have:</span></span>
*   <span data-ttu-id="88bc7-110">Systém s Visual Studio 2012, 2013, 2015 nebo 2017 nainstalována.</span><span class="sxs-lookup"><span data-stu-id="88bc7-110">A system with Visual Studio 2012, 2013, 2015, or 2017 installed.</span></span>
*   <span data-ttu-id="88bc7-111">Nainstalovat Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="88bc7-111">Azure Powershell installed.</span></span> <span data-ttu-id="88bc7-112">[Stáhnout prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="88bc7-112">[Download Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
*   <span data-ttu-id="88bc7-113">Konfigurace nastavení tooinitialize hello transformaci dat úlohy (pokyny tooobtain tato nastavení jsou zahrnuty v tomto poli).</span><span class="sxs-lookup"><span data-stu-id="88bc7-113">Configuration settings tooinitialize hello Data Transformation job (instructions tooobtain these settings are included here).</span></span>
*   <span data-ttu-id="88bc7-114">Definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="88bc7-114">A job definition that has been correctly configured in a Hybrid Data Resource within a Resource Group.</span></span>
*   <span data-ttu-id="88bc7-115">Všechny knihovny DLL hello vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="88bc7-115">All hello required dlls.</span></span> <span data-ttu-id="88bc7-116">Stáhněte si tyto knihovny DLL z hello [úložiště GitHub](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).</span><span class="sxs-lookup"><span data-stu-id="88bc7-116">Download these dlls from hello [GitHub repository](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).</span></span>
*   <span data-ttu-id="88bc7-117">`Get-ConfigurationParams.ps1`[skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) z úložiště githubu hello.</span><span class="sxs-lookup"><span data-stu-id="88bc7-117">`Get-ConfigurationParams.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) from hello github repository.</span></span>

## <a name="step-by-step"></a><span data-ttu-id="88bc7-118">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="88bc7-118">Step-by-step</span></span>

<span data-ttu-id="88bc7-119">Proveďte následující kroky toouse .NET toolaunch úlohu transformace datového hello.</span><span class="sxs-lookup"><span data-stu-id="88bc7-119">Perform hello following steps toouse .NET toolaunch a data transformation job.</span></span>

1. <span data-ttu-id="88bc7-120">parametry konfigurace hello tooretrieve, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="88bc7-120">tooretrieve hello configuration parameters, do hello following steps:</span></span>
    1. <span data-ttu-id="88bc7-121">Stáhnout hello `Get-ConfigurationParams.ps1` ze skriptu úložiště github hello v `C:\DataTransformation` umístění.</span><span class="sxs-lookup"><span data-stu-id="88bc7-121">Download hello `Get-ConfigurationParams.ps1` from hello github repository script in `C:\DataTransformation` location.</span></span>
    1. <span data-ttu-id="88bc7-122">Spustit hello `Get-ConfigurationParams.ps1` skript z úložiště github hello.</span><span class="sxs-lookup"><span data-stu-id="88bc7-122">Run hello `Get-ConfigurationParams.ps1` script from hello github repository.</span></span> <span data-ttu-id="88bc7-123">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="88bc7-123">Type hello following command:</span></span>

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        <span data-ttu-id="88bc7-124">Abyste mohli předávat žádné hodnoty pro hello ActiveDirectoryKey a AppName.</span><span class="sxs-lookup"><span data-stu-id="88bc7-124">You can pass in any values for hello ActiveDirectoryKey and AppName.</span></span>


2. <span data-ttu-id="88bc7-125">Skript vypíše hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="88bc7-125">This script outputs hello following values:</span></span>
    * <span data-ttu-id="88bc7-126">ID klienta</span><span class="sxs-lookup"><span data-stu-id="88bc7-126">Client ID</span></span>
    * <span data-ttu-id="88bc7-127">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="88bc7-127">Tenant ID</span></span>
    * <span data-ttu-id="88bc7-128">Klíčů služby Active Directory (stejný jako jeden výše uvedených hello)</span><span class="sxs-lookup"><span data-stu-id="88bc7-128">Active Directory key (same as hello one entered above)</span></span>
    * <span data-ttu-id="88bc7-129">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="88bc7-129">Subscription ID</span></span>

3. <span data-ttu-id="88bc7-130">Pomocí sady Visual Studio 2012, 2013 nebo 2015, vytvořte konzolovou aplikaci C# .NET.</span><span class="sxs-lookup"><span data-stu-id="88bc7-130">Using Visual Studio 2012, 2013 or 2015, create a C# .NET console application.</span></span>

    1. <span data-ttu-id="88bc7-131">Spusťte **Visual Studio 2012/2013 nebo 2015**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-131">Launch **Visual Studio 2012/2013/2015**.</span></span>
    1. <span data-ttu-id="88bc7-132">Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-132">Click **File**, point too**New**, and click **Project**.</span></span>
    2. <span data-ttu-id="88bc7-133">Rozbalte **Šablony** a vyberte **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-133">Expand **Templates**, and select **Visual C#**.</span></span>
    3. <span data-ttu-id="88bc7-134">Vyberte **konzolové aplikace** hello seznamu typů projektu na hello správné.</span><span class="sxs-lookup"><span data-stu-id="88bc7-134">Select **Console Application** from hello list of project types on hello right.</span></span>
    4. <span data-ttu-id="88bc7-135">Zadejte **DataTransformationApp** pro hello **název**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-135">Enter **DataTransformationApp** for hello **Name**.</span></span>
    5. <span data-ttu-id="88bc7-136">Vyberte **C:\DataTransformation** pro hello **umístění**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-136">Select **C:\DataTransformation** for hello **Location**.</span></span>
    6. <span data-ttu-id="88bc7-137">Klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="88bc7-137">Click **OK** toocreate hello project.</span></span>

4.  <span data-ttu-id="88bc7-138">Nyní přidejte všechny knihovny DLL v hello [knihovny DLL](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) složky jako **odkazy** v hello projektu, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="88bc7-138">Now, add all DLLs present in hello [dlls](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) folder as **References** in hello project that you created.</span></span> <span data-ttu-id="88bc7-139">soubory knihoven dll hello toodownload, hello následující:</span><span class="sxs-lookup"><span data-stu-id="88bc7-139">toodownload hello dll files, do hello following:</span></span>

    1. <span data-ttu-id="88bc7-140">V sadě Visual Studio přejděte příliš**zobrazení > Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-140">In Visual Studio, go too**View > Solution Explorer**.</span></span>
    1. <span data-ttu-id="88bc7-141">Klikněte na tlačítko hello šipka doleva toohello aplikace transformaci dat projektu.</span><span class="sxs-lookup"><span data-stu-id="88bc7-141">Click hello arrow toohello left of Data Transformation App project.</span></span> <span data-ttu-id="88bc7-142">Klikněte na tlačítko **odkazy** a potom klikněte pravým tlačítkem na příliš**přidat odkaz na**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-142">Click **References** and then right-click too**Add Reference**.</span></span>
    2. <span data-ttu-id="88bc7-143">Vyhledejte umístění toohello hello balíčky složky, vyberte všechny knihovny DLL hello a klikněte na **přidat**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-143">Browse toohello location of hello packages folder, select all hello DLLs and click **Add**, and then click **OK**.</span></span>

5. <span data-ttu-id="88bc7-144">Přidejte následující hello **pomocí** příkazy toohello zdrojový soubor (Program.cs) v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="88bc7-144">Add hello following **using** statements toohello source file (Program.cs) in hello project.</span></span>

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. <span data-ttu-id="88bc7-145">Následující kód Hello inicializuje instance úlohy transformace dat hello.</span><span class="sxs-lookup"><span data-stu-id="88bc7-145">hello following code initializes hello data transformation job instance.</span></span> <span data-ttu-id="88bc7-146">Přidejte tuto v hello **metodu Main**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-146">Add this in hello **Main method**.</span></span> <span data-ttu-id="88bc7-147">Nahraďte hodnoty hello nad parametry konfigurace jako dříve získali.</span><span class="sxs-lookup"><span data-stu-id="88bc7-147">Replace hello values of configuration parameters as obtained earlier.</span></span> <span data-ttu-id="88bc7-148">Zařadit hodnoty hello **název skupiny prostředků** a **název zdroje dat hybridní**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-148">Plug in hello values of **Resource Group Name** and **Hybrid Data Resource name**.</span></span> <span data-ttu-id="88bc7-149">Hello **název skupiny prostředků** hello je ten, který je hostitelem hello hybridní datový prostředek, na které hello byla nakonfigurována definice úlohy.</span><span class="sxs-lookup"><span data-stu-id="88bc7-149">hello **Resource Group Name** is hello one that hosts hello Hybrid Data Resource on which hello job definition was configured.</span></span>

    ```
    // Setup hello configuration parameters.
    var configParams = new ConfigurationParams
    {
        ClientId = "client-id",
        TenantId = "tenant-id",
        ActiveDirectoryKey = "active-directory-key",
        SubscriptionId = "subscription-id",
        ResourceGroupName = "resource-group-name",
        ResourceName = "resource-name"
    };

    // Initialize hello Data Transformation Job instance.
    DataTransformationJob dataTransformationJob = new DataTransformationJob(configParams);

    ```

7. <span data-ttu-id="88bc7-150">Zadejte hello spuštění parametry, pomocí které hello definice úlohy musí toobe</span><span class="sxs-lookup"><span data-stu-id="88bc7-150">Specify hello parameters with which hello job definition needs toobe run</span></span>

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    <span data-ttu-id="88bc7-151">(NEBO)</span><span class="sxs-lookup"><span data-stu-id="88bc7-151">(OR)</span></span>

    <span data-ttu-id="88bc7-152">Pokud chcete parametry definice úlohy hello toochange během doby běhu, přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="88bc7-152">If you want toochange hello job definition parameters during run time, then add hello following code:</span></span>

    ```
    string jobDefinitionName = "job-definition-name";
    // Must start with a '\'
    var rootDirectories = new List<string> {@"\root"};

    // Name of hello volume on hello StorSimple device.
    var volumeNames = new List<string> {"volume-name"};

    var dataTransformationInput = new DataTransformationInput
    {
        // If you require hello latest existing backup toobe picked else use TakeNow tootrigger a new backup.
        BackupChoice = BackupChoice.UseExistingLatest.ToString(),
        // Name of hello StorSimple device.
        DeviceName = "device-name",
        // Name of hello container in Azure storage where hello files will be placed after execution.
        ContainerName = "container-name",
        // File name filter (search pattern) toobe applied on files under hello root directory. * - Match all files.
        FileNameFilter = "*",
        // List of root directories.
        RootDirectories = rootDirectories,
        // Name of hello volume on StorSimple device on which hello relevant data is present. 
        VolumeNames = volumeNames
    };
    
    ```

8. <span data-ttu-id="88bc7-153">Po inicializaci hello přidejte následující kód tootrigger úlohu transformace dat v definici úlohy hello hello.</span><span class="sxs-lookup"><span data-stu-id="88bc7-153">After hello initialization, add hello following code tootrigger a data transformation job on hello job definition.</span></span> <span data-ttu-id="88bc7-154">Zařadit hello odpovídající **název definice úlohy**.</span><span class="sxs-lookup"><span data-stu-id="88bc7-154">Plug in hello appropriate **Job Definition Name**.</span></span>

    ```
    // Trigger a job, retrieve hello jobId and hello retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. <span data-ttu-id="88bc7-155">Tato úloha ukládání hello shodná souborech pod kořenovým adresářem hello na hello StorSimple svazku toohello zadaného kontejneru.</span><span class="sxs-lookup"><span data-stu-id="88bc7-155">This job uploads hello matched files present under hello root directory on hello StorSimple volume toohello specified container.</span></span> <span data-ttu-id="88bc7-156">Při odeslání souboru je vyřazeno zprávu ve frontě hello (v hello stejný účet úložiště jako kontejner hello) s hello stejný název jako definice úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="88bc7-156">When a file is uploaded, a message is dropped in hello queue (in hello same storage account as hello container) with hello same name as hello job definition.</span></span> <span data-ttu-id="88bc7-157">Tuto zprávu můžete použít jako aktivační událost tooinitiate, žádné další zpracování souboru hello.</span><span class="sxs-lookup"><span data-stu-id="88bc7-157">This message can be used as a trigger tooinitiate any further processing of hello file.</span></span>

10. <span data-ttu-id="88bc7-158">Jakmile má byla spuštěna úloha hello, přidejte hello následující úlohu hello tootrack kód pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="88bc7-158">Once hello job has been triggered, add hello following code tootrack hello job for completion.</span></span>

    ```
    Job jobDetails = null;

    // Poll hello job.
    do
    {
        jobDetails = dataTransformationJob.GetJob(jobDefinitionName, jobId);

        // Wait before polling for hello status again.
        Thread.Sleep(TimeSpan.FromSeconds(retryAfter));

    } while (jobDetails.Status == JobStatus.InProgress);

    // Completion status of hello job.
    Console.WriteLine("JobStatus: {0}", jobDetails.Status);
    
    // toohold hello console before exiting.
    Console.Read();

    ```


## <a name="next-steps"></a><span data-ttu-id="88bc7-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="88bc7-159">Next steps</span></span>

<span data-ttu-id="88bc7-160">[Pomocí uživatelského rozhraní Správce dat StorSimple tootransform dat](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="88bc7-160">[Use StorSimple Data Manager UI tootransform your data](storsimple-data-manager-ui.md).</span></span>
