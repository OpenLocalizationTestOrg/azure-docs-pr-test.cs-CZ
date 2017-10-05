---
title: "Škálovatelnost a cíle výkonnosti Azure Storage | Microsoft Docs"
description: "Další informace o škálovatelnosti a výkonu cíle pro Azure Storage, včetně kapacitu, rychlost požadavků a příchozí a odchozí šířky pásma pro oba účty úložiště standard a premium. Pochopení cíle výkonnosti pro oddíly v rámci každé ze služby Azure Storage."
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
ms.openlocfilehash: ed90e5d63e4c93f9c5054b02d2b4457b44caf6eb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Škálovatelnost a výkonnostní cíle Azure Storage
## <a name="overview"></a>Přehled
Toto téma popisuje témata škálovatelnost a výkon pro Microsoft Azure Storage. Souhrn další omezení Azure, najdete v části [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).

> [!NOTE]
> Všechny účty úložiště spustit na nové ploché síťové topologie a podporovat škálovatelnost a výkon cíle, které jsou uvedené níže, bez ohledu na to, kdy byly vytvořeny. Další informace o architektuře ploché sítě Azure Storage a o škálovatelnost, najdete v části [Microsoft Azure Storage: A vysoce dostupné cloudového úložiště se silnou konzistencí](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 
> [!IMPORTANT]
> Škálovatelnost a výkon cíle, které jsou zde uvedeny vyšší kategorie cíle, ale jsou dosažitelné. Ve všech případech, rychlost požadavků a šířku pásma vašeho úložiště účet závisí na velikosti objekty uložené, vzory přístupu k použity, a typ úlohy aplikace provede. Ujistěte se, že testovací služby k určení, zda jeho výkon vyhovovat vašim požadavkům. Pokud je to možné vyhnout nečekané špičky v míru přenosu dat a zajistěte, aby byl provoz dobře distribuované napříč oddíly.
> 
> Když aplikace dosáhne limitu co dokáže zpracovat oddíl pro úlohy, začnou Azure Storage, vrátí kód chyby 503 (zaneprázdněný Server) nebo kód chyby 500 odpovědí (časový limit operace). V takovém případě by aplikace měla použít zásadu exponenciálního omezení rychlosti pro opakování. Exponenciálního omezení rychlosti umožňuje zatížení v oddílu snížit a doběhu špičky na provoz oddílu.
> 
> 

Pokud potřebám vaší aplikace přesahují cíle škálovatelnosti účtu jedno úložiště, můžete sestavit aplikace k používání více účtů úložiště a oddílu datové objekty mezi tyto účty úložiště. V tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/) informace o cenách.

## <a name="scalability-targets-for-blobs-queues-tables-and-files"></a>Cíle škálovatelnosti pro objekty BLOB, fronty, tabulky a soubory
[!INCLUDE [azure-storage-limits](../../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
## <a name="scalability-targets-for-virtual-machine-disks"></a>Cíle škálovatelnosti pro disky virtuálního počítače
[!INCLUDE [azure-storage-limits-vm-disks](../../includes/azure-storage-limits-vm-disks.md)]

V tématu [velikosti virtuálních počítačů Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [velikosti virtuálního počítače s Linuxem](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) další podrobnosti.

## <a name="managed-virtual-machine-disks"></a>Disky spravovaných virtuálních počítačů

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../includes/azure-storage-limits-vm-disks-managed.md)]

## <a name="unmanaged-virtual-machine-disks"></a>Disky nespravované virtuálního počítače
[!INCLUDE [azure-storage-limits-vm-disks-standard](../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="scalability-targets-for-azure-resource-manager"></a>Cíle škálovatelnosti pro Azure resource Manageru
[!INCLUDE [azure-storage-limits-azure-resource-manager](../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="partitions-in-azure-storage"></a>Oddíly v úložišti Azure
Každý objekt, který obsahuje data, která je uložená ve službě Azure Storage (objekty BLOB, zprávy, entit a soubory) patří do oddílu, je identifikovaný klíč oddílu. Oddíl Určuje, jak vyrovnává zatížení Azure Storage objekty BLOB, zprávy, entit a soubory na serverech, aby vyhovovaly provozu těchto objektů. Klíč oddílu je jedinečný a je používaná k nalezení objektů blob, zprávy nebo entity.

V tabulce výše uvedeném v [cíle škálovatelnosti pro standardní účty úložiště](#standard-storage-accounts) uvádí výkonnostní cíle pro jeden oddíl pro každou službu.

Oddíly ovlivnit Vyrovnávání zatížení a škálovatelnost pro jednotlivé služby úložiště následujícím způsobem:

* **Objekty BLOB**: klíč oddílu pro objekt blob je název účtu + název kontejneru + název objektu blob. To znamená, že každý objekt blob může mít svůj vlastní oddíl Pokud zatížení u objektu blob se vyžaduje. Objekty BLOB mohou být distribuovány na počtu serverů, aby bylo možné škálovat přístup k nim, ale jediného objektu blob může obsluhovat pouze jeden server. Zatímco objekty BLOB mohou být logicky seskupeny do kontejnery objektů blob, neexistují žádné rozdělení dopady z toto seskupení.
* **Soubory**: klíč oddílu pro soubor je účet název + soubor název sdílené složky. To znamená, že všechny soubory ve sdílené složce jsou také v jeden oddíl.
* **Zprávy**: klíč oddílu zprávy je název účtu + název fronty, takže všechny zprávy ve frontě jsou seskupené do jednoho oddílu a zpracovává jeden server. Různé fronty může zpracovávat různé servery k vyrovnávání zatížení pro ale mnoho fronty může mít účet úložiště.
* **Entity**: klíč oddílu pro entitu je účet název + název tabulky + oddílu klíč, kde klíč oddílu je hodnota požadované uživatelem definované **PartitionKey** vlastnost pro entitu. Všechny entity se stejnou hodnotou klíče oddílu jsou seskupené do stejného oddílu a zpracovává stejný server oddílu. Toto je bod důležité pochopit, při navrhování vaší aplikace. Aplikace by měla vyvážit výhody škálovatelnost šíření entity napříč více oddílů s výhody přístup dat seskupování entity v jeden oddíl.  

Klíčové výhody na seskupení sady entit v tabulce do jednoho oddílu je, aby bylo možné provádět atomic dávkových operací na entity v stejného oddílu, protože oddíl existuje na jednom serveru. Pokud chcete provést dávkové operace pro skupinu entit, zvažte proto je seskupit se stejným klíčem oddílu. 

Na druhé straně entit, které jsou ve stejné tabulce, tabulka však mají různé klíče oddílů může být zatížení rovnoměrně rozdělen mezi různými servery, které umožňují mít větší škálovatelnost.

Doporučení k návrhu, vytváření oddílů strategie pro tabulky naleznete podrobné [zde](https://msdn.microsoft.com/library/azure/hh508997.aspx).

## <a name="see-also"></a>Viz také
* [Podrobnosti o cenách úložiště](https://azure.microsoft.com/pricing/details/storage/)
* [Předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md)
* [Storage úrovně Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md)
* [Replikace Azure Storage](storage-redundancy.md)
* [Microsoft Azure Storage výkon a škálovatelnost kontrolní seznam](storage-performance-checklist.md)
* [Microsoft Azure Storage: Vysoce dostupný cloudové úložiště služby se silnou konzistenci](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

