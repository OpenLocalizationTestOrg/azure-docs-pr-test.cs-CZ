---
title: "aaaMigrate vaše stávající Azure datového skladu toopremium úložiště | Microsoft Docs"
description: "Pokyny k migraci existujících dat skladu toopremium úložiště"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: barbkess
editor: 
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 11/29/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 145199c2da1f6f1fb8898626cd04886c42d82204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data-warehouse-toopremium-storage"></a>Migrace úložiště toopremium datového skladu
Azure SQL Data Warehouse nedávno zaveden [storage úrovně premium pro vyšší výkon, předvídatelnost][premium storage for greater performance predictability]. Stávající data, která teď může být sklady aktuálně na standardní úložiště migrovat toopremium úložiště. Můžete využít výhod Automatická migrace, nebo pokud dáváte přednost toocontrol při toomigrate (které zahrnovat výpadky), můžete provést migraci hello sami.

Pokud máte více než jeden datový sklad, použijte hello [automatickou migraci plánu] [ automatic migration schedule] toodetermine, když bude taky migrovat.

## <a name="determine-storage-type"></a>Stanovení typů úložiště
Pokud jste vytvořili datového skladu před hello následující data, se aktuálně používá standardní úložiště.

| **Oblast** | **Vytvořené před tímto datem datového skladu** |
|:--- |:--- |
| Austrálie – východ |Storage úrovně Premium zatím není k dispozici |
| Čína – východ |1. listopadu 2016 |
| Čína – sever |1. listopadu 2016 |
| Německo – střed |1. listopadu 2016 |
| Německo – severovýchod |1. listopadu 2016 |
| Indie – západ |Storage úrovně Premium zatím není k dispozici |
| Japonsko – západ |Storage úrovně Premium zatím není k dispozici |
| Střed USA – sever |10 od listopadu 2016 |

## <a name="automatic-migration-details"></a>Automatická migrace podrobnosti
Ve výchozím nastavení, jsme bude databázi migrujte pro vás od 18:00:00 do 6:00:00 ve vaší oblasti místní čas během hello [automatickou migraci plánu][automatic migration schedule]. Během migrace hello nepoužitelný existující datový sklad. Hello migrace bude trvat přibližně za jednu hodinu za terabajt úložiště za datového skladu. Vám nebude nic účtováno během jakékoli její části hello automatickou migraci.

> [!NOTE]
> Po dokončení migrace hello datového skladu bude zpět online a dá se použít.
>
>

Microsoft trvá hello následující kroky toocomplete hello migrace (ty nevyžadují žádné zapojení z vaší strany). V tomto příkladu Představte si, že vaše existující datový sklad na standardní úložiště je aktuálně s názvem "MyDW."

1. Microsoft přejmenuje "MyDW" příliš "MyDW_DO_NOT_USE_ [časové razítko]."
2. Microsoft pozastaví "MyDW_DO_NOT_USE_ [časové razítko]." Během této doby je převzat zálohu. Pokud jsme dojde k potížím během tohoto procesu může se zobrazit více zastaví a obnoví.
3. Microsoft vytvoří nový datový sklad s názvem "MyDW" na storage úrovně premium ze zálohy hello prováděné v kroku 2. "MyDW" se zobrazí až po dokončení obnovení hello.
4. Po dokončení obnovení hello "MyDW" vrátí toohello stejný datový sklad jednotky a stavu (pozastaven nebo aktivní), který byl před migrací hello.
5. Po dokončení migrace hello, odstraní Microsoft "MyDW_DO_NOT_USE_ [časové razítko]".

> [!NOTE]
> Hello následující nastavení se nepřenesou jako součást migrace hello:
>
> * Auditování na úrovni databáze hello musí toobe funkce znovu povolena.
> * Pravidla brány firewall na úrovni databáze hello potřebovat toobe znovu přidat. Pravidla brány firewall na úrovni serveru hello nemá vliv.
>
>

### <a name="automatic-migration-schedule"></a>Automatická migrace plánu
Automatické migrace dojde během hello následující výpadku plán od 18:00:00 do 6:00 AM (místní čas na oblast).

