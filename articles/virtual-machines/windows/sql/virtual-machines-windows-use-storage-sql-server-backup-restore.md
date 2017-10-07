---
title: "aaaHow toouse úložiště Azure pro obnovení a zálohování serveru SQL Server | Microsoft Docs"
description: "Zjistěte, jak tooback až SQL Server tooAzure úložiště. Vysvětluje výhody hello zálohování SQL databáze tooAzure úložiště."
services: virtual-machines-windows
documentationcenter: 
author: MikeRayMSFT
manager: jhubbard
tags: azure-service-management
ms.assetid: 0db7667d-ef63-4e2b-bd4d-574802090f8b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: mikeray
ms.openlocfilehash: 67ebe8377be97df1312f8c1345e23576aabe0c4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Použít úložiště Azure pro obnovení a zálohování systému SQL Server
## <a name="overview"></a>Přehled
Od verze SQL Server 2012 SP1 CU2, můžete nyní napsat záloh systému SQL Server přímo toohello služby úložiště objektů Blob Azure. Tato funkce tooback až tooand obnovení ze služby objektů Blob Azure hello můžete použít místní databázi systému SQL Server nebo databázi serveru SQL Server ve virtuálním počítači Azure. Zálohování toocloud nabízí výhody dostupnosti, neomezenou geograficky replikované odlehlého úložiště a snadnou migrací dat tooand z cloudu hello. Můžete použít příkazy zálohování nebo obnovení pomocí jazyka Transact-SQL nebo SMO.

