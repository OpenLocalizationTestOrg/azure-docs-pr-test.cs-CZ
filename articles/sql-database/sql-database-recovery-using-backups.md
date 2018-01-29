---
title: "Obnovení ze zálohy Azure SQL database | Microsoft Docs"
description: "Další informace o obnovení bodu v čase, která umožňuje vrácení databáze SQL Azure do předchozího bodu v čase (až 35 dnů)."
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
ms.workload: Active
ms.date: 11/20/2017
ms.author: carlrab
ms.openlocfilehash: 06eb02e408b95176ca99ca391b437a3751876fac
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Obnovit pomocí zálohy automatizované databáze Azure SQL database
SQL Database nabízí tyto možnosti pro databázi pomocí obnovení [automatizované zálohování databáze](sql-database-automated-backups.md) a [záloh v dlouhodobé uchovávání](sql-database-long-term-retention.md). Můžete obnovit ze zálohy databáze pro:

* Novou databázi na stejný logický server obnovit do určeného bodu v čase za dobu uchování. 
* Databázi na stejný logický server obnovit do času odstranění pro odstraněnou databázi.
* Novou databázi na libovolném logického serveru v libovolné oblasti obnovit do bodu nejnovější denní zálohy v geograficky replikované blob storage (RA-GRS).

> [!IMPORTANT]
> Během obnovení nelze přepsat stávající databázi.
>

Obnovené databáze způsobuje náklady na úložiště navíc za následujících podmínek: 
- Obnovení P11 – P15 S4 S12 nebo P1 – P6 pokud maximální velikost databáze je větší než 500 GB.
- Obnovte P1 – P6 nebo PRS1 – PRS6 k S4 S12, pokud maximální velikost databáze je větší než 250 GB.

