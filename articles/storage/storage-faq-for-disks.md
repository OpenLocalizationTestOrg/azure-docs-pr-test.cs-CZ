---
title: "Nejčastější dotazy (FAQ) o disky virtuálních počítačů Azure IaaS | Microsoft Docs"
description: "Nejčastější dotazy týkající se disky virtuálního počítače Azure IaaS a prémiové disky (spravovaných a nespravovaných)"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 31d0aa67b6ca58b75b432ae94f93ebcf6d730380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a>Nejčastější dotazy týkající se disky virtuálního počítače Azure IaaS a spravovanými a nespravovanými prémiové disky

Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se Azure spravované disky a Azure Premium Storage.

## <a name="managed-disks"></a>Managed Disks

**Co je Azure spravované disky?**

Spravované disků je funkce, která zjednodušuje Správa disku pro virtuální počítače Azure IaaS tím, že zpracování Správa účtů úložiště pro vás. Další informace najdete v tématu hello [spravované disky přehled](storage-managed-disks-overview.md).

**Když vytvořím standardní se spravovaným diskem z existujícího VHD, který je 80 GB, jaké budou který náklady mi?**

Standardní se spravovaným diskem vytvořen z disku VHD 80 GB je považována za hello další dostupné standardní velikost disku, který je k dispozici S10 disk. Se vám účtovat podle toohello S10 disku ceny. Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).

**Existují žádné transakce náklady na standardní spravované disky?**

Ano. Vám účtovat každou transakci. Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).

**Pro standardní se spravovaným diskem I odečte hello skutečná velikost hello dat na disku hello nebo hello zřídit kapacitu disku hello?**

Že se vám účtovat podle hello zřídit kapacitu disku hello. Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).

**Jak je ceny disků premium spravované liší od nespravované disky?**

Hello ceny disků spravované premium je hello stejné jako nespravované prémiové disky.

**Můžete změnit hello typ účtu úložiště (Standard nebo Premium) Moje spravovaných disků?**

Ano. Typ účtu úložiště hello spravované disky můžete změnit pomocí hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure.

**Existuje způsob, že umožní kopírování a exportovat účet spravovaných disků na tooa privátní úložiště?**

Ano. Vaše spravované disky můžete exportovat pomocí hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure.

**Můžete použít soubor virtuálního pevného disku v toocreate účet úložiště Azure se spravovaným diskem s jiného předplatného?**

Ne.

**Můžete použít soubor virtuálního pevného disku v toocreate účet úložiště Azure se spravovaným diskem v jiné oblasti.**

Ne.

**Existují nějaká omezení škálování pro zákazníky, kteří používají spravované disky?**

Spravované disky eliminuje hello omezení spojená s účty úložiště. Hello počet spravovaných disků na jedno předplatné, je však omezená too2 000 ve výchozím nastavení. Podpora tooincrease můžete volat toto číslo.

**Může trvat přírůstkový snímek spravovaného disku?**

Ne. aktuální vytváření snímků Hello provede úplnou kopii se spravovaným diskem. Jsme ale plánování toosupport přírůstkové snímky v budoucnu hello.

**Virtuální počítače v nastavení dostupnosti se může skládat z kombinace spravovanými a nespravovanými disky?**

Ne. Hello virtuálních počítačů v nastavení dostupnosti musí používat disky všechny spravované nebo nespravované všechny disky. Když vytvoříte skupinu dostupnosti, můžete typu disky, kterého chcete toouse.

**Spravované disky hello výchozí možnost je ve hello portál Azure?**

Aktuálně ale bude výchozí hello v budoucnu hello.

**Můžete vytvořit prázdný disk spravované?**

Ano. Můžete vytvořit prázdný disk. Spravovaný disk lze vytvořit nezávisle na virtuální počítač, například bez jeho připojení tooa virtuálních počítačů.

**Co je podporováno hello počet domén selhání pro skupinu dostupnosti, který používá disky spravované?**

V závislosti na hello oblasti, kde se nachází skupina dostupnosti hello, který používá disky spravovat je počet domén selhání hello podporované 2 nebo 3.

**Jak je hello standardní účet úložiště pro diagnostiku nastavení?**

Nastavit účet privátní úložiště pro diagnostiku virtuálního počítače. V budoucích hello, plánujeme tooswitch diagnostiky i tooManaged disky.

**Jaký druh řízení přístupu na základě Role podpora je k dispozici pro spravované disky?**

Spravované disky podporuje tři klíčové výchozích rolí:

* Vlastník: Můžou spravovat všechno včetně přístupu
* Přispěvatel: Můžou spravovat všechno kromě přístupu
* Čtečka: Zobrazit vše, co, ale nelze provádět změny

