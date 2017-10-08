---
title: "aaaMigrate tooSQL databáze serveru SQL Server na virtuálním počítači | Microsoft Docs"
description: "Informace o tom, jak toomigrate místního uživatele databáze tooSQL Server ve virtuálním počítači Azure."
services: virtual-machines-windows
documentationcenter: 
author: sabotta
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 00fd08c6-98fa-4d62-a3b8-ca20aa5246b1
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: carlasab
ms.openlocfilehash: 9c7aba30304ea40796412d2ddc885f6d4a58be2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-a-sql-server-database-toosql-server-in-an-azure-vm"></a>Migrace tooSQL databáze serveru SQL Server ve virtuálním počítači Azure

Existuje několik metod toomigrate tooSQL uživatele databáze systému SQL Server na místní Server ve virtuálním počítači Azure. Tento článek se stručně popisují různé metody a doporučujeme hello nejlepší metody pro různé scénáře.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="what-are-hello-primary-migration-methods"></a>Jaké jsou metody primární migrace hello?
Hello primární migrace metody jsou následující:

* Místní zálohování pomocí komprese a ručně kopírovat hello záložní soubor do hello virtuální počítač Azure
* Proveďte zálohování tooURL a obnovení do hello virtuální počítač Azure z adresy URL hello
* Odpojte a pak zkopírujte hello protokolu a data souborů tooAzure úložiště objektů blob a pak připojte tooSQL Server ve virtuálním počítači Azure z adresy URL
* Převést fyzický místní počítač tooHyper-V virtuální pevný disk, nahrajte tooAzure úložiště objektů Blob a pak nasadit jako nový virtuální počítač pomocí nahrát virtuální pevný disk
* Dodávat pevný disk pomocí služby Windows importu a exportu
* Pokud máte nasazení AlwaysOn místně, použijte hello [Průvodce přidáním Azure repliky](../classic/sql-onprem-availability.md) toocreate repliky ve službě Azure a pak převzetí služeb při selhání, odkazující instance databáze Azure toohello uživatelů
* Pomocí systému SQL Server [transakční replikace](https://msdn.microsoft.com/library/ms151176.aspx) tooconfigure hello Azure SQL Server instance jako odběratel a potom zakažte replikaci, odkazující instance databáze Azure toohello uživatelů

> [!TIP]
> Můžete také použít tyto stejné databáze toomove techniky mezi virtuálním počítačům systému SQL Server v Azure. Není třeba žádné podporované způsob tooupgrade Galerie image virtuálního počítače systému SQL Server z jednoho tooanother verzi nebo edici. V takovém případě by měl vytvořit nový virtuální počítač SQL Server s novou verzi nebo edici hello a pak použijte jednu z techniky hello migrace v této toomove článku vaší databáze. 

## <a name="choosing-your-migration-method"></a>Volba způsob migrace
Pro optimální data výkonu přenosu migrace hello soubory databáze do hello virtuálního počítače Azure pomocí komprimovaný soubor zálohy.

výpadek toominimize během procesu migrace databáze hello, použijte buď hello AlwaysOn možnost nebo hello transakční replikace.

Pokud není možné toouse hello výše metody, ruční migraci databáze. Tuto metodu použijete, se obecně začínat zálohu databáze, za nímž následuje kopii zálohy databáze hello do Azure a poté proveďte obnovení databáze. Můžete také zkopírovat soubory databáze hello, sami do Azure a připojte je. Existuje několik metod, které můžete provést tento ruční proces migrace databáze do virtuálního počítače Azure.

> [!NOTE]
> Když upgradujete tooSQL Server 2014 nebo SQL Server 2016 ze starších verzí systému SQL Server, měli byste zvážit, zda je nutné provést změny. Doporučujeme, abyste předem vyřešili všechny závislosti na funkcích nepodporuje hello novou verzi systému SQL Server jako součást migrace projektu. Další informace o hello podporována edice a scénáře, najdete v části [Upgrade tooSQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

Hello následující tabulka obsahuje seznam všech metod hello primární migrace a popisuje, když je nejvhodnější hello použití každé metody.

| Metoda | Verze databáze zdroje | Verze cíle pro databáze | Omezení velikosti zálohy zdrojové databáze | Poznámky |
| --- | --- | --- | --- | --- |
| [Místní zálohování pomocí komprese a ručně kopírovat hello záložní soubor do hello virtuální počítač Azure](#backup-and-restore) |SQL Server 2005 nebo vyšší |SQL Server 2005 nebo vyšší |[Azure limitu úložiště virtuálních počítačů](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | To je velmi jednoduché a dobře otestované technika pro přesunutí databází mezi počítače. |
| [Proveďte zálohování tooURL a obnovení do hello virtuální počítač Azure z adresy URL hello](#backup-to-url-and-restore) |SQL Server 2012 SP1 CU2 nebo vyšší |SQL Server 2012 SP1 CU2 nebo vyšší |< 12,8 TB pro SQL Server 2016, jinak < 1 TB | Tato metoda je jenom další způsob toomove hello záložní soubor toohello virtuálních počítačů pomocí úložiště Azure. |
| [Odpojte a pak zkopírujte hello protokolu a data souborů tooAzure úložiště objektů blob a pak připojte tooSQL Server ve virtuálním počítači Azure z adresy URL](#detach-and-attach-from-url) |SQL Server 2005 nebo vyšší |SQL Server 2014 nebo vyšší |[Azure limitu úložiště virtuálních počítačů](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Tuto metodu použijte, když plánujete příliš[ukládání těchto souborů pomocí služby Azure Blob storage hello](https://msdn.microsoft.com/library/dn385720.aspx) a připojte je tooSQL serveru spuštěna ve virtuálním počítači Azure, zejména s velmi velké databáze |
| [Převést na místní počítač tooHyper-V virtuální pevné disky, nahrání tooAzure úložiště objektů Blob a potom nasazení nového virtuálního počítače pomocí nahraný virtuální pevný disk](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) |SQL Server 2005 nebo vyšší |SQL Server 2005 nebo vyšší |[Azure limitu úložiště virtuálních počítačů](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Použijte, když [přinesou vlastní licenci na SQL Server](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md), když migrace databáze, který se spustí ve starší verzi systému SQL Server, nebo když migrace systému a uživatelských databází společně jako součást migrace databáze závislý na dalších hello uživatel databáze nebo systémové databáze. |
| [Dodávat pevný disk pomocí služby Windows importu a exportu](#ship-hard-drive) |SQL Server 2005 nebo vyšší |SQL Server 2005 nebo vyšší |[Azure limitu úložiště virtuálních počítačů](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Použití hello [importu/exportu služby systému Windows](../../../storage/common/storage-import-export-service.md) po příliš pomalé, například s velmi velké databáze ruční copy – metoda |
| [Použití hello Průvodce přidáním Azure repliky](../classic/sql-onprem-availability.md) |SQL Server 2012 nebo vyšší |SQL Server 2012 nebo vyšší |[Azure limitu úložiště virtuálních počítačů](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Minimalizuje prostoje, použijte v případě nasazení místní AlwaysOn |
| [Pomocí transakční replikace systému SQL Server](https://msdn.microsoft.com/library/ms151176.aspx) |SQL Server 2005 nebo vyšší |SQL Server 2005 nebo vyšší |[Azure limitu úložiště virtuálních počítačů](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Použijte v případě, že potřebujete toominimize výpadky a nemají místní nasazení AlwaysOn |

## <a name="backup-and-restore"></a>Zálohování a obnovení
Zálohovat databázi s kompresí zkopírujte hello zálohování toohello virtuálních počítačů a potom obnovte databáze hello. Pokud záložní soubor je větší než 1 TB, musí rozkládají protože hello maximální velikost disku virtuálního počítače je 1 TB. Použijte následující obecné kroky toomigrate uživatele databáze pomocí této metody Ruční hello:

1. Proveďte úplnou databázi zálohování tooan místní umístění.
2. Vytvořit nebo nahrát virtuální počítač s hello verzi systému SQL Server potřeby.
3. Nastavte připojení podle svých požadavků. V tématu [připojit tooa virtuálního počítače systému SQL Server na platformě Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).
4. Zkopírujte vaší tooyour záložní soubory virtuálního počítače pomocí vzdálené plochy, Průzkumníka Windows nebo hello kopie příkazu z příkazového řádku.

## <a name="backup-toourl-and-restore"></a>TooURL zálohování a obnovení
Namísto místního souboru tooa zálohování, můžete použít hello [zálohování tooURL](https://msdn.microsoft.com/library/dn435916.aspx) a potom obnovte z adresy URL toohello virtuálních počítačů. Prokládané zálohovací sklady s SQL Server 2016, jsou podporovány, – se doporučují pro výkon a vyžaduje omezení velikosti hello tooexceed na objekt blob. Pro velké databáze hello použití hello [importu/exportu služby systému Windows](../../../storage/common/storage-import-export-service.md) se doporučuje.

## <a name="detach-and-attach-from-url"></a>Odpojit a připojit z adresy URL
Odpojení vaší databáze a soubory protokolu a přenos je příliš[úložiště objektů Azure Blob](https://msdn.microsoft.com/library/dn385720.aspx). Pak připojte databázi hello z adresy URL hello na vašem virtuálním počítači Azure. Použijte, pokud chcete, aby tooreside soubory hello fyzická databáze v úložišti objektů Blob. To může být užitečná pro velké databáze. Použijte následující obecné kroky toomigrate uživatele databáze pomocí této metody Ruční hello:

1. Odpojte hello soubory databáze z instance hello místní databáze.
2. Zkopírujte soubory databáze hello odpojit do Azure blob storage pomocí hello [příkazového řádku azcopy](../../../storage/common/storage-use-azcopy.md).
3. Připojte soubory databáze hello z instance systému SQL Server toohello hello adresy URL Azure v hello virtuálního počítače Azure.

## <a name="convert-toovm-and-upload-toourl-and-deploy-as-new-vm"></a>Převést tooVM a tooURL nahrát a nasadit jako nový virtuální počítač
Na virtuálním počítači místní instance systému SQL Server tooAzure pomocí toomigrate tato metoda všechny systémové a uživatelské databáze. Použijte následující obecné kroky toomigrate instanci systému SQL Server pomocí této metody Ruční hello:

1. Převést fyzický nebo virtuální počítače, virtuální pevné disky tooHyper-V pomocí [Microsoft Virtual Machine Converter](https://technet.microsoft.com/library/dn874008(v=ws.11).aspx).
2. Nahrání virtuálního pevného disku soubory tooAzure úložiště pomocí hello [rutinu Add-AzureVHD](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3. Nasazení nového virtuálního počítače pomocí hello nahrát virtuální pevný disk.

> [!NOTE]
> toomigrate celou aplikaci, zvažte použití [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Dodávat pevný disk
Použití hello [metoda importu/exportu služby systému Windows](../../../storage/common/storage-import-export-service.md) tootransfer velkých objemů dat tooAzure soubor úložiště objektů Blob v situacích, kde odesílání přes síť hello je výtažkovými nebo není vhodný. S touto službou soubor odešlete jeden nebo více pevné disky obsahující data tooan Azure datového centra, kde budou data tooyour účet úložiště.

## <a name="next-steps"></a>Další kroky
Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

Pokyny pro vytvoření virtuálního počítače serveru SQL Azure ze zaznamenané bitové kopie, naleznete v části [tipy a triky na klonování virtuálních počítačích Azure SQL ze zaznamenané bitové kopie](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) na blogu šablon stylů CSS SQL serveru technici hello.

