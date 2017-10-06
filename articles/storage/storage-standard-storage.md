---
title: "na základě aaaHD finančně efektivní standardní úložiště a disky virtuálních počítačů Azure | Microsoft Docs"
description: "Prodiskutujte finančně efektivní standardní úložiště a disky virtuálních počítačů a nespravované."
services: storage
documentationcenter: 
author: yuemlu
manager: aungoo-msft
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: yuemlu
ms.openlocfilehash: c454c61254c6b160bdf2cd39ea3319452e3e4898
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cost-effective-standard-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Nákladově efektivní úložiště Standard a nespravované a spravované disky virtuálních počítačů Azure

Azure Standard Storage nabízí podporu spolehlivou a cenově disků pro virtuální počítače spuštěné úlohy kterému latence nevadí. Podporuje také objekty BLOB, tabulky, fronty a soubory. S použitím standardního úložiště je hello data uložená na pevných disků (HDD). Při práci s virtuálními počítači, můžete použít disky standardní úložiště pro scénáře vývoje/testování a méně důležité úlohy a prémiové disky úložiště pro kritické výrobní aplikace. Standardní úložiště je k dispozici ve všech oblastech Azure. 

Tento článek se soustředí na používání hello standardní úložiště pro disky virtuálních počítačů. Další informace o použití hello úložiště s objekty BLOB, tabulky, fronty a soubory, naleznete v toohello [Úvod tooStorage](storage-introduction.md).

## <a name="disk-types"></a>Typy disků

Existují dva způsoby toocreate standardní disky pro virtuální počítače Azure:

**Nespravované disky**: Toto je hello původní metody, kde budete spravovat hello úložiště účtů používaných toostore hello souborů VHD, které odpovídají toohello disky virtuálních počítačů. Soubory VHD jsou uloženy jako objekty BLOB stránky v účtech úložiště. Nespravované disků může být připojené tooany velikost virtuálního počítače Azure, včetně hello virtuálních počítačů, které především používat úložiště úrovně Premium, jako je například hello DSv2 a GS řady. Virtuální počítače Azure podporují připojení několik standardní disky, což až too256 TB místa na virtuální počítač.

[**Disky systému Azure spravované**](storage-managed-disks-overview.md): Tato funkce spravuje hello účty úložiště pro vás použil pro hello disky virtuálních počítačů. Zadejte typ hello (Standard nebo Premium) a velikost disku je nutné, a Azure vytváří a spravuje hello disku za vás. Nemáte tooworry o uvedení hello disky napříč více účtů úložiště v pořadí tooensure, že zůstat v hello limitech škálovatelnosti pro účty úložiště hello – Azure zpracovává, který pro vás.

Přestože oba typy disků jsou k dispozici, doporučujeme používat spravované disky tootake výhod jejich mnoha funkcí.

