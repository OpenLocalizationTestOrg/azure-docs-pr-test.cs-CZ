---
title: "databáze upraveným cloudu aaaManaging | Microsoft Docs"
description: "Znázorňuje hello služba úlohy elastické databáze"
metakeywords: azure sql database elastic databases
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 6fa47cf2-1162-4534-a206-6e2d95b78580
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: b6d330cd712421b8cba781e835830772e6e5b77e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-scaled-out-cloud-databases"></a>Správa databází upraveným cloudu
hello toomanage upraveným horizontálně dělené databází, **úlohy elastické databáze** funkci (preview) umožňuje tooreliably můžete spustit skript Transact-SQL (T-SQL) napříč skupinou databází, včetně:

* vlastní kolekce databáze (vysvětlení níže)
* všechny databáze [elastického fondu](sql-database-elastic-pool.md)
* Sada horizontálního oddílu (vytvořený [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md)). 

## <a name="documentation"></a>Dokumentace
* [Nainstalujte komponenty úlohy elastické databáze hello](sql-database-elastic-jobs-service-installation.md). 
* [Začínáme s úlohami elastické databáze](sql-database-elastic-jobs-getting-started.md).
* [Vytvářet a spravovat úlohy pomocí prostředí PowerShell](sql-database-elastic-jobs-powershell.md).
* [Vytvoření a správa škálovaný databází SQL Azure](sql-database-elastic-jobs-getting-started.md)

**Elastické databáze úlohy** právě zákazníka hostovaná Cloudová služba Azure, která umožňuje provádění hello ad-hoc a naplánované úlohy správy, které se nazývají **úlohy**. S úlohami můžete snadno a spolehlivě spravovat velké skupiny databáze SQL Azure tak, že spustíte Transact-SQL skriptů tooperform operace správy. 

![Služba úlohy elastické databáze][1]

## <a name="why-use-jobs"></a>Proč používat úlohy?
**Správa**

Jednoduše proveďte změny schématu, Správa přihlašovacích údajů, aktualizace dat odkaz, shromažďování dat výkonu nebo kolekce telemetrie klienta (zákazníka).

**Sestavy**

Agregovaná data z kolekce databáze SQL Azure do jedné cílové tabulky.

**Snížení režie**

Za normálních okolností, musíte připojit databázi tooeach nezávisle v příkazech jazyka Transact-SQL toorun pořadí nebo provádět další úlohy správy. Úloha zpracuje hello úkolů protokolování v databázi tooeach v hello cílovou skupinu. Můžete také definovat, udržovat a zachovat Transact-SQL skriptů toobe provést napříč skupinou databází SQL Azure.

**Monitorování účtů**

Úlohy spouštět hello skript a protokolu hello stavu spuštění pro každou databázi. Získáte také automatické opakování, když dojde k selhání.

**Flexibilita**

Definovat vlastní skupiny databází SQL Azure a plány pro spuštění úlohy.

> [!NOTE]
> V hello portálu Azure pouze omezená sada funkcí omezené tooSQL Azure elastické fondy je k dispozici. Použijte hello úplnou sadu rozhraní API prostředí PowerShell tooaccess hello aktuální funkcí.
> 
> 

## <a name="applications"></a>Aplikace
* Provádění úloh správy, jako je nasazení nové schéma.
* Aktualizujte data produktu informace společné napříč všechny databáze. Nebo plány automatické aktualizace každý den v týdnu po hodinách.
* Opětovné sestavení indexů tooimprove dotazu výkonu. znovu sestavit Hello může být nakonfigurované tooexecute v kolekci databází pravidelně, například mimo špičku.
* Shromážděte výsledky dotazu ze sady databází do centrální tabulky na základě průběžné. Dotazy na výkon může provést průběžně a nakonfigurované toobe další úkoly tootrigger provést.
* Spuštění delší spuštěné dotazy zpracování dat napříč velkou sadou databází, například hello kolekce telemetrie zákazníka. Výsledky se shromažďují do jedné cílové tabulky pro další analýzu.

