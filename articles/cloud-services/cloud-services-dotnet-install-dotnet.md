---
title: "Instalace rozhraní .NET pro role Azure Cloud Services | Microsoft Docs"
description: "Tento článek popisuje, jak ručně nainstalovat rozhraní .NET Framework na vaše cloudové služby webových a pracovních rolí"
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
ms.openlocfilehash: a9cffa275ae6b9315b821d3160b17a997a1523f7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="install-net-on-azure-cloud-services-roles"></a><span data-ttu-id="9731d-103">Instalace rozhraní .NET pro role Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="9731d-103">Install .NET on Azure Cloud Services roles</span></span>
<span data-ttu-id="9731d-104">Tento článek popisuje postup instalace verze rozhraní .NET Framework, které nejsou součástí hostovaného operačního systému Azure.</span><span class="sxs-lookup"><span data-stu-id="9731d-104">This article describes how to install versions of .NET Framework that don't come with the Azure Guest OS.</span></span> <span data-ttu-id="9731d-105">.NET na hostovaného operačního systému vám pomůže nakonfigurovat role cloudové služby webové a pracovní.</span><span class="sxs-lookup"><span data-stu-id="9731d-105">You can use .NET on the Guest OS to configure your cloud service web and worker roles.</span></span>

<span data-ttu-id="9731d-106">Například můžete nainstalovat rozhraní .NET 4.6.1 v hostovaného operačního systému řady 4, který není součástí žádné verzi rozhraní .NET 4.6.</span><span class="sxs-lookup"><span data-stu-id="9731d-106">For example, you can install .NET 4.6.1 on the Guest OS family 4, which doesn't come with any release of .NET 4.6.</span></span> <span data-ttu-id="9731d-107">(Hostovaného operačního systému rodiny 5 se dodávají s .NET 4.6.) Nejnovější informace o verzích hostovaného operačního systému Azure, najdete v článku [Azure hostovaného operačního systému verze zprávy](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="9731d-107">(The Guest OS family 5 does come with .NET 4.6.) For the latest information on the Azure Guest OS releases, see the [Azure Guest OS release news](cloud-services-guestos-update-matrix.md).</span></span> 

>[!IMPORTANT]
><span data-ttu-id="9731d-108">Azure SDK 2.9 obsahuje omezení nasazení .NET 4.6 na hostovaného operačního systému rodiny 4 nebo dřívější.</span><span class="sxs-lookup"><span data-stu-id="9731d-108">The Azure SDK 2.9 contains a restriction on deploying .NET 4.6 on the Guest OS family 4 or earlier.</span></span> <span data-ttu-id="9731d-109">Oprava pro omezení je k dispozici na [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) lokality.</span><span class="sxs-lookup"><span data-stu-id="9731d-109">A fix for the restriction is available on the [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) site.</span></span>

<span data-ttu-id="9731d-110">Na webových a pracovních rolí nainstalovat rozhraní .NET, patří instalační program webové rozhraní .NET v rámci projekt cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9731d-110">To install .NET on your web and worker roles, include the .NET web installer as part of your cloud service project.</span></span> <span data-ttu-id="9731d-111">Spusťte instalační program jako součást role spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="9731d-111">Start the installer as part of the role's startup tasks.</span></span> 

## <a name="add-the-net-installer-to-your-project"></a><span data-ttu-id="9731d-112">Do projektu přidejte instalační program rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9731d-112">Add the .NET installer to your project</span></span>
<span data-ttu-id="9731d-113">Chcete-li stáhnout instalační program webové rozhraní .NET Framework, vyberte verzi, která chcete nainstalovat:</span><span class="sxs-lookup"><span data-stu-id="9731d-113">To download the web installer for the .NET Framework, choose the version that you want to install:</span></span>

