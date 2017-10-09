---
title: aaaInstall .NET na role Azure Cloud Services | Microsoft Docs
description: "Tento článek popisuje, jak toomanually instalovat hello rozhraní .NET Framework na vaše cloudové služby webových a pracovních rolí"
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: 
ms.assetid: 8d1243dc-879c-4d1f-9ed0-eecd1f6a6653
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: adegeo
ms.openlocfilehash: 45f0f30221292f98c591511b091b02ebe1c1272c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-net-on-azure-cloud-services-roles"></a>Instalace rozhraní .NET pro role Azure Cloud Services
Tento článek popisuje, jak tooinstall verze rozhraní .NET Framework, které nejsou součástí hello Azure hostovaného operačního systému. Rozhraní .NET můžete použít na tooconfigure hello hostovaného operačního systému vaší cloudové služby webových a pracovních rolí.

Například můžete nainstalovat rozhraní .NET 4.6.1 na hello hostovaného operačního systému rodiny 4, který není součástí žádné verzi rozhraní .NET 4.6. (hello hostovaného operačního systému rodiny 5 se dodávají s .NET 4.6.) Hello nejnovější informace o hello uvolní Azure hostovaného operačního systému najdete v tématu hello [Azure hostovaného operačního systému verze zprávy](cloud-services-guestos-update-matrix.md). 

>[!IMPORTANT]
>Hello sadu Azure SDK 2.9 obsahuje omezení nasazení .NET 4.6 na hello hostovaného operačního systému rodiny 4 nebo dřívější. Oprava hello omezení je k dispozici na hello [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) lokality.

tooinstall .NET na webových a pracovních rolí, patří instalační program webové rozhraní .NET hello jako součást projekt cloudové služby. Spusťte instalační program hello jako součást hello role spuštění úlohy. 

## <a name="add-hello-net-installer-tooyour-project"></a>Přidání projektu tooyour instalační program rozhraní .NET hello
Instalační program webové hello toodownload pro hello rozhraní .NET Framework, zvolte hello verze, kterou chcete tooinstall:

* [Instalační program webové 4.7 rozhraní .NET](http://go.microsoft.com/fwlink/?LinkId=825298)
* [Instalační program webové rozhraní .NET 4.6.1](http://go.microsoft.com/fwlink/?LinkId=671729)

Instalační program hello tooadd pro *webové* role:
  1. V **Průzkumníku řešení**v části **role** v projekt cloudové služby, klikněte pravým tlačítkem na vaše *webové* roli a vyberte **přidat**  >  **Novou složku**. Vytvořte složku s názvem **bin**.
  2. Klikněte pravým tlačítkem na složku Koš hello a vyberte **přidat** > **existující položka**. Vyberte instalační program rozhraní .NET hello a přidejte ho toohello složky Koš.
  
Instalační program hello tooadd pro *pracovní* role:
* Klikněte pravým tlačítkem na vaše *pracovní* roli a vyberte **přidat** > **existující položka**. Vyberte instalační program rozhraní .NET hello a přidejte ho toohello role. 

Když soubory jsou přidány do této obsahu složky způsob toohello role, se automaticky přidají balíček tooyour cloudové služby. Hello soubory jsou pak konzistentní umístění nasazené tooa hello virtuálního počítače. Tento postup opakujte pro každý web a pracovní role v rámci cloudové služby tak, aby všechny role kopii instalačního programu hello.

> [!NOTE]
> Musíte nainstalovat rozhraní .NET 4.6.1 na vaše cloudové služby role i v případě, že vaše aplikace zaměřuje .NET 4.6. Hello hostovaného operačního systému zahrnuje hello znalostní báze Knowledge Base [aktualizace 3098779](https://support.microsoft.com/kb/3098779) a [aktualizace 3097997](https://support.microsoft.com/kb/3097997). Problémy se můžou objevit při spuštění aplikace rozhraní .NET, pokud .NET 4.6 je nainstalovaná na aktualizace hello znalostní báze Knowledge Base. tooavoid tyto potíže, nainstalujte rozhraní .NET 4.6.1 spíše než verze 4.6. Další informace najdete v tématu hello [článku znalostní báze 3118750](https://support.microsoft.com/kb/3118750).
> 
> 

![Obsah role s soubory Instalační služby systému][1]

## <a name="define-startup-tasks-for-your-roles"></a>Definování spuštění úloh pro své role
Spuštění úlohy tooperform operace můžete před zahájením roli. Instalace hello rozhraní .NET Framework jako součástí hello spuštění úlohy zajistí, že hello framework se instaluje, před spuštěním jakékoli kódu aplikace. Další informace o spuštění úlohy najdete v tématu [spouštět úlohy spuštění v Azure](cloud-services-startup-tasks.md). 

1. Přidejte následující soubor ServiceDefinition.csdef obsahu toohello pod hello hello **WebRole** nebo **WorkerRole** uzlu u všech rolí:
   
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```
   
    Hello předchozí konfigurace spustí příkaz konzoly hello `install.cmd` s tooinstall oprávnění správce hello rozhraní .NET Framework. Konfigurace Hello také vytvoří **LocalStorage** element s názvem **NETFXInstall**. spuštění skriptu Hello nastaví hello dočasnou složku toouse tento prostředek Místní úložiště. 
    
    > [!IMPORTANT]
    > tooensure opravte instalaci hello framework, velikost hello sadu tento prostředek tooat alespoň 1 024 MB.
    
    Další informace o spuštění úlohy najdete v tématu [běžné Azure Cloud Services spuštění úlohy](cloud-services-startup-tasks-common.md).

2. Vytvořte soubor s názvem **soubor install.cmd** a přidejte následující hello nainstalovat toohello souboru skriptu.

    Hello skript kontroluje, zda zadaná verze hello hello rozhraní .NET Framework je již nainstalována na počítači hello dotazováním hello registru. Pokud není nainstalovaná verze rozhraní .NET hello, je otevřené hello instalačního programu webové rozhraní .NET. toohelp vyřešte všechny problémy, hello skript protokoluje všechny aktivity toohello soubor startuptasklog-(aktuálním datem a časem) .txt uložených v **InstallLogs** místní úložiště.

    > [!IMPORTANT]
    > Pomocí základní textového editoru, jako je soubor install.cmd hello toocreate Poznámkový blok systému Windows. Pokud používáte Visual Studio toocreate textového souboru a změnit hello rozšíření too.cmd, hello soubor může obsahovat stále značka pořadí bajtů ve formátu UTF-8. Tato značka může způsobit chybu při spuštění prvního řádku hello hello skriptu. tooavoid tato chyba, zkontrolujte hello je první řádek hello skript REM příkaz, který mohou být přeskočeny hello bajtů pořadí zpracování. 
    > 
    >
   
    ```cmd
    REM Set hello value of netfx tooinstall appropriate .NET Framework. 
    REM ***** tooinstall .NET 4.5.2 set hello variable netfx too"NDP452" *****
    REM ***** tooinstall .NET 4.6 set hello variable netfx too"NDP46" *****
    REM ***** tooinstall .NET 4.6.1 set hello variable netfx too"NDP461" *****
    REM ***** tooinstall .NET 4.6.2 set hello variable netfx too"NDP462" *****
    REM ***** tooinstall .NET 4.7 set hello variable netfx too"NDP47" *****
    set netfx="NDP47"

    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."

    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit

    REM ***** Needed toocorrectly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%

    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP47" goto NDP47
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp

    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp

    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp

    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"

    :NDP47
    set "netfxinstallfile=NDP47-KB3186500-Web.exe"
    set netfxregkey="0x707FE"

    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%

    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed

    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%    
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%

    :restart
    echo Restarting toocomplete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%

    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%

    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%

    :exit
    EXIT /B 0
    ```
   
   > [!NOTE]
   > Tento skript je ukázkou, jak tooinstall .NET 4.5.2 nebo verze 4.6 pro kontinuitu, i když je již k dispozici v rozhraní .NET 4.5.2 hello Azure hostovaného operačního systému. Měli byste přímo nainstalovat rozhraní .NET 4.6.1 spíše než verze 4.6, jak je popsáno v hello [článku znalostní báze 3118750](https://support.microsoft.com/kb/3118750).
   > 
   > 

3. Přidat hello soubor install.cmd souboru tooeach role pomocí **přidat** > **existující položka** v **Průzkumníku řešení** jak je popsáno výše v tomto tématu. 

    Po dokončení tohoto kroku se všechny role měli hello souboru Instalační služby rozhraní .NET a soubor install.cmd hello.

   ![Obsah role se všechny soubory][2]

## <a name="configure-diagnostics-tootransfer-startup-logs-tooblob-storage"></a>Konfigurace diagnostiky tootransfer spuštění protokoly tooBlob úložiště
toosimplify řešení problémů instalace, můžete nakonfigurovat Azure Diagnostics tootransfer všechny soubory protokolů generovaných hello spuštění skriptu nebo hello úložiště objektů Blob tooAzure instalační program rozhraní .NET. Když použijete tuto metodu, můžete zobrazit protokoly hello stahování souborů protokolu hello z úložiště objektů Blob, místo aby tooremote plochu do hello role.

Diagnostika tooconfigure, otevřete soubor diagnostics.wadcfgx hello a přidejte následující obsah v části hello hello **adresáře** uzlu: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Tato konfigurace XML nakonfiguruje diagnostiky tootransfer hello soubory v adresáři protokolu hello v hello **NETFXInstall** toohello prostředků účtu úložiště diagnostiky v hello **netfx instalace** kontejner objektů blob.

## <a name="deploy-your-cloud-service"></a>Nasazení cloudové služby
Při nasazení cloudové služby, nainstalujte hello spuštění úlohy hello rozhraní .NET Framework, pokud je ještě není nainstalován. Role cloudové služby jsou v hello *zaneprázdněn* stavu hello framework je během instalace. Pokud hello framework instalace vyžaduje restartování, může také restartovat hello služby rolí. 

## <a name="additional-resources"></a>Další zdroje
* [Instalace hello rozhraní .NET Framework][Installing hello .NET Framework]
* [Zjištění nainstalovaných verzí rozhraní .NET Framework][How to: Determine Which .NET Framework Versions Are Installed]
* [Řešení potíží s instalací rozhraní .NET Framework][Troubleshooting .NET Framework Installations]

[How to: Determine Which .NET Framework Versions Are Installed]: https://msdn.microsoft.com/library/hh925568.aspx
[Installing hello .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Troubleshooting .NET Framework Installations]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
