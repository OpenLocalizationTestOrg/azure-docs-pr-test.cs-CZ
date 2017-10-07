---
title: "aaaWhat toodo v události hello výpadku Azure Storage | Microsoft Docs"
description: "Jaké toodo v události hello výpadku Azure Storage"
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 1/19/2017
ms.author: robinsh
ms.openlocfilehash: 93e1e831c35b96b8bf190fa2b56ab89350bbac13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-toodo-if-an-azure-storage-outage-occurs"></a>Jaké toodo, když dojde k výpadku Azure Storage
Ve společnosti Microsoft můžeme fungovat pevný toomake se, že našich služeb jsou vždy k dispozici. V některých případech vynutí nad rámec naše řízení dopad nám způsoby, které způsobit neplánované výpadky v jedné nebo více oblastech. toohelp zpracování těchto výjimečných výskytů, poskytujeme hello vysoké úrovně pokynů pro služby Azure Storage.

## <a name="how-tooprepare"></a>Jak tooprepare
Je důležité pro každý zákazník tooprepare vlastní plánu zotavení po havárii. Hello úsilí toorecover z úložiště výpadku obvykle zahrnuje operace pracovníky a automatizované postupy uvedené v pořadí tooreactivate aplikace ve stavu, funkční. Naleznete v dokumentaci k Azure pod toobuild toohello vlastního plánu zotavení po havárii:

