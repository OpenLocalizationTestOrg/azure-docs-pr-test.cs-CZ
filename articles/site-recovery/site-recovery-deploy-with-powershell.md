---
title: "aaaReplicate tooAzure virtuálních počítačů Hyper-V na portálu classic hello pomocí prostředí PowerShell | Microsoft Docs"
description: "Automatizovat hello replikaci virtuálních počítačů technologie Hyper-V v cloudech VMM pomocí Site Recovery a prostředí PowerShell na portálu classic hello"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: tysonn
ms.assetid: 9011f567-e0b4-4306-951a-b30da19f5db6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: d6847b46ac227209e6890de4ab603b23f827360f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-vms-tooazure-with-powershell-in-hello-classic-portal"></a>Replikovat virtuální počítače Hyper-V tooAzure pomocí prostředí PowerShell na portálu classic hello
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Portál Classic](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Classic](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a>Přehled
Azure Site Recovery přispívá tooyour obchodní kontinuitu a po havárii (BCDR) strategii zotavení tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů v různých scénářích nasazení. Úplný seznam nasazení scénářů najdete v části hello [přehled Azure Site Recovery](site-recovery-overview.md).

Tento článek ukazuje, jak toouse prostředí PowerShell tooautomate běžné úlohy je nutné tooperform při nastavení Azure Site Recovery tooreplicate technologie Hyper-V virtuálních počítačů v System Center VMM cloudy tooAzure úložiště.

Hello článek obsahuje požadavky pro hello scénář a ukazuje, jak instalovat tooset nahoru k trezoru Site Recovery zprostředkovatele Azure Site Recovery hello na zdrojovém serveru VMM hello, zaregistrujte hello server v hello trezoru, přidat účet úložiště Azure, nainstalujte hello Azure Agent služeb zotavení na hostitelských serverech technologie Hyper-V, nakonfigurovat nastavení ochrany pro cloudy VMM, které budou použité tooall chráněné virtuální počítače a pak povolte ochranu pro tyto virtuální počítače. Nakonec testování převzetí služeb při selhání hello toomake se, že vše funguje podle očekávání.

Pokud narazíte na problémy nastavení tento scénář, zveřejněte svoje otázky na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello.
>
>

## <a name="before-you-start"></a>Než začnete
Ujistěte se, že máte zavedenou tyto požadavky:

### <a name="azure-prerequisites"></a>Požadavky Azure
* Budete potřebovat účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
* Budete potřebovat data toostore replikovat účtu úložiště Azure. Hello účet potřebuje povolenou geografickou replikací. By mělo být ve stejné oblasti jako trezor Azure Site Recovery hello hello a přidružen hello stejného předplatného. [Další informace o Azure storage](../storage/common/storage-introduction.md).
* Musíte mít jistotu, že virtuální počítače, které chcete tooprotect v souladu s toomake [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

### <a name="vmm-prerequisites"></a>Požadavky VMM
* Budete potřebovat VMM server běžící na System Center 2012 R2.
* Budete potřebovat alespoň jeden cloud na serveru VMM má tooprotect hello. Hello cloud by měl obsahovat:
  * Minimálně jednu skupinu hostitelů VMM.
  * Jeden nebo více hostitelských serverů Hyper-V nebo clusterů v každé skupině hostitelů.
  * Jeden nebo více virtuálních počítačů na serveru hello zdroj technologie Hyper-V.

### <a name="hyper-v-prerequisites"></a>Požadavky technologie Hyper-V
* Hello servery hostitele technologie Hyper-V musí běžet minimálně **systému Windows Server 2012** s rolí Hyper-V nebo **Microsoft Hyper-V Server 2012** a mít hello nainstalované nejnovější aktualizace.
* Pokud používáte technologii Hyper-V v clusteru, je důležité vědět, že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte clustery založené na statických IP adresách. Zprostředkovatel clusteru hello tooconfigure budete potřebovat ručně. toodo to, ve Správci serveru > Správce clusteru převzetí služeb při selhání, připojte toohello cluster, klikněte na tlačítko **konfigurovat roli** a vyberte **zprostředkovatele replik technologie Hyper-V** v hello **vybrat roli**obrazovka Průvodce vysokou dostupností hello.
* Libovolný server hostitele technologie Hyper-V nebo clusteru, pro které chcete toomanage ochrany musí být součástí cloudu VMM.

### <a name="network-mapping-prerequisites"></a>Požadavky na mapování sítě
Při ochraně virtuálních počítačů v síti Azure mapování mapuje sítě virtuálních počítačů na zdrojovém serveru VMM hello a cílové sítě Azure tooenable hello následující:

* Všechny počítače, které převzetí služeb při selhání na hello stejné tooeach jiných, bez ohledu na plánu obnovení se mohou připojit.
* Pokud bránu sítě je nastavena na hello cílovou síť Azure, můžete virtuální počítače připojit tooother místní virtuální počítače.
* Pokud nenakonfigurujete sítě mapování jenom virtuální počítače, které převzetí služeb při selhání v hello stejný plán obnovení bude možné tooconnect tooeach jiných po tooAzure převzetí služeb při selhání.

Pokud chcete, aby mapování sítě toodeploy budete potřebovat následující hello:

* Hello virtuálních počítačů má tooprotect na zdrojovém serveru VMM hello by měl být připojený tooa síť virtuálních počítačů. Tuto síť by měla být propojené tooa logické sítě, který je přidružený k hello cloudu.
* Po převzetí služeb při selhání můžete připojit síti Azure toowhich replikovat virtuální počítače. Tuto síť vyberete v době hello převzetí služeb při selhání. Hello síť musí být ve hello stejné oblasti jako vaše předplatné Azure Site Recovery.

### <a name="powershell-prerequisites"></a>Požadavky prostředí PowerShell
Ujistěte se, že máte připravené toogo prostředí Azure PowerShell. Pokud už používáte prostředí PowerShell, budete potřebovat tooupgrade tooversion 0.8.10 nebo novější. Informace o nastavení prostředí PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs). Po nastavení a nakonfigurovat prostředí PowerShell, můžete zobrazit všechny hello dostupných rutin služby hello [zde](/powershell/azure/overview).

toolearn o tipy, které vám pomohou při používání hello rutin, jako je například jak hodnoty parametrů, vstupy a výstupy jsou obvykle zpracovávány v prostředí Azure PowerShell najdete v části [Začínáme s Azure rutiny](/powershell/azure/get-started-azureps).

## <a name="step-1-set-hello-subscription"></a>Krok 1: Nastavení odběru hello
V prostředí PowerShell spusťte tyto rutiny:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Hello elementů v rámci hello "< >" nahraďte konkrétní informace.

## <a name="step-2-create-a-site-recovery-vault"></a>Krok 2: Vytvoření trezoru Site Recovery
V prostředí PowerShell hello elementů v rámci hello "< >" nahraďte konkrétní informace a spusťte tyto příkazy:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Krok 3: Vygenerování registračního klíče trezoru
Vygenerujte registrační klíč v trezoru hello. Po stažení hello zprostředkovatele Azure Site Recovery a nainstalujte ji na serveru VMM hello, použijete tento klíč tooregister hello VMM server v trezoru hello.

1. Soubor nastavení hello úložiště získat a nastavit kontext hello:

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. Nastavit kontext hello trezoru spuštěním hello následující příkazy:

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-hello-azure-site-recovery-provider"></a>Krok 4: Instalace hello zprostředkovatele Azure Site Recovery
1. Na počítači VMM hello vytvořte adresář spuštěním hello následující příkaz:

   ```

   pushd C:\ASR\

   ```
2. Extrahujte soubory hello pomocí zprostředkovatele hello stáhnout tak, že spustíte následující příkaz hello

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. Nainstalujte zprostředkovatele hello pomocí hello následující příkazy:

   ```

   .\SetupDr.exe /i

   ```

   ```

   $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
         $isNotInstalled = $false;
     }
   }While($isNotInstalled)

   ```

   Počkejte toofinish instalace hello.
4. Hello server zaregistrujte v trezoru hello pomocí hello následující příkaz:

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a>Krok 5: Vytvoření účtu úložiště Azure
Pokud nemáte účet úložiště Azure, vytvořte účet povolenou geografickou replikací spuštěním hello následující příkaz:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Všimněte si, že účet úložiště hello musí být ve stejné oblasti jako služba Azure Site Recovery hello hello a přidružen hello stejného předplatného.

## <a name="step-6-install-hello-azure-recovery-services-agent"></a>Krok 6: Instalace hello agenta služeb zotavení Azure
Z hello portál Azure, nainstalujte agenta služeb zotavení Azure hello na každém serveru hostitele technologie Hyper-V v cloudech VMM hello chcete tooprotect.

Spusťte následující příkaz na všech hostitelích VMM hello:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Krok 7: Konfigurace cloudu nastavení ochrany
1. Vytvořte tooAzure profilu ochrany cloudu spuštěním hello následující příkaz:

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. Kontejner ochrany získáte spuštěním hello následující příkazy:

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. Přidružení hello kontejneru ochrany hello začněte hello cloudu:

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. Po dokončení úlohy hello spusťte hello následující příkaz:

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. Po dokončení zpracování úlohy hello spusťte hello následující příkaz:

        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).