tooget začít s Azure Standard Storage, navštivte [začněte zadarmo](https://azure.microsoft.com/pricing/free-trial/). 

Informace o tom, jak toocreate virtuální počítač s spravované disky prosím najdete v jednom z hello následující články.

* [Vytvoření virtuálního počítače pomocí Resource Manageru a PowerShellu](../virtual-machines/virtual-machines-windows-ps-create.md)
* [Vytvoření virtuálního počítače s Linuxem pomocí hello 2.0 rozhraní příkazového řádku Azure](../virtual-machines/linux/quick-create-cli.md)

## <a name="standard-storage-features"></a>Funkce úložiště Standard Storage 

Podívejme se na některé z funkcí hello úložiště Standard Storage. Další podrobnosti najdete v tématu [Úvod tooAzure úložiště](storage-introduction.md).

**Standardní úložiště**: Azure Standard Storage podporuje disky Azure, objektů BLOB služby Azure, úložiště Azure File, Azure tabulek a front Azure. služby Standard Storage toouse, začínat [vytvoření účtu úložiště Azure](storage-create-storage-account.md#create-a-storage-account).

**Disky úložiště Standard storage:** standardní úložiště může být disky připojené virtuální počítače Azure tooall včetně velikost řady virtuálních počítačů použít s Storage úrovně Premium například hello DSv2 a GS řady. Disk standardního úložiště může být pouze připojené tooone virtuálních počítačů. Můžete však připojit nejméně jeden z těchto tooa disky virtuálních počítačů, až toohello disku maximální počet definované pro velikost tohoto virtuálního počítače. V následující části na standardní úložiště škálovatelnost a cíle výkonnosti hello jsme popíše hello specifikace podrobněji. 

**Objekt blob stránky standardní**: objekty BLOB standardní stránky jsou použité toohold trvalé disky pro virtuální počítače a také být přístup přímo prostřednictvím REST podobně jako ostatní typy objektů BLOB Azure. [Objekty BLOB stránek](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) jsou kolekce 512 bajtů stránky optimalizované pro náhodné čtení a zápisu operace. 

**Replikace úložiště:** ve většině oblastí, data ve standardní účet úložiště může být místně replikované nebo geograficky replikované v několika datových centrech. Hello čtyři typy replikace, které jsou k dispozici jsou místně redundantní úložiště (LRS), Zónově redundantní úložiště (ZRS), geograficky redundantní úložiště (GRS) a geograficky redundantní úložiště s přístupem pro čtení (RA-GRS). Spravované disků ve standardní úložiště aktuálně pouze podporují místně redundantní úložiště (LRS). Další informace najdete v tématu [replikace úložiště](storage-redundancy.md).

## <a name="scalability-and-performance-targets"></a>Cíle škálovatelnost a výkonnosti

V této části jsme se popisují hello škálovatelnosti a cílech výkonnosti potřebujete tooconsider při použití standardní úložiště.

### <a name="account-limits--does-not-apply-toomanaged-disks"></a>Limity účtu – doporučení se netýká toomanaged disky

| **Prostředek** | **Výchozí omezení** |
|--------------|-------------------|
| TB na účet úložiště  | 500 TB |
| Příjem příchozích dat Max<sup>1</sup> na účet úložiště (nám oblastech) | 10 GB/s Pokud je povoleno GRS nebo ZRS, 20 GB/s pro LRS |
| Maximální počet odchozí<sup>1</sup> na účet úložiště (nám oblastech) | 20 GB/s Pokud je povoleno RA-GRS nebo GRS nebo ZRS, 30 GB/s pro LRS |
| Příjem příchozích dat Max<sup>1</sup> na účet úložiště (Evropské a asijské oblasti) | 5 GB/s Pokud je povoleno GRS nebo ZRS, 10 GB/s pro LRS |
| Maximální počet odchozí<sup>1</sup> na účet úložiště (Evropské a asijské oblasti) | 10 GB/s Pokud je povoleno RA-GRS nebo GRS nebo ZRS, 15 GB/s pro LRS |
| Celkový počet požadavků (za předpokladu, že velikost objektu 1 KB) na účet úložiště | Too20, 000 IOPS, entity za sekundu nebo zpráv za sekundu |

<sup>1</sup> příjem příchozích dat odkazuje tooall dat (počet požadavků) odesílány tooa účet úložiště. Odchozí odkazuje tooall dat (odpovědí) přijatých z účtu úložiště.

Další informace najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md).

Překročí-li hello musí aplikace hello cíle škálovatelnosti účtu jedno úložiště, sestavení vaší aplikace toouse více účtů úložiště a oddílu data mezi různými tyto účty úložiště. Alternativně můžete spravovat disky Azure a Azure budou spravovat hello vytváření oddílů a umístění vašich dat za vás.

### <a name="standard-disks-limits"></a>Standardní disky omezení

Na rozdíl od prémiové disky nebyly zřízené hello vstupně výstupních operací za sekundu (IOPS) a propustnost (šířka pásma) standardní disky. Hello výkon standardní disky se liší podle hello virtuální počítač je připojený disk hello toowhich velikosti, není toohello velikost disku hello. Očekáváte může tooachieve až toohello omezení výkonu uvedené v tabulce hello.

**Standardní disky omezení (spravovaných a nespravovaných)**

| **Vrstvu virtuálního počítače**            | **Základní úroveň virtuálního počítače** | **Standardní vrstvy virtuálního počítače** |
|------------------------|-------------------|----------------------|
| Velikost disku maximální          | 4095 GB           | 4095 GB              |
| Max. 8 KB IOPS na disk | Až too300         | Až too500            |
| Maximální šířka pásma na disku | Až too60 MB/s     | Až too60 MB/s        |

Pokud vaše úlohy vyžadují podporu disků vysoce výkonné, nízkou latencí, měli byste zvážit použití služby Premium Storage. tooknow další výhody úložiště Premium Storage najdete [vysoce výkonné úložiště Premium a disky virtuálních počítačů Azure](storage-premium-storage.md). 

## <a name="snapshots-and-copy-blob"></a>Snímků a kopírování objektů blob

toohello služba úložiště, soubor virtuálního pevného disku hello je objekt blob stránky. Můžete pořizovat snímky objekty BLOB stránky a zkopírujte je tooanother umístění, jako například jiný účet úložiště.

### <a name="unmanaged-disks"></a>Nespravované disky

Můžete vytvořit [přírůstkové snímky](storage-incremental-snapshots.md) pro nespravovaná standardní disky v hello stejný způsob, jak použít snímky s standardní úložiště. Doporučujeme vytvářet snímky a poté zkopírujte tyto snímky tooa geograficky redundantní standardní účet úložiště, pokud váš zdrojový disk je v účtu místně redundantní úložiště. Další informace najdete v tématu [možnosti redundance úložiště Azure](storage-redundancy.md).

Pokud disk připojené tooa virtuálních počítačů, nejsou povoleny určité operace rozhraní API na discích hello. Například nelze provést [kopírovat objekt Blob](/rest/api/storageservices/Copy-Blob) operace u tohoto objektu blob, pokud je hello disk připojena tooa virtuálních počítačů. Místo toho nejprve vytvořte snímek tomuto objektu blob pomocí hello [snímku Blob](/rest/api/storageservices/Snapshot-Blob) REST API metoda a potom proveďte hello [kopírovat objekt Blob](/rest/api/storageservices/Copy-Blob) disk připojil hello snímku toocopy hello. Alternativně můžete odpojit hello disk a potom proveďte všechny potřebné operace.

geograficky redundantní kopie toomaintain vaše snímků, snímky můžete zkopírovat z místně redundantního úložiště účet tooa geograficky redundantní standardní účet úložiště pomocí nástroje AzCopy nebo kopírovat objekt Blob. Další informace najdete v tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md) a [kopírovat objekt Blob](/rest/api/storageservices/Copy-Blob).

