---
title: "aaaReplicate virtuální počítače Hyper-V ve VMM tooa sekundární lokality pomocí prostředí PowerShell (Azure Resource Manager) | Microsoft Docs"
description: "Popisuje, jak toodeploy Azure Site Recovery tooorchestrate replikace, převzetí služeb při selhání a obnovení virtuálních počítačů technologie Hyper-V v nástroji VMM cloudů sekundární lokalita VMM tooa pomocí prostředí PowerShell (Resource Manager)"
services: site-recovery
documentationcenter: 
author: sujaytalasila
manager: rochakm
editor: raynew
ms.assetid: 9d38e9c3-217c-4e44-830c-575e9a4141f2
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: a769dcc68d66c18b9dc47539071f4d0e0f1db70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site-using-powershell-resource-manager"></a>Replikace virtuálních počítačů technologie Hyper-V v nástroji VMM cloudy tooa sekundární lokalita VMM pomocí prostředí PowerShell (Resource Manager)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Portál Classic](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Vítejte tooAzure Site Recovery! Tento článek použijte, pokud chcete, aby tooreplicate místní virtuálních počítačů technologie Hyper-V spravovaných v System Center Virtual Machine Manager (VMM) cloudy tooa sekundární lokality.

Tento článek ukazuje, jak toouse prostředí PowerShell tooautomate běžné úlohy je nutné tooperform při nastavování virtuální počítače Azure Site Recovery tooreplicate Hyper-V v cloudech VMM tooSystem Center System Center VMM cloudy v sekundární lokalitě.

Hello článek obsahuje požadavky pro hello scénář a ukazuje

* Jak tooset do trezoru služeb zotavení
* Nainstalujte hello zprostředkovatele Azure Site Recovery na serveru VMM hello zdrojovém a cílovém serveru VMM hello
* Zaregistrujte hello servery VMM v trezoru hello
* Nakonfigurujte zásady replikace pro hello cloudu VMM. nastavení replikace Hello v zásadách hello bude použité tooall chráněné virtuální počítače
* Povolení ochrany pro virtuální počítače hello.
* Testovací převzetí služeb při selhání hello virtuálních počítačů jednotlivě nebo jako součást toomake plán obnovení, která je opravdu že všechno funguje podle očekávání.
* Proveďte plánované nebo neplánované převzetí služeb při selhání virtuálních počítačů jednotlivě nebo jako součást toomake plán obnovení, která je opravdu že všechno funguje podle očekávání.

Pokud narazíte na problémy nastavení tento scénář, zveřejněte svoje otázky na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure má dva různé [modely nasazení](../azure-resource-manager/resource-manager-deployment-model.md) pro vytváření prostředků a práci s nimi: Azure Resource Manager a klasický model. Azure má také dva portály – portál Azure classic, která podporuje model nasazení classic hello hello a hello portál Azure s podporou pro oba modely nasazení. Tento článek se týká modelu nasazení Resource Manager hello.
>
>

## <a name="on-premises-prerequisites"></a>Místní požadavky
Tady je co budete potřebovat v hello primární a sekundární místní lokality toodeploy tento scénář:

