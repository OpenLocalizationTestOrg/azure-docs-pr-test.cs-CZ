---
title: "aaaDeploy hello služba Mobility obnovení lokality pomocí Azure Automation DSC. | Microsoft Docs"
description: "Popisuje, jak nasazovat toouse Azure Automation DSC tooautomatically hello Azure Site Recovery Mobility service a Azure agenta pro virtuální počítač VMware a tooAzure replikace fyzického serveru"
services: site-recovery
documentationcenter: 
author: krnese
manager: lorenr
editor: 
ms.assetid: 1f8cd3ac-0522-48eb-a5f0-679ee9192ddb
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: krnese
ms.openlocfilehash: 52cdd13ceb61718a21137180c55db86919af5929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a>Nasazení služby Mobility hello s Azure Automation DSC pro replikaci virtuálního počítače
V Operations Management Suite jsme poskytují komplexní zálohování a řešení pro zotavení po havárii, které můžete použít jako součást plánu pro kontinuitu podnikových.

Spuštění této cesty společně s technologií Hyper-V pomocí repliky technologie Hyper-V. Ale rozšířené toosupport heterogenní instalace vzhledem k tomu, že zákazníci, mají více hypervisorů a platformy v jejich cloudy.

Pokud používáte úlohy VMware nebo fyzických serverů dnes, server pro správu se spustí všechny hello součásti Azure Site Recovery ve vaší prostředí toohandle hello komunikace a data replikace s Azure, případě Azure je cíl.

## <a name="deploy-hello-site-recovery-mobility-service-by-using-automation-dsc"></a>Nasazení služby Site Recovery Mobility hello pomocí Automation DSC
Začněme tím, provádění rychlé rozpis jaké tento server pro správu.

server pro správu Hello spouští několik rolí serveru. Jedna z těchto rolí se *konfigurace*, který koordinuje komunikaci a spravuje procesy data replikace a obnovení.

Kromě toho hello *proces* role funguje jako replikační brána. Tato role přijímá data replikace z chráněných zdrojových počítačů, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odešle ji tooan účtu úložiště Azure. Jeden z hello funkce pro roli hello procesu se taky toopush instalaci počítače tooprotected služby Mobility hello a provádět automatického zjišťování virtuálních počítačů VMware.

Pokud dojde navrácení služeb po obnovení z Azure, hello *hlavní cíl* role, bude zpracovávat data replikace hello jako součást této operace.

Pro počítače chráněné hello spoléháme na hello *služba Mobility*. Tato součást je nasazené tooevery počítače (virtuální počítač VMware nebo fyzických serverů), které chcete tooreplicate tooAzure. Se zaznamenává datové zápisy na počítači hello a předává je na serveru pro správu toohello (proces role).

V případě, že pracujete s kontinuity podnikových procesů, je důležité toounderstand úlohy, infrastruktury a součásti hello zahrnuta. Potom můžete splňovat požadavky hello cíli času obnovení (RTO) a cíl bodu obnovení (RPO). V tomto kontextu hello služba Mobility je klíče tooensuring která jsou chráněná úlohy, jako byste očekávali.

Jak tedy, optimalizované způsobem, zajistíte, že máte spolehlivé chráněné instalace pomoci z některé součásti služby Operations Management Suite?

Tento článek obsahuje příklad použití Azure Automation požadovaného stavu konfigurace (DSC), společně s Site Recovery, tooensure který:

* Služba Mobility Hello a agenta virtuálního počítače Azure jsou nasazené toohello počítače s Windows, které chcete tooprotect.
* Služba Mobility Hello a agenta virtuálního počítače Azure jsou vždy spuštěny Azure je cílem replikace hello.

## <a name="prerequisites"></a>Požadavky
* Instalace vyžaduje hello toostore s úložiště
* Úložiště toostore hello vyžaduje heslo tooregister se serverem pro správu hello

  > [!NOTE]
  > Jedinečné heslo se vygeneruje pro každý server pro správu. Chcete-li toodeploy více serverů pro správu, máte tooensure této hello správné heslo je uloženo v souboru passphrase.txt hello.
  >
  >
