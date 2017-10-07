---
title: "aaaAzure úložiště škálovatelnost a cíle výkonnosti | Microsoft Docs"
description: "Další informace o hello škálovatelnosti a cílech výkonnosti pro Azure Storage, včetně kapacitu, rychlost požadavků a příchozí a odchozí šířky pásma pro oba účty úložiště standard a premium. Pochopení cíle výkonnosti pro oddíly v rámci každé ze služby Azure Storage hello."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: be721bd3-159f-40a1-88c1-96418537fe75
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 07/12/2017
ms.author: robinsh
ms.openlocfilehash: 7afe4366a02887b4e3d9781c26c8adda81adce95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Škálovatelnost a výkonnostní cíle Azure Storage
## <a name="overview"></a>Přehled
Toto téma popisuje témata hello škálovatelnost a výkon pro Microsoft Azure Storage. Souhrn další omezení Azure, najdete v části [předplatné Azure a omezení služby, kvóty a omezení](../../azure-subscription-service-limits.md).

> [!NOTE]
> Všechny účty úložiště spustit na hello nové ploché síťové topologie a podporovat hello škálovatelnosti a cílech výkonnosti uvedené níže, bez ohledu na to, kdy byly vytvořeny. Další informace o architektura ploché sítě hello úložiště Azure a o škálovatelnost, najdete v části [Microsoft Azure Storage: A vysoce dostupné cloudového úložiště se silnou konzistencí](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 
> [!IMPORTANT]
> Hello škálovatelnosti a cílech výkonnosti tady jsou vyšší kategorie cíle, ale jsou dosažitelné. Ve všech případech se žádost o hello rychlost a šířku pásma pomocí svého účtu úložiště závisí na velikosti hello objekty uložené, hello přístupové vzorce využité a hello typ úlohy, které aplikace provádí. Být zda tootest toodetermine vaší služby, zda jeho výkon vyhovovat vašim požadavkům. Pokud je to možné Vyhněte se nečekané špičky hello rychlost přenosů do a zajistěte, aby byl provoz dobře distribuované napříč oddíly.
> 
> Když aplikace dosáhne limitu hello co dokáže zpracovat oddíl pro úlohy, Azure Storage bude zahájena tooreturn kód chyby 503 (zaneprázdněný Server) nebo kód chyby 500 odpovědí (časový limit operace). V takovém případě by měl hello aplikace pomocí zásady exponenciálního omezení rychlosti pro opakování. exponenciálního omezení rychlosti Hello umožňuje hello zatížení hello oddílu toodecrease a tooease out špičky v oddílu toothat provoz.
> 
> 

Překročí-li hello musí aplikace hello cíle škálovatelnosti účtu jedno úložiště, můžete vytvořit více účtů úložiště toouse vaší aplikace a oddílu datových objektů mezi různými tyto účty úložiště. V tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/) informace o cenách.

## <a name="scalability-targets-for-blobs-queues-tables-and-files"></a>Cíle škálovatelnosti pro objekty BLOB, fronty, tabulky a soubory
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies toounmanaged and managed -->
## <a name="scalability-targets-for-virtual-machine-disks"></a>Cíle škálovatelnosti pro disky virtuálního počítače
[!INCLUDE [azure-storage-limits-vm-disks](../../../includes/azure-storage-limits-vm-disks.md)]

V tématu [velikosti virtuálních počítačů Windows](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [velikosti virtuálního počítače s Linuxem](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) další podrobnosti.

## <a name="managed-virtual-machine-disks"></a>Disky spravovaných virtuálních počítačů

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../../includes/azure-storage-limits-vm-disks-managed.md)]