| **Požadavky** | **Podrobnosti** |
| --- | --- |
| **VMM** |Doporučujeme že nasadit server VMM v primární lokalitě hello a server VMM v sekundární lokalitě hello.<br/><br/> Můžete také [replikace mezi cloudy na jednom serveru VMM](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment). toodo to budete potřebovat aspoň dva cloudy, které jsou nakonfigurované na serveru VMM hello.<br/><br/> Servery VMM by měl používat minimálně System Center 2012 SP1 s nejnovějšími aktualizacemi hello.<br/><br/> Každý server VMM musí mít na jednom nebo více cloudů nakonfigurované a všechny cloudy musí mít nastaven profil hello kapacity technologie Hyper-V. <br/><br/>Cloudy musí obsahovat jeden nebo více skupin hostitelů VMM.<br/><br/>Další informace o nastavení cloudů VMM v [konfigurace hello VMM cloudové infrastruktury](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [návod: vytvoření privátních cloudů pomocí System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> Servery VMM by měl mít přístup k Internetu. |
| **Hyper-V** |Servery Hyper-V musí běžet minimálně Windows Server 2012 s rolí hello technologie Hyper-V a mít hello nainstalované nejnovější aktualizace.<br/><br/> Server Hyper-V by měl obsahovat jeden nebo více virtuálních počítačů.<br/><br/>  Hostitelské servery Hyper-V by měl být umístěn v skupiny hostitelů v hello primárních a sekundárních cloudech VMM.<br/><br/> Pokud používáte technologii Hyper-V v clusteru na Windows Server 2012 R2 nainstalujte [aktualizovat 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Pokud používáte technologii Hyper-V v clusteru na Windows Server 2012 Poznámka že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte statické IP adresy na základě clusteru. Zprostředkovatel clusteru hello tooconfigure budete potřebovat ručně. [Přečtěte si další informace](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Poskytovatel** |Během nasazování Site Recovery nainstalujete zprostředkovatele Azure Site Recovery hello na serverech VMM. Hello zprostředkovatele Site Recovery komunikuje přes protokol HTTPS 443 tooorchestrate replikace. Dojde k replikaci dat mezi hello primární a sekundární servery technologie Hyper-V přes hello LAN nebo připojení k síti VPN.<br/><br/> Hello zprostředkovatel, který běží na serveru VMM hello potřebuje přístup k adresám URL toothese: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.<br/><br/> Kromě toho povolit bránu firewall komunikaci z toohello servery VMM hello [rozsahy IP adres Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) a povolit protokol HTTPS (443) hello. |

### <a name="network-mapping-prerequisites"></a>Požadavky na mapování sítě
Mapování mapuje sítě mezi sítěmi virtuálních počítačů nástroje VMM na hello primární a sekundární servery VMM pro:

* Optimálně umístíte virtuální počítače repliky na sekundární hostitelů Hyper-V po převzetí služeb při selhání.
* Připojte virtuální počítače repliky, tooappropriate sítě virtuálních počítačů.
* Pokud nenakonfigurujete sítě mapování repliky virtuální počítače nebudou připojené tooany sítě po převzetí služeb při selhání.
* Pokud chcete, aby tooset sítě mapování během obnovení lokality tady je nasazení co budete potřebovat:

  * Zajistěte, aby virtuální počítače na zdrojovém hello hostitelský server Hyper-V byla připojené tooa sítě virtuálních počítačů nástroje VMM. Tuto síť by měla být propojené tooa logické sítě, který je přidružený k hello cloudu.
  * Ověřte, zda text hello sekundární cloudu, který budete používat pro obnovení má odpovídající síť virtuálních počítačů konfigurovanou. Tato síť virtuálních počítačů musí být propojené tooa logické sítě, který je spojen s hello sekundární cloudu.

Další informace o konfiguraci sítě VMM v hello níže články

* [Jak tooconfigure logické sítě v nástroji VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [Jak tooconfigure virtuálních počítačů sítí a bran v nástroji VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Další informace](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) o tom, jak funguje mapování sítě.

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
1. Vytvořte skupinu prostředků Azure Resource Manager, pokud již nemáte

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Vytvořit nový trezor služeb zotavení a uložte hello vytvořený objekt trezoru automatické obnovení systému v proměnné (použijí později). Můžete také načíst hello automatické obnovení systému trezoru post vytvoření objektu pomocí rutiny Get-AzureRMRecoveryServicesVault hello:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a>Krok 3: Nastavte kontext hello trezoru služeb zotavení
1. Pokud máte již vytvořili trezor, spusťte hello níže trezor hello tooget příkaz.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Nastavit kontext hello trezoru spuštěním hello následující příkaz.

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

## <a name="step-5-create-and-associate-a-replication-policy"></a>Krok 5: Vytvořte a přidružte zásadu replikace
1. Vytvořte zásadu replikace 2012 R2 Hyper-V tak, že spustíte následující příkaz hello:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify hello number of hours tooretain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify hello frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify hello port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > Hello cloudu VMM může obsahovat hostitelů Hyper-V s různými verzemi systému Windows Server (jak je uvedeno v hello požadavky technologie Hyper-V), ale zásady replikace hello je konkrétní verze operačního systému. Pokud máte různých hostitelích, které jsou spuštěné v různých verzí operačních systémů, vytvořte samostatné replikace zásady pro každý typ verze operačního systému. Pro např: Pokud máte pět hostitelů, které se spuštěným operačním systémem Windows 2012 serverů a tři na Windows Server 2012 R2, vytvořte dvě zásady replikace – jednu pro každý typ verze operačního systému.

1. Spustit hello následující příkazy a získáte hello primární ochranu kontejneru (primární Cloud VMM) a obnovení kontejneru ochrany (obnovení cloudu VMM):

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Načtení hello zásady, které jste vytvořili v kroku 1 pomocí hello popisný název zásady hello

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Přidružení hello hello kontejneru ochrany (VMM Cloud) začněte zásady replikace hello:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Počkejte toocomplete úlohy přidružení zásad hello. Můžete se podívat, pokud hello úlohy pomocí hello následující fragment kódu prostředí PowerShell.

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   Po dokončení zpracování úlohy hello spusťte hello následující příkaz:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).

## <a name="step-6-configure-network-mapping"></a>Krok 6: Konfigurace mapování sítě
1. První příkaz Hello získá servery pro aktuální trezoru Azure Site Recovery hello. příkaz Hello ukládá v proměnné pole hello $Servers hello servery Microsoft Azure Site Recovery.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Hello níže příkazy získat hello síti pro obnovení lokality pro hello zdrojový server VMM a hello cílovém serveru VMM.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Hello zdrojový server VMM můžete hello první z nich nebo hello druhý jeden v hello servery pole. Zkontrolujte hello názvy serverů VMM hello a odpovídajícím způsobem získat hello sítě


1. Hello konečné rutina vytvoří mapování mezi primární a sítí obnovení hello hello. rutiny Hello určuje hello primární síť jako první prvek hello $PrimaryNetworks a hello obnovení sítě jako první prvek $RecoveryNetworks hello.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a>Krok 7: Konfigurace mapování úložiště
1. Hello níže příkaz získá hello seznam klasifikací úložiště do $storageclassifications proměnné.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. Hello níže příkazy získat hello zdroj klasifikace do proměnné $SourceClassificaion a klasifikaci cíl do $TargetClassification proměnné.

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > klasifikace Hello zdrojem a cílem může být libovolný prvek v poli hello. Naleznete toohello výstup hello níže příkaz toofigure hello index zdrojové a cílové klasifikace v poli $storageclassifications.

    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object - vlastnost FriendlyName, Id | Format-Table


1. Hello níže rutina vytvoří mapování mezi hello zdroj klasifikaci a klasifikaci cíl hello.

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a>Krok 8: Povolení ochrany pro virtuální počítače
Po hello serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu pro virtuální počítače v cloudu hello.

1. Ochrana tooenable, spusťte následující příkaz tooget hello ochrany kontejneru hello:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Entita ochrany hello (VM) získáte spuštěním hello následující příkaz:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Povolení replikace pro virtuální počítač hello spuštěním hello následující příkaz:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a>Otestujte nasazení
Plánování nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro hello tootest. Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti.

> [!NOTE]
> Můžete vytvořit plán obnovení pro aplikaci na portálu Azure.
>
>

dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
1. Spustit hello níže rutiny tooget hello virtuálních počítačů sítě toowhich chcete tootest převzetí služeb při selhání pro virtuální počítače.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Proveďte testovací převzetí služeb virtuálního počítače pomocí tohoto postupu hello následující:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. Proveďte testovací převzetí služeb při selhání plánu obnovení pomocí tohoto postupu hello následující:

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a>Spusťte plánované převzetí služeb při selhání
1. Proveďte plánované převzetí služeb při selhání virtuálního počítače pomocí tohoto postupu hello následující:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. Proveďte plánované převzetí služeb při selhání plánu obnovení pomocí tohoto postupu hello následující:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Spustit neplánované převzetí služeb při selhání
1. Proveďte neplánované převzetí služeb při selhání virtuálního počítače pomocí tohoto postupu hello následující:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. proveďte neplánované převzetí služeb při selhání plánu obnovení pomocí tohoto postupu hello následující:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

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