* [Zotavení po havárii a vysoká dostupnost pro aplikace Azure](/azure/architecture/resiliency/disaster-recovery-high-availability-azure-applications.md)
* [Technické pokyny k odolnosti Azure](/azure/architecture/resiliency.md)
* [Služba Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/)
* [Účet replikace Azure Storage](storage-redundancy.md)
* [Služba Azure Backup](https://azure.microsoft.com/services/backup/)

## <a name="how-toodetect"></a>Jak toodetect
Hello doporučený způsob toodetermine hello stav služby Azure je toosubscribe toohello [řídicím panelu stavu služeb Azure](https://azure.microsoft.com/status/).

## <a name="what-toodo-if-a-storage-outage-occurs"></a>Jaké toodo, pokud dojde k výpadku úložiště
Pokud jeden nebo více služeb úložiště nejsou dostupné na jeden nebo více oblastí, existují dvě možnosti, můžete tooconsider. Pokud vyžadujete okamžitý přístup k datům tooyour, zvažte možnost 2.

### <a name="option-1-wait-for-recovery"></a>Možnost 1: Počkejte obnovení
V takovém případě není třeba žádné akce z vaší strany. Pracujeme usilovně toorestore hello služba Azure dostupnosti. Můžete monitorovat stav služby hello na hello [řídicím panelu stavu služeb Azure](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Možnost 2: Kopírování dat z sekundární
Pokud jste zvolili [geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (doporučeno) pro účty úložiště, bude mít přístup pro čtení tooyour data ze sekundární oblasti hello. Můžete použít nástroje, jako [AzCopy](storage-use-azcopy.md), [prostředí Azure PowerShell](storage-powershell-guide-full.md)a hello [knihovna pro přesun dat Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) toocopy data ze sekundární oblasti hello do jiný účet úložiště v unimpacted oblast a potom bodu úložiště toothat aplikace účet pro číst a zapisovat dostupnosti.

## <a name="what-tooexpect-if-a-storage-failover-occurs"></a>Jaké tooexpect, pokud dojde k selhání úložiště
Pokud jste zvolili [geograficky redundantní úložiště (GRS)](storage-redundancy.md#geo-redundant-storage) nebo [geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (doporučeno), Azure Storage bude uchovávat data odolná v dvou oblastí (primární i sekundární). V obou oblastí Azure Storage neustále udržuje víc replik vaše data.

Při místní po havárii ovlivní primární oblasti, nejprve pokusíme toorestore hello služby v této oblasti. Závisí na povaze hello hello po havárii a jeho dopadů, v některých výjimečných případech jsme nemusí být možné toorestore hello primární oblasti. V tomto bodě provedeme geo-převzetí služeb při selhání. replikace dat mezi oblastmi Hello je, že asynchronní proces, který zahrnuje zpoždění, takže je možné, že změny, které ještě nebyly replikované sekundární oblasti toohello mohou být ztracena. Můžete zadat dotaz hello ["Čas poslední synchronizace" svého účtu úložiště](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) tooget podrobnosti o stavu replikace hello.

Několik bodů týkající se možnosti geo-převzetí služeb při selhání úložiště hello:

* Úložiště geo převzetí aktivuje pouze hello tým Azure Storage – není vyžadována žádná akce zákazníka.
* Existující službu úložiště, které koncové body pro objekty BLOB, tabulky, fronty a soubory zůstane stejný hello po převzetí služeb při selhání hello; Hello záznam DNS, bude nutné aktualizovat toobe tooswitch ze sekundární oblasti toohello hello primární oblasti.
* Před a během hello geo selhání nebudete mít přístup pro zápis tooyour účet úložiště z důvodu toohello dopad po havárii hello ale můžete pořád číst z hello sekundární Pokud váš účet úložiště byl nakonfigurován jako RA-GRS.
* Když hello geo převzetí byla dokončena a hello rozšíří změny DNS, bude obnoveno účet úložiště tooyour oprávnění ke čtení a zápisu; To ukazuje toobe toowhat používá sekundární koncový bod. 
* Všimněte si, že pokud máte GRS nebo RA-GRS nakonfigurované pro účet úložiště hello budete mít přístup pro zápis. 
* Můžete zadat dotaz ["Geograficky převzetí služeb při selhání čas poslední" svého účtu úložiště](https://msdn.microsoft.com/library/azure/ee460802.aspx) tooget více podrobností.
* Po převzetí služeb při selhání hello účtu úložiště bude být plně funkční, ale stav "sníženou", protože je ve skutečnosti hostované v samostatné oblasti s žádné možné geografická replikace. toomitigate toto riziko, obnovíme hello původní primární oblasti a pak proveďte geo-navrácení služeb po obnovení toorestore hello původního stavu. Pokud je původní primární oblasti hello Neopravitelná, jsme se přidělit jiné sekundární oblasti.
  Další informace o hello infrastruktury geografická replikace Azure Storage, naleznete v toohello článek na blogu týmu úložiště hello o [možnosti redundance a RA-GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

## <a name="best-practices-for-protecting-your-data"></a>Osvědčené postupy pro ochranu dat
Existují některé doporučené přístupy tooback úložiště dat v pravidelných intervalech.

* Disky virtuálních počítačů – použití hello [služby Azure Backup](https://azure.microsoft.com/services/backup/) tooback disky virtuálních počítačů hello používá virtuální počítače Azure.
* Objekty BLOB bloku – vytvořit [snímku](https://msdn.microsoft.com/library/azure/hh488361.aspx) každý objekt blob bloku nebo zkopírujte účet úložiště tooanother hello objekty BLOB v jiné oblasti pomocí [AzCopy](storage-use-azcopy.md), [prostředí Azure PowerShell](storage-powershell-guide-full.md), nebo hello [ Knihovna Azure přesun dat](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* Pomocí tabulky – [AzCopy](storage-use-azcopy.md) tooexport hello tabulky dat na jiný účet úložiště v jiné oblasti.
* Použít soubory – [AzCopy](storage-use-azcopy.md) nebo [prostředí Azure PowerShell](storage-powershell-guide-full.md) toocopy tooanother úložiště souborů účet v jiné oblasti.

Informace o vytváření aplikací, které plně využít výhod funkce hello RA-GRS, najdete v článku [navrhování vysoce dostupné aplikace pomocí RA-GRS úložiště](../storage-designing-ha-apps-with-ragrs.md)

