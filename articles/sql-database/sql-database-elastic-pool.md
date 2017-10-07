---
title: "aaaWhat jsou elastické fondy? Správa více databází SQL - Azure | Microsoft Docs"
description: "Spravovat a škálování více databází SQL - stovky a s tisíci - pomocí elastické fondy. Jeden ceny pro prostředky, které můžete distribuovat, kde je potřeba."
keywords: "více databází, databáze prostředků, výkon databáze"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 07/31/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 2098d7817ebe1277b5c131421f23c00803ec78f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-sql-databases"></a>Elastických fondů pomáhají spravovat a škálování více databází SQL

Fondy elastické databáze SQL jsou jednoduché a nákladově efektivní řešení pro správu a škálování více databází, které mají různou a nepředvídatelným nároky na využití. Hello databází v elastickém fondu se na jednom serveru Azure SQL Database a sdílet se stanoveným počtem prostředky ([jednotky transakcí elastické databáze](sql-database-what-is-a-dtu.md) (Edtu)) za cenu sady. Elastické fondy ve službě Azure SQL Database povolit SaaS vývojáři toooptimize hello ceny výkonu pro skupinu databází v rámci předepsaných nároky při dodávání pružnost výkonu pro každou databázi.   

> [!NOTE]
> Elastické fondy jsou obecně dostupné ve všech oblastech Azure s výjimkou oblasti Západní Indie, kde jsou aktuálně ve verzi Preview.  Verze GA pro elastické fondy se v této oblasti objeví co nejdřív.
>

## <a name="what-are-sql-elastic-pools"></a>Jaké jsou elastické fondy SQL? 

Vývojáři SaaS sestavují aplikace nad datovými úrovněmi velkého rozsahu, které se skládají z několika databází. Běžný vzor aplikací je tooprovision jedné databáze pro každého zákazníka. Ale různých zákazníků mají často mění a nepředvídatelným vzorce, a je těžké toopredict požadavky na prostředky hello každého uživatele jednotlivé databáze. Tradičně máte dvě možnosti: 

- Přepsání zřízení prostředků založenou na využití ve špičce a přes platím, nebo
- Snížení zřídit toosave náklady na náklady hello výkonu a k zákaznickým spokojenosti během vrcholů. 