| **Oblast** | **Odhadované datum** | **Odhadované datum ukončení** |
|:--- |:--- |:--- |
| Austrálie – východ |Není-li určit ještě |Není-li určit ještě |
| Čína – východ |9. ledna 2017 |13. ledna 2017 |
| Čína – sever |9. ledna 2017 |13. ledna 2017 |
| Německo – střed |9. ledna 2017 |13. ledna 2017 |
| Německo – severovýchod |9. ledna 2017 |13. ledna 2017 |
| Indie – západ |Není-li určit ještě |Není-li určit ještě |
| Japonsko – západ |Není-li určit ještě |Není-li určit ještě |
| Střed USA – sever |9. ledna 2017 |13. ledna 2017 |

## <a name="self-migration-toopremium-storage"></a>Vlastní migrace toopremium úložiště
Pokud chcete toocontrol, když dojde k vaší výpadku, můžete použít následující kroky toomigrate existující datový sklad na standardní úložiště toopremium úložiště hello. Pokud zvolíte tuto možnost, musíte dokončit migraci vlastní hello před zahájením hello Automatická migrace v této oblasti. To zajistí, že zabránění nebezpečí hello automatickou migraci způsobuje konflikt (odkazovat toohello [automatickou migraci plánu][automatic migration schedule]).

### <a name="self-migration-instructions"></a>Pokyny k migraci vlastní
toomigrate data skladu sami, použijte hello zálohování a obnovení funkce. část obnovení Hello hello migrace je očekávané tootake přibližně jedné hodiny za terabajt úložiště za datového skladu. Pokud chcete, aby tookeep hello stejný název po dokončení migrace, postupujte podle hello [toorename kroky při migraci][steps toorename during migration].

1. [Pozastavení] [ Pause] datového skladu. Tato akce trvá automatické zálohování.
2. [Obnovit] [ Restore] z vaší poslední snímek.
3. Odstraňte existující datový sklad na standardní úložiště. **Pokud selže toodo tento krok vám bude účtována pro obě datových skladů.**

> [!NOTE]
> Hello následující nastavení se nepřenesou jako součást migrace hello:
>
> * Auditování na úrovni databáze hello musí toobe funkce znovu povolena.
> * Pravidla brány firewall na úrovni databáze hello potřebovat toobe znovu přidat. Pravidla brány firewall na úrovni serveru hello nemá vliv.
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a>Přejmenování datového skladu během migrace (volitelné)
Dvě databáze na hello nemůže mít stejný logický server hello stejný název. SQL Data Warehouse teď podporuje hello možnost toorename datového skladu.

V tomto příkladu Představte si, že vaše existující datový sklad na standardní úložiště je aktuálně s názvem "MyDW."

1. Přejmenujte "MyDW" pomocí hello následující příkaz ALTER DATABASE. (V tomto příkladu jsme budete přejmenujte ji "MyDW_BeforeMigration.")  Tento příkaz zastaví všechny stávající transakce a je třeba provést v hlavní databázi toosucceed hello.
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. [Pozastavení] [ Pause] "MyDW_BeforeMigration." Tato akce trvá automatické zálohování.
3. [Obnovit] [ Restore] z nejnovější snímku novou databázi s názvem hello použije toobe (například "MyDW").
4. Odstranit "MyDW_BeforeMigration." **Pokud selže toodo tento krok vám bude účtována pro obě datových skladů.**


## <a name="next-steps"></a>Další kroky
Hello změnit toopremium úložiště, máte také zvýšením počtu objektů blob soubory databáze v hello základní Architektura datového skladu. toomaximize hello výkonu výhody této změny znovu sestavit vaše Clusterované indexy columnstore pomocí hello následující skript. skript Hello funguje tak, že některé z existujících dat toohello další objektů BLOB vynucení. Pokud nepodniknete žádnou akci, hello dat bude přirozeně znovu distribuovat časem jako načtení více dat do tabulek.

**Požadavky:**

- Hello datového skladu měly být spuštěny s jednotky 1000 datového skladu nebo vyšší (viz [škálování výpočetního výkonu][scale compute power]).
- Hello uživatel provádění skriptu hello by měl být v hello [mediumrc role] [ mediumrc role] nebo vyšší. tooadd toothis role uživatele, spusťte následující hello:````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create table toocontrol index rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go

--------------------------------------------------------------------------------
-- Step 2: Execute index rebuilds. If script fails, hello below can be re-run toorestart where last left off.
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Clean up table created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Pokud narazíte na potíže s datovým skladem, [vytvořit lístek podpory] [ create a support ticket] a odkazovat na "migrace toopremium úložiště" jako možnou příčinou hello.

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration tooPremium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md#pause-compute
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps toorename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