Nadbytečné není náklady, protože maximální velikost obnovené databáze je větší než velikost úložiště, které jsou zahrnuté pro úrovně výkonu a je velmi účtovat žádné dodatečné úložiště zřízený výše zahrnuté množství.  Ceny navíc úložiště, najdete v článku [SQL Database stránce s cenami](https://azure.microsoft.com/pricing/details/sql-database/).  Pokud skutečná velikost využitého místa je menší než velikost úložiště, které jsou zahrnuty, pak tato zpoplatněné se vyhnout snížením maximální velikost databáze na zahrnuté množství. Další informace o velikosti úložiště databáze a Změna maximální velikosti databáze najdete v tématu [jednotné limitů databáze prostředků](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels).  

> [!NOTE]
> [Automatizované zálohování databáze](sql-database-automated-backups.md) se používají při vytváření [kopírování databáze](sql-database-copy.md). 
>


## <a name="recovery-time"></a>Doba obnovení
Doba obnovení obnovte databázi pomocí databáze automatizované zálohování je ovlivněno několika různými faktory: 

* Velikost databáze
* Úroveň výkonu databáze
* Počet zahrnutých protokoly transakcí
* Množství aktivit, které musí být přehrány za účelem obnovení do bodu obnovení
* Pokud obnovení je v jiné oblasti šířku pásma sítě 
* Počet souběžných obnovení požadavků zpracovaných v cílové oblasti. 
  
  Pro velmi velká nebo aktivní databázi obnovení může trvat několik hodin. Pokud je v oblasti provozu v případě delších výpadků, je možné, že jsou velký počet požadavků geografické obnovení, které jsou právě zpracovávány jiných oblastí. Pokud nejsou k dispozici mnoho požadavků, může zvýšit čas obnovení pro databáze v této oblasti. Většina databáze obnoví dokončení do 12 hodin.

Pro v rámci jednoho předplatného, že některá omezení na počet souběžných obnovení požadavků (včetně bodu v době obnovení, geografické obnovení a obnovení ze zálohy dlouhodobé uchovávání), které je odeslána a pokračovalo:
|  | **Max. počet zpracovávaných souběžných požadavků** | **Maximální počet souběžných požadavků odesílána.** |
| :--- | --: | --: |
|Izolované databáze (na jedno předplatné)|10|60|
|Elastický fond (na každý fond)|4|200|
||||

Neexistuje žádné integrované funkce, chcete-li hromadně obnovení. [Azure SQL Database: úplné obnovení serveru](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) skript představuje příklad jednoho způsobu provádění této úlohy.

> [!IMPORTANT]
> Pokud chcete obnovit pomocí automatizované zálohování, musíte být členem role Přispěvatel SQL serveru v rámci předplatného nebo se vlastník předplatného. Obnovení můžete provést pomocí webu Azure Portal, prostředí PowerShell nebo rozhraní REST API. Nelze použít jazyk Transact-SQL. 
> 

## <a name="point-in-time-restore"></a>Obnovení k určitému bodu v čase

Existující databáze můžete obnovit do dřívějšího bodu v čase jako novou databázi na stejný logický server pomocí portálu Azure [prostředí PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), nebo [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> Skript prostředí PowerShell ukázkový znázorňující postup proveďte v okamžiku obnovení databáze, najdete v části [obnovte databázi SQL pomocí prostředí PowerShell](scripts/sql-database-restore-database-powershell.md).
>

Databázi můžete obnovit, úroveň výkonu a vrstvy jakékoli služby a jako jedné databáze nebo do fondu elastické databáze. Zkontrolujte, zda že máte dostatek prostředků na logickém serveru nebo ve fondu elastické, do které chcete obnovit databázi. Po dokončení obnovenou databázi je normální, plně přístupný, online databáze. Obnovenou databázi je účtovat normální tempem podle jeho úrovně a výkonu služby. Není platit poplatky, dokud se nedokončí obnovení databáze.

Obecně obnovení databáze do dřívějšího bodu pro účely obnovení. Když to uděláte, můžete k nahrazení původní databáze zacházet s obnovenou databázi nebo ji použít k načtení dat z a aktualizujte původní databázi. 

* ***Databáze nahrazení:*** Pokud obnovené databáze je určena k nahrazení původní databáze, byste měli ověřit úroveň výkonu a vrstvu služby jsou vhodné a škálovat databázi, v případě potřeby. Můžete přejmenovat původní databáze a pak umožnit obnovené databáze na původní název pomocí [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) v T-SQL. 
* ***Obnovení dat:*** Pokud budete chtít načíst data z obnovené databáze pro obnovení po chybě uživatele nebo aplikace, budete muset zapisovat a spouštět skripty obnovení potřebná data extrahovat data z obnovené databáze do původní databáze. Přestože operace obnovení může být časově náročná, obnovení databáze je zobrazená v seznamu databáze v průběhu procesu obnovení. Odstranění databáze během obnovení zrušeny operace obnovení a vám není účtován databázi, která se nedokončila obnovení. 

### <a name="azure-portal"></a>Azure Portal

Pokud chcete obnovit do bodu v čase pomocí portálu Azure, otevřete stránku pro vaši databázi a klikněte na tlačítko **obnovení** na panelu nástrojů.

![point-in-time-restore](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

## <a name="deleted-database-restore"></a>Obnovení odstraněné databáze
K odstranění dobu odstraněnou databázi na stejný logický server pomocí portálu Azure můžete obnovit odstraněnou databázi [prostředí PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), nebo [REST (createMode = obnovit)](https://msdn.microsoft.com/library/azure/mt163685.aspx). Můžete obnovit odstraněnou databázi do dřívějšího bodu v době uchování pomocí [prostředí PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase).

> [!TIP]
> Skript prostředí PowerShell ukázkový znázorňující postup obnovit odstraněnou databázi, naleznete v části [obnovte databázi SQL pomocí prostředí PowerShell](scripts/sql-database-restore-database-powershell.md).
>

> [!IMPORTANT]
> Pokud odstraníte instanci serveru Azure SQL Database, budou odstraněny také její databáze a nelze jej obnovit. Aktuálně neexistuje žádná podpora pro obnovení odstraněné serveru.
> 

### <a name="azure-portal"></a>Azure Portal

Chcete-li obnovit odstraněnou databázi během jeho [dobu uchování](sql-database-service-tiers.md) pomocí portálu Azure, otevřete stránku pro váš server a v oblasti operace, klikněte na tlačítko **odstranila databáze**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)


![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

## <a name="geo-restore"></a>Geografické obnovení
Můžete obnovit databázi SQL na libovolném serveru v libovolné oblasti Azure z nejnovější geograficky replikované úplné a rozdílové zálohy. Geografické obnovení používá geograficky redundantní zálohy jako zdroj a slouží k obnovení databáze, i když je nedostupná v důsledku výpadku databáze nebo datacenter. 

Geografické obnovení je výchozí možnost, když databáze není k dispozici z důvodu incidentu v oblasti, který je hostitelem databáze. Pokud ve velkém měřítku incident v oblasti výsledky v nedostupnosti databázovou aplikaci, můžete obnovit databázi ze zálohy geograficky replikované na server v jiné oblasti. Dochází ke zpoždění mezi při provést rozdílovou zálohu a kdy je geograficky replikovat do Azure blob v jiné oblasti. Toto zpoždění může být až hodinu, proto, pokud dojde k havárii, může být až ke ztrátě dat. jednu hodinu. Následující obrázek znázorňuje obnovit databázi z poslední zálohy dostupné v jiné oblasti.

![geografické obnovení](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Skript prostředí PowerShell ukázkový znázorňující postup geografické obnovení, najdete v části [obnovte databázi SQL pomocí prostředí PowerShell](scripts/sql-database-restore-database-powershell.md).
> 

Obnovení bodu v čase geo sekundární není aktuálně podporován. V okamžiku obnovení lze provést pouze v primární databázi. Podrobné informace o používání geografické obnovení obnovit výpadku, najdete v části [zotavit výpadku](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Obnovení ze zálohy je nejzákladnější řešení zotavení po havárii, která je k dispozici v databázi SQL s cíl bodu nejdelší obnovení (RPO) a odhad obnovení čas (Vložit). Pro řešení pomocí základní databáze geografické obnovení je často přiměřené řešení zotavení po Havárii s vložit 12 hodin. Pro řešení pomocí větší Standard nebo Premium databází, které vyžadují kratší časy obnovení, měli byste zvážit použití [aktivní geografickou replikací](sql-database-geo-replication-overview.md). Aktivní geografickou replikací nabízí mnohem menší plánovaný bod obnovení a vložit jako vyžaduje pouze zahájit převzetí služeb při selhání na nepřetržitě replikované sekundární. Další informace o možnosti kontinuity obchodních najdete v tématu [přehled kontinuity podnikových procesů](sql-database-business-continuity.md).
> 

### <a name="azure-portal"></a>Azure Portal

K geografické obnovení a databáze během jeho [dobu uchování](sql-database-service-tiers.md) pomocí portálu Azure, otevřete stránku databází SQL a pak klikněte na tlačítko **přidat**. V **zvolit zdroj** textového pole, vyberte **zálohování**. Zadejte záloha, ze které se má provést obnovení v oblasti a na serveru podle svého výběru. 

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Prostřednictvím kódu programu obnovení pomocí automatizované zálohování
Jako uvedeno výše, kromě portálu Azure lze provést obnovení databáze programově pomocí Azure PowerShell nebo rozhraní REST API. Následující tabulky popisují sadu příkazů, které jsou k dispozici.

### <a name="powershell"></a>PowerShell
| Rutina | Popis |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Získá jednu nebo více databází. |
| [Get-AzureRMSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Získá odstraněné databáze, kterou můžete obnovit. |
| [Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) |Získá geograficky redundantní zálohy databáze. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) |Obnoví databázi SQL. |
|  | |

### <a name="rest-api"></a>REST API
| Rozhraní API | Popis |
| --- | --- |
| [REST (createMode = obnovení)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Obnoví databázi |
| [Získat vytvořit nebo aktualizovat stav databáze](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Vrátí stav během operace obnovení |
|  | |

## <a name="summary"></a>Souhrn
Automatické zálohování chránit vaše databáze z uživatelů a chyby aplikace, databáze náhodného odstranění a provozu v případě delších výpadků. Tato integrované funkce je dostupná pro všechny úrovně služeb a úrovně výkonu. 

## <a name="next-steps"></a>Další postup
* Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).
* Další informace o Azure SQL Database automatizované zálohování najdete v tématu [automatizované zálohování SQL Database](sql-database-automated-backups.md).
* Další informace o dlouhodobé uchovávání záloh, najdete v části [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md).
* Pokud chcete konfigurovat, spravovat a obnovit ze dlouhodobé uchovávání automatické zálohy v trezoru služeb zotavení Azure pomocí portálu Azure najdete v tématu [konfigurace a použití dlouhodobé zálohování uchování](sql-database-long-term-backup-retention-configure.md). 
* Další informace o možnosti rychlejší obnovení najdete v tématu [převzetí služeb při selhání skupiny a aktivní geografickou replikací](sql-database-geo-replication-overview.md).  
