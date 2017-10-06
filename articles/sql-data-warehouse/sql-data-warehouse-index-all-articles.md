---
title: "témata aaaAll pro službu SQL Data Warehouse | Microsoft Docs"
description: "Tabulka všech témat pro hello služby Azure s názvem SQL Data Warehouse, který neexistuje v http://azure.microsoft.com/documentation/articles/, název a popis."
services: sql-data-warehouse
documentationcenter: 
author: barbkess
manager: jhubbard
editor: 
ms.assetid: a26a6dec-9c08-4415-8f58-4ee1dd41f718
ms.service: sql-data-warehouse
ms.workload: sql-data-warehouse
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: reference
ms.date: 03/30/2017
ms.author: barbkess
ms.openlocfilehash: 6f71d35b76b50764a5904525445675dafaa56b85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Všechna témata týkající se služby Azure SQL Data Warehouse
Toto téma obsahuje seznam každých téma, které se vztahuje přímo toohello **SQL Data Warehouse** služby Azure. Tato webová stránka pro klíčová slova můžete hledat pomocí **Ctrl + F**, toofind hello témata týkající se aktuální.

## <a name="new"></a>Nový
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 1 |[Zálohování SQL Data Warehouse](sql-data-warehouse-backups.md) |Další informace o zálohování integrovanou databází SQL Data Warehouse, které umožňují toorestore bod obnovení tooa Azure SQL Data Warehouse nebo jiné zeměpisné oblasti. |

## <a name="updated-articles-sql-data-warehouse"></a>Aktualizované články, datový sklad SQL
Tato část obsahuje články, které byly nedávno aktualizován, kde hello aktualizovat byla velká nebo důležité. Pro každý článek aktualizované hrubý fragmentu hello přidat markdownu text se zobrazí. články Hello se aktualizovaly v rámci hello obdobím **2016-08-22** příliš**2016-10-05**.

| &nbsp; | Článek | Aktualizovaný text, fragment kódu | Při aktualizaci |
| ---:|:--- |:--- |:--- |
| 2 |[Načtení dat z Azure blob storage do SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) |/ tootrack bajtů a soubory vyberte r.command, s.request_id, r.status, počet (odlišné input_name) jako nbr_files, sum (s.bytes_processed) / 1024/1024 jako gb_processed z sys.dm_pdw_exec_requests r vnitřní spojení sys.dm_pdw_dms_external_work s na r.request_id = s.request_id WHERE r. Popisek = ' funkce CTAS: cso zatížení. DimProduct ' OR r. Popisek = ' funkce CTAS: cso zatížení. 'FactOnlineSales GROUP BY r.command, s.request_id, r.status Order nbr_files desc, gb_processed desc; |2016-09-07 |
| 3 |[Obnovení SQL Data Warehouse](sql-data-warehouse-restore-database-overview.md) |** Lze obnovit pozastavenou datového skladu? ** toorestore datového skladu, který je pozastaven, budete potřebovat toofirst převeďte ho zpátky do online režimu. Až hello datový sklad je zpět do režimu online, budete mít sedmi dnech toochoose body obnovení z. ** Obnovit tooa geograficky redundantní oblast ** Pokud používáte hello geograficky redundantní úložiště, můžete obnovit hello datového skladu tooyour spárované datového centra v jiné zeměpisné oblasti. Hello datového skladu je obnoven ze hello poslední denní zálohování. ** Obnovte časová osa ** můžete obnovit do databáze tooany obnovení bodu v rámci hello posledních sedmi dnů. Snímky spuštění každé čtyři hodiny tooeight a jsou k dispozici sedm dní. Pokud snímek je starší než 7 dní, vypršení jeho platnosti a jeho bod obnovení již není k dispozici. ** Obnovte náklady ** hello úložiště zdarma pro hello obnovit datového skladu se fakturuje Azure Premium Storage rychlostí hello. Pokud pozastavíte obnovené datového skladu, budou se vám účtovat pro úložiště Azure Premium Storage rychlostí hello. Výhodou Hello pozastavení je, že nejste zdarma |2016-09-29 |

