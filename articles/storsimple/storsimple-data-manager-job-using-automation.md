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
# <a name="use-azure-automation-tootrigger-a-job-private-preview"></a>Použití Azure Automation tootrigger úlohy (soukromém náhledu).

Tento článek popisuje, jak toouse Azure Automation tootrigger StorSimple Data Manager úlohy.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, zda máte:

*   Nainstalovat Azure Powershell. [Stáhnout prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Konfigurace nastavení tooinitialize hello transformaci dat úlohy (pokyny tooobtain tato nastavení jsou zahrnuty v tomto poli).
*   Definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.
*   Stáhněte si `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) soubor z úložiště github hello.
*   Stáhněte si `Get-ConfigurationParams.ps1` [skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) z úložiště githubu hello.
*   Stáhněte si `Trigger-DataTransformation-Job.ps1` [skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) z úložiště githubu hello.

## <a name="step-by-step"></a>Podrobný postup

### <a name="get-azure-active-directory-permissions-for-hello-automation-job-toorun-hello-job-definition"></a>Získat oprávnění služby Azure Active Directory pro definici úlohy hello automatizace úloh toorun hello

1. tooretrieve hello parametry konfigurace pro službu Active Directory, hello následující kroky:

    1. Otevřete prostředí Windows PowerShell v místním počítači. Ujistěte se, že [prostředí Azure PowerShell](https://azure.microsoft.com/downloads/) je nainstalovaná.
    1. Spustit hello `Get-ConfigurationParams.ps1` skriptu (ve složce hello jste stáhli výše). Zadejte následující příkaz v okně PowerShell hello hello:

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        Hello ActiveDirectoryKey je heslo, které použijete později. Zadejte heslo podle svého výběru. AppName může být libovolný řetězec.

2. Skript vypíše hello následující hodnoty, které se mají použít při spuštění hello automation runbook. Tyto hodnoty si poznamenejte.

    - ID klienta
    - ID tenanta
    - Klíčů služby Active Directory (stejný jako jeden výše uvedených hello)
    - ID předplatného

### <a name="set-up-hello-automation-account"></a>Nastavit hello účet Automation.

1. Přihlaste se na tooAzure a otevřete svůj účet Automation.
2. Klikněte na tlačítko **prostředky** dlaždice tooopen hello seznamu prostředků.
3. Klikněte na tlačítko **moduly** dlaždice tooopen hello seznamu modulů.
4. Klikněte na tlačítko **+ přidat modul** spuštění tlačítko a hello okno Přidat modul.

    ![Nastavení účtu Automation.](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. Po výběru hello `DataTransformationApp.zip` souboru z místního počítače, klikněte na tlačítko **OK** tooimport hello modulu.

   Jakmile Azure Automation importuje účet tooyour modulu, extrahuje metadata o hello modulu. Tato operace může trvat několik minut.

   ![Nastavení účtu Automation.](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. Přijímat oznámení Tenhle modul hello se nasazuje a jiné oznámení po dokončení procesu hello.  Můžete také zkontrolovat stav hello ve **moduly** dlaždici.

### <a name="tooimport-hello-runbook-that-triggers-hello-job-definition"></a>tooimport hello runbook, která aktivuje definice úlohy hello

1. V hello portálu Azure otevřete účet Automation.
2. Klikněte na tlačítko **Runbooky** dlaždice tooopen hello seznamu sad runbook.
3. Klikněte na tlačítko **+ přidat runbook** a potom **importovat stávající runbook**.

   ![Importovat stávající runbook](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. Klikněte na tlačítko **soubor sady Runbook** a vyberte hello souboru tooimport `Trigger-DataTransformation-Job.ps1`.
5. Klikněte na tlačítko **vytvořit** tooimport hello runbook. Nová sada runbook Hello se zobrazí v seznamu hello sad runbook pro hello účet Automation.
7. Klikněte na tlačítko **aktivační událost-DataTransformation-Job** runbook a potom klikněte na **upravit**.
8. Klikněte na tlačítko **publikovat** a potom **Ano** po zobrazení výzvy k potvrzení.


### <a name="toorun-hello-runbook"></a>toorun hello runbook:
1. V hello portálu Azure otevřete účet Automation.
2. Klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.
3. Klikněte na tlačítko **aktivační událost DataTransformation-Job**.
4. Klikněte na tlačítko **spustit** toostart hello runbook.

   ![Spuštění runbooku](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. V hello **spuštění sady runbook** okno, zadejte všechny parametry hello. Klikněte na tlačítko **OK** toosubmit hello transformaci dat úlohy.

   ![Spuštění runbooku](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a>Další kroky

[Pomocí uživatelského rozhraní Správce dat StorSimple tootransform dat](storsimple-data-manager-ui.md).