## <a name="unmanaged-virtual-machine-disks"></a>Disky nespravované virtuálního počítače
[!INCLUDE [azure-storage-limits-vm-disks-standard](../../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="scalability-targets-for-azure-resource-manager"></a>Cíle škálovatelnosti pro Azure resource Manageru
[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="partitions-in-azure-storage"></a>Oddíly v úložišti Azure
Každý objekt, který obsahuje data, která je uložená ve službě Azure Storage (objekty BLOB, zprávy, entit a soubory) patří tooa oddílu a je identifikována klíč oddílu. oddíl Hello Určuje, jak vyrovnává zatížení Azure Storage objekty BLOB, zprávy, entit a soubory mezi servery toomeet hello provoz potřebám těchto objektů. Hello klíč oddílu je jedinečná a nemá použité toolocate objektů blob, zprávy nebo entity.

Tabulka Hello výše uvedeném v [cíle škálovatelnosti pro standardní účty úložiště](#standard-storage-accounts) seznamy hello výkonnostní cíle pro jeden oddíl pro každou službu.

Oddíly ovlivnit Vyrovnávání zatížení a škálovatelnost pro jednotlivé služby úložiště hello hello následující způsoby:

* **Objekty BLOB**: hello klíč oddílu pro objekt blob je název účtu + název kontejneru + název objektu blob. To znamená, že každý objekt blob může mít vlastní oddíl zatížení u objektu blob hello vyžaduje, je-li. Objekty BLOB mohou být distribuovány na mnoha serverech v pořadí tooscale out toothem přístup, ale jediného objektu blob může obsluhovat pouze jeden server. Zatímco objekty BLOB mohou být logicky seskupeny do kontejnery objektů blob, neexistují žádné rozdělení dopady z toto seskupení.
* **Soubory**: klíč oddílu hello souboru je účet název + soubor název sdílené složky. To znamená, že všechny soubory ve sdílené složce jsou také v jeden oddíl.
* **Zprávy**: klíč oddílu hello zprávy je název účtu hello + název fronty, takže všechny zprávy ve frontě jsou seskupené do jednoho oddílu a zpracovává jeden server. Jiné fronty může zpracovávat různé servery toobalance hello načíst pro ale mnoho fronty může mít účet úložiště.
* **Entity**: hello klíč oddílu pro entitu je název účtu a název tabulky + klíč oddílu, kde hello klíč oddílu je hodnota hello hello požadované uživatelem definované **PartitionKey** vlastnost pro hello entity. Všechny entity se stejnou hodnotou klíče oddílu jsou seskupené do hello stejné oddílu a zpracovává hello stejné hello oddílu serveru. Jde důležité toounderstand bod při navrhování vaší aplikace. Aplikace by měla vyvážit výhody škálovatelnost hello šíření entity napříč více oddílů s hello data přístup výhody seskupování entity v jeden oddíl.  

Klíčové výhody toogrouping sady entit v tabulce do jednoho oddílu je, že je možné tooperform atomic dávkových operací napříč entity v hello stejné oddílu, protože oddíl existuje na jednom serveru. Proto, pokud chcete tooperform dávkových operací ve skupině entit, vezměte v úvahu je seskupit pomocí hello stejným klíčem oddílu. 

Na dobrý den druhé straně, entit, které jsou ve stejné tabulce, tabulka však mají různé klíče oddílů hello může být vyrovnáváno zatížení napříč různými servery, takže je možné toohave větší škálovatelnost.

Doporučení k návrhu, vytváření oddílů strategie pro tabulky naleznete podrobné [zde](https://msdn.microsoft.com/library/azure/hh508997.aspx).

## <a name="see-also"></a>Viz také
* [Podrobnosti o cenách úložiště](https://azure.microsoft.com/pricing/details/storage/)
* [Předplatné Azure a omezení služby, kvóty a omezení](../../azure-subscription-service-limits.md)
* [Storage úrovně Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../storage-premium-storage.md)
* [Replikace Azure Storage](../storage-redundancy.md)
* [Microsoft Azure Storage výkon a škálovatelnost kontrolní seznam](../storage-performance-checklist.md)
* [Microsoft Azure Storage: Vysoce dostupný cloudové úložiště služby se silnou konzistenci](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

