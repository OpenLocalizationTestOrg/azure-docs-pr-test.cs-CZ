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
# <a name="install-net-on-azure-cloud-services-roles"></a><span data-ttu-id="0fe89-103">Instalace rozhraní .NET pro role Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="0fe89-103">Install .NET on Azure Cloud Services roles</span></span>
<span data-ttu-id="0fe89-104">Tento článek popisuje, jak tooinstall verze rozhraní .NET Framework, které nejsou součástí hello Azure hostovaného operačního systému.</span><span class="sxs-lookup"><span data-stu-id="0fe89-104">This article describes how tooinstall versions of .NET Framework that don't come with hello Azure Guest OS.</span></span> <span data-ttu-id="0fe89-105">Rozhraní .NET můžete použít na tooconfigure hello hostovaného operačního systému vaší cloudové služby webových a pracovních rolí.</span><span class="sxs-lookup"><span data-stu-id="0fe89-105">You can use .NET on hello Guest OS tooconfigure your cloud service web and worker roles.</span></span>

<span data-ttu-id="0fe89-106">Například můžete nainstalovat rozhraní .NET 4.6.1 na hello hostovaného operačního systému rodiny 4, který není součástí žádné verzi rozhraní .NET 4.6.</span><span class="sxs-lookup"><span data-stu-id="0fe89-106">For example, you can install .NET 4.6.1 on hello Guest OS family 4, which doesn't come with any release of .NET 4.6.</span></span> <span data-ttu-id="0fe89-107">(hello hostovaného operačního systému rodiny 5 se dodávají s .NET 4.6.) Hello nejnovější informace o hello uvolní Azure hostovaného operačního systému najdete v tématu hello [Azure hostovaného operačního systému verze zprávy](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="0fe89-107">(hello Guest OS family 5 does come with .NET 4.6.) For hello latest information on hello Azure Guest OS releases, see hello [Azure Guest OS release news](cloud-services-guestos-update-matrix.md).</span></span> 

>[!IMPORTANT]
><span data-ttu-id="0fe89-108">Hello sadu Azure SDK 2.9 obsahuje omezení nasazení .NET 4.6 na hello hostovaného operačního systému rodiny 4 nebo dřívější.</span><span class="sxs-lookup"><span data-stu-id="0fe89-108">hello Azure SDK 2.9 contains a restriction on deploying .NET 4.6 on hello Guest OS family 4 or earlier.</span></span> <span data-ttu-id="0fe89-109">Oprava hello omezení je k dispozici na hello [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) lokality.</span><span class="sxs-lookup"><span data-stu-id="0fe89-109">A fix for hello restriction is available on hello [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) site.</span></span>

<span data-ttu-id="0fe89-110">tooinstall .NET na webových a pracovních rolí, patří instalační program webové rozhraní .NET hello jako součást projekt cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0fe89-110">tooinstall .NET on your web and worker roles, include hello .NET web installer as part of your cloud service project.</span></span> <span data-ttu-id="0fe89-111">Spusťte instalační program hello jako součást hello role spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="0fe89-111">Start hello installer as part of hello role's startup tasks.</span></span> 

## <a name="add-hello-net-installer-tooyour-project"></a><span data-ttu-id="0fe89-112">Přidání projektu tooyour instalační program rozhraní .NET hello</span><span class="sxs-lookup"><span data-stu-id="0fe89-112">Add hello .NET installer tooyour project</span></span>
<span data-ttu-id="0fe89-113">Instalační program webové hello toodownload pro hello rozhraní .NET Framework, zvolte hello verze, kterou chcete tooinstall:</span><span class="sxs-lookup"><span data-stu-id="0fe89-113">toodownload hello web installer for hello .NET Framework, choose hello version that you want tooinstall:</span></span>

