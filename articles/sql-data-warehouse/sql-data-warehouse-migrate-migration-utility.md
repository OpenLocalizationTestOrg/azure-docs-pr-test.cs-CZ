---
title: "Migrace: Datového skladu nástroje Migrace | Microsoft Docs"
description: "Migrujte tooSQL datového skladu."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 4d7ad981-ef31-4513-b337-50bdc4709c09
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: c89909883fb42b0b04dd87a9973e5ee3e30d8f0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-warehouse-migration-utility-preview"></a>Nástroj migrace data Warehouse (Preview)
> [!div class="op_single_selector"]
> * [Stáhněte si nástroj pro migraci][Download Migration Utility]
> 
> 

Nástroj migrace datového skladu Hello je nástroj určený toomigrate schéma a data z tooAzure SQL Server a databáze SQL Azure SQL Data Warehouse. Během migrace schématu hello nástroj automaticky mapuje hello odpovídající schématu z toodestination zdroje. Po migraci schématu hello hello nástrojů poskytuje hello možnost toomove data se automaticky generovaných skriptů.

Kromě toho tooschema a data migrace, tento nástroj umožňuje hello možnost toogenerate kompatibility sestavy, které shrnují problémům s kompatibilitou mezi hello zdrojové a cílové instance, které by jinak znemožňovaly zjednodušený migrace.

## <a name="get-started"></a>Začínáme
Předpokladem pro instalaci budete potřebovat hello BCP nástroj příkazového řádku toorun migrační skripty a Office tooview hello kompatibility sestavy. Po spuštění hello bude spustitelný soubor, který byl stažen výzvami tooaccept standardní smlouvy EULA předtím, než se nainstaluje nástroj hello.

Navíc toorun hello Utiliy migrace, můžete se třeba hello jeden následující oprávnění v databázi hello hledáte toomigrate: vytvoření databáze, ALTER ANY DATABASE nebo ANY definice zobrazení.

### <a name="launching-hello-tool-and-connecting"></a>Spuštění nástroje hello a připojení
Nainstalujte nástroj pro spuštění hello kliknutím na ikony na ploše hello zobrazené post. Při otevírání hello nástroj, zobrazí se výzva počáteční připojení stránky, kde si můžete vybrat zdroj a cíl pro nástroj pro migraci hello. V současné době podporujeme systému SQL Server a Azure SQL Database jako zdroje a SQL Data Warehouse jako cíl. Po výběru to zobrazí se výzva tooconnect tooyour zdrojový server naplnění v názvu serveru a ověřování a potom klikněte na "Připojit".

Po ověření, zobrazí nástroj hello seznam databází, které se nacházejí v hello serveru, který jste připojeni k. Hello migrace můžete začít tak, že vyberete databázi, která chcete toomigrate a kliknete na 'Migrací vybrané'.

## <a name="migration-report"></a>Sestavy migrace
Výběr zkontrolovat kompatibilitu databáze v nástroji hello způsobí vygenerování sestavy shrnutí všech nekompatibility objektu v databázi hello požadujete toomigrate. Širší seznam některých hello funkci systému SQL Server, který se nenachází v SQL Data Warehouse najdete v našich [dokumentace k migraci][migration documentation]. Po vygenerování sestavy hello bude možné toosave a hello otevřete sestavu v aplikaci Excel.

Pamatujte, že při generování hello migrace schématu, identifikovány jako 'Objektu' je nutné upravit pořadí tooallow okamžité migraci dat většiny problémů. Přečtěte si tooensure změny hello nechcete toomake další úpravy před použitím schématu hello.

## <a name="migrate-schema"></a>Migrace schématu
Po připojení, vygeneruje výběr 'migraci schématu skript pro migraci schématu pro hello vybrané tabulky. Tato struktura skriptu porty hello hello tabulky mapuje nekompatibilní datové typy toomore kompatibilní formuláře a vytvoří zabezpečovací přihlašovací údaje a schéma, pokud to vyplývá z hello uživatele v nastavení migrace hello. Tento kód můžete spustit proti hello cílové instanci SQL Data Warehouse, uložili soubor tooa, zkopírovat tooyour schránky nebo ani upravit v řádku před provedením další akce.  

Jak je uvedeno výše, až se migrace schématu zkontrolujte hello migrace změní této hello udělal nástroj v pořadí tooensure, že plně rozumíte jim.  

## <a name="migrate-data"></a>Migrace dat
Kliknutím na možnost "migrovat Data hello může generovat BCP skripty, které se přesune datové soubory první tooflat na serveru, a pak přímo do SQL Data Warehouse. Doporučujeme tento proces pro přesun malé množství dat a jako opakování nejsou předdefinované a selhání může dojít, pokud dojde ke ztrátě hello síťové připojení. V pořadí toorun, to, budete potřebovat toohave hello BCP nainstalovaný nástroj příkazového řádku a hello schéma pro hello data musí již byly vytvořeny.

Po vyplnění hello parametry výše můžete jednoduše potřebovat tooclick spuštění migrace a sadu dva balíčky se budou generovat tooyour zadané umístění. Spusťte soubor exportu hello v pořadí tooexport data ze zdroje migrace do plochých souborů a spusťte soubor importu hello v pořadí tooimport data do SQL Data Warehouse.

## <a name="next-steps"></a>Další kroky
Teď, když jste migrovali některá data, podívejte se jak příliš[vyvíjet][develop].

<!--Image references-->

<!--Article references-->
[migration documentation]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Download Migration Utility]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
