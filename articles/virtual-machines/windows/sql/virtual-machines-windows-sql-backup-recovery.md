---
title: "aaaBackup a obnovení pro SQL Server | Microsoft Docs"
description: "Popisuje aspekty zálohování a obnovení pro databáze systému SQL Server, které jsou spuštěné na virtuálních počítačích Azure."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/15/2016
ms.author: mikeray
ms.openlocfilehash: f85248fecdd5867d91d09650a1a34ad7c7caa920
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Zálohování a obnovení pro SQL Server v Azure Virtual Machines
## <a name="overview"></a>Přehled
Úložiště Azure udržuje 3 kopie každý virtuální počítač Azure disk tooguarantee ochrany proti ztrátě dat nebo poškození dat fyzické. Na rozdíl od místní, proto není nutné tooworry o těchto. Však měli stále zálohování vaší tooprotect databáze systému SQL Server proti chybám aplikaci nebo uživatele (např. vkládání chybná data nebo odstranění tabulky) a je možné toorestore tooa bodu v čase.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Pro SQL Server běžící ve virtuálních počítačích Azure můžete pomocí nativního zálohování a obnovení techniky pomocí připojených disků pro cíl hello hello zálohování souborů. Existuje však omezit toohello, počet disků můžete připojit tooan virtuální počítač Azure podle hello [velikost virtuálního počítače hello](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Je také režii hello tooconsider Správa disku.

Od verze SQL Server 2014, můžete zálohovat a obnovit tooMicrosoft Azure Blob storage. SQL Server 2016 také nabízí vylepšení pro tuto možnost. Kromě toho pro databázové soubory uložené v úložišti objektů Blob Microsoft Azure, SQL Server 2016 poskytuje možnost pro téměř okamžité zálohy a pro rychlé obnovení pomocí Azure snímků. Tento článek obsahuje přehled těchto možností a další informace naleznete na [zálohování systému SQL Server a obnovení službou Microsoft Azure Blob Storage](https://msdn.microsoft.com/library/jj919148.aspx).

> [!NOTE]
> Diskuzi o hello možnosti pro zálohování velmi velké databáze, v tématu [Víceterabajtové SQL Server databáze zálohování strategie pro virtuální počítače Azure](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).
> 
> 

v níže uvedených částech Hello zahrnují informace o konkrétní toohello různé verze systému SQL Server podporovanou ve virtuálním počítači Azure.

## <a name="sql-server-virtual-machines"></a>Virtuální počítače se systémem SQL Server
Pokud vaše instance systému SQL Server běží na virtuální počítač Azure, soubory databáze již nacházet v datových disků v Azure. Tyto disky za provozu v Azure Blob storage. Proto hello důvodů, proč pro zálohování vaší databáze a hello přístupy, které že je mírně provést změnu. Vezměte v úvahu následující hello. 

* Již nepotřebujete tooperform databáze zálohy tooprovide ochranu proti selhání hardwaru nebo média, protože Microsoft Azure poskytuje tuto ochranu jako součást hello služby Microsoft Azure.
* Stále potřebujete tooperform databáze zálohy tooprovide ochranu proti chybám uživatele nebo pro archivační účely, regulačních důvodů nebo pro účely správy.
* Můžete uložit záložní soubor hello přímo v Azure. Další informace najdete v tématu hello následující oddíly, které obsahují pokyny pro hello různé verze systému SQL Server.

## <a name="sql-server-2016"></a>SQL Server 2016
Podporuje Microsoft SQL Server 2016 [zálohování a obnovení s Azure blob](https://msdn.microsoft.com/library/jj919148.aspx) funkce nalézt v systému SQL Server 2014. Ale zahrnuje také hello následující vylepšení:

| Vylepšení 2016 | Podrobnosti |
| --- | --- |
| **Proložení** |Při zálohování tooMicrosoft úložiště objektů blob v Azure, SQL Server 2016 podporuje zálohování tooenable objekty BLOB toomultiple zálohování velké databáze, až tooa maximálně 12,8 TB. |
| **Zálohy snímku** |Zálohy snímku souboru SQL serveru prostřednictvím hello použití Azure snímků, poskytuje téměř okamžité zálohy a rychlé obnovení pro soubory databáze uložené pomocí služby Azure Blob storage hello. Tato funkce umožňuje vám toosimplify zásad zálohování a obnovení. Zálohy snímku souboru také podporuje bod v době obnovení. Další informace najdete v tématu [snímek zálohy pro soubory databáze v Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx). |
| **Spravované plánování zálohování** |TooAzure spravované zálohování systému SQL Server nyní podporuje vlastní plány. Další informace najdete v tématu [tooMicrosoft spravované zálohování systému SQL Server Azure](https://msdn.microsoft.com/library/dn449496.aspx). |

Kurz hello funkcí systému SQL Server 2016 při použití Azure Blob storage, najdete v části [kurz: pomocí služby Microsoft Azure Blob storage hello s databázemi SQL serveru 2016](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014
SQL Server 2014 zahrnuje hello následující vylepšení:

1. **Zálohování a obnovení tooAzure**:
   
   * *Zálohování systému SQL Server tooURL* má nyní podpora v systému SQL Server Management Studio. možnost tooback Hello až tooAzure je nyní k dispozici, pokud používáte úlohu zálohování nebo obnovení, nebo Průvodce plánu údržby v aplikaci SQL Server Management Studio. Další informace najdete v tématu [zálohování systému SQL Server tooURL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
   * *Spravované zálohování SQL serveru tooAzure* obsahuje nové funkce, která umožňuje automatické zálohování správy. To je užitečné zejména pro automatizaci správy zálohování pro SQL Server 2014 instancí spuštěných na počítači Azure. Další informace najdete v tématu [tooMicrosoft spravované zálohování systému SQL Server Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
   * *Automatizované zálohování* poskytuje další automatizace tooautomatically povolit *spravované zálohování systému SQL Server tooAzure* na všechny stávající a nové databáze pro virtuální počítač SQL Server v Azure. Další informace najdete v tématu [Automatizované zálohování pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).
   * Přehled všech možností hello tooAzure zálohování systému SQL Server 2014 najdete v tématu [zálohování systému SQL Server a obnovení službou Microsoft Azure Blob Storage](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
2. **Šifrování**: SQL Server 2014 podporuje šifrování dat při vytváření zálohy. Podporuje několik algoritmy šifrování a hello použít osf certifikát nebo asymetrický klíč. Další informace najdete v tématu [šifrování zálohy](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012
Podrobné informace o zálohování systému SQL Server a obnovení v systému SQL Server 2012 najdete v tématu [zálohování a obnovení databází SQL serveru (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Od verze SQL Server 2012 SP1 kumulativní aktualizaci 2, můžete zálohovat tooand obnovení tak, z hello služby Azure Blob Storage. Toto vylepšení můžou být použité tooback zálohu databáze systému SQL Server na SQL serveru se systémem na virtuální počítač Azure nebo místní instance. Další informace najdete v tématu [zálohování systému SQL Server a obnovení služby úložiště objektů Blob Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Mezi výhody používání služby Azure Blob storage hello hello patří hello možnost toobypass hello 16 omezení na disku pro připojené disky, snadnou správu hello přímé dostupnosti instance tooanother záložní soubor hello instance systému SQL Server, které jsou spuštěné v Azure virtuální počítač, nebo místní instance pro migraci nebo po havárii pro účely obnovení. Úplný seznam toousing výhody služby Azure blob storage pro zálohování systému SQL Server, najdete v části hello *výhody* kapitoly [zálohování systému SQL Server a obnovení služby úložiště objektů Blob Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Doporučení osvědčených postupů a informace o řešení potíží najdete v tématu [zálohování a obnovení osvědčených postupů (služby úložiště objektů Blob Azure)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008
Zálohování systému SQL Server a obnovení v systému SQL Server 2008 R2 najdete v tématu [zálohování a obnovení databází v systému SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

Zálohování systému SQL Server a obnovení v systému SQL Server 2008 naleznete v tématu [zálohování a obnovení databází v systému SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Další kroky
Pokud plánujete nasazení systému SQL Server ve virtuálním počítači Azure, můžete najít zřizování pokyny v následujícím kurzu hello: [zřizování virtuálního počítače systému SQL Server na platformě Azure pomocí Azure Resource Manageru](virtual-machines-windows-portal-sql-server-provision.md).

I když zálohování a obnovení lze použít toomigrate vaše data, jsou potenciálně jednodušší dat migrace cesty tooSQL Server na virtuálním počítači Azure. Úplnou diskusi o možnostech migrace a doporučení, najdete v části [migrace databáze tooSQL Server na virtuálním počítači Azure](virtual-machines-windows-migrate-sql.md).

Přečtěte si další [prostředky pro spuštění systému SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

