# <a name="common-errors-during-classic-tooazure-resource-manager-migration"></a>Běžné chyby během tooAzure Classic při migraci správce prostředků
Tento článek katalogů hello většiny běžných chyb a jejich zmírnění během migrace hello prostředků IaaS z zásobník Azure Resource Manager toohello modelu nasazení Azure classic.

## <a name="list-of-errors"></a>Seznam chyb
| Text chyby | Omezení rizik |
| --- | --- |
| Vnitřní chyba serveru |V některých případech se jedná o přechodnou chybu, která zmizí při opakování pokusu. Pokud pokračuje toopersist, [požádejte podporu Azure](../articles/azure-supportability/how-to-create-azure-support-request.md) jako potřebuje šetření platformy protokolů. <br><br> **Poznámka:** po hello incidentu sledována tým podpory hello, prosím nepokoušejte všechny vlastní zmírnění jako to může mít nežádoucích důsledků ve vašem prostředí. |
| Migrace se u nasazení {název_nasazení} v hostované službě {název_hostované_služby} nepodporuje, protože jde o nasazení PaaS (webová role/role pracovního procesu). |K tomu dochází v případě, že nasazení obsahuje webovou roli nebo roli pracovního procesu. Vzhledem k tomu, že migrace je podporovaná jenom pro virtuální počítače, odeberte z nasazení hello hello role web nebo worker a zkuste migrovat znovu. |
| Nasazení šablony {název_šablony} selhalo. CorrelationId={guid} |V hello back-end služby migrace použijeme prostředky toocreate šablony Azure Resource Manager v zásobníku hello Azure Resource Manager. Vzhledem k tomu, že šablony jsou idempotent, obvykle můžete bezpečně znovu hello migrace operaci tooget po této chybě. Pokud tato chyba přetrvává toopersist, [požádejte podporu Azure](../articles/azure-supportability/how-to-create-azure-support-request.md) a řekněte jim hello CorrelationId. <br><br> **Poznámka:** po hello incidentu sledována tým podpory hello, prosím nepokoušejte všechny vlastní zmírnění jako to může mít nežádoucích důsledků ve vašem prostředí. |
| Hello virtuální sítě {-název virtuální sítě-} neexistuje. |To může nastat, když jste vytvořili hello virtuální sítě v hello nový portál Azure. se následující hello Hello skutečné virtuální síť s názvem "skupina * <VNET name>" |
| Virtuální počítač {název_virtuálního_počítače} v hostované službě {název_hostované_služby} obsahuje rozšíření {název_rozšíření}, které se v Azure Resource Manageru nepodporuje. Je doporučeno toouninstall z hello virtuální počítač před pokračováním migrace. |Rozšíření XML, jako je například BGInfo 1.*, nejsou podporována v Azure Resource Manageru. Proto tato rozšíření není možné migrovat. Pokud jsou tato rozšíření levém hello nainstalovat do virtuálního počítače, jsou automaticky odinstalovat před dokončením migrace hello. |
| Virtuální počítač {název_virtuálního_počítače} v hostované službě {název_hostované_služby} obsahuje rozšíření VMSnapshot/VMSnapshotLinux, jehož migrace se aktuálně nepodporuje. Odinstalovat z hello virtuálních počítačů a znovu přidat pomocí Azure Resource Manager po hello migrace je dokončen. |Toto je hello scénář, kde hello virtuální počítač je nakonfigurovaný pro zálohování Azure. Vzhledem k tomu, že aktuálně se jedná o nepodporovaný scénář, postupujte podle hello řešení v https://aka.ms/vmbackupmigration |
| Virtuální počítač {název virtuálního počítače} v hostované službě {hostované service-name} obsahuje rozšíření {název rozšíření}, jejichž stav není nehlásí z hello virtuálních počítačů. Virtuální počítač proto není možné migrovat. Zajistěte, aby byl že tento stav rozšíření hello je hlášena nebo odinstalovat rozšíření hello z hello virtuálního počítače a zkuste migraci opakovat. <br><br> Virtuální počítač {název_virtuálního_počítače} v hostované službě {název_hostované_služby} obsahuje rozšíření {název_rozšíření}, které hlásí stav obslužné rutiny: {stav_obslužné_rutiny}. Proto se nedají migrovat hello virtuálních počítačů. Zajistěte, aby byl stav obslužné rutiny rozšíření hello nehlásí {obslužná rutina status} nebo ho odinstalovat z hello virtuálního počítače a opakujte migraci. <br><br> Agent virtuálního počítače pro virtuální počítač {název virtuálního počítače} v hostované službě {hostované service-name} je generování sestav hello celkový stav agenta jako Nepřipravená. Proto hello virtuálních počítačů nemusí být migrovali, pokud má příponu migrovat. Zkontrolujte, zda že tento hello agenta virtuálního počítače je generování sestav celkového stavu agenta jako připravené. Naleznete toohttps://aka.ms/classiciaasmigrationfaqs. |Agent hosta Azure & rozšíření virtuálního počítače nutné odchozí internetové přístup toohello virtuálních počítačů úložiště účet toopopulate jejich stav. Mezi běžné příčiny chyby stavu patří: <li> Skupinu zabezpečení sítě, které blokuje odchozí přístup toohello Internetu <li> Pokud hello virtuální sítě má místní servery DNS, se ztratí připojení DNS <br><br> Pokud budete pokračovat toosee nepodporovaný stav, můžete odinstalovat rozšíření tooskip hello této kontroly a přejít pomocí nástroje migrace. |
| Migrace se u nasazení {název_nasazení} v hostované službě {název_hostované_služby} nepodporuje, protože má více skupin dostupnosti. |Aktuálně je možné migrovat pouze hostované služby s 1 nebo žádnou skupinou dostupnosti. toowork tento problém vyřešit, přesuňte hello další sady dostupnosti a virtuální počítače v těchto sad dostupnosti tooa jiné hostované služby. |
| Migrace není podporována pro nasazení {název nasazení} v hostované službě {hostované service-name protože obsahuje virtuální počítače, které nejsou součástí hello skupiny dostupnosti, i když hello HostedService obsahuje jeden. |alternativní řešení Hello pro tento scénář je tooeither přesuňte všechny virtuální počítače hello do jednoho dostupnosti nastavit nebo odebrat všechny virtuální počítače z hello sady dostupnosti v hello hostované služby. |
| Úložiště účet nebo hostované službě nebo virtuální síť {-název virtuální sítě-} se proces hello se migruje a proto nejde změnit |K této chybě dojde, když dokončení hello operace migrace "Přípravy" hello prostředku a operace, která by prostředku toohello změnu být se aktivuje. Z důvodu hello zámku v rovině řízení hello po operaci "Příprava" jsou zablokované prostředků toohello žádné změny. toounlock hello správu rovině, můžete spustit hello "Potvrzení" migrace operaci toocomplete migrace nebo hello operace "Přípravy" hello "Zrušení" migrace operaci tooroll zpět. |
| Migrace se pro hostovanou službu {název_hostované_služby} nepovoluje, protože má virtuální počítač {název_virtuálního_počítače} ve stavu: Neznámý stav role. Migrace je povolen, pouze když hello virtuální počítač je v jednom z následujících hello stavy - spuštěn, zastaven, zastavena navrácena. |Hello virtuálních počítačů může probíhat přes přechod stavu, což obvykle se stane, když během operace aktualizace na hello HostedService například restartování, instalace rozšíření atd. Doporučujeme pro toocomplete operace aktualizace hello na hello HostedService než to zkusíte migrace. |
| {Název nasazení} nasazení v hostované službě {hostované service-name} obsahuje virtuální počítač {název virtuálního počítače} s datový Disk {dat název disku}, jehož velikost {size-of-the-vhd-blob-backing-the-data-disk} bajtů fyzického blob neodpovídá {logické velikosti hello datový Disk virtuálního počítače size-of-the-data-disk-specified-in-the-VM-API} bajtů. Migrace bude pokračovat bez určení velikosti pro hello datový disk pro virtuální počítač Azure Resource Manager hello. | Tato chyba se stane, když jste změnit objekt blob VHD hello bez aktualizace hello velikost v modelu hello API virtuálních počítačů. Podrobné kroky pro zmírnění rizika jsou uvedené [níže](#vm-with-data-disk-whose-physical-blob-size-bytes-does-not-match-the-vm-data-disk-logical-size-bytes).|
| Došlo k výjimce úložiště při ověřování datového disku {název_datového_disku} s odkazem na médium {identifikátor_URI_datového_disku} pro virtuální počítač {název_virtuálního_počítače} v cloudové službě {název_cloudové_služby}. Ujistěte se, že tento odkaz na médium virtuálního pevného disku hello je přístupný pro tento virtuální počítač | K této chybě může dojít, pokud hello disky hello virtuálního počítače byla odstraněna nebo už nejsou dostupné. Zkontrolujte, zda text hello disky pro hello virtuálních počítačů existují.|
| Virtuální počítač {název_virtuálního_počítače} v hostované službě {název_hostované_služby} obsahuje disk s odkazem na médium {identifikátor_URI_VHD} s názvem objektu blob {název_objektu_blob_VHD}, který se v Azure Resource Manageru nepodporuje. | K této chybě dojde, když má hello název objektu hello blob "/" v něm, který není aktuálně podporovaný v výpočetní zprostředkovateli prostředků. |
| Migrace není povolená pro nasazení {název nasazení} v hostované službě {název cloudové služby}, protože není v oboru regionální hello. Pro přesun tento obor tooregional nasazení naleznete toohttp://aka.ms/regionalscope. | V 2014 Azure oznámila, že síťové prostředky přesune z oboru tooregional clusteru úrovni oboru. Další informace najdete na adrese [http://aka.ms/regionalscope](http://aka.ms/regionalscope). K této chybě dojde, když hello nasazení migrovaného neproběhla operaci aktualizace, které automaticky ji přesune tooa místní rozsah. Nejlepším řešením je tooeither přidat tooa koncový bod virtuálního počítače nebo datový disk toohello virtuálních počítačů a opakujte migraci. <br> V tématu [jak tooset až koncových bodů na klasické virtuální počítač Windows v Azure](../articles/virtual-machines/windows/classic/setup-endpoints.md#create-an-endpoint) nebo [připojit datový disk tooa virtuálního počítače s Windows vytvořené pomocí modelu nasazení classic hello](../articles/virtual-machines/windows/classic/attach-disk.md)|


## <a name="detailed-mitigations"></a>Podrobné způsoby zmírnění rizik

### <a name="vm-with-data-disk-whose-physical-blob-size-bytes-does-not-match-hello-vm-data-disk-logical-size-bytes"></a>Virtuální počítač s datový Disk, jejichž fyzického blob velikost v bajtech neodpovídá bajtů logické velikosti hello datový Disk virtuálního počítače.

To se stane, když hello logické velikost datového disku můžete získat synchronizován s hello skutečná velikost objektu blob virtuálního pevného disku. To lze snadno ověřit pomocí hello následující příkazy:

#### <a name="verifying-hello-issue"></a>Ověření hello problém

```PowerShell
# Store hello VM details in hello VM object
$vm = Get-AzureVM -ServiceName $servicename -Name $vmname

# Display hello data disk properties
# NOTE hello data disk LogicalDiskSizeInGB below which is 11GB. Also note hello MediaLink Uri of hello VHD blob as we'll use this in hello next step
$vm.VM.DataVirtualHardDisks


HostCaching         : None
DiskLabel           : 
DiskName            : coreosvm-coreosvm-0-201611230636240687
Lun                 : 0
LogicalDiskSizeInGB : 11
MediaLink           : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
SourceMediaLink     : 
IOType              : Standard
ExtensionData       : 

# Now get hello properties of hello blob backing hello data disk above
# NOTE hello size of hello blob is about 15 GB which is different from LogicalDiskSizeInGB above
$blob = Get-AzureStorageblob -Blob "coreosvm-dd1.vhd" -Container vhds 

$blob

ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudPageBlob
BlobType          : PageBlob
Length            : 16106127872
ContentType       : application/octet-stream
LastModified      : 11/23/2016 7:16:22 AM +00:00
SnapshotTime      : 
ContinuationToken : 
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext
Name              : coreosvm-dd1.vhd
```

#### <a name="mitigating-hello-issue"></a>Zmírnění hello problém

```PowerShell
# Convert hello blob size in bytes tooGB into a variable which we'll use later
$newSize = [int]($blob.Length / 1GB)

# See hello calculated size in GB
$newSize

15

# Store hello disk name of hello data disk as we'll use this tooidentify hello disk toobe updated
$diskName = $vm.VM.DataVirtualHardDisks[0].DiskName

# Identify hello LUN of hello data disk tooremove
$lunToRemove = $vm.VM.DataVirtualHardDisks[0].Lun

# Now remove hello data disk from hello VM so that hello disk isn't leased by hello VM and it's size can be updated
Remove-AzureDataDisk -LUN $lunToRemove -VM $vm | Update-AzureVm -Name $vmname -ServiceName $servicename

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureVM       213xx1-b44b-1v6n-23gg-591f2a13cd16   Succeeded  

# Verify we have hello right disk that's going toobe updated
Get-AzureDisk -DiskName $diskName

AffinityGroup        : 
AttachedTo           : 
IsCorrupted          : False
Label                : 
Location             : East US
DiskSizeInGB         : 11
MediaLink            : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
DiskName             : coreosvm-coreosvm-0-201611230636240687
SourceImageName      : 
OS                   : 
IOType               : Standard
OperationDescription : Get-AzureDisk
OperationId          : 0c56a2b7-a325-123b-7043-74c27d5a61fd
OperationStatus      : Succeeded

# Now update hello disk toohello new size
Update-AzureDisk -DiskName $diskName -ResizedSizeInGB $newSize -Label $diskName

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureDisk     cv134b65-1b6n-8908-abuo-ce9e395ac3e7 Succeeded 

# Now verify that hello "DiskSizeInGB" property of hello disk matches hello size of hello blob 
Get-AzureDisk -DiskName $diskName


AffinityGroup        : 
AttachedTo           : 
IsCorrupted          : False
Label                : coreosvm-coreosvm-0-201611230636240687
Location             : East US
DiskSizeInGB         : 15
MediaLink            : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
DiskName             : coreosvm-coreosvm-0-201611230636240687
SourceImageName      : 
OS                   : 
IOType               : Standard
OperationDescription : Get-AzureDisk
OperationId          : 1v53bde5-cv56-5621-9078-16b9c8a0bad2
OperationStatus      : Succeeded

# Now we'll add hello disk back toohello VM as a data disk. First we need tooget an updated VM object
$vm = Get-AzureVM -ServiceName $servicename -Name $vmname

Add-AzureDataDisk -Import -DiskName $diskName -LUN 0 -VM $vm -HostCaching ReadWrite | Update-AzureVm -Name $vmname -ServiceName $servicename

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureVM       b0ad3d4c-4v68-45vb-xxc1-134fd010d0f8 Succeeded      
```

### <a name="moving-a-vm-tooa-different-subscription-after-completing-migration"></a>Přesunutí virtuálního počítače tooa jiného předplatného po dokončení migrace

Po dokončení procesu migrace hello můžete toomove hello virtuálních počítačů tooanother předplatné. Ale pokud máte tajný klíč nebo certifikát na hello přesunout virtuální počítač, který odkazuje na prostředek, Key Vault, hello není aktuálně podporováno. Hello níže pokyny vám umožní tooworkaround hello problém. 

#### <a name="powershell"></a>PowerShell
```powershell
$vm = Get-AzureRmVM -ResourceGroupName "MyRG" -Name "MyVM"
Remove-AzureRmVMSecret -VM $vm
Update-AzureRmVM -ResourceGroupName "MyRG" -VM $vm
```
#### <a name="azure-cli-20"></a>Azure CLI 2.0

```bash
az vm update -g "myrg" -n "myvm" --set osProfile.Secrets=[]
```
