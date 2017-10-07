---
title: "aaaImport a export ve službě Azure Database pro databázi MySQL | Microsoft Docs"
description: "Tento článek vysvětluje běžné způsoby tooimport a export databáze v databázi Azure pro databázi MySQL, pomocí nástrojů, jako je například MySQL Workbench."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: e2c036773d975df2eea2a59d166ea10567218a41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-mysql-database-by-using-import-and-export"></a>Migraci databáze MySQL pomocí import a export
Tento článek vysvětluje dvě běžné tooimporting přístupy a export dat tooan Azure databáze MySQL serveru pomocí MySQL Workbench. 

## <a name="before-you-begin"></a>Než začnete
toostep prostřednictvím této jak tooguide, budete potřebovat:
- Databázi Azure pro server databáze MySQL, pomocí následujících [vytvoření databáze Azure pro server databáze MySQL pomocí portálu Azure](quickstart-create-mysql-server-database-using-azure-portal.md).
- MySQL Workbench [Stáhnout](https://dev.mysql.com/downloads/workbench/), nebo jiný nástroj tooimport MySQL a exportu.

## <a name="use-common-tools"></a>Běžné nástroje pro použití
Běžné nástroje pro použití jako je například MySQL Workbench, Toad nebo Navicat tooremotely připojit a importovat nebo exportovat data do Azure databáze pro databázi MySQL. 

Pomocí těchto nástrojů v klientském počítači s tooAzure tooconnect připojení Internetu databáze pro databázi MySQL. Použít připojení zašifrované protokolem SSL pro osvědčené postupy zabezpečení, jak je popsáno v [připojení konfigurace protokolu SSL ve službě Azure Database pro databázi MySQL](concepts-ssl-connection-security.md).

Není nutné toomove import a export souborů tooany speciální cloudu umístění při migraci tooAzure databáze pro databázi MySQL. 

## <a name="create-a-database-on-hello-azure-database-for-mysql-server"></a>Vytvořit databázi na hello Azure databáze MySQL serveru
Vytvořte prázdnou databázi na hello Azure databáze MySQL serveru, kam chcete toomigrate hello data. Pomocí nástroje, jako je databáze MySQL Workbench, Toad nebo Navicat hello toocreate. Hello databáze může mít stejný název jako hello databáze, která obsahuje data hello zálohované, nebo můžete vytvořit databázi s jiným názvem hello.

tooget připojený, vyhledejte informace o připojení hello na hello **vlastnosti** podokně ve službě Azure Database pro databázi MySQL.

![Najít informace o připojení hello v hello portálu Azure](./media/concepts-migrate-import-export/1_server-properties-name-login.png)

Přidejte hello připojení informace tooMySQL Workbench.

![MySQL Workbench připojovací řetězec](./media/concepts-migrate-import-export/2_setup-new-connection.png)

## <a name="determine-when-toouse-import-and-export-techniques-instead-of-a-dump-and-restore"></a>Určení, kdy toouse importovat a exportovat techniky místo výpisu a obnovení
Pomocí nástroje tooimport MySQL a export databáze do databáze MySQL Azure v hello následující scénáře. V jiných scénářích může být vhodné pomocí hello [dump a obnovení](concepts-migrate-dump-restore.md) přístupu místo. 

- Pokud budete potřebovat tooselectively vybrat několik tabulek tooimport z existující databázi MySQL do Azure databáze MySQL, nejlepší toouse hello import a export techniku.  Díky tomu je možné vynechat všechny nepotřebné tabulky z hello migrace toosave čas a prostředky. Například použít hello `--include-tables` nebo `--exclude-tables` přepínač s [mysqlpump](https://dev.mysql.com/doc/refman/5.7/en/mysqlpump.html#option_mysqlpump_include-tables) a hello `--tables` přepínač s [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#option_mysqldump_tables).
- Když přesouváte hello databázové objekty než tabulky, explicitně je vytvořte. Zahrnout omezení (primární klíč, cizí klíč, indexy), zobrazení, funkce, postupy, aktivační události a všechny ostatní databázové objekty, které chcete toomigrate.
- Když se migraci dat z externích zdrojů dat. než databázi MySQL, vytvořit plochých souborů a importovat pomocí [mysqlimport](https://dev.mysql.com/doc/refman/5.7/en/mysqlimport.html).

Ujistěte se, že všechny tabulky v databázi hello použít modul úložiště InnoDB hello při načítání dat do Azure databáze pro databázi MySQL. Azure databáze pro databázi MySQL podporuje pouze hello InnoDB stroj úložiště, takže nepodporuje moduly alternativní úložiště. Pokud vaše tabulky vyžadují moduly alternativní úložiště, se že tooconvert je toouse hello InnoDB modul formátu před hello migrace tooAzure databáze pro databázi MySQL. 

Například pokud máte WordPress nebo webovou aplikaci, která používá modul MyISAM hello, nejprve převeďte hello tabulky migraci dat hello do InnoDB tabulek. Pak obnovte tooAzure databáze pro databázi MySQL. Použití klauzule hello `ENGINE=INNODB` tooset hello modul pro vytvoření tabulky a potom přeneste hello data do tabulky kompatibilní hello před migrací hello. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```

## <a name="performance-recommendations-for-import-and-export"></a>Výkon doporučení pro import a export
-   Před načtením dat vytvořte Clusterované indexy a primární klíče. Načtení dat do primární klíče pořadí. 
-   Zpoždění vytváření sekundárních indexů až po dat je načten. Všechny sekundární indexy vytvořte po načtení. 
-   Zakažte omezení cizích klíčů před načtením. Zakázání cizího klíče kontroly poskytuje výrazné zvýšení výkonu. Povolit omezení hello a ověření dat hello po hello zatížení tooensure referenční integrity.
-   Načtení dat paralelně. Vyhněte se příliš mnoho paralelismus, který by způsobit, že toohit limitu prostředků a sledování prostředků pomocí hello metriky, které jsou k dispozici v hello portálu Azure. 
-   Dělené tabulky v případě nutnosti použijte.

## <a name="import-and-export-by-using-mysql-workbench"></a>Import a export pomocí MySQL Workbench
Existují dva způsoby tooexport a import dat v MySQL Workbench. Každý slouží k jinému účelu. 

### <a name="table-data-export-and-import-wizards-from-hello-object-browsers-context-menu"></a>Data tabulky exportovat a importovat průvodců z prohlížeče objektů hello kontextové nabídky
![Průvodci MySQL Workbench na prohlížeč objektů hello kontextové nabídky](./media/concepts-migrate-import-export/p1.png)

Hello průvodců pro data tabulky podporují import a export operace pomocí souborů sdílených svazků clusteru a JSON. Obsahují několik možností konfigurace, například oddělovačů, výběr sloupců a výběru kódování. Každý průvodce pro místní nebo vzdálené připojení MySQL servery, můžete provést. Akce importu Hello obsahuje tabulky, sloupce a mapování typu. 

Tito průvodci dostanete z kontextové nabídky Prohlížeč objektů hello kliknutím pravým tlačítkem na tabulku. Potom vyberte buď **Průvodce exportem dat tabulky** nebo **Průvodce importem tabulky dat**. 

#### <a name="table-data-export-wizard"></a>Průvodce exportem dat tabulky
Následující příklad exportuje hello tabulky tooa souboru .csv Hello: 
1. Klikněte pravým tlačítkem na hello tabulky toobe databáze hello exportovali. 
2. Vyberte **tabulky Průvodce exportem dat**. Vyberte hello sloupce toobe exportovali, posun řádku (pokud existuje) a počet (pokud existuje). 
3. Na hello **vyberte data pro export** klikněte na tlačítko **Další**. Vyberte cestu k souboru hello, CSV nebo JSON typ souboru. Také vyberte hello řádku oddělovač, způsob uzavření řetězce a pole oddělovače. 
4. Na hello **vyberte výstupní soubor umístění** klikněte na tlačítko **Další**. 
5. Na hello **Export dat** klikněte na tlačítko **Další**.

#### <a name="table-data-import-wizard"></a>Průvodce importem tabulky dat
Hello následující příklad importuje hello tabulky ze souboru CSV:
1. Klikněte pravým tlačítkem na tabulku hello toobe databáze hello importovat. 
2. Vyberte tooand procházet hello toobe soubor CSV importovat a potom klikněte na **Další**. 
3. Vyberte cílové tabulky hello (nový nebo existující) a zaškrtněte nebo zrušte hello **Truncate table před importem** zaškrtávací políčko. Klikněte na **Další**.
4. Vyberte kódování a hello toobe sloupce importovat a potom klikněte na **Další**. 
5. Na hello **importovat data** klikněte na tlačítko **Další**. Hello průvodce importuje hello data odpovídajícím způsobem.

### <a name="sql-data-export-and-import-wizards-from-hello-navigator-pane"></a>SQL data exportovat a importovat průvodců z podokna Navigátor hello
Použijte Průvodce tooexport nebo importovat SQL z databáze MySQL Workbench generovány nebo vygenerovat z příkazu mysqldump hello. Přístup k těmto průvodcům z hello **Navigátor** podokně nebo výběrem **Server** z hlavní nabídky hello. Potom vyberte **exportu dat** nebo **Import dat**. 

#### <a name="data-export"></a>Export dat
![Export dat MySQL Workbench panelu Navigátor hello](./media/concepts-migrate-import-export/p2.png)

Můžete použít hello **exportu dat** kartě tooexport MySQL data. 
1. Vyberte každý schématu, zda chcete tooexport, volitelně vybrat konkrétní schématu objekty nebo tabulky z každé schématu a generovat hello export. Možnosti konfigurace zahrnují samostatný SQL soubor nebo složku projektu tooa exportu, dump uložené rutiny a události nebo přeskočit dat v tabulce. 
 
   Můžete taky použít **exportovat sady výsledků** tooexport výsledku konkrétní sada v hello SQL editor tooanother formátu, například CSV, JSON, HTML a XML. 
3. Vyberte hello databáze objekty tooexport a nakonfigurujte hello související možnosti.
4. Klikněte na tlačítko **aktualizovat** tooload hello aktuální objekty.
5. Volitelně můžete otevřít hello **pokročilé možnosti** kartě toorefine hello exportu. Například umožňuje přidáte zámky tabulky, použijte nahradit místo příkazech insert a identifikátory uvozovky s backtick znaky.
6. Klikněte na tlačítko **spusťte Export** procesu exportu toobegin hello.


#### <a name="data-import"></a>Import dat
![Import dat MySQL Workbench pomocí správy Navigátor](./media/concepts-migrate-import-export/p3.png)

Můžete použít hello **Import dat** kartě tooimport nebo obnovit exportovaná data z hello operace exportu dat nebo z příkazu mysqldump hello. 
1. Vyberte složky projektu hello nebo samostatný soubor SQL, zvolte hello schématu tooimport do nebo zvolte **nový** toodefine nové schéma. 
2. Klikněte na tlačítko **spusťte Import** toobegin hello importu.

## <a name="next-steps"></a>Další kroky
Jako další migrace přístup pro čtení [migrací pomocí databáze MySQL dump a obnovení ve službě Azure Database pro databázi MySQL](concepts-migrate-dump-restore.md). 
