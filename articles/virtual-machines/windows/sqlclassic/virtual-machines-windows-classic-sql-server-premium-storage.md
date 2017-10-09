---
title: aaaUse Azure Premium Storage s SQL serverem | Microsoft Docs
description: "Tento článek používá prostředky, které jsou vytvořené pomocí modelu nasazení classic hello a poskytuje pokyny k používání Azure Premium Storage s SQL Server běžící na virtuálních počítačích Azure."
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 7ccf99d7-7cce-4e3d-bbab-21b751ab0e88
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/01/2017
ms.author: jroth
ms.openlocfilehash: 393ea2020b39ea686302ae632e1049935c24af00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Použití Azure Premium Storage s SQL Serverem na virtuálních počítačích
## <a name="overview"></a>Přehled
[Azure Premium Storage](../../../storage/common/storage-premium-storage.md) je hello nové generace úložiště, které zajišťuje nízkou latencí a vysokou propustnost vstupně-výstupní operace. Je nejvhodnější pro klíče vstupně-výstupní operace náročné úlohy, jako je SQL Server na IaaS [virtuální počítače](https://azure.microsoft.com/services/virtual-machines/).

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Tento článek obsahuje plánování a pokyny k migraci virtuálního počítače se systémem SQL Server toouse Storage úrovně Premium. To zahrnuje infrastrukturu Azure (sítě, úložiště) a hostovaný virtuální počítač s Windows kroky. Příklad Hello v hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) ukazuje migraci za úplné komplexní end tooend jak toomove větší virtuální počítače tootake výhod vylepšené místní úložiště SSD pomocí prostředí PowerShell.

Je důležité toounderstand hello začátku do konce proces využitím Azure Premium Storage s SQL serverem na virtuální počítače IAAS. To zahrnuje následující:

* Identifikace hello požadavky toouse Storage úrovně Premium.
* Příklady nasazení systému SQL Server na IaaS tooPremium úložiště pro nová nasazení.
* Příklady migraci existujícího nasazení samostatných serverů a nasazení pomocí SQL skupin dostupnosti Always On.
* Migrace možných přístupů.
* Příklad úplného začátku do konce zobrazující Azure, Windows a SQL Server kroky pro migraci hello stávající implementaci Always On.