* [<span data-ttu-id="9731d-114">Instalační program webové 4.7 rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9731d-114">.NET 4.7 web installer</span></span>](http://go.microsoft.com/fwlink/?LinkId=825298)
* [<span data-ttu-id="9731d-115">Instalační program webové rozhraní .NET 4.6.1</span><span class="sxs-lookup"><span data-stu-id="9731d-115">.NET 4.6.1 web installer</span></span>](http://go.microsoft.com/fwlink/?LinkId=671729)

<span data-ttu-id="9731d-116">Chcete-li přidat instalační program *webové* role:</span><span class="sxs-lookup"><span data-stu-id="9731d-116">To add the installer for a *web* role:</span></span>
  1. <span data-ttu-id="9731d-117">V **Průzkumníku řešení**v části **role** v projekt cloudové služby, klikněte pravým tlačítkem na vaše *webové* roli a vyberte **přidat**  >  **Novou složku**.</span><span class="sxs-lookup"><span data-stu-id="9731d-117">In **Solution Explorer**, under **Roles** in your cloud service project, right-click your *web* role and select **Add** > **New Folder**.</span></span> <span data-ttu-id="9731d-118">Vytvořte složku s názvem **bin**.</span><span class="sxs-lookup"><span data-stu-id="9731d-118">Create a folder named **bin**.</span></span>
  2. <span data-ttu-id="9731d-119">Klikněte pravým tlačítkem na složku Koš a vyberte **přidat** > **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="9731d-119">Right-click the bin folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="9731d-120">Vyberte instalační program rozhraní .NET a přidejte ji do složky bin.</span><span class="sxs-lookup"><span data-stu-id="9731d-120">Select the .NET installer and add it to the bin folder.</span></span>
  
<span data-ttu-id="9731d-121">Chcete-li přidat instalační program *pracovní* role:</span><span class="sxs-lookup"><span data-stu-id="9731d-121">To add the installer for a *worker* role:</span></span>
* <span data-ttu-id="9731d-122">Klikněte pravým tlačítkem na vaše *pracovní* roli a vyberte **přidat** > **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="9731d-122">Right-click your *worker* role and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="9731d-123">Vyberte instalační program rozhraní .NET a přidejte ji do role.</span><span class="sxs-lookup"><span data-stu-id="9731d-123">Select the .NET installer and add it to the role.</span></span> 

<span data-ttu-id="9731d-124">Když soubory jsou přidány do složky obsahu role tímto způsobem, se automaticky přidá do vašeho balíčku cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9731d-124">When files are added in this way to the role content folder, they're automatically added to your cloud service package.</span></span> <span data-ttu-id="9731d-125">Soubory se pak nasadí do konzistentní umístění na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="9731d-125">The files are then deployed to a consistent location on the virtual machine.</span></span> <span data-ttu-id="9731d-126">Tento postup opakujte pro každý web a pracovní role v rámci cloudové služby tak, aby všechny role kopii Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="9731d-126">Repeat this process for each web and worker role in your cloud service so that all roles have a copy of the installer.</span></span>

> [!NOTE]
> <span data-ttu-id="9731d-127">Musíte nainstalovat rozhraní .NET 4.6.1 na vaše cloudové služby role i v případě, že vaše aplikace zaměřuje .NET 4.6.</span><span class="sxs-lookup"><span data-stu-id="9731d-127">You should install .NET 4.6.1 on your cloud service role even if your application targets .NET 4.6.</span></span> <span data-ttu-id="9731d-128">Hostovaného operačního systému zahrnuje znalostní báze [aktualizace 3098779](https://support.microsoft.com/kb/3098779) a [aktualizace 3097997](https://support.microsoft.com/kb/3097997).</span><span class="sxs-lookup"><span data-stu-id="9731d-128">The Guest OS includes the Knowledge Base [update 3098779](https://support.microsoft.com/kb/3098779) and [update 3097997](https://support.microsoft.com/kb/3097997).</span></span> <span data-ttu-id="9731d-129">Problémy se můžou objevit při spuštění aplikace rozhraní .NET, pokud je nainstalován .NET 4.6 nad aktualizace znalostní báze Knowledge Base.</span><span class="sxs-lookup"><span data-stu-id="9731d-129">Issues can occur when you run your .NET applications if .NET 4.6 is installed on top of the Knowledge Base updates.</span></span> <span data-ttu-id="9731d-130">Abyste se těmto problémům, nainstalujte rozhraní .NET 4.6.1 spíše než verze 4.6.</span><span class="sxs-lookup"><span data-stu-id="9731d-130">To avoid these issues, install .NET 4.6.1 rather than version 4.6.</span></span> <span data-ttu-id="9731d-131">Další informace najdete v tématu [článku znalostní báze 3118750](https://support.microsoft.com/kb/3118750).</span><span class="sxs-lookup"><span data-stu-id="9731d-131">For more information, see the [Knowledge Base article 3118750](https://support.microsoft.com/kb/3118750).</span></span>
> 
> 

![Obsah role s soubory Instalační služby systému][1]

## <a name="define-startup-tasks-for-your-roles"></a><span data-ttu-id="9731d-133">Definování spuštění úloh pro své role</span><span class="sxs-lookup"><span data-stu-id="9731d-133">Define startup tasks for your roles</span></span>
<span data-ttu-id="9731d-134">Spuštění úlohy můžete použít k provádění operací před zahájením roli.</span><span class="sxs-lookup"><span data-stu-id="9731d-134">You can use startup tasks to perform operations before a role starts.</span></span> <span data-ttu-id="9731d-135">Instalace rozhraní .NET Framework jako součást spuštění úloh zajistí, že rozhraní je nainstalovat před spuštěním jakékoli kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="9731d-135">Installing the .NET Framework as part of the startup task ensures that the framework is installed before any application code is run.</span></span> <span data-ttu-id="9731d-136">Další informace o spuštění úlohy najdete v tématu [spouštět úlohy spuštění v Azure](cloud-services-startup-tasks.md).</span><span class="sxs-lookup"><span data-stu-id="9731d-136">For more information on startup tasks, see [Run startup tasks in Azure](cloud-services-startup-tasks.md).</span></span> 

1. <span data-ttu-id="9731d-137">Přidejte následující obsah do souboru ServiceDefinition.csdef pod **WebRole** nebo **WorkerRole** uzlu u všech rolí:</span><span class="sxs-lookup"><span data-stu-id="9731d-137">Add the following content to the ServiceDefinition.csdef file under the **WebRole** or **WorkerRole** node for all roles:</span></span>
   
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
   
    <span data-ttu-id="9731d-138">Předchozí konfiguraci spouští příkaz konzoly `install.cmd` s oprávněním správce pro instalaci rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9731d-138">The preceding configuration runs the console command `install.cmd` with administrator privileges to install the .NET Framework.</span></span> <span data-ttu-id="9731d-139">Vytvoří také konfigurace **LocalStorage** element s názvem **NETFXInstall**.</span><span class="sxs-lookup"><span data-stu-id="9731d-139">The configuration also creates a **LocalStorage** element named **NETFXInstall**.</span></span> <span data-ttu-id="9731d-140">Spuštění skriptu Nastaví dočasnou složku používat tento prostředek Místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="9731d-140">The startup script sets the temp folder to use this local storage resource.</span></span> 
    
    > [!IMPORTANT]
    > <span data-ttu-id="9731d-141">Aby se zajistilo správné instalaci rozhraní, nastavte velikost tohoto prostředku na alespoň 1 024 MB.</span><span class="sxs-lookup"><span data-stu-id="9731d-141">To ensure correct installation of the framework, set the size of this resource to at least 1,024 MB.</span></span>
    
    <span data-ttu-id="9731d-142">Další informace o spuštění úlohy najdete v tématu [běžné Azure Cloud Services spuštění úlohy](cloud-services-startup-tasks-common.md).</span><span class="sxs-lookup"><span data-stu-id="9731d-142">For more information about startup tasks, see [Common Azure Cloud Services startup tasks](cloud-services-startup-tasks-common.md).</span></span>

2. <span data-ttu-id="9731d-143">Vytvořte soubor s názvem **soubor install.cmd** a přidejte následující nainstalujete skript do souboru.</span><span class="sxs-lookup"><span data-stu-id="9731d-143">Create a file named **install.cmd** and add the following install script to the file.</span></span>

    <span data-ttu-id="9731d-144">Skript ověřuje, zda zadaná verze rozhraní .NET Framework je již nainstalována v počítači pomocí dotazu na registru.</span><span class="sxs-lookup"><span data-stu-id="9731d-144">The script checks whether the specified version of the .NET Framework is already installed on the machine by querying the registry.</span></span> <span data-ttu-id="9731d-145">Pokud není nainstalovaná verze rozhraní .NET, je otevřít web instalační program rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="9731d-145">If the .NET version is not installed, then the .NET web installer is opened.</span></span> <span data-ttu-id="9731d-146">Chcete-li vyřešit jakékoliv problémy, skript protokoluje všechny aktivity do souboru startuptasklog-(aktuální datum a čas) .txt, který je uložen v **InstallLogs** místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="9731d-146">To help troubleshoot any issues, the script logs all activity to the file startuptasklog-(current date and time).txt that is stored in **InstallLogs** local storage.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="9731d-147">K vytvoření souboru soubor install.cmd použijte základní textový editor, například Poznámkový blok systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9731d-147">Use a basic text editor like Windows Notepad to create the install.cmd file.</span></span> <span data-ttu-id="9731d-148">Pokud používáte Visual Studio k vytvoření textového souboru a změňte příponu na .cmd, soubor může obsahovat stále značka pořadí bajtů ve formátu UTF-8.</span><span class="sxs-lookup"><span data-stu-id="9731d-148">If you use Visual Studio to create a text file and change the extension to .cmd, the file might still contain a UTF-8 byte order mark.</span></span> <span data-ttu-id="9731d-149">Tato značka může způsobit chybu při spuštění první řádek skriptu.</span><span class="sxs-lookup"><span data-stu-id="9731d-149">This mark can cause an error when the first line of the script is run.</span></span> <span data-ttu-id="9731d-150">Chcete-li se vyhnout této chybě, zkontrolujte první řádek skriptu REM příkaz, který může být vynecháno zpracování pořadí bajtů.</span><span class="sxs-lookup"><span data-stu-id="9731d-150">To avoid this error, make the first line of the script a REM statement that can be skipped by the byte order processing.</span></span> 
    > 
    >
   
    ```cmd
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    REM ***** To install .NET 4.7 set the variable netfx to "NDP47" *****
    set netfx="NDP47"

    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."

    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit

    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
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
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%

    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%

    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%

    :exit
    EXIT /B 0
    ```
   
   > [!NOTE]
   > <span data-ttu-id="9731d-151">Tento skript ukazuje, jak nainstalovat rozhraní .NET 4.5.2 nebo verze 4.6 pro kontinuitu, i když .NET 4.5.2 již dostupný v Azure hostovaného operačního systému.</span><span class="sxs-lookup"><span data-stu-id="9731d-151">This script shows how to install .NET 4.5.2 or version 4.6 for continuity, even though .NET 4.5.2 is already available on the Azure Guest OS.</span></span> <span data-ttu-id="9731d-152">Měli byste přímo nainstalovat rozhraní .NET 4.6.1 spíše než verze 4.6, jak je popsáno v [článku znalostní báze 3118750](https://support.microsoft.com/kb/3118750).</span><span class="sxs-lookup"><span data-stu-id="9731d-152">You should directly install .NET 4.6.1 rather than version 4.6, as described in the [Knowledge Base article 3118750](https://support.microsoft.com/kb/3118750).</span></span>
   > 
   > 

3. <span data-ttu-id="9731d-153">Přidejte soubor install.cmd ke každé roli pomocí **přidat** > **existující položka** v **Průzkumníku řešení** jak je popsáno výše v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="9731d-153">Add the install.cmd file to each role by using **Add** > **Existing Item** in **Solution Explorer** as described earlier in this topic.</span></span> 

    <span data-ttu-id="9731d-154">Po dokončení tohoto kroku se všechny role měli soubor Instalační služby rozhraní .NET a soubor install.cmd.</span><span class="sxs-lookup"><span data-stu-id="9731d-154">After this step is complete, all roles should have the .NET installer file and the install.cmd file.</span></span>

   ![Obsah role se všechny soubory][2]

## <a name="configure-diagnostics-to-transfer-startup-logs-to-blob-storage"></a><span data-ttu-id="9731d-156">Konfiguraci diagnostiky protokoly spuštění přenést do úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="9731d-156">Configure Diagnostics to transfer startup logs to Blob storage</span></span>
<span data-ttu-id="9731d-157">Pro zjednodušení odstraňování problémů s instalací, můžete nakonfigurovat Azure Diagnostics pro přenos všechny soubory protokolů generovaných spouštěcí skript nebo instalačního programu .NET do úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="9731d-157">To simplify troubleshooting installation issues, you can configure Azure Diagnostics to transfer any log files generated by the startup script or the .NET installer to Azure Blob storage.</span></span> <span data-ttu-id="9731d-158">Když použijete tuto metodu, můžete zobrazit protokoly stahování souborů protokolu z úložiště objektů Blob, namísto nutnosti vzdálenou plochu do role.</span><span class="sxs-lookup"><span data-stu-id="9731d-158">By using this approach, you can view the logs by downloading the log files from Blob storage rather than having to remote desktop into the role.</span></span>

<span data-ttu-id="9731d-159">Ke konfiguraci diagnostiky, otevřete soubor diagnostics.wadcfgx a přidejte do něj následující obsah v části **adresáře** uzlu:</span><span class="sxs-lookup"><span data-stu-id="9731d-159">To configure Diagnostics, open the diagnostics.wadcfgx file and add the following content under the **Directories** node:</span></span> 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

<span data-ttu-id="9731d-160">Tato konfigurace XML nakonfiguruje diagnostiky pro přenos souborů v adresáři protokolu v **NETFXInstall** prostředek pro účet úložiště diagnostiky **netfx instalace** kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="9731d-160">This XML configures Diagnostics to transfer the files in the log directory in the **NETFXInstall** resource to the Diagnostics storage account in the **netfx-install** blob container.</span></span>

## <a name="deploy-your-cloud-service"></a><span data-ttu-id="9731d-161">Nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="9731d-161">Deploy your cloud service</span></span>
<span data-ttu-id="9731d-162">Při nasazení cloudové služby, spuštění úlohy instalaci rozhraní .NET Framework, pokud je ještě není nainstalován.</span><span class="sxs-lookup"><span data-stu-id="9731d-162">When you deploy your cloud service, the startup tasks install the .NET Framework if it's not already installed.</span></span> <span data-ttu-id="9731d-163">Role cloudové služby jsou v *zaneprázdněn* stav rozhraní je během instalace.</span><span class="sxs-lookup"><span data-stu-id="9731d-163">Your cloud service roles are in the *busy* state while the framework is being installed.</span></span> <span data-ttu-id="9731d-164">Pokud framework instalace vyžaduje restartování, může také restartovat službu role.</span><span class="sxs-lookup"><span data-stu-id="9731d-164">If the framework installation requires a restart, the service roles might also restart.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9731d-165">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9731d-165">Additional resources</span></span>
* <span data-ttu-id="9731d-166">[Instalace rozhraní .NET Framework][Installing the .NET Framework]</span><span class="sxs-lookup"><span data-stu-id="9731d-166">[Installing the .NET Framework][Installing the .NET Framework]</span></span>
* <span data-ttu-id="9731d-167">[Zjištění nainstalovaných verzí rozhraní .NET Framework][How to: Determine Which .NET Framework Versions Are Installed]</span><span class="sxs-lookup"><span data-stu-id="9731d-167">[Determine which .NET Framework versions are installed][How to: Determine Which .NET Framework Versions Are Installed]</span></span>
* <span data-ttu-id="9731d-168">[Řešení potíží s instalací rozhraní .NET Framework][Troubleshooting .NET Framework Installations]</span><span class="sxs-lookup"><span data-stu-id="9731d-168">[Troubleshooting .NET Framework installations][Troubleshooting .NET Framework Installations]</span></span>

[How to: Determine Which .NET Framework Versions Are Installed]: https://msdn.microsoft.com/library/hh925568.aspx
[Installing the .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Troubleshooting .NET Framework Installations]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
