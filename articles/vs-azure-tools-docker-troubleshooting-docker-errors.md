---
title: "aaaTroubleshooting chyby Docker klienta v systému Windows pomocí sady Visual Studio | Microsoft Docs"
description: "Řešení problémů, které zaznamenáte při používání sady Visual Studio toocreate a nasaďte tooDocker webové aplikace v systému Windows pomocí sady Visual Studio."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 7421ae8e044d58fc412d748fb870da4c9b2fdb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a><span data-ttu-id="78f9a-103">Řešení potíží s Visual Studio Docker vývoj</span><span class="sxs-lookup"><span data-stu-id="78f9a-103">Troubleshoot Visual Studio Docker development</span></span>

<span data-ttu-id="78f9a-104">Při práci s nástroji Visual Studio Tools pro Docker Preview, se může vyskytnout některé problémy, kvůli hello povaha hello preview.</span><span class="sxs-lookup"><span data-stu-id="78f9a-104">When you're working with Visual Studio Tools for Docker Preview, you may encounter some problems because of hello nature of hello preview.</span></span>
<span data-ttu-id="78f9a-105">Toto jsou některé běžné problémy a řešení.</span><span class="sxs-lookup"><span data-stu-id="78f9a-105">Following are some common issues and resolutions.</span></span>  

## <a name="visual-studio-2017-rc"></a><span data-ttu-id="78f9a-106">Visual Studio 2017 RC</span><span class="sxs-lookup"><span data-stu-id="78f9a-106">Visual Studio 2017 RC</span></span>

### <a name="linux-containers"></a><span data-ttu-id="78f9a-107">**Kontejnery Linux**</span><span class="sxs-lookup"><span data-stu-id="78f9a-107">**Linux containers**</span></span>

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a><span data-ttu-id="78f9a-108">Sestavení dojít k chybám při ladění aplikace web nebo konzoly .NET Core</span><span class="sxs-lookup"><span data-stu-id="78f9a-108">Build errors occur when debugging a .NET Core web or console application</span></span>  

<span data-ttu-id="78f9a-109">To může být související toonot sdílení s Docker pro Windows hello jednotky, kde se nachází hello projektu.</span><span class="sxs-lookup"><span data-stu-id="78f9a-109">This could be related toonot sharing hello drive where hello project resides with Docker for Windows.</span></span>  <span data-ttu-id="78f9a-110">Může dojít chybě jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="78f9a-110">You may receive an error like hello following:</span></span>

