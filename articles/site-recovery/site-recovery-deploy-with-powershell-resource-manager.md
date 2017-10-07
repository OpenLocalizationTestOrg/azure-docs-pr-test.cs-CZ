---
title: "aaaReplicate virtuálních počítačů technologie Hyper-V pomocí prostředí PowerShell a Azure Resource Manager | Microsoft Docs"
description: "Automatizovat replikaci hello tooAzure virtuálních počítačů Hyper-V s Azure Site Recovery pomocí Powershellu a Azure Resource Manager."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: 4fb15ce2e9ad54f1dd6f54ff769eb912aa4b0272
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Replikaci mezi místními technologie Hyper-V virtuální počítače a Azure pomocí prostředí PowerShell a Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Portál Classic](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a>Přehled
Azure Site Recovery přispívá tooyour obchodní kontinuitu a po havárii obnovení strategie tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů v různých scénářích nasazení. Úplný seznam scénářů nasazení najdete v tématu hello [přehled Azure Site Recovery](site-recovery-overview.md).

Prostředí Azure PowerShell je modul, který poskytuje toomanage rutiny Azure pomocí prostředí Windows PowerShell. Můžete pracovat se dva typy modulů: hello profilu Azure modulu nebo modulu Azure Resource Manager hello.

Rutiny prostředí PowerShell obnovení lokality pomocí prostředí Azure PowerShell pro Azure Resource Manager, můžete chránit a obnovit vaše servery v Azure.

Tento článek popisuje, jak toouse prostředí Windows PowerShell, společně s Azure Resource Manager, tooconfigure toodeploy Site Recovery a orchestraci tooAzure ochrany serveru. Příklad Hello používané v tomto článku se dozvíte, jak tooprotect, převzetí služeb při selhání a obnovení virtuálních počítačů na hostitele technologií Hyper-V tooAzure pomocí prostředí Azure PowerShell s Azure Resource Manager.

> [!NOTE]
> Hello rutiny prostředí PowerShell obnovení lokality aktuálně umožňují tooconfigure hello následující: jeden tooanother lokality nástroje Virtual Machine Manager, tooAzure lokality nástroje Virtual Machine Manager a tooAzure lokality technologie Hyper-V.
>
>

Nepotřebujete toobe odborné toouse prostředí PowerShell v tomto článku, ale potřebujete toounderstand hello základní koncepty, jako jsou moduly, rutiny a relace. Další informace o prostředí Windows PowerShell najdete v tématu [Začínáme s prostředím Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Také další informace o [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).

> [!NOTE]
> Partneři Microsoftu, které jsou součástí programu hello Cloud Solution Provider (CSP) můžete konfigurovat a spravovat ochranu systému serverů se svým zákazníkům tootheir zákazníků příslušného poskytovatele CSP odběry (odběry klienta).
>
>

## <a name="before-you-start"></a>Než začnete
Ujistěte se, že máte zavedenou tyto požadavky:

* A [Microsoft Azure](https://azure.microsoft.com/) účtu. Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). Kromě toho si můžete přečíst o [cenách Azure Site Recovery Manageru](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Informace o tomto vydání a jak tooinstall, najdete v části [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* Hello [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) a [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) moduly. Můžete získat nejnovější verze hello tyto moduly hello [Galerie prostředí PowerShell](https://www.powershellgallery.com/)

Tento článek ukazuje, jak toouse prostředí Azure Powershell s Azure Resource Manager tooconfigure a spravovat ochranu vašich serverů. Příklad Hello používané v tomto článku se dozvíte, jak tooprotect virtuálního počítače na hostiteli technologie Hyper-V, tooAzure spuštěná. Hello následující požadované součásti jsou konkrétní toothis příklad. Další komplexní sadu požadavků pro hello různé scénáře obnovení lokality, naleznete v dokumentaci toohello toothat scénář, která se týkají.

* Hostitele Hyper-V se systémem Windows Server 2012 R2 nebo Microsoft Hyper-V Server 2012 R2, který obsahuje jeden nebo více virtuálních počítačů.
* Servery Hyper-V připojen toohello Internetu, buď přímo nebo prostřednictvím proxy serveru.
* Hello virtuálních počítačů má tooprotect musí být v souladu s [požadavky virtuálního počítače](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="step-1-sign-in-tooyour-azure-account"></a>Krok 1: Zaregistrujte v tooyour účet Azure
1. Otevřete konzolu prostředí PowerShell a spusťte tento příkaz toosign v tooyour účet Azure. rutiny Hello otevře na webové stránce, který zobrazí výzvu k zadání přihlašovacích údajů účtu.

        Login-AzureRmAccount

    Alternativně může také obsahovat svoje přihlašovací údaje jako parametr toohello `Login-AzureRmAccount` rutiny pomocí hello `-Credential` parametr.

    Pokud jste poskytovatel CSP partnera práce jménem klienta, zadejte hello zákazníka jako klient, pomocí jejich název primární domény tenantID nebo klienta.

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. Účet může mít několik odběrů, takže přidružíte hello předplatné chcete toouse účtem hello.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. Ověřte, zda je vaše předplatné registrované toouse hello Azure zprostředkovatele služeb zotavení a obnovení lokality pomocí hello následující příkazy:

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   V hello výstup z těchto příkazů, pokud hello **RegistrationState** je nastaven příliš**registrovaná**, abyste mohli pokračovat tooStep 2. V opačném případě byste měli zaregistrovat hello chybějící zprostředkovatele v rámci vašeho předplatného.

   tooregister hello zprostředkovatele Azure Site Recovery a služeb zotavení, spusťte následující příkazy hello:

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   Ověří, zda hello zprostředkovatelé úspěšně registrován pomocí hello následující příkazy: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` a `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-hello-recovery-services-vault"></a>Krok 2: Nastavení hello trezor služeb zotavení
1. Vytvořte skupinu prostředků Azure Resource Manager, ve kterém budete vytvoření trezoru hello nebo použít existující skupinu prostředků. Můžete vytvořit novou skupinu prostředků s použitím hello následující příkaz:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    kde hello $ResourceGroupName proměnná obsahuje hello název skupiny prostředků hello chcete toocreate a proměnná hello $Geo obsahuje hello Azure oblast, ve které toocreate skupině prostředků hello (například "Brazílie – jih").

    Seznam skupin prostředků v rámci vašeho předplatného můžete získat pomocí hello `Get-AzureRmResourceGroup` rutiny.
2. Vytvořte nový trezor služeb zotavení Azure následujícím způsobem:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Seznam trezorů existující můžete načíst pomocí hello `Get-AzureRmRecoveryServicesVault` rutiny.

> [!NOTE]
> Pokud chcete tooperform operací na trezorů Site Recovery vytvořené pomocí portálu classic hello nebo modulu Azure Service Management PowerShell text hello, můžete načíst seznam takové trezorů pomocí hello `Get-AzureRmSiteRecoveryVault` rutiny. Měli byste vytvořit nový trezor služeb zotavení pro všechny nové operace. Hello trezorů Site Recovery, který jste dříve vytvořili podporují, ale nemáte hello nejnovější funkce.
>
>

## <a name="step-3-set-hello-recovery-services-vault-context"></a>Krok 3: Nastavte hello kontextu trezoru služeb zotavení
1. Nastavit kontext hello trezoru spuštěním hello následující příkaz:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-hello-site"></a>Krok 4: Vytvoření serveru technologie Hyper-V a vygenerovat nový registrační klíč trezoru pro lokalitu hello.
1. Vytvoří nový web s technologií Hyper-V následujícím způsobem:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Tato rutina se spustí lokality hello toocreate úlohy Site Recovery a vrátí objekt úlohy Site Recovery. Počkejte toocomplete hello úlohy a ověřte, že hello úloha úspěšně dokončena.

    Můžete načíst objekt úlohy hello a tím i kontrola hello aktuální stav úlohy hello, pomocí rutiny Get-AzureRmSiteRecoveryJob hello.
2. Vygenerování a stažení registrační klíč pro lokalitu hello následujícím způsobem:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Kopírování hello stáhnout hostitele klíče toohello technologie Hyper-V. Je nutné hello klíče tooregister hello technologie Hyper-V hostiteli toohello lokality.

## <a name="step-5-install-hello-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Krok 5: Instalace hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure na vašem hostiteli Hyper-V
1. Stažení instalačního programu hello hello nejnovější verzi poskytovatele hello z [Microsoft](https://aka.ms/downloaddra).
2. Instalační program spusťte hello na vašem hostiteli Hyper-V a na konci hello hello instalace pokračovat v kroku toohello registrace.
3. Po zobrazení výzvy zadejte, že hello stáhli lokality registrační klíč a dokončení registrace web toohello hostitele Hyper-V hello.
4. Ověřte, že je tento hostitel hello technologie Hyper-V lokality registrované toohello pomocí hello následující příkaz:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-hello-protection-container"></a>Krok 6: Vytvoření zásady replikace a přidružte ji k kontejneru ochrany hello
1. Vytvořte zásadu replikace následujícím způsobem:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Kontrola hello vrátil tooensure úlohu, která byla úspěšná, vytvoření zásad replikace hello.

   > [!IMPORTANT]
   > Hello zadaný účet úložiště by měl být ve stejné oblasti Azure jako trezor služeb zotavení hello a musí mít povolenou geografickou replikací.
   >
   > * Pokud hello zadaný účet úložiště je typu Azure Storage (klasické) pro obnovení, převzetí služeb při selhání hello chráněné počítače obnovit hello počítač tooAzure IaaS (klasické).
   > * Pokud hello zadané obnovení účtu úložiště je typu úložiště Azure (Azure Resource Manager), převzetí služeb při selhání hello chráněné počítače obnovit hello počítač tooAzure IaaS (Azure Resource Manager).
   >
   >
2. Získáte hello ochrany kontejneru odpovídající toohello lokality, následujícím způsobem:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Začněte hello přidružení kontejneru ochrany hello hello zásady replikace, následujícím způsobem:

     $Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = Start AzureRmSiteRecoveryPolicyAssociationJob-zásady $Policy - PrimaryProtectionContainer $protectionContainer

   Počkejte hello přidružení úlohy toocomplete a ujistěte se, že byla úspěšně dokončena.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Krok 7: Povolení ochrany pro virtuální počítače
1. Získáte hello ochrany entity odpovídající toohello virtuálních počítačů, které chcete tooprotect, následujícím způsobem:

        $VMFriendlyName = "Fabrikam-app"                    #Name of hello VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Začněte s ochranou hello virtuálního počítače, následujícím způsobem:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > Hello zadaný účet úložiště by měl být ve stejné oblasti Azure jako trezor služeb zotavení hello a musí mít povolenou geografickou replikací.
   >
   > * Pokud hello zadaný účet úložiště je typu Azure Storage (klasické) pro obnovení, převzetí služeb při selhání hello chráněné počítače obnovit hello počítač tooAzure IaaS (klasické).
   > * Pokud hello zadané obnovení účtu úložiště je typu úložiště Azure (Azure Resource Manager), převzetí služeb při selhání hello chráněné počítače obnovit hello počítač tooAzure IaaS (Azure Resource Manager).
   >
   > Pokud hello chráníte virtuální počítač má více než jeden disk připojené tooit, zadejte hello disk operačního systému pomocí hello *OSDiskName* parametr.
   >
   >
3. Počkejte tooreach hello virtuální počítače chráněné stavu po počáteční replikaci hello. To může nějakou dobu trvat, v závislosti na faktorech jako hello množství dat toobe replikovat a hello tooAzure dostupnou šířku pásma nadřazený. Stav úlohy Hello a StateDescription jsou aktualizované následujícím způsobem, při hello dosažení chráněném stavu virtuálního počítače.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Aktualizujte vlastnosti, obnovení, například hello velikost role virtuálního počítače a hello síť Azure tooattach hello virtuálního počítače síťových rozhraní karty tooupon převzetí služeb při selhání.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update hello virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Krok 8: Spustit testovací převzetí služeb
1. Testovací převzetí služeb při selhání úlohy, spusťte následujícím způsobem:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Ověřte testu hello virtuálních počítačů je vytvořena v Azure. (hello testovací převzetí služeb při selhání úlohy je pozastaven, po vytvoření hello testovací virtuální počítač v Azure. Hello dokončení úlohy tak, že vyčistí rušivé vlivy hello vytvořen při obnovení úlohy hello, jak je ukázáno v dalším kroku hello.)
3. Dokončení hello testovací převzetí služeb při selhání, následujícím způsobem:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Další kroky
[Další informace](https://msdn.microsoft.com/library/azure/mt637930.aspx) o Azure Site Recovery pomocí rutin prostředí PowerShell Azure Resource Manager.