## <a name="step-8-configure-network-mapping"></a>Krok 8: Nakonfigurování mapování sítě
Před zahájením mapování sítě ověřte, zda virtuální počítače na zdrojovém serveru VMM hello jsou připojené tooa síť virtuálních počítačů. Kromě toho můžete vytvořte jeden nebo více virtuálních sítí Azure. Všimněte si, že více sítí virtuálních počítačů může být namapované tooa jednu síť Azure.

Všimněte si, že pokud hello Cílová síť více podsítí a jedna z těchto podsítí má hello stejný název jako podsíť, na které hello zdrojový virtuální počítač se nachází, pak bude virtuální počítač repliky hello připojené toothat cílové podsíti po převzetí služeb při selhání. Pokud není cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.

První příkaz Hello získá servery pro aktuální trezoru Azure Site Recovery hello. příkaz Hello ukládá v proměnné pole hello $Servers hello servery Microsoft Azure Site Recovery.

    $Servers = Get-AzureSiteRecoveryServer


Hello druhém příkazu je získán hello síti pro obnovení lokality pro první server hello hello $Servers pole. příkaz Hello ukládá hello sítě hello $Networks proměnné.

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

třetí příkaz Hello získá předplatné Azure pomocí rutiny Get-AzureSubscription hello a pak uloží tuto hodnotu v hello $Subscriptions proměnné.

    $Subscriptions = Get-AzureSubscription



