# <a name="azure-managed-disks-overview"></a>Přehled Azure spravované disky

Disky systému Azure spravované zjednodušuje Správa disku pro virtuální počítače Azure IaaS pomocí správy hello [účty úložiště](../articles/storage/common/storage-introduction.md) přidružené hello disky virtuálních počítačů. Můžete mít pouze typ hello toospecify ([Premium](../articles/storage/common/storage-premium-storage.md) nebo [standardní](../articles/storage/common/storage-standard-storage.md)) a hello velikost disku je nutné, a Azure vytváří a spravuje hello disku za vás.

## <a name="benefits-of-managed-disks"></a>Výhody spravovaných disků

Podívejme se na některé z výhod hello získáte pomocí spravovaného disky, počínaje toto video Channel 9 [lepší odolnost virtuální počítač Azure s spravované disky](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency).
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency/player]

### <a name="simple-and-scalable-vm-deployment"></a>Jednoduché a škálovatelné nasazení virtuálního počítače.

Spravované obslužné rutiny úložiště disky pro vás pozadí hello. Dříve bylo toocreate úložiště účtů toohold hello disky (soubory VHD) pro virtuální počítače Azure. Při rozšiřování prostředků, bylo toomake se, že jste vytvořili další účty úložiště, nebylo překročíte hello limit IOPS pro úložiště s žádným disky. S spravované disky zpracování úložiště, můžete se už není omezené podle hello limity účtu úložiště (například 20 000 IOPS / účet). Také již máte toocopy účtů úložiště toomultiple vlastních bitových kopií (soubory VHD). Můžete spravovat v centrálním umístění – jeden účet úložiště podle oblasti Azure – a použít je toocreate stovky virtuálních počítačů v předplatném.

Spravované disky vám umožní toocreate až too10 000 virtuálních počítačů **disky** v odběru, který vám umožní toocreate tisíc z **virtuální počítače** v rámci jednoho předplatného. Tato funkce také další zvyšuje škálovatelnost hello [virtuální počítač škálování sady (VMSS)](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) tím, že toocreate až tooa tisíc virtuálních počítačů v VMSS, pomocí image pořízenou prostřednictvím Marketplace.

### <a name="better-reliability-for-availability-sets"></a>Vyšší spolehlivost pro skupiny dostupnosti

Spravované disky zajišťuje vyšší spolehlivost pro skupiny dostupnosti zajištěním této hello disky [virtuální počítače ve skupině dostupnosti](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) jsou dostatečně izolované od sebe navzájem tooavoid jediný bod selhání. Dělá to tak, že automaticky hello disky jednotek škálování jiného úložiště (razítka). Pokud se razítko (stamp) se nezdaří z důvodu selhání toohardware nebo softwaru, hello instancí virtuálního počítače jenom s disky na těchto razítka se nezdaří. Předpokládejme například že máte aplikace běžící na virtuálních počítačích pět a hello virtuální počítače jsou ve skupině dostupnosti. Hello disky pro tyto virtuální počítače všechny neuloží do hello stejné razítka, takže pokud jedno razítko přestane fungovat, hello ostatní instance aplikace hello dál toorun.

### <a name="highly-durable-and-available"></a>Vysoká odolnost a dostupnost

Disky Azure jsou navržené pro 99,999% dostupnost. REST snadnější zároveň budete vědět, že máte tři repliky vaše data, která umožňuje vysokou odolnost. Pokud jedna nebo dvě i repliky dochází k problémům, zbývající repliky hello zajištění trvalosti dat a vysokou toleranci proti selhání. Tato architektura pomohla Azure soustavně zajišťovat odolnost pro disky IaaS na podnikové úrovni, se špičkovou NULOVOU roční chybovostí. 

### <a name="granular-access-control"></a>Řízení přístupu granulární

Můžete použít [řízení řízení přístupu (RBAC)](../articles/active-directory/role-based-access-control-what-is.md) tooassign určitá oprávnění pro tooone spravovaných disků nebo víc uživatelů. Spravované disky zpřístupňuje a různé operace, včetně čtení, zápisu (vytvořit nebo aktualizovat), odstranění a načítání [sdílený přístupový podpis (SAS) URI](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) hello disku. Můžete udělit přístup tooonly hello operace, že uživatel musí tooperform jeho úlohy. Například pokud nechcete, aby osoba toocopy účet spravovaných disků na tooa úložiště, můžete není toogrant přístup toohello export akce pro tohoto spravovaného disku. Podobně, pokud nechcete, aby osoba toouse toocopy identifikátor URI pro SAS se spravovaným diskem, můžete vybrat není toogrant tohoto oprávnění toohello spravovaného disku.