Další informace pozadí na serveru SQL Server v Azure Virtual Machines najdete v tématu [systému SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

**Autor:** ADAM Sol **odborní recenzenti:** Leoš Carlos Vargas sleďů, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Předpoklady pro Storage úrovně Premium
Existuje několik předpokladů pro použití služby Premium Storage.

### <a name="machine-size"></a>Velikost počítače
Pro použití služby Premium Storage budete potřebovat toouse DS řady virtuální počítače (VM). Pokud jste nepoužili řady DS počítače v cloudové službě před, musíte odstranit hello existující virtuální počítač, zachovat hello připojené disky a pak vytvořte novou cloudovou službu před opětovným vytvořením hello virtuální počítač jako velikost role DS *. Další informace o velikosti virtuálních počítačů najdete v tématu [virtuálního počítače a Cloud velikost služeb pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="cloud-services"></a>Cloud Services
Při jejich vytváření v novou cloudovou službu, můžete použít pouze virtuální počítače DS * Storage úrovně Premium. Pokud používáte SQL serveru Always On v Azure, bude odkazovat hello vždy na naslouchací proces adresu toohello Azure interní nebo externí IP nástroje pro vyrovnávání zatížení, která je přidružená ke cloudové službě. Tento článek se zaměřuje na toomigrate při zachování dostupnosti v tomto scénáři.

> [!NOTE]
> Musí být první virtuální počítač, který je nasazený toohello hello řady DS * novou Cloudovou službu.
>
>

### <a name="regional-vnets"></a>Virtuální místní sítě
Pro virtuální počítače DS * je nutné nakonfigurovat hello virtuální síť (VNET) hostování vaší místní toobe virtuálních počítačů. Tento "rozšiřuje" hello virtuální sítě je tooallow hello větší virtuální počítače toobe zřízené v jiné clustery a povolit komunikaci mezi nimi. V hello následující snímek obrazovky hello zvýrazněná umístění ukazuje regionální virtuální sítě, zatímco první výsledek hello ukazuje "úzké" virtuální sítě.

![RegionalVNET][1]

Můžete zvýšit tooa toomigrate lístek podpory Microsoft oblastní virtuální síť, Microsoft bude změňte, pak toocomplete hello migrace tooregional virtuálních sítí, změňte vlastnost hello AffinityGroup v konfiguraci sítě hello. Nejprve vyexportovat hello konfigurace sítě v prostředí PowerShell a potom můžete nahradit hello **AffinityGroup** vlastnost hello **VirtualNetworkSite** element s **umístění** Vlastnost. Zadejte `Location = XXXX` kde `XXXX` je oblast Azure. Pak importujte hello novou konfiguraci.

Například zvažování hello následující konfiguraci virtuální sítě:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

toomove tento tooa oblastní virtuální síť v oblasti západní Evropa, změnit toohello konfigurace hello následující:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Účty úložiště
Budete potřebovat toocreate nový účet úložiště, který je nakonfigurován pro Storage úrovně Premium. Všimněte si, že použití hello úložiště Premium Storage je nastavený na účet úložiště hello, nikoli na jednotlivé virtuální pevné disky, ale při použití Virtuálního řady DS * z účtů Premium a Standard Storage můžete připojit virtuální pevný disk na. To může zvažte, pokud nechcete, aby tooplace hello virtuálního pevného disku operačního systému na toohello účet služby Premium Storage.

Následující Hello **New-AzureStorageAccountPowerShell** s hello "Premium_LRS" **typ** vytvoří účet úložiště Premium:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Nastavení mezipaměti virtuálních pevných disků
Hello hlavní rozdíl mezi vytváření disků, které jsou součástí účtu úložiště Premium je nastavení mezipaměti hello disku. Pro svazek dat systému SQL Server disků se doporučuje použít '**ukládání do mezipaměti pro čtení**'. Pro svazky protokolu transakcí, hello disku mezipaměti nastavení by mělo být příliš '**žádné**'. To se liší od hello doporučení pro účty úložiště Standard Storage.

Jakmile byly připojeny hello virtuální pevné disky, nelze změnit nastavení mezipaměti hello. By třeba toodetach a připojte hello virtuální pevný disk s nastavením aktualizované mezipaměti.

### <a name="windows-storage-spaces"></a>Prostory úložiště ve Windows
Můžete použít [prostory úložiště ve Windows](https://technet.microsoft.com/library/hh831739.aspx) jako u předchozí standardního úložiště, bude možné toomigrate virtuální počítač, který je již využívá prostory úložiště. Příklad Hello v [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (krok 9 a předat dál) ukazuje hello tooextract kódu prostředí Powershell a import virtuálního počítače s více připojených virtuálních pevných disků.

Fondy úložiště používaly s propustností tooenhance účet úložiště Standard Azure a snižování latence. Je možné, hodnota při testování fondy úložiště Storage úrovně Premium pro nová nasazení, ale zvyšují zvýšenou složitostí při instalaci úložiště.

#### <a name="how-toofind-which-azure-virtual-disks-map-toostorage-pools"></a>Jak toofind které virtuální disky Azure mapování toostorage fondy
Jako existují různé mezipaměti nastavení doporučení pro virtuální pevné disky připojené, můžete se rozhodnout toocopy hello virtuální pevné disky tooa účet služby Premium Storage. Pokud jste je připojte toohello novou DS řadu virtuálních počítačů, však může být zapotřebí nastavení mezipaměti tooalter hello. Je jednodušší tooapply hello Storage úrovně Premium doporučené nastavení mezipaměti, pokud máte samostatný virtuální pevné disky pro hello dat SQL soubory a protokolu souborů (spíše než jeden virtuální pevný disk obsahující oba).

> [!NOTE]
> Pokud máte soubory protokolu a data systému SQL Server na hello stejný svazek, hello ukládání do mezipaměti možnost, kterou zvolíte, závisí na hello vzory přístupu k vstupně-výstupní operace pro zatížení databáze. Pouze testování můžete ukazují, které možnost ukládání do mezipaměti je nejvhodnější pro tento scénář.
>
>

Pokud používáte prostory úložiště systému Windows, které se skládají z více virtuálních pevných disků, budete potřebovat toolook na vaše původní tooidentify skripty, které připojit virtuální pevné disky jsou však v jaké konkrétní fondu, takže pak můžete nastavit nastavení mezipaměti hello odpovídajícím způsobem pro každý disk.

Pokud nemáte původní skript k dispozici tooshow jste, které virtuální pevné disky mapování toohello fondu úložiště, můžete použít následující kroky toodetermine hello disku/úložiště fondu mapování hello.

Pro každý disk použijte hello následující kroky:

1. Získání seznamu disky připojené tooVM s hello **Get-AzureVM** příkaz:

    Get-AzureVM - ServiceName <servicename> -název <vmname> | Get-AzureDataDisk
2. Poznámka: hello Diskname a logické jednotky.

    ![DisknameAndLUN][2]
3. Vzdálená plocha do hello virtuálních počítačů. Potom přejděte příliš**Správa počítače** | **Správce zařízení** | **diskové jednotky**. Podívejte se na vlastnosti hello jednotlivých hello Microsoft virtuálních disků

    ![VirtualDiskProperties][3]
4. Zde Hello číslo logické jednotky je referenční toohello číslo logické jednotky, které můžete zadat při připojování toohello hello virtuálního pevného disku virtuálního počítače.
5. Hello Microsoft virtuální Disk najdete toohello **podrobnosti** kartě, pak v hello **vlastnost** seznamu, přejděte příliš**klíč ovladače**. V hello **hodnotu**, Poznámka hello **posun**, což je 0002 v hello následující snímek obrazovky. Hello 0002 označuje hello PhysicalDisk2 hello odkazy fondu úložiště.

    ![VirtualDiskPropertyDetails][4]
6. Pro každý fond úložiště výpisu se hello přidružené disky:

    Get-StoragePool - FriendlyName AMS1pooldata | Get-PhysicalDisk

    ![GetStoragePool][5]

Nyní můžete pomocí této informace tooassociate připojit virtuální pevné disky tooPhysical disků ve fondu úložiště.

Jakmile jsou namapovány disky tooPhysical virtuální pevné disky ve fondech úložiště, můžete odpojit a zkopírujte je přes tooa účet služby Premium Storage, připojte je s mezipamětí správné hello nastavení. Podrobnosti najdete příklad hello v hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), kroky 8 až 12. Tyto kroky ukazují, jak tooextract soubor virtuálního pevného disku virtuálního počítače připojeného disku konfigurace tooa CSV kopírování hello virtuální pevné disky, změnit nastavení mezipaměti konfigurace disku hello a nakonec znovu nasaďte hello virtuálního počítače jako řady DS virtuálních počítačů s všechny hello připojenými disky.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>Šířka pásma úložiště virtuálních počítačů a propustnost úložiště virtuálního pevného disku
Hello množství výkon úložiště závisí na hello zadaná velikost virtuálního počítače DS * a hello velikosti virtuálního pevného disku. virtuální počítače Hello mají různé příspěvky pro hello počet virtuálních pevných disků, které lze připojit a hello maximální šířka pásma se podporují (MB/s). Čísla hello konkrétní šířky pásma, najdete v části [virtuálního počítače a Cloud velikost služeb pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Při větší velikosti disků jsou dosáhnout vyšší IOPS. Byste měli zvážit při úvahách o vaši cestu migrace. Podrobnosti najdete [IOPS a typech disků najdete v části hello tabulky](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).

Nakonec zvažte, že virtuální počítače mají jiný disk maximální šířek pásma, které se podporují pro všechny disky připojené. Vysoké zátěži může saturate hello disku maximální šířka pásma pro velikost role tohoto virtuálního počítače. Například Standard_DS14 bude podporovat až too512MB/s; Proto může s tři disky P30 saturate hello disku šířka pásma hello virtuálních počítačů. Ale v tomto příkladu může být překročen limit hello propustnost v závislosti na hello směs ke čtení a zápisu IOs.

## <a name="new-deployments"></a>Nová nasazení
Hello následujících dvou částech ukazují, jak můžete nasadit virtuální počítače serveru SQL tooPremium úložiště. Jak je uvedeno nahoře, není nutné nutně tooplace hello disk operačního systému do úložiště úrovně Premium. Můžete zvolit, toodo to pokud jste se záměrné tooplace žádné zatížení s intenzivním vstupně-výstupních operací na hello virtuálního pevného disku operačního systému.

Hello první příklad ukazuje, využívá existující obrázky z Galerie Azure. Hello druhý příklad ukazuje, jak toouse vlastní virtuální bitové kopie, kterou máte ve stávající účet úložiště Standard storage.

> [!NOTE]
> Tyto příklady předpokládejme, že jste již vytvořili oblastní virtuální sítě.
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Vytvořit nový virtuální počítač s Storage úrovně Premium s bitovou kopií Galerie
Následující příklad Hello ukazuje, jak tooplace hello virtuálního pevného disku operačního systému do úložiště úrovně premium a připojte virtuální pevné disky úložiště Premium. Můžete však také umístit hello disk operačního systému v standardní účet úložiště a pak připojte virtuální pevné disky, které jsou umístěny v účtu Storage úrovně Premium. Je ukázán oba scénáře.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Krok 1: Vytvoření účtu služby Storage úrovně Premium
    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Krok 2: Vytvořte novou Cloudovou službu
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Krok 3: Rezervovat VIP cloudové služby (volitelné)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Krok 4: Vytvořte kontejner virtuálních počítačů
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Krok 5: Umístění virtuálního pevného disku operačního systému na Standard nebo Premium Storage
    #NOTE: Set up subscription and default storage account which will be used tooplace hello OS VHD in

    #If you want tooplace hello OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted tooplace hello OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Krok 6: Vytvoření virtuálního počítače
    #Get list of available SQL Server Images from hello Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember toochange tooDS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks tooVM Config
    #Note hello size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising hello Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-toouse-premium-storage-with-a-custom-image"></a>Vytvoření nového virtuálního počítače toouse Premium Storage pomocí vlastní image
Tento scénář předvádí, kdy máte existující přizpůsobené bitové kopie, které jsou umístěny ve standardní účet úložiště. Jak je uvedeno, pokud chcete tooplace hello virtuálního pevného disku operačního systému na Storage úrovně Premium, budete potřebovat toocopy hello bitovou kopii, která existuje v hello standardní účet úložiště a je před použitím přenos tooa Storage úrovně Premium. Pokud máte bitovou kopii na místě, můžete také použít tuto metodu toocopy přímo toohello účet služby Premium Storage.

#### <a name="step-1-create-storage-account"></a>Krok 1: Vytvoření účtu úložiště
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Krok 2 Vytvoření cloudové služby
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Krok 3: Použití existující bitová kopie
Můžete použít stávající image. Nebo můžete [trvat image existujícího počítače](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Poznámka: hello počítači, na kterém je obrázek nemá toobe DS * počítače. Jakmile máte hello image, hello následující kroky zobrazit jak toocopy ho toohello účet úložiště Premium se hello **Start-AzureStorageBlobCopy** prostředí PowerShell..

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for hello storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Krok 4: Kopírování objektu Blob mezi účty úložiště
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Krok 5: Pravidelné kontroly stavu kopie:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-tooazure-disk-repository-in-subscription"></a>Krok 6: Přidejte bitové kopie disku tooAzure úložiště v předplatném
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [!NOTE]
> Můžete zjistit, že i když jako úspěšné sestavy stavu hello, může stále dostanete zapůjčení chyba disku. V takovém případě Počkejte přibližně 10 minut.
>
>

#### <a name="step-7--build-hello-vm"></a>Krok 7: Vytvoření hello virtuálních počítačů
Tady jsou sestavování hello virtuálního počítače z image a připojení dvě disků VHD úložiště Premium:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need toobe a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use tooDS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Existující nasazení, které nepoužívají skupin dostupnosti Always On
> [!NOTE]
> Existující nasazení, nejprve najdete v části hello [požadavky](#prerequisites-for-premium-storage) části tohoto tématu.
>
>

Existují různé aspekty pro nasazení systému SQL Server, které nepoužívají skupin dostupnosti Always On a ty, které provádějí. Pokud nepoužíváte Always On a máte existující samostatný systém SQL Server, můžete upgradovat tooPremium úložiště pomocí nového účtu služby a úložiště cloudu. Vezměte v úvahu hello následující možnosti:

* **Vytvoření nového virtuálního počítače SQL serveru**. Můžete vytvořit nový virtuální počítač SQL Server, který používá účet úložiště Premium, jak je uvedeno v nová nasazení. Potom zálohování a obnovení konfigurace a uživatele databáze SQL Server. Hello aplikace bude nutné aktualizovat toobe tooreference hello nový Server SQL, pokud je právě přístupu interně nebo externě. Potřebovali byste toocopy všechny objekty 'z databáze, jako když jste dělali migrace systému SQL Server (SxS) vedle sebe. To zahrnuje objekty, jako je například přihlášení, certifikáty a propojené servery.
* **Migrovat existující virtuální počítač serveru SQL**. To bude vyžadovat převedení hello virtuální počítač SQL Server do režimu offline a pak ho přenosu tooa novou cloudovou službu, která zahrnuje kopírování všech jeho připojené virtuální pevné disky toohello účet služby Premium Storage. Při přechodu do režimu online hello virtuálních počítačů, bude aplikace hello odkazovat hello název serveru hostitele jako před. Upozorňujeme, že velikost hello stávající disk hello ovlivní hello výkonové charakteristiky. Například 400 GB místa na disku získá zaokrouhlit tooa P20. Pokud víte, že výkon disku nevyžadují, může znovu vytvořte virtuální počítač jako virtuální počítač řady DS hello a připojte virtuální pevné disky úložiště Premium hello specifikace velikosti a výkonu, kterou požadujete. Pak můžete odpojit a připojte hello soubory databáze SQL.

> [!NOTE]
> Při kopírování disků VHD hello byste měli vědět o velikosti hello, v závislosti na velikosti hello bude znamenat, jaký typ disku úložiště Premium se spadají do, tato hodnota určuje specifikace výkon disku. Velikost Azure se zaokrouhlí nahoru toohello nejbližší disku, takže pokud máte 400 GB místa na disku, to bude zaokrouhlený nahoru tooa P20. V závislosti na vaší stávající vstupně-výstupní požadavky hello virtuálního pevného disku operačního systému nemusí potřebovat toomigrate tento tooa účet služby Premium Storage.
>
>

Pokud je externě přístup k systému SQL Server, se změní hello cloudové služby virtuálních IP adres. Bude také nutné tooupdate koncové body, seznamy řízení přístupu a DNS nastavení.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Existující nasazení, které používají skupiny dostupnosti Always On
> [!NOTE]
> Existující nasazení, nejprve najdete v části hello [požadavky](#prerequisites-for-premium-storage) části tohoto tématu.
>
>

Nejprve v této části se podíváme na jak Always On komunikuje s sítí Azure. Jsme se potom rozčlenit v tootwo scénáře migrace: migrace, kde musí dosáhnout minimální dobou výpadku a migrace, kde lze tolerovat výpadky.

Použít místní SQL Server skupin dostupnosti Always On naslouchací proces místní které zaregistruje virtuální název DNS společně s IP adresu, která jsou sdílena mezi jeden nebo více serverů SQL. Pokud se klienti připojují směrování v hello naslouchací proces IP toohello primární systému SQL Server. Toto je hello serveru, který je vlastníkem hello prostředek vždy na IP v daném čase.

![DeploymentsUseAlways na][6]

Ve službě Microsoft Azure může mít pouze jednu adresu přiřazenou tooa IP síťový adaptér na hello virtuálních počítačů, takže v pořadí tooachieve hello stejnou úroveň abstrakce jako místní, Azure využívá hello IP adresu, která je přiřazena toohello interní/externí služby Vyrovnávání zatížení (ILB/ELB). Zdroj Hello IP, jež jsou sdílena mezi servery hello je nastaven toohello stejnou IP Adresou jako hello ILB/ELB. To je publikován v hello DNS a přenosy klientů je předána hello ILB/ELB toohello primární SQL serveru repliky. Hello ILB/ELB ví, což SQL Server je primární, protože používá sondy tooprobe hello vždy na IP prostředků. V předchozím příkladu hello ho sondy každý uzel, který má koncový bod odkazuje hello ELB/ILB, podle toho, která odpovídá je hello primární systému SQL Server.

> [!NOTE]
> Hello ILB a ELB obě přiřazené tooa konkrétní cloudové služby Azure, proto žádné migrace do cloudu v Azure s největší pravděpodobností znamená, že hello, které změní IP nástroje pro vyrovnávání zatížení.
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migrace vždy na nasazení, která můžete povolit výpadky
Existují dvě strategie toomigrate Always On nasazení, které umožňují výpadky:

1. **Přidat další tooan sekundární repliky existující vždy na clusteru**
2. **Migrace tooa nového vždy na clusteru**

#### <a name="1-add-more-secondary-replicas-tooan-existing-always-on-cluster"></a>1. Přidat další tooan sekundární repliky existující vždy na Cluster
Jednou z možných strategií je tooadd více sekundárních toohello vždy na skupiny dostupnosti. Potřebujete tooadd tyto do novou cloudovou službu a aktualizovat hello naslouchací proces s hello nových IP nástroje pro vyrovnávání zatížení.

##### <a name="points-of-downtime"></a>Body výpadek:
* Ověření clusteru.
* Testování vždy na převzetí služeb při selhání pro novou sekundární repliky.

Pokud používáte fondy úložišť systému Windows v rámci hello virtuálních počítačů pro vyšší propustnost vstupně-výstupní operace, pak tyto režimu budou během úplného ověření clusteru. Při přidání uzlů toohello clusteru je potřeba Hello ověřovací test. Doba trvání testu hello toorun Hello se může měnit, měli byste otestovat to ve vaší reprezentativního testovacího prostředí tooget Přibližná doba o tom, jak dlouho to bude trvat.

By měl zřídit čas, kde můžete provádět ruční převzetí služeb při selhání a chaos testování v hello nově přidali uzly tooensure vždy na vysokou dostupnost funkcí podle očekávání.

![DeploymentUseAlways On2][7]

> [!NOTE]
> Všechny instance systému SQL Server, kdy se používá fondy úložiště hello před spuštěním technologie hello ověření by se měla zastavit.
>
> ##### <a name="high-level-steps"></a>Postup vysoké úrovně
>

1. Vytvoření dvou nových serverů SQL v novou cloudovou službu s připojeným úložištěm Premium.
2. Zkopírujte přes úplné zálohování a obnovení pomocí **NORECOVERY**.
3. Zkopírujte přes "mimo uživatelem DB" závislé objekty, jako je například přihlášení atd.
4. Vytvořit nový nové vyrovnávání pro interní zatížení (ILB) nebo použijte externí zatížení vyrovnávání (ELB) a potom nastavit koncové body skupinu s vyrovnáváním zatížení v obou uzlech nového.

   > [!NOTE]
   > Zkontrolujte, jestli že všechny uzly mají hello správné konfigurace koncového bodu, než budete pokračovat
   >
   >
5. Zastavte toohello přístup uživatele nebo aplikace SQL Server (Pokud používáte fondy úložišť).
6. Zastavte služby stroje SQL serveru na všech uzlech (Pokud používáte fondy úložišť).
7. Přidejte nové uzly toocluster a spuštění úplného ověření.
8. Po úspěšném ověření spusťte všechny služby SQL Server.
9. Protokoly transakcí zálohování a obnovení uživatelských databází.
10. Přidání nových uzlů do hello vždy na skupiny dostupnosti a umístěte replikace do **Synchronous**.
11. Přidat hello IP adresu prostředek hello nové cloudové služby ILB/ELB pomocí prostředí PowerShell pro Always On podle příkladu více lokalit hello hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage). V systému vytváření clusterů systému Windows, nastavte hello **Možní vlastníci** z hello **IP adresu** prostředků toohello nové uzly starý. V části hello 'Přidání prostředek IP adresy ve stejné podsíti, s hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
12. Převzetí služeb při selhání tooone hello nové uzly.
13. Proveďte hello nové uzly partnery automatické převzetí služeb při selhání a testovací převzetí služeb při selhání.
14. Odeberte původní uzly ze skupiny dostupnosti.

##### <a name="advantages"></a>Výhody
* Nové servery SQL Server může být testována (SQL Server a aplikace) před přidáním tooAlways na.
* Můžete změnit velikost virtuálního počítače hello a přizpůsobit přesných požadavcích tooyour hello úložiště. Nicméně, je výhodné tookeep všechny cesty k souborům SQL hello hello stejné.
* Můžete ovládat, když jsou spuštěny hello přenos hello DB zálohy toohello sekundární repliky. Tím se liší od použití Azure **Start-AzureStorageBlobCopy** příkaz toocopy virtuální pevné disky, protože se jedná o asynchronní kopírování.

##### <a name="disadvantages"></a>Nevýhody
* Při použití fondů úložišť systému Windows, je během hello úplného ověření clusteru pro hello nové další uzly clusteru výpadku.
* V závislosti na hello verze serveru SQL a hello existující počet sekundárních replikách, pravděpodobně není být schopný tooadd více sekundárních replik bez odebrání stávající sekundární repliky.
* Může dojít při nastavování hello sekundárních dlouhá doba přenosu dat SQL.
* Není další náklady během migrace, když máte nové počítače, které běží paralelně.

#### <a name="2-migrate-tooa-new-always-on-cluster"></a>2. Migrace tooa nového vždy na clusteru
Další strategie je toocreate zcela nový vždy na Cluster s uzly, které zcela nový novou cloudovou službu, a potom přesměrování hello klientům toouse ho.

##### <a name="points-of-downtime"></a>Body výpadku
Při přenosu aplikace a uživatelé toohello nové Always On naslouchací proces dojde k výpadku. výpadek Hello závisí na:

* Hello doba toorestore konečné transakce protokolu zálohování toodatabases na nové servery.
* Hello doba tooupdate klienta aplikace toouse nové Always On naslouchací proces.

##### <a name="advantages"></a>Výhody
* Můžete otestovat hello skutečné produkční prostředí, SQL Server, a změny sestavení operačního systému.
* Máte hello možnost toocustomize hello úložiště a toopotentially zmenšit velikost virtuálního počítače. To může mít za následek snížení nákladů.
* Během tohoto procesu můžete aktualizovat sestavení systému SQL Server nebo verze. Rovněž lze upgradovat hello operačního systému.
* Hello předchozí vždy na clusteru může fungovat jako cíl plnou vrácení zpět.

##### <a name="disadvantages"></a>Nevýhody
* Název DNS hello toochange hello naslouchacího procesu budete potřebovat, pokud chcete, aby obě Always On clustery se systémem současně. Tento postup přidá správy režii během migrace hello jako řetězců aplikace klienta musí odrážet hello nový název naslouchacího procesu.
* Je nutné implementovat synchronizační mechanismus mezi dvěma tookeep prostředí hello je jako zavřete jako možné toominimize hello konečná synchronizace požadavky před migrací.
* Během migrace se se přidá náklady na době, kdy máte hello nové prostředí spuštěna.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migrace vždy na nasazení pro minimální dobou výpadku
Pro minimální dobou výpadku existují dvě strategie migrace nasazení Always On:

1. **Využívat stávající sekundární: jedné lokalitě**
2. **Využívat stávající sekundární následující: více lokalit**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. Využívat stávající sekundární: jedné lokalitě
Jednou z možných strategií pro minimální dobou výpadku je tootake ve stávající sekundární cloudu a odebere ji z hello aktuální cloudové služby. Pak zkopírujte hello virtuální pevné disky toohello nový účet úložiště Premium a vytvořte hello virtuálních počítačů v hello novou cloudovou službu. Aktualizujte hello naslouchací proces ve clustering a převzetí služeb při selhání.

##### <a name="points-of-downtime"></a>Body výpadku
* Při aktualizaci hello poslední uzel s koncovým bodem s vyrovnáváním zatížení se hello dojde k výpadku.
* Vaše opětovné připojení klienta může být zpoždění v závislosti na konfiguraci klienta a DNS.
* Pokud se rozhodnete tootake hello vždy na clusteru skupiny offline tooswap out hello IP adresy je další výpadku. To se můžete vyhnout pomocí nebo závislostí a možní vlastníci pro hello přidat prostředek IP adresy. V části hello 'Přidání prostředek IP adresy ve stejné podsíti, s hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).

> [!NOTE]
> Pokud chcete hello toopartake přidané uzlu v jako vždy na převzetí služeb při selhání Partner, je potřeba tooadd koncový bod Azure s odkaz toohello, skupinu s vyrovnáváním zatížení nastavit. Když spustíte hello **přidat AzureEndpoint** příkaz toodo se aktuální tooremain připojení otevřené, ale nové naslouchání toohello připojení nebude možné toobe navázat, dokud nástroj pro vyrovnávání zatížení hello byl aktualizován. Při testování šlo zaznamenané toolast 90-120seconds, by měla být testována.
>
>

##### <a name="advantages"></a>Výhody
* Žádné další náklady vzniklé během migrace.
* 1: 1 migrace.
* Menší složitost.
* Umožňuje vyšší IOPS z SKU úložiště Premium. Pokud hello disky odpojit od hello virtuální počítač a zkopírovat toohello novou cloudovou službu 3. stran nástroj může být velikost virtuálního pevného disku hello použité tooincrease, která poskytuje vyšší propustnost. Zvýšení velikosti virtuálního pevného disku, najdete [diskuzi na fóru](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Nevýhody
* Při migraci dojde k dočasné ztrátě HA a zotavení po Havárii.
* Protože se jedná o migraci 1:1, budete mít toouse minimální velikost virtuálního počítače, která bude podporovat počtu virtuální pevné disky, takže nemusí být možné toodownsize virtuální počítače.
* Tento scénář využije hello Azure **Start-AzureStorageBlobCopy** příkaz, který je asynchronní. Neexistuje žádná smlouva SLA na dokončení kopírování. čas Hello kopií hello se liší, když to závisí na čekání ve frontě, bude také záviset na hello množství dat tootransfer. Čas kopie Hello zvyšuje, pokud přenos hello budete tooanother Azure datového centra, který podporuje službu Premium Storage v jiné oblasti. Pokud máte právě 2 uzly, vezměte v úvahu možné snížení rizika v případě, že kopírování hello trvá déle než při testování. To může zahrnovat následující návrhy hello.
  * Přidáte dočasný uzel systému SQL Server 3. pro HA před migrací hello odsouhlaseného výpadku.
  * Spusťte migraci hello mimo Azure plánované údržby.
  * Zkontrolujte, zda že jste správně nakonfigurovali vaší kvorum clusteru.  

##### <a name="high-level-steps"></a>Postup vysoké úrovně
Tento dokument nepředvádí v příkladu tooend dokončení end, ale hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) poskytuje podrobné informace, které mohou být využity tooperform to.

![MinimalDowntime][8]

* Konfigurace disku shromáždění a odebrat hello uzlu (neodstraňujte připojené virtuální pevné disky).
* Vytvořit účet úložiště Premium a zkopírovat virtuální pevné disky z hello standardní účet úložiště
* Vytvořte novou cloudovou službu a znovu nasaďte hello SQL2 virtuálních počítačů v rámci této cloudové služby. Vytvoření hello virtuálních počítačů pomocí hello zkopíruje původní virtuální pevný disk operačního systému a připojení hello zkopírují virtuální pevné disky.
* Konfigurace ILB / ELB a přidat koncové body.
* Buď aktualizujte naslouchací proces:
  * Pořízení hello vždy na skupiny offline a aktualizuje hello vždy na naslouchací proces s novou ILB / ELB IP adres.
  * Nebo přidání hello IP adresu prostředku z nové cloudové služby ILB/ELB pomocí prostředí PowerShell do clusteringu Windows. Sada hello Možní vlastníci hello IP adresu prostředku toohello pak migrovat uzlu SQL2 a nastavit jako závislosti nebo v hello název sítě. V části hello 'Přidání prostředek IP adresy ve stejné podsíti, s hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
* Zkontrolujte klienty toohello konfigurace/šíření DNS.
* Migrujte virtuální počítač SQL1 a projít kroky 2 – 4.
* Pokud používáte 5ii kroky, přidejte počítač SQL1 jako možného vlastníka pro hello přidat prostředek IP adresy
* Testovací převzetí služeb při selhání.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. Využívat stávající sekundární následující: Multi-Site
Pokud máte uzly ve více než jeden datové centrum Azure (DC) nebo pokud máte hybridní prostředí, můžete použít konfigurace vždy na tento výpadek toominimize prostředí.

Hello přístup je toochange hello Always On synchronizace tooSynchronous hello místní nebo sekundární řadič domény Azure a převzetí služeb při selhání přes toothat systému SQL Server. Pak zkopírujte hello virtuální pevné disky tooa účet služby Premium Storage a znovu nasadit počítač hello do novou cloudovou službu. Aktualizujte hello naslouchací proces a pak navrácení služeb po obnovení.

##### <a name="points-of-downtime"></a>Body výpadku
výpadek Hello se skládá z hello čas toofailover toohello alternativní řadič domény a zpět. Také závisí na konfiguraci klienta a DNS a může být zpoždění vaší opětovné připojení klienta.
Vezměte v úvahu následující ukázka hybridní Always On konfigurace hello:

![MultiSite1][9]

##### <a name="advantages"></a>Výhody
* Můžete využít stávající infrastrukturu.
* Nejprve máte hello možnost toopre upgrade hello úložiště Azure na hello řadič domény Azure zotavení po Havárii.
* můžete třeba překonfigurovat Hello úložiště Azure pro zotavení po Havárii řadiče domény.
* Během migrace, s výjimkou testovací převzetí služeb při selhání není k dispozici minimálně dvě převzetí služeb při selhání.
* Není nutné toomove dat systému SQL Server pomocí zálohování a obnovení.

##### <a name="disadvantages"></a>Nevýhody
* V závislosti na klienta tooSQL přístup k serveru může být zvýší latence při systém SQL Server běží v aplikaci toohello alternativní řadič domény.
* čas Hello kopírování virtuálních pevných disků tooPremium úložiště může trvat dlouho. Může to mít vliv na rozhodnutí o tom, jestli tookeep hello uzlu v hello skupiny dostupnosti. Zvažte proto pro při protokolu náročné pracovním zatížením běží během migrace hello je nutné, protože hello primárním uzlu, který bude mít tookeep hello nereplikovaných transakce v protokolu transakcí. Proto to může výrazně zvýší.
* Tento scénář využije hello Azure **Start-AzureStorageBlobCopy** příkaz, který je asynchronní. Neexistuje žádná smlouva SLA na dokončení. Hello čas kopií hello se liší, když to závisí na čekání ve frontě, bude také záviset na hello množství dat tootransfer. Proto máte jenom jeden uzel v 2. datové centrum, můžete provést kroky zmírňující rizika v případě, že kopírování hello trvá déle než při testování. To může zahrnovat následující návrhy hello.
  * Přidáte dočasný uzel SQL 2. pro HA před migrací hello odsouhlaseného výpadku.
  * Spusťte migraci hello mimo Azure plánované údržby.
  * Zkontrolujte, zda že jste správně nakonfigurovali vaší kvorum clusteru.

Tento scénář předpokládá, že můžete mít zdokumentovaný instalaci vaší a vědět, jak je v pořadí toomake změny pro disk optimální nastavení mezipaměti namapované hello úložiště.

##### <a name="high-level-steps"></a>Postup vysoké úrovně
![Multisite2][10]

* Ujistěte se, hello místní / alternativní řadič domény Azure hello primární Server SQL a nastavte jej hello jiné automatické převzetí služeb při selhání partnera (AFP).
* Shromážděte informace o konfiguraci disku z SQL2 a odebrat hello uzlu (neodstraňujte připojené virtuální pevné disky).
* Vytvořit účet úložiště Premium a zkopírovat virtuální pevné disky z hello standardní účet úložiště.
* Vytvořte novou cloudovou službu a vytvořte hello SQL2 virtuálních počítačů s jeho úložiště prémií disky připojené.
* Konfigurace ILB / ELB a přidat koncové body.
* Aktualizace hello vždy na naslouchací proces s novou ILB / ELB IP adresu a testovací převzetí služeb při selhání.
* Zkontrolujte konfiguraci DNS hello.
* Změnit hello AFP tooSQL2 migrovat SQL1 a projít kroky 2 až 5.
* Testovací převzetí služeb při selhání.
* Přepnout zpět tooSQL1 hello AFP a SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-toopremium-storage"></a>Dodatek: Migrace tooPremium nasazení ve více lokalitách vždy na clusteru úložiště
Hello zbývající část tohoto tématu poskytuje podrobný příklad převodu více lokalit Always On tooPremium úložiště clusteru. Převede také hello naslouchací proces pomocí k externí nástroje pro vyrovnávání (ELB) tooan interní služby load pro vyrovnávání zatížení (ILB).

### <a name="environment"></a>Prostředí
* Windows 2k 12 / SQL 2k 12
* 1 DB soubory na SP
* 2 x fondy úložiště na každý uzel

![Appendix1][11]

### <a name="vm"></a>VIRTUÁLNÍ POČÍTAČ:
V tomto příkladu přidáme toodemonstrate přechod ze ELB tooILB. Režim Manageout nebylo k dispozici před ILB, takže to ukazuje, jak hello tooswitch toothis během migrace.

![Appendix2][12]

### <a name="pre-steps-connect-toosubscription"></a>Předběžné kroky: Připojit tooSubscription
    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Krok 1: Vytvořte nový účet úložiště a cloudové služby
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where hello vm toomigrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-hello-permitted-failures-on-resources-optional"></a>Krok 2: Zvýšení hello povolená selhání na prostředky<Optional>
Na některé prostředky, které patří tooyour vždy na skupiny dostupnosti neexistují omezení na tom, kolik chyby, ke kterým může dojít v době, kdy hello Clusterová služba se pokusí skupiny prostředků toorestart hello. Doporučuje se, že to zvýšíte a přitom jste se s návodem tento postup, vzhledem k tomu, že pokud to neuděláte ruční převzetí služeb při selhání a aktivační události převzetí služeb při selhání pomocí vypínání počítačů můžete získat zavřít toothis limit.

Ho by být vhodné toodouble hello selhání příspěvek, toodo to ve Správci clusteru převzetí služeb při selhání, přejděte toohello vlastnosti hello Always On skupiny zdrojů:

![Appendix3][13]

Změňte hello too6 maximální počet selhání.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Krok 3: Přidání IP adresu prostředku pro skupinu clusteru<Optional>
Pokud máte pouze jednu IP adresu pro hello skupina clusteru a toto je zarovnaný toohello cloudu podsíť, mějte na paměti, pokud jste omylem převést do režimu offline všechny uzly clusteru v cloudu hello síť pak prostředek hello IP clusteru a název sítě s clustery nebude možné toocome online. Události tohoto, které zabrání v hello aktualizuje tooother prostředky clusteru.

#### <a name="step-4-dns-configuration"></a>Krok 4: Konfigurace serveru DNS
tooimplement s hladkým přechodem závisí na tom, jak se DNS využité a aktualizovat.
Když Always On je nainstalován, vytvoří skupinu prostředků clusteru systému Windows, pokud otevřete Správce clusteru převzetí služeb při selhání, uvidíte, že minimálně bude mít tři zdroje, hello dva, které hello dokumentu odkazuje tooare:

* Název virtuální sítě (VNN) – Toto je hello DNS název tohoto klienta připojit toowhen podmínka mimo tooconnect tooSQL servery prostřednictvím Always On.
* Prostředek IP adresy – je hello IP adresu, kterou přidružené hello VNN, můžete mít více než jednu a v konfiguraci ve více lokalitách bude mít za lokality a podsítě IP adresu.

Při připojování tooSQL Server hello ovladač klienta systému SQL Server bude načítat záznamy DNS hello přidružený naslouchací proces hello a zkuste tooconnect tooeach Always On přidružené IP adresu, níže probereme některé faktory, které mohou mít vliv na to.

Hello počet souběžných záznamy DNS, které jsou přidruženy název naslouchacího procesu hello závisí nejen na hello počet IP adresám, ale hello ' RegisterAllIpProviders'setting clusteringu pro převzetí služeb při selhání pro hello Always ON VNN prostředků.

Když nasadíte Always On v Azure existují různé kroky toocreate hello naslouchací proces a IP adresy, máte toomanually konfigurace too1 "RegisterAllIpProviders" hello, toto je jiný tooan místní vždy na nasazení, kde je již nastaven too1.

Pokud 'RegisterAllIpProviders' 0, zobrazí se jenom jeden záznam DNS ve službě DNS přidružené hello naslouchací proces:

![Appendix4][14]

Je-li 'RegisterAllIpProviders' 1:

![Appendix5][15]

Následující kód Hello bude dump out hello VNN nastavení a nastavení za vás, prosím Poznámka pro hello změnit efekt tootake budete potřebovat tootake hello VNN offline a zapněte ji zpět do režimu online, tento pořízení hello naslouchací proces offline což způsobilo klientských připojení k přerušení.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

V pozdější fázi migrace budete potřebovat tooupdate hello Always On naslouchací proces s aktualizovanou adresou IP, která bude odkazovat na službu Vyrovnávání zatížení, to bude zahrnovat odebrání prostředků IP adresy a přidání. Po aktualizaci hello IP je třeba tooensure hello novou IP adresu byl aktualizován v zóně DNS a že hello klientům aktualizují své místní mezipaměti DNS.

Pokud vaši klienti nacházejí v různých síťových segmentu a odkazovat na jiný server DNS, je třeba tooconsider, co se stane o přenos zóny DNS v průběhu migrace hello jako hello aplikace znovu připojit čas budou omezené. pomocí alespoň hello čas přenosu zóny všechny nové IP adres pro naslouchání hello. Pokud jste v části času omezení tady, by měl zabývat a otestovat vynucení přenos zóny přírůstkové s týmům Windows a taky hello DNS tooa záznamu hostitele nižší Time (tooLive TTL), takže klienti hello aktualizovat. Další informace najdete v tématu [přírůstkové přenosy zóny](https://technet.microsoft.com/library/cc958973.aspx) a [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Ve výchozím nastavení hello TTL pro DNS záznam, který je přidružen hello naslouchací proces ve Always On v Azure je 1 200 sekund. Můžete tooreduce to pokud jste v rámci omezení čas během migrace tooensure hello klientům aktualizace jejich DNS s hello aktualizovat IP adres pro naslouchání hello. Můžete zobrazit a upravit konfiguraci hello vypsání out hello konfiguraci hello VNN:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Poznámka: hello nižší hello 'HostRecordTTL', dojde k vyšší množství přenosy DNS.

##### <a name="client-application-settings"></a>Nastavení aplikace klienta
Pokud vaše aplikace podporuje klient SQL hello .net 4.5 SQLClient a pak můžete použít "MULTISUBNETFAILOVER = TRUE" – klíčové slovo, tato možnost se doporučuje toobe použít jako během převzetí služeb při selhání umožňuje rychlejší připojení tooSQL vždy na skupiny dostupnosti. Vytvoří výčet prostřednictvím všechny IP adresy přidružené k hello Always On naslouchací proces paralelně a provede agresivnější rychlost opakování připojení TCP při selhání.

Další informace týkající se nastavení hello výše, najdete v tématu [MultiSubnetFailover – klíčové slovo a související funkce](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Viz také [SqlClient podporu pro vysokou dostupnost a zotavení po havárii](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Krok 5: Nastavení kvora clusteru
Jak budete toobe vyřazení alespoň jeden Server SQL dolů v čase, by měl hello nastavení kvora clusteru, upravit, pokud pomocí souboru sdílené složky s kopií clusteru (FSW) s uzly, 2, doporučujeme nastavit Většina uzlů tooallow hello kvora a využívat dynamické hlasování, a to je tooallow pro stálého tooremain jeden uzel.

    Set-ClusterQuorum -NodeMajority  

Další informace o správě a nastavení kvora clusteru hello, najdete v tématu [konfigurace a správa kvora v clusteru převzetí služeb při selhání ve Windows serveru 2012 hello](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Krok 6: Extrahování stávající koncové body a seznamy řízení přístupu
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for hello Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Uložte tyto tooa textového souboru.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Krok 7: Změnit režimy replikace a převzetí služeb při selhání partnery
Pokud máte více než 2 servery SQL Server, by se měl změnit hello převzetí služeb při selhání jinou sekundární v jiné řadiče domény nebo místní too'Synchronous' se bude automatické převzetí služeb při selhání partnera (AFP), to je tak spravovat HA a přitom provádíte změny. Můžete to provést prostřednictvím TSQL systému změnit, když aplikace SSMS:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Krok 8: Odeberte sekundární virtuální počítač z cloudové služby
Byste měli počítat toomigrate sekundární uzel cloudu první, pokud je aktuálně primární, by měl spustíte ruční převzetí služeb při selhání.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns hello disks associated with hello VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Krok 9: Změňte disku ukládání do mezipaměti nastavení v souboru CSV a uložit
Pro datové svazky tyto měli nastavit tooREADONLY.

Pro svazky protokolu tyto měli nastavit tooNONE.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Krok 10: Kopírování virtuálních pevných disků
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Můžete zkontrolovat stav kopírování hello hello virtuální pevné disky toohello účet služby Premium Storage:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Počkejte, dokud všechny tyto se zaznamenávají jako úspěšné.

Informace pro jednotlivé objekty BLOB:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Krok 11: Registrace operačního systému disku
    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Krok 12: Importujte sekundární do novou cloudovou službu
Kód Hello pod také používá hello přidána možnost sem můžete můžete importovat hello počítače a použít hello retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember toochange tooXIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks tooa VM during a deploy tooa new cloud service and new storage account is different from just attaching VHDs toojust a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Krok 13: Vytvoření ILB na nové cloudové Svc zatížit vyrovnáváním koncových bodů a seznamy řízení přístupu
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

#### <a name="step-14-update-always-on"></a>Krok 14: Aktualizace vždy na
    #Code toobe executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # hello azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency tooListener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Nyní odeberte hello staré Cloudová služba IP adresu.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Krok 15: Kontrola aktualizací DNS
Nyní zkontrolujte servery DNS na vaše klientské sítě systému SQL Server a ujistěte se, že clustering přidala hello záznam navíc hostitele pro hello přidat IP adresu. Pokud nebyly aktualizovány tyto servery DNS, zvažte vynucení přenos zóny DNS a zkontrolujte, zda hello klientům v podsíti, že jsou možné tooresolve tooboth vždy na IP adresy, to je proto není nutné toowait pro automatické replikaci DNS.

#### <a name="step-16-reconfigure-always-on"></a>Krok 16: Překonfigurujte vždy na
V tomto okamžiku je počkat hello sekundární tento uzel, který byl migrované toofully Opětovná synchronizace s hello místní uzel a přepněte uzel toosynchronous replikace a nastavit jej jako hello AFP.  

#### <a name="step-17-migrate-second-node"></a>Krok 17: Migrace druhého uzlu
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Krok 18: Změňte disku ukládání do mezipaměti nastavení v souboru CSV a uložit
Pro datové svazky tyto měli nastavit tooREADONLY.

Pro svazky protokolu tyto měli nastavit tooNONE.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Krok 19: Vytvoření nového účtu nezávislé úložiště pro sekundární uzel
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset hello storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Krok 20: Kopírování virtuálních pevných disků
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Pro všechny virtuální pevné disky můžete zkontrolovat stav kopie virtuálního pevného disku hello: příkazu ForEach ($disk v $diskobjects) {$lun = $disk. Logická jednotka $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Jmenovka_disku $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Počkejte, dokud všechny tyto se zaznamenávají jako úspěšné.

Informace pro jednotlivé objekty BLOB:

    #Check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Krok 21: Registrace operačního systému disku
    #change storage account toohello new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join tooexisting Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different toojust a straight cloud service change
    #note if you do not have a disk label hello command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Krok 22: Přidejte zatížení vyrovnáváním koncových bodů a seznamy řízení přístupu
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in hello Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Krok 23: Test převzetí služeb při selhání
Teď by měla umožňují hello migrované uzlu synchronizovat s hello místní vždy na uzlu, umístěte režim replikace toosynchronous a počkejte, dokud je synchronizován. Následně převzetí služeb při selhání z prvního uzlu v místní toohello migrován, což je hello AFP. Jakmile který pracoval, změna hello poslední migrace AFP toohello uzlu.

Doporučujeme testovací převzetí služeb při selhání mezi všemi uzly a spustit v případě chaos testy, které tooensure převzetí služeb při selhání fungovat jako očekáváno a včas manor.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Krok 24: Vrátit zpět nastavení kvora clusteru nebo DNS TTL nebo Pntrs převzetí služeb při selhání nebo nastavení synchronizace
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Přidání prostředku IP adresy ve stejné podsíti
Pokud máte pouze 2 servery SQL a chcete toomigrate je tooa nová Cloudová služba, ale chcete tookeep je na hello stejné podsíti, můžete vyhnout trvá naslouchací proces hello offline toodelete hello vždy původní na IP adresu a přidat hello novou IP adresu. Pokud provádíte migraci hello virtuální počítače tooanother podsíť nebudete potřebovat toodo to jak bude síť další clusteru, který bude odkazovat na této podsíti.

Jakmile budete mít zapínají hello migrovat sekundární a přidán v hello novou IP adresu prostředku pro hello novou cloudovou službu, než existující primární hello převzetí služeb při selhání, můžete provést tyto kroky v rámci hello Správce clusteru převzetí služeb při selhání:

tooadd IP adresu, najdete v části hello [příloha](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), krok 14.

1. Pro hello aktuální prostředek IP adresu, změnit hello možných vlastníků too'Existing primárního serveru SQL', v níže 'dansqlams4' hello příkladu:

    ![Appendix13][23]
2. Pro hello nový prostředek IP adresu, změnit hello možných vlastníků too'Migrated sekundárního serveru SQL', v níže 'dansqlams5' hello příkladu:

    ![Appendix14][24]
3. Toto nastavení můžete převzetí služeb při selhání, a při migraci poslední uzel hello Možní vlastníci hello by měl být upraven tak, že uzel je přidán jako možného vlastníka:

    ![Appendix15][25]

## <a name="additional-resources"></a>Další zdroje
* [Azure Premium Storage](../../../storage/common/storage-premium-storage.md)
* [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/)
* [Systému SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
