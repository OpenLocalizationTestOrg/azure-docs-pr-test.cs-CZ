---
title: aaaScaling se s Azure SQL Database | Microsoft Docs
description: "Software jako služba (SaaS) vývojáři můžete snadno vytvořit elastické, škálovatelná databáze v hello cloudu pomocí těchto nástrojů"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: d15a2e3f-5adf-41f0-95fa-4b945448e184
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: 82a561e07389d8619727a540fa9424248c087eda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-out-with-azure-sql-database"></a>Horizontální navýšení kapacity s Azure SQL Database
Můžete snadno škálovat databáze Azure SQL pomocí hello **elastické databáze** nástroje. Tyto nástroje a funkce, budete moct použít hello prakticky neomezené databáze prostředků **Azure SQL Database** toocreate řešení pro transakční zatížení a hlavně Software jako služba (SaaS) aplikace. Funkce elastické databáze se skládají z hello následující:

* [Klientská knihovna pro elastické databáze](sql-database-elastic-database-client-library.md): hello klientské knihovny je funkce, která vám umožní toocreate a udržovat horizontálně dělené databáze.  V tématu [začít pracovat s nástroji elastické databáze](sql-database-elastic-scale-get-started.md).
* [Nástroj rozdělení sloučení elastické databáze](sql-database-elastic-scale-overview-split-and-merge.md): přesouvá data mezi horizontálně dělené databáze. To je užitečné pro přesun dat z více klientů databáze tooa jednoho klienta databáze (nebo naopak). V tématu [kurzu nástroje elastické databáze rozdělení sloučení](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Elastické databáze úlohy](sql-database-elastic-jobs-overview.md) (preview): použití úlohy toomanage velkého počtu databází Azure SQL. Snadno proveďte operace správy, například změny schématu, Správa přihlašovacích údajů, aktualizace dat odkaz, shromažďování dat výkonu nebo pomocí úlohy kolekce telemetrie klienta (zákazníka).
* [Elastické databáze dotazu](sql-database-elastic-query-overview.md) (preview): umožňuje dotazu toorun Transact-SQL, která přesahuje více databází. To umožňuje připojení tooreporting nástroje, jako je aplikace Excel, PowerBI, Tableau, atd.
* [Elastické transakce](sql-database-elastic-transactions-overview.md): Tato funkce vám umožní toorun transakcí, které jsou rozmístěny v několika databází v Azure SQL Database. Transakce elastické databáze jsou k dispozici pro aplikace .NET pomocí rozhraní ADO .NET a integrovat hello známé programovací prostředí pomocí hello [System.Transaction třídy](https://msdn.microsoft.com/library/system.transactions.aspx).

Hello obrázek níže ukazuje architekturu, která zahrnuje hello **elastické databáze funkce** v kolekci tooa vztah databází.

V této grafiky představují barvy hello databáze schémat. Databáze s hello hello se stejným barva sdílejí stejné schéma.

1. Sadu **databází Azure SQL** jsou hostované v Azure pomocí architektury horizontálního dělení.
2. Hello **klientské knihovny pro elastické databáze** nastavena použité toomanage horizontálního oddílu.
3. Podmnožinu hello databáze jsou vloženy do **elastický fond**. (Viz [co je fond?](sql-database-elastic-pool.md)).
4. **Úlohy elastické databáze** spustí naplánované nebo ad hoc skriptů T-SQL pro všechny databáze.
5. Hello **nástroji pro sloučení rozdělení** je použité toomove data z jedné horizontálního oddílu tooanother.
6. Hello **elastické databáze dotazu** vám umožní toowrite dotaz, který zahrnuje všechny databáze v sadě hello horizontálního oddílu.
7. **Elastické transakce** vám umožní toorun transakcí, které jsou rozmístěny v několika databází. 

![Nástroje pro elastické databáze][1]

## <a name="why-use-hello-tools"></a>Proč používat nástroje hello?
Dosažení pružnost a škálování pro cloudové aplikace byla přehledné pro virtuální počítače a úložiště objektů blob – jednoduše přidat odečtena jednotky nebo zvýšit výkon. Ale zůstal výzvu pro zpracování stavových dat v relačních databází. Výzvy vyplývá v těchto scénářích:

* Zvětšování a zmenšování kapacity pro hello relační databáze část vašich úloh.
* Správa aktivní body, které může způsobit, které mají vliv na určitou podmnožinu dat – například zvlášť zaneprázdněn koncoví zákazník (klientů).

Obvyklým scénáře takovéto mít byla řešit Investujete do databáze rozsáhlejších servery toosupport hello aplikace. Tato možnost je však omezená hello cloudu, kde se stane veškeré zpracování na předdefinované komoditním hardwaru. Místo toho distribuci dat a zpracování mezi mnoha databázemi stejně jako strukturovaná (známé jako "horizontálního dělení" vzor Škálováním na více systémů) poskytuje alternativní tootraditional přístupy škálování z hlediska nákladů a pružnost.

## <a name="horizontal-and-vertical-scaling"></a>Vodorovného a svislého škálování
Hello obrázek níže ukazuje hello vodorovného a svislého rozměry škálování, které jsou hello základní způsoby, které je možné rozšířit hello elastické databáze.

![Vodorovný versus svislé škálování.][2]

Vodorovné škálování odkazuje tooadding nebo odebírání databáze v pořadí tooadjust kapacitu nebo celkový výkon. To je také označován "škálování" out. Horizontálního dělení, ve kterém jsou data rozdělena v kolekci stejně jako strukturovaný databází, je běžné tooimplement způsob vodorovné škálování.  

Svislé škálování odkazuje tooincreasing nebo snížení úrovně výkonu hello jednotlivé databáze – to se také označuje jako "vertikálním navýšení kapacity."

Většina databázových aplikací cloudového škálovatelného použije kombinaci těchto dvou strategií. Software jako služba aplikace může například použít vodorovné škálování tooprovision nové koncovým zákazníkům a vertical škálování tooallow každého koncoví zákazníka databáze toogrow nebo zmenšení prostředky podle potřeby hello zatížením.

* Vodorovné škálování je spravovat pomocí hello [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md).
* Svislé škálování se provádí pomocí prostředí Azure PowerShell rutiny toochange hello služby úroveň, nebo umístění databází v elastickém fondu.

## <a name="sharding"></a>Horizontálního dělení
*Horizontálního dělení* je technika toodistribute velkých objemů stejně jako strukturovaná data mezi několik nezávislých databáze. Jde zejména oblíbených s vývojáři cloudu vytváření Software jako služba (SAAS) nabídky pro koncové zákazníky nebo firmy. Tyto koncovým zákazníkům jsou často označují tooas "klienty". Horizontálního dělení může být nutný pro libovolný počet důvodů:  

* Hello celkové množství dat je příliš velký toofit omezením hello služby jedné databáze
* propustnost transakce Hello hello překračuje celkové zatížení hello možnosti služby jedné databáze
* Klienti mohou vyžadovat fyzické izolace od sebe navzájem tak samostatné databáze jsou potřeba pro každého klienta
* Různé části databáze může být nutné tooreside v různých zeměpisných regionech pro dodržování předpisů, výkon nebo geopolitické důvodů.

V dalších scénářích, třeba přijímání dat z distribuované zařízení může být horizontálního dělení použité toofill sadu databází, která jsou uspořádána dočasně. Například samostatné databáze může být vyhrazený tooeach dne nebo týdne. V takovém případě hello horizontálního dělení klíč může být celé číslo představující hello datum (součástí všechny řádky horizontálně dělené tabulky hello) a dotazy načítají se informace o rozsah dat musí směrovaný hello aplikace toohello podmnožinu databází pokrývajících hello rozsah v Otázka.

Horizontálního dělení funguje nejlíp, když lze každou transakci v aplikaci s omezeným přístupem tooa jedna hodnota klíče horizontálního dělení. Která zajistí, že všechny transakce bude místní tooa konkrétní databáze.

## <a name="multi-tenant-and-single-tenant"></a>Více klientů a jednoho klienta
Některé aplikace používají hello nejjednodušší způsob vytváření samostatnou databázi pro každého klienta. Toto je hello **jednoho klienta horizontálního dělení vzor** izolace, možnost zálohování a obnovení a prostředků, škálování na hello členitosti hello klienta, který poskytuje. S horizontálního dělení jednoho klienta je přidružená hodnota ID konkrétní klienta (nebo hodnota klíče zákazníka) každou databázi, ale klíči nemusí být vždy přítomen v hello samotná data. Ho je hello aplikace odpovědnost tooroute toohello příslušné databázi každého požadavku – a to zjednodušit hello klientské knihovny.

![Jednoho klienta a více klientů][4]

Jiné scénáře pack víc klientů současně do databáze, místo izoluje je do samostatné databáze. Toto je typické **horizontálního dělení víceklientské vzor** - a mohou být řízeny hello fakt, že aplikaci spravuje velkého počtu klientů velmi malé. Hello řádků v tabulkách databáze hello v víceklientské horizontálního dělení, jsou všechny toocarry určený klíč, který identifikuje ID nebo horizontálního dělení klíč tenanta hello. Znovu hello aplikační vrstvě je zodpovědná za směrování klienta požadavek toohello příslušnou databázi, a to může podporovat klientské knihovny pro elastické databáze hello. Kromě toho zabezpečení na úrovni řádků může být použité toofilter řádky k přístup každý klient – podrobnosti najdete v tématu [víceklientským aplikacím s nástroje elastické databáze a zabezpečení na úrovni řádků](sql-database-elastic-tools-multi-tenant-row-level-security.md). Redistribuce data mezi databáze může být potřeba pomocí vzoru horizontálního dělení víceklientské hello, a to usnadňují nástrojem rozdělení sloučení hello elastické databáze. toolearn Další informace o návrhových schématech aplikací SaaS pomocí elastické fondy, najdete v části [návrhová schémata pro víceklientské aplikace SaaS ve službě Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-toosingle-tenancy-databases"></a>Přesun dat z více databází toosingle klientů
Při vytváření aplikace SaaS, je typické toooffer potenciální zákazníci zkušební verzi softwaru hello. V takovém případě je nákladově efektivní toouse víceklientské databáze pro hello data. Když se stane potenciálního zákazníka zákazníka, databázi jednoho klienta je nicméně lepší vzhledem k tomu, že poskytuje lepší výkon. Pokud zákazník hello vytvořili dat během zkušební doby hello, použijte hello [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md) toomove hello data z hello víceklientské toohello novou databázi jednoho klienta.

## <a name="next-steps"></a>Další kroky
Ukázkovou aplikaci, která demonstruje hello klientské knihovny, najdete v části [začít pracovat s nástroji elastické Datababase](sql-database-elastic-scale-get-started.md).

tooconvert existující databáze nástroje hello toouse najdete v tématu [migrací existující databáze na více systémů tooscale](sql-database-elastic-convert-to-use-elastic-tools.md).

specifika hello toosee hello elastického fondu, najdete v části [cenové a výkonové požadavky fondu elastické databáze](sql-database-elastic-pool.md), nebo vytvořit nový fond s [elastické fondy](sql-database-elastic-pool-manage-portal.md).  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