```
hello "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with hello default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
<span data-ttu-id="78f9a-111">tooresolve tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="78f9a-111">tooresolve this issue:</span></span>

1. <span data-ttu-id="78f9a-112">Klikněte pravým tlačítkem na **Docker pro systém Windows** v oznamovací oblasti text hello a potom vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="78f9a-112">Right-click **Docker for Windows** in hello notification area, and then select **Settings**.</span></span>  
2. <span data-ttu-id="78f9a-113">Vyberte **sdílené disky** a sdílet hello jednotku, kde se nachází hello projektu.</span><span class="sxs-lookup"><span data-stu-id="78f9a-113">Select **Shared Drives** and share hello drive where hello project resides.</span></span>

### <a name="windows-containers"></a><span data-ttu-id="78f9a-114">**Kontejnery Windows**</span><span class="sxs-lookup"><span data-stu-id="78f9a-114">**Windows containers**</span></span>

<span data-ttu-id="78f9a-115">Hello následující problémy jsou konkrétní toodebugging rozhraní .NET Framework web a konzole aplikace v kontejnerech systému Windows.</span><span class="sxs-lookup"><span data-stu-id="78f9a-115">hello following issues are specific toodebugging .NET Framework web and console applications in Windows containers.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="78f9a-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="78f9a-116">Prerequisites</span></span>

1. <span data-ttu-id="78f9a-117">Visual Studio 2017 RC (nebo novější) s hello .NET Core a zatížení Docker Preview musí být nainstalován.</span><span class="sxs-lookup"><span data-stu-id="78f9a-117">Visual Studio 2017 RC (or later) with hello .NET Core and Docker Preview workload must be installed.</span></span>
2. <span data-ttu-id="78f9a-118">Windows 10 Anniversary aktualizaci s aby Windows Update nejnovější opravy.</span><span class="sxs-lookup"><span data-stu-id="78f9a-118">Windows 10 Anniversary Update with that latest Windows Update patches.</span></span> <span data-ttu-id="78f9a-119">Konkrétně [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) musí být nainstalován.</span><span class="sxs-lookup"><span data-stu-id="78f9a-119">Specifically [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) must be installed.</span></span> 
3. <span data-ttu-id="78f9a-120">[Docker pro systém Windows](https://docs.docker.com/docker-for-windows/) (sestavení 1.13.0 nebo novější) musí být nainstalován.</span><span class="sxs-lookup"><span data-stu-id="78f9a-120">[Docker for Windows](https://docs.docker.com/docker-for-windows/) (build 1.13.0 or later) must be installed.</span></span>
4. <span data-ttu-id="78f9a-121">**Přepínač tooWindows kontejnery** musí být vybrán.</span><span class="sxs-lookup"><span data-stu-id="78f9a-121">**Switch tooWindows containers** must be selected.</span></span> <span data-ttu-id="78f9a-122">V oznamovací oblasti hello, klikněte na tlačítko **Docker pro systém Windows**a potom vyberte **přepínač kontejnery tooWindows**.</span><span class="sxs-lookup"><span data-stu-id="78f9a-122">In hello notification area, click **Docker for Windows**, and then select **Switch tooWindows containers**.</span></span> <span data-ttu-id="78f9a-123">Po restartování počítače hello, ujistěte se, že toto nastavení je zachován.</span><span class="sxs-lookup"><span data-stu-id="78f9a-123">After hello machine restarts, ensure that this setting is retained.</span></span>

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a><span data-ttu-id="78f9a-124">Výstup konzoly se nezobrazí v okně výstupu sady Visual Studio při ladění aplikace konzoly</span><span class="sxs-lookup"><span data-stu-id="78f9a-124">Console output does not appear in Visual Studio's output window while debugging a console application</span></span>

<span data-ttu-id="78f9a-125">Jedná se o známý problém s hello ladicí program Visual Studio (msvsmon.exe), který není aktuálně určený pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="78f9a-125">This is a known issue with hello Visual Studio debugger (msvsmon.exe), which is currently not designed for this scenario.</span></span> <span data-ttu-id="78f9a-126">Podpora pro tento scénář mohou být zahrnuta v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="78f9a-126">Support for this scenario might be included in a future release.</span></span> <span data-ttu-id="78f9a-127">toosee výstup hello konzolové aplikace v sadě Visual Studio, použijte **Docker: spuštění projektu**, což je totéž příliš**spustit bez ladění**.</span><span class="sxs-lookup"><span data-stu-id="78f9a-127">toosee output from hello console application in Visual Studio, use **Docker: Start Project**, which is equivalent too**Start without Debugging**.</span></span>

#### <a name="debugging-web-applications-with-hello-release-configuration-fails-with-403-forbidden-error"></a><span data-ttu-id="78f9a-128">Ladění webových aplikací s hello verze konfigurace selže s chybou (403) zakázáno</span><span class="sxs-lookup"><span data-stu-id="78f9a-128">Debugging web applications with hello release configuration fails with (403) Forbidden error</span></span>

<span data-ttu-id="78f9a-129">toowork tento problém odstranit, otevřete web.release.config v hello řešení a poznámky nebo odstranit hello následující řádky:</span><span class="sxs-lookup"><span data-stu-id="78f9a-129">toowork around this issue, open web.release.config in hello solution and comment out or delete hello following lines:</span></span>

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a><span data-ttu-id="78f9a-130">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="78f9a-130">Visual Studio 2015</span></span>

### <a name="linux-containers"></a><span data-ttu-id="78f9a-131">**Kontejnery Linux**</span><span class="sxs-lookup"><span data-stu-id="78f9a-131">**Linux containers**</span></span>

#### <a name="unable-toovalidate-volume-mapping"></a><span data-ttu-id="78f9a-132">Mapování nelze toovalidate svazku</span><span class="sxs-lookup"><span data-stu-id="78f9a-132">Unable toovalidate volume mapping</span></span>
<span data-ttu-id="78f9a-133">Mapování svazek je požadovaná tooshare hello zdrojový kód a binární soubory aplikace se složkou aplikace hello v kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="78f9a-133">Volume mapping is required tooshare hello source code and binaries of your application with hello app folder in hello container.</span></span>  <span data-ttu-id="78f9a-134">Mapování konkrétní svazku jsou obsaženy v rámci docker compose.dev.debug.yml a docker compose.dev.release.yml.</span><span class="sxs-lookup"><span data-stu-id="78f9a-134">Specific volume mappings are contained within docker-compose.dev.debug.yml and docker-compose.dev.release.yml.</span></span> <span data-ttu-id="78f9a-135">Jak se změnily soubory na hostitelském počítači, hello kontejnery podle těchto změn v podobné struktura složek.</span><span class="sxs-lookup"><span data-stu-id="78f9a-135">As files are changed on your host machine, hello containers reflect these changes in a similar folder structure.</span></span>

<span data-ttu-id="78f9a-136">mapování tooenable svazku:</span><span class="sxs-lookup"><span data-stu-id="78f9a-136">tooenable volume mapping:</span></span>

1. <span data-ttu-id="78f9a-137">Klikněte na tlačítko **Moby** v oznamovací oblasti hello a vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="78f9a-137">Click **Moby** in hello notification area and select **Settings**.</span></span>
2. <span data-ttu-id="78f9a-138">Vyberte **sdílené disky**.</span><span class="sxs-lookup"><span data-stu-id="78f9a-138">Select **Shared Drives**.</span></span>
3. <span data-ttu-id="78f9a-139">Vyberte jednotku hello, který je hostitelem vašeho projektu a hello disku, kde se nachází % USERPROFILE %.</span><span class="sxs-lookup"><span data-stu-id="78f9a-139">Select hello drive that hosts your project and hello drive where %USERPROFILE% resides.</span></span>
4. <span data-ttu-id="78f9a-140">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="78f9a-140">Click **Apply**.</span></span>

<span data-ttu-id="78f9a-141">tootest Pokud funguje mapování svazek znovu sestavit a po jedné nebo více jednotek byly sdílené, nebo spusťte následující kód z příkazového řádku hello možnost F5 z Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="78f9a-141">tootest if volume mapping is functioning, Rebuild and select F5 from within Visual Studio after one or more drives have been shared, or run hello following code from a command prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="78f9a-142">Tento příklad předpokládá, že vaši uživatelé složka je umístěna na jednotce C a zda je sdílen.</span><span class="sxs-lookup"><span data-stu-id="78f9a-142">This example assumes that your Users folder is located on drive C and that it has been shared.</span></span>
> <span data-ttu-id="78f9a-143">Upraveno v případě potřeby, pokud jste sdíleli jinou jednotku.</span><span class="sxs-lookup"><span data-stu-id="78f9a-143">Revise as necessary if you have shared a different drive.</span></span>

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

<span data-ttu-id="78f9a-144">Spusťte následující kód v kontejneru Linux hello hello.</span><span class="sxs-lookup"><span data-stu-id="78f9a-144">Run hello following code in hello Linux container.</span></span>

```
/ # ls
```

<span data-ttu-id="78f9a-145">Měli byste vidět ze složky uživatele a veřejného hello výpis adresáře.</span><span class="sxs-lookup"><span data-stu-id="78f9a-145">You should see a directory listing from hello Users/Public folder.</span></span> <span data-ttu-id="78f9a-146">Pokud jsou zobrazeny žádné soubory a složky /c/Users/Public není prázdný, mapování svazek není správně nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="78f9a-146">If no files are displayed and your /c/Users/Public folder isn't empty, volume mapping is not configured properly.</span></span>

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

<span data-ttu-id="78f9a-147">Změňte adresář díry toohello toosee hello obsah hello `/c/Users/Public` directory:</span><span class="sxs-lookup"><span data-stu-id="78f9a-147">Change toohello wormhole directory toosee hello contents of hello `/c/Users/Public` directory:</span></span>

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> <span data-ttu-id="78f9a-148">Při práci s virtuálními počítači Linux, systém souborů kontejneru hello je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="78f9a-148">When you're working with Linux VMs, hello container file system is case-sensitive.</span></span>

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a><span data-ttu-id="78f9a-149">Sestavení: Úlohy "PrepareForBuild" se nezdařilo neočekávaně</span><span class="sxs-lookup"><span data-stu-id="78f9a-149">Build: "PrepareForBuild" task failed unexpectedly</span></span>

<span data-ttu-id="78f9a-150">Microsoft.DotNet.Docker.CommandLine.ClientException: Došlo k chybě snažíme tooconnect.</span><span class="sxs-lookup"><span data-stu-id="78f9a-150">Microsoft.DotNet.Docker.CommandLine.ClientException: An error occurred trying tooconnect.</span></span>

<span data-ttu-id="78f9a-151">Ověřte, že tento hello výchozího Docker hostitele běží.</span><span class="sxs-lookup"><span data-stu-id="78f9a-151">Verify that hello default Docker host is running.</span></span> <span data-ttu-id="78f9a-152">Otevřete příkazový řádek a spusťte:</span><span class="sxs-lookup"><span data-stu-id="78f9a-152">Open a command prompt and execute:</span></span>

```
docker info
```

<span data-ttu-id="78f9a-153">Pokud tento příkaz vrátí chybu, poté pokus toostart hello **Docker pro systém Windows** aplikace na ploše.</span><span class="sxs-lookup"><span data-stu-id="78f9a-153">If this returns an error, then attempt toostart hello **Docker for Windows** desktop app.</span></span> <span data-ttu-id="78f9a-154">Pokud je spuštěná aplikace na ploše hello, pak **Moby** by měly jít vidět v oznamovací oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="78f9a-154">If hello desktop app is running, then **Moby** should be visible in hello notification area.</span></span> <span data-ttu-id="78f9a-155">Klikněte pravým tlačítkem na **Moby** a otevřete **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="78f9a-155">Right-click **Moby** and open **Settings**.</span></span> <span data-ttu-id="78f9a-156">Klikněte na tlačítko **resetovat**a pak restartujte Docker.</span><span class="sxs-lookup"><span data-stu-id="78f9a-156">Click **Reset**, and then restart Docker.</span></span>

## <a name="an-error-dialog-occurs-when-attempting-tooadd-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a><span data-ttu-id="78f9a-157">Dialogové okno chyby dojde při pokusu o tooadd podporu Docker nebo ladění aplikace ASP.NET Core v kontejneru (F5)</span><span class="sxs-lookup"><span data-stu-id="78f9a-157">An error dialog occurs when attempting tooadd Docker Support or debug (F5) an ASP.NET Core application in a container</span></span>

<span data-ttu-id="78f9a-158">Po odinstalaci a instalace rozšíření, může být poškozen hello mezipaměti Managed Extensibility Framework (MEF) v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78f9a-158">After uninstalling and installing extensions, hello Managed Extensibility Framework (MEF) cache in Visual Studio can become corrupted.</span></span> <span data-ttu-id="78f9a-159">V takovém případě může způsobit různé chybové zprávy, když se přidání podpory Docker nebo pokusu o toorun nebo ladění aplikace ASP.NET Core (F5).</span><span class="sxs-lookup"><span data-stu-id="78f9a-159">When this occurs, it can cause various error messages when you're adding Docker Support and/or attempting toorun or debug (F5) your ASP.NET Core application.</span></span> <span data-ttu-id="78f9a-160">Jako dočasné řešení použijte následující kroky toodelete a znovu vygenerovat hello MEF mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78f9a-160">As a temporary workaround, use hello following steps toodelete and regenerate hello MEF cache.</span></span>

1. <span data-ttu-id="78f9a-161">Zavřete všechny instance sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78f9a-161">Close all instances of Visual Studio.</span></span>
1. <span data-ttu-id="78f9a-162">Otevřete % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.</span><span class="sxs-lookup"><span data-stu-id="78f9a-162">Open %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.</span></span>
1. <span data-ttu-id="78f9a-163">Odstraňte hello následující složky:</span><span class="sxs-lookup"><span data-stu-id="78f9a-163">Delete hello following folders:</span></span>
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. <span data-ttu-id="78f9a-164">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78f9a-164">Open Visual Studio.</span></span>
1. <span data-ttu-id="78f9a-165">Scénář hello pokus opakujte.</span><span class="sxs-lookup"><span data-stu-id="78f9a-165">Attempt hello scenario again.</span></span>
