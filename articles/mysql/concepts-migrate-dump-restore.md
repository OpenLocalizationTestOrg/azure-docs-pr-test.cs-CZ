---
title: "aaaMigrate pomocí databáze MySQL dump a obnovení ve službě Azure Database pro databázi MySQL | Microsoft Docs"
description: "Tento článek vysvětluje dvě běžné způsoby tooback zálohu a obnovení databází ve vaší databázi Azure pro databázi MySQL, pomocí nástrojů, jako je mysqldump, MySQL Workbench a PHPMyAdmin."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: d05d483ff53483df8e005eae2d9a4f8190e8f663
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-mysql-database-tooazure-database-for-mysql-using-dump-and-restore"></a>Migrace vaší tooAzure databáze MySQL databáze pro databázi MySQL pomocí výpisu a obnovení
Tento článek popisuje dvě běžné způsoby tooback nahoru a obnovení databází v Azure Database pro databázi MySQL
- Výpis a obnovení ze hello příkazového řádku (pomocí mysqldump) 
- Výpis stavu a obnovení pomocí PHPMyAdmin 

## <a name="before-you-begin"></a>Než začnete
toostep prostřednictvím této tooguide postupy, je třeba toohave:
- [Vytvoření Azure databáze MySQL serveru – portál Azure](quickstart-create-mysql-server-database-using-azure-portal.md)
- [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) na počítači nainstalovaný nástroj příkazového řádku.
- MySQL Workbench [MySQL Workbench Stáhnout](https://dev.mysql.com/downloads/workbench/), Toad, Navicat nebo jiných třetích stran MySQL nástroj toodo dump a obnovení příkazy.

## <a name="use-common-tools"></a>Běžné nástroje pro použití
Použít běžné nástroje a nástroje, jako je MySQL Workbench, mysqldump, Toad nebo Navicat tooremotely připojit a obnovit data do Azure databáze pro databázi MySQL. Pomocí těchto nástrojů na klientský počítač s toohello tooconnect připojení Internetu Azure Database pro databázi MySQL. Používat připojení šifrovaná protokolem SSL pro osvědčené postupy zabezpečení, viz také [připojení konfigurace protokolu SSL ve službě Azure Database pro databázi MySQL](concepts-ssl-connection-security.md). Při migraci tooAzure databáze pro databázi MySQL nepotřebujete toomove hello odkládacích souborů tooany speciální cloudu umístění. 

## <a name="common-uses-for-dump-and-restore"></a>Běžná použití pro výpis a obnovení
MySQL nástroje jako jsou třeba mysqldump a mysqlpump toodump a zatížení databáze do databáze MySQL Azure můžete používat v několika běžné scénáře. V jiných scénářích může použít hello [Import a Export](concepts-migrate-import-export.md) přístupu místo.

- Použít databázi vypíše při migraci celé databáze hello. Toto doporučení obsahuje při přesunu velké množství dat MySQL, nebo když chcete přerušení služby toominimize za provozu webů nebo aplikací. 
-  Ujistěte se, že všechny tabulky v databázi hello použít modul úložiště InnoDB hello při načítání dat do Azure databáze pro databázi MySQL. Azure databáze pro databázi MySQL podporuje pouze stroj InnoDB úložiště a proto nepodporuje moduly alternativní úložiště. Pokud vaše tabulky jsou nakonfigurované další moduly úložiště, je převeďte do formátu modul InnoDB hello před tooAzure migrace databáze pro databázi MySQL.
   Například pokud máte WordPress nebo WebApp pomocí hello MyISAM tabulky, nejprve převeďte těchto tabulek a migrujte do formátu InnoDB před obnovením tooAzure databáze pro databázi MySQL. Použití klauzule hello `ENGINE=InnoDB` tooset hello modul použít při vytváření nové tabulky a pak přenosu dat hello do tabulky kompatibilní hello před hello obnovení. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- tooavoid žádné kompatibility problémy, zajistěte hello stejnou verzi databáze MySQL se používá na hello zdrojové a cílové systémy pro vypsání databáze. Například pokud je existující server MySQL verzi 5.7, pak je potřeba migrovat tooAzure databáze MySQL nakonfigurované toorun verzi 5.7. Hello `mysql_upgrade` příkaz nefunguje v databázi Azure pro server databáze MySQL a není podporována. Pokud potřebujete tooupgrade mezi verzemi MySQL, nejprve dump nebo exportovat nižší verze databáze na vyšší verzi MySQL ve svém vlastním prostředí. Spusťte `mysql_upgrade`, před migrací do Azure Database pro databázi MySQL.

## <a name="performance-considerations"></a>Otázky výkonu
toooptimize výkon, proveďte oznámení o tyto důležité informace k vypsání velké databáze:
-   Použití hello `exclude-triggers` možnost v mysqldump při vypsání databáze. Vyloučení ze souborů výpisu paměti aktivuje tooavoid hello aktivační událost příkazy ohlásí během obnovení dat hello. 
-   Vyhněte se hello `single-transaction` možnost v mysqldump při vypsání velmi velké databáze. Vypsání mnoho tabulek v rámci jedné transakce způsobí dodatečné úložiště a využívat během obnovení toobe prostředky paměti a může způsobit zpoždění výkonu nebo omezení zdrojů.
-   Použití s více hodnotami vloží při načítání s SQL toominimize příkaz provádění režií při vypsání databáze. Při použití souborů výpisu paměti generované mysqldump nástroj, jsou ve výchozím nastavení povolené vloží s více hodnotami. 
-  Použití hello `order-by-primary` možnost v mysqldump při vypsání databáze, tak, aby hello data je vytvořena v primární klíče pořadí.
-   Použití hello `disable-keys` možnost v mysqldump při vypsání data, omezení cizího klíče toodisable před zatížení. Zakázání cizího klíče kontroly poskytuje větší zvýšení výkonu. Povolit omezení hello a ověření dat hello po hello zatížení tooensure referenční integrity.
-   Dělené tabulky v případě nutnosti použijte.
-   Načtení dat paralelně. Vyhněte se příliš mnoho paralelismus, který by způsobit, že toohit limitu prostředků a sledování prostředků pomocí hello metriky, které jsou k dispozici v hello portálu Azure. 
-   Použití hello `defer-table-indexes` možnost v mysqlpump při vypsání databáze, takže vytvoření indexu se stane po načtení dat tabulky.

## <a name="create-a-backup-file-from-hello-command-line-using-mysqldump"></a>Vytvořte soubor zálohy z hello příkazového řádku pomocí mysqldump
tooback zálohu stávající databáze MySQL na místní server hello nebo ve virtuálním počítači, spusťte následující příkaz hello: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Hello parametry tooprovide jsou:
- [uname] Vaše uživatelské jméno databáze 
- [průchodu] hello heslo pro vaši databázi (Všimněte si, není mezera mezi -p a heslo hello) 
- [dbname] hello název vaší databáze 
- Název souboru hello [backupfile.sql] pro zálohování databáze 
- [--opt] hello mysqldump možnost 

Například tooback zálohu databáze s názvem 'testdb' na serveru databáze MySQL s hello uživatelského jména 'testuser' a testdb_backup.sql souboru tooa žádné heslo, použijte následující příkaz hello. Hello příkaz zálohuje hello `testdb` databáze do souboru s názvem `testdb_backup.sql`, která obsahuje všechny příkazy SQL hello potřeby toore-vytvořit databázi hello. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
tooselect konkrétní tabulky ve vaší databázi tooback nahoru, názvy tabulek hello seznamu oddělené mezerami. Například tooback vytváření pouze tabulky1 a tabulky2 tabulek z hello 'testdb', použijte tento příklad: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```

Přepněte tooback až více než jednu databázi na jednou, použijte hello – databáze a názvy databází hello seznamu oddělené mezerami. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```
tooback až všechny hello databází na serveru hello najednou, měli byste použít hello – možnost všechny databáze.
```
$ mysqldump -u root -p --all-databases > alldb_backup.sql 
```

## <a name="create-a-database-on-hello-target-azure-database-for-mysql-server"></a>Vytvořit databázi na cíli hello Azure databáze MySQL serveru
Vytvořte prázdnou databázi na cíli hello Azure databáze MySQL serveru, kam chcete toomigrate hello data. Pomocí nástroje, jako je databáze MySQL Workbench, Toad nebo Navicat hello toocreate. Hello databáze může mít stejný název jako hello databáze, který je obsažený hello zálohované data hello nebo můžete vytvořit databázi s jiným názvem.

tooget připojené, vyhledejte informace o připojení hello na stránce vlastností hello ve službě Azure Database pro databázi MySQL.
![Najít informace o připojení hello v hello portálu Azure](./media/concepts-migrate-dump-restore/1_server-properties-name-login.png)

Přidáte informace o připojení hello do vaší databáze MySQL Workbench.
![MySQL Workbench připojovací řetězec](./media/concepts-migrate-dump-restore/2_setup-new-connection.png)


## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Obnovení databáze MySQL pomocí příkazového řádku nebo MySQL Workbench
Jakmile vytvoříte hello cílová databáze, můžete použít příkaz mysql hello nebo MySQL Workbench toorestore hello data do konkrétní hello nově vytvořený databáze ze souboru s výpisem hello.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
V tomto příkladu obnovte hello data do hello nově vytvořit databázi na cíli hello Azure databáze pro server databáze MySQL.
```bash
$ mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>Exportovat pomocí PHPMyAdmin
tooexport, můžete použít hello běžné nástroj phpMyAdmin, které mohou již byl nainstalován místně ve vašem prostředí. tooexport databáze MySQL pomocí PHPMyAdmin:
- Otevřete phpMyAdmin.
- Vyberte svou databázi. Klikněte na název databáze hello v hello seznamu na levé straně hello. 
- Klikněte na tlačítko hello **exportovat** odkaz. Nová stránka se zobrazí výpis hello tooview databáze.
- V oblasti Export hello, klikněte na tlačítko hello **Vybrat vše** odkaz toochoose hello tabulek v databázi. 
- V oblasti možnosti SQL hello klikněte na požadované možnosti hello. 
- Klikněte na tlačítko hello **uložit jako soubor** hello odpovídající komprese a možnost a potom klikněte na hello **přejděte** tlačítko. Dialogové okno by se zobrazit nabízení jste toosave hello soubor místně.

## <a name="import-using-phpmyadmin"></a>Import pomocí PHPMyAdmin
Import vaše databáze je podobné tooexporting. Hello následující akce:
- Otevřete phpMyAdmin. 
- Na stránce instalace phpMyAdmin hello, klikněte na **přidat** tooadd vaše Azure databáze pro server databáze MySQL. Zadejte podrobnosti připojení hello a přihlašovací údaje.
- Vytvoření příslušně pojmenovaných databáze a vyberte jej na hello nalevo od úvodní obrazovka. toorewrite hello existující databázi, klikněte na název databáze hello, vyberte všechny hello zaškrtnutí políček vedle názvů tabulek hello a vyberte **vyřadit** toodelete hello existující tabulky. 
- Klikněte na tlačítko hello **SQL** odkaz tooshow hello stránky, kde můžete zadat v příkazech SQL nebo nahrát soubor SQL. 
- Použití hello **Procházet** tlačítko toofind hello databázový soubor. 
- Klikněte na tlačítko hello **přejděte** tlačítko tooexport hello zálohování, spuštěním příkazů SQL hello a znovu vytvořit databázi.

## <a name="next-steps"></a>Další kroky
[Připojení aplikace tooAzure databáze pro databázi MySQL](./howto-connection-string.md)
