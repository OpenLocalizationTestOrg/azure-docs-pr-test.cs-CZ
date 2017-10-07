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
# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Webové role a role pracovních procesů Pythonu při použití nástrojů Python Tools for Visual Studio

Tento článek obsahuje přehled používání webových rolí a rolí pracovních procesů Python pomocí nástrojů [Python Tools for Visual Studio][Python Tools for Visual Studio]. Zjistěte, jak toocreate toouse Visual Studio a nasazení základní Cloudová služba, která používá Python.

## <a name="prerequisites"></a>Požadavky
* [Visual Studio 2013, 2015 nebo 2017](https://www.visualstudio.com/)
* [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS)
* [Azure SDK Tools for VS 2013][Azure SDK Tools for VS 2013] nebo  
[Azure SDK Tools for VS 2015][Azure SDK Tools for VS 2015] nebo  
[Azure SDK Tools for VS 2017][Azure SDK Tools for VS 2017]
* [Python 2.7 (32bitová verze)][Python 2.7 32-bit] nebo [Python 3.5 (32bitová verze)][Python 3.5 32-bit]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Co jsou webové role a role pracovních procesů Pythonu?
Azure nabízí tři počítačové modely pro spouštění aplikací: [funkci Web Apps v Azure App Service][execution model-web sites], [Azure Virtual Machines][execution model-vms] a [Azure Cloud Services][execution model-cloud services]. Všechny tři modely podporují Python. Služby Cloud Services, které obsahují webové role a role pracovních procesů, zajišťují *PaaS (Platform as a Service)*. V rámci cloudové služby webové role poskytuje vyhrazené toohost front-end webového serveru web Internetové informační služby (IIS) aplikace, zatímco role pracovních procesů spuštěním asynchronní, dlouhotrvající nebo trvalé úlohy, které jsou nezávislé na vstupu nebo interakci uživatelů.

Další informace najdete v článku [Co je cloudová služba?].

> [!NOTE]
> *Hledáte toobuild jednoduchý Web?*
> Pokud váš scénář zahrnuje jen jednoduchý Web front-end, zvažte použití odlehčené funkce Web Apps hello v Azure App Service. Můžete snadno upgradovat tooa cloudové služby jako váš web rozroste a vaše požadavky se změní. V tématu hello <a href="/develop/python/">středisku pro vývojáře Python</a> pro články, které se týkají vývoj hello funkce Web Apps v Azure App Service.
> <br />
> 
> 

## <a name="project-creation"></a>Vytvoření projektu
V sadě Visual Studio, můžete vybrat **Azure Cloud Service** v hello **nový projekt** dialogovém **Python**.

![Dialogové okno Nový projekt](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

V Průvodci hello Azure Cloud Service můžete vytvořit nový web a rolí pracovního procesu.

![Dialog Cloudová služba Azure](./media/cloud-services-python-ptvs/new-service-wizard.png)

součástí šablony role pracovního procesu Hello je často používaný kód tooconnect tooan účtu úložiště Azure nebo Azure Service Bus.

![Řešení cloudových služeb](./media/cloud-services-python-ptvs/worker.png)

Kdykoli můžete přidat web nebo worker role tooan stávající cloudovou službu.  Můžete vybrat existující projekty tooadd ve vašem řešení, nebo vytvořit nové.

![Příkaz pro přidání role](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Cloudové služby můžou obsahovat role implementované v různých jazycích.  Můžete mít například webovou roli Pythonu implementovanou pomocí rolí pracovního procesu Django, Python nebo C#.  Mezi svými rolemi můžete snadno komunikovat pomocí front služeb Storage nebo Service Bus.

## <a name="install-python-on-hello-cloud-service"></a>Instalaci Pythonu na hello cloudové služby
> [!WARNING]
> Hello instalačních skriptů, které jsou nainstalované s Visual Studio (v době hello, poslední aktualizace v tomto článku) nefungují. Tato část popisuje alternativní řešení.
> 
> 

Hello hlavní problém s skripty instalace hello je, že python není nainstalovaný. Nejprve definovat dvě [spuštění úlohy](cloud-services-startup-tasks.md) v hello [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) souboru. první úlohou Hello (**PrepPython.ps1**) se stáhne a nainstaluje modul Python runtime hello. druhé úloze Hello (**PipInstaller.ps1**) spouští pip tooinstall všechny závislosti může mít.

Hello následující skripty byly napsány cílení Python 3.5. Pokud chcete, aby toouse hello verze 2.x jazyka python, sada hello **PYTHON2** proměnné souboru příliš**na** hello dvou spuštění úlohy a úkolů runtime hello: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.

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

Hello **PYTHON2** a **PYPATH** proměnné je nutné přidat úloha spuštění toohello pracovního procesu. Hello **PYPATH** proměnná se používá, pokud hello **PYTHON2** proměnná se nastaví příliš**na**.

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

#### <a name="sample-servicedefinitioncsdef"></a>Ukázkový soubor ServiceDefinition.csdef
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



Dále vytvořte hello **PrepPython.ps1** a **PipInstaller.ps1** soubory v hello **. / bin** složky role.

#### <a name="preppythonps1"></a>PrepPython.ps1
Tento skript nainstaluje Python. Pokud hello **PYTHON2** proměnná prostředí se nastaví příliš**na**, pak je nainstalován jazyk Python 2.7, v opačném případě je nainstalován jazyk Python 3.5.

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

#### <a name="pipinstallerps1"></a>PipInstaller.ps1
Tento skript volá až pip a nainstaluje všechny závislosti hello hello **requirements.txt** souboru. Pokud hello **PYTHON2** proměnná prostředí se nastaví příliš**na**, pak se používá Python 2.7, v opačném případě se používá Python 3.5.

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

#### <a name="modify-launchworkerps1"></a>Úprava souboru LaunchWorker.ps1
> [!NOTE]
> V případě hello **role pracovního procesu** projektu **LauncherWorker.ps1** soubor je soubor spuštění požadované tooexecute hello. V **webovou roli** projektu, hello spuštění souboru se místo toho definované v okně Vlastnosti projektu hello.
> 
> 

Hello **bin\LaunchWorker.ps1** byl původně vytvořen toodo spoustu přípravný pracovní, ale ve skutečnosti nefunguje. Hello obsah v tomto souboru nahraďte hello následující skript.

Tento skript volá hello **worker.py** soubor z projektu python. Pokud hello **PYTHON2** proměnná prostředí se nastaví příliš**na**, pak se používá Python 2.7, v opačném případě se používá Python 3.5.

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

#### <a name="pscmd"></a>ps.cmd
šablony sady Visual Studio Hello měli jste vytvořili **ps.cmd** souboru v hello **. / bin** složky. Tento skript prostředí volá out hello prostředí PowerShell obálku skripty výše a zajišťuje protokolování na základě názvu hello obálku prostředí PowerShell hello volat. Pokud tento soubor nebyl vytvořen, zde je uveden jeho očekávaný obsah. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Spuštění v místním prostředí
Pokud projekt cloudové služby nastavíte jako projekt po spuštění hello a stiskněte klávesu F5, spustí hello cloudové služby v hello emulátoru místního prostředí Azure.

Nástroje PTVS sice podporují spouštění v emulátoru hello, ladění (například zarážky) není funkční.

toodebug webových a pracovních rolí, můžete nastavit jako spouštěný projekt hello hello projekt role a ladit místo.  Můžete také nastavit více projektů po spuštění.  Klikněte pravým tlačítkem na hello řešení a potom vyberte **nastavit projekty po spuštění**.

![Vlastnosti projektu po spuštění pro řešení](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-tooazure"></a>Publikování tooAzure
Klikněte pravým tlačítkem na projekt cloudové služby v řešení hello hello toopublish a potom vyberte **publikovat**.

![Přihlášení pro publikování v Microsoft Azure](./media/cloud-services-python-ptvs/publish-sign-in.png)

Postupujte podle pokynů Průvodce hello. V případě potřeby povolte vzdálenou plochu. Vzdálená plocha je užitečné, když potřebujete, aby toodebug něco.

Po dokončení nastavení konfigurace klikněte na **Publikovat**.

V okně výstupu hello se zobrazí průběh a pak se zobrazí okno protokoly aktivit Microsoft Azure hello.

![Okno Protokoly aktivit Microsoft Azure](./media/cloud-services-python-ptvs/publish-activity-log.png)

Nasazení trvá několik minut toocomplete, pak váš web nebo rolí pracovního procesu spustit v Azure!

### <a name="investigate-logs"></a>Prozkoumání protokolů
Po hello cloudové služby virtuálního počítače po spuštění a nainstaluje Python, můžete si prohlédnout hello protokoly toofind zprávy o selhání. Tyto protokoly jsou umístěné v hello **C:\Resources\Directory\\{role} \LogFiles** složky. **PrepPython.err.txt** má alespoň jednu chybu v ní z když hello skript pokusí toodetect, pokud je nainstalován jazyk Python a **PipInstaller.err.txt** může stěžují na zastaralou verzi pip.

## <a name="next-steps"></a>Další kroky
Podrobnější informace o práci s webových a pracovních rolí v nástrojích Python Tools pro sadu Visual Studio najdete v dokumentaci k těmto nástrojům hello:

* [Projekty cloudových služeb][Cloud Service Projects]

Podrobnosti o používání služeb Azure z vaší webové a pracovní role, například pomocí Azure Storage nebo Service Bus, najdete v části hello následující články:

* [Služba Blob][Blob Service]
* [Služba Table][Table Service]
* [Služba front][Queue Service]
* [Fronty služby Service Bus][Service Bus Queues]
* [Témata služby Service Bus][Service Bus Topics]

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