**Existuje způsob, že umožní kopírování a exportovat účet spravovaných disků na tooa privátní úložiště?**

Jen pro čtení sdíleného přístupového podpisu identifikátor URI pro hello spravované disk a použít ho můžete získat toocopy hello obsah tooa privátní úložiště účet nebo místní úložiště.

**Můžete vytvořit kopii Moje spravovaného disku?**

Zákazníci můžete pořízení snímku jejich spravované disky a pak použít hello snímku toocreate spravovaných disků na jiný.

**Jsou nespravované disky stále podporovány?**

Ano. Podporujeme nespravované a spravované disky. Doporučujeme používat spravované disky pro nové úlohy a vaše aktuální toomanaged disky úlohy migrace.


**Je-li vytvořit 128 GB disk a poté zvýšit hello velikost too130 GB, bude I vám účtována hello další velikost disku (512 GB)?**

Ano.

**Můžete vytvořit místně redundantní úložiště, geograficky redundantní úložiště, a zónově redundantní úložiště spravované disky?**

Disky systému Azure spravované aktuálně podporuje pouze místně redundantního úložiště spravované disky.

**Můžete zmenšit nebo downsize Moje spravované disky?**

Ne. Tato funkce není aktuálně podporován. 

**Můžete změnit vlastnost název počítače hello, pokud specializované (ne vytvořili pomocí nástroje pro přípravu systému hello nebo zobecněn) operačního systému disku jsou použité tooprovision virtuálního počítače?**

Ne. Nelze aktualizovat vlastnosti název počítače hello. Hello nový virtuální počítač zdědí ho hello nadřazený virtuální počítač, který byl disk operačního systému použít toocreate hello. 

