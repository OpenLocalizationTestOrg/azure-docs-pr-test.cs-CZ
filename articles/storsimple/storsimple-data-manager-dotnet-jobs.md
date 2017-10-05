---
title: "Použití sady .NET SDK pro Microsoft Azure StorSimple Data Manager úlohy | Microsoft Docs"
description: "Další informace o použití sady .NET SDK ke spuštění úlohy StorSimple Manager dat (soukromém náhledu)."
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
ms.openlocfilehash: 44d243a034b20b99faf284c8615e470bc6f9d020
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-the-net-sdk-to-initiate-data-transformation-private-preview"></a><span data-ttu-id="d017e-103">Pomocí .net SDK zahájíte transformace dat (soukromém náhledu).</span><span class="sxs-lookup"><span data-stu-id="d017e-103">Use the .Net SDK to initiate data transformation (Private Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="d017e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d017e-104">Overview</span></span>

<span data-ttu-id="d017e-105">Tento článek vysvětluje, jak můžete použít funkci transformaci dat v rámci služby StorSimple Manager dat k transformaci dat zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="d017e-105">This article explains how you can use the data transformation feature within the StorSimple Data Manager service to transform StorSimple device data.</span></span> <span data-ttu-id="d017e-106">Transformovaná data se pak spotřebované jinými službami Azure v cloudu.</span><span class="sxs-lookup"><span data-stu-id="d017e-106">The transformed data is then consumed by other Azure services in the cloud.</span></span> <span data-ttu-id="d017e-107">Článek má také návod k usnadnění vytváření ukázkovou aplikaci konzoly .NET zahájíte úlohu transformace dat a pak ji sledovat pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="d017e-107">The article also has a walkthrough to help create a sample .NET console application to initiate a data transformation job and then track it for completion.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d017e-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d017e-108">Prerequisites</span></span>

