---
title: "aaaCreate virtuální počítač s Windows pomocí prostředí PowerShell | Microsoft Docs"
description: "Vytvořte virtuální počítače s Windows pomocí Azure PowerShell a modelu nasazení classic hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 42c0d4be-573c-45d1-b9b0-9327537702f7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 5339f458c1f08f6e1752a53191f19402fab8ea25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-hello-classic-deployment-model"></a>Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell a hello modelu nasazení classic
> [!div class="op_single_selector"]
> * [Portál Azure – Windows](tutorial.md)
> * [PowerShell – Windows](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Tyto kroky vám ukážou, jak toocustomize sadu Azure PowerShell příkazy, vytvoření a nastavení systému Windows Azure virtuálního počítače pomocí stavebním blokem přístup. Můžete použít tento proces tooquickly vytvořit sadu příkazů pro nový virtuální počítač systému Windows a rozbalte stávajícího nasazení, nebo toocreate více příkaz sad, které rychle vytváří vlastní vývoj/testování nebo pro IT prostředí.

Tyto kroky použijte přístup doplňovat-v-the-prázdné hodnoty pro vytvoření sady příkazů prostředí Azure PowerShell. Tento přístup může být užitečné, pokud jsou nové tooPowerShell nebo jenom chcete tooknow toospecify jaké hodnoty pro úspěšné konfiguraci. Pokročilí uživatelé prostředí PowerShell můžete pořídit hello příkazy a nahraďte vlastní hodnoty pro proměnné hello (hello řádky začínající "$").

Pokud jste tak ještě neučinili, použijte pokyny hello v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) tooinstall prostředí Azure PowerShell v místním počítači. Potom otevřete příkazový řádek prostředí Windows PowerShell.

## <a name="step-1-add-your-account"></a>Krok 1: Přidání účtu
1. Zadejte v příkazovém prostředí PowerShell hello **Add-AzureAccount** a klikněte na tlačítko **Enter**. 
2. Zadejte e-mailovou adresu hello spojené s předplatným Azure a klikněte na tlačítko **pokračovat**. 
3. Zadejte heslo hello k vašemu účtu. 
4. Klikněte na tlačítko **přihlášení**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Krok 2: Nastavení vaše předplatné a účet úložiště
Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell hello nastavte předplatné a účet úložiště. Nahraďte vše v rámci hello uvozovky, včetně hello < a > znaky, správné názvy hello.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Můžete získat název správné předplatné hello hello Název_předplatného vlastnost hello výstup hello **Get-AzureSubscription** příkaz. Název účtu hello správné úložiště můžete získat z hello vlastnosti Label hello výstup hello **Get-AzureStorageAccount** příkaz po spuštění hello **Select-AzureSubscription** příkaz.

## <a name="step-3-determine-hello-imagefamily"></a>Krok 3: Určení hello ImageFamily
Dále musíte toodetermine hello ImageFamily nebo hodnota popisek pro odpovídající toohello hello konkrétní image virtuálního počítače Azure chcete toocreate. Můžete získat hello seznam dostupných hodnot ImageFamily pomocí tohoto příkazu.

    Get-AzureVMImage | select ImageFamily -Unique

Tady jsou některé příklady ImageFamily hodnot pro počítače se systémem Windows:

* Windows Server 2012 R2 Datacenter
* Windows Server 2008 R2 SP1
* Windows Server 2016 Technical Preview 4
* SQL Server 2012 SP1 Enterprise na systém Windows Server 2012

Pokud zjistíte hello bitovou kopii, kterou hledáte, otevřete novou instanci hello textového editoru volba nebo hello prostředí PowerShell integrovaném skriptovacím prostředí (ISE). Zkopírujte následující hello do hello nový textový soubor nebo hello PowerShell ISE, nahraďte hodnotu ImageFamily hello.

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

V některých případech název bitové kopie hello se hello vlastnosti Label místo hello ImageFamily hodnotu. Pokud nebyl nalezen hello bitovou kopii, která hledáte pomocí vlastnosti ImageFamily hello, seznam vlastností jejich popisek pomocí tohoto příkazu hello bitové kopie.

    Get-AzureVMImage | select Label -Unique