### <a name="azure-backup-service-support"></a>Podporu služby Azure Backup
Služba Azure Backup pomocí spravované disky toocreate úlohu zálohování s zálohy založené na čase, snadno obnovení virtuálních počítačů a zásady uchovávání záloh. Spravované disky podporují pouze místně redundantní úložiště (LRS) jako možnost replikace hello; To znamená, že udržuje tři kopie dat hello v jedné oblasti. Místní zotavení po havárii, je nutné zálohovat vaše disky virtuálních počítačů v jiné oblasti pomocí [služba Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md) a účet úložiště GRS jako úložiště záloh. Aktuálně Azure Backup podporuje velikosti disku dat si too1TB pro zálohování. Další informace o to v [služby pomocí zálohování Azure pro virtuální počítače s spravované disky](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="pricing-and-billing"></a>Ceny a fakturace

Při používání spravované disky, platí následující aspekty fakturace hello:
* Typ úložiště

* Velikost disku

* Počet transakcí

* Přenosy odchozích dat

* Spravovaný Disk snímků (kopie celého disku)

Podívejme bližší pohled na tyto.

**Typ úložiště:** spravované disky nabízí 2 úrovně výkonu: [Premium](../articles/storage/common/storage-premium-storage.md) (založené na jednotku SSD) a [standardní](../articles/storage/common/storage-standard-storage.md) (založené na HDD). fakturace Hello spravovaného disku závisí na typu úložiště, kterou jste vybrali pro hello disk.


**Kapacita disku**: fakturace pro spravované disky závisí na velikosti hello zřízený hello disku. Azure mapy hello zřízené velikost (zaokrouhlený nahoru) toohello nejbližší možnost spravovat disky uvedeného v následujících tabulkách hello. Každý tooone mapy spravovaných disků na dobrý den podporované zřízené velikosti a se fakturuje odpovídajícím způsobem. Například pokud vytvoříte standardní se spravovaným diskem a zadejte velikost zřízeného 200 GB, můžete se účtují podle hello ceny z hello typ S20 disku.

Zde jsou k dispozici pro premium se spravovaným diskem hello velikosti disků:

| **Premium spravované <br>typ disku** | **P4** | **P6** |**P10** | **P20** | **P30** | **P40** | **P50** | 
|------------------|---------|---------|---------|---------|----------------|----------------|----------------|  
| Velikost disku        | 32 GB   | 64 GB   | 128 GB  | 512 GB  | 1024 GB (1 TB) | 2 048 GB (2 TB) | 4095 GB (4 TB) | 


Zde jsou k dispozici pro standardní se spravovaným diskem hello velikosti disků:

| **Standardní spravované <br>typ disku** | **S4** | **S6** | **S10** | **S20** | **ÚROVNĚ S30** | **S40** | **S50** |
|------------------|---------|---------|--------|--------|----------------|----------------|----------------| 
| Velikost disku        | 32 GB   | 64 GB   | 128 GB | 512 GB | 1024 GB (1 TB) | 2 048 GB (2 TB) | 4095 GB (4 TB) | 


**Počet transakcí**: vám fakturují hello počet transakcí, které můžete provádět na standardní spravovaného disku. Neexistuje žádné náklady pro transakce pro premium se spravovaným diskem.

**Odchozí přenosy dat**: [odchozí přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/) (data přejdete mimo datových center Azure) jsou zpoplatněné podle využití šířky pásma.

Podrobné informace o cenách pro spravované disky, najdete v části [spravované ceny disků](https://azure.microsoft.com/pricing/details/managed-disks).


## <a name="managed-disk-snapshots"></a>Spravovaný Disk úrovně snímky

Spravované snímek je jen pro čtení úplnou kopii spravovaným diskem, který je uložený jako standardní spravovaných disků ve výchozím nastavení. Snímky můžete zálohovat vaše spravované disky v libovolném bodě v čase. Tyto snímky existují nezávisle na hello zdrojový disk a může být použité toocreate nové disky spravované. Budou se účtují podle velikosti hello používá. Například pokud vytvoříte snímek se spravovaným diskem s zřízená kapacita 64 GB a skutečná data použité velikosti 10 GB, bude účtován snímku pouze pro velikost dat hello používá 10 GB.  

[Přírůstkové snímky](../articles/virtual-machines/windows/incremental-snapshots.md) nejsou aktuálně podporovány pro spravované disky, ale bude podporovaný v budoucnu hello.

toolearn Další informace o tom, jak toocreate snímky s spravované disky, podrobnosti naleznete v těchto prostředků:

* [Vytvoření kopie VHD uložené jako spravovaný disk pomocí snímků ve Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Vytvoření kopie VHD uložené jako spravovaný disk pomocí snímků v Linuxu](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)


## <a name="images"></a>Image

Spravované disky také podporují vytváření spravované vlastní image. Můžete vytvořit bitovou kopii z vašeho vlastního virtuálního pevného disku v účtu úložiště, nebo přímo z zobecněný virtuální počítač (sys prepped). To zaznamená do jediného image všechny spravované disků přidružených virtuálních počítačů, včetně hello operačního systému a datové disky. To umožňuje vytváření stovky virtuálních počítačů pomocí vlastní image bez hello potřebovat toocopy nebo spravovat všechny účty úložiště.

Informace o vytváření bitové kopie najdete v článku hello následující články:
* [Jak toocapture spravované obrázek zobecněný virtuální počítač v Azure](../articles/virtual-machines/windows/capture-image-resource.md)
* [Jak toogeneralize a zachycení Linux virtuálního počítače pomocí hello 2.0 rozhraní příkazového řádku Azure](../articles/virtual-machines/linux/capture-image.md)

## <a name="images-versus-snapshots"></a>Bitové kopie a snímky

Zobrazí často hello slovo "image" používalo s virtuálními počítači a nyní se zobrazí "snímky" také. Je důležité toounderstand hello rozdíl mezi tyto. S spravované disky můžete využít image zobecněný virtuální počítač, který bylo zrušeno. Tento image bude obsahovat všechny disky připojené toohello hello virtuálních počítačů. Můžete použít tuto bitovou kopii toocreate nový virtuální počítač, a bude zahrnovat všechny disky hello.

Snímek je kopii disku v hello bodu v čase, kdy je odebrán. Platí jenom tooone disku. Pokud máte virtuální počítač, který má jenom jeden disk (hello operačního systému), můžete provést snímek nebo bitovou kopii je a vytvoření virtuálního počítače ze snímku hello nebo hello image.

Co když virtuální počítač má pět disků a že jsou rozdělené? Můžete zachytit snímek jednotlivých disků hello, ale neexistuje žádné sledování v rámci hello virtuální počítač stav hello hello disků – snímky hello pouze vědět o jeden disk. V takovém případě hello snímky potřebovat toobe koordinované mezi sebou a který se aktuálně nepodporuje.

## <a name="managed-disks-and-encryption"></a>Spravované disky a šifrování

Existují dva druhy toodiscuss šifrování disků toomanaged odkaz. Hello nejprve jeden je šifrování služby úložiště (SSE), která se provádí služba hello úložiště. Hello druhá je Azure Disk Encryption, která můžete povolit na hello operačního systému a datové disky pro virtuální počítače.

### <a name="storage-service-encryption-sse"></a>Šifrování služby úložiště (SSE)

[Azure šifrování služby úložiště](../articles/storage/common/storage-service-encryption.md) poskytuje šifrování na rest a chrání vaše data toomeet organizační závazky zabezpečení a dodržování předpisů. SSE je ve výchozím nastavení povolena pro všechny spravované disky, snímky a obrázků ve všech oblastech hello, kde je k dispozici spravované disky. Od 10. června 2017, všechny nové spravované disky, snímky nebo obrázky a nová data tooexisting spravované disky, jsou automaticky šifrovat na rest klíče spravované microsoftem.  Navštivte hello [stránka spravované nejčastější dotazy disky](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption) další podrobnosti.


### <a name="azure-disk-encryption-ade"></a>Azure Disk Encryption (ADE)

Azure Disk Encryption umožňuje tooencrypt hello operačního systému a datové disky použít podle IaaS virtuálního počítače. To zahrnuje spravovaných disky. Pro systém Windows jsou šifrované jednotky hello pomocí standardních technologií šifrování nástroje BitLocker. Pro systémy Linux jsou disky hello šifrované pomocí hello DM-Crypt technologie. Toto je integrovaná s Azure Key Vault tooallow jste toocontrol a správu hello disku šifrovacích klíčů. Další informace najdete v tématu [Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux](../articles/security/azure-security-disk-encryption.md).

## <a name="next-steps"></a>Další kroky

Další informace o discích spravovaných naleznete toohello následující články.

### <a name="get-started-with-managed-disks"></a>Začínáme se spravovanými disky

* [Vytvoření virtuálního počítače pomocí Resource Manageru a PowerShellu](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md)

* [Vytvoření virtuálního počítače s Linuxem pomocí hello 2.0 rozhraní příkazového řádku Azure](../articles/virtual-machines/linux/quick-create-cli.md)

* [Připojit disk tooa spravovaných dat virtuálního počítače s Windows pomocí prostředí PowerShell](../articles/virtual-machines/windows/attach-disk-ps.md)

* [Přidat tooa spravovaných disků na virtuální počítač s Linuxem](../articles/virtual-machines/linux/add-disk.md)

* [Spravované disky prostředí PowerShell ukázkové skripty](https://github.com/Azure-Samples/managed-disks-powershell-getting-started)

* [Používat spravované disky v šablonách Azure Resource Manager](../articles/virtual-machines/windows/using-managed-disks-template-deployments.md)

### <a name="compare-managed-disks-storage-options"></a>Porovnání možností úložiště spravované disky

* [Storage úrovně Premium a disků](../articles/storage/common/storage-premium-storage.md)

* [Standardní úložiště a disků](../articles/storage/common/storage-standard-storage.md)

### <a name="operational-guidance"></a>Provozní pokyny

* [Migrace z AWS a jiné platformy tooManaged disky v Azure](../articles/virtual-machines/windows/on-prem-to-azure.md)

* [Převeďte virtuální počítače Azure toomanaged disky v Azure](../articles/virtual-machines/windows/migrate-to-managed-disks.md)