## <a name="elastic-database-jobs-end-to-end"></a>Úlohy elastické databáze: začátku do konce
1. Nainstalujte hello **úlohy elastické databáze** součásti. Další informace najdete v tématu [úlohy instalace elastické databáze](sql-database-elastic-jobs-service-installation.md). V případě selhání instalace hello najdete v části [jak toouninstall](sql-database-elastic-jobs-uninstall.md).
2. Použijte hello rozhraní API prostředí PowerShell tooaccess další funkce, například vytváření vlastní databázi kolekce, přidání plány nebo shromažďování sad výsledků. Použití hello portál pro vytvoření nebo monitorování, úloh a jednoduchou instalaci omezená tooexecution proti **elastický fond**. 
3. Vytvoření zašifrované přihlašovací údaje pro provádění úlohy a [přidání hello uživatele (nebo rolí) tooeach databáze ve skupině hello](sql-database-security-overview.md).
4. Vytvořte idempotent skriptu T-SQL, který lze spustit na každou databázi ve skupině hello. 
5. Postupujte podle těchto kroků toocreate úloh pomocí hello portálu Azure: [vytváření a Správa úloh elastické databáze](sql-database-elastic-jobs-create-and-manage.md). 
6. Nebo použití skriptů prostředí PowerShell: [vytvořit a spravovat úlohy elastické databáze databáze SQL pomocí prostředí PowerShell (preview)](sql-database-elastic-jobs-powershell.md).

