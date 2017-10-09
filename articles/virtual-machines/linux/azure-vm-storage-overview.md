---
title: "aaaAzure virtuální počítače s Linuxem a Azure Storage | Microsoft Docs"
description: "Popisuje Azure Standard a Premium Storage a spravované i nespravované disky se virtuální počítače s Linuxem."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: d364c69e-0bd1-4f80-9838-bbc0a95af48c
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 2/7/2017
ms.author: rasquill
ms.openlocfilehash: d34441698a4e59721847685099e5fb3aa378c597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-and-linux-vm-storage"></a>Úložiště Azure a virtuální počítač s Linuxem
Azure Storage je řešení cloudového úložiště pro moderní aplikace, které jsou závislé na odolnost, dostupnost a škálovatelnost toomeet hello potřebám zákazníků hello.  V přidání toomaking je možné pro vývojáře toobuild aplikace ve velkém měřítku toosupport nové scénáře, Azure Storage taky poskytuje hello základy úložiště pro virtuální počítače Azure.

## <a name="managed-disks"></a>Managed Disks

Virtuální počítače Azure jsou nyní k dispozici pomocí [Azure spravované disky](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), což je toocreate umožňuje virtuální počítače bez vytváření a správě žádné [účty Azure Storage](../../storage/common/storage-introduction.md) sami. Můžete určit, zda chcete Premium nebo standardního úložiště a jak velký hello disku by měla být a hello disky virtuálních počítačů pro vás vytvoří Azure. Virtuální počítače s spravované disky mají mnoho důležitých funkcí, včetně:

- Podpora automatického škálovatelnost. Azure vytvoří hello disky a spravuje hello základní úložiště toosupport až too10, 000 disky na jedno předplatné.
- Vyšší spolehlivost skupiny dostupnosti. Azure zajišťuje, aby byly disky virtuálního počítače automaticky od sebe navzájem oddělené v rámci skupiny dostupnosti.
- Řízení přístupu vyšší. Spravované disky vystavit různých operací, které řídí [řízení řízení přístupu (RBAC)](../../active-directory/role-based-access-control-what-is.md).

