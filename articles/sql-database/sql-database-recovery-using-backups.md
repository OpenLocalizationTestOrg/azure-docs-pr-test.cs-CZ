---
title: "aaaRestore ze zálohy Azure SQL database | Microsoft Docs"
description: "Informace o obnovení bodu v čase, který vám umožní tooroll zpět Azure SQL Database tooa předchozího bodu v čase (až too35 dnů)."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: fd1d334d-a035-4a55-9446-d1cf750d9cf7
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.openlocfilehash: 84ecc306a2072445c10dbf1e9d7cf3d1b43860cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Obnovit pomocí zálohy automatizované databáze Azure SQL database
SQL Database nabízí tyto možnosti pro databázi pomocí obnovení [automatizované zálohování databáze](sql-database-automated-backups.md) a [záloh v dlouhodobé uchovávání](sql-database-long-term-retention.md). Můžete obnovit ze zálohy databáze pro:

* Novou databázi na stejný logický server obnovit hello tooa Zadaný bod v čase za dobu uchování hello. 
* A databáze na hello stejný logický server obnovit toohello odstranění dobu odstraněnou databázi.
* Novou databázi na libovolném logického serveru v libovolné oblasti obnovit toohello bod hello nejnovější denní zálohy v geograficky replikované blob storage (RA-GRS).

> [!IMPORTANT]
> Během obnovení nelze přepsat stávající databázi.
>

Můžete také použít [automatizované zálohování databáze](sql-database-automated-backups.md) toocreate [kopírování databáze](sql-database-copy.md) na libovolném logického serveru v libovolné oblasti. 

## <a name="recovery-time"></a>Doba obnovení
Hello toorestore čas obnovení, které databázi pomocí databáze automatizované zálohování je ovlivněno několika různými faktory: 

* Hello velikost databáze hello
* úroveň výkonu Hello hello databáze
* Hello počet zahrnutých protokoly transakcí
* Hello množství aktivit, které potřebuje toobe přehrány bod obnovení toohello toorecover
* Šířka pásma sítě Hello, pokud obnovit hello je tooa jiné oblasti 
* Hello počet souběžných obnovení požadavků zpracovaných v hello cílová oblast. 
  
  Pro velmi velká nebo aktivní databázi hello obnovení může trvat několik hodin. Pokud je v oblasti provozu v případě delších výpadků, je možné, že jsou velký počet požadavků geografické obnovení, které jsou právě zpracovávány jiných oblastí. Po mnoho požadavků se může zvýšit hello čas obnovení pro databáze v této oblasti. Většina databáze obnoví dokončení do 12 hodin.
  
Není k dispozici žádné předdefinované funkce toodo hromadné obnovení. Hello [Azure SQL Database: úplné obnovení serveru](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) skript představuje příklad jednoho způsobu provádění této úlohy.

> [!IMPORTANT]
> pomocí toorecover automatizované zálohování, musíte být členem role Přispěvatel SQL Server hello v předplatném hello nebo se vlastník předplatného hello. Můžete ho obnovit pomocí hello portálu Azure, PowerShell nebo hello REST API. Nelze použít jazyk Transact-SQL. 
> 

## <a name="point-in-time-restore"></a>Obnovení bodu v čase

Můžete obnovit existující databázi tooan dříve bodu v čase jako novou databázi na hello stejný logický server pomocí portálu Azure, hello [prostředí PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/restore-azurermsqldatabase), nebo hello [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> Pro skript prostředí PowerShell ukázkový znázorňující, jak tooperform v okamžiku obnovení databáze, najdete v části [obnovte databázi SQL pomocí prostředí PowerShell](scripts/sql-database-restore-database-powershell.md).
>

Hello databáze může být obnovené tooany vrstvy služeb nebo výkonu úroveň a jako jedné databáze nebo do fondu elastické databáze. Ujistěte se, máte dostatek prostředků na hello logickém serveru nebo v elastickém fondu toowhich hello obnovujete databázi hello. Po dokončení hello obnovit databáze je normální, plně přístupný, online databází. Hello obnovit databáze je účtovat normální tempem podle jeho úrovně a výkonu služby. Není platit poplatky, dokud se nedokončí obnovení databáze hello.

Obecně obnovit databáze tooan dříve bod pro účely obnovení. V tom případě lze považovat hello obnovit databázi k nahrazení hello původní databáze nebo použít ho tooretrieve data z a aktualizujte hello původní databázi. 

* ***Databáze nahrazení:*** Pokud hello obnovit databáze je určena k nahrazení původní databáze hello, měli byste ověřit, úroveň výkonu hello nebo úroveň služby jsou vhodné a škálovat hello databáze v případě potřeby. Můžete přejmenovat hello původní databáze a pak umožnit hello obnovit hello původní název databáze pomocí příkazu ALTER DATABASE hello v T-SQL. 
* ***Obnovení dat:*** Pokud máte v plánu tooretrieve data z databáze toorecover hello obnovit z chyby uživatele nebo aplikace, potřebujete toowrite a provést hello data potřebná pro obnovení skripty tooextract data z databáze toohello hello obnovit původní databázi. I když hello obnovení operace může trvat dlouhou dobu toocomplete, je viditelný v seznamu hello databáze v průběhu procesu obnovení hello hello obnovení databáze. Pokud odstraníte hello databáze během obnovení hello, operace obnovení hello je zrušena a vám není účtován hello databázi, která se nedokončila hello obnovení. 

### <a name="azure-portal"></a>portál Azure

hello toorecover tooa bodu v současně pomocí portálu Azure, otevřete hello stránky pro databázi a klikněte na tlačítko **obnovení** na panelu nástrojů hello.

![bod v době obnovení](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

## <a name="deleted-database-restore"></a>Obnovení odstraněné databáze
Čas odstranění toohello odstraněné databáze můžete obnovit odstraněnou databázi na hello pomocí stejné logický server hello portál Azure, [prostředí PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/restore-azurermsqldatabase), nebo hello [REST (createMode = obnovit)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> Pro ukázku prostředí PowerShell skriptu znázorňující, jak zjistit, toorestore odstraněné databáze [obnovte databázi SQL pomocí prostředí PowerShell](scripts/sql-database-restore-database-powershell.md).
>

> [!IMPORTANT]
> Pokud odstraníte instanci serveru Azure SQL Database, budou odstraněny také její databáze a nelze jej obnovit. Aktuálně neexistuje žádná podpora pro obnovení odstraněné serveru.
> 

### <a name="azure-portal"></a>portál Azure

toorecover odstraněné databáze během jeho [dobu uchování](sql-database-service-tiers.md) pomocí hello portál Azure, otevřete hello stránky pro váš server a v oblasti hello operace, klikněte na tlačítko **odstranila databáze**.

![Odstranit database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)


![Odstranit database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

## <a name="geo-restore"></a>Geografické obnovení
Můžete obnovit databázi SQL na libovolném serveru v libovolné oblasti Azure z hello nejnovější geograficky replikované úplné a rozdílové zálohy. Geografické obnovení používá geograficky redundantní zálohy jako zdroj a dá se použít toorecover databáze i v případě, že je nepřístupný z důvodu výpadku tooan hello databáze nebo datacenter. 

Geografické obnovení je hello výchozí možnost pro obnovení, když databáze není k dispozici z důvodu incidentu v oblasti hello je hostitelem databáze hello. Pokud ve velkém měřítku incident v oblasti výsledky v nedostupnosti databázovou aplikaci, můžete obnovit databázi ze serveru tooa hello geograficky replikované záloh v jiné oblasti. Dochází ke zpoždění mezi při je provést rozdílovou zálohu, a když je geograficky replikované tooan Azure blob v jiné oblasti. Toto zpoždění může být až hodinu tooan Ano, pokud dojde k havárii, může být až ztráty dat tooone hodinu. Hello následující obrázek znázorňuje proces obnovení databáze hello z hello poslední dostupné zálohy v jiné oblasti.

![geografické obnovení](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Pro ukázku prostředí PowerShell skriptu znázorňující, jak zjistit, tooperform geografické obnovení, [obnovte databázi SQL pomocí prostředí PowerShell](scripts/sql-database-restore-database-powershell.md).
> 

Obnovení bodu v čase geo sekundární není aktuálně podporován. V okamžiku obnovení lze provést pouze v primární databázi. Podrobné informace o používání toorecover geografické obnovení výpadku najdete v tématu [zotavit výpadku](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Obnovení ze zálohy je hello nejzákladnější hello katastrofě řešení obnovení k dispozici v databázi SQL s hello nejdelší plánovaný bod obnovení a odhad obnovení čas (Vložit). Pro řešení pomocí základní databáze geografické obnovení je často přiměřené řešení zotavení po Havárii s vložit 12 hodin. Pro řešení pomocí větší Standard nebo Premium databází, které vyžadují kratší časy obnovení, měli byste zvážit použití [aktivní geografickou replikací](sql-database-geo-replication-overview.md). Aktivní geografickou replikací nabízí mnohem menší plánovaný bod obnovení a vložit jako vyžaduje pouze zahájit tooa nepřetržitě replikované sekundární převzetí služeb při selhání. Další informace o možnosti contiuity firmy, najdete v části [přes kontinuity podnikových procesů](sql-database-business-continuity.md).
> 

### <a name="azure-portal"></a>portál Azure

toogeo obnovení a databáze během jeho [dobu uchování](sql-database-service-tiers.md) pomocí hello portálu Azure, otevřete stránku hello databází SQL a pak klikněte na tlačítko **přidat**. V hello **zvolit zdroj** textového pole, vyberte **zálohování**. Zadejte hello záloha, ze které tooperform hello obnovení v oblasti hello a na serveru hello podle svého výběru. 

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Prostřednictvím kódu programu obnovení pomocí automatizované zálohování
Jak bylo popsáno dříve kromě toohello portál Azure, obnovení databáze lze provést programově pomocí prostředí Azure PowerShell nebo hello REST API. Hello následující tabulky popisují hello sadu příkazů, které jsou k dispozici.

### <a name="powershell"></a>PowerShell
| Rutina | Popis |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Získá jednu nebo více databází. |
| [Get-AzureRMSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Získá odstraněné databáze, kterou můžete obnovit. |
| [Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) |Získá geograficky redundantní zálohy databáze. |
| [Obnovení AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) |Obnoví databázi SQL. |
|  | |

### <a name="rest-api"></a>REST API
| Rozhraní API | Popis |
| --- | --- |
| [REST (createMode = obnovení)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Obnoví databázi |
| [Získat vytvořit nebo aktualizovat stav databáze](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Vrátí stav hello během operace obnovení |
|  | |

## <a name="summary"></a>Souhrn
Automatické zálohování chránit vaše databáze z uživatelů a chyby aplikace, databáze náhodného odstranění a provozu v případě delších výpadků. Tato integrované funkce je dostupná pro všechny úrovně služeb a úrovně výkonu. 

## <a name="next-steps"></a>Další kroky
* Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md)
* toolearn o zálohování Azure SQL Database automatizované, najdete v části [automatizované zálohování SQL Database](sql-database-automated-backups.md)
* toolearn o dlouhodobé uchovávání záloh, najdete v části [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md)
* tooconfigure, spravovat a obnovit ze dlouhodobé uchovávání automatizované zálohování v trezoru služeb zotavení Azure pomocí hello Azure portálu, najdete v tématu [konfigurace a použití dlouhodobé zálohování uchování](sql-database-long-term-backup-retention-configure.md). 
* toolearn o rychlejší možnosti obnovení, najdete v části [aktivní geografickou replikaci](sql-database-geo-replication-overview.md)  
