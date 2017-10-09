---
title: aaaSync dat (Preview) | Microsoft Docs
description: "Tento přehled zavádí synchronizaci dat SQL Azure (Preview)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: douglasl
ms.openlocfilehash: d5b2bbd6a502ba94dba7fb309a6583d2d95cc1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync"></a>Synchronizaci dat mezi několika databází cloudu a místně s synchronizaci dat SQL

Synchronizaci dat SQL je služba založená na Azure SQL Database, která umožňuje synchronizaci dat hello služby, kterou vyberete obousměrně napříč více databází SQL a instance systému SQL Server.

Synchronizace dat je založena kolem hello koncept synchronizace skupiny. Synchronizace skupiny je skupina databází, které chcete toosynchronize.

Synchronizace skupiny má hello následující vlastnosti:

-   Hello **schématu synchronizace** popisuje data, která se synchronizuje.

-   Hello **směr synchronizace** může být obousměrně nebo můžete pouze v jednom směru toku. To znamená, hello směr synchronizace může být *rozbočovače tooMember* nebo *člen tooHub*, nebo obojí.

-   Hello **Interval synchronizace** jak často dochází k synchronizaci.

-   Hello **zásady překladu IP adres konflikt** je úrovni zásady skupiny, který může být *rozbočovače wins* nebo *člen wins*.

Synchronizaci dat používá hvězdicové topologie toosynchronize data. Definujte jedné z hello databází ve skupině hello jako hello databáze rozbočovače. Hello zbytek hello databáze jsou databází člena. Synchronizace nastávají jenom mezi hello rozbočovače a jednotlivé členy.
-   Hello **rozbočovače databáze** musí být databáze SQL Azure.
-   Hello **databází člena** může být databáze SQL, databáze systému SQL Server pro místní nebo instance systému SQL Server na virtuálních počítačích Azure.
-   Hello **databáze Sync** obsahuje hello metadata a protokolu pro synchronizaci dat. hello databáze Sync má toobe databáze SQL Azure, který je umístěný v hello stejné oblasti jako hello databáze rozbočovače. Hello databáze Sync je zákazník vytvořit a zákazníka, které jsou ve vlastnictví.

> [!NOTE]
> Pokud používáte databázi na místní, máte příliš[konfigurace místního agenta.](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-sql-data-sync)

![Synchronizaci dat mezi databázemi](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-toouse-data-sync"></a>Při synchronizaci dat toouse

Synchronizace dat je užitečné v případech, kdy dat je toobe zachovány až toodate napříč několika databází SQL Azure nebo databáze systému SQL Server. Zde jsou hello případů se využívá pro synchronizaci dat:

-   **Synchronizace dat hybridní:** se synchronizací dat, abyste zajistili, že data synchronizovat mezi vaší místní databáze a databáze SQL Azure tooenable hybridní aplikace. Tato funkce může odvolat toocustomers, který zvažuje přesunutí toohello cloudu a chcete tooput některé své aplikace v Azure.

-   **Distribuované aplikace:** v mnoha případech je výhodné tooseparate různé úlohy v různých databází. Například pokud máte velké provozní databáze, ale také potřebujete toorun generování sestav nebo analýza zatížení na tato data, je užitečné toohave druhý databáze pro tento další úlohy. Tento postup minimalizuje dopad na výkon hello na vaše produkční úlohy. Můžete použít synchronizaci dat tookeep synchronizovat zdrojová a cílová databáze.

-   **Globálně distribuované aplikace:** mnoho firem rozmístěny v několika oblastech a i několika zemích. toominimize latence sítě, je nejlepší toohave zavřete data v oblasti tooyou. S synchronizaci dat se snadnou vejdou databází v oblastech kolem hello, world synchronizovány.

Není doporučeno synchronizaci dat pro hello následující scénáře:

-   Zotavení po havárii

-   Škálování pro čtení

-   ETL (OLTP tooOLAP)

-   Migrace ze systému SQL Server tooAzure místní databáze SQL

## <a name="how-does-data-sync-work"></a>Jak funguje synchronizaci dat? 

-   **Sledování změn dat:** synchronizaci dat sleduje změny pomocí vložit, aktualizovat a odstranit aktivační události. změny Hello se zaznamenávají v tabulce straně v hello uživatelské databáze.

-   **Synchronizace dat:** synchronizaci dat slouží v hvězdicové modelu. Hello rozbočovače synchronizuje s každý člen jednotlivě. Změny z hello centra jsou stažené toohello člen a pak změny z hello člen jsou nahrané toohello rozbočovače.

-   **Řešení konfliktů:** synchronizaci dat nabízí dvě možnosti pro řešení konfliktů *rozbočovače wins* nebo *člen wins*.
    -   Pokud vyberete *rozbočovače wins*, hello změny v centru hello vždy přepsat změny ve členovi hello.
    -   Pokud vyberete *člen wins*, hello změny v hello člena přepsat, změny v centru hello. Pokud existuje více než jednoho člena, na které člen synchronizuje se nejprve závisí hello konečná hodnota.

## <a name="limitations-and-considerations"></a>Omezení a důležité informace

### <a name="performance-impact"></a>Vliv na výkon
Synchronizace dat se používají vložit, aktualizovat a odstranit změny tootrack aktivační události. Vytvoří straně tabulky v databázi hello uživatele pro sledování změn. Tyto aktivity sledování změn mají vliv na vaše zatížení databáze. Vyhodnocení vašeho vrstvy služeb a upgradujte v případě potřeby.

### <a name="eventual-consistency"></a>Konzistence typu případné
Vzhledem k tomu, že synchronizace dat je na základě aktivační události, není zaručena konzistence transakcí. Microsoft zaručuje, že jsou všechny změny provedené nakonec a synchronizaci dat není způsobit ztrátu dat..

### <a name="unsupported-data-types"></a>Nepodporované datové typy

-   FileStream

-   UDT SQL/CLR

-   Kolekci XMLSchemaCollection (XML podporována)

-   Kurzor, časové razítko, Hierarchyid

### <a name="requirements"></a>Požadavky

-   Každá tabulka musí mít primární klíč.

-   Tabulka nemůže obsahovat sloupec identity, který není hello primární klíč.

-   Hello názvy objektů (databáze, tabulek a sloupců) nesmí obsahovat hello tisknutelná znaky tečka (.), zbývající hranaté závorky ([]), nebo přímo hranatá závorka (]).