**Kde najdu ukázka Azure Resource Manager šablony toocreate virtuálních počítačů s spravované disky?**
* [Seznam šablon pomocí spravovaných disků](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

## <a name="managed-disks-and-storage-service-encryption"></a>Spravované disky a šifrování služby úložiště 

**Šifrování služby úložiště Azure ve výchozím nastavení zapnutá po vytvoření se spravovaným diskem?**

Ano.

**Kdo spravuje hello šifrovacích klíčů?**

Spravuje Microsoft hello šifrovací klíče.

**Můžete zakázat šifrování služby úložiště pro moje spravované disky?**

Ne.

**Je šifrování služby úložiště k dispozici pouze v určitých oblastí?**

Ne. Je k dispozici ve všech oblastech hello, kde je k dispozici spravované disky. Spravované disků je k dispozici ve všech veřejných oblastí a Německu.

**Jak můžete zjistit, pokud je zašifrovaná Moje spravovaných disků?**

Můžete zjistit hello čas vytvoření spravovaného disku z hello portálu Azure, hello rozhraní příkazového řádku Azure a prostředí PowerShell. Pokud je doba hello po 9. června 2017, se šifrují na disku. 

**Jak můžete šifrovat Můj stávající disky, které byly vytvořeny před 10 června 2017?**

Od verze 10 června 2017 je nová data tooexisting spravované disky šifrují automaticky. Můžeme také plánování tooencrypt stávající data a šifrování hello proběhne asynchronně hello pozadí. Pokud je nutné šifrovat všechna existující data, vytvoří se kopie disku. Bude se šifrovat nové disky.

* [Kopírovat spravovaných disků pomocí hello rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)
* [Zkopírujte spravované disky pomocí prostředí PowerShell](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

**Jsou spravované snímky a bitové kopie zašifrovaná?**

Ano. Všechny spravované snímky a bitové kopie vytvořené po 9. června 2017, se šifrují automaticky. 

**Můžete převést virtuální počítače s nespravované disky, které jsou umístěny v účtech úložiště, které jsou nebo byly dříve šifrované toomanaged disky?**

Ano

**Budou exportovaného virtuálního pevného disku z spravovaného disku nebo snímek také zašifrovaná?**

Ne. Pokud exportujete tooan virtuálního pevného disku, ale šifrované účet úložiště z šifrované spravovaného disku nebo snímek a potom je zašifrovaná. 

## <a name="premium-disks-managed-and-unmanaged"></a>Pro prémiové disky: spravovaných a nespravovaných

**Pokud virtuální počítač používá velikost série, která podporuje službu Premium Storage, jako je například DSv2, můžu připojit premium a standard datové disky?** 

Ano.

**Můžete připojit premium a standard data disky tooa velikost řady, která nepodporuje Storage úrovně Premium, jako je například D Dv2, G nebo F řady?**

Ne. Můžete připojit pouze disky tooVMs standardní data, která nepoužívají velikost série, která podporuje službu Premium Storage.

**Když vytvořím premium datový disk z existujícího VHD, který byl 80 GB, kolik bude která stojí?**

Datový disk premium vytvořen z disku VHD 80 GB je považována za hello k dispozici další premium disku velikost, která je P10 disk. Se vám účtovat podle toohello P10 disku ceny.

**Existují transakce náklady toouse Premium Storage?**

Není opravené náklady pro každou velikost disku, která pochází zřízené pomocí omezení na IOPS a propustnosti. Hello další náklady jsou šířky odchozího pásma a kapacity snímku, pokud je k dispozici. Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).

**Jaká jsou omezení hello IOPS a propustnosti, kterou můžete dostat z hello diskové mezipaměti?**

Hello kombinované omezení pro mezipaměť a místní SSD pro řady DS 4000 IOPS na základní a 33 MB za sekundu za jádra. Hello GS řady nabízí 5 000 IOPS na základní a 50 MB za sekundu za jádra.

**Je hello nepodporuje místní SSD pro virtuální počítač spravovaný disky?**

Hello místní SSD je dočasné úložiště, která je součástí spravované virtuální disky. Existuje nejsou zpoplatněné pro toto dočasný úložiště. Doporučujeme vám, že nepoužijete tento místní SSD toostore data aplikací protože není trvalý v Azure Blob storage.

**Jsou existuje žádné dopady na hello použít Trim na prémiové disky?**

Neexistuje žádné nevýhodou použití toohello TRIM na disky Azure na buď premium nebo standardní disky.

## <a name="new-disk-sizes-managed-and-unmanaged"></a>Nové velikosti disků: spravovaných a nespravovaných

**Co je největší velikost disku hello podporovaná pro operační systém a datové disky?**

Typ Hello oddílu, který podporuje Azure pro disk operačního systému je hello hlavní spouštěcí záznam (MBR). formát MBR Hello podporuje velikost disku až too2 TB. Hello největší velikost, který podporuje Azure pro disk operačního systému je 2 TB. Azure podporuje až too4 TB pro datové disky. 

**Co je hello největší stránky velikost objektu blob podporovanou?**

Hello největší stránky velikost objektu blob podporující Azure je 8 TB (8 191 GB). Nepodporujeme objekty BLOB stránky větší než 4 TB (4095 GB) připojené tooa virtuálního počítače jako data nebo disky operačního systému.

**Potřebuji toouse novou verzí z nástroje Azure toocreate připojit, přizpůsobit a odeslat disky, které jsou větší než 1 TB?**

Nepotřebujete tooupgrade vaší existující toocreate nástroje Azure, připojit nebo změna velikosti disků, které jsou větší než 1 TB. tooupload svůj disk VHD souboru z místní přímo tooAzure jako objekt blob stránky nebo nespravované disku, bude nutné toouse hello nejnovější sady nástrojů:

|Nástroje Azure      | Podporované verze                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | Číslo verze 4.1.0: červen 2017 verzi nebo novější|
|Rozhraní příkazového řádku Azure v1     | Číslo verze 0.10.13: pravděpodobně 2017 verzi nebo novější|
|AzCopy           | Číslo verze 6.1.0: červen 2017 verzi nebo novější|

Podpora Hello v2 rozhraní příkazového řádku Azure a Azure Storage Explorer tu bude brzo dostupná. 

**Jsou podporovány P4 a P6 velikosti disků pro nespravovaná disky nebo objekty BLOB stránky?**

Ne. P4 (32 GB) a P6 velikosti disků (64 GB) jsou podporovány pouze u spravovaných disky. Podpora pro nespravovaná disky a objekty BLOB stránky bude brzo.

**Pokud mé existující premium spravované disku menší než 64 GB byl vytvořen před povolením malý disk hello (přibližně 15. června 2017), jak se jeho fakturuje?**

Stávající malých prémiové disky menší než 64 GB pokračovat toobe účtují podle toohello P10 cenovou úroveň. 

**Jak můžete přepnout hello disku úroveň malé prémiové disky menší než 64 GB z P10 tooP4 nebo P6?**

Může pořízení snímku menší disky a poté vytvořit hello disku tooautomatically přepínač cenová úroveň tooP4 nebo P6 podle velikosti hello zřízený. 


## <a name="what-if-my-question-isnt-answered-here"></a>Co když není zde odpovědi můj dotaz?

Pokud váš dotaz není zde uvedeno, dejte nám vědět, a pomůžeme vám najít odpověď. V komentářích hello můžete odeslat dotaz na konci hello tohoto článku. tooengage týmem hello Azure Storage a ostatními členy komunity o tomto článku použít hello MSDN [fórum pro Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).

Funkce toorequest odeslání vaší žádosti a nápady toohello [fóru pro zpětnou vazbu Azure Storage](https://feedback.azure.com/forums/217298-storage).
