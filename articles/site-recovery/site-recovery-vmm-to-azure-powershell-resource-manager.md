---
title: "virtuální počítače aaaReplicate technologie Hyper-V v cloudech VMM pomocí Azure Site Recovery a prostředí PowerShell (Resource Manager) | Microsoft Docs"
description: "Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM pomocí Azure Site Recovery a prostředí PowerShell"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: raynew
ms.assetid: 6ac509ad-5024-43d8-b621-d8fec019b9a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: b0f641de4e10a600ead415ceb9bd488fb4d1659d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-powershell-and-azure-resource-manager"></a>Replikace virtuálních počítačů technologie Hyper-V v tooAzure cloudů VMM pomocí prostředí PowerShell a Azure Resource Manager
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

Hello článek obsahuje požadavky pro hello scénář a ukazuje

* Jak tooset do trezoru služeb zotavení
* Nainstalujte na zdrojovém serveru VMM hello hello zprostředkovatele Azure Site Recovery
* Zaregistrujte hello server v hello trezoru, přidat účet úložiště Azure
* Nainstalujte agenta služeb zotavení Azure hello na hostitelských serverech technologie Hyper-V
* Konfigurovat nastavení ochrany pro cloudy VMM, které budou použité tooall chráněné virtuální počítače
* Povolení ochrany pro tyto virtuální počítače.
* Testovací převzetí služeb při selhání toomake hello, se, že vše funguje podle očekávání.

Pokud narazíte na problémy nastavení tento scénář, zveřejněte svoje otázky na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Resource Manager hello.
>
>

## <a name="before-you-start"></a>Než začnete
Ujistěte se, že máte zavedenou tyto požadavky:

