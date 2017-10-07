---
title: "aaaManage databází v Azure SQL Data Warehouse | Microsoft Docs"
description: "Přehled správy databází SQL Data Warehouse. Zahrnuje nástroje pro správu, Dwu a Škálováním na více systémů, řešení potíží s výkon dotazů, vytvoření zásad zabezpečení a obnovení databáze z poškození dat nebo z místní výpadku výkonu."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 8576fdb3-71fe-4b3b-a4e0-5e8a7f148acf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: caec6572c4ab395107c3b095adc69a53eae8bd88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Spravovat databáze v Azure SQL Data Warehouse
SQL Data Warehouse automatizuje mnoho aspektů správy vašich databází. Například tooscale výkonu stačí tooadjust a platit hello přímo úroveň výpočetní prostředky a potom nechat SQL Data Warehouse práci všechny hello škálování a škálování zpět.

Nepochybně můžete toomonitor vaše úlohy tooidentify výkon musí a také vyřešit dlouho běžící dotazy. Budete také potřebovat tooperform několik zabezpečení úlohy toomanage oprávnění pro uživatele a přihlášení.

Tento přehled popisuje tyto aspekty správy SQL Data Warehouse.

* Nástroje pro správu
* Škálování výpočetní kapacity
* Pozastavení a obnovení
* Osvědčené postupy z hlediska výkonu
* Monitorování dotazu
* Zabezpečení
* Zálohování a obnovení

## <a name="management-tools"></a>Nástroje pro správu
Můžete použít celou řadu nástrojů toomanage databází v SQL Data Warehouse. Jak budete spravovat databáze, budete vyvíjet předvoleb nástroje pro každý typ úlohy je nutné tooperform.

### <a name="azure-portal"></a>portál Azure
Hello [portál Azure] [ Azure portal] je webový portál, kde můžete vytvářet, aktualizovat a odstranit databáze a monitorovat prostředky v databázi. Tento nástroj je skvělým je, pokud jste se právě Začínáme s Azure, Správa malý počet databází datového skladu, nebo třeba tooquickly něco udělat.

tooget začít s hello portál Azure, najdete v části [vytvoření SQL Data Warehouse (portál Azure)][Create a SQL Data Warehouse (Azure portal)].

### <a name="sql-server-data-tools-in-visual-studio"></a>Nástroje SQL Server Data Tools v sadě Visual Studio
[SQL Server Data Tools] [ SQL Server Data Tools] (SSDT) v sadě Visual Studio můžete tooconnect pro správu a vývoj vaší databáze. Pokud jste vývojář aplikací obeznámeni s Visual Studio nebo jiné integrované vývojové prostředí (integrovaného vývojového prostředí), zkuste použít rozšíření SSDT v sadě Visual Studio.

Rozšíření SSDT zahrnuje hello Průzkumník objektů SQL serveru, což vám umožní toovisualize, připojit a spouštět skripty pro databáze SQL Data Warehouse. tooquickly připojit tooSQL datového skladu, jednoduše klikněte na hello **otevřete v sadě Visual Studio** tlačítka na panelu příkazů hello při zobrazení podrobností databáze hello hello portálu Azure Classic.  

tooget spuštění s rozšířením SSDT v sadě Visual Studio, najdete v části [dotazu Azure SQL Data Warehouse pomocí sady Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].

### <a name="command-line-tools"></a>Nástroje příkazového řádku
Nástroje příkazového řádku jsou ideální pro automatizaci vašich úloh.  Prostředí PowerShell a sqlcmd jsou dva způsoby skvělé tooautomate procesů.  Doporučujeme, abyste tyto nástroje pro správu velkého počtu logických serverů a nasazení prostředků změny v produkčním prostředí, jako hello úlohy nezbytné jde skriptování a potom automatizovat.

### <a name="dynamic-management-views"></a>Zobrazení dynamické správy
Zobrazení dynamické správy jsou hello chléb a másla správy SQL Data Warehouse. Téměř všechny informace, které poskytuje portálu hello spoléhá na zobrazení dynamické správy. toosee seznam SQL Data Warehouse zobrazení dynamické správy, najdete v části [zobrazení systému SQL Data Warehouse][SQL Data Warehouse system views].