SQL Server 2016 zavádí nové funkce; můžete použít [zálohy snímku souboru](http://msdn.microsoft.com/library/mt169363.aspx) tooperform téměř okamžité zálohy a velmi rychlé obnovení.

Toto téma vysvětluje, proč zvolit toouse úložiště pro zálohování SQL Azure a pak popisuje hello součásti související se situací. Můžete použít prostředky hello zadaná na konci hello hello článku tooaccess návody a toostart Další informace o používání této služby s záloh systému SQL Server.

## <a name="benefits-of-using-hello-azure-blob-service-for-sql-server-backups"></a>Výhody použití hello služby objektů Blob Azure pro zálohování serveru SQL
Existuje několik výzvy, které při zálohování serveru SQL Server. Tyto problémy jsou správy úložiště, riziko selhání úložiště, přístup k webu toooff úložiště a konfiguraci hardwaru. Řadu tyto problémy řešeny pomocí hello služby úložiště objektů Blob v Azure pro zálohování systému SQL Server. Vezměte v úvahu hello následující výhody:

* **Snadné použití**: ukládání záloh v Azure BLOB může být řešením odlehlého tooaccess praktické, flexibilní a snadné. Vytváření odlehlého úložiště pro zálohování serveru SQL Server může být stejně snadná jako úprava stávající skripty a úloh toouse hello **zálohování tooURL** syntaxe. Mimo místo úložiště by měl obvykle být dostatečně daleko od hello produkční databáze umístění tooprevent jeden po havárii, které mohou ovlivnit hello odlehlého pracoviště a umístění provozní databáze. Výběrem příliš[objekty BLOB vaší Azure geograficky replikovat](../../../storage/common/storage-redundancy.md), máte další vrstvu ochrany v hello události po havárii, která by mohla ovlivnit hello celou oblast.
* **Zálohování archivu**: hello služby Azure Blob Storage nabízí lepší pásky alternativní toohello často používá možnost tooarchive zálohy. Páskové úložiště můžou vyžadovat fyzické Transport tooan odlehlého zařízením a míry tooprotect hello média. Ukládání záloh ve službě Azure Blob Storage poskytuje rychlých, vysoce dostupný, a trvale archivovat možnost.
* **Spravované hardwaru**: neexistuje žádné režijní náklady na správu hardwaru se službami Azure. Azure services Správa hello hardwaru a zadejte geografická replikace redundance a ochranu proti selhání hardwaru.
* **Neomezené úložiště**: povolením objekty BLOB přímé zálohování tooAzure máte přístup toovirtually neomezené úložiště. Můžete taky zálohování disku virtuálního počítače Azure tooan je omezení na základě velikosti počítače. Je číslo toohello limit disků můžete připojit tooan virtuálního počítače, které jsou pro zálohování Azure. Tento limit je 16 disků pro instanci velmi velké a méně pro menší instance.
* **Zálohování dostupnosti**: záloh uložených v Azure BLOB jsou k dispozici odkudkoli a kdykoli a mohou být snadno přístupné pro obnovení tooeither jenom místní SQL Server nebo jiný Server SQL spuštěn v virtuální počítač Azure, bez potřebovat hello pro databáze, připojení a odpojení nebo stahování a připojení hello virtuálního pevného disku.
* **Náklady na**: platit pouze pro hello službu, která se používá. Může být cenově jako archiv mimo pracoviště a záložní možnost. V tématu hello [Azure cenové kalkulačky](http://go.microsoft.com/fwlink/?LinkId=277060 "cenové kalkulačky")a hello [Azure – ceny článku](http://go.microsoft.com/fwlink/?LinkId=277059 "cenová článku") Další informace informace.
* **Úložiště snímků**: Pokud soubory databáze jsou uložená v objektu blob Azure a používáte SQL Server 2016, můžete použít [zálohy snímku souboru](http://msdn.microsoft.com/library/mt169363.aspx) tooperform téměř okamžité zálohy a velmi rychlé obnovení.

Další podrobnosti najdete v tématu [zálohování systému SQL Server a obnovení služby úložiště objektů Blob Azure](http://go.microsoft.com/fwlink/?LinkId=271617).

Následující dva oddíly Hello zavést hello služby úložiště objektů Blob Azure, včetně hello požadované součásti systému SQL Server. Je důležité toounderstand hello součásti a jejich interakce toosuccessfully použití zálohování a obnovení ze služby úložiště objektů Blob v Azure hello.

## <a name="azure-blob-storage-service-components"></a>Součásti služby úložiště objektů Blob v Azure
Hello následující Azure součásti jsou používány při zálohování toohello služby úložiště objektů Blob Azure.

| Komponenta | Popis |
| --- | --- |
| **Účet úložiště** |účet úložiště Hello je hello výchozí bod pro všechny služby úložiště. tooaccess služby Azure Blob Storage nejdřív vytvořit účet úložiště Azure. Další informace o službě Azure Blob storage najdete v tématu [jak toouse hello služby úložiště objektů Blob Azure](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Kontejner** |Kontejner zajišťuje seskupení sady objektů BLOB a můžete uložit libovolný počet objektů BLOB. toowrite zálohování tooan systému SQL Server služby objektů Blob Azure, musíte mít alespoň hello Kořenový kontejner vytvořit. |
| **Objekt BLOB** |Soubor libovolného typu a velikosti. Objekty BLOB jsou hello následující formát adresy URL adresovatelné: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Další informace o objekty BLOB stránky najdete v tématu [Principy bloku a objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>Součásti SQL serveru
Hello následující součásti systému SQL Server se používají při zálohování toohello služby úložiště objektů Blob Azure.

| Komponenta | Popis |
| --- | --- |
| **ADRESA URL** |Adresa URL Určuje záložní soubor identifikátor URI (Uniform Resource) tooa jedinečný. Adresa URL Hello je použité tooprovide hello umístění a název souboru zálohy hello systému SQL Server. Adresa URL Hello musí odkazovat tooan skutečné blob, ne jenom do kontejneru. Pokud objekt blob hello neexistuje, vytvoří se. Pokud existující objekt blob je zadán, zálohování se nezdaří, pokud hello > je zadána možnost klauzuli WITH FORMAT. Následuje příklad adresy URL hello zadáte v hello příkaz Zálohovat Hello: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS je doporučená, ale není potřeba. |
| **Přihlašovací údaje** |Hello informace, které je požadované tooconnect a ověření tooAzure služby úložiště objektů Blob je uloženo jako pověření.  Aby systém SQL Server toowrite zálohy tooan objektů Blob v Azure nebo obnovení z něj musí být vytvořený přihlašovací údaje systému SQL Server. Další informace najdete v tématu [přihlašovací údaje SQL serveru](https://msdn.microsoft.com/library/ms189522.aspx). |

> [!NOTE]
> Pokud zvolíte toocopy a nahrát soubor zálohy toohello služby úložiště objektů Blob Azure, musíte použít typu Objekt blob stránky jako svoji možnost úložiště plánujete toouse tento soubor pro operace obnovení. OBNOVENÍ z typu Objekt blob bloku se nezdaří s chybou.
> 
> 

## <a name="next-steps"></a>Další kroky
1. Pokud jste ještě nemáte, vytvořte účet Azure. Pokud hodnotíte Azure, vezměte v úvahu hello [bezplatnou zkušební verzi](https://azure.microsoft.com/free/).
2. Potom přejděte prostřednictvím jednoho z následujících návodů, které vás provede procesem vytvoření účtu úložiště a provádění obnovení hello.
   
   * **SQL Server 2014**: [kurz: SQL Server 2014 zálohování a obnovení tooMicrosoft služby úložiště objektů Blob Azure](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
   * **SQL Server 2016**: [kurz: pomocí služby Microsoft Azure Blob storage hello s databází SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx)
3. Přečtěte si další dokumentaci od [zálohování systému SQL Server a obnovení službou Microsoft Azure Blob Storage](https://msdn.microsoft.com/library/jj919148.aspx).

Pokud máte potíže, přečtěte si téma hello [zálohování systému SQL Server tooURL osvědčené postupy a řešení potíží](https://msdn.microsoft.com/library/jj919149.aspx).

Pro jiný Server SQL zálohování a možnosti obnovení, najdete v části [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