### <a name="limitations-on-service-and-database-dimensions"></a>Omezení dimenzí služby a databáze

|                                                                 |                        |                             |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| **Dimenze**                                                      | **Limit**              | **Alternativní řešení**              |
| Maximální počet skupin synchronizace všechny databáze může patřit do.       | 5                      |                             |
| Maximální počet koncových bodů ve skupině jedním synchronizačním              | 30                     | Vytvoření více skupin synchronizace |
| Maximální počet koncových bodů místně v jednom synchronizace skupiny. | 5                      | Vytvoření více skupin synchronizace |
| Databáze, tabulky, schémat a sloupce                       | 50 znaků na název |                             |
| Tabulky ve skupině pro synchronizaci                                          | 500                    | Vytvoření více skupin synchronizace |
| Sloupce v tabulce v skupiny synchronizace                              | 1000                   |                             |
| Velikost řádek dat v tabulce                                        | 24 mb                  |                             |
| Interval minimální synchronizace                                           | 5 minut              |                             |

## <a name="common-questions"></a>Nejčastější dotazy

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Jak často můžete synchronizaci dat synchronizovat data? 
minimální frekvence Hello je každých pět minut.

### <a name="can-i-use-data-sync-toosync-between-sql-server-on-premises-databases-only"></a>Můžete použít toosync synchronizaci dat mezi pouze místní databáze systému SQL Server? 
Ne přímo. Pro synchronizaci mezi místní databáze systému SQL Server nepřímo, ale vytvoření centra databáze v Azure a pak přidáním hello místní databáze toohello synchronizace skupiny.
   
### <a name="can-i-use-data-sync-tooseed-data-from-my-production-database-tooan-empty-database-and-then-keep-them-synchronized"></a>Lze používat synchronizaci dat tooseed data z mé produkční tooan prázdné databáze a pak je synchronizovat zachovat? 
Ano. Vytvořte schéma hello ručně v hello novou databázi pomocí skriptování z původní hello. Po vytvoření hello schéma, přidat hello tabulky tooa synchronizaci skupiny toocopy hello dat a udržovat synchronizované.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Proč se zobrazuje, tabulky, které I nevytvořila?  
Synchronizaci dat vytváří straně tabulky v databázi pro sledování změn. Neodstraňujte ho nebo synchronizaci dat přestane fungovat.
   
### <a name="i-got-an-error-message-that-said-cannot-insert-hello-value-null-into-hello-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-hello-error"></a>Chybová zpráva, která je uvedená "nelze vložit hello hodnoty NULL do sloupce hello \<sloupec\>. Sloupec nepovoluje hodnoty Null." Co to znamená, a jak můžete opravit chyby hello? 
Tato chybová zpráva indikuje mezi hello dvě následující problémy:
1.  Může být tabulky bez primárního klíče. toofix-li tento problém, přidejte primární klíče tooall hello tabulky se synchronizuje.
2.  Může být klauzule WHERE v příkazu CREATE INDEX. Synchronizace nezpracovává tuto podmínku. toofix tento problém, odeberte hello klauzule WHERE, nebo ručně provést změny hello tooall databáze. 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-hello-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Jak synchronizaci dat zpracovat. cyklické odkazy? To znamená, když hello stejná data je synchronizovaný v několika skupinách pro synchronizaci a udržuje v důsledku změny?
Synchronizaci dat nemůže pracovat. cyklické odkazy. Být jisti tooavoid je. 

## <a name="next-steps"></a>Další kroky

Další informace o synchronizaci dat SQL najdete v tématu:

-   [Začínáme s synchronizaci dat SQL](sql-database-get-started-sql-data-sync.md)

-   Dokončit příklady prostředí PowerShell, které ukazují, jak tooconfigure synchronizaci dat SQL:
    -   [Použití prostředí PowerShell toosync mezi více databází Azure SQL](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Použití prostředí PowerShell toosync mezi databáze SQL Azure a místní databáze SQL serveru](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Stáhnout hello úplnou synchronizaci dat SQL technická dokumentace](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)

-   [Stáhněte si dokumentaci k REST API služby SQL Data synchronizace hello](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Další informace o databázi SQL najdete v tématu:

-   [Databáze SQL – přehled](sql-database-technical-overview.md)

-   [Správa životního cyklu databáze](https://msdn.microsoft.com/library/jj907294.aspx)
