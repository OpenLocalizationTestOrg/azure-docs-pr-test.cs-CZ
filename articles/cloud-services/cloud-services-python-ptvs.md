---
title: "aaaGet začít s Python a Azure Cloud Services | Microsoft Docs"
description: "Přehled použití nástrojů Python Tools pro Visual Studio toocreate cloudových služeb Azure včetně webových rolí a rolí pracovního procesu."
services: cloud-services
documentationcenter: python
author: thraka
manager: timlt
editor: 
ms.assetid: 5489405d-6fa9-4b11-a161-609103cbdc18
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: f5fd85e754839f146abe912351c59dc4a148c990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a><span data-ttu-id="57b9b-103">Webové role a role pracovních procesů Pythonu při použití nástrojů Python Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="57b9b-103">Python web and worker roles with Python Tools for Visual Studio</span></span>

<span data-ttu-id="57b9b-104">Tento článek obsahuje přehled používání webových rolí a rolí pracovních procesů Python pomocí nástrojů [Python Tools for Visual Studio][Python Tools for Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="57b9b-104">This article provides an overview of using Python web and worker roles using [Python Tools for Visual Studio][Python Tools for Visual Studio].</span></span> <span data-ttu-id="57b9b-105">Zjistěte, jak toocreate toouse Visual Studio a nasazení základní Cloudová služba, která používá Python.</span><span class="sxs-lookup"><span data-stu-id="57b9b-105">Learn how toouse Visual Studio toocreate and deploy a basic Cloud Service that uses Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57b9b-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="57b9b-106">Prerequisites</span></span>
* [<span data-ttu-id="57b9b-107">Visual Studio 2013, 2015 nebo 2017</span><span class="sxs-lookup"><span data-stu-id="57b9b-107">Visual Studio 2013, 2015, or 2017</span></span>](https://www.visualstudio.com/)
* <span data-ttu-id="57b9b-108">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS)</span><span class="sxs-lookup"><span data-stu-id="57b9b-108">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS)</span></span>
* <span data-ttu-id="57b9b-109">[Azure SDK Tools for VS 2013][Azure SDK Tools for VS 2013] nebo</span><span class="sxs-lookup"><span data-stu-id="57b9b-109">[Azure SDK Tools for VS 2013][Azure SDK Tools for VS 2013] or</span></span>  
<span data-ttu-id="57b9b-110">[Azure SDK Tools for VS 2015][Azure SDK Tools for VS 2015] nebo</span><span class="sxs-lookup"><span data-stu-id="57b9b-110">[Azure SDK Tools for VS 2015][Azure SDK Tools for VS 2015] or</span></span>  
<span data-ttu-id="57b9b-111">[Azure SDK Tools for VS 2017][Azure SDK Tools for VS 2017]</span><span class="sxs-lookup"><span data-stu-id="57b9b-111">[Azure SDK Tools for VS 2017][Azure SDK Tools for VS 2017]</span></span>
* <span data-ttu-id="57b9b-112">[Python 2.7 (32bitová verze)][Python 2.7 32-bit] nebo [Python 3.5 (32bitová verze)][Python 3.5 32-bit]</span><span class="sxs-lookup"><span data-stu-id="57b9b-112">[Python 2.7 32-bit][Python 2.7 32-bit] or [Python 3.5 32-bit][Python 3.5 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a><span data-ttu-id="57b9b-113">Co jsou webové role a role pracovních procesů Pythonu?</span><span class="sxs-lookup"><span data-stu-id="57b9b-113">What are Python web and worker roles?</span></span>
<span data-ttu-id="57b9b-114">Azure nabízí tři počítačové modely pro spouštění aplikací: [funkci Web Apps v Azure App Service][execution model-web sites], [Azure Virtual Machines][execution model-vms] a [Azure Cloud Services][execution model-cloud services].</span><span class="sxs-lookup"><span data-stu-id="57b9b-114">Azure provides three compute models for running applications: [Web Apps feature in Azure App Service][execution model-web sites], [Azure Virtual Machines][execution model-vms], and [Azure Cloud Services][execution model-cloud services].</span></span> <span data-ttu-id="57b9b-115">Všechny tři modely podporují Python.</span><span class="sxs-lookup"><span data-stu-id="57b9b-115">All three models support Python.</span></span> <span data-ttu-id="57b9b-116">Služby Cloud Services, které obsahují webové role a role pracovních procesů, zajišťují *PaaS (Platform as a Service)*.</span><span class="sxs-lookup"><span data-stu-id="57b9b-116">Cloud Services, which include web and worker roles, provide *Platform as a Service (PaaS)*.</span></span> <span data-ttu-id="57b9b-117">V rámci cloudové služby webové role poskytuje vyhrazené toohost front-end webového serveru web Internetové informační služby (IIS) aplikace, zatímco role pracovních procesů spuštěním asynchronní, dlouhotrvající nebo trvalé úlohy, které jsou nezávislé na vstupu nebo interakci uživatelů.</span><span class="sxs-lookup"><span data-stu-id="57b9b-117">Within a cloud service, a web role provides a dedicated Internet Information Services (IIS) web server toohost front end web applications, while a worker role can run asynchronous, long-running, or perpetual tasks independent of user interaction or input.</span></span>

<span data-ttu-id="57b9b-118">Další informace najdete v článku [Co je cloudová služba?].</span><span class="sxs-lookup"><span data-stu-id="57b9b-118">For more information, see [What is a Cloud Service?].</span></span>

> [!NOTE]
> <span data-ttu-id="57b9b-119">*Hledáte toobuild jednoduchý Web?*</span><span class="sxs-lookup"><span data-stu-id="57b9b-119">*Looking toobuild a simple website?*</span></span>
> <span data-ttu-id="57b9b-120">Pokud váš scénář zahrnuje jen jednoduchý Web front-end, zvažte použití odlehčené funkce Web Apps hello v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="57b9b-120">If your scenario involves just a simple website front end, consider using hello lightweight Web Apps feature in Azure App Service.</span></span> <span data-ttu-id="57b9b-121">Můžete snadno upgradovat tooa cloudové služby jako váš web rozroste a vaše požadavky se změní.</span><span class="sxs-lookup"><span data-stu-id="57b9b-121">You can easily upgrade tooa Cloud Service as your website grows and your requirements change.</span></span> <span data-ttu-id="57b9b-122">V tématu hello <a href="/develop/python/">středisku pro vývojáře Python</a> pro články, které se týkají vývoj hello funkce Web Apps v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="57b9b-122">See hello <a href="/develop/python/">Python Developer Center</a> for articles that cover development of hello Web Apps feature in Azure App Service.</span></span>
> <br />
> 
> 

## <a name="project-creation"></a><span data-ttu-id="57b9b-123">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="57b9b-123">Project creation</span></span>
<span data-ttu-id="57b9b-124">V sadě Visual Studio, můžete vybrat **Azure Cloud Service** v hello **nový projekt** dialogovém **Python**.</span><span class="sxs-lookup"><span data-stu-id="57b9b-124">In Visual Studio, you can select **Azure Cloud Service** in hello **New Project** dialog box, under **Python**.</span></span>

![Dialogové okno Nový projekt](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

<span data-ttu-id="57b9b-126">V Průvodci hello Azure Cloud Service můžete vytvořit nový web a rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="57b9b-126">In hello Azure Cloud Service wizard, you can create new web and worker roles.</span></span>

![Dialog Cloudová služba Azure](./media/cloud-services-python-ptvs/new-service-wizard.png)

<span data-ttu-id="57b9b-128">součástí šablony role pracovního procesu Hello je často používaný kód tooconnect tooan účtu úložiště Azure nebo Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="57b9b-128">hello worker role template comes with boilerplate code tooconnect tooan Azure storage account or Azure Service Bus.</span></span>

![Řešení cloudových služeb](./media/cloud-services-python-ptvs/worker.png)

<span data-ttu-id="57b9b-130">Kdykoli můžete přidat web nebo worker role tooan stávající cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="57b9b-130">You can add web or worker roles tooan existing cloud service at any time.</span></span>  <span data-ttu-id="57b9b-131">Můžete vybrat existující projekty tooadd ve vašem řešení, nebo vytvořit nové.</span><span class="sxs-lookup"><span data-stu-id="57b9b-131">You can choose tooadd existing projects in your solution, or create new ones.</span></span>

![Příkaz pro přidání role](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

<span data-ttu-id="57b9b-133">Cloudové služby můžou obsahovat role implementované v různých jazycích.</span><span class="sxs-lookup"><span data-stu-id="57b9b-133">Your cloud service can contain roles implemented in different languages.</span></span>  <span data-ttu-id="57b9b-134">Můžete mít například webovou roli Pythonu implementovanou pomocí rolí pracovního procesu Django, Python nebo C#.</span><span class="sxs-lookup"><span data-stu-id="57b9b-134">For example, you can have a Python web role implemented using Django, with Python, or with C# worker roles.</span></span>  <span data-ttu-id="57b9b-135">Mezi svými rolemi můžete snadno komunikovat pomocí front služeb Storage nebo Service Bus.</span><span class="sxs-lookup"><span data-stu-id="57b9b-135">You can easily communicate between your roles using Service Bus queues or storage queues.</span></span>

## <a name="install-python-on-hello-cloud-service"></a><span data-ttu-id="57b9b-136">Instalaci Pythonu na hello cloudové služby</span><span class="sxs-lookup"><span data-stu-id="57b9b-136">Install Python on hello cloud service</span></span>
> [!WARNING]
> <span data-ttu-id="57b9b-137">Hello instalačních skriptů, které jsou nainstalované s Visual Studio (v době hello, poslední aktualizace v tomto článku) nefungují.</span><span class="sxs-lookup"><span data-stu-id="57b9b-137">hello setup scripts that are installed with Visual Studio (at hello time this article was last updated) do not work.</span></span> <span data-ttu-id="57b9b-138">Tato část popisuje alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="57b9b-138">This section describes a workaround.</span></span>
> 
> 

<span data-ttu-id="57b9b-139">Hello hlavní problém s skripty instalace hello je, že python není nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="57b9b-139">hello main problem with hello setup scripts is that they do not install python.</span></span> <span data-ttu-id="57b9b-140">Nejprve definovat dvě [spuštění úlohy](cloud-services-startup-tasks.md) v hello [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) souboru.</span><span class="sxs-lookup"><span data-stu-id="57b9b-140">First, define two [startup tasks](cloud-services-startup-tasks.md) in hello [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) file.</span></span> <span data-ttu-id="57b9b-141">první úlohou Hello (**PrepPython.ps1**) se stáhne a nainstaluje modul Python runtime hello.</span><span class="sxs-lookup"><span data-stu-id="57b9b-141">hello first task (**PrepPython.ps1**) downloads and installs hello Python runtime.</span></span> <span data-ttu-id="57b9b-142">druhé úloze Hello (**PipInstaller.ps1**) spouští pip tooinstall všechny závislosti může mít.</span><span class="sxs-lookup"><span data-stu-id="57b9b-142">hello second task (**PipInstaller.ps1**) runs pip tooinstall any dependencies you may have.</span></span>

<span data-ttu-id="57b9b-143">Hello následující skripty byly napsány cílení Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="57b9b-143">hello following scripts were written targeting Python 3.5.</span></span> <span data-ttu-id="57b9b-144">Pokud chcete, aby toouse hello verze 2.x jazyka python, sada hello **PYTHON2** proměnné souboru příliš**na** hello dvou spuštění úlohy a úkolů runtime hello: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.</span><span class="sxs-lookup"><span data-stu-id="57b9b-144">If you want toouse hello version 2.x of python, set hello **PYTHON2** variable file too**on** for hello two startup tasks and hello runtime task: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.</span></span>

```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>

  </Task>

</Startup>
```

<span data-ttu-id="57b9b-145">Hello **PYTHON2** a **PYPATH** proměnné je nutné přidat úloha spuštění toohello pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="57b9b-145">hello **PYTHON2** and **PYPATH** variables must be added toohello worker startup task.</span></span> <span data-ttu-id="57b9b-146">Hello **PYPATH** proměnná se používá, pokud hello **PYTHON2** proměnná se nastaví příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="57b9b-146">hello **PYPATH** variable is only used if hello **PYTHON2** variable is set too**on**.</span></span>

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a><span data-ttu-id="57b9b-147">Ukázkový soubor ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="57b9b-147">Sample ServiceDefinition.csdef</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



<span data-ttu-id="57b9b-148">Dále vytvořte hello **PrepPython.ps1** a **PipInstaller.ps1** soubory v hello **. / bin** složky role.</span><span class="sxs-lookup"><span data-stu-id="57b9b-148">Next, create hello **PrepPython.ps1** and **PipInstaller.ps1** files in hello **./bin** folder of your role.</span></span>

#### <a name="preppythonps1"></a><span data-ttu-id="57b9b-149">PrepPython.ps1</span><span class="sxs-lookup"><span data-stu-id="57b9b-149">PrepPython.ps1</span></span>
<span data-ttu-id="57b9b-150">Tento skript nainstaluje Python.</span><span class="sxs-lookup"><span data-stu-id="57b9b-150">This script installs python.</span></span> <span data-ttu-id="57b9b-151">Pokud hello **PYTHON2** proměnná prostředí se nastaví příliš**na**, pak je nainstalován jazyk Python 2.7, v opačném případě je nainstalován jazyk Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="57b9b-151">If hello **PYTHON2** environment variable is set too**on**, then Python 2.7 is installed, otherwise Python 3.5 is installed.</span></span>

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }

        Write-Output "Not found, downloading $url too$outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a><span data-ttu-id="57b9b-152">PipInstaller.ps1</span><span class="sxs-lookup"><span data-stu-id="57b9b-152">PipInstaller.ps1</span></span>
<span data-ttu-id="57b9b-153">Tento skript volá až pip a nainstaluje všechny závislosti hello hello **requirements.txt** souboru.</span><span class="sxs-lookup"><span data-stu-id="57b9b-153">This script calls up pip and installs all of hello dependencies in hello **requirements.txt** file.</span></span> <span data-ttu-id="57b9b-154">Pokud hello **PYTHON2** proměnná prostředí se nastaví příliš**na**, pak se používá Python 2.7, v opačném případě se používá Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="57b9b-154">If hello **PYTHON2** environment variable is set too**on**, then Python 2.7 is used, otherwise Python 3.5 is used.</span></span>

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a><span data-ttu-id="57b9b-155">Úprava souboru LaunchWorker.ps1</span><span class="sxs-lookup"><span data-stu-id="57b9b-155">Modify LaunchWorker.ps1</span></span>
> [!NOTE]
> <span data-ttu-id="57b9b-156">V případě hello **role pracovního procesu** projektu **LauncherWorker.ps1** soubor je soubor spuštění požadované tooexecute hello.</span><span class="sxs-lookup"><span data-stu-id="57b9b-156">In hello case of a **worker role** project, **LauncherWorker.ps1** file is required tooexecute hello startup file.</span></span> <span data-ttu-id="57b9b-157">V **webovou roli** projektu, hello spuštění souboru se místo toho definované v okně Vlastnosti projektu hello.</span><span class="sxs-lookup"><span data-stu-id="57b9b-157">In a **web role** project, hello startup file is instead defined in hello project properties.</span></span>
> 
> 

<span data-ttu-id="57b9b-158">Hello **bin\LaunchWorker.ps1** byl původně vytvořen toodo spoustu přípravný pracovní, ale ve skutečnosti nefunguje.</span><span class="sxs-lookup"><span data-stu-id="57b9b-158">hello **bin\LaunchWorker.ps1** was originally created toodo a lot of prep work but it doesn't really work.</span></span> <span data-ttu-id="57b9b-159">Hello obsah v tomto souboru nahraďte hello následující skript.</span><span class="sxs-lookup"><span data-stu-id="57b9b-159">Replace hello contents in that file with hello following script.</span></span>

<span data-ttu-id="57b9b-160">Tento skript volá hello **worker.py** soubor z projektu python.</span><span class="sxs-lookup"><span data-stu-id="57b9b-160">This script calls hello **worker.py** file from your python project.</span></span> <span data-ttu-id="57b9b-161">Pokud hello **PYTHON2** proměnná prostředí se nastaví příliš**na**, pak se používá Python 2.7, v opačném případě se používá Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="57b9b-161">If hello **PYTHON2** environment variable is set too**on**, then Python 2.7 is used, otherwise Python 3.5 is used.</span></span>

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize tooyour local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a><span data-ttu-id="57b9b-162">ps.cmd</span><span class="sxs-lookup"><span data-stu-id="57b9b-162">ps.cmd</span></span>
<span data-ttu-id="57b9b-163">šablony sady Visual Studio Hello měli jste vytvořili **ps.cmd** souboru v hello **. / bin** složky.</span><span class="sxs-lookup"><span data-stu-id="57b9b-163">hello Visual Studio templates should have created a **ps.cmd** file in hello **./bin** folder.</span></span> <span data-ttu-id="57b9b-164">Tento skript prostředí volá out hello prostředí PowerShell obálku skripty výše a zajišťuje protokolování na základě názvu hello obálku prostředí PowerShell hello volat.</span><span class="sxs-lookup"><span data-stu-id="57b9b-164">This shell script calls out hello PowerShell wrapper scripts above and provides logging based on hello name of hello PowerShell wrapper called.</span></span> <span data-ttu-id="57b9b-165">Pokud tento soubor nebyl vytvořen, zde je uveden jeho očekávaný obsah.</span><span class="sxs-lookup"><span data-stu-id="57b9b-165">If this file wasn't created, here is what should be in it.</span></span> 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a><span data-ttu-id="57b9b-166">Spuštění v místním prostředí</span><span class="sxs-lookup"><span data-stu-id="57b9b-166">Run locally</span></span>
<span data-ttu-id="57b9b-167">Pokud projekt cloudové služby nastavíte jako projekt po spuštění hello a stiskněte klávesu F5, spustí hello cloudové služby v hello emulátoru místního prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="57b9b-167">If you set your cloud service project as hello startup project and press F5, hello cloud service runs in hello local Azure emulator.</span></span>

<span data-ttu-id="57b9b-168">Nástroje PTVS sice podporují spouštění v emulátoru hello, ladění (například zarážky) není funkční.</span><span class="sxs-lookup"><span data-stu-id="57b9b-168">Although PTVS supports launching in hello emulator, debugging (for example, breakpoints) does not work.</span></span>

<span data-ttu-id="57b9b-169">toodebug webových a pracovních rolí, můžete nastavit jako spouštěný projekt hello hello projekt role a ladit místo.</span><span class="sxs-lookup"><span data-stu-id="57b9b-169">toodebug your web and worker roles, you can set hello role project as hello startup project and debug that instead.</span></span>  <span data-ttu-id="57b9b-170">Můžete také nastavit více projektů po spuštění.</span><span class="sxs-lookup"><span data-stu-id="57b9b-170">You can also set multiple startup projects.</span></span>  <span data-ttu-id="57b9b-171">Klikněte pravým tlačítkem na hello řešení a potom vyberte **nastavit projekty po spuštění**.</span><span class="sxs-lookup"><span data-stu-id="57b9b-171">Right-click hello solution and then select **Set StartUp Projects**.</span></span>

![Vlastnosti projektu po spuštění pro řešení](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-tooazure"></a><span data-ttu-id="57b9b-173">Publikování tooAzure</span><span class="sxs-lookup"><span data-stu-id="57b9b-173">Publish tooAzure</span></span>
<span data-ttu-id="57b9b-174">Klikněte pravým tlačítkem na projekt cloudové služby v řešení hello hello toopublish a potom vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="57b9b-174">toopublish, right-click hello cloud service project in hello solution and then select **Publish**.</span></span>

![Přihlášení pro publikování v Microsoft Azure](./media/cloud-services-python-ptvs/publish-sign-in.png)

<span data-ttu-id="57b9b-176">Postupujte podle pokynů Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="57b9b-176">Follow hello wizard.</span></span> <span data-ttu-id="57b9b-177">V případě potřeby povolte vzdálenou plochu.</span><span class="sxs-lookup"><span data-stu-id="57b9b-177">If you need to, enable remote desktop.</span></span> <span data-ttu-id="57b9b-178">Vzdálená plocha je užitečné, když potřebujete, aby toodebug něco.</span><span class="sxs-lookup"><span data-stu-id="57b9b-178">Remote desktop is helpful when you need toodebug something.</span></span>

<span data-ttu-id="57b9b-179">Po dokončení nastavení konfigurace klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="57b9b-179">When you are done configuring settings, click **Publish**.</span></span>

<span data-ttu-id="57b9b-180">V okně výstupu hello se zobrazí průběh a pak se zobrazí okno protokoly aktivit Microsoft Azure hello.</span><span class="sxs-lookup"><span data-stu-id="57b9b-180">Some progress appears in hello output window, then you'll see hello Microsoft Azure Activity Log window.</span></span>

![Okno Protokoly aktivit Microsoft Azure](./media/cloud-services-python-ptvs/publish-activity-log.png)

<span data-ttu-id="57b9b-182">Nasazení trvá několik minut toocomplete, pak váš web nebo rolí pracovního procesu spustit v Azure!</span><span class="sxs-lookup"><span data-stu-id="57b9b-182">Deployment takes several minutes toocomplete, then your web and/or worker roles run on Azure!</span></span>

### <a name="investigate-logs"></a><span data-ttu-id="57b9b-183">Prozkoumání protokolů</span><span class="sxs-lookup"><span data-stu-id="57b9b-183">Investigate logs</span></span>
<span data-ttu-id="57b9b-184">Po hello cloudové služby virtuálního počítače po spuštění a nainstaluje Python, můžete si prohlédnout hello protokoly toofind zprávy o selhání.</span><span class="sxs-lookup"><span data-stu-id="57b9b-184">After hello cloud service virtual machine starts up and installs Python, you can look at hello logs toofind any failure messages.</span></span> <span data-ttu-id="57b9b-185">Tyto protokoly jsou umístěné v hello **C:\Resources\Directory\\{role} \LogFiles** složky.</span><span class="sxs-lookup"><span data-stu-id="57b9b-185">These logs are located in hello **C:\Resources\Directory\\{role}\LogFiles** folder.</span></span> <span data-ttu-id="57b9b-186">**PrepPython.err.txt** má alespoň jednu chybu v ní z když hello skript pokusí toodetect, pokud je nainstalován jazyk Python a **PipInstaller.err.txt** může stěžují na zastaralou verzi pip.</span><span class="sxs-lookup"><span data-stu-id="57b9b-186">**PrepPython.err.txt** has at least one error in it from when hello script tries toodetect if Python is installed and **PipInstaller.err.txt** may complain about an outdated version of pip.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57b9b-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57b9b-187">Next steps</span></span>
<span data-ttu-id="57b9b-188">Podrobnější informace o práci s webových a pracovních rolí v nástrojích Python Tools pro sadu Visual Studio najdete v dokumentaci k těmto nástrojům hello:</span><span class="sxs-lookup"><span data-stu-id="57b9b-188">For more detailed information about working with web and worker roles in Python Tools for Visual Studio, see hello PTVS documentation:</span></span>

* <span data-ttu-id="57b9b-189">[Projekty cloudových služeb][Cloud Service Projects]</span><span class="sxs-lookup"><span data-stu-id="57b9b-189">[Cloud Service Projects][Cloud Service Projects]</span></span>

<span data-ttu-id="57b9b-190">Podrobnosti o používání služeb Azure z vaší webové a pracovní role, například pomocí Azure Storage nebo Service Bus, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="57b9b-190">For more details about using Azure services from your web and worker roles, such as using Azure Storage or Service Bus, see hello following articles:</span></span>

* <span data-ttu-id="57b9b-191">[Služba Blob][Blob Service]</span><span class="sxs-lookup"><span data-stu-id="57b9b-191">[Blob Service][Blob Service]</span></span>
* <span data-ttu-id="57b9b-192">[Služba Table][Table Service]</span><span class="sxs-lookup"><span data-stu-id="57b9b-192">[Table Service][Table Service]</span></span>
* <span data-ttu-id="57b9b-193">[Služba front][Queue Service]</span><span class="sxs-lookup"><span data-stu-id="57b9b-193">[Queue Service][Queue Service]</span></span>
* <span data-ttu-id="57b9b-194">[Fronty služby Service Bus][Service Bus Queues]</span><span class="sxs-lookup"><span data-stu-id="57b9b-194">[Service Bus Queues][Service Bus Queues]</span></span>
* <span data-ttu-id="57b9b-195">[Témata služby Service Bus][Service Bus Topics]</span><span class="sxs-lookup"><span data-stu-id="57b9b-195">[Service Bus Topics][Service Bus Topics]</span></span>

<!--Link references-->

[Co je cloudová služba?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]:../virtual-machines/windows/overview.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[Blob Service]:../storage/blobs/storage-python-how-to-use-blob-storage.md
[Queue Service]: ../storage/queues/storage-python-how-to-use-queue-storage.md
[Table Service]:../cosmos-db/table-storage-how-to-use-python.md
[Service Bus Queues]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Service Bus Topics]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Cloud Service Projects]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure SDK Tools for VS 2013]: http://go.microsoft.com/fwlink/?LinkId=746482
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=746481
[Azure SDK Tools for VS 2017]: http://go.microsoft.com/fwlink/?LinkId=746483
[Python 2.7 32-bit]: https://www.python.org/downloads/
[Python 3.5 32-bit]: https://www.python.org/downloads/