příkaz čtvrtý Hello získá virtuálních sítí Azure pomocí rutiny Get-AzureVNetSite hello a potom tuto hodnotu v hello $AzureVmNetworks proměnné.

    $AzureVmNetworks = Get-AzureVNetSite



Hello konečné rutina vytvoří mapování mezi primární a sítí virtuálních počítačů Azure hello hello. rutiny Hello určuje hello primární síť jako první prvek $Networks hello. rutiny Hello určuje síť virtuálních počítačů jako první prvek hello $AzureVmNetworks pomocí jeho ID. příkaz Hello obsahuje ID vašeho předplatného Azure.

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Krok 9: Povolení ochrany pro virtuální počítače
Po serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu pro virtuální počítače v cloudu hello. Vezměte na vědomí následující hello:

Virtuální počítače, musí splňovat [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

tooenable ochrany hello operační systém a vlastnosti disku operačního systému musí být nastavena pro virtuální počítač hello. Při vytváření virtuálního počítače v nástroji VMM pomocí šablony virtuálního počítače můžete nastavit vlastnost hello. Můžete také nastavit tyto vlastnosti pro existující virtuální počítače v hello **Obecné** a **konfigurace hardwaru** karty hello vlastnosti virtuálního počítače. Pokud tyto vlastnosti nenastavíte v nástroji VMM budete moct tooconfigure je na portálu Azure Site Recovery hello.

1. Ochrana tooenable, spusťte následující příkaz tooget hello ochrany kontejneru hello:

     $ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-název $CloudName
2. Entita ochrany hello (VM) získáte spuštěním hello následující příkaz:

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. Povolte hello zotavení po Havárii pro hello virtuálních počítačů tak, že spustíte následující příkaz hello:

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a>Otestujte nasazení
Plánování nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro hello tootest. Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti. Poznámky:

* Pokud chcete, aby tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy po převzetí služeb při selhání hello, povolte připojení ke vzdálené ploše na virtuálním počítači hello před spuštěním hello testovací převzetí služeb při selhání.
* Po převzetí služeb při selhání budete používat veřejnou IP adresu tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy. Pokud chcete, toodo to, ujistěte se, že nemáte žádné zásady domény, které vám zabrání připojování tooa virtuálnímu počítači pomocí veřejné adresy.

dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).

### <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení
1. Vytvoření souboru XML jako šablona pro váš plán obnovení pomocí hello dat níže a pak ho uložte jako "C:\RPTemplatePath.xml".
2. Změnit hello RecoveryPlan uzlu Id, názvu, PrimaryServerId a SecondaryServerId.
3. Změňte hello ProtectionEntity uzlu PrimaryProtectionEntityId (vmid z nástroje VMM).
4. Přidáním více uzlů ProtectionEntity můžete přidat další virtuální počítače.

        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"     PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"     SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03-    cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"     Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"     After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



1. Vyplňte hello dat v šabloně hello:

        $TemplatePath = "C:\RPTemplatePath.xml";



1. Vytvořte hello RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
1. Získejte objekt RecoveryPlan hello spuštěním hello následující příkaz:

     $RPObject = get-AzureSiteRecoveryRecoveryPlan-název $RPName;
2. Spusťte hello testovací převzetí služeb při selhání spuštěním hello následující příkaz:

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <a name=monitor></a>Sledování aktivity
Použijte následující příkazy toomonitor hello aktivity hello. Všimněte si, že máte toowait mezi úlohy pro zpracování toofinish hello.

    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Další kroky
[Další informace](/powershell/azure/overview) o rutin Powershellu pro Azure Site Recovery. </a>.
