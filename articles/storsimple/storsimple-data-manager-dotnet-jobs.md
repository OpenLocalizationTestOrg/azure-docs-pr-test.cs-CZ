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
# <a name="use-hello-net-sdk-tooinitiate-data-transformation-private-preview"></a>Použití hello .net SDK transformaci dat tooinitiate (soukromém náhledu).

## <a name="overview"></a>Přehled

Tento článek vysvětluje, jak můžete použít funkce transformace hello dat v rámci tootransform služby StorSimple Manager dat hello data zařízení StorSimple. Hello Transformovaná data se pak spotřebované jinými službami Azure v cloudu hello. Hello článek má také návod toohelp vytvoření tooinitiate aplikace konzoly .NET ukázkové úlohy transformace dat a pak ho sledování pro dokončení.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, zda máte:
*   Systém s Visual Studio 2012, 2013, 2015 nebo 2017 nainstalována.
*   Nainstalovat Azure Powershell. [Stáhnout prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Konfigurace nastavení tooinitialize hello transformaci dat úlohy (pokyny tooobtain tato nastavení jsou zahrnuty v tomto poli).
*   Definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.
*   Všechny knihovny DLL hello vyžaduje. Stáhněte si tyto knihovny DLL z hello [úložiště GitHub](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   `Get-ConfigurationParams.ps1`[skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) z úložiště githubu hello.

## <a name="step-by-step"></a>Podrobný postup

Proveďte následující kroky toouse .NET toolaunch úlohu transformace datového hello.

1. parametry konfigurace hello tooretrieve, hello následující kroky:
    1. Stáhnout hello `Get-ConfigurationParams.ps1` ze skriptu úložiště github hello v `C:\DataTransformation` umístění.
    1. Spustit hello `Get-ConfigurationParams.ps1` skript z úložiště github hello. Zadejte hello následující příkaz:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        Abyste mohli předávat žádné hodnoty pro hello ActiveDirectoryKey a AppName.


2. Skript vypíše hello následující hodnoty:
    * ID klienta
    * ID tenanta
    * Klíčů služby Active Directory (stejný jako jeden výše uvedených hello)
    * ID předplatného

3. Pomocí sady Visual Studio 2012, 2013 nebo 2015, vytvořte konzolovou aplikaci C# .NET.

    1. Spusťte **Visual Studio 2012/2013 nebo 2015**.
    1. Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**.
    2. Rozbalte **Šablony** a vyberte **Visual C#**.
    3. Vyberte **konzolové aplikace** hello seznamu typů projektu na hello správné.
    4. Zadejte **DataTransformationApp** pro hello **název**.
    5. Vyberte **C:\DataTransformation** pro hello **umístění**.
    6. Klikněte na tlačítko **OK** toocreate hello projektu.

4.  Nyní přidejte všechny knihovny DLL v hello [knihovny DLL](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) složky jako **odkazy** v hello projektu, který jste vytvořili. soubory knihoven dll hello toodownload, hello následující:

    1. V sadě Visual Studio přejděte příliš**zobrazení > Průzkumníku řešení**.
    1. Klikněte na tlačítko hello šipka doleva toohello aplikace transformaci dat projektu. Klikněte na tlačítko **odkazy** a potom klikněte pravým tlačítkem na příliš**přidat odkaz na**.
    2. Vyhledejte umístění toohello hello balíčky složky, vyberte všechny knihovny DLL hello a klikněte na **přidat**a potom klikněte na **OK**.

5. Přidejte následující hello **pomocí** příkazy toohello zdrojový soubor (Program.cs) v projektu hello.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. Následující kód Hello inicializuje instance úlohy transformace dat hello. Přidejte tuto v hello **metodu Main**. Nahraďte hodnoty hello nad parametry konfigurace jako dříve získali. Zařadit hodnoty hello **název skupiny prostředků** a **název zdroje dat hybridní**. Hello **název skupiny prostředků** hello je ten, který je hostitelem hello hybridní datový prostředek, na které hello byla nakonfigurována definice úlohy.

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

7. Zadejte hello spuštění parametry, pomocí které hello definice úlohy musí toobe

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    (NEBO)

    Pokud chcete parametry definice úlohy hello toochange během doby běhu, přidejte následující kód hello:

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

8. Po inicializaci hello přidejte následující kód tootrigger úlohu transformace dat v definici úlohy hello hello. Zařadit hello odpovídající **název definice úlohy**.

    ```
    // Trigger a job, retrieve hello jobId and hello retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. Tato úloha ukládání hello shodná souborech pod kořenovým adresářem hello na hello StorSimple svazku toohello zadaného kontejneru. Při odeslání souboru je vyřazeno zprávu ve frontě hello (v hello stejný účet úložiště jako kontejner hello) s hello stejný název jako definice úlohy hello. Tuto zprávu můžete použít jako aktivační událost tooinitiate, žádné další zpracování souboru hello.

10. Jakmile má byla spuštěna úloha hello, přidejte hello následující úlohu hello tootrack kód pro dokončení.

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


## <a name="next-steps"></a>Další kroky

[Pomocí uživatelského rozhraní Správce dat StorSimple tootransform dat](storsimple-data-manager-ui.md).