* Windows Management Framework (WMF) 5.0 nainstalován v počítačích hello chcete tooenable pro ochranu (požadavek pro Automation DSC)

  > [!NOTE]
  > Pokud chcete počítače toouse DSC pro systém Windows, které mají WMF 4.0 nainstalovat, najdete v tématu hello [použití DSC v odpojených prostředích](## Use DSC in disconnected environments).
  

Služba Mobility Hello dají nainstalovat přes hello příkazového řádku a přijímá několik argumentů. To je důvod, proč potřebujete toohave hello binárních souborů (po jejich extrahování z vašeho nastavení) a ukládat je na místě, kde je můžete obnovit pomocí konfigurace DSC.

## <a name="step-1-extract-binaries"></a>Krok 1: Extrahování binárních souborů
1. tooextract hello soubory, které potřebujete pro tento instalační program, procházet toohello následující directory na vašem serveru pro správu:

    **Recovery\home\svsystems\pushinstallsvc\repository \Microsoft azure lokality**

    V této složce měli byste vidět soubor MSI s názvem:

    **Microsoft ASR_UA_version_Windows_GA_date_Release.exe**

    Použijte následující příkaz tooextract hello instalačního programu hello:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**
2. Vyberte všechny soubory a jejich odeslání tooa komprimované složky (ZIP).

Nyní máte hello binární soubory, je nutné tooautomate hello instalace služby Mobility hello pomocí Automation DSC.

### <a name="passphrase"></a>Přístupové heslo
Dále musíte toodetermine místo tooplace této komprimované složky. Účet úložiště Azure můžete použít jako uvedené později toostore hello heslo, které potřebujete k instalaci hello. Hello agenta bude potom proveďte registraci se serverem pro správu hello jako součást procesu hello.

Hello heslo, které jste získali při nasazení serveru pro správu hello můžete uložit jako passphrase.txt tooa textového souboru.

Umístěte hello komprimované složce a hello heslo v vyhrazené kontejneru v hello účtu úložiště Azure.

![Umístění složky](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Pokud dáváte přednost tookeep tyto soubory ve sdílené složce v síti, můžete tak učinit. Stačí tooensure, má přístup hello DSC prostředek, který budete používat později a můžete získat hello nastavení a heslo.

## <a name="step-2-create-hello-dsc-configuration"></a>Krok 2: Vytvoření konfigurace hello DSC
Instalační program Hello závisí na WMF 5.0. Pro počítač toosuccessfully hello použít konfiguraci hello prostřednictvím Automation DSC, WMF 5.0 musí toobe přítomen.

prostředí Hello používá následující příklad konfigurace DSC hello:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
provede konfigurace Hello hello následující:

* proměnné Hello bude informovat hello konfigurace, kde tooget hello binární soubory pro služby Mobility hello a hello agenta virtuálního počítače Azure, kde tooget hello přístupové heslo, a kde toostore hello výstup.
* Hello konfigurace naimportuje hello xPSDesiredStateConfiguration DSC prostředků, tak, aby můžete použít `xRemoteFile` toodownload hello soubory z úložiště hello.
* Konfigurace Hello vytvoří adresáře, kde chcete toostore hello binární soubory.
* Hello archivu prostředků bude extrahujte hello soubory z komprimované složky hello.
* Hello balíček instalace prostředků bude z hello UNIFIEDAGENT instalaci služby Mobility hello. Instalační program EXE s konkrétní argumenty hello. (hello proměnné, které vytvořit hello argumenty potřebovat změnit toobe tooreflect prostředí.)
* Hello balíček prostředků AzureAgent nainstaluje agenta hello virtuálního počítače Azure, který se doporučuje pro každý virtuální počítač, který běží v Azure. agent virtuálního počítače Azure Hello také je možné tooadd toohello rozšíření virtuálního počítače po převzetí služeb při selhání.
* Hello prostředek služby nebo prostředky zajišťují, že hello související služby Mobility a hello Azure vždy běží služby.

Uložte konfiguraci hello jako **ASRMobilityService**.

> [!NOTE]
> Nezapomeňte tooreplace hello CSIP ve vaší konfiguraci tooreflect hello samotnou správu serveru, tak, aby hello agenta správně připojí a bude používat hello správné heslo.
>
>

## <a name="step-3-upload-tooautomation-dsc"></a>Krok 3: Nahrát na server tooAutomation DSC
Protože hello DSC konfigurace, které jste provedli importuje modul požadovaný prostředek DSC (xPSDesiredStateConfiguration), je třeba tooimport Tenhle modul ve službě Automation před nahráním hello DSC konfigurace.

Přihlášení tooyour účet Automation, procházet příliš**prostředky** > **moduly**a klikněte na tlačítko **procházet galerii**.

Zde můžete vyhledat hello modulu a importujte ho tooyour účtu.

![Import modulu](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Když toto dokončíte, přejděte tooyour počítače, ve které máte nainstalované moduly Azure Resource Manageru hello a pokračovat konfigurace DSC tooimport hello nově vytvořený.

### <a name="import-cmdlets"></a>Rutiny importu
V prostředí PowerShell Přihlaste se tooyour předplatného Azure. Upravte tooreflect hello rutin prostředí a zaznamenat informace o účtu Automation v proměnné:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Odešlete hello konfigurace tooAutomation DSC pomocí hello následující rutiny:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-hello-configuration-in-automation-dsc"></a>Zkompilování konfigurace hello v Automation DSC
Dále musíte toocompile hello konfigurace v Automation DSC, tak, aby bylo možné spustit tooit tooregister uzlů. Můžete toho dosáhnout spuštěním následující rutiny hello:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Může to trvat několik minut, protože v podstatě nasazujete hello konfigurace toohello hostované DSC vyžádání služby.

Po zkompilujete hello konfigurace, můžete načíst informace o úlohách hello pomocí prostředí PowerShell (Get-AzureRmAutomationDscCompilationJob) nebo pomocí hello [portál Azure](https://portal.azure.com/).

![Načtení úlohy](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Teď úspěšně publikována a nahrát váš tooAutomation DSC konfigurace DSC.

## <a name="step-4-onboard-machines-tooautomation-dsc"></a>Krok 4: Zařadit počítače tooAutomation DSC
> [!NOTE]
> Jedním z hello předpoklady pro dokončení tohoto scénáře je, že vaše počítače Windows jsou aktualizovány s hello nejnovější verze WMF. Můžete stáhnout a nainstalovat správnou verzi hello pro vaši platformu z hello [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).
>
>

Nyní vytvoříte metaconfig pro DSC, že použijete tooyour uzlů. toosucceed s tím, potřebujete tooretrieve hello koncový bod adresy URL a hello primární klíč pro vybrané účtu Automation v Azure. Můžete najít tyto hodnoty v části **klíče** na hello **všechna nastavení** okně hello účet Automation.

![Hodnoty klíče](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

V tomto příkladu budete mít fyzický server Windows Server 2012 R2 chcete tooprotect pomocí Site Recovery.

### <a name="check-for-any-pending-file-rename-operations-in-hello-registry"></a>Zkontrolujte všechny čekající operace přejmenování souboru v registru hello
Než začnete tooassociate hello server s koncovým bodem hello Automation DSC, doporučujeme zkontrolovat pro všechny čekající operace přejmenování souboru v registru hello. Může vylučují hello instalace z dokončení kvůli tooa čekat na restartování.

Spusťte následující rutinu tooverify se žádné čeká na restartování na serveru hello hello:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Pokud se zobrazí prázdné, jste OK tooproceed. V opačném případě by to vyřešit tak, že restartování serveru hello během časového období údržby.

Konfigurace hello tooapply na serveru hello, otevřete hello prostředí PowerShell integrovaném skriptovacím prostředí (ISE) a spusťte následující skript hello. Toto je v podstatě místní konfigurace DSC, který bude pokyn hello správce místní konfigurace modulu tooregister s hello služby Automation DSC a načíst konkrétní konfiguraci hello (ASRMobilityService.localhost).

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Tato konfigurace způsobí hello správce místní konfigurace modulu tooregister samotné s Automation DSC. Také se určí, jak pracovat hello modul, jak ho postupovat, pokud je konfigurace odlišily (ApplyAndAutoCorrect) a jak by měla postupovat s konfigurací hello Pokud je vyžadován restart.

Po spuštění tohoto skriptu hello uzlu by měl začínat tooregister Automation DSC.

![Registrace uzlu v průběhu](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Pokud přejdete zpět toohello portálu Azure, najdete v tomto uzlu nově zaregistrovaný hello má nyní zobrazovaly v portálu hello.

![Registrovaný uzlu hello portálu](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Na serveru hello můžete spustit hello následující tooverify rutiny, která hello uzel má správně zaregistrovány prostředí PowerShell:

```powershell
Get-DscLocalConfigurationManager
```

Po konfiguraci hello načtený a použitých toohello serveru, můžete to ověřit spuštěním následující rutiny hello:

```powershell
Get-DscConfigurationStatus
```

výstup Hello ukazuje, že tento server hello má úspěšně vyžádat jeho konfigurace:

![Výstup](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Kromě toho instalace služby Mobility hello má svou vlastní protokolu, který se nachází v *SystemDrive*\ProgramData\ASRSetupLogs.

Je to. Teď úspěšně nasazen a zaregistrován hello služba Mobility na počítači hello chcete tooprotect pomocí Site Recovery. DSC se přesvědčíte, že hello požadované vždy běží služby.

![Úspěšné nasazení](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Po server pro správu hello zjistí hello úspěšné nasazení, můžete nakonfigurovat ochranu a zapnout replikaci na počítači hello pomocí Site Recovery.

## <a name="use-dsc-in-disconnected-environments"></a>Použití DSC v odpojených prostředích
Pokud vaše počítače nejsou připojené toohello Internetu, můžete stále závisí na DSC toodeploy a nakonfigurovat službu Mobility hello na hello úlohy, které chcete tooprotect.

Můžete vytvořit vlastní server DSC za instanci ve vašem prostředí tooessentially zadejte hello stejné funkce, které jste získali z Automation DSC. To znamená, bude hello klientům načítat konfigurace hello (po dokončení registrace) toohello DSC koncový bod. Další možností je však toomanually nabízené hello DSC konfigurace tooyour počítače, místně nebo vzdáleně.

Všimněte si, že v tomto příkladu je přidaný parametr pro název počítače hello. Hello vzdálené soubory jsou umístěné ve vzdálené sdílené složce, která by měla být přístupné hello počítače, které chcete tooprotect. Hello konec skriptu hello představuje hello konfigurace a pak spustí tooapply hello DSC konfigurace toohello cílový počítač.

### <a name="prerequisites"></a>Požadavky
Zkontrolujte, zda že je nainstalován tento modul PowerShell xPSDesiredStateConfiguration hello. Pro počítače s Windows, kde je nainstalován WMF 5.0 můžete nainstalovat modul xPSDesiredStateConfiguration hello spuštěním následující rutiny na cílových počítačích hello hello:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Můžete také stáhnout a uložit hello modulu v případě, že potřebujete toodistribute ho tooWindows počítače, které mají WMF 4.0. Spusťte tuto rutinu na počítači, kde se nachází PowerShellGet (WMF 5.0):

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Pro WMF 4.0, ujistěte se také, že hello [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) nainstalován v počítačích hello.

Hello následující konfigurace mohou poslat tooWindows počítače, které mají WMF 5.0 a WMF 4.0:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Pokud chcete tooinstantiate vlastní DSC vyžádání obsahu server na možnostech hello toomimic vaší podnikové síti, které můžete získat z Automation DSC, najdete v části [nastavení webového serveru vyžádané replikace DSC](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Volitelné: Konfigurace DSC nasazení pomocí šablony Azure Resource Manager
Tento článek se zaměřuje na tom, jak můžete vytvořit vlastní tooautomatically konfigurace DSC nasadit službu Mobility hello a hello agenta virtuálního počítače Azure – a ujistěte se, že jsou spuštěny v hello počítače, které chcete tooprotect. Máme také šablonu Azure Resource Manager, nasadíte tuto DSC konfigurace tooa nový nebo existující účet Azure Automation. Šablona Hello použije vstupní parametry toocreate automatizace prostředky, které budou obsahovat hello proměnné prostředí.

Poté, co nasadíte hello šablony, jednoduše najdete toostep 4 v této příručce tooonboard vašich počítačů.

Šablona Hello provede hello následující:

1. Použít existující účet Automation nebo vytvořit novou
2. Trvat vstupní parametry:
   * ASRRemoteFile – hello umístění pro uložení instalace služby Mobility hello
   * ASRPassphrase – hello umístění pro uložení souboru passphrase.txt hello
   * ASRCSEndpoint – hello IP adresa serveru pro správu
3. Importujte modul PowerShell xPSDesiredStateConfiguration hello
4. Vytvoření a kompilaci konfigurace DSC hello

Všechny hello předchozích kroků proběhne ve správném pořadí hello, aby mohl začít registrace vašich počítačů pro ochranu.

Šablona Hello s pokyny pro nasazení, se nachází na [Githubu](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Nasazení šablony hello pomocí prostředí PowerShell:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Další kroky
Poté, co nasadíte agenty služby Mobility hello, můžete [povolit replikaci](site-recovery-vmware-to-azure.md) hello virtuálních počítačů.