## <a name="idempotent-scripts"></a>Idempotent skriptů
musí být Hello skripty [idempotent](https://en.wikipedia.org/wiki/Idempotence). Jednoduše řečeno "idempotent" znamená, že pokud hello skript uspěje a znovu spusťte, hello stejného výsledku dojde. Skript se nemusí zdařit kvůli tootransient problémů se sítí. V takovém případě hello úlohy bude automaticky opakovat spouštění skriptu hello přednastavené počet časů před desisting. Skript idempotent má hello stejný výsledek i v případě, že byl úspěšně spuštěn dvakrát. 

Jednoduché cílem je tootest pro hello existenci objekt před jejím vytváření.  

    IF NOT EXIST (some_object)
    -- Create hello object 
    -- If it exists, drop hello object before recreating it.

Podobně skript musí být schopný tooexecute úspěšně logicky pro testování a zamezením jakékoliv podmínky, které najde.

## <a name="failures-and-logs"></a>Chyby a protokoly
Pokud skript selže po několika pokusech, hello úloha zapíše hello chybu a pokračuje. Po skončení úlohy (tj. spustit pro všechny databáze ve skupině hello), můžete zkontrolovat její seznam neúspěšných pokusů o přihlášení. protokoly Hello poskytují podrobnosti toodebug vadný skripty. 

## <a name="group-types-and-creation"></a>Skupiny typy a vytvoření
Existují dva typy skupin: 

1. Nastaví horizontálního oddílu
2. Vlastní skupiny

Horizontálního oddílu sady skupiny jsou vytvořeny pomocí hello [nástroje elastické databáze](sql-database-elastic-scale-introduction.md). Když vytvoříte skupinu sadu horizontálního oddílu, jsou databáze přidat nebo odebrat ze skupiny hello automaticky. Například nové horizontálního oddílu bude automaticky ve skupině hello při přidání toohello horizontálního oddílu mapy. Úlohu můžete pak spusťte proti hello skupiny.

Vlastní skupiny, na druhé straně text hello, jsou pevně definované. Musíte explicitně přidat nebo odebrat databáze z vlastní skupiny. Pokud databáze ve skupině hello je vynechána, pokusí hello úlohy skriptu hello toorun hello databázi, což vede k jeho případnému selhání. Skupiny vytvořené pomocí hello portál Azure aktuálně jsou vlastní skupiny. 

## <a name="components-and-pricing"></a>Součásti a ceny
Hello následující součásti spolupracují toocreate Azure cloudové služby, která umožňuje ad-hoc provádění úloh správy. Hello součásti instalace a konfigurace automaticky během instalace, v rámci vašeho předplatného. Hello služby můžete identifikovat jako spolu mají stejné automaticky generovaný hello název. Hello název jedinečný a skládá se z hello předponu "edj" následované 21 náhodně generovaných znaků.

* **Cloudovou službu Azure**: úlohy elastické databáze (preview) je dodávána jako zákazník hostované Azure Cloud service tooperform provádění hello požadované úlohy. Z portálu hello hello služby nasazení a hostované v rámci vašeho předplatného Microsoft Azure. Hello výchozí nasazené služba běží s hello minimálně dva pracovní role pro vysokou dostupnost. Hello výchozí velikost jednotlivých rolí pracovního procesu (ElasticDatabaseJobWorker) běží na instanci A0. Ceny, najdete v části [ceny služby Azure Cloud services](https://azure.microsoft.com/pricing/details/cloud-services/). 
* **Azure SQL Database**: Služba hello používá databázi SQL Azure, označuje jako hello **řízení databáze** toostore všechny hello úlohy metadat. úroveň služby výchozí Hello je S0. Ceny, najdete v části [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database/).
* **Azure Service Bus**: Azure Service Bus je pro koordinaci hello práce v rámci hello Azure Cloud Service. V tématu [Service Bus ceny](https://azure.microsoft.com/pricing/details/service-bus/).
* **Úložiště Azure**: slouží Azure Storage účet toostore diagnostiky výstup protokolování v hello událost, která problém vyžaduje další ladění (najdete v části [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](../cloud-services/cloud-services-dotnet-diagnostics.md)). Ceny, najdete v části [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-elastic-database-jobs-work"></a>Jak fungují úlohy elastické databáze
1. Je určený k Azure SQL Database **řízení databáze** které ukládá všechna data metadata a stavu.
2. databáze řízení Hello přistupuje hello **úlohy služby** tooexecute toolaunch a sledování úloh.
3. Dva různé role komunikují s databází řízení hello: 
   * Řadič: Určuje, jaké úlohy vyžadují úlohy tooperform hello požadoval úlohy a opakování nezdařených úloh vytvoření nové úlohy.
   * Provádění úloh úkolu: Provede hello úlohy.

### <a name="job-task-types"></a>Typy úloh úloh
Existují různé druhy úlohy, které provádějí spouštění úloh:

* ShardMapRefresh: Dotazy hello horizontálního oddílu mapy toodetermine všechny databáze hello používá jako horizontálních oddílů
* ScriptSplit: Rozdělí mezi tlačítko Přejít' příkazy do dávek skriptu hello
* ExpandJob: Vytváří podřízené úlohy pro každou databázi z úlohy, která je cílena skupinu databází
* ScriptExecution: Spustí skript na konkrétní databázi pomocí přihlašovacích údajů definovaných
* Dacpac: Konkrétní databázi DACPAC tooa pomocí konkrétní přihlašovacích údajů se vztahuje

## <a name="end-to-end-job-execution-work-flow"></a>Spuštění úlohy začátku do konce pracovní flow
1. S použitím hello portál nebo hello rozhraní API prostředí PowerShell, úlohy budou vložena do hello **řízení databáze**. Úloha Hello požaduje spuštění skriptu jazyka Transact-SQL pro skupinu databází pomocí konkrétní přihlašovací údaje.
2. Hello řadiče identifikuje hello novou úlohu. Úlohy jsou vytvářeny a spustit skript hello toosplit a toorefresh hello skupinu databází. Nakonec novou úlohu se vytvoří a provést tooexpand hello úlohy a vytvářet nové podřízené úlohy, kde je každá úloha podřízené zadaný tooexecute hello skript Transact-SQL proti může jedna databáze ve skupině hello.
3. Hello řadiče identifikuje hello vytvořit podřízené úlohy. Pro každou úlohu hello řadiče vytvoří a spustí skript úlohy úloh tooexecute hello proti databázi. 
4. Po dokončení úlohy všechny, aktualizuje řadič hello hello úlohy tooa dokončit stavu. 
   Kdykoli během provádění úlohy hello rozhraní API prostředí PowerShell může být použité tooview hello aktuální stav provádění úlohy. Všechny časy vrátila hello rozhraní API prostředí PowerShell jsou reprezentované ve standardu UTC. V případě potřeby může být žádost o zrušení iniciovaná toostop úlohu. 

## <a name="next-steps"></a>Další kroky
[Nainstalujte komponenty hello](sql-database-elastic-jobs-service-installation.md), pak [vytvořit a přidat do protokolu v databázi tooeach ve skupině hello databází](sql-database-manage-logins.md). toofurther pochopit úlohy vytváření a správu najdete v tématu [vytvářením a správou úloh elastické databáze](sql-database-elastic-jobs-create-and-manage.md). Viz také [Začínáme s úlohami elastické databáze](sql-database-elastic-jobs-getting-started.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->