## <a name="get-started"></a>Začínáme
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 4 |[TooAzure ověřování SQL datového skladu](sql-data-warehouse-authentication.md) |Azure Active Directory (AAD) a SQL Server ověřování tooAzure SQL Data Warehouse. |
| 5 |[Osvědčené postupy pro službu Azure SQL Data Warehouse](sql-data-warehouse-best-practices.md) |Doporučení a osvědčené postupy, které byste měli znát, když budete vyvíjet řešení pro službu Azure SQL Data Warehouse. Pomohou vám stát se úspěšnými. |
| 6 |[Ovladače pro Azure SQL Data Warehouse](sql-data-warehouse-connection-strings.md) |Připojovací řetězce a ovladače pro datový sklad SQL |
| 7 |[Připojit tooAzure SQL Data Warehouse](sql-data-warehouse-connect-overview.md) |Jak název serveru hello toofind a připojovací řetězce pro vaše tooAzure SQL Data Warehouse |
| 8 |[Analýza dat pomocí Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) |Pomocí Azure Machine Learning toobuild prediktivní strojového učení modelu na základě dat uložené v Azure SQL Data Warehouse. |
| 9 |[Dotaz na Azure SQL Data Warehouse (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) |Dotaz na Azure SQL Data Warehouse pomocí sqlcmd hello nástroj příkazového řádku. |
| 10 |[Vytvořit databázi SQL Data Warehouse pomocí Transact-SQL (TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) |Zjistěte, jak toocreate Azure SQL datového skladu pomocí TSQL |
| 11 |[Jak toocreate podporu lístek pro datový sklad SQL](sql-data-warehouse-get-started-create-support-ticket.md) |Jak toocreate podporu lístek v Azure SQL Data Warehouse. |
| 12 |[Načtení dat pomocí Azure Data Factory.](sql-data-warehouse-get-started-load-with-azure-data-factory.md) |Další informace tooload dat pomocí Azure Data Factory |
| 13 |[Načtení dat pomocí funkce PolyBase v SQL Data Warehouse](sql-data-warehouse-get-started-load-with-polybase.md) |Zjistěte, co je PolyBase a jak toouse pro scénáře datových skladů. |
| 14 |[Vytvoření služby Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md) |Zjistěte, jak hello toocreate na Azure SQL Data Warehouse v portálu Azure |
| 15 |[Vytvoření SQL Data Warehouse pomocí prostředí PowerShell](sql-data-warehouse-get-started-provision-powershell.md) |Vytvoření SQL Data Warehouse pomocí prostředí PowerShell |
| 16 |[Vizualizace dat pomocí Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) |Vizualizace dat SQL Data Warehouse pomocí Power BI |
| 17 |[Dotazování Azure SQL Data Warehouse (Visual Studio)](sql-data-warehouse-query-visual-studio.md) |Dotazování SQL Data Warehouse pomocí sady Visual Studio. |

## <a name="develop"></a>Vývoj
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 18 |[Optimalizace transakcí pro SQL Data Warehouse](sql-data-warehouse-develop-best-practices-transactions.md) |Příručky nejlepších praktik o zápisu efektivní transakce aktualizace v Azure SQL Data Warehouse |
| 19 |[Souběžnost a úlohy správy v SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md) |Porozumět souběžnosti a úlohy správy v Azure SQL Data Warehouse pro vývoj řešení. |
| 20 |[Vytvoření tabulky jako vyberte (funkce CTAS) v SQL Data Warehouse](sql-data-warehouse-develop-ctas.md) |Tipy pro psaní kódu s použitím hello vytvořit tabulku jako vyberte příkaz (funkce CTAS) v Azure SQL Data Warehouse na vývoj řešení. |
| 21 |[Dynamické SQL v SQL Data Warehouse](sql-data-warehouse-develop-dynamic-sql.md) |Tipy pro používání dynamických SQL v Azure SQL Data Warehouse pro vývoj řešení. |
| 22 |[Seskupit podle možností v SQL Data Warehouse](sql-data-warehouse-develop-group-by-options.md) |Tipy pro implementaci skupiny pomocí možnosti v Azure SQL Data Warehouse na vývoj řešení. |
| 23 |[Pomocí popisků tooinstrument dotazů v SQL Data Warehouse](sql-data-warehouse-develop-label.md) |Tipy pro používání dotazy tooinstrument popisky v Azure SQL Data Warehouse pro vývoj řešení. |
| 24 |[Smyčky v SQL Data Warehouse](sql-data-warehouse-develop-loops.md) |Tipy pro smyčky Transact-SQL a nahraďte kurzory v Azure SQL Data Warehouse na vývoj řešení. |
| 25 |[Uložené procedury v SQL Data Warehouse](sql-data-warehouse-develop-stored-procedures.md) |Tipy pro implementaci uložené procedury v Azure SQL Data Warehouse na vývoj řešení. |
| 26 |[Transakce v SQL Data Warehouse](sql-data-warehouse-develop-transactions.md) |Tipy pro provádění transakcí v Azure SQL Data Warehouse na vývoj řešení. |
| 27 |[Uživatelem definované schémata v SQL Data Warehouse](sql-data-warehouse-develop-user-defined-schemas.md) |Tipy pro používání schémata Transact-SQL v Azure SQL Data Warehouse pro vývoj řešení. |
| 28 |[Přiřazení proměnné v SQL Data Warehouse](sql-data-warehouse-develop-variable-assignment.md) |Tipy pro přiřazení proměnné jazyka Transact-SQL v Azure SQL Data Warehouse na vývoj řešení. |
| 29 |[Zobrazení v SQL Data Warehouse](sql-data-warehouse-develop-views.md) |Tipy pro používání zobrazení jazyka Transact-SQL v Azure SQL Data Warehouse na vývoj řešení. |
| 30 |[Rozhodnutí o návrhu a kódování techniky pro SQL Data Warehouse](sql-data-warehouse-overview-develop.md) |Vývoj koncepty, rozhodnutí o návrhu, doporučení a kódování techniky pro SQL Data Warehouse. |

## <a name="manage"></a>Spravovat
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 31 |[Spravovat výpočetní výkon v Azure SQL Data Warehouse (přehled)](sql-data-warehouse-manage-compute-overview.md) |Výkon škálování možnosti v Azure SQL Data Warehouse. Horizontální navýšení kapacity úpravou Dwu nebo pozastavení a obnovení toosave náklady na výpočetní prostředky. |
| 32 |[Spravovat výpočetní výkon v Azure SQL Data Warehouse (portál Azure)](sql-data-warehouse-manage-compute-portal.md) |Portál Azure úlohy toomanage výpočetní výkon. Škálovat výpočetní prostředky úpravou Dwu. Nebo, pozastavení a obnovení toosave náklady na výpočetní prostředky. |
| 33 |[Spravovat výpočetní výkon v Azure SQL Data Warehouse (PowerShell)](sql-data-warehouse-manage-compute-powershell.md) |Prostředí PowerShell úlohy toomanage výpočetní výkon. Škálovat výpočetní prostředky úpravou Dwu. Nebo, pozastavení a obnovení toosave náklady na výpočetní prostředky. |
| 34 |[Spravovat výpočetní výkon v Azure SQL Data Warehouse (REST)](sql-data-warehouse-manage-compute-rest-api.md) |Prostředí PowerShell úlohy toomanage výpočetní výkon. Škálovat výpočetní prostředky úpravou Dwu. Nebo, pozastavení a obnovení toosave náklady na výpočetní prostředky. |
| 35 |[Spravovat výpočetní výkon v Azure SQL Data Warehouse (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) |Příkaz Transact-SQL (T-SQL) úlohy se na více systémů tooscale výkonu úpravou Dwu. Uložte náklady škálování zpět v době mimo špičku. |
| 36 |[Monitorování úloh pomocí zobrazení dynamických zpráv](sql-data-warehouse-manage-monitor.md) |Zjistěte, jak toomonitor úlohy pomocí zobrazení dynamické správy. |
| 37 |[Spravovat databáze v Azure SQL Data Warehouse](sql-data-warehouse-overview-manage.md) |Přehled správy databází SQL Data Warehouse. Zahrnuje nástroje pro správu, Dwu a Škálováním na více systémů, řešení potíží s výkon dotazů, vytvoření zásad zabezpečení a obnovení databáze z poškození dat nebo z místní výpadku výkonu. |
| 38 |[Monitorování uživatelských dotazů v Azure SQL Data Warehouse](sql-data-warehouse-overview-manage-user-queries.md) |Přehled aspektů hello, osvědčené postupy a úlohy pro monitorování uživatelských dotazů v Azure SQL Data Warehouse |
| 39 |[Obnovení SQL Data Warehouse](sql-data-warehouse-restore-database-overview.md) |Přehled možností obnovení hello databáze pro obnovení databáze v Azure SQL Data Warehouse. |
| 40 |[Obnovit Azure SQL Data Warehouse (portál)](sql-data-warehouse-restore-database-portal.md) |Azure portálu úlohy pro obnovení Azure SQL Data Warehouse. |
| 41 |[Obnovit Azure SQL Data Warehouse (PowerShell)](sql-data-warehouse-restore-database-powershell.md) |Prostředí PowerShell úlohy pro obnovení Azure SQL Data Warehouse. |
| 42 |[Obnovit Azure SQL Data Warehouse (REST API)](sql-data-warehouse-restore-database-rest-api.md) |Úlohy REST API pro obnovení Azure SQL Data Warehouse. |

## <a name="tables-and-indexes"></a>Tabulky a indexy
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 43 |[Datové typy pro tabulky v SQL Data Warehouse](sql-data-warehouse-tables-data-types.md) |Začínáme s datovými typy pro tabulky Azure SQL Data Warehouse. |
| 44 |[Distribuce tabulek v SQL Data Warehouse](sql-data-warehouse-tables-distribute.md) |Začínáme s distribuci tabulek v Azure SQL Data Warehouse. |
| 45 |[Indexování tabulek v SQL Data Warehouse](sql-data-warehouse-tables-index.md) |Začínáme s tabulkou indexování v Azure SQL Data Warehouse. |
| 46 |[Přehled tabulek v SQL Data Warehouse](sql-data-warehouse-tables-overview.md) |Začínáme s Azure SQL Data Warehouse tabulky. |
| 47 |[Vytváření oddílů tabulky v SQL Data Warehouse](sql-data-warehouse-tables-partition.md) |Začínáme s vytváření oddílů tabulky v Azure SQL Data Warehouse. |
| 48 |[Správa statistiky u tabulek v SQL Data Warehouse](sql-data-warehouse-tables-statistics.md) |Začínáme s statistiky pro tabulky v Azure SQL Data Warehouse. |
| 49 |[Dočasné tabulky v SQL Data Warehouse](sql-data-warehouse-tables-temporary.md) |Začínáme s dočasných tabulek v Azure SQL Data Warehouse. |

## <a name="integrate"></a>Integrace
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 50 |[Použít pro vytváření dat Azure s datovým skladem SQL](sql-data-warehouse-integrate-azure-data-factory.md) |Tipy pro používání Azure Data Factory (ADF) s datovým skladem SQL Azure pro vývoj řešení. |
| 51 |[SQL Data Warehouse pomocí Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) |Kurz pro používání Azure Machine Learning s Azure SQL Data Warehousem pro vývoj řešení. |
| 52 |[Použití služby Azure Stream Analytics s SQL Data Warehouse](sql-data-warehouse-integrate-azure-stream-analytics.md) |Tipy pro používání Azure Stream Analytics s datovým skladem SQL Azure pro vývoj řešení. |
| 53 |[SQL Data Warehouse pomocí Power BI](sql-data-warehouse-integrate-power-bi.md) |Tipy pro používání Power BI s datovým skladem SQL Azure pro vývoj řešení. |
| 54 |[Využít dalším službám pomocí SQL Data Warehouse](sql-data-warehouse-overview-integrate.md) |Nástroje a partnery, se řešení, které se integrují se službou SQL Data Warehouse. |

## <a name="load"></a>Načtení
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 55 |[Načtení dat z Azure blob storage do Azure SQL Data Warehouse (Azure Data Factory)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) |Další informace tooload dat pomocí Azure Data Factory |
| 56 |[Načtení dat z Azure blob storage do SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) |Zjistěte, jak toouse PolyBase tooload dat z Azure blob storage do SQL Data Warehouse. Načtení několik tabulek z veřejná data do schématu hello Contoso prodejní datového skladu. |
| 57 |[Načtení dat z SQL serveru do Azure SQL Data Warehouse (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) |Využívá bcp tooexport data z tooflat soubory systému SQL Server, úložiště objektů blob tooAzure data tooimport AZCopy a PolyBase tooingest hello data do Azure SQL Data Warehouse. |
| 58 |[Načtení dat z SQL serveru do Azure SQL Data Warehouse (ploché soubory)](sql-data-warehouse-load-from-sql-server-with-bcp.md) |Pro malou velikost dat využívá bcp tooexport data z tooflat soubory systému SQL Server a importovat data hello přímo do Azure SQL Data Warehouse. |
| 59 |[Načtení dat z SQL serveru do Azure SQL Data Warehouse služby SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) |Ukazuje, jak toocreate toomove data balíčku integrační služby SSIS (SQL Server) ze široké škály data zdroje tooSQL datového skladu. |
| 60 |[Načtení dat pomocí funkce PolyBase v SQL Data Warehouse](sql-data-warehouse-load-from-sql-server-with-polybase.md) |Využívá bcp tooexport data z tooflat soubory systému SQL Server, úložiště objektů blob tooAzure data tooimport AZCopy a PolyBase tooingest hello data do Azure SQL Data Warehouse. |
| 61 |[Příručka pro používání funkce PolyBase v SQL Data Warehouse](sql-data-warehouse-load-polybase-guide.md) |Pokyny a doporučení pro používání funkce PolyBase v SQL Data Warehouse scénáře. |
| 62 |[Načtení ukázkových dat do SQL Data Warehouse](sql-data-warehouse-load-sample-databases.md) |Načtení ukázkových dat do SQL Data Warehouse |
| 63 |[Načtení dat pomocí bcp](sql-data-warehouse-load-with-bcp.md) |Zjistěte, co je bcp a jak toouse pro scénáře datových skladů. |
| 64 |[Načtení dat do Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md) |Další běžné scénáře hello dat načítání do SQL Data Warehouse. Patří sem pomocí PolyBase, úložiště objektů blob v Azure, plochých souborů a přesouvání disku. Můžete také použít nástroje třetích stran. |

## <a name="migrate"></a>Migrace
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 65 |[Migrace vaší tooSQL kódu SQL datového skladu](sql-data-warehouse-migrate-code.md) |Tipy pro migraci vaší SQL kódu tooAzure SQL Data Warehouse na vývoj řešení. |
| 66 |[Migrace dat](sql-data-warehouse-migrate-data.md) |Tipy pro migraci vaše data tooAzure SQL Data Warehouse na vývoj řešení. |
| 67 |[Nástroj migrace data Warehouse (Preview)](sql-data-warehouse-migrate-migration-utility.md) |Migrujte tooSQL datového skladu. |
| 68 |[Migrace vaší tooSQL schématu datového skladu](sql-data-warehouse-migrate-schema.md) |Tipy pro migraci vašeho schématu tooAzure SQL Data Warehouse na vývoj řešení. |
| 69 |[Migrace vašeho řešení tooSQL datového skladu](sql-data-warehouse-overview-migrate.md) |Migrace pokyny k uvedení platforma SQL Data Warehouse tooAzure vaše řešení. |

## <a name="partners"></a>Partneři
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 70 |[SQL Data Warehouse business intelligence partnery](sql-data-warehouse-partner-business-intelligence.md) |Seznamy partnerů třetích stran business intelligence s řešení, které podporují SQL Data Warehouse. |
| 71 |[Partneři integraci dat SQL Data Warehouse](sql-data-warehouse-partner-data-integration.md) |Seznam partnerů třetích stran s řešeními integraci dat, které podporují Azure SQL Data Warehouse. |
| 72 |[Partneři správy dat SQL Data Warehouse](sql-data-warehouse-partner-data-management.md) |Seznamy z partnerských serverů pro správu dat třetích stran s řešení, které podporují SQL Data Warehouse. |

## <a name="reference"></a>Referenční informace
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 73 |[Témata odkazů pro SQL Data Warehouse](sql-data-warehouse-overview-reference.md) |Referenční obsahu odkazy pro SQL Data Warehouse. |
| 74 |[Rutiny prostředí PowerShell a rozhraní REST API pro SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md) |Najít hello nejvyšší rutiny prostředí PowerShell pro Azure SQL Data Warehouse včetně jak toopause a obnovit databázi. |
| 75 |[Jazykové elementy](sql-data-warehouse-reference-tsql-language-elements.md) |Seznam odkazů tooreference obsah pro prvky jazyka Transact-SQL hello používané pro SQL Data Warehouse. |
| 76 |[Témata Transact-SQL](sql-data-warehouse-reference-tsql-statements.md) |Obsah tooreference odkazy pro témata Transact-SQL hello používá SQL Data Warehouse. |
| 77 |[Systémová zobrazení](sql-data-warehouse-reference-tsql-system-views.md) |Obsah zobrazení toosystem odkazy pro SQL Data Warehouse. |

## <a name="security"></a>Zabezpečení
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 78 |[SQL Data Warehouse – podpora klientů nižší úrovně pro auditování a dynamické maskování dat](sql-data-warehouse-auditing-downlevel-clients.md) |Další informace o podpoře klientů nižší úrovně SQL Data Warehouse pro auditování dat |
| 79 |[Auditování v Azure SQL Data Warehouse](sql-data-warehouse-auditing-overview.md) |Začínáme s auditování v Azure SQL Data Warehouse |
| 80 |[Začínáme s transparentní dat šifrování (TDE) v SQL Data Warehouse](sql-data-warehouse-encryption-tde.md) |Transparentní šifrování dat (šifrování TDE) v SQL Data Warehouse |
| 81 |[Začínáme s transparentní šifrování dat (TDE)](sql-data-warehouse-encryption-tde-tsql.md) |Transparentní šifrování dat (šifrování TDE) v datovém skladu SQL (T-SQL) |
| 82 |[Zabezpečení databáze v SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md) |Tipy pro zabezpečení databáze v Azure SQL Data Warehouse na vývoj řešení. |

## <a name="miscellaneous"></a>Různé
| &nbsp; | Název | Popis |
| ---:|:--- |:--- |
| 83 |[Instalace sady Visual Studio a SSDT pro SQL Data Warehouse](sql-data-warehouse-install-visual-studio.md) |Instalace sady Visual Studio a SQL Server Development Tools (SSDT) pro Azure SQL Data Warehouse |
| 84 |[Migrace tooPremium podrobnosti úložiště](sql-data-warehouse-migrate-to-premium-storage.md) |Pokyny k migraci existující toopremium úložiště SQL Data Warehouse |
| 85 |[Začínáme s detekce hrozeb.](sql-data-warehouse-security-threat-detection.md) |Jak tooget pracovat s detekce hrozeb. |
| 86 |[Limity kapacity SQL Data Warehouse](sql-data-warehouse-service-capacity-limits.md) |Maximální hodnoty pro připojení, databáze, tabulky a dotazy pro SQL Data Warehouse. |
| 87 |[Řešení potíží s datovým skladem Azure SQL](sql-data-warehouse-troubleshoot.md) |Řešení potíží s datovým skladem Azure SQL. |