tooget začít, najdete v části [připojit a zadávat dotazy pomocí sqlcmd][Connect and query with sqlcmd], a [vytvoření databáze (PowerShell)][Create a database (PowerShell)].

## <a name="scale-compute"></a>Škálování výpočetní kapacity
V SQL Data Warehouse můžete rychle škálovat výkonu out nebo zpět zvýšením nebo snížením výpočetní prostředky procesoru, paměti a šířky pásma vstupně-výstupní operace. výkon tooscale, stačí toodo je upravit hello počet že SQL Data Warehouse přiděluje tooyour databáze jednotky datového skladu (Dwu). SQL Data Warehouse rychle vytvoří hello změnit a zpracovává všechny změny toohardware základní hello nebo softwaru.

toolearn Další informace o škálování Dwu, najdete v části [škálování výkonu].

## <a name="pause-and-resume"></a>Pozastavení a obnovení
toosave náklady, můžete pozastavit a obnovit výpočetní prostředky na vyžádání. Například pokud nebudete používat hello databáze během noční hello a o víkendech, můžete pozastavit tyto v době a obnovit během dne hello. Vám nebude nic účtováno pro Dwu při hello databáze byla pozastavena.

Další informace najdete v tématu [pozastavit výpočetní][Pause compute], a [obnovit výpočty][Resume compute].

## <a name="performance-best-practices"></a>Osvědčené postupy z hlediska výkonu
Při Začínáme s novou technologií, zjišťování hello tipy a triky, které fungují nejlepší vpravo od začátku hello můžete ušetřit spoustu času.  Osvědčené postupy v rámci řadu témata s našimi zjistíte.

toosee mnoho souhrn hello nejdůležitější aspekty při vývoji úlohy, najdete v části [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].

## <a name="query-monitoring"></a>Monitorování dotazu
Někdy dotazu běží příliš dlouho, ale nejsou opravdu který z nich je který hello. SQL Data Warehouse má zobrazení dynamické správy (zobrazení dynamické správy), které můžete použít toofigure limitu dotazu, který trvá příliš dlouho.

toofind dlouho běžící dotazy, najdete v části [sledovat vaše úlohy pomocí zobrazení dynamické správy][Monitor your workload using DMVs].

## <a name="security"></a>Zabezpečení
toomaintain zabezpečení systému, musí být na hello upozornění a chránit proti libovolného typu neoprávněného přístupu. Zabezpečení systému musí toomake se, že jsou pravidla brány firewall na místě, takže jenom oprávnění IP adresy se mohou připojit. Musí být správné ověření přihlašovacích údajů uživatele. Poté, co uživatel má připojen toohello databáze, hello uživatele by měla mít pouze tooperform oprávnění minimální počet akcí. toosecure data, můžete použít šifrování. Je také důležité toohave auditování a sledování tak události můžete vrátit, pokud je podezřelé aktivity.

toolearn o správě zabezpečení, head přes toohello [Přehled zabezpečení][Security overview].

## <a name="backup-and-restore"></a>Zálohování a obnovení
Spolehlivé backps vašich dat je nedílnou součást vámi vyžádaných žádné produkční databázi. SQL Data Warehouse zajišťuje dat bezpečné zálohováním automaticky aktivní databáze v pravidelných intervalech. Tyto zálohy povolit toorecover z scénáře, kde jste poškozená data nebo neúmyslně vyřazen data nebo databáze.  Pro hello plán zálohování dat, zásady uchovávání informací a jak zjistit, toorestore databáze, [obnovení ze snímku][Restore from snapshot].

## <a name="next-steps"></a>Další kroky
Pomocí zásady designu dobrý databáze bude snazší toomanage vaše databáze v SQL Data Warehouse. Další, toolearn head přes toohello [přehled vývoje][Development overview].

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Create a database (PowerShell)]: sql-data-warehouse-get-started-provision-powershell.md
[connection]: sql-data-warehouse-develop-connections.md
[Query Azure SQL Data Warehouse with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Connect and query with sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Development overview]: sql-data-warehouse-overview-develop.md
[Monitor your workload using DMVs]: sql-data-warehouse-manage-monitor.md
[Pause compute]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Restore from snapshot]: sql-data-warehouse-restore-database-overview.md
[Resume compute]: sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[škálování výkonu]: sql-data-warehouse-manage-compute-overview.md#scale-compute
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse system views]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
