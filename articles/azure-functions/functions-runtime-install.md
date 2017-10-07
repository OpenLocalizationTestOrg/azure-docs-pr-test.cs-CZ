---
title: Instalace funkce modulu CLR aaaAzure | Microsoft Docs
description: Jak tooInstall hello Azure Functions Runtime
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 67c6d10b5c0ac43e880d29cff0ae7b099f82bdb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-azure-functions-runtime-preview"></a>Nainstalujte hello Azure funkce Runtime Preview

Pokud chcete tooinstall hello Azure Functions Runtime preview, postupujte podle těchto kroků:

1. Zajistěte, aby byl že váš počítač úspěšně projde hello minimální požadavky
1. Stáhnout hello [Azure funkce Runtime Preview instalační](https://aka.ms/azafr). 
1. Nainstalujte hello Runtime funkce Azure preview
1. Konfigurace dokončení hello hello Runtime funkce Azure Preview

## <a name="prerequisites"></a>Požadavky

Než nainstalujete hello Azure Functions Runtime preview, musíte mít následující hello:

1. Počítač se systémem Microsoft Windows Server 2016 nebo Microsoft Windows 10 Creators Update (Professional nebo Enterprise Edition).
1. Instance SQL serveru spuštěna v rámci vaší sítě.  Požadavek na minimální verzi je SQL Server Express.

## <a name="install-hello-azure-functions-runtime-preview"></a>Nainstalujte hello Azure funkce Runtime Preview

Instalační program preview Azure Functions Runtime Hello vás provede instalaci hello hello Azure Functions Runtime preview správy a rolí pracovního procesu.  Je možné tooinstall hello správy a roli pracovního procesu na hello stejný počítač.  Však jako přidáte další funkce, je nutné nasadit více rolí pracovního procesu na další počítače toobe možné tooscale funkcí do více pracovníků.

## <a name="install-hello-management-and-worker-role-on-hello-same-machine"></a>Nainstalujte hello správy a roli pracovního procesu na hello stejný počítač

1. Spusťte instalační program Azure funkce Runtime Preview hello.

    ![Instalační program Preview Runtime Azure Functions][1]

1. **Klikněte na tlačítko Další** záloh za hello první fázi instalačního programu hello
1. Jakmile jste četli hello podmínky hello **smlouvy EULA**, **políčko hello** tooaccept hello podmínky a **klikněte na tlačítko Další** tooadvance.
1. Nyní vyberte hello role chcete tooinstall na tomto počítači **Role správy funkce** nebo **Role pracovního procesu funkce** a **kliknutím na tlačítko Další**

    ![Instalační program Preview Runtime Azure Functions - výběr Role][3]

    > [!NOTE]
    > Můžete nainstalovat hello **Role pracovního procesu funkce** na mnoha dalších počítačů toodo Ano, postupujte podle těchto pokynů a vybrat pouze **Role pracovního procesu funkce** v hello Instalační služby.

1. **Klikněte na tlačítko Další** toohave hello **instalační program modulu Runtime Azure funkce** nainstalovat na svůj počítač.
1. Po dokončení hello instalační program se spustí hello **nástroj Konfigurace modulu Runtime Azure funkce**.

    ![Dokončení instalační Preview Runtime Azure Functions][5]

    > [!NOTE]
    > Pokud instalujete na **Windows 10** a hello **kontejneru** funkce není povolená dříve, hello **Azure Functions Runtime** instalační program zobrazí výzvu tooreboot váš počítač toocomplete hello nainstalovat.

## <a name="configure-hello-azure-functions-runtime"></a>Konfigurace hello Azure Functions Runtime

toocomplete hello Azure Functions Runtime instalace musí dokončit konfiguraci hello.

1. Hello **nástroj Konfigurace Runtime funkce Azure** ukazuje, jaké role jsou nainstalovány v počítači.

    ![Nástroj konfigurace Preview Runtime Azure Functions][6]

1. Klikněte na tlačítko hello **databáze** zadejte hello **podrobnosti připojení pro vaše Instance systému SQL Server** a **kliknutím na tlačítko použít**.  To je nutné v pořadí toohello toocreate Azure Functions Runtime databáze toosupport hello modulu Runtime.
    
    ![Konfigurace databáze Preview modulu Runtime Azure Functions][7]

1. Klikněte na tlačítko hello **pověření** kartě.  Na této obrazovce je nutné vytvořit dva nové přihlašovací údaje pro použití s sdílení souborů pro hostování všech Azure Functions.  **Zadejte uživatelské jméno a heslo** kombinací pro hello **vlastníka sdílené složky souboru** a hello **uživatele sdílené složky souboru** a klikněte na tlačítko **použít**.

    ![Přihlašovací údaje Preview Runtime Azure Functions][8]

1. Klikněte na tlačítko hello **sdílené složky** kartě.  V této obrazovce je nutné zadat podrobnosti o hello hello **umístění sdílené složky**.  To je možné vytvořit za vás, nebo můžete použít existující sdílené složky a klikněte na tlačítko **použít**.  Pokud vyberete nové umístění sdílené složky, musíte zadat adresář pro použití ve hello Azure Functions Runtime.
    
    ![Azure Functions Runtime Preview sdílené složky][9]

1. Klikněte na tlačítko hello **IIS** kartě.  Tato karta obsahuje podrobnosti hello hello webů ve službě IIS tento hello instalace modulu Runtime funkce Azure vytvoří.  **Klikněte na tlačítko použít** toocomplete.

    ![Azure Functions Preview Runtime služby IIS][10]

1. Klikněte na tlačítko hello **služby** kartě.  Tato karta zobrazuje stav hello hello služeb v instalaci Azure Functions Runtime.  Pokud po počáteční konfiguraci hello **služba Aktivace hostitele funkce Azure** neběží klikněte na tlačítko **spustit službu**

    ![Dokončení Configruation Preview Runtime Azure Functions][11]

1. Nakonec procházet toohello **modulu Runtime Azure Functions na portálu** jako`https://<machinename>/`

    ![Modul Runtime Azure Functions na portálu Preview][12]


<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-InstallComplete.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png