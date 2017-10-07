---
title: aaaTroubleshooting Azure SQL Data Warehouse | Microsoft Docs
description: "Řešení potíží s datovým skladem Azure SQL."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/30/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 3c53a39f77057fe0bcc053a0b896555abf397515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Řešení potíží s datovým skladem Azure SQL
Toto téma obsahuje seznam některých hello častější řešení problémů s dotazy, které jsme od vás odpověď od našich zákazníků.

## <a name="connecting"></a>Připojení
| Problém | Řešení |
|:--- |:--- |
| Přihlášení uživatele 'Přihlášení NT AUTHORITY\ANONYMOUS' se nezdařilo. (Microsoft SQL Server, chyba: 18456) |K této chybě dojde, když uživatel s AAD pokusí tooconnect toohello hlavní databázi, ale nemá uživatel v předloze.  Tento problém buď zadejte toocorrect hello SQL Data Warehouse chcete čas připojení tooat tooconnect nebo přidejte hello uživatele toohello hlavní databázi.  V tématu [Přehled zabezpečení] [ Security overview] další podrobnosti najdete v článku. |
| server Hello hlavní "uživatelské_jméno" není možné tooaccess hello databáze "hlavní" v části hello aktuální kontext zabezpečení. Výchozí databázi uživatele nelze otevřít. Přihlášení se nezdařilo. Přihlášení uživatele 'Uživatelské_jméno' se nezdařilo. (Microsoft SQL Server, chyba: 916) |K této chybě dojde, když uživatel s AAD pokusí tooconnect toohello hlavní databázi, ale nemá uživatel v předloze.  Tento problém buď zadejte toocorrect hello SQL Data Warehouse chcete čas připojení tooat tooconnect nebo přidejte hello uživatele toohello hlavní databázi.  V tématu [Přehled zabezpečení] [ Security overview] další podrobnosti najdete v článku. |
| Chyba CTAIP |Této chybě může dojít po vytvoření přihlášení na hello hlavní databáze systému SQL server, ale není v databázi SQL Data Warehouse hello.  Pokud dojde k této chybě, prohlédněte si hello [Přehled zabezpečení] [ Security overview] článku.  Tento článek vysvětluje, jak vytvořit toocreate přihlášení a uživatele na serveru a poté jak hello toocreate a uživatele v databázi SQL Data Warehouse. |
| Blokováno bránou Firewall |Databáze Azure SQL jsou chráněny známé pouze IP adresy, které mají přístup tooa databáze serveru a databáze tooensure úrovně brány firewall. brány firewall Hello jsou ve výchozím nastavení, což znamená, že je potřeba explicitně povolit a IP adresu nebo rozsah adres zabezpečené před připojením.  tooconfigure brány firewall pro přístup, postupujte podle kroků hello v [konfigurovat přístup k serveru brány firewall pro vaše IP adresa klienta] [ Configure server firewall access for your client IP] v hello [zřizování pokyny] [Provisioning instructions]. |
| Nelze se připojit s nástrojem pro nebo ovladače |SQL Data Warehouse doporučuje použít [SSMS][SSMS], [SSDT pro Visual Studio][SSDT for Visual Studio], nebo [sqlcmd] [ sqlcmd] tooquery vaše data. Další informace o ovladače a připojování tooSQL datového skladu v [ovladače pro Azure SQL Data Warehouse] [ Drivers for Azure SQL Data Warehouse] a [připojit tooAzure SQL Data Warehouse] [ Connect tooAzure SQL Data Warehouse] články. |

## <a name="tools"></a>Nástroje
| Problém | Řešení |
|:--- |:--- |
| Průzkumník objektů systému Visual Studio chybí uživatele AAD |Jedná se o známý problém.  Jako alternativní řešení, zobrazení hello uživatelů v [sys.database_principals][sys.database_principals].  V tématu [tooAzure ověřování SQL Data Warehouse] [ Authentication tooAzure SQL Data Warehouse] toolearn Další informace o používání služby Azure Active Directory s SQL Data Warehouse. |
|Ruční skriptování, pomocí Průvodce skriptování hello nebo připojení přes SSMS je pomalé, "zamrzlých" nebo vytváření chyb| Ujistěte se, že uživatelé byla vytvořena v hlavní databázi hello. V možnosti skriptování Ujistěte se také že hello modul edition je nastaven jako "Microsoft Azure SQL Data Warehouse Edition" a "Microsoft Azure SQL Database" je typ modulu.|

## <a name="performance"></a>Výkon
| Problém | Řešení |
|:--- |:--- |
| Řešení potíží s výkonem dotazu |Pokud se pokoušíte tootroubleshoot konkrétní dotaz, začínat [učení jak toomonitor své dotazy][Learning how toomonitor your queries]. |
| Dotaz nízký výkon a plány často je výsledkem chybějící statistiky |Nejběžnější příčina Hello nízký výkon je nedostatek statistik na tabulky.  V tématu [zachování statistiky tabulky] [ Statistics] podrobnosti o tom toocreate statistiky a proč jsou kritické tooyour výkonu. |
| Nízkou souběžnosti / ve frontě dotazů |Principy [úlohy správy] [ Workload management] je důležité v pořadí toounderstand jak toobalance přidělení paměti se souběžností. |
| Jak tooimplement osvědčené postupy |je technologie Hello místní toostart toolearn způsoby tooimprove dotazy vracely co nejlepší [osvědčené postupy pro SQL Data Warehouse] [ SQL Data Warehouse best practices] článku. |
| Jak tooimprove výkon při změně měřítka |Někdy je výkon tooimproving řešení hello toosimply přidat více výpočetního výkonu tooyour dotazy pomocí [škálování SQL Data Warehouse][Scaling your SQL Data Warehouse]. |
| Dotaz nízký výkon v důsledku nízký index kvality |Některé časy dotazy můžete zpomalení z důvodu [kvality indexu columnstore nízký][Poor columnstore index quality].  Najdete v tomto článku Další informace a jak příliš[znovu sestavit indexy kvality segment tooimprove][Rebuild indexes tooimprove segment quality]. |

## <a name="system-management"></a>Správa systému
| Problém | Řešení |
|:--- |:--- |
| Msg 40847: Nelze provést operaci hello, protože server by překročil povolené kvóty jednotky transakcí databáze 45000 hello. |Snižte hello [DWU] [ DWU] hello databáze se pokoušíte toocreate nebo [požádat o zvýšení kvóty][request a quota increase]. |
| Zkoumání využití místa |V tématu [tabulky velikosti] [ Table sizes] toounderstand hello místa využití vašeho systému. |
| Usnadnit správu tabulek |V tématu hello [tabulky přehled] [ Overview] článku Další informace o správě tabulek.  Tento článek také obsahuje odkazy na podrobnější témata jako [tabulky datové typy][Data types], [distribuci tabulku][Distribute], [indexování tabulku][Index], [vytváření oddílů tabulky][Partition], [zachování statistiky tabulky] [ Statistics] a [dočasných tabulek][Temporary]. |
|Indikátor průběhu šifrování (TDE) transparentní dat není aktualizace v hello portálu Azure|Můžete zobrazit stav hello TDE prostřednictvím [prostředí powershell](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption).|

## <a name="polybase"></a>Polybase
| Problém | Řešení |
|:--- |:--- |
| Zatížení se nezdaří z důvodu velkého řádků |Podpora velké řádek aktuálně není k dispozici pro Polybase.  To znamená, že pokud tabulka obsahuje VARCHAR(MAX), NVARCHAR(MAX) nebo VARBINARY(MAX), externí tabulky nemůže být použité tooload vaše data.  Načítání pro velké řádků je momentálně podporována pouze prostřednictvím Azure Data Factory (pomocí BCP), Azure Stream Analytics, SSIS, BCP nebo hello SQLBulkCopy rozhraní .NET třídy. PolyBase podporu pro velké řádky budou přidány v budoucí verzi. |
| selhává zatížení BCP tabulky s maximální datový typ |Je známý problém, který vyžaduje, že na konci hello hello tabulky v některých scénářích umístit VARCHAR(MAX), NVARCHAR(MAX) nebo VARBINARY(MAX).  Zkuste přesunout vaše maximální počet sloupců toohello konec hello tabulky. |

## <a name="differences-from-sql-database"></a>Rozdíly z databáze SQL
| Problém | Řešení |
|:--- |:--- |
| Nepodporované funkce databáze SQL |V tématu [nepodporované funkce tabulky][Unsupported table features]. |
| Nepodporované typy dat databáze SQL |V tématu [nepodporované datové typy][Unsupported data types]. |
| ODSTRANĚNÍ a omezení aktualizace |V tématu [alternativní řešení aktualizace][UPDATE workarounds], [odstranění řešení] [ DELETE workarounds] a [pomocí funkce CTAS toowork kolem nepodporované aktualizace a ODSTRANĚNÍ syntaxe][Using CTAS toowork around unsupported UPDATE and DELETE syntax]. |
| Příkaz MERGE není podporován. |V tématu [SLOUČENÍ řešení][MERGE workarounds]. |
| Omezení uložené procedury |V tématu [uložené procedury omezení] [ Stored procedure limitations] toounderstand některé hello omezení uložených procedur. |
| Funkce UDF nepodporují příkazy SELECT |Toto je aktuální omezení naše UDF.  V tématu [CREATE FUNCTION] [ CREATE FUNCTION] podporujeme syntaxe hello. |

## <a name="next-steps"></a>Další kroky
Pokud jste byly nelze toofind problémem tooyour řešení vyšší, tady jsou některé prostředky, které můžete zkusit.

* [Blogy]
* [Žádosti o funkce]
* [Videa]
* [Blogy týmu CAT]
* [Vytvoření lístku podpory]
* [Fórum MSDN]
* [Fórum Stack Overflow]
* [Twitter]

<!--Image references-->

<!--Article references-->
[Security overview]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT for Visual Studio]: ./sql-data-warehouse-install-visual-studio.md
[Drivers for Azure SQL Data Warehouse]: ./sql-data-warehouse-connection-strings.md
[Connect tooAzure SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Vytvoření lístku podpory]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Scaling your SQL Data Warehouse]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md
[request a quota increase]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Learning how toomonitor your queries]: ./sql-data-warehouse-manage-monitor.md
[Provisioning instructions]: ./sql-data-warehouse-get-started-provision.md
[Configure server firewall access for your client IP]: ./sql-data-warehouse-get-started-provision.md#create-a-server-level-firewall-rule-in-the-azure-portal
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[Table sizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild indexes tooimprove segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Workload management]: ./sql-data-warehouse-develop-concurrency.md
[Using CTAS toowork around unsupported UPDATE and DELETE syntax]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[UPDATE workarounds]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[DELETE workarounds]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[MERGE workarounds]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Stored procedure limitations]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentication tooAzure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md
[Working around hello PolyBase UTF-8 requirement]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[CREATE FUNCTION]: https://msdn.microsoft.com/library/mt203952.aspx
[sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogy]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogy týmu CAT]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Žádosti o funkce]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum MSDN]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Fórum Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videa]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
