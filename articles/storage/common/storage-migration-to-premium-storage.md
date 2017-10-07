---
title: "virtuální počítače tooAzure aaaMigrating Premium Storage | Microsoft Docs"
description: "Migrujte existující virtuální počítače tooAzure Storage úrovně Premium. Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro I náročnými úlohy běžící na virtuálních počítačích Azure."
services: storage
documentationcenter: na
author: yuemlu
manager: tadb
editor: tysonn
ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: yuemlu
ms.openlocfilehash: 19aaf2b7594e570f5a964baa00958a7a8eaae97b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-premium-storage-unmanaged-disks"></a>Migrace tooAzure Storage úrovně Premium (nespravované disků)

> [!NOTE]
> Tento článek popisuje, jak toomigrate virtuální počítač, který používá tooa nespravované standardní disky virtuálního počítače, který používá nespravované prémiové disky. Doporučujeme použít Azure spravované disky pro nové virtuální počítače a převést vaše předchozí toomanaged disky nespravované disky. Disky popisovač hello základní úložiště účty spravované, tak nemusíte. Další informace najdete v tématu naše [přehled disky spravované](../../virtual-machines/windows/managed-disks-overview.md).
>

Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro virtuální počítače spuštěné I náročnými úlohy. Pomocí migrace vaší aplikace virtuálních počítačů disky tooAzure Premium Storage můžete využít výhod hello rychlosti a výkonu z těchto disků.

Hello účel tohoto průvodce je, že noví uživatelé toohelp Azure Premium Storage lepší Příprava toomake plynulý přechod z jejich aktuálního tooPremium systému úložiště. Hello Průvodce řeší tři klíčové komponenty hello v tomto procesu:

* [Plánování migrace tooPremium hello úložiště](#plan-the-migration-to-premium-storage)
* [Příprava a tooPremium kopírování virtuálních pevných disků (VHD) úložiště](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [Vytvoření virtuálního počítače Azure pomocí služby Storage úrovně Premium](#create-azure-virtual-machine-using-premium-storage)

Můžete migrovat virtuální počítače z jiných platforem tooAzure Storage úrovně Premium nebo migrovat existující virtuální počítače Azure z úložiště Standard Storage tooPremium úložiště. Tento průvodce popisuje kroky pro oba dva scénáře. Postupujte podle kroků hello zadaný v příslušné části hello v závislosti na vašem scénáři.

> [!NOTE]
> Najdete přehled funkcí a cen. úložiště Premium Storage v úložiště Premium: [vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md). Doporučujeme migraci všech disku virtuálního počítače vyžadující vysokou IOPS tooAzure Storage úrovně Premium hello nejlepšího výkonu pro vaši aplikaci. Pokud disk nevyžadují vysokou IOPS, můžete omezit náklady na údržbu ve standardní úložiště, která ukládá data disku virtuálního počítače na jednotkách pevných disků (HDD) namísto SSD.
>

Dokončení procesu migrace hello v celé jeho šíři může vyžadovat další akce před i po hello kroků uvedených v této příručce. Mezi příklady patří konfiguraci virtuální sítě nebo koncových bodů nebo provádění změn kódu v rámci aplikace hello, sám sebe, což může vyžadovat výpadky v aplikaci. Tyto akce jsou jedinečné tooeach aplikace a měli byste pokračovat společně s hello kroků uvedených v této příručce toomake hello úplné přechod tooPremium úložiště jako bezproblémové nejblíže.

## <a name="plan-the-migration-to-premium-storage"></a>Plánování migrace tooPremium hello úložiště
Tato část zajistí jsou připravené toofollow hello postupy migrace popsané v tomto článku a vám pomůže toomake hello nejlepší rozhodnutí o typy virtuálního počítače a disku.

### <a name="prerequisites"></a>Požadavky
* Budete potřebovat předplatné Azure. Pokud nemáte, můžete vytvořit jeden měsíc [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/) předplatné nebo navštivte [Azure – ceny](https://azure.microsoft.com/pricing/) další možnosti.
* tooexecute rutiny prostředí PowerShell, je nutné modul Microsoft Azure PowerShell hello. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) hello nainstalovat bod a pokyny k instalaci.
* Pokud máte v plánu toouse Azure virtuálních počítačů spuštěných na Storage úrovně Premium, je potřeba toouse hello podporující virtuálních počítačů služby Storage úrovně Premium. Standard a Premium Storage disků můžete použít s podporou virtuálních počítačů služby Storage úrovně Premium. Disky úložiště Premium je k dispozici s více typy virtuálních počítačů v budoucí hello. Další informace o všech dostupných typů disku virtuálního počítače Azure a velikosti najdete v tématu [velikosti virtuálních počítačů](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [velikosti pro cloudové služby](../../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Požadavky
Virtuální počítač Azure podporuje připojení několik disků úložiště Premium tak, aby vaše aplikace může mít až too256 TB místa na virtuální počítač. Storage úrovně Premium můžete aplikace dosáhnout 80 000 IOPS (vstupně výstupních operací za sekundu) na virtuální počítač a 2000 MB za druhé propustnost disku na virtuální počítač s velmi nízkou latenci pro operace čtení. Máte možností v různých virtuálních počítačů a disků. Tato část se toohelp toofind možnost, která nejlépe vyhovuje vaší zatížení.

#### <a name="vm-sizes"></a>Velikost virtuálních počítačů
Specifikace velikosti virtuálního počítače Azure Hello jsou uvedeny v [velikosti virtuálních počítačů](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Zkontrolujte hello výkonové charakteristiky virtuálních počítačů, které pracovat s Storage úrovně Premium a zvolte hello nejvhodnější velikost virtuálního počítače, který nejlépe vyhovuje vaší zatížení. Ujistěte se, zda je dostatečnou šířku pásma k dispozici na disku provozu hello toodrive virtuálních počítačů.

#### <a name="disk-sizes"></a>Velikost disků
Existuje pět typů disků, které lze použít s vaší virtuálních počítačů a každá z nich má konkrétní IOPs a propustnost omezení. Vzít v úvahu tyto limity, když pro svého virtuálního počítače zvolit typ disku hello podle potřeb hello vaší aplikace z hlediska kapacity, výkon, škálovatelnost a načte ve špičce.

| Disky typu Premium  | P10   | P20   | P30            | P40            | P50            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| Velikost disku           | 128 GB| 512 GB| 1024 GB (1 TB) | 2 048 GB (2 TB) | 4095 GB (4 TB) | 
| Vstupně-výstupní operace za sekundu / disk       | 500   | 2300  | 5000           | 7500           | 7500           | 
| Propustnost / disk | 100 MB za sekundu | 150 MB za sekundu | 200 MB za sekundu | 250 MB za sekundu | 250 MB za sekundu |

V závislosti na velikosti pracovní zátěže určí, jestli dalších datových disků jsou nezbytné pro virtuální počítač. Můžete připojit několik trvalých datových disků tooyour virtuálních počítačů. V případě potřeby můžete rozkládají napříč hello disky tooincrease hello kapacitu a výkon hello svazku. (Zjistíte novinky prokládání disků [zde](storage-premium-storage-performance.md#disk-striping).) Pokud rozkládají Storage úrovně Premium datových disků pomocí [prostory úložiště][4], byste měli nakonfigurovat s jedním sloupcem pro každý disk, který se používá. V opačném hello celkový výkon hello rozdělená svazku může být nižší, než se očekávalo kvůli toouneven distribuce přenosů mezi disky hello. Pro virtuální počítače s Linuxem můžete použít hello *mdadm* tooachieve nástroj hello stejné. Najdete v článku [konfigurace RAID softwaru v systému Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) podrobnosti.

#### <a name="storage-account-scalability-targets"></a>Cíle škálovatelnosti účtu Storage
Prémiové účty úložiště mají hello následující cíle škálovatelnosti v přidání toohello [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md). Pokud vaše požadavky aplikací přesahují hello cíle škálovatelnosti účtu jedno úložiště, sestavení vaší aplikace toouse více účtů úložiště a oddílu data mezi různými tyto účty úložiště.

| Celkový počet účet kapacity | Celková šířka pásma pro účet místně redundantní úložiště |
|:--- |:--- |
| Kapacita disku: 35TB<br />Pořízení snímku kapacity: 10 TB |Až too50 gigabity za sekundu pro příchozí a odchozí |

Pro hello Další informace o specifikacích Premium Storage, podívejte se na [škálovatelnost a cíle výkonnosti při použití služby Premium Storage](storage-premium-storage.md#scalability-and-performance-targets).

#### <a name="disk-caching-policy"></a>Zásady ukládání do mezipaměti na disku
Ve výchozím nastavení, je disk ukládání do mezipaměti zásad *jen pro čtení* pro všechny hello Premium datových disků, a *pro čtení a zápis* pro disk operačního systému Premium hello připojené toohello virtuálních počítačů. Toto nastavení konfigurace se doporučuje tooachieve hello optimální výkon pro aplikace IOs. Těžký zápisu nebo pouze pro zápis datové disky (například soubory protokolu serveru SQL Server) zakažte ukládání do mezipaměti disku, takže můžete dosáhnout lepší výkon aplikace. nastavení mezipaměti Hello pro existující datové disky můžete aktualizovat pomocí [portálu Azure](https://portal.azure.com) nebo hello *- HostCaching* parametr hello *Set-AzureDataDisk* rutiny.

#### <a name="location"></a>Umístění
Vyberte umístění, kde je k dispozici Azure Premium Storage. V tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services) aktuální informace o dostupných umístění. Virtuální počítače umístěné v hello stejné oblasti jako účet úložiště ukládá pro hello virtuálních počítačů získáte mnohem lepší výkon než pokud jsou v oblastech text hello disky hello.

#### <a name="other-azure-vm-configuration-settings"></a>Další nastavení konfigurace virtuálního počítače Azure
Při vytváření virtuálního počítače Azure, budete vyzváni tooconfigure určitá nastavení virtuálního počítače. Pamatujte si, že se stanoví několik nastavení pro hello životnost hello virtuálních počítačů, zatímco můžete upravit nebo jiné přidat později. Zkontrolujte nastavení konfigurace virtuálního počítače Azure a přesvědčte se, že jsou správně nakonfigurována toomatch požadavků na zatížení.

### <a name="optimization"></a>Optimalizace
[Azure Premium Storage: Návrh vysoce výkonné](storage-premium-storage-performance.md) poskytuje pokyny pro vytváření vysoce výkonné aplikace pomocí Azure Premium Storage. Můžete provést hello pokyny v kombinaci s výkonu osvědčených postupů použít tootechnologies používá vaše aplikace.

## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Příprava a zkopírovat virtuální pevné disky (VHD) tooPremium úložiště
Hello následující část obsahuje pokyny pro přípravu virtuální pevné disky z virtuálního počítače a kopírování virtuálních pevných disků tooAzure úložiště.

* [Scénář 1: "I mě migrace tooAzure existující virtuální počítače Azure Premium Storage."](#scenario1)
* [Scénář 2: "I mě migrace virtuálních počítačů z jiných platforem tooAzure Storage úrovně Premium."](#scenario2)

### <a name="prerequisites"></a>Požadavky
tooprepare hello virtuální pevné disky pro migraci, budete potřebovat:

* Předplatné Azure, účet úložiště a kontejneru v tomto úložišti účtu toowhich můžete zkopírovat svůj disk VHD. Všimněte si, že hello cílový účet úložiště může být účet Standard nebo Premium Storage podle potřeby.
* Nástroj toogeneralize text hello virtuálního pevného disku, pokud máte v plánu toocreate více instancí virtuálního počítače z něj. Například nástroj sysprep pro systém Windows nebo virt.krychle sysprep pro Ubuntu.
* Nástroj tooupload hello virtuálního pevného disku soubor toohello účet úložiště. V tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md) nebo použijte [Azure storage Exploreru](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Tato příručka popisuje kopírování svůj disk VHD pomocí nástroje AzCopy hello.

> [!NOTE]
> Pokud zvolíte možnost synchronní kopie s AzCopy, optimální výkon, zkopírujte svůj disk VHD spuštěním jednoho z těchto nástrojů z virtuálního počítače Azure, který je v hello stejné oblasti jako hello cílový účet úložiště. Pokud kopírujete virtuální pevný disk z virtuálního počítače Azure v jiné oblasti, může být pomalejší výkon.
>
> Kopírování velkého množství dat přes omezenou šířkou pásma, zvažte [pomocí hello Azure Import/Export služby tootransfer data tooBlob úložiště](../storage-import-export-service.md); to vám umožní tootransfer data přesouvání pevným diskem disky tooan Azure Datacenter. Můžete použít Azure Import/Export služby toocopy data tooa standardní účet úložiště pouze hello. Jakmile hello data ve vašem účtu standardní úložiště, můžete použít buď hello [kopírovat objekt Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) nebo AzCopy tootransfer hello data tooyour prémiový účet úložiště.
>
> Všimněte si, že Microsoft Azure podporuje pouze soubory virtuálního pevného disku pevné velikosti. Soubory VHDX nebo dynamické virtuální pevné disky nejsou podporovány. Pokud máte dynamický virtuální pevný disk, můžete ji převést toofixed velikost pomocí hello [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) rutiny.
>
>

### <a name="scenario1"></a>Scénář 1: "I mě migrace tooAzure existující virtuální počítače Azure Premium Storage."
Pokud migrujete existující virtuální počítače Azure, hello zastavení virtuálního počítače, virtuální pevné disky na hello typ virtuálního pevného disku, které chcete připravit a zkopírujte hello virtuálního pevného disku s AzCopy nebo prostředí PowerShell.

Hello virtuálních počítačů musí toobe úplně dolů toomigrate do čistého stavu. Budou existovat výpadky až po dokončení migrace hello.

#### <a name="step-1-prepare-vhds-for-migration"></a>Krok 1. Příprava na migraci virtuálních pevných disků
Pokud provádíte migraci virtuálních počítačů Azure tooPremium existující úložiště, může být svůj disk VHD:

* Bitovou kopii operačního systému
* Disk jedinečné operačního systému
* Datový disk

Níže budeme provede tyto 3 scénáře pro přípravu svůj disk VHD.

##### <a name="use-a-generalized-operating-system-vhd-toocreate-multiple-vm-instances"></a>Používání zobecněný virtuální pevný disk operačního systému toocreate více instancí virtuálního počítače
Pokud nahráváte virtuálního pevného disku, které budou použité toocreate více obecné instancí virtuálního počítače Azure, musíte nejprve generalize virtuální pevný disk pomocí nástroje sysprep. To platí tooa virtuální pevný disk, který je místně nebo v cloudu hello. Nástroj Sysprep odebere všechny informace specifické pro počítač hello virtuálního pevného disku.

> [!IMPORTANT]
> Pořízení snímku nebo zálohování virtuálního počítače před jeho generalizací. Spuštěný nástroj sysprep zastaví a navrátit hello instance virtuálního počítače. Postupujte podle kroků toosysprep virtuálního pevného disku operačního systému Windows. Všimněte si, že spustíte příkaz Sysprep hello bude vyžadovat tooshut dolů hello virtuálního počítače. Další informace o nástroji Sysprep najdete v tématu [přehled nástroje Sysprep](http://technet.microsoft.com/library/hh825209.aspx) nebo [technické informace o nástroji Sysprep](http://technet.microsoft.com/library/cc766049.aspx).
>
>

1. Otevřete okno příkazového řádku jako správce.
2. Zadejte následující příkaz tooopen Sysprep hello:

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. V hello nástroj pro přípravu systému, vyberte možnost zadejte systému Out-of-Box prostředí (při prvním zapnutí), vyberte hello generalizace zaškrtávací políčko, vyberte **vypnutí**a potom klikněte na **OK**, jak ukazuje následující obrázek hello. Nástroj Sysprep se generalize hello operačního systému a vypnout hello systém.

    ![][1]

U virtuálního počítače s Ubuntu hello tooachieve virt.krychle sysprep pomocí stejné. V tématu [virt.krychle sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) další podrobnosti. Viz také některé softwaru hello open source [Linux serveru zřizování softwaru](http://www.cyberciti.biz/tips/server-provisioning-software.html) pro jiné operační systémy Linux.

##### <a name="use-a-unique-operating-system-vhd-toocreate-a-single-vm-instance"></a>Použijte jedinečný virtuální pevný disk operačního systému toocreate jedna instance virtuálního počítače
Pokud máte aplikace běžící v hello virtuálních počítačů, které vyžaduje hello počítače konkrétní data, není generalize hello virtuálního pevného disku. Zobecněný virtuální pevný disk se dá použít toocreate jedinečné instance virtuálního počítače Azure. Například pokud máte řadiče domény na svůj disk VHD, provádění sysprep znamená, že neúčinná jako řadič domény. Zkontrolujte hello aplikací běžících na váš počítač a hello vlivu spuštěním programu sysprep v nich před generalizací hello virtuálního pevného disku.

##### <a name="register-data-disk-vhd"></a>Registrovat datový disk VHD
Pokud máte datové disky v Azure toobe migrovat, musí zkontrolujte, zda text hello virtuálních počítačů, které pomocí těchto dat, které disky jsou vypnout.

Postupujte podle kroků hello popsaných níže toocopy virtuálního pevného disku tooAzure Storage úrovně Premium a zaregistrovat ji jako zřízené datový disk.

#### <a name="step-2-create-hello-destination-for-your-vhd"></a>Krok 2. Vytvoření hello cíl pro svůj disk VHD
Vytvořte účet úložiště pro údržbu virtuální pevné disky. Vezměte v úvahu následující body při plánování kde hello toostore virtuální pevné disky:

* cíl Hello prémiový účet úložiště.
* Hello umístění účtu úložiště musí být stejná jako úložiště Premium podporuje virtuální počítače Azure v konečné fázi hello vytvoříte. Může zkopírovat tooa nový účet úložiště, nebo plán toouse hello stejný účet úložiště podle vašich potřeb.
* Zkopírujte a uložte klíč účtu úložiště hello hello cílový účet úložiště pro další fáze hello.

Pro datové disky, můžete zvolit tookeep některé datové disky v standardní účet úložiště (například disky, které obsahují chladič úložiště), ale důrazně doporučujeme přesouvání všech dat pro produkční zatížení toouse premium storage.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Krok 3. Zkopírujte virtuální pevný disk s AzCopy nebo prostředí PowerShell
Budete potřebovat toofind cesta kontejneru a úložiště účet klíče tooprocess jednu z těchto dvou možností. Klíč účtu úložiště a cesta kontejneru lze nalézt v **portálu Azure** > **úložiště**. jako "https://myaccount.blob.core.windows.net/mycontainer/" bude mít adresu URL kontejneru Hello.

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Možnost 1: Zkopírujte virtuální pevný disk s AzCopy (asynchronní kopie)
Pomocí AzCopy, můžete snadno odeslat hello VHD přes hello Internetu. V závislosti na velikosti hello hello virtuálních pevných disků může to trvat čas. Při použití této možnosti mějte na paměti limity vstupní/výstupní účtu úložiště toocheck hello. V tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md) podrobnosti.

1. Stáhněte a nainstalujte AzCopy odsud: [nejnovější verzi AzCopy](http://aka.ms/downloadazcopy)
2. Otevřete prostředí Azure PowerShell a přejděte toohello složku, kde je nainstalován nástroj AzCopy.
3. Použití hello následující příkaz toocopy hello souboru virtuálního pevného disku z "Zdroje" příliš "Cílové".

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Příklad:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    Zde je uveden popis hello parametry použité v hello AzCopy příkaz:

   * **/ Zdroj:  *&lt;zdroj&gt;:***  umístění složky hello nebo URL kontejneru úložiště, která obsahuje hello virtuálního pevného disku.
   * **/ SourceKey:  *&lt;klíč účtu zdroj&gt;:***  klíč účtu úložiště účtu úložiště hello zdroje.
   * **/ Cíle:  *&lt;cílové&gt;:***  toocopy URL kontejneru úložiště hello virtuálního pevného disku na.
   * **/ DestKey:  *&lt;klíč účtu cíle&gt;:***  klíč účtu úložiště hello cílový účet úložiště.
   * **/ Vzor:  *&lt;název souboru&gt;:***  zadejte název souboru hello toocopy hello virtuálního pevného disku.

Podrobnosti o použití nástroje AzCopy nástroje najdete v tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Možnost 2: Zkopírujte virtuální pevný disk pomocí prostředí PowerShell (Synchronized kopie)
Můžete také zkopírovat soubor virtuálního pevného disku hello pomocí rutiny prostředí PowerShell hello AzureStorageBlobCopy spuštění. Použijte následující příkaz v prostředí Azure PowerShell toocopy virtuálního pevného disku hello. Nahraďte hodnoty hello v <> odpovídající hodnoty z vašeho zdrojového a cílového účtu úložiště. Tento příkaz, musíte mít volat virtuální pevné disky ve vašem účtu úložiště cílový kontejner toouse. Pokud hello kontejner neexistuje, vytvořte ji před spuštěním příkazu hello.

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

Příklad:

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <a name="scenario2"></a>Scénář 2: "I mě migrace virtuálních počítačů z jiných platforem tooAzure Storage úrovně Premium."
Pokud migrujete virtuální pevný disk z úložiště v Azure Cloud tooAzure, musíte napřed exportovat hello virtuálního pevného disku tooa místního adresáře. Cesta kompletní zdrojový hello hello místnímu adresáři, kde je uložený užitečný virtuálního pevného disku a potom použitím AzCopy tooupload ho tooAzure úložiště.

#### <a name="step-1-export-vhd-tooa-local-directory"></a>Krok 1. Export virtuálního pevného disku tooa místního adresáře
##### <a name="copy-a-vhd-from-aws"></a>Zkopírujte virtuální pevný disk z AWS
1. Pokud používáte AWS, exportujte hello EC2 instance tooa virtuálního pevného disku v sady Amazon S3. Postupujte podle kroků hello popsaných v hello Amazon dokumentaci k rozhraní příkazového řádku (CLI) nástroj pro export instancí EC2 Amazon tooinstall hello Amazon EC2 a spusťte soubor VHD tooa hello EC2 hello vytvořit instanci export úkolů příkaz tooexport instance. Být jisti toouse **virtuálního pevného disku** pro hello DISK &#95; IMAGE &#95; Formát proměnné při spuštění hello **vytvořit instanci export úkolů** příkaz. Hello exportovaný soubor virtuálního pevného disku se uloží do sady hello Amazon S3, které určíte během tohoto procesu.

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. Stáhněte si soubor virtuálního pevného disku hello ze sady S3 hello. Vyberte hello souboru virtuálního pevného disku, pak **akce** > **Stáhnout**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Zkopírujte virtuální pevný disk z jiné mimo Azure cloud
Pokud migrujete virtuální pevný disk z úložiště v Azure Cloud tooAzure, musíte napřed exportovat hello virtuálního pevného disku tooa místního adresáře. Zkopírujte hello dokončení zdrojová cesta hello místnímu adresáři, kde je uložený virtuální pevný disk.

##### <a name="copy-a-vhd-from-on-premises"></a>Zkopírujte virtuální pevný disk z místní
Pokud migrujete virtuální pevný disk z místního prostředí, budete potřebovat hello kompletní zdrojový cestu, kde je uložený virtuální pevný disk. Hello zdrojová cesta může být server umístění nebo sdílené složky.

#### <a name="step-2-create-hello-destination-for-your-vhd"></a>Krok 2. Vytvoření hello cíl pro svůj disk VHD
Vytvořte účet úložiště pro údržbu virtuální pevné disky. Vezměte v úvahu následující body při plánování kde hello toostore virtuální pevné disky:

* Hello cílový účet úložiště může být úložiště standard nebo premium, v závislosti na požadavcích vaší aplikace.
* Hello oblast účtu úložiště musí být stejná jako úložiště Premium podporuje virtuální počítače Azure v konečné fázi hello vytvoříte. Může zkopírovat tooa nový účet úložiště, nebo plán toouse hello stejný účet úložiště podle vašich potřeb.
* Zkopírujte a uložte klíč účtu úložiště hello hello cílový účet úložiště pro další fáze hello.

Důrazně doporučujeme přesouvání všech dat pro produkční zatížení toouse premium storage.

#### <a name="step-3-upload-hello-vhd-tooazure-storage"></a>Krok 3. Nahrání virtuálního pevného disku tooAzure hello úložiště
Nyní když máte v místních adresářových hello svůj disk VHD, můžete použít AzCopy nebo AzurePowerShell tooupload hello VHD soubor tooAzure úložiště. Obě možnosti jsou k dispozici zde:

##### <a name="option-1-using-azure-powershell-add-azurevhd-tooupload-hello-vhd-file"></a>Možnost 1: Použití Azure PowerShell Add-AzureVhd tooupload hello VHD souboru

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

Příklad <Uri> může být ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***. Příklad <FileInfo> může být ***"C:\path\to\upload.vhd"***.

##### <a name="option-2-using-azcopy-tooupload-hello-vhd-file"></a>Možnost 2: Pomocí souboru VHD hello tooupload AzCopy
Pomocí AzCopy, můžete snadno odeslat hello VHD přes hello Internetu. V závislosti na velikosti hello hello virtuálních pevných disků může to trvat čas. Při použití této možnosti mějte na paměti limity vstupní/výstupní účtu úložiště toocheck hello. V tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md) podrobnosti.

1. Stáhněte a nainstalujte AzCopy odsud: [nejnovější verzi AzCopy](http://aka.ms/downloadazcopy)
2. Otevřete prostředí Azure PowerShell a přejděte toohello složku, kde je nainstalován nástroj AzCopy.
3. Použití hello následující příkaz toocopy hello souboru virtuálního pevného disku z "Zdroje" příliš "Cílové".

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Příklad:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    Zde je uveden popis hello parametry použité v hello AzCopy příkaz:

   * **/ Zdroj:  *&lt;zdroj&gt;:***  umístění složky hello nebo URL kontejneru úložiště, která obsahuje hello virtuálního pevného disku.
   * **/ SourceKey:  *&lt;klíč účtu zdroj&gt;:***  klíč účtu úložiště účtu úložiště hello zdroje.
   * **/ Cíle:  *&lt;cílové&gt;:***  toocopy URL kontejneru úložiště hello virtuálního pevného disku na.
   * **/ DestKey:  *&lt;klíč účtu cíle&gt;:***  klíč účtu úložiště hello cílový účet úložiště.
   * **/ BlobType: stránka:** Určuje, že cíl hello je objekt blob stránky.
   * **/ Vzor:  *&lt;název souboru&gt;:***  zadejte název souboru hello toocopy hello virtuálního pevného disku.

Podrobnosti o použití nástroje AzCopy nástroje najdete v tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Další možnosti pro odesílání virtuálního pevného disku
Můžete také nahrát účtu úložiště tooyour virtuální pevný disk pomocí jedné z hello následujícím způsobem:

* [Objekt Blob služby Azure Storage kopie rozhraní API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [Objektů BLOB Azure Storage Explorer odesílání](https://azurestorageexplorer.codeplex.com/)
* [Referenční dokumentace rozhraní API úložiště importu/exportu služby REST](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> Doporučujeme používat službu Import/Export, pokud je předpokládaná doba odesílání doba je delší než 7 dní. Můžete použít [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) čas hello tooestimate z jednotky velikost a přenos dat.
>
> Import a Export lze použít toocopy tooa standardní účet úložiště. Budete potřebovat toocopy z účtu úložiště toopremium standardní úložiště pomocí nástroje, například AzCopy.
>
>

## <a name="create-azure-virtual-machine-using-premium-storage"></a>Vytvořit virtuální počítače Azure pomocí služby Storage úrovně Premium
Po hello virtuálního pevného disku je nahrané nebo zkopírovaný toohello potřeby účet úložiště, postupujte podle pokynů hello v této části tooregister hello virtuálního pevného disku jako bitovou kopii operačního systému, nebo disk operačního systému v závislosti na vašem scénáři a pak vytvořte instanci virtuálního počítače z něj. Hello datový disk VHD může být připojené toohello virtuálního počítače, po jejím vytvoření.
Ukázkový skript migrace je k dispozici na konci hello v této části. Tento jednoduchý skript neodpovídá všechny scénáře. Může být nutné tooupdate hello skriptu toomatch s konkrétní scénář. Níže najdete toosee, pokud se tento skript vztahuje tooyour scénáři [A ukázkový migrace skript](#a-sample-migration-script).

### <a name="checklist"></a>Kontrolní seznam
1. Počkat, až všechny disky VHD hello kopírování je dokončena.
2. Ujistěte se, že úložiště Premium je k dispozici v hello oblasti, které provádíte migraci do.
3. Rozhodněte, hello nový virtuální počítač řad, které budete používat. Měla by být schopný úložiště Premium a hello velikost by měla být v závislosti na dostupnosti hello v oblasti hello a podle potřeb.
4. Rozhodněte, hello přesnou velikost virtuálního počítače, které budete používat. Velikost virtuálního počítače musí toobe dostatečně velké na to toosupport hello počet datových disků, které máte. Například Pokud máte 4 datových disků, musí mít hello virtuálních počítačů 2 nebo více jader. Zvažte také výpočetní výkon, paměti a šířky pásma sítě musí.
5. Vytvořte účet úložiště Premium hello cílová oblast. Toto je účet hello budete používat pro hello nového virtuálního počítače.
6. Máte hello aktuální virtuální počítač podrobnosti o užitečný, včetně hello seznam disků a odpovídající BLOB VHD.

Připravte aplikaci výpadek. toodo čisté migrace, máte toostop veškeré zpracování hello v aktuálním systému hello. Pak můžete ho získat tooconsistent stavu, který je možné migrovat toohello nová platforma. Doba trvání výpadku bude záviset na hello množství dat v toomigrate disky hello.

> [!NOTE]
> Pokud vytváříte virtuální počítač Azure Resource Manager z specializované disku VHD, naleznete příliš[této šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) pro nasazení Resource Manager virtuálního počítače pomocí stávající disk.
>
>

### <a name="register-your-vhd"></a>Zaregistrovat svůj disk VHD
toocreate virtuálního počítače z virtuálního pevného disku operačního systému nebo tooattach tooa disku data nového virtuálního počítače, nejprve je nutné zaregistrovat je. Postupujte podle kroků v závislosti na scénáři svůj disk VHD.

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a>Zobecněný virtuální pevný disk operačního systému toocreate více instancí virtuálního počítače Azure
Po odeslání účet úložiště toohello virtuální pevný disk zobecněný bitovou kopii operačního systému ji jako zaregistrovat **Image virtuálního počítače Azure** tak, aby z ní můžete vytvořit jeden nebo více instancí virtuálního počítače. Použijte svůj disk VHD hello následující tooregister rutiny prostředí PowerShell jako image operačního systému virtuálního počítače Azure. Zadejte adresu URL dokončení kontejneru hello kde virtuálního pevného disku byla zkopírována do.

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

Zkopírujte a uložte hello název tuto novou bitovou kopii virtuálního počítače Azure. V předchozím příkladu hello, je *OSImageName*.

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a>Jedinečný virtuální pevný disk operačního systému toocreate jedna instance virtuálního počítače Azure
Po hello jedinečný virtuální pevný disk operačního systému je úložiště nahrané toohello účet, zaregistrujte si ho jako **Disk s operačním systémem Azure** tak, aby instance virtuálního počítače můžete vytvořit z něj. Použijte tyto tooregister rutiny prostředí PowerShell svůj disk VHD jako Disk operačního systému Azure. Zadejte adresu URL dokončení kontejneru hello kde virtuálního pevného disku byla zkopírována do.

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

Zkopírujte a uložte hello název tento nový Disk operačního systému Azure. V předchozím příkladu hello, je *OSDisk*.

#### <a name="data-disk-vhd-toobe-attached-toonew-azure-vm-instances"></a>Připojit datový disk VHD toobe toonew instance virtuálního počítače Azure
Po hello datový disk VHD je odeslání toostorage účet, zaregistrovat ji jako datový Disk Azure tak, aby bylo možné ho připojené tooyour nové DS Series, DSv2 series nebo GS virtuálního počítače Azure řady instance.

Použijte tyto tooregister rutiny prostředí PowerShell svůj disk VHD jako datový Disk Azure. Zadejte adresu URL dokončení kontejneru hello kde virtuálního pevného disku byla zkopírována do.

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

Zkopírujte a uložte hello název tento nový datový Disk Azure. V předchozím příkladu hello, je *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Vytvoření virtuálního počítače s podporou služby Storage úrovně Premium
Jednou hello bitové kopie operačního systému nebo disk operačního systému jsou zaregistrovány, vytvořte nový DS-series, DSv2-series nebo GS-series virtuálních počítačů. Budete používat hello bitovou kopii operačního systému nebo název disku operačního systému, který je zaregistrovaný. Vyberte typ hello virtuálního počítače z vrstvy úložiště Premium hello. V následujícím příkladu používáme hello *Standard_DS2* velikost virtuálního počítače.

> [!NOTE]
> Aktualizace se, že odpovídá kapacitu a požadavky na výkon a velikosti disku Azure hello toomake velikost disku hello.
>
>

Rutiny Powershellu krok za krokem postupujte podle hello níže toocreate hello nového virtuálního počítače. Nastavte nejprve, hello společné parametry:

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

Nejprve vytvořte Cloudová služba, ve kterém bude hostitelem nové virtuální počítače.

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

Dále v závislosti na scénáři, vytvořte instanci virtuálního počítače Azure hello z hello bitovou kopii operačního systému nebo Disk operačního systému, který je zaregistrovaný.

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a>Zobecněný virtuální pevný disk operačního systému toocreate více instancí virtuálního počítače Azure
Vytvořte jeden nebo více nový virtuální počítač Azure řady DS instancí pomocí hello hello **bitovou kopii operačního systému služby Azure** který je zaregistrovaný. Při vytváření nového virtuálního počítače, jak je uvedeno níže, zadejte název této bitové kopie operačního systému v konfiguraci virtuálního počítače hello.

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a>Jedinečný virtuální pevný disk operačního systému toocreate jedna instance virtuálního počítače Azure
Vytvoření nové služby DS řady virtuálního počítače Azure instance pomocí hello **Disk s operačním systémem Azure** který je zaregistrovaný. Při vytváření nového virtuálního počítače hello, jak je uvedeno níže, zadejte v konfiguraci virtuálního počítače hello tento název disku operačního systému.

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

Zadejte další virtuální počítač Azure informace, například cloudové služby, oblast, účet úložiště, skupinu dostupnosti a zásady ukládání do mezipaměti. Poznámka: hello instance virtuálního počítače musí být umístěn společně s přidružené operační systém nebo datové disky, takže hello vybrané cloudové služby, oblast a účet úložiště musí být v hello stejné umístění jako hello základní virtuální pevné disky těchto disků.

### <a name="attach-data-disk"></a>Připojení datového disku
Nakonec, pokud jste si zaregistrovali dat disku VHD, připojte je toohello nové úložiště Premium podporující virtuálního počítače Azure.

Použijte následující prostředí PowerShell rutinu tooattach datového disku toohello nového virtuálního počítače a zadejte hello zásady ukládání do mezipaměti. V příkladu níže hello zásady ukládání do mezipaměti je nastaven příliš*jen pro čtení*.

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> Mohou existovat další kroky potřebné toosupport aplikace, který je nesmí být zahrnuté v této příručce.
>
>

### <a name="checking-and-plan-backup"></a>Kontrola a plánování zálohování
Jednou hello nový virtuální počítač je zapnutý a běží, pomocí hello stejné id a heslo pro přístup k jako hello původní virtuální počítač a ověřte, zda vše funguje podle očekávání. Všechna nastavení hello, včetně hello prokládané svazky, budou existovat v hello nového virtuálního počítače.

posledním krokem Hello je tooplan plán zálohování a údržby pro nový virtuální počítač podle potřeb hello aplikace hello.

### <a name="a-sample-migration-script"></a>Ukázkový skript migrace
Pokud máte více toomigrate virtuální počítače, bude užitečné automatizace prostřednictvím skriptů prostředí PowerShell. Následuje ukázkový skript, který zautomatizuje hello migrace virtuálního počítače. Poznámka: níže uvedený skript se pouze příklad a neexistují několik předpokladům o hello aktuální disky virtuálních počítačů. Může být nutné tooupdate hello skriptu toomatch s konkrétní scénář.

předpoklady Hello jsou:

* Vytváříte klasické virtuální počítače Azure.
* Vaše zdrojové disky operačního systému a datové disky zdroje jsou ve stejném účtu úložiště a kontejneru. Pokud vaše disky operačního systému a datové disky nejsou v hello stejné umístění, můžete použít AzCopy nebo Azure PowerShell toocopy disků VHD přes účty úložiště a kontejnery. Najdete v předchozím kroku toohello: [kopie virtuálního pevného disku s AzCopy nebo prostředí PowerShell](#copy-vhd-with-azcopy-or-powershell). Úpravy tento skript toomeet váš scénář je další možnost, ale doporučujeme použít AzCopy nebo prostředí PowerShell, protože je jednodušší a rychlejší.

skriptu pro automatizaci Hello najdete níže. Nahradí text s informacemi a aktualizujte hello skriptu toomatch s konkrétní scénář.

> [!NOTE]
> Pomocí existujícího skriptu, který hello nezachovává hello síťovou konfiguraci vašeho zdrojového virtuálního počítače. Nastavení sítě hello toore-config budete potřebovat na migrovaných virtuálních počítačů.
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE tooshow how toomigrate a VM from a standard storage account tooa premium storage account. You can customize it according tooyour specific requirements.

    .Description
    hello script will copy hello vhds (page blobs) of hello source VM toohello new storage account.
    And then it will create a new VM from these copied vhds based on hello inputs that you specified for hello new VM.
    You can modify hello script toosatisfy your specific requirement, but please be aware of hello items specified
    in hello Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED toohello IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. hello ENTIRE RISK OF USE, INABILITY tooUSE, OR
    RESULTS FROM hello USE OF THIS CODE REMAINS WITH hello USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    toofind more information about how tooset up Azure PowerShell, refer toohello following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # hello cloud service name of hello VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # hello VM name toocopy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # hello destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # hello destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # hello destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # hello destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # hello location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not toocopy hello os disk, hello default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently tooreport hello copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # hello name suffix tooadd toonew created disks tooavoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import hello Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check hello Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer toothis article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ tooconnect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if hello VM is shut down
    #  Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - hello source VM doesn't exist in hello current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation. Azure does not support live migration at this time. If you'd like toocreate a VM from a generalized image, sys-prep hello Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish toostop $SourceVMName now? Input 'N' if you want tooshut down hello VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until hello VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName tooshut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting hello sourve vm tooa configuration file, you can restore hello original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration too$vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy hello vhds of hello source vm
    #  You can choose toocopy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly toobe $false. hello default is toocopy only data disk vhds
    #  and hello new VM will boot from hello original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering hello data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in hello destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need toocopy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting hello source VM so that dest VM can boot
        # from hello same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose toocopy data disks only. Moving VM requires removing hello original VM (hello disks and backing vhd files will NOT be deleted) so that hello new VM can boot from hello same vhd. This is an irreversible action. Do you wish tooproceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing hello original VM (hello vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill hello OS disk is released by source VM. This may take up tooseveral minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy hello os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update hello media link toopoint toohello target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy toocomplete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  hello new VM can be created from hello copied disks or hello original os disk.
    #  You can ddd your own logic here toosatisfy your specific requirements of hello vm.
    #######################################################################

    # Create a VM from hello existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached hello copied data disks toohello new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of hello source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want tooadd more custimization toohello new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <a name="optimization"></a>Optimalizace
Aktuální konfigurace virtuálních počítačů lze přizpůsobit konkrétně toowork dobře u standardní disky. Například tooincrease pomocí mnoho disků v prokládaný svazek výkonu hello. Místo použití 4 disky samostatně na Storage úrovně Premium, například může být schopný toooptimize hello náklady tak, že jeden disk. Optimalizace jako tento nutné toobe zpracovává případ od případu a vyžadují vlastní kroky po migraci hello. Všimněte si také, že tento proces nemusí fungovat i pro databáze a aplikace, které závisí na rozložení disku hello definované v instalačním programu hello.

##### <a name="preparation"></a>Příprava
1. Dokončení hello jednoduché migrace, jak je popsáno v hello dříve části. Optimalizace se provede na hello nový virtuální počítač po migraci hello.
2. Zadejte nové velikosti disku hello potřebného pro konfiguraci hello optimalizované.
3. Určuje mapování hello aktuální disky nebo svazky toohello nové specifikace disku.

##### <a name="execution-steps"></a>Provedení kroků
1. Vytvořte nové disky hello se správnými velikostmi hello na hello virtuálního počítače služby Storage úrovně Premium.
2. Přihlášení toohello virtuálních počítačů a zkopírujte hello data z hello aktuální svazek toohello nový disk, který mapuje toothat svazku. To lze proveďte pro všechny svazky aktuální hello, které je třeba toomap tooa nový disk.
3. V dalším kroku změnit hello aplikace nastavení tooswitch toohello nové disky a odpojit hello starých svazků.

Ladění aplikace hello pro lepší výkon disku, naleznete příliš[optimalizace výkonu aplikace](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Migrace aplikací
Databáze a jiné komplexní aplikace může vyžadovat zvláštní postup podle definice poskytovatele aplikace hello hello migrace. Naleznete v dokumentaci toorespective aplikace. Například Obvykle můžete migrovat databází pomocí zálohování a obnovení.

## <a name="next-steps"></a>Další kroky
V tématu hello následující prostředků u konkrétních scénářů pro migraci virtuálních počítačů:

* [Migrovat virtuální počítače, které jsou mezi účty úložiště Azure](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Vytvoření a nahrání virtuálního pevného disku serveru Windows tooAzure.](../../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Vytvoření a nahrání virtuálního pevného disku tohoto hello obsahuje operační systém Linux](../../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Migrace virtuálních počítačů z Amazon AWS tooMicrosoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Viz také hello následující prostředky toolearn Další informace o Azure Storage a virtuálních počítačích Azure:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Storage úrovně Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