### <a name="azure-prerequisites"></a>Požadavky Azure
* Budete potřebovat účet [Microsoft Azure](https://azure.microsoft.com/). Pokud nemáte, začínat [bezplatný účet](https://azure.microsoft.com/free). Kromě toho si můžete přečíst o hello [cenách Azure Site Recovery Manageru](https://azure.microsoft.com/pricing/details/site-recovery/).
* Pokud se pokoušíte se hello replikace tooa CSP předplatné scénáři budete potřebovat předplatného poskytovatele CSP. Další informace o programu hello CSP v [jak tooenroll CSP programu hello](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
* Budete potřebovat tooAzure dat replikovaných účet toostore Azure v2 úložiště (Resource Manager). Hello účet potřebuje povolenou geografickou replikací. Je nutné v hello stejné oblasti jako hello služba Azure Site Recovery a možné přidružit k hello stejného předplatného nebo hello předplatného poskytovatele CSP. toolearn Další informace o nastavení služby Azure storage, najdete v části hello [tooMicrosoft Úvod Azure Storage](../storage/common/storage-introduction.md) pro referenci.
* Musíte mít jistotu, že virtuální počítače, které chcete tooprotect vyhovují hello toomake [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

> [!NOTE]
> V současné době pouze úrovně operace virtuálního počítače je možné pomocí prostředí Powershell. Podpora pro úrovně operace plánu obnovení, bude brzy se.  Zatím jste omezené tooperforming selhání Center pouze na rozlišením "chráněné virtuální počítač" a ne na úrovni plán obnovení.
>
>

### <a name="vmm-prerequisites"></a>Požadavky VMM
* Budete potřebovat VMM server běžící na System Center 2012 R2.
* Jakýkoli server VMM obsahuje virtuální počítače budete chtít tooprotect musí být spuštěna hello zprostředkovatele Azure Site Recovery. To je nainstalován během hello nasazení Azure Site Recovery.
* Budete potřebovat alespoň jeden cloud na serveru VMM má tooprotect hello. Hello cloud by měl obsahovat:
  * Minimálně jednu skupinu hostitelů VMM.
  * Minimálně jeden hostitelský server nebo cluster Hyper-V v každé skupině hostitelů.
  * Jeden nebo více virtuálních počítačů na serveru hello zdroj technologie Hyper-V.
* Další informace o nastavení cloudů VMM:
  * Další informace o privátních cloudů VMM ve [co je nového v privátním cloudu s nástrojem System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) a v [cloudy VMM 2012 a hello](http://go.microsoft.com/fwlink/?LinkId=324956).
  * Další informace o [konfigurace hello VMM cloudových prostředků infrastruktury](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
  * Po vaší cloudové infrastruktury prvky jsou zavedené Další informace o vytváření privátních cloudů ve [vytvoření privátního cloudu v nástroji VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) a v [návod: vytvoření privátních cloudů pomocí System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).

### <a name="hyper-v-prerequisites"></a>Požadavky technologie Hyper-V
* Hello servery hostitele technologie Hyper-V musí běžet minimálně **systému Windows Server 2012** s rolí Hyper-V nebo **Microsoft Hyper-V Server 2012** a mít hello nainstalované nejnovější aktualizace.
* Pokud používáte technologii Hyper-V v clusteru, je důležité vědět, že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte clustery založené na statických IP adresách. Zprostředkovatel clusteru hello tooconfigure budete potřebovat ručně. Pro
* Pokyny naleznete v části [jak tooConfigure zprostředkovatele replik technologie Hyper-V](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
* Libovolný server hostitele technologie Hyper-V nebo clusteru, pro které chcete toomanage ochrany musí být součástí cloudu VMM.

### <a name="network-mapping-prerequisites"></a>Požadavky na mapování sítě
Když chráníte virtuální počítače v Azure, hello mapování sítě zajišťuje mapování sítě virtuálních počítačů hello na serveru VMM hello zdrojové a cílové sítě Azure tooenable hello následující:

* Všechny počítače, které převzetí služeb při selhání na hello stejné tooeach jiných, bez ohledu na plánu obnovení se mohou připojit.
* Pokud bránu sítě je nastavena na hello cílovou síť Azure, můžete virtuální počítače připojit tooother místní virtuální počítače.
* Pokud nenakonfigurujete mapování sítě, hello pouze virtuální počítače, které převzetí služeb při selhání v hello stejný plán obnovení bude možné tooconnect tooeach jiných po tooAzure převzetí služeb při selhání.

Pokud chcete, aby mapování sítě toodeploy budete potřebovat následující hello:

* Hello virtuálních počítačů má tooprotect na zdrojovém serveru VMM hello by měl být připojený tooa síť virtuálních počítačů. Tuto síť by měla být propojené tooa logické sítě, který je přidružený k hello cloudu.
* Po převzetí služeb při selhání můžete připojit síti Azure toowhich replikovat virtuální počítače. Tuto síť vyberete v době hello převzetí služeb při selhání. Hello síť musí být ve hello stejné oblasti jako vaše předplatné Azure Site Recovery.

Další informace o mapování sítě v

* [Jak tooconfigure logické sítě v nástroji VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [Jak tooconfigure virtuálních počítačů sítí a bran v nástroji VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
* [Jak tooconfigure a monitorování virtuálních sítí v Azure](https://azure.microsoft.com/documentation/services/virtual-network/)

### <a name="powershell-prerequisites"></a>Požadavky prostředí PowerShell
Ujistěte se, že máte připravené toogo prostředí Azure PowerShell. Pokud už používáte prostředí PowerShell, budete potřebovat tooupgrade tooversion 0.8.10 nebo novější. Informace o nastavení prostředí PowerShell najdete v tématu hello [Průvodce tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs). Po nastavení a nakonfigurovat prostředí PowerShell, můžete zobrazit všechny hello dostupných rutin služby hello [zde](/powershell/azure/overview).

toolearn o tipy, které vám pomohou při používání hello rutin, jako je například jak hodnoty parametrů, vstupy a výstupy jsou obvykle zpracovávány v prostředí Azure PowerShell najdete v části hello [Průvodce Začínáme s rutinami Azure tooget](/powershell/azure/get-started-azureps).

## <a name="step-1-set-hello-subscription"></a>Krok 1: Nastavení odběru hello
1. Z prostředí Azure powershell, tooyour přihlašovací účet Azure: pomocí následující rutiny hello

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Získejte seznam předplatných. Pro každé z předplatných hello to taky zobrazí seznam hello subscriptionIDs. Poznamenejte si ID předplatného hello hello předplatného, ve kterém chcete trezor služeb zotavení toocreate hello

        Get-AzureRmSubscription
3. Nastavit hello předplatné, ve které hello trezoru služeb zotavení je toobe vytvořené zmínit ID předplatného hello

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a>Krok 2 – Vytvoření trezoru Služeb zotavení.
1. Vytvořte skupinu prostředků ve službě Správce prostředků Azure, pokud již nemáte

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Vytvořit nový trezor služeb zotavení a uložte hello vytvořený objekt trezoru automatické obnovení systému v proměnné (použijí později). Můžete také načíst hello automatické obnovení systému trezoru post vytvoření objektu pomocí rutiny Get-AzureRMRecoveryServicesVault hello:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a>Krok 3: Nastavte kontext hello trezoru služeb zotavení

Nastavit kontext hello trezoru spuštěním hello následující příkaz.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-hello-azure-site-recovery-provider"></a>Krok 4: Instalace hello zprostředkovatele Azure Site Recovery
1. Na počítači VMM hello vytvořte adresář spuštěním hello následující příkaz:

       New-Item c:\ASR -type directory
2. Extrahujte soubory hello pomocí zprostředkovatele hello stáhnout tak, že spustíte následující příkaz hello

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Nainstalujte zprostředkovatele hello pomocí hello následující příkazy:

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

   Počkejte toofinish instalace hello.
4. Hello server zaregistrujte v trezoru hello pomocí hello následující příkaz:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-an-azure-storage-account"></a>Krok 5: Vytvoření účtu úložiště Azure

Pokud nemáte účet úložiště Azure, vytvořte účet povolenou geografickou replikací v hello stejném geograficky redundantním jako hello trezoru spuštěním hello následující příkaz:

        $StorageAccountName = "teststorageacc1"    #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"     
        $ResourceGroupName =  “myRG”             #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Všimněte si, že účet úložiště hello musí být ve stejné oblasti jako služba Azure Site Recovery hello hello a přidružen hello stejného předplatného.

## <a name="step-6-install-hello-azure-recovery-services-agent"></a>Krok 6: Instalace hello agenta služeb zotavení Azure
1. Stáhnout agenta služeb zotavení Azure hello v [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) a nainstalujte ji na každém serveru hostitele technologie Hyper-V umístěný v hello VMM cloudů je chcete tooprotect.
2. Spusťte následující příkaz na všech hostitelích VMM hello:

       marsagentinstaller.exe /q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>Krok 7: Konfigurace cloudu nastavení ochrany
1. Vytvořte tooAzure zásady replikace spuštěním hello následující příkaz:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

1. Kontejner ochrany získáte spuštěním hello následující příkazy:

       $PrimaryCloud = "testcloud"
       $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  
2. Získáte hello zásad podrobnosti tooa proměnnou pomocí hello úlohy, která byla vytvořena a zmínit hello zásad popisný název:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Přidružení hello kontejneru ochrany hello začněte zásady replikace hello:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  
4. Po dokončení úlohy hello spusťte hello následující příkaz:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }
5. Po dokončení zpracování úlohy hello spusťte hello následující příkaz:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).

## <a name="step-8-configure-network-mapping"></a>Krok 8: Nakonfigurování mapování sítě
Před zahájením mapování sítě ověřte, zda virtuální počítače na zdrojovém serveru VMM hello jsou připojené tooa síť virtuálních počítačů. Kromě toho můžete vytvořte jeden nebo více virtuálních sítí Azure.

Další informace o tom, jak toocreate a virtuální sítě pomocí Azure Resource Manageru a prostředí PowerShell v [vytvořit virtuální síť s připojením VPN typu site-to-site pomocí Azure Resource Manageru a prostředí PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Všimněte si, že více sítí virtuálních počítačů může být namapované tooa jednu síť Azure. Pokud hello Cílová síť více podsítí a jedna z těchto podsítí má hello nachází stejný název jako podsíť, na které hello zdrojového virtuálního počítače a potom virtuální počítač repliky hello budou po převzetí služeb při selhání připojené toothat cílové podsíti. Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.

1. První příkaz Hello získá servery pro aktuální trezoru Azure Site Recovery hello. příkaz Hello ukládá v proměnné pole hello $Servers hello servery Microsoft Azure Site Recovery.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Hello druhém příkazu je získán hello síti pro obnovení lokality pro první server hello hello $Servers pole. příkaz Hello ukládá hello sítě hello $Networks proměnné.

        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

1. třetí příkaz Hello získá virtuálních sítí Azure a pak tuto hodnotu v hello $AzureVmNetworks proměnné.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork
2. Hello konečné rutina vytvoří mapování mezi primární a sítí virtuálních počítačů Azure hello hello. rutiny Hello určuje hello primární síť jako první prvek $Networks hello. rutiny Hello určuje síť virtuálních počítačů jako první prvek $AzureVmNetworks hello.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]

## <a name="step-9-enable-protection-for-virtual-machines"></a>Krok 9: Povolení ochrany pro virtuální počítače
Po hello serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu pro virtuální počítače v cloudu hello.

 Vezměte na vědomí následující hello:

* Virtuální počítače musí splňovat požadavky pro Azure. Zkontrolujte tyto v [požadavky a podpora](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) v příručce plánování na hello.
* tooenable ochrany, hello operačního systému a vlastnosti disku operačního systému musí být nastavena pro virtuální počítač hello. Při vytváření virtuálního počítače v nástroji VMM pomocí šablony virtuálního počítače můžete nastavit vlastnost hello. Můžete také nastavit tyto vlastnosti pro existující virtuální počítače v hello **Obecné** a **konfigurace hardwaru** karty hello vlastnosti virtuálního počítače. Pokud tyto vlastnosti nenastavíte v nástroji VMM budete moct tooconfigure je na portálu Azure Site Recovery hello.

1. Ochrana tooenable, spusťte následující příkaz tooget hello ochrany kontejneru hello:

          $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName
2. Entita ochrany hello (VM) získáte spuštěním hello následující příkaz:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer
3. Povolte hello zotavení po Havárii pro hello virtuálních počítačů tak, že spustíte následující příkaz hello:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1

## <a name="test-your-deployment"></a>Otestujte nasazení
tootest nasazení můžete spustit test převzetí služeb při selhání pro jeden virtuální počítač, nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spusťte test selhání pro plán hello. Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a obnovení v izolované síti. Poznámky:

* Pokud chcete, aby tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy po převzetí služeb při selhání hello, povolte připojení ke vzdálené ploše na virtuálním počítači hello před spuštěním hello testovací převzetí služeb při selhání.
* Po převzetí služeb při selhání použijete veřejnou IP adresu tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy. Pokud chcete, toodo to, ujistěte se, že nemáte žádné zásady domény, které vám zabrání připojování tooa virtuálnímu počítači pomocí veřejné adresy.

dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
- Spusťte hello testovací převzetí služeb při selhání spuštěním hello následující příkaz:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Spusťte plánované převzetí služeb při selhání
- Spuštění hello plánované převzetí služeb při selhání tak, že spustíte následující příkaz hello:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Spustit neplánované převzetí služeb při selhání
- Spustit hello neplánované převzetí služeb při selhání tak, že spustíte následující příkaz hello:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

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
[Další informace](/powershell/module/azurerm.recoveryservices.backup/#recovery) o Azure Site Recovery pomocí rutin prostředí PowerShell Azure Resource Manager.