Pokud zjistíte hello správné bitové kopie pomocí tohoto příkazu, otevřete novou instanci hello textového editoru volba nebo hello PowerShell ISE. Zkopírujte následující hello do hello nový textový soubor nebo hello PowerShell ISE, nahraďte hodnotu popisku hello.

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a>Krok 4: Vytvoření vaší sadu příkazů
Sestavení hello zbytek příkazu nastavit tak, že zkopírujete příslušnou sadu bloků níže hello do své nový textový soubor nebo hello ISE a pak zadáním hodnoty proměnné hello a odebírání hello < a > znaků. V tématu hello dva [příklady](#examples) na konci hello tohoto článku představu o konečný výsledek hello.

Spuštění příkazu nastavte volbou jedné z těchto dvou příkaz bloky (povinné).

Možnost 1: Zadejte název virtuálního počítače a velikost.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Možnost 2: Zadejte název, velikost a název sady dostupnosti.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

Hello InstanceSize hodnoty pro D – DS- a G-series virtuálních počítačů naleznete v části [virtuálního počítače a Cloud velikost služeb pro Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

> [!NOTE]
> Pokud máte smlouvu Enterprise Agreement pomocí programu Software Assurance a chcete využít tootake hello systému Windows Server [výhody použití hybridní](https://azure.microsoft.com/pricing/hybrid-use-benefit/), přidejte **- LicenseType** parametr toohello  **Nové AzureVMConfig** rutiny předávání hello hodnotu **Windows_Server** pro hello typické případy použití.  Ujistěte se, že používáte bitovou kopii, kterou jste nahráli; standardní bitové kopie z hello Galerie nelze použít s hello hybridní použít výhody.
> 
> 

Volitelně můžete pro samostatné počítače se systémem Windows zadejte hello účet místního správce a heslo.

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

Zvolte silné heslo. toocheck jeho síly, najdete v části [kontrolu hesla: pomocí silného hesla](https://www.microsoft.com/security/pc-security/password-checker.aspx).

Volitelně tooadd hello Windows počítače tooan existující doméně Active Directory, zadejte účet místního správce hello a heslo, hello domény a hello jméno a heslo účtu domény.

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
    $domaindns="<FQDN of hello domain that hello machine is joining>"
    $domacctdomain="<domain of hello account that has permission tooadd hello machine toohello domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

Další možnosti předběžné konfigurace pro virtuální počítače se systémem Windows najdete v tématu hello syntaxe hello **Windows** a **třídy WindowsDomain** parametr nastaví v [ Přidat AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).

Virtuální počítač hello volitelně přiřadíte konkrétní IP adresu, označuje jako statické vyhrazené IP adresy.

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

Můžete ověřit, že je k dispozici s konkrétní IP adresu:

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

Volitelně můžete přiřadíte konkrétní podsítě tooa hello virtuálního počítače ve virtuální sítě Azure.

    $vm1 | Set-AzureSubnet -SubnetNames "<name of hello subnet>"

Volitelně lze přidáte jeden datový disk toohello virtuálního počítače.

    $disksize=<size of hello disk in GB>
    $disklabel="<hello label on hello disk>"
    $lun=<Logical Unit Number (LUN) of hello disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

Pro řadič domény služby Active Directory, nastavte $hcaching příliš "Žádný".

Volitelně lze přidáte hello sadu virtuálních počítačů tooan existující Vyrovnávání zatížení sítě pro externí přenosy.

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of hello internal port>
    $pubport=<port number of hello external port>
    $endpointname="<name of hello endpoint>"
    $lbsetname="<name of hello existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

Nakonec vyberte jednu z těchto bloků požadovaný příkaz pro vytvoření hello virtuálního počítače.

Možnost 1: Vytvoření hello virtuálního počítače ve stávající cloudovou službu.

    New-AzureVM –ServiceName "<short name of hello cloud service>" -VMs $vm1

krátký název Hello hello cloudové služby je hello název, který se zobrazí v seznamu hello cloudové služby v hello portál Azure nebo v seznamu hello skupin prostředků v hello portálu Azure.

Možnost 2: Vytvoření hello virtuálního počítače do existující cloudové služby a virtuální sítě.

    $svcname="<short name of hello cloud service>"
    $vnetname="<name of hello virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a>Krok 5: Spuštění vaší sadu příkazů
Zkontrolovat sadu příkazů prostředí Azure PowerShell text hello, které jste vytvořili v textovém editoru nebo hello PowerShell ISE skládající se z více bloků příkazy z kroku 4. Ujistěte se, zda jste zadali všechny proměnné hello potřeby a zda mají hello správné hodnoty. Také se ujistěte, že jste odstranili všechny hello < a > znaků.

Pokud používáte textového editoru, příkaz copy hello nastavte toohello schránky a klikněte pravým tlačítkem na otevřete příkazový řádek prostředí Windows PowerShell. To bude vydávat sadu příkazů hello jako řadu příkazů prostředí PowerShell a vytvořte virtuální počítač Azure. Alternativně spusťte příkaz hello nastavit v hello PowerShell ISE.

Pokud vytvoříte tento virtuální počítač znovu nebo podobné jeden, můžete:

* Uložte tento příkaz nastavit jako soubor skriptu prostředí PowerShell (*.ps1).
* Uložit tento příkaz nastavit jako runbook služby Azure Automation v hello **účty Automation** části hello portálu Azure.

## <a id="examples"></a>Příklady
Tady jsou dva příklady použití hello postup uvedený výš toobuild prostředí Azure PowerShell příkaz sad, které vytvoření systému Windows Azure virtuálních počítačů.

### <a name="example-1"></a>Příklad 1
Potřebuji prostředí PowerShell příkaz nastavit toocreate hello počáteční virtuálního počítače pro řadič domény služby Active Directory, který:

* Využívá image systému Windows Server 2012 R2 Datacenter hello.
* Obsahuje název hello AZDC1.
* Je samostatný počítač.
* Má k dispozici další data disk 20 GB.
* Má hello statickou IP adresu 192.168.244.4.
* Je v podsíti back-end hello hello AZDatacenter virtuální sítě.
* Je v cloudové službě Azure-Válcovny hello.

Zde je hello odpovídající prostředí Azure PowerShell příkaz set toocreate tento virtuální počítač s prázdné řádky mezi každého bloku čitelnější.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### <a name="example-2"></a>Příklad 2
Potřebuji prostředí PowerShell sady příkazů toocreate virtuálního počítače pro server obchodní který:

* Využívá image systému Windows Server 2012 R2 Datacenter hello.
* Obsahuje název hello LOB1.
* Je členem domény corp.contoso.com hello.
* Má k dispozici další data disk 200 GB.
* Je v podsíti front-endu hello hello AZDatacenter virtuální sítě.
* Je v cloudové službě Azure-Válcovny hello.

Zde je hello odpovídající prostředí Azure PowerShell příkaz set toocreate tohoto virtuálního počítače.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="next-steps"></a>Další kroky
Pokud budete potřebovat disk s operačním systémem, která je větší než 127 GB, můžete [rozbalte hello operačního systému disku](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