Podrobné informace o provádění operací REST pro objekty BLOB stránky v účtech úložiště standard storage najdete v tématu [Azure Storage Services REST API](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

### <a name="managed-disks"></a>Managed Disks

Snímek se spravovaným diskem je jen pro čtení kopie hello spravovaného disku, který je uložený jako standardní se spravovaným diskem. Přírůstkové snímky nejsou aktuálně podporovány pro spravované disky, ale bude podporovaný v budoucnu hello.

Pokud se spravovaným diskem připojené tooa virtuálních počítačů, nejsou povoleny určité operace rozhraní API na discích hello. Například nelze generovat sdílený přístupový podpis (SAS) tooperform operace kopírování při hello disku připojené tooa virtuálních počítačů. Místo toho nejprve vytvořte snímek hello disku a pak proveďte hello kopii hello snímku. Alternativně můžete odpojit hello disk a pak generovat operace kopírování hello tooperform sdílený přístupový podpis (SAS).

## <a name="pricing-and-billing"></a>Ceny a fakturace

Při použití standardní úložiště, hello fakturace, platí následující aspekty:

* Velikost úložiště Standard storage nespravované disky nebo dat 
* Standardní disky spravované
* Standardní úložiště snímků
* Přenosy odchozích dat
* Transakce

**Nespravované velikost úložiště dat a disku:** pro nespravovaná disky a další data (objekty BLOB, tabulky, fronty a soubory), budou se vám účtovat pouze pro hello množství místa, kterou používáte. Například, pokud máte virtuální počítač, jehož objekt blob stránky je zřízená jako 127 GB, ale hello virtuální počítač je ve skutečnosti jenom pomocí 10 GB místa, budou platit pro 10 GB místa. Podporujeme standardní úložiště až too8191 GB a standardní disky nespravované až too4095 GB. 

**Spravované disky:** spravované disky se účtují na velikosti hello zřízený. Pokud disk je zřízená jako 10 GB místa na disku a používáte pouze 5 GB, je stále účtovat pro hello zřídit velikost 10 GB.

**Snímky**: snímky standardní disky se účtují hello používá hello snímky dodatečnou kapacitu. Informace o snímků, najdete v části [vytvoření snímku objektu Blob](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

**Odchozí přenosy dat**: [odchozí přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/) (data přejdete mimo datových center Azure) jsou zpoplatněné podle využití šířky pásma.

**Transakce**: Azure účtuje 0.0036 na 100 000 transakcí pro standardní úložiště. Transakce zahrnout oprávnění ke čtení a zápisu toostorage operace.

Podrobné informace o cenách pro standardní úložiště, virtuální počítače a spravovat disky najdete v části:

* [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/)
* [Ceny virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Spravované disky ceny](https://azure.microsoft.com/pricing/details/managed-disks)

## <a name="azure-backup-service-support"></a>Podporu služby Azure Backup 

Virtuální počítače s nespravované disky lze zálohovat pomocí služby Azure Backup. [Další podrobnosti](../backup/backup-azure-vms-first-look-arm.md).

Můžete taky hello služba Azure Backup s spravované disky toocreate úlohu zálohování s zálohy založené na čase, snadno obnovení virtuálních počítačů a zásady uchovávání záloh. Můžete získat další informace v [služby pomocí zálohování Azure pro virtuální počítače s spravované disky](../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="next-steps"></a>Další kroky

* [Úvod tooAzure úložiště](storage-introduction.md)

* [Vytvoření účtu úložiště](storage-create-storage-account.md)

* [Přehled služby Managed Disks](storage-managed-disks-overview.md)

* [Vytvoření virtuálního počítače pomocí Resource Manageru a PowerShellu](../virtual-machines/virtual-machines-windows-ps-create.md)

* [Vytvoření virtuálního počítače s Linuxem pomocí hello 2.0 rozhraní příkazového řádku Azure](../virtual-machines/linux/quick-create-cli.md)