<span data-ttu-id="d017e-109">Než začnete, ujistěte se, zda máte:</span><span class="sxs-lookup"><span data-stu-id="d017e-109">Before you begin, ensure that you have:</span></span>
*   <span data-ttu-id="d017e-110">Systém s Visual Studio 2012, 2013, 2015 nebo 2017 nainstalována.</span><span class="sxs-lookup"><span data-stu-id="d017e-110">A system with Visual Studio 2012, 2013, 2015, or 2017 installed.</span></span>
*   <span data-ttu-id="d017e-111">Nainstalovat Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="d017e-111">Azure Powershell installed.</span></span> <span data-ttu-id="d017e-112">[Stáhnout prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="d017e-112">[Download Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
*   <span data-ttu-id="d017e-113">Nastavení konfigurace k chybě při inicializaci úlohu transformace dat (pokyny k získání těchto nastavení jsou zde uvedena).</span><span class="sxs-lookup"><span data-stu-id="d017e-113">Configuration settings to initialize the Data Transformation job (instructions to obtain these settings are included here).</span></span>
*   <span data-ttu-id="d017e-114">Definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="d017e-114">A job definition that has been correctly configured in a Hybrid Data Resource within a Resource Group.</span></span>
*   <span data-ttu-id="d017e-115">Všechny požadované knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="d017e-115">All the required dlls.</span></span> <span data-ttu-id="d017e-116">Stáhněte si tyto knihovny DLL z [úložiště GitHub](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).</span><span class="sxs-lookup"><span data-stu-id="d017e-116">Download these dlls from the [GitHub repository](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).</span></span>
*   <span data-ttu-id="d017e-117">`Get-ConfigurationParams.ps1`[skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) z úložiště githubu.</span><span class="sxs-lookup"><span data-stu-id="d017e-117">`Get-ConfigurationParams.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) from the github repository.</span></span>

## <a name="step-by-step"></a><span data-ttu-id="d017e-118">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="d017e-118">Step-by-step</span></span>

<span data-ttu-id="d017e-119">Proveďte následující kroky spusťte úlohu transformace dat pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="d017e-119">Perform the following steps to use .NET to launch a data transformation job.</span></span>

1. <span data-ttu-id="d017e-120">Chcete-li načíst konfigurační parametry, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d017e-120">To retrieve the configuration parameters, do the following steps:</span></span>
    1. <span data-ttu-id="d017e-121">Stažení `Get-ConfigurationParams.ps1` ze skriptu úložiště github v `C:\DataTransformation` umístění.</span><span class="sxs-lookup"><span data-stu-id="d017e-121">Download the `Get-ConfigurationParams.ps1` from the github repository script in `C:\DataTransformation` location.</span></span>
    1. <span data-ttu-id="d017e-122">Spustit `Get-ConfigurationParams.ps1` skript z úložiště githubu.</span><span class="sxs-lookup"><span data-stu-id="d017e-122">Run the `Get-ConfigurationParams.ps1` script from the github repository.</span></span> <span data-ttu-id="d017e-123">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d017e-123">Type the following command:</span></span>

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        <span data-ttu-id="d017e-124">Abyste mohli předávat žádné hodnoty pro ActiveDirectoryKey a AppName.</span><span class="sxs-lookup"><span data-stu-id="d017e-124">You can pass in any values for the ActiveDirectoryKey and AppName.</span></span>


2. <span data-ttu-id="d017e-125">Skript vypíše následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d017e-125">This script outputs the following values:</span></span>
    * <span data-ttu-id="d017e-126">ID klienta</span><span class="sxs-lookup"><span data-stu-id="d017e-126">Client ID</span></span>
    * <span data-ttu-id="d017e-127">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="d017e-127">Tenant ID</span></span>
    * <span data-ttu-id="d017e-128">Klíčů služby Active Directory (stejný jako ten, který je zadaný výše)</span><span class="sxs-lookup"><span data-stu-id="d017e-128">Active Directory key (same as the one entered above)</span></span>
    * <span data-ttu-id="d017e-129">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="d017e-129">Subscription ID</span></span>

3. <span data-ttu-id="d017e-130">Pomocí sady Visual Studio 2012, 2013 nebo 2015, vytvořte konzolovou aplikaci C# .NET.</span><span class="sxs-lookup"><span data-stu-id="d017e-130">Using Visual Studio 2012, 2013 or 2015, create a C# .NET console application.</span></span>

    1. <span data-ttu-id="d017e-131">Spusťte **Visual Studio 2012/2013 nebo 2015**.</span><span class="sxs-lookup"><span data-stu-id="d017e-131">Launch **Visual Studio 2012/2013/2015**.</span></span>
    1. <span data-ttu-id="d017e-132">Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="d017e-132">Click **File**, point to **New**, and click **Project**.</span></span>
    2. <span data-ttu-id="d017e-133">Rozbalte **Šablony** a vyberte **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="d017e-133">Expand **Templates**, and select **Visual C#**.</span></span>
    3. <span data-ttu-id="d017e-134">V seznamu typů projektů napravo vyberte **Konzolová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d017e-134">Select **Console Application** from the list of project types on the right.</span></span>
    4. <span data-ttu-id="d017e-135">Zadejte **DataTransformationApp** pro **název**.</span><span class="sxs-lookup"><span data-stu-id="d017e-135">Enter **DataTransformationApp** for the **Name**.</span></span>
    5. <span data-ttu-id="d017e-136">Vyberte **C:\DataTransformation** pro **umístění**.</span><span class="sxs-lookup"><span data-stu-id="d017e-136">Select **C:\DataTransformation** for the **Location**.</span></span>
    6. <span data-ttu-id="d017e-137">Kliknutím na tlačítko **OK** vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="d017e-137">Click **OK** to create the project.</span></span>

4.  <span data-ttu-id="d017e-138">Nyní přidejte všechny knihovny DLL v [knihovny DLL](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) složky jako **odkazy** v projektu, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d017e-138">Now, add all DLLs present in the [dlls](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) folder as **References** in the project that you created.</span></span> <span data-ttu-id="d017e-139">Ke stažení souborů dll, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d017e-139">To download the dll files, do the following:</span></span>

    1. <span data-ttu-id="d017e-140">V sadě Visual Studio, přejděte na **zobrazení > Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="d017e-140">In Visual Studio, go to **View > Solution Explorer**.</span></span>
    1. <span data-ttu-id="d017e-141">Klikněte na šipku nalevo od projekt aplikace transformaci dat.</span><span class="sxs-lookup"><span data-stu-id="d017e-141">Click the arrow to the left of Data Transformation App project.</span></span> <span data-ttu-id="d017e-142">Klikněte na tlačítko **odkazy** , klikněte pravým tlačítkem na **přidat odkaz na**.</span><span class="sxs-lookup"><span data-stu-id="d017e-142">Click **References** and then right-click to **Add Reference**.</span></span>
    2. <span data-ttu-id="d017e-143">Přejděte do umístění složky balíčků, vyberte všechny knihovny DLL a klikněte na tlačítko **přidat**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d017e-143">Browse to the location of the packages folder, select all the DLLs and click **Add**, and then click **OK**.</span></span>

5. <span data-ttu-id="d017e-144">Do zdrojového souboru (Program.cs) v projektu přidejte následující příkazy **using**.</span><span class="sxs-lookup"><span data-stu-id="d017e-144">Add the following **using** statements to the source file (Program.cs) in the project.</span></span>

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. <span data-ttu-id="d017e-145">Následující kód inicializuje instance úlohy transformace data.</span><span class="sxs-lookup"><span data-stu-id="d017e-145">The following code initializes the data transformation job instance.</span></span> <span data-ttu-id="d017e-146">Přidejte tuto v **metodu Main**.</span><span class="sxs-lookup"><span data-stu-id="d017e-146">Add this in the **Main method**.</span></span> <span data-ttu-id="d017e-147">Nahraďte hodnoty parametrů konfigurace jako dříve získali.</span><span class="sxs-lookup"><span data-stu-id="d017e-147">Replace the values of configuration parameters as obtained earlier.</span></span> <span data-ttu-id="d017e-148">Zařadit hodnoty **název skupiny prostředků** a **název zdroje dat hybridní**.</span><span class="sxs-lookup"><span data-stu-id="d017e-148">Plug in the values of **Resource Group Name** and **Hybrid Data Resource name**.</span></span> <span data-ttu-id="d017e-149">**Název skupiny prostředků** je ten, který hostuje datový prostředek hybridní, na kterém byl nakonfigurován definici úlohy.</span><span class="sxs-lookup"><span data-stu-id="d017e-149">The **Resource Group Name** is the one that hosts the Hybrid Data Resource on which the job definition was configured.</span></span>

    ```
    // Setup the configuration parameters.
    var configParams = new ConfigurationParams
    {
        ClientId = "client-id",
        TenantId = "tenant-id",
        ActiveDirectoryKey = "active-directory-key",
        SubscriptionId = "subscription-id",
        ResourceGroupName = "resource-group-name",
        ResourceName = "resource-name"
    };

    // Initialize the Data Transformation Job instance.
    DataTransformationJob dataTransformationJob = new DataTransformationJob(configParams);

    ```

7. <span data-ttu-id="d017e-150">Zadejte parametry, pomocí kterých je potřeba spustit definici úlohy</span><span class="sxs-lookup"><span data-stu-id="d017e-150">Specify the parameters with which the job definition needs to be run</span></span>

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    <span data-ttu-id="d017e-151">(NEBO)</span><span class="sxs-lookup"><span data-stu-id="d017e-151">(OR)</span></span>

    <span data-ttu-id="d017e-152">Pokud chcete změnit parametry definice úlohy během doby běhu, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="d017e-152">If you want to change the job definition parameters during run time, then add the following code:</span></span>

    ```
    string jobDefinitionName = "job-definition-name";
    // Must start with a '\'
    var rootDirectories = new List<string> {@"\root"};

    // Name of the volume on the StorSimple device.
    var volumeNames = new List<string> {"volume-name"};

    var dataTransformationInput = new DataTransformationInput
    {
        // If you require the latest existing backup to be picked else use TakeNow to trigger a new backup.
        BackupChoice = BackupChoice.UseExistingLatest.ToString(),
        // Name of the StorSimple device.
        DeviceName = "device-name",
        // Name of the container in Azure storage where the files will be placed after execution.
        ContainerName = "container-name",
        // File name filter (search pattern) to be applied on files under the root directory. * - Match all files.
        FileNameFilter = "*",
        // List of root directories.
        RootDirectories = rootDirectories,
        // Name of the volume on StorSimple device on which the relevant data is present. 
        VolumeNames = volumeNames
    };
    
    ```

8. <span data-ttu-id="d017e-153">Po inicializaci přidejte následující kód k aktivaci úlohy transformace dat v definici úlohy.</span><span class="sxs-lookup"><span data-stu-id="d017e-153">After the initialization, add the following code to trigger a data transformation job on the job definition.</span></span> <span data-ttu-id="d017e-154">Zařadit do příslušné **název definice úlohy**.</span><span class="sxs-lookup"><span data-stu-id="d017e-154">Plug in the appropriate **Job Definition Name**.</span></span>

    ```
    // Trigger a job, retrieve the jobId and the retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. <span data-ttu-id="d017e-155">Tato úloha odešle odpovídající nachází v kořenovém adresáři souborů na svazku zařízení StorSimple do zadaného kontejneru.</span><span class="sxs-lookup"><span data-stu-id="d017e-155">This job uploads the matched files present under the root directory on the StorSimple volume to the specified container.</span></span> <span data-ttu-id="d017e-156">Při odeslání souboru se zahodí zprávy ve frontě (ve stejném účtu úložiště jako kontejner) se stejným názvem jako definici úlohy.</span><span class="sxs-lookup"><span data-stu-id="d017e-156">When a file is uploaded, a message is dropped in the queue (in the same storage account as the container) with the same name as the job definition.</span></span> <span data-ttu-id="d017e-157">Tato zpráva slouží jako trigger k zahájení dalšího zpracování souboru.</span><span class="sxs-lookup"><span data-stu-id="d017e-157">This message can be used as a trigger to initiate any further processing of the file.</span></span>

10. <span data-ttu-id="d017e-158">Jakmile úloha byla spuštěna, přidejte následující kód se sledovat úlohu pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="d017e-158">Once the job has been triggered, add the following code to track the job for completion.</span></span>

    ```
    Job jobDetails = null;

    // Poll the job.
    do
    {
        jobDetails = dataTransformationJob.GetJob(jobDefinitionName, jobId);

        // Wait before polling for the status again.
        Thread.Sleep(TimeSpan.FromSeconds(retryAfter));

    } while (jobDetails.Status == JobStatus.InProgress);

    // Completion status of the job.
    Console.WriteLine("JobStatus: {0}", jobDetails.Status);
    
    // To hold the console before exiting.
    Console.Read();

    ```


## <a name="next-steps"></a><span data-ttu-id="d017e-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d017e-159">Next steps</span></span>

<span data-ttu-id="d017e-160">[Data Manager zařízení StorSimple pomocí uživatelského rozhraní pro transformaci dat](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="d017e-160">[Use StorSimple Data Manager UI to transform your data](storsimple-data-manager-ui.md).</span></span>
