---
title: "aaaMoving velkých objemů dat do nebo ze cloudové úložiště v Azure | Microsoft Docs"
description: "Přehled hello různé metody pro přesunutí tooand data ze služby Azure Storage."
services: storage
documentationcenter: 
author: JarrettRenshaw
manager: msmets
editor: tysonn
ms.assetid: 5e3947a9-d99b-4108-9d57-3eb67c03e7ba
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: jarrettr
ms.openlocfilehash: 8f7105fea7c2d28ba9954898743070d338f46a37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="moving-data-tooand-from-azure-storage"></a>Přesun dat tooand ze služby Azure Storage
Pokud chcete, aby toomove místní data tooAzure úložiště (nebo naopak), existují různé způsoby toodo to. Hello přístup, který vám nejlépe vyhovuje bude záviset na váš scénář. Tento článek vám poskytne rychlý přehled o různých scénářů a příslušné nabídky pro každé z nich.

## <a name="building-applications"></a>Tvorba aplikací
Pokud vytváříte aplikace, vývoj proti hello REST API nebo jednoho z našich mnoho klientské knihovny je tooand skvělý způsob toomove data ze služby Azure Storage.

Azure Storage poskytuje množství knihoven klienta pro rozhraní .NET, iOS, Java, Android, univerzální platformu Windows (UWP), Xamarin, C++, Node.JS, PHP, Ruby a Python. Hello knihovny klienta nabízí pokročilé funkce, jako je například logika opakovaných pokusů, protokolování a paralelní ukládání. Také můžete vyvíjet přímo na hello REST API, které může zavolat jakýkoli jazyk schopný požadavky HTTP/HTTPS.

V tématu [Začínáme s Azure Blob Storage](storage-dotnet-how-to-use-blobs.md) toolearn Další.

Kromě toho jsme také nabízí hello [knihovna pro přesun dat úložiště Azure](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) tedy knihovny určená pro vysoce výkonné kopírování dat tooand z Azure. Podrobnosti najdete tooour knihovna pro přesun dat [dokumentace](https://github.com/Azure/azure-storage-net-data-movement) toolearn Další. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Rychlé zobrazení dat a interakce s nimi
Jestliže chcete tooview snadný způsob daty Azure Storage přitom má také možnost tooupload hello a stáhněte si vaše data, pak zvažte použití Azure Storage Explorer.

Podívejte se na naše seznam [Průzkumníci úložiště Azure](storage-explorers.md) toolearn Další.

## <a name="system-administration"></a>Správa systému
Pokud vyžadují nebo jsou pohodlnější pomocí nástroje příkazového řádku (např. Správci systému), jsou zde několik možností pro tooconsider můžete:

### <a name="azcopy"></a>AzCopy
AzCopy je nástroj příkazového řádku Windows určený pro vysoce výkonné kopírování dat tooand ze služby Azure Storage. Můžete také zkopírovat data v rámci účtu úložiště nebo mezi jiným účtům úložiště.

V tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md) toolearn Další.

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell je modul, který nabízí rutiny pro správu služeb v Azure. Je to prostředí příkazového řádku založené na úlohách a skriptovací jazyk určený speciálně pro správu systému.

V tématu [použití Azure Powershellu s Azure Storage](storage-powershell-guide-full.md) toolearn Další.

### <a name="azure-cli"></a>Azure CLI
Rozhraní příkazového řádku Azure poskytuje sadu softwaru open source, příkazy a platformy pro práci se službami Azure. Rozhraní příkazového řádku Azure je dostupná v systému Windows, na OSX a Linux.

V tématu [hello pomocí rozhraní příkazového řádku Azure s Azure Storage](storage-azure-cli.md) toolearn Další.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Přesunutí velkých objemů dat s pomalou síť
Jedním z nejnáročnějších úkolů hello přidružené k přesunu velkých objemů dat je dobu přenosu hello. Pokud chcete, bez starostí o náklady na sítě nebo psaní kódu tooget dat z Azure Storage, Azure Import/Export je vhodné řešení.

V tématu [Azure Import/Export](storage-import-export-service.md) toolearn Další.

## <a name="backing-up-your-data"></a>Zálohování dat
Pokud budete potřebovat jednoduše toobackup vaše data tooAzure úložiště, Azure Backup je způsob, jak toogo hello. Toto je výkonné řešení pro zálohování místních dat. a virtuálních počítačích Azure.

V tématu [Azure Backup](../backup/backup-introduction-to-azure-backup.md) toolearn Další.

## <a name="accessing-your-data-on-premises-and-from-hello-cloud"></a>Přístup k vaší data místně a z cloudu hello
Pokud potřebujete řešení pro přístup k vaší data místně a z cloudu hello, pak měli byste zvážit použití Azure hybridní řešení cloudového úložiště, StorSimple. Toto řešení se skládá z fyzického zařízení StorSimple, že inteligentně úložiště často používá data na jednotkách SSD, občas používají data na HDD a neaktivní nebo zálohování nebo archivaci dat v úložišti Azure.

V tématu [StorSimple](../storsimple/storsimple-overview.md) toolearn Další.

## <a name="recovering-your-data"></a>Obnovení dat
Pokud máte místní úlohy a aplikace, budete potřebovat řešení, které umožňuje vaší firmy toocontinue spuštěné v hello události havárie. Azure Site Recovery zpracovává replikace, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Replikovaná data jsou uložena ve službě Azure Storage, což vám tooeliminate hello potřebu sekundárního datového centra na místě.

V tématu [Azure Site Recovery](../site-recovery/site-recovery-overview.md) toolearn Další.
### <a name="moving-data-faq"></a>Přesun dat – nejčastější dotazy:
## <a name="can-i-migrate-vhds-from-one-region-tooanother-without-copying"></a>Můžete migrovat virtuální pevné disky z jedné oblasti tooanother bez kopírování?
Hello pouze způsob toocopy virtuální pevné disky, mezi oblasti jsou toocopy hello data mezi účty úložiště na každou oblast. To můžete pomocí nástroje AZCopy. V tématu přenos dat pomocí příkazového řádku Azcopy toolearn hello Další. Pro velmi velké objemy dat můžete také Azure Import/Export. V tématu [Azure Import/Export](https://docs.microsoft.com/en-us/azure/storage/storage-import-export-service) toolearn Další.