Elastické fondy vyřešit tento problém tím, že zajistí databáze získat hello výkonu prostředky, které potřebují, kdy je potřebují. Poskytují jednoduchý mechanismus přidělování prostředků v mezích předvídatelného rozpočtu. toolearn Další informace o návrhových schématech aplikací SaaS pomocí elastické fondy, najdete v části [návrhová schémata pro víceklientské aplikace SaaS ve službě Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Elastické fondy povolit hello vývojáře toopurchase [jednotky transakcí elastické databáze](sql-database-what-is-a-dtu.md) (Edtu) pro fond sdílen více databází tooaccommodate nepředvídatelným období využití jednotlivých databází. požadavek Hello eDTU pro fond je určen podle hello agregační využití její databáze. Hello počet jednotek Edtu fondu k dispozici toohello řídí nároky vývojáře hello. Hello vývojáře jednoduše přidá fondu toohello databází, nastaví hello minimální a maximální počet jednotek Edtu pro hello databáze a nastaví hello eDTU fondu hello podle jejich nároky. Vývojář může použít fondy tooseamlessly růst své služby v štíhlého spuštění tooa vyspělá podnikových na stále rostoucí škálování.

V rámci fondu hello jsou uvedeny jednotlivé databáze hello flexibilitu tooauto rozsahu v rámci sady parametrů. V případě velkého zatížení databáze spotřebovat další vyžádání toomeet Edtu. Databáze při nízkém zatížení spotřebovávají méně eDTU a databáze bez zatížení nespotřebovávají žádné eDTU. Zřizování prostředků pro celý fond hello místo pro izolované databáze zjednodušuje úkoly správy. A navíc mají předvídatelný rozpočtu hello fondu. Další jednotek Edtu, které lze přidat existující fond tooan bez výpadků databáze, s tím rozdílem, že hello databáze může být nutné přesunout toobe tooprovide hello další výpočetní prostředky pro novou rezervaci eDTU hello. Podobně platí, že pokud již přidané eDTU nejsou potřebné, lze je z existujícího fondu kdykoli odebrat. A můžete přidat nebo odečíst databáze toohello fondu. Pokud databáze podle předpokladu nedostatečně využívá prostředky, odeberte ji.

Můžete vytvořit a spravovat fondu elastické databáze pomocí hello [portál Azure](sql-database-elastic-pool-manage-portal.md), [prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md), [Transact-SQL](sql-database-elastic-pool-manage-tsql.md), [C#](sql-database-elastic-pool-manage-csharp.md), a hello REST API. 

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Pokud byste zvážit elastického fondu SQL Database?

Fondy jsou vhodné pro velký počet databází s konkrétními vzory využití. Pro danou databázi je tento vzor charakterizován nízkou mírou průměrného využití s relativně málo častými nárůsty využití.

Hello více databází můžete přidat tooa fondu hello větší že stane vaše úspory. V závislosti na vaší vzor využití aplikací je možné toosee úspor s pouhými dvěma S3 databáze.  

Hello následující části Nápověda porozumíte jak tooassess Pokud konkrétní kolekce databází můžete využívat výhody ve fondu. Hello příklady používat standardní fondy ale hello stejné zásady platí také tooBasic a fondy Premium.

### <a name="assessing-database-utilization-patterns"></a>Vyhodnocení vzorů využití databáze

Hello následující obrázek znázorňuje příklad databáze, která tráví mnoho času nečinnosti, ale také pravidelně špičky s aktivitou. Tento vzor využití se pro fond skvěle hodí:

   ![izolovaná databáze vhodná pro fond](./media/sql-database-elastic-pool/one-database.png)

Pro hello pětiminutovou období zobrazený, DB1 vrcholů až too90 Dtu, ale jeho celkové průměrné využití je menší než 5 Dtu. S3 úroveň výkonu je nezbytné toorun tímto zatížením v jedné databáze, ale to zanechává většinu hello prostředků nepoužívané během období nízkou aktivity.

Fond umožňuje tyto nepoužívané toobe Dtu sdílet mezi více databází a snižuje tak náklady na potřebné a celkový počet jednotek Dtu hello.

Vytváření v předchozím příkladu hello Předpokládejme, že existují další databáze pomocí podobné vzorů využití jako DB1. V hello další dva následující obrázky, hello využití čtyři databází a 20 databáze jsou vrstvu na hello stejné graf tooillustrate hello nepřekrývají povaze jejich využití v čase:

   ![Čtyři databáze se vzorem využití, který je vhodný pro fond](./media/sql-database-elastic-pool/four-databases.png)

  ![Dvacet databází se vzorem využití, který je vhodný pro fond](./media/sql-database-elastic-pool/twenty-databases.png)

hello černá čára v hello předchozím obrázku je znázorněn Hello agregační využití DTU napříč všechny 20 databáze. Ukazuje to, že agregační využití DTU hello nikdy překračuje 100 Dtu a určuje, že hello 20 databází můžete sdílet 100 Edtu za toto časové období. To vede k 20 x snížení Dtu a 13 x cena snížení porovnání tooplacing každý hello databází v S3 úrovně výkonu pro izolované databáze.

V tomto příkladu je ideální pro hello následujících důvodů:

* Ukazuje velké rozdíly mezi využitím ve špičce a průměrným využitím jednotlivých databází.  
* Hello ve špičce využití pro každou databázi nastane v různých okamžicích v čase.
* Jednotky eDTU jsou sdílené mezi mnoha databázemi.

cena Hello fondu je funkce Edtu fondu hello. 1,5 x větší než hello DTU jednotkové ceny pro jednu databázi, při hello eDTU jednotkové ceny pro fond **fondu může být sdílen počet databází a méně celkový počet jednotek Edtu, které jsou potřeba**. Tyto rozdíly v ceny a sdílení eDTU jsou hello základ pro potenciální úspora cena hello, která poskytují fondy.  

Hello následující zásady související s toodatabase počet a databáze využití nápovědy tooensure který doručí fond snížit náklady na porovnání toousing úrovně výkonu pro izolované databáze.

### <a name="minimum-number-of-databases"></a>Maximální počet databází

Pokud hello součet hello počet jednotek Dtu úrovně výkonu pro izolované databáze více než 1,5 x hello Edtu pro fond hello potřeba, větší finanční efektivita je fondu elastické databáze. Dostupné velikosti najdete v tématu [Omezení úložiště a eDTU pro elastické fondy a elastické databáze](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

***Příklad***<br>
Alespoň dva S3 databáze nebo alespoň 15 S0 databáze je třeba 100 eDTU fondu toobe cenově výhodnější než použití úrovně výkonu pro izolované databáze.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maximální počet databází se souběžnými špičkami

Sdílení Edtu, ne všechny databáze ve fondu současně pomocí Edtu až toohello omezení, které jsou k dispozici při použití úrovně výkonu pro izolované databáze. Dobrý den méně databáze, které současně ve špičkách, hello eDTU fondu nižší hello může být sada a hello se stává další nákladově efektivní hello fondu. Obecně platí není větší než 2 nebo 3 (nebo 67 %) hello databází ve fondu hello by měl současně ve špičkách tootheir eDTU omezit.

***Příklad***<br>
tooreduce náklady na tři databáze S3 ve 200 eDTU fondu, maximálně dva z těchto databází můžete současně ve špičkách jejich využití. Pokud více než dva z těchto čtyř databází S3 současně ve špičkách, by měla mít hello fondu, jinak hodnota toomore toobe velikost než 200 Edtu. Pokud fond hello se změněnou velikostí toomore než 200 Edtu, další S3 databáze potřebovat toobe přidat toohello fondu tookeep náklady nižší než úrovně výkonu pro izolované databáze.

Všimněte si, že v tomto příkladu nebere v úvahu využití jiných databází ve fondu hello. Pokud všechny databáze mají některé využití v libovolném bodě v čase, pak je menší než 2 nebo 3 (nebo 67 %) databází hello může ve špičkách současně.

### <a name="dtu-utilization-per-database"></a>Využití DTU na databázi
Velký rozdíl mezi hello maximální a průměrné využití databáze označuje delší doba nízkou úrovní využití a krátkodobě vysoké využití. Tento vzor využití je ideální pro sdílení prostředků mezi databázemi. Použití fondu pro databázi byste měli zvážit, pokud je její využití ve špičce přibližně 1,5krát větší než průměrné využití.

***Příklad***<br>
Databáze technologie S3 píků too100 Dtu a v průměru používá 67 Dtu nebo menší je vhodným kandidátem pro sdílení Edtu ve fondu. Alternativně databázi S1, který píků too20 Dtu a v průměru používá 13 Dtu nebo menší je vhodným kandidátem pro fond.

## <a name="how-do-i-choose-hello-correct-pool-size"></a>Jak vybrat velikost správné fondu hello?

Hello doporučené velikosti pro fond závisí na hello agregační Edtu a prostředky úložiště, které jsou potřebné pro všechny databáze ve fondu hello. To zahrnuje určení hello větší z hello následující:

* Maximální počet jednotek Dtu využívaných všechny databáze ve fondu hello.
* Maximální velikost úložiště bajtů využívaných všechny databáze ve fondu hello.

Dostupné velikosti najdete v tématu [Omezení úložiště a eDTU pro elastické fondy a elastické databáze](#what-are-the-resource-limits-for-elastic-pools).

SQL Database automaticky vyhodnotí využití historických prostředků hello databází v existující databázi SQL server a doporučuje hello odpovídajícímu fondu adres konfigurace v hello portálu Azure. Integrované prostředí v přidání toohello doporučení, odhadne hello využití eDTU pro vlastní skupinu databází na serveru hello. To umožňuje toodo "" analýz interaktivně přidáním databáze toohello fondu a jejich odebrání tooget analýza využití prostředků a změna velikosti Rady, jak před potvrzením změny. Postupy najdete v tématu [Monitorování, správa a nastavení velikosti elastického fondu](sql-database-elastic-pool-manage-portal.md).

V případech, kdy nelze používat nástroje hello následující podrobné můžete odhadnout, zda fond je cenově výhodnější než izolovaných databází:

1. Odhad hello Edtu potřebné pro fond hello následujícím způsobem:

   MAX(<*celkový počet databází* X *průměrné využití DTU na databázi*>,<br>
   <*počet databází se souběžnou špičkou* X *využití DTU ve špičce na databázi*)
2. Odhad hello úložiště potřebné pro fond hello přidáním hello počet bajtů, které jsou potřebné pro všechny databáze hello ve fondu hello. Pak určete velikost hello eDTU fondu, která poskytuje toto množství úložiště. Omezení úložiště fondů na základě velikosti fondu v jednotkách eDTU najdete v tématu [Omezení úložiště a eDTU pro elastické fondy a elastické databáze](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).
3. Trvat hello větší z odhadované eDTU hello z kroku 1 a 2 krok.
4. V tématu hello [SQL Database stránce s cenami](https://azure.microsoft.com/pricing/details/sql-database/) a najít nejmenší eDTU fondu hello velikost, která je větší než odhad hello z kroku 3.
5. Porovnejte hello fondu cena z kroku 5 toohello cena pomocí úrovně hello odpovídající výkonu pro izolované databáze.

### <a name="changing-elastic-pool-resources"></a>Změna elastického fondu prostředků

Můžete zvýšit nebo snížit hello prostředky k dispozici tooan elastického fondu na základě potřeb prostředků.

* Změna hello minimální počet jednotek Edtu na databázi nebo max Edtu na databázi obvykle dokončení během 5 minut nebo méně.
* Změna hello Edtu na fond závisí na celkovém množství místo využité všechny databáze ve fondu hello hello. Změny průměrně trvají méně než 90 minut na každých 100 GB. Například pokud celkové místo hello používá všechny databáze ve fondu hello je 200 GB, pak hello očekává, že latence pro změnu eDTU fondu hello na fondu je 3 hodiny nebo méně.

## <a name="what-are-hello-resource-limits-for-elastic-pools"></a>Jaká jsou omezení hello prostředků pro elastické fondy?

Hello následující tabulky popisují omezení prostředků hello elastické fondy.  Upozorňujeme, že omezení prostředků hello jednotlivých databází v elastické fondy jsou obecně stejná jako u izolovaných databází mimo fondů na základě Dtu a úroveň služby hello hello.  Například hello maximální počet souběžných pracovních procesů pro databázi S2 je 120 pracovních procesů.  Ano hello maximální počet souběžných pracovních procesů pro databázi v standardní fond je také 120 pracovníci Pokud hello maximální počet jednotek DTU na databázi ve fondu hello 50 Dtu (což je ekvivalentní tooS2).

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Pokud se používají všechny Dtu elastického fondu, každá databáze ve fondu hello obdrží stejné množství prostředků tooprocess dotazy.  Hello služba SQL Database poskytuje sdílení rovnosti mezi databázemi zajištěním rovna výpočetní časové úseky prostředků. Sdílení rovnosti elastický fond zdrojů je navíc tooany množství prostředků, v opačném případě zaručit tooeach databáze hello minimální počet jednotek DTU na databázi nastavena tooa nenulovou hodnotu.

### <a name="database-properties-for-pooled-databases"></a>Vlastnosti databáze pro databáze ve fondu

Hello následující tabulka popisuje vlastnosti hello databází ve fondu.

| Vlastnost | Popis |
|:--- |:--- |
| Maximální počet eDTU na databázi |Hello maximální počet jednotek Edtu, všechny databáze ve fondu hello může použít, pokud je k dispozici na základě využití jiných databází ve fondu hello.  Maximální počet eDTU na databázi není garancí prostředků pro databázi.  Toto nastavení je globální nastavení, která se použije tooall databáze ve fondu hello. Nastavte maximální počet jednotek Edtu na databázi dostatečně vysoký toohandle vrcholů databáze využití. Určitý stupeň overcommitting je očekáváno od fondu hello obecně předpokládá úrovněmi horkého a studeného vzorce pro databáze, kde nejsou současně peaking všechny databáze. Předpokládejme například, využití ve špičce hello za databáze je 20 Edtu a jenom 20 % hello 100 databáze ve fondu hello jsou ve špičce na hello stejnou dobu.  Pokud hello eDTU max na databázi nastavena too20 Edtu, je 5krát a sadu hello Edtu na fond too400 přiměřené tooovercommit hello fondu. |
| Minimální počet eDTU na databázi |Hello minimální počet Edtu, který zaručeně všechny databáze ve fondu hello.  Toto nastavení je globální nastavení, která se použije tooall databáze ve fondu hello. Hello min eDTU na databázi, může být nastaven too0 který je taky hello výchozí hodnotu. Tato vlastnost nastavena tooanywhere mezi 0 a hello využití průměrná eDTU na databázi. Hello produkt hello počet databází ve fondu hello a hello minimální počet jednotek Edtu na databázi nesmí překročit hello Edtu na fond.  Například pokud fond obsahuje 20 databází a minimální hello eDTU na databázi nastavit too10 Edtu, pak hello Edtu na fond musí být alespoň tak velké, jaká 200 Edtu. |
| Maximální velikost úložiště dat na databázi |Hello maximální velikost úložiště pro databázi ve fondu. Ve fondu databáze sdílet fond úložiště, takže úložiště databáze je omezená toohello menší zbývající fondu úložiště a maximální úložiště na databázi. Maximální počet úložiště na databázi odkazuje toohello maximální velikost hello datové soubory a nezahrnuje hello místu soubory protokolu. |
|||

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Další funkce SQL Database pomocí elastické fondy

### <a name="elastic-jobs-and-elastic-pools"></a>Elastické úlohy a elastické fondy

U fondu jsou úlohy správy zjednodušené díky spouštění skriptů v **[elastických úlohách](sql-database-elastic-jobs-overview.md)**. Elastická úloha eliminuje většinu únavných úkolů spojených s velkým počtem databází. toobegin, najdete v části [Začínáme s elastické úlohy](sql-database-elastic-jobs-getting-started.md).

Další informace o ostatních databázových nástrojích pro práci s více databázemi najdete v tématu [Horizontální navýšení kapacity se službou Azure SQL Database](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Možnosti kontinuity podnikových procesů u databází v elastickém fondu
Databáze ve fondu obvykle podporu hello stejné [kontinuitu podnikových procesů obchodní](sql-database-business-continuity.md) , které jsou k dispozici toosingle databáze.

- **Obnovení bodu v čase**: bodu v době obnovení používá toorecover databáze automatické zálohování databáze v fondu tooa určitého bodu v čase. Viz [Obnovení k určitému bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore).

- **Geografické obnovení**: geografické obnovení poskytuje možnost obnovení hello výchozí, když databáze není k dispozici z důvodu incidentu v oblasti hello je hostitelem databáze hello. V tématu [obnovit databáze SQL Azure nebo převzetí služeb při selhání tooa sekundární](sql-database-disaster-recovery.md)

- **Aktivní geografickou replikací**: aplikace, které mají agresivnější požadavky na obnovení než nabízejí geografické obnovení, nakonfigurujte [aktivní geografickou replikací](sql-database-geo-replication-overview.md).

## <a name="manage-sql-database-elastic-pools-using-hello-azure-portal"></a>Spravovat fondy elastické databáze SQL pomocí hello portálu Azure

### <a name="creating-a-new-sql-database-elastic-pool-using-hello-azure-portal"></a>Vytvoření nového elastického fondu SQL Database pomocí portálu Azure hello

Existují dva způsoby fondu elastické databáze můžete vytvořit v hello portálu Azure. Můžete provést od začátku Pokud znáte hello fondu instalaci nebo spuštění s doporučením ze služby hello. SQL Database má vestavěné inteligentní, která doporučuje instalační program elastického fondu, pokud je cenově výhodnější založené na hello po telemetrii využití databází. 

Vytvoření fondu elastické databáze z existující **server** okna portálu hello je hello nejjednodušší způsob, jak toomove existující databáze do pružného fondu. Můžete také vytvořit fondu elastické databáze vyhledávání **elastický fond SQL** v hello **Marketplace** nebo kliknutím na tlačítko **+ přidat** v hello **SQL elastické fondy**Procházet okna. Budete mít toospecify nového nebo existujícího serveru přes tento fond zřizování pracovního postupu.

> [!NOTE]
> Můžete vytvořit více fondů na serveru, ale nemůžete přidat databáze z různých serverů do hello stejného fondu.
>  

Hello cenová úroveň fondu určuje hello funkce dostupné toohello elastics v hello fondu a hello maximální počet jednotek Edtu (eDTU MAX) a úložiště (GB) k dispozici tooeach databáze. Podrobnosti najdete v tématu [úrovně služeb](#edtu-and-storage-limits-for-elastic-pools).

Klikněte na tlačítko toochange hello cenovou úroveň pro fond hello **cenová úroveň**, klikněte na tlačítko hello cenové úrovně a pak klikněte na tlačítko **vyberte**.

> [!IMPORTANT]
> Pokud zvolíte hello cenová úroveň a potvrdíte svoji volbu kliknutím na **OK** v posledním kroku hello, nebude možné toochange hello cenovou úroveň fondu hello. toochange hello cenovou úroveň existujícího elastického fondu, vytvoření fondu elastické databáze v hello požadovanou cenovou úrovní a migrace hello databáze toothis nový fond.
>

Pokud pracujete s databází hello mají dostatek historických telemetrických dat, hello **Odhadované využití eDTU a GB** grafu a hello **skutečné využití eDTU** toohelp aktualizace pruhový graf provedete konfiguraci rozhodnutí. Navíc služba hello může zobrazovat toohelp zprávy doporučení je optimální velikost fondu hello.

Hello služba SQL Database vyhodnocuje historii využití a doporučí jeden nebo více fondů, pokud bude cenově výhodnější než použití izolované databáze. Každé doporučení je konfigurováno pro jedinečnou podmnožinu databází serveru hello, které co nejlépe vyhovovaly hello fondu.

![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

Hello doporučení fondu zahrnuje:

- Cenovou úroveň pro hello fond (Basic, Standard, Premium nebo Premium RS)
- vhodnou hodnotu **POOL eDTU** (označovanou také jako Max eDTU pro fond),
- Hello **eDTU MAX** a **eDTU Min** na databázi
- Hello seznam doporučených databází pro fond hello

> [!IMPORTANT]
> Služba Hello bere hello posledních 30 dnech telemetrie v úvahu při doporučujeme fondy. Pro databáze toobe, který zařazena mezi doporučené fondu elastické databáze musí existovat alespoň 7 dní. Databáze, které již v elastickém fondu jsou, se nepovažují za kandidáty pro doporučení elastického fondu.
>

Hello služba vyhodnocuje potřebné prostředky a nákladů plynoucí z přesunutí hello jedné databáze na jednotlivých úrovních služby do fondů hello stejné úrovně. Například pro všechny databáze na serveru, které jsou v úrovni Standard, se zvažuje výhodnost jejich přesunu do elastického fondu také s úrovní Standard. To znamená, že služba hello neprovádí vrstvě mezi úrovněmi, například přesun standardní databáze ve fondu Premium.

Po přidání databází toohello fondu, doporučení se dynamicky vygeneruje, na základě historie využití hello hello databází, které jste vybrali. Tato doporučení jsou zobrazeny v hello eDTU a GB využití graf a v hlavičce doporučení hello horní části hello **konfigurace fondu** okno. Tato doporučení jsou určený tooassist můžete při vytváření fondu elastické databáze optimalizované pro vaše konkrétní databáze.

![dynamická doporučení](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

### <a name="manage-and-monitor-an-elastic-pool"></a>Správa a sledování fondu elastické databáze

Hello portálu Azure můžete sledovat využití hello fondu elastické databáze a databáze hello v tomto fondu. Můžete také nastavit sadu změn tooyour elastický fond a odeslat všechny změny v hello stejný čas. Tyto změny zahrnují přidávání nebo odebírání databáze, změna nastavení služby elastického fondu nebo změna nastavení databáze.

Hello následující obrázek znázorňuje příklad elastickém fondu. zobrazení Hello zahrnuje:

*  Grafy pro sledování využití prostředků hello elastického fondu a databáze hello obsažené ve fondu hello.
*  Hello **konfigurace** fondu tlačítko toomake změny toohello elastického fondu.
*  Hello **vytvořit databázi** tlačítko, která vytváří databázi a přidává ji toohello aktuální elastického fondu.
*  Elastické úlohy, které vám pomůžou spravovat velké počty databází pomocí spouštění skriptů Transact SQL pro všechny databáze v seznamu.

![Zobrazení fondu](./media/sql-database-elastic-pool-manage-portal/basic.png)

Můžete přejít tooa konkrétní fond toosee jeho využití prostředků. Ve výchozím nastavení hello fond je nakonfigurované tooshow využití úložiště a eDTU pro hello poslední hodinu. Hello graf může být nakonfigurované tooshow jiné metriky v různých časových oken. Klikněte na tlačítko hello **využití prostředků** grafu v části **elastického fondu monitorování** tooshow podrobný přehled o hello zadaný metriky přes hello zadaný časový interval.

![Monitorování elastického fondu](./media/sql-database-elastic-pool-manage-portal/basic-2.png)

![Okno Metrika](./media/sql-database-elastic-pool-manage-portal/metric.png)

### <a name="toocustomize-hello-chart-display"></a>zobrazení grafu toocustomize hello

Graf hello a hello metriky okno toodisplay můžete upravit jiné metriky jako procento, procento vstupů/výstupů dat a protokolu vstupně-výstupní operace % využití procesoru.

![Klikněte na tlačítko Upravit](./media/sql-database-elastic-pool-manage-portal/edit-metric.png)

Na hello **upravit graf** formuláře, můžete vybrat časové rozmezí (po hodině, ještě dnes, nebo za minulý týden), nebo klikněte na tlačítko **vlastní** tooselect libovolné datum v rozsahu v hello poslední dva týdny. Můžete zvolit pás nebo spojnicový graf a pak vyberte prostředky toomonitor hello.

> [!Note]
> Pouze metriky s hello tutéž jednotku lze zobrazit v hello grafu v hello stejný čas. Například pokud zvolíte možnost "eDTU procento" pak můžete zvolit pouze další metriky s procentem jako měrné jednotky hello.
>

[Klikněte na tlačítko Upravit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

### <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Spravovat a monitorovat databází v elastickém fondu

Jednotlivé databáze je možné monitorovat také pro potenciální problémy. V části **elastické databáze monitorování**, je graf, který zobrazí metriky pro pět databáze. Ve výchozím nastavení, hello graf zobrazuje hello top 5 databáze ve fondu hello podle využití eDTU průměrná v hello poslední hodinu. 

![Monitorování elastického fondu](./media/sql-database-elastic-pool-manage-portal/basic-3.png)

Klikněte na tlačítko hello **využití eDTU pro databáze pro hello poslední hodinu** pod **elastické databáze monitorování**. Tím se otevře **využití prostředků databáze** a poskytuje podrobný přehled o využití hello databáze ve fondu hello. Pomocí hello mřížky v dolní části okna hello hello, můžete vybrat všechny databáze ve fondu toodisplay hello jeho použití v grafu hello (až too5 databáze). Můžete také upravit hello metriky a časového okna zobrazí v grafu hello kliknutím **upravit graf**.

![Okna využití prostředků databáze](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="toocustomize-hello-view"></a>zobrazení toocustomize hello

Můžete upravit graf tooselect hello časový interval (za hodinu nebo za posledních 24 hodin), nebo klikněte na tlačítko **vlastní** tooselect různých denně v hello po toodisplay 2 týdny.

![Klikněte na tlačítko Upravit graf](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

![Klikněte na tlačítko Vlastní](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

Můžete také kliknout na hello **porovnat databází tak, že** rozevírací tooselect jiné metriky toouse při porovnávání databáze.

![Upravit graf hello](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="tooselect-databases-toomonitor"></a>toomonitor tooselect databáze

V seznamu hello databáze v hello **využití prostředků databáze** okno, můžete vyhledat konkrétní databáze hello stránky v seznamu hello zaměřením nebo zadáním v hello název databáze. Použijte hello políčko tooselect hello databázi.

![Vyhledejte toomonitor databáze](./media/sql-database-elastic-pool-manage-portal/select-dbs.png)


### <a name="add-an-alert-tooan-elastic-pool-resource"></a>Přidat prostředek výstrahy tooan elastického fondu

Můžete přidat tooan pravidla elastické se fond, který toopeople nebo výstrahu tooURL koncových bodů řetězce odeslání e-mailu, pokud se elastický fond hello dotkne prahovou hodnotu využití, které jste nastavili.

**tooadd prostředek tooany výstrahy:**

1. Klikněte na tlačítko hello **využití prostředků** grafu tooopen hello **metrika** okně klikněte na tlačítko **přidat upozornění**a potom vyplňte informace hello v hello **přidat oznámení pravidlo** okno (**prostředků** je automaticky nastavení fondu hello toobe pracujete s).
2. Zadejte **název** a **popis** identifikující tooyou hello výstrah a příjemce hello.
3. Vyberte **metrika** , které chcete tooalert hello seznamu.

    Graf Hello dynamicky znázorňuje využití prostředků pro tuto metriky toohelp, že si vyberete prahovou hodnotu.

4. Vyberte **podmínku** (větší než menší než atd) a **prahová hodnota**.
5. Vyberte **období** času, který hello metrika pravidlo je nutné splnit před hello výstrahy aktivační události.
6. Klikněte na **OK**.

Další informace najdete v tématu [vytvářet výstrahy, SQL Database na portálu Azure](sql-database-insights-alerts-portal.md).

### <a name="move-a-database-into-an-elastic-pool"></a>Přesun databáze do pružného fondu

Můžete přidat nebo odebrat databáze z existující fond. Hello databáze může být v jiných fondech. Však lze přidat pouze databáze, které jsou na hello stejný logický server.

 ![Klikněte na tlačítko Konfigurovat fond](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

![Klikněte na tlačítko Přidat toopool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)

![Vyberte tooadd databáze](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

![Přidání čekající fondu](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

![Kliknutí na Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="move-a-database-out-of-an-elastic-pool"></a>Přesunutí databáze z fondu elastické databáze

![výpis databáze](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

![výpis databáze](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

![Náhled databáze přidávání a odebírání](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

![Kliknutí na Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="change-performance-settings-of-an-elastic-pool"></a>Změňte nastavení výkonu elastického fondu

Při sledování využití prostředků hello elastického fondu, může se stát, že některé změny jsou potřeba. Možná hello fondu musí změnu omezení hello výkon nebo úložiště. Pravděpodobně budete chtít toochange hello nastavení databáze ve fondu hello. Můžete změnit nastavení hello hello fondu na všechny čas tooget hello nejlepší kompromis výkonu a nákladů. V tématu [při fondu elastické databáze slouží?](sql-database-elastic-pool.md) Další informace.

toochange hello Edtu nebo úložiště, omezuje na fond a Edtu na databázi:

![Využití elastického fondu prostředků](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

![Aktualizace služby elastického fondu a měsíční náklady na nový](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="manage-sql-database-elastic-pools-using-powershell"></a>Spravovat fondy elastické databáze SQL pomocí prostředí PowerShell

toocreate a spravovat fondy elastické databáze SQL Azure PowerShell, použijte hello následující rutiny prostředí PowerShell. Pokud potřebujete tooinstall nebo upgrade prostředí PowerShell, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps). toocreate a spravovat databáze, servery a pravidla brány firewall, viz [vytvořit a spravovat servery Azure SQL Database a databáze pomocí prostředí PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell). 

> [!TIP]
> Příklad skriptů prostředí PowerShell, najdete v části [vytvořit elastické fondy a přesunutí databází mezi fondy a z fondu pomocí prostředí PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) a [toomonitor použijte PowerShell a škálování SQL elastického fondu v Azure SQL Database](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Rutina | Popis |
| --- | --- |
|[Nový AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Vytvoří fond elastické databáze na logický SQL server.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Získá elastické fondy a jejich hodnoty vlastností na logický SQL server.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Upraví vlastnosti fondu elastické databáze na logický SQL server.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Odstraní fond elastické databáze na logický SQL server.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Získá stav hello operací v elastickém fondu na logický SQL server.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Vytvoří novou databázi v existující fond nebo jedné databáze. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Získá jednu nebo více databází.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Nastaví vlastnosti pro databázi nebo existující databázi se přesune do, mimo nebo mezi elastické fondy.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Odebere databáze.|

> [!TIP]
> Vytváření mnoho databází v elastickém fondu může trvat dobu, pokud se provádí pomocí hello portálu nebo rutiny prostředí PowerShell, který současně vytvořit pouze jednu databázi. Vytvoření tooautomate do pružného fondu, najdete v části [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="manage-sql-database-elastic-pools-using-hello-azure-cli"></a>Spravovat fondy elastické databáze SQL pomocí hello rozhraní příkazového řádku Azure

toocreate a spravovat fondy elastické databáze SQL se hello [rozhraní příkazového řádku Azure](/cli/azure/overview), použijte hello [databáze SQL Azure CLI](/cli/azure/sql/db) příkazy. Použití hello [cloudové prostředí](/azure/cloud-shell/overview) toorun hello rozhraní příkazového řádku v prohlížeči nebo [nainstalovat](/cli/azure/install-azure-cli) ji v systému macOS, Linux nebo Windows. 

> [!TIP]
> Příklad skriptů příkazového řádku Azure CLI, najdete v části [toomove použití rozhraní příkazového řádku Azure SQL database v elastický fond SQL](scripts/sql-database-move-database-between-pools-cli.md) a [použití Azure CLI tooscale elastický fond SQL v Azure SQL Database](scripts/sql-database-scale-pool-cli.md).
>

| Rutina | Popis |
| --- | --- |
|[Vytvoření az sql elastického fondu](/cli/azure/sql/elastic-pool#create)|Vytvoří fondu elastické databáze.|
|[seznam elastického fondu sql az](/cli/azure/sql/elastic-pool#list)|Vrátí seznam hodnot elastické fondy na serveru.|
|[seznam – databáze az sql elastického fondu](/cli/azure/sql/elastic-pool#list-dbs)|Vrátí seznam databází v elastickém fondu.|
|[seznam edicí az sql elastického fondu](/cli/azure/sql/elastic-pool#list-editions)|Také zahrnuje nastavení DTU fondu k dispozici, limity úložiště a pro nastavení databáze. V pořadí tooreduce podrobností, limity další úložiště a na databázi, jsou ve výchozím nastavení skryté nastavení.|
|[aktualizace elastického fondu sql az](/cli/azure/sql/elastic-pool#update)|Aktualizace fondu elastické databáze.|
|[odstranění elastického fondu sql az](/cli/azure/sql/elastic-pool#delete)|Odstraní hello elastického fondu.|

## <a name="manage-sql-database-elastic-pools-using-transact-sql"></a>Spravovat fondy elastické databáze SQL pomocí jazyka Transact-SQL

toocreate a přesunutí databází v rámci existující elastické fondy nebo tooreturn informace o elastického fondu SQL Database pomocí jazyka Transact-SQL, použijte následující příkazy T-SQL hello. Můžete použít tyto příkazy pomocí hello portál Azure, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), nebo jiný program, který se můžete připojit server Azure SQL Database tooan a předat Transact-SQL příkazy. toocreate a spravovat databáze, servery a pravidla brány firewall, viz [vytvořit a spravovat servery Azure SQL Database a databází pomocí jazyka Transact-SQL](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> Nelze vytvořit, aktualizovat nebo odstranit fondu elastické databáze Azure SQL Database pomocí jazyka Transact-SQL. Můžete přidat nebo odebrat z fondu elastické databáze, a pomocí zobrazení dynamické správy tooreturn informací o existující elastické fondy.
>

| Příkaz | Popis |
| --- | --- |
|[Vytvoření databáze (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Vytvoří novou databázi v existující fond nebo jedné databáze. Musí být připojené toohello hlavní databázi toocreate novou databázi.|
| [Příkaz ALTER DATABASE (databáze Azure SQL)](/sql/t-sql/statements/alter-database-azure-sql-database) |Přesun databáze do, mimo nebo mezi elastické fondy.|
|[ODPOJENÍ databáze (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Odstraní databázi.|
|[Sys.elastic_pool_resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Vrátí statistiku využití prostředků pro všechny fondy elastické databáze hello logického serveru. Pro každý fond elastické databáze je jeden řádek pro každou 15 sekundu reporting okno (čtyři řádky za minutu). To zahrnuje využití procesoru, vstupně-výstupní operace, protokolu, spotřebu úložiště a souběžných požadavku nebo relace využití všech databází ve fondu hello.|
|[Sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Vrátí hello edition (vrstva služby), cíl služby (cenové úrovně) a název elastického fondu, pokud existuje, pro databázi Azure SQL nebo Azure SQL Data Warehouse. Pokud přihlášení toohello hlavní databázi v serveru Azure SQL Database, vrátí informace na všechny databáze. Pro Azure SQL Data Warehouse musí být připojené toohello hlavní databázi.|

## <a name="manage-sql-database-elastic-pools-using-hello-rest-api"></a>Spravovat fondy elastické databáze SQL pomocí hello REST API

toocreate a spravovat fondy elastické databáze SQL pomocí hello REST API, přečtěte si [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Další kroky

* Video najdete v tématu [Microsoft Virtual Academy video kurzu na možnostech elastické databáze SQL Azure](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* toolearn Další informace o návrhových schématech aplikací SaaS pomocí elastické fondy, najdete v části [návrhová schémata pro víceklientské aplikace SaaS ve službě Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* SaaS pomocí elastické fondy, na adrese [Úvod toohello aplikace Wingtip SaaS](sql-database-wtp-overview.md).