Ceny pro spravované disky je jiný než pro který nespravované disků. Podrobnosti naleznete v tématu [ceny a fakturace pro spravované disky](../windows/managed-disks-overview.md#pricing-and-billing).

Můžete převést stávající virtuální počítače, které používají nespravovaná disky toouse spravované disky s [převést virtuální počítač az](/cli/azure/vm#convert). Další informace najdete v tématu [jak tooconvert virtuálního počítače s Linuxem z nespravovaných disky disky spravované tooAzure](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nelze převést nespravované disku se spravovaným diskem Pokud hello nespravované disk v účtu úložiště, který je nebo kdykoli, už šifrovat pomocí [Azure úložiště služby šifrování (SSE)](../../storage/common/storage-service-encryption.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Následující kroky podrobností jak tootooconvert nespravované disky, které jsou, nebo byla v účtu úložiště šifrované Hello:

- Zkopírujte hello virtuální pevný disk (VHD) s [az úložiště objektů blob kopie počáteční](/cli/azure/storage/blob/copy#start) tooa účet úložiště, který nikdy bylo povoleno šifrování služby úložiště Azure.
- Vytvoření virtuálního počítače, které používá spravované disky a zadejte tento soubor virtuálního pevného disku při vytvoření s [vytvořit virtuální počítač az](/cli/azure/vm#create), nebo
- Připojte hello zkopírovat virtuální pevný disk s [připojit disk virtuálního počítače az](/cli/azure/vm/disk#attach) discích spravovaných tooa spuštění virtuálního počítače s.


## <a name="azure-storage-standard-and-premium"></a>Úložiště Azure: Standard a Premium
Virtuální počítače Azure – jestli pomocí disků spravované nebo nespravované – může být založena na standardní úložiště disků nebo disků úložiště premium. Při použití portálu toochoose hello virtuálního počítače musí přepnout rozevírací na hello **Základy** obrazovky tooview disky standard a premium. Když přepnuto tooSSD, pouze storage úrovně premium povoleno virtuální počítače se zobrazí, všechny založenou na SSD disky.  Když přepnuto tooHDD povoleno úložiště standard virtuální počítače zajišťoval otáčí diskové jednotky se zobrazují, společně s založenou na SSD virtuální počítače služby storage úrovně premium.

Při vytváření virtuálního počítače z hello `azure-cli` můžete zvolit standard a premium při výběru hello velikost virtuálního počítače prostřednictvím hello `-z` nebo `--vm-size` příznak rozhraní příkazového řádku.

## <a name="creating-a-vm-with-a-managed-disk"></a>Vytvoření virtuálního počítače se spravovaným diskem

Hello následující příklad vyžaduje hello 2.0 rozhraní příkazového řádku Azure, která můžete [nainstalovat zde](/cli/azure/install-azure-cli).

Nejprve vytvořte hello toomanage prostředků skupiny prostředků s [vytvořit skupinu az](/cli/azure/group#create):

```azurecli
az group create --location westus --name myResourceGroup
```

Nyní vytvoří hello virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create). Zadejte jedinečný `--public-ip-address-dns-name` argument, jako `mypublicdns` pravděpodobně nebyla provedena.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns
```

Hello předchozí příklad vytvoří virtuální počítač se spravovaným diskem v standardní účet úložiště. toouse prémiový účet úložiště, přidejte hello `--storage-sku Premium_LRS` argument, jako je hello následující ukázka:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns \
    --storage-sku Premium_LRS
```

## <a name="standard-storage"></a>Storage úrovně Standard
Azure Standard Storage je hello výchozí typ úložiště.  Ale stále mít původce je nákladově efektivní úložiště Standard storage.  

## <a name="premium-storage"></a>Storage úrovně Premium
Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro virtuální počítače spuštěné I náročnými úlohy. Virtuální počítač (VM) disků, které používají úložiště Premium ukládat data do jednotek SSD (Solid-State Drive). Můžete migrovat tooAzure disky virtuálních počítačů vaší aplikace Storage úrovně Premium tootake výhod hello rychlosti a výkonu z těchto disků.

Funkce úložiště Premium:

* Disky úložiště Premium: Azure Premium Storage podporuje disky virtuálních počítačů, které může být připojené tooDS, DSv2 nebo GS virtuálních počítačích Azure řady.
* Objekt Blob stránky Premium: Premium Storage podporuje objekty BLOB stránky Azure, které jsou používané toohold trvalé disky pro virtuální počítače Azure (VM).
* Místně redundantní úložiště Premium: Účet úložiště Premium pouze podporuje místně redundantní úložiště (LRS) jako možnost replikace hello a udržuje tři kopie dat hello v jedné oblasti.
* [Storage úrovně Premium](../../storage/common/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Storage úrovně Premium podporované virtuální počítače
Premium Storage podporuje DS-series, DSv2-series, GS-series a Fs-series Azure virtuální počítače (VM). Můžete vytvořit disky úložiště Standard a Premium Storage úrovně Premium podporované virtuálních počítačů. Ale disky úložiště Premium nelze použít s řadou virtuálních počítačů, které nejsou kompatibilní úložiště Premium.

Toto jsou hello Linuxových distribucích, jsme ověřit Storage úrovně Premium.

| Distribuce | Verze | Podporované jádra |
| --- | --- | --- |
| Ubuntu |12.04 |3.2.0-75.110+ |
| Ubuntu |14.04 |3.13.0-44.73+ |
| Debian |7.x, 8.x |3.16.7-ckt4-1+ |
| SLES |SLES 12 |3.12.36-38.1+ |
| SLES |SLES 11 SP4 |3.0.101-0.63.1+ |
| CoreOS |584.0.0+ |3.18.4+ |
| Centos |6.5, 6.6, 6.7, 7.0, 7.1 |3.10.0-229.1.2.el7+ |
| RHEL |6.8+, 7.2+ | |

## <a name="azure-file-storage"></a>Úložiště Azure File
Azure File storage nabízí sdílené složky v cloudu hello pomocí standardního protokol SMB hello. Soubory Azure můžete migrovat podnikové aplikace, které jsou závislé na souborových serverech tooAzure. Aplikace běžící v Azure můžete snadno připojit sdílené složky z virtuálních počítačů Azure s Linuxem. A hello nejnovější verze služby úložiště File, můžete také připojit sdílenou složku z lokální aplikace, který podporuje protokol SMB 3.0.  Protože sdílené složky jsou sdílené složky protokolu SMB, můžete k nim přistupovat přes standardní systém souborů rozhraní API.

Úložiště souborů je založen na hello stejnou technologii jako úložiště objektů Blob, Table a Queue, takže File storage nabízí hello dostupnosti, odolnost, škálovatelnosti a geografická redundance, je integrovaná platforma hello úložiště Azure. Podrobnosti o souboru cíle výkonnosti úložiště a omezení najdete v tématu Azure úložiště škálovatelnost a cíle výkonnosti.

* [Jak toouse Azure File storage s Linuxem](../../storage/files/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Horkého úložiště
Hello Azure horké úrovni úložiště je optimalizovaná pro ukládání dat, která se využívají často.  Horkého úložiště je hello výchozí typ úložiště pro úložiště objektů blob.

## <a name="cool-storage"></a>Studeného úložiště
úroveň Hello studené úložiště Azure je optimalizovaná pro ukládání dat, která se nevyužívají dlohotrvající. Příklad případů použití pro studenou zahrnují zálohy, mediální obsah, vědeckých dat, dodržování předpisů a archivní data. Obecně platí všechna data, která přistupuje málokdy je ideální kandidátem na studených úložiště.

|  | Úroveň horkého úložiště | Úroveň studeného úložiště |
|:--- |:---:|:---:|
| Dostupnost |99,9 % |99 % |
| Dostupnost (čtení RA-GRS) |99,99 % |99,9 % |
| Poplatky za používání |Vyšší poplatky za uložení |Nižší poplatky za uložení |
| Nižší přístup |Vyšší přístup | |
| a cena za transakci |a cena za transakci | |

## <a name="redundancy"></a>Redundance
Hello data v Microsoft Azure účet úložiště se vždy replikují tooensure odolnost a vysokou dostupnost, splňuje hello smlouvy SLA pro úložiště Azure i v hello vzhled selhání hardwaru.

Při vytváření účtu úložiště, musíte vybrat jednu z hello následující možnosti replikace:

* Místně redundantní úložiště (LRS)
* Zónově redundantní úložiště (ZRS)
* Geograficky redundantní úložiště (GRS)
* Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)

### <a name="locally-redundant-storage"></a>Místně redundantní úložiště
Místně redundantní úložiště (LRS) replikuje data v rámci hello oblasti, ve které jste vytvořili účet úložiště. toomaximize odolnost, každý požadavek směřovaný na data ve vašem účtu úložiště se replikují třikrát. Tyto tři repliky každý nacházet v samostatné selhání domén a domén upgradu.  Žádost o vrátí úspěšně pouze po jejím napsané tooall tři repliky.

### <a name="zone-redundant-storage"></a>Zónově redundantní úložiště
Zónově redundantní úložiště (ZRS) replikuje data mezi dvěma toothree zařízení v jedné oblasti nebo v rámci dvou oblastí, nabízí tak větší odolnost než LRS. Pokud má váš účet úložiště povolená ZRS, vaše data byla odolná i v případě hello selhání v některém z pracoviště hello.

### <a name="geo-redundant-storage"></a>Geograficky redundantní úložiště
Geograficky redundantní úložiště (GRS) replikuje vaše data tooa sekundární oblast, která je stovky miles od primární oblasti hello. Pokud má váš účet úložiště GRS povoleno, vaše data byla odolná i v případě hello dokončení regionální výpadku nebo havárii, ve které hello primární oblast není použitelná pro obnovení.

### <a name="read-access-geo-redundant-storage"></a>Geograficky redundantní úložiště s přístupem pro čtení
Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) maximalizuje dostupnost pro váš účet úložiště, tím, že poskytuje přístup jen pro čtení toohello data v hello sekundárního umístění, kromě replikace toohello rámci dvou oblastí poskytované GRS. V hello události, která data se stane není k dispozici v hello primární oblasti můžete aplikaci přečíst data z hello sekundární oblast.

Pro podrobné informace do úložiště Azure redundance najdete v tématu:

* [Účet replikace Azure Storage](../../storage/common/storage-redundancy.md)

## <a name="scalability"></a>Škálovatelnost
Azure Storage je nesmírně škálovatelná služba, proto můžete ukládat a zpracovávat stovky terabajtů dat toosupport hello velkých objemů dat scénáře vyžadují vědeckou nebo finanční analýzu a aplikace média. Nebo můžete ukládat malá množství dat pro malé firmy web hello. Kdykoli budete potřebovat cokoli, platit pouze pro hello data, která ukládáte. Azure Storage aktuálně poskytuje úložiště pro bilión jedinečných zákaznických objektů a v průměru zpracovává miliony požadavků za sekundu.

Pro účty úložiště standard storage: standardní účet úložiště má rychlost maximální celkový počet požadavků z 20 000 IOPS. Hello celkový počet IOPS pro všechny disky virtuálního počítače v standardní účet úložiště nesmí být delší než tento limit.

Pro účty úložiště premium: prémiový účet úložiště je míra maximální celková propustnost 50 GB/s. Celková propustnost Hello všechny disky virtuálního počítače by neměl překročí toto omezení.

## <a name="availability"></a>Dostupnost
Nemůžeme zaručit, že alespoň 99,99 % (99,9 % Chladná úroveň přístupu) hello času, jsme se úspěšně zpracování požadavků tooread dat z účtů přístup pro čtení geograficky redundantní úložiště (RA-GRS), za předpokladu, že se nezdařilo pokusy o tooread data z primární oblasti hello jsou Opakovat v sekundární oblasti hello.

Nemůžeme zaručit, že minimálně 99,9 % (99 % Chladná úroveň přístupu) hello času, jsme se úspěšně proces vyžaduje tooread data z místně redundantní úložiště (LRS), zóny redundantní úložiště (ZRS) a účty geograficky redundantní úložiště (GRS).

Nemůžeme zaručit, že minimálně 99,9 % (99 % Chladná úroveň přístupu) hello času, jsme se úspěšně proces požaduje toowrite data tooLocally redundantní úložiště (LRS), zóny redundantní úložiště (ZRS) a účty geograficky redundantní úložiště (GRS) a přístup pro čtení geograficky redundantní Účty úložiště (RA-GRS).

* [Azure SLA pro úložiště](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)

## <a name="regions"></a>Oblasti
Azure v kolem hello, world 30 oblastech je všeobecně dostupná a má oznámeno plány pro 4 další oblasti. Geografické rozšíření je prioritu pro Azure, protože umožňuje, aby naše zákazníky tooachieve vyšší výkon a podporovat jejich požadavky a předvolby týkající se umístění dat.  Azures nejnovější toolaunch oblast je v Německu.

Hello Německo Microsoft Cloud poskytuje toohello odlišné možnost cloudové služby společnosti Microsoft již k dispozici ve Evropa, vytváření vyšší příležitosti pro inovace a hospodářského růstu pro vysoce regulovaná partnery a zákazníky v Německu, Hello Evropské unie (EU) a hello Evropského sdružení volného obchodu (ESVO).

Zákaznická data v těchto nových datových centrech v Magdeburgu a Frankfurt, je spravovaný pod kontrolou hello zplnomocněnec dat, systémy T mezinárodní, nezávislé němčině společnosti a pobočky německých Telekom. Komerční cloudové služby společnosti Microsoft v těchto datových centrech dodržovat předpisy na práci s daty tooGerman a poskytnout další výběr jak a kde je zpracování dat zákazníků.

* [Mapa oblastí Azure](https://azure.microsoft.com/regions/)

## <a name="security"></a>Zabezpečení
Úložiště Azure poskytuje komplexní sadu funkcí zabezpečení, které společně umožňují vývojářům toobuild zabezpečených aplikací. účet úložiště Hello, samotné lze zabezpečit pomocí řízení přístupu na základě Role a Azure Active Directory. Data lze zabezpečit během přenosu mezi aplikací a Azure pomocí šifrování na straně klienta, HTTPS nebo SMB 3.0. Data lze nastavit toobe šifrují automaticky při zápisu tooAzure úložiště pomocí šifrování služby úložiště (SSE). Operačního systému a datové disky používat virtuální počítače lze nastavit toobe šifrovat pomocí Azure Disk Encryption. Delegovaný přístup toohello datových objektů ve službě Azure Storage můžete udělit pomocí podpisy sdíleného přístupu.

### <a name="management-plane-security"></a>Zabezpečení roviny Management
Hello správu roviny se skládá z hello prostředky využívané toomanage účtu úložiště. V této části se budeme mluvit o hello modelu nasazení Azure Resource Manager a přístupu účty úložiště tooyour toocontrol toouse řízení přístupu na základě Role (RBAC). Bude také faktorech mluvíme o správě klíče účtu úložiště a jak tooregenerate je.

### <a name="data-plane-security"></a>Zabezpečení dat roviny
V této části se podíváme povolením přístupu toohello skutečné datové objekty v účtu úložiště, jako jsou objekty BLOB, soubory, fronty a tabulky, pomocí uložené zásady přístupu a podpisy sdíleného přístupu. Vám nabídneme SAS úrovně služby a SAS účtu úrovni. Také ukážeme, jak toolimit přistupovat k tooa konkrétní IP adresu (nebo rozsah IP adres), jak použít protokol hello toolimit tooHTTPS a jak toorevoke a sdíleného přístupového podpisu bez čekání na jeho tooexpire.

## <a name="encryption-in-transit"></a>Šifrování během přenosu
Tato část pojednává o tom, jak toosecure dat při přenosu do nebo z Azure Storage. Budeme mluvit o hello doporučuje použití protokolu HTTPS a hello šifrování používá protokol SMB 3.0 pro sdílené složky Azure File. Také jsme bude trvat podívejte se na straně klienta šifrování, které vám umožní tooencrypt hello data předtím, než bude převedena do úložiště v aplikaci klienta a toodecrypt hello data, jakmile se přenáší z úložiště.

## <a name="encryption-at-rest"></a>Šifrování v klidovém stavu
Bude faktorech mluvíme o šifrování služby úložiště (SSE) a jak můžete ji povolit pro účet úložiště, což vede k objektů BLOB bloku, objekty BLOB stránky a doplňovací objekty BLOB se šifrují automaticky, když zapsána tooAzure úložiště. Podíváme se také na tom, jak můžete použít Azure Disk Encryption a prozkoumat základní rozdíly hello a případů šifrování disku versus SSE versus šifrování na straně klienta. Podíváme se stručně na soulad se standardem FIPS pro USA Počítače, Government.

* [Průvodce zabezpečením služby Azure Storage](../../storage/common/storage-security-guide.md)

## <a name="temporary-disk"></a>Dočasné disku
Každý virtuální počítač obsahuje dočasné disk. Hello dočasným diskovým poskytuje krátkodobé úložiště pro aplikace a procesy a je určený tooonly úložiště dat jako jsou soubory stránky nebo odkládacího souboru. Data na dočasné disku hello mohou být ztraceny při [údržby](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) nebo když jste [znovu nasadit virtuální počítač](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Během restartu standardní hello virtuální počítač by měl zachovat hello data na dočasné jednotce hello.

Na virtuální počítače s Linuxem, je obvykle hello disku **/dev/sdb** a je naformátován a připojit příliš**/mnt** podle hello Azure Linux Agent. velikost Hello hello dočasné disku se liší, na základě velikosti hello hello virtuálního počítače. Další informace najdete v tématu [velikosti virtuálních počítačů Linux](sizes.md).

Další informace o tom, jak Azure používá dočasným diskovým hello najdete v tématu [pochopení hello jednotku ve virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="cost-savings"></a>Úspora nákladů
* [Náklady na úložiště](https://azure.microsoft.com/pricing/details/storage/)
* [Kalkulačky úložiště náklady.](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Limity úložiště
* [Omezení služby úložiště](../../azure-subscription-service-limits.md#storage-limits)