* [<span data-ttu-id="0fe89-114">Instalační program webové 4.7 rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="0fe89-114">.NET 4.7 web installer</span></span>](http://go.microsoft.com/fwlink/?LinkId=825298)
* [<span data-ttu-id="0fe89-115">Instalační program webové rozhraní .NET 4.6.1</span><span class="sxs-lookup"><span data-stu-id="0fe89-115">.NET 4.6.1 web installer</span></span>](http://go.microsoft.com/fwlink/?LinkId=671729)

<span data-ttu-id="0fe89-116">Instalační program hello tooadd pro *webové* role:</span><span class="sxs-lookup"><span data-stu-id="0fe89-116">tooadd hello installer for a *web* role:</span></span>
  1. <span data-ttu-id="0fe89-117">V **Průzkumníku řešení**v části **role** v projekt cloudové služby, klikněte pravým tlačítkem na vaše *webové* roli a vyberte **přidat**  >  **Novou složku**.</span><span class="sxs-lookup"><span data-stu-id="0fe89-117">In **Solution Explorer**, under **Roles** in your cloud service project, right-click your *web* role and select **Add** > **New Folder**.</span></span> <span data-ttu-id="0fe89-118">Vytvořte složku s názvem **bin**.</span><span class="sxs-lookup"><span data-stu-id="0fe89-118">Create a folder named **bin**.</span></span>
  2. <span data-ttu-id="0fe89-119">Klikněte pravým tlačítkem na složku Koš hello a vyberte **přidat** > **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="0fe89-119">Right-click hello bin folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="0fe89-120">Vyberte instalační program rozhraní .NET hello a přidejte ho toohello složky Koš.</span><span class="sxs-lookup"><span data-stu-id="0fe89-120">Select hello .NET installer and add it toohello bin folder.</span></span>
  
<span data-ttu-id="0fe89-121">Instalační program hello tooadd pro *pracovní* role:</span><span class="sxs-lookup"><span data-stu-id="0fe89-121">tooadd hello installer for a *worker* role:</span></span>
* <span data-ttu-id="0fe89-122">Klikněte pravým tlačítkem na vaše *pracovní* roli a vyberte **přidat** > **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="0fe89-122">Right-click your *worker* role and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="0fe89-123">Vyberte instalační program rozhraní .NET hello a přidejte ho toohello role.</span><span class="sxs-lookup"><span data-stu-id="0fe89-123">Select hello .NET installer and add it toohello role.</span></span> 

<span data-ttu-id="0fe89-124">Když soubory jsou přidány do této obsahu složky způsob toohello role, se automaticky přidají balíček tooyour cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0fe89-124">When files are added in this way toohello role content folder, they're automatically added tooyour cloud service package.</span></span> <span data-ttu-id="0fe89-125">Hello soubory jsou pak konzistentní umístění nasazené tooa hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0fe89-125">hello files are then deployed tooa consistent location on hello virtual machine.</span></span> <span data-ttu-id="0fe89-126">Tento postup opakujte pro každý web a pracovní role v rámci cloudové služby tak, aby všechny role kopii instalačního programu hello.</span><span class="sxs-lookup"><span data-stu-id="0fe89-126">Repeat this process for each web and worker role in your cloud service so that all roles have a copy of hello installer.</span></span>

> [!NOTE]
> <span data-ttu-id="0fe89-127">Musíte nainstalovat rozhraní .NET 4.6.1 na vaše cloudové služby role i v případě, že vaše aplikace zaměřuje .NET 4.6.</span><span class="sxs-lookup"><span data-stu-id="0fe89-127">You should install .NET 4.6.1 on your cloud service role even if your application targets .NET 4.6.</span></span> <span data-ttu-id="0fe89-128">Hello hostovaného operačního systému zahrnuje hello znalostní báze Knowledge Base [aktualizace 3098779](https://support.microsoft.com/kb/3098779) a [aktualizace 3097997](https://support.microsoft.com/kb/3097997).</span><span class="sxs-lookup"><span data-stu-id="0fe89-128">hello Guest OS includes hello Knowledge Base [update 3098779](https://support.microsoft.com/kb/3098779) and [update 3097997](https://support.microsoft.com/kb/3097997).</span></span> <span data-ttu-id="0fe89-129">Problémy se můžou objevit při spuštění aplikace rozhraní .NET, pokud .NET 4.6 je nainstalovaná na aktualizace hello znalostní báze Knowledge Base.</span><span class="sxs-lookup"><span data-stu-id="0fe89-129">Issues can occur when you run your .NET applications if .NET 4.6 is installed on top of hello Knowledge Base updates.</span></span> <span data-ttu-id="0fe89-130">tooavoid tyto potíže, nainstalujte rozhraní .NET 4.6.1 spíše než verze 4.6.</span><span class="sxs-lookup"><span data-stu-id="0fe89-130">tooavoid these issues, install .NET 4.6.1 rather than version 4.6.</span></span> <span data-ttu-id="0fe89-131">Další informace najdete v tématu hello [článku znalostní báze 3118750](https://support.microsoft.com/kb/3118750).</span><span class="sxs-lookup"><span data-stu-id="0fe89-131">For more information, see hello [Knowledge Base article 3118750](https://support.microsoft.com/kb/3118750).</span></span>
> 
> 

![Obsah role s soubory Instalační služby systému][1]

## <a name="define-startup-tasks-for-your-roles"></a><span data-ttu-id="0fe89-133">Definování spuštění úloh pro své role</span><span class="sxs-lookup"><span data-stu-id="0fe89-133">Define startup tasks for your roles</span></span>
<span data-ttu-id="0fe89-134">Spuštění úlohy tooperform operace můžete před zahájením roli.</span><span class="sxs-lookup"><span data-stu-id="0fe89-134">You can use startup tasks tooperform operations before a role starts.</span></span> <span data-ttu-id="0fe89-135">Instalace hello rozhraní .NET Framework jako součástí hello spuštění úlohy zajistí, že hello framework se instaluje, před spuštěním jakékoli kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="0fe89-135">Installing hello .NET Framework as part of hello startup task ensures that hello framework is installed before any application code is run.</span></span> <span data-ttu-id="0fe89-136">Další informace o spuštění úlohy najdete v tématu [spouštět úlohy spuštění v Azure](cloud-services-startup-tasks.md).</span><span class="sxs-lookup"><span data-stu-id="0fe89-136">For more information on startup tasks, see [Run startup tasks in Azure](cloud-services-startup-tasks.md).</span></span> 

1. <span data-ttu-id="0fe89-137">Přidejte následující soubor ServiceDefinition.csdef obsahu toohello pod hello hello **WebRole** nebo **WorkerRole** uzlu u všech rolí:</span><span class="sxs-lookup"><span data-stu-id="0fe89-137">Add hello following content toohello ServiceDefinition.csdef file under hello **WebRole** or **WorkerRole** node for all roles:</span></span>
   
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
   
    <span data-ttu-id="0fe89-138">Hello předchozí konfigurace spustí příkaz konzoly hello `install.cmd` s tooinstall oprávnění správce hello rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0fe89-138">hello preceding configuration runs hello console command `install.cmd` with administrator privileges tooinstall hello .NET Framework.</span></span> <span data-ttu-id="0fe89-139">Konfigurace Hello také vytvoří **LocalStorage** element s názvem **NETFXInstall**.</span><span class="sxs-lookup"><span data-stu-id="0fe89-139">hello configuration also creates a **LocalStorage** element named **NETFXInstall**.</span></span> <span data-ttu-id="0fe89-140">spuštění skriptu Hello nastaví hello dočasnou složku toouse tento prostředek Místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="0fe89-140">hello startup script sets hello temp folder toouse this local storage resource.</span></span> 
    
    > [!IMPORTANT]
    > <span data-ttu-id="0fe89-141">tooensure opravte instalaci hello framework, velikost hello sadu tento prostředek tooat alespoň 1 024 MB.</span><span class="sxs-lookup"><span data-stu-id="0fe89-141">tooensure correct installation of hello framework, set hello size of this resource tooat least 1,024 MB.</span></span>
    
    <span data-ttu-id="0fe89-142">Další informace o spuštění úlohy najdete v tématu [běžné Azure Cloud Services spuštění úlohy](cloud-services-startup-tasks-common.md).</span><span class="sxs-lookup"><span data-stu-id="0fe89-142">For more information about startup tasks, see [Common Azure Cloud Services startup tasks](cloud-services-startup-tasks-common.md).</span></span>

2. <span data-ttu-id="0fe89-143">Vytvořte soubor s názvem **soubor install.cmd** a přidejte následující hello nainstalovat toohello souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="0fe89-143">Create a file named **install.cmd** and add hello following install script toohello file.</span></span>

    <span data-ttu-id="0fe89-144">Hello skript kontroluje, zda zadaná verze hello hello rozhraní .NET Framework je již nainstalována na počítači hello dotazováním hello registru.</span><span class="sxs-lookup"><span data-stu-id="0fe89-144">hello script checks whether hello specified version of hello .NET Framework is already installed on hello machine by querying hello registry.</span></span> <span data-ttu-id="0fe89-145">Pokud není nainstalovaná verze rozhraní .NET hello, je otevřené hello instalačního programu webové rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="0fe89-145">If hello .NET version is not installed, then hello .NET web installer is opened.</span></span> <span data-ttu-id="0fe89-146">toohelp vyřešte všechny problémy, hello skript protokoluje všechny aktivity toohello soubor startuptasklog-(aktuálním datem a časem) .txt uložených v **InstallLogs** místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="0fe89-146">toohelp troubleshoot any issues, hello script logs all activity toohello file startuptasklog-(current date and time).txt that is stored in **InstallLogs** local storage.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="0fe89-147">Pomocí základní textového editoru, jako je soubor install.cmd hello toocreate Poznámkový blok systému Windows.</span><span class="sxs-lookup"><span data-stu-id="0fe89-147">Use a basic text editor like Windows Notepad toocreate hello install.cmd file.</span></span> <span data-ttu-id="0fe89-148">Pokud používáte Visual Studio toocreate textového souboru a změnit hello rozšíření too.cmd, hello soubor může obsahovat stále značka pořadí bajtů ve formátu UTF-8.</span><span class="sxs-lookup"><span data-stu-id="0fe89-148">If you use Visual Studio toocreate a text file and change hello extension too.cmd, hello file might still contain a UTF-8 byte order mark.</span></span> <span data-ttu-id="0fe89-149">Tato značka může způsobit chybu při spuštění prvního řádku hello hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="0fe89-149">This mark can cause an error when hello first line of hello script is run.</span></span> <span data-ttu-id="0fe89-150">tooavoid tato chyba, zkontrolujte hello je první řádek hello skript REM příkaz, který mohou být přeskočeny hello bajtů pořadí zpracování.</span><span class="sxs-lookup"><span data-stu-id="0fe89-150">tooavoid this error, make hello first line of hello script a REM statement that can be skipped by hello byte order processing.</span></span> 
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
   > <span data-ttu-id="0fe89-151">Tento skript je ukázkou, jak tooinstall .NET 4.5.2 nebo verze 4.6 pro kontinuitu, i když je již k dispozici v rozhraní .NET 4.5.2 hello Azure hostovaného operačního systému.</span><span class="sxs-lookup"><span data-stu-id="0fe89-151">This script shows how tooinstall .NET 4.5.2 or version 4.6 for continuity, even though .NET 4.5.2 is already available on hello Azure Guest OS.</span></span> <span data-ttu-id="0fe89-152">Měli byste přímo nainstalovat rozhraní .NET 4.6.1 spíše než verze 4.6, jak je popsáno v hello [článku znalostní báze 3118750](https://support.microsoft.com/kb/3118750).</span><span class="sxs-lookup"><span data-stu-id="0fe89-152">You should directly install .NET 4.6.1 rather than version 4.6, as described in hello [Knowledge Base article 3118750](https://support.microsoft.com/kb/3118750).</span></span>
   > 
   > 

3. <span data-ttu-id="0fe89-153">Přidat hello soubor install.cmd souboru tooeach role pomocí **přidat** > **existující položka** v **Průzkumníku řešení** jak je popsáno výše v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="0fe89-153">Add hello install.cmd file tooeach role by using **Add** > **Existing Item** in **Solution Explorer** as described earlier in this topic.</span></span> 

    <span data-ttu-id="0fe89-154">Po dokončení tohoto kroku se všechny role měli hello souboru Instalační služby rozhraní .NET a soubor install.cmd hello.</span><span class="sxs-lookup"><span data-stu-id="0fe89-154">After this step is complete, all roles should have hello .NET installer file and hello install.cmd file.</span></span>

   ![Obsah role se všechny soubory][2]

## <a name="configure-diagnostics-tootransfer-startup-logs-tooblob-storage"></a><span data-ttu-id="0fe89-156">Konfigurace diagnostiky tootransfer spuštění protokoly tooBlob úložiště</span><span class="sxs-lookup"><span data-stu-id="0fe89-156">Configure Diagnostics tootransfer startup logs tooBlob storage</span></span>
<span data-ttu-id="0fe89-157">toosimplify řešení problémů instalace, můžete nakonfigurovat Azure Diagnostics tootransfer všechny soubory protokolů generovaných hello spuštění skriptu nebo hello úložiště objektů Blob tooAzure instalační program rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="0fe89-157">toosimplify troubleshooting installation issues, you can configure Azure Diagnostics tootransfer any log files generated by hello startup script or hello .NET installer tooAzure Blob storage.</span></span> <span data-ttu-id="0fe89-158">Když použijete tuto metodu, můžete zobrazit protokoly hello stahování souborů protokolu hello z úložiště objektů Blob, místo aby tooremote plochu do hello role.</span><span class="sxs-lookup"><span data-stu-id="0fe89-158">By using this approach, you can view hello logs by downloading hello log files from Blob storage rather than having tooremote desktop into hello role.</span></span>

<span data-ttu-id="0fe89-159">Diagnostika tooconfigure, otevřete soubor diagnostics.wadcfgx hello a přidejte následující obsah v části hello hello **adresáře** uzlu:</span><span class="sxs-lookup"><span data-stu-id="0fe89-159">tooconfigure Diagnostics, open hello diagnostics.wadcfgx file and add hello following content under hello **Directories** node:</span></span> 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

<span data-ttu-id="0fe89-160">Tato konfigurace XML nakonfiguruje diagnostiky tootransfer hello soubory v adresáři protokolu hello v hello **NETFXInstall** toohello prostředků účtu úložiště diagnostiky v hello **netfx instalace** kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0fe89-160">This XML configures Diagnostics tootransfer hello files in hello log directory in hello **NETFXInstall** resource toohello Diagnostics storage account in hello **netfx-install** blob container.</span></span>

## <a name="deploy-your-cloud-service"></a><span data-ttu-id="0fe89-161">Nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="0fe89-161">Deploy your cloud service</span></span>
<span data-ttu-id="0fe89-162">Při nasazení cloudové služby, nainstalujte hello spuštění úlohy hello rozhraní .NET Framework, pokud je ještě není nainstalován.</span><span class="sxs-lookup"><span data-stu-id="0fe89-162">When you deploy your cloud service, hello startup tasks install hello .NET Framework if it's not already installed.</span></span> <span data-ttu-id="0fe89-163">Role cloudové služby jsou v hello *zaneprázdněn* stavu hello framework je během instalace.</span><span class="sxs-lookup"><span data-stu-id="0fe89-163">Your cloud service roles are in hello *busy* state while hello framework is being installed.</span></span> <span data-ttu-id="0fe89-164">Pokud hello framework instalace vyžaduje restartování, může také restartovat hello služby rolí.</span><span class="sxs-lookup"><span data-stu-id="0fe89-164">If hello framework installation requires a restart, hello service roles might also restart.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0fe89-165">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0fe89-165">Additional resources</span></span>
* <span data-ttu-id="0fe89-166">[Instalace hello rozhraní .NET Framework][Installing hello .NET Framework]</span><span class="sxs-lookup"><span data-stu-id="0fe89-166">[Installing hello .NET Framework][Installing hello .NET Framework]</span></span>
* <span data-ttu-id="0fe89-167">[Zjištění nainstalovaných verzí rozhraní .NET Framework][How to: Determine Which .NET Framework Versions Are Installed]</span><span class="sxs-lookup"><span data-stu-id="0fe89-167">[Determine which .NET Framework versions are installed][How to: Determine Which .NET Framework Versions Are Installed]</span></span>
* <span data-ttu-id="0fe89-168">[Řešení potíží s instalací rozhraní .NET Framework][Troubleshooting .NET Framework Installations]</span><span class="sxs-lookup"><span data-stu-id="0fe89-168">[Troubleshooting .NET Framework installations][Troubleshooting .NET Framework Installations]</span></span>

[How to: Determine Which .NET Framework Versions Are Installed]: https://msdn.microsoft.com/library/hh925568.aspx
[Installing hello .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Troubleshooting .NET Framework Installations]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
