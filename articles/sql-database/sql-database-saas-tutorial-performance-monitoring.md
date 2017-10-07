---
title: "výkon aaaMonitor mnoho databází Azure SQL v aplikaci SaaS víceklientské | Microsoft Docs"
description: "Sledování a správa výkonu databáze a fondy v aplikaci Azure SQL Database Wingtip SaaS hello"
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: sstein
ms.openlocfilehash: f0d7ba456c485b7de249a56abac3cf4be3857285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-performance-of-hello-wingtip-saas-application"></a>Sledování výkonu hello Wingtip SaaS aplikace

V tomto kurzu jsou prozkoumali několik klíčových scénářů správy používány aplikací SaaS. Použití aktivitu zatížení generátor toosimulate ve všech databází klienta, hello integrované monitorování a výstrah funkce SQL Database a elastické fondy je ukázán.

aplikace Wingtip SaaS Hello používá klienta jeden datový model, kde každý místo (klient) má své vlastní databázi. Mnoho aplikací SaaS, jako je hello očekává, že vzor zatížení klienta je nepředvídatelným a ojediněle. Jinými slovy to znamená, že prodej lístků může probíhat kdykoli. tootake výhodou tohoto vzoru typické databáze využití, klienta, které databáze jsou nasazeny do fondů elastické databáze. Elastické fondy optimalizovat náklady hello řešení sdílení prostředků mezi mnoha databázemi. U tohoto typu vzor je důležité toomonitor databáze a tooensure využití prostředků fondu, který načítá se to bude přiměřeně rovnoměrně mezi fondy. Musíte taky tooensure splnit jednotlivé databáze s dostatkem prostředků a že nejsou stiskne fondy jejich [eDTU](sql-database-what-is-a-dtu.md) omezení. V tomto kurzu jsou zde popsány toomonitor způsoby a spravovat databáze a fondy a jak tootake opravné akce v odpovědi toovariations v zatížení.

V tomto kurzu se naučíte:

> [!div class="checklist"]

> * Simulovat využití v databázích hello klienta spuštěním generátor zadaný zatížení
> * Databáze monitorování hello klienta jako jejich reagovat toohello zvýšení zatížení
> * Škálování hello elastického fondu v odpovědi toohello databáze zvýšené zatížení
> * Zřídit aktivitu databáze služby druhý elastický fond tooload zůstatek


toocomplete dokončení tohoto kurzu, ujistěte se, hello následující požadavky:

* Hello Wingtip SaaS aplikace je nasazená. toodeploy za méně než pět minut, najdete v části [nasazení a seznamte se s hello Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-toosaas-performance-management-patterns"></a>Vzory správy výkonu tooSaaS Úvod

Správa výkonu databáze se skládá z kompilování a analýza dat výkonu a pak reaktivní toothis dat úpravou parametrů toomaintain přijatelnou dobu odezvy pro vaši aplikaci. Při hostování více klientů, fondy elastické databáze jsou nákladově efektivní způsob tooprovide a spravovat prostředky pro skupinu databází s nepředvídatelným úlohy. Při určitých vzorcích úloh může být správa ve fondu užitečná pro pouhé dvě databáze S3.

![média](./media/sql-database-saas-tutorial-performance-monitoring/app-diagram.png)

Fondy a hello databází ve fondech, by měla být monitorovaná tooensure, které zůstanou v rámci přijatelné rozsahů výkonu. Vyladění konfigurace fondu hello toomeet hello potřebám hello agregační zatížení všech databází, zajistíte, že Edtu fondu hello jsou vhodné pro hello celkové zatížení. Upravte hello jednotlivé databáze min a -database max eDTU hodnoty tooappropriate hodnoty pro vaše konkrétní požadavky aplikací.

### <a name="performance-management-strategies"></a>Strategie výkonu aplikací

* tooavoid s toomanually monitorování výkonu, je příliš co nejúčinnější**nastavit výstrahy, které aktivují, když databáze nebo fondy Stray Zbloudilá mimo normální rozsahy**.
* hello toorespond tooshort termín kolísání hello úroveň agregační výkonu fondu, **úroveň eDTU fondu je možné rozšířit nahoru nebo dolů**. Pokud dojde k této kolísání na základě pravidelných nebo předvídatelný **škálování hello fond může být naplánované toooccur automaticky**. Pokud například víte, že je úloha malého rozsahu, třeba přes noc nebo o víkendech, můžete vertikálně snížit kapacitu.
* toorespond toolonger termín kolísání nebo změny v hello počet databází, **jednotlivé databáze lze přesunout do jiných fondů**.
* termín tooshort toorespond nárůstu *jednotlivých* zatížení databáze **jednotlivé databáze může být převzaté z fondu a přiřazení úroveň výkonu jednotlivých**. Jakmile se snižuje zatížení hello, hello databáze můžete vracen toohello fondu. Když je znám předem, databáze se dají přesunout pre-emptively, že databáze hello tooensure vždy má hello prostředky, které potřebuje a tooavoid dopad na jiné databáze ve fondu hello. Pokud tento požadavek je předvídatelný, jako je místo, má expresní prodeje lístků oblíbených události, může toto chování správy integrovaná do aplikace hello.

Hello [portál Azure](https://portal.azure.com) poskytuje integrované monitorování a výstrahy na nejvíce zdrojů. Ve službě SQL Database je monitorování a upozorňování k dispozici v databázích a fondech. Toto integrované monitorování a výstrah je konkrétní prostředky, takže je vhodné toouse pro malý počet prostředků, ale není možnost je užitečná při práci s množství prostředků.

Pro rozsáhlé scénáře, kde pracujete s mnoha reources [analýzy protokolů (OMS)](sql-database-saas-tutorial-log-analytics.md) lze použít. Toto je samostatný služba Azure, která nabízí v porovnání s emitovaného diagnostické protokoly a telemetrie získané v pracovním prostoru analýzy protokolů analýzy. Analýzy protokolů můžete shromažďovat telemetrická data z mnoha služeb a být použité tooquery a nastavit výstrahy.

## <a name="get-hello-wingtip-application-source-code-and-scripts"></a>Zdrojový kód hello Wingtip aplikace a skripty

Hello Wingtip SaaS skripty a zdrojový kód aplikace jsou k dispozici v hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Kroky toodownload hello Wingtip SaaS skripty](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="provision-additional-tenants"></a>Zřízení dalších tenantů

Fondy mohou být nákladově efektivní s pouze dvěma databázemi S3, hello další databáze, které jsou v hello fondu hello cenově výhodnější stane hello průměrování vliv. Pro zajištění správného porozumění fungování monitorování a správy výkonu na škále vyžaduje tento kurz, abyste měli nasazených nejméně 20 databází.

Pokud už zřízené dávky klienty v předchozí kurzu, přeskočte toohello [simulovat využití na všechny databáze klienta](#simulate-usage-on-all-tenant-databases) části.

1. Otevřete... \\Learning moduly\\sledování výkonu a správy\\*ukázku PerformanceMonitoringAndManagement.ps1* v hello *prostředí PowerShell ISE*. Tento skript nechte otevřený, protože během tohoto kurzu budete spouštět několik scénářů.
1. Nastavte **$DemoScenario** = **1**, **Zřízení dávky tenantů**
1. Stiskněte klávesu **F5** toorun hello skriptu.

skript Hello nasadí 17 klientů v méně než pět minut.

Hello *New-TenantBatch* skript používá sadu vnořené nebo propojené [Resource Manager](../azure-resource-manager/index.md) šablony, které vytvoření dávky klientů, který ve výchozím nastavení zkopíruje databáze hello **basetenantdb** na hello katalogu serveru toocreate hello nové klienta databáze, pak zaregistruje v katalogu hello a nakonec je inicializuje hello klientovi název a místo typu. To je konzistentní způsob hello aplikace hello zřídí nového klienta. Veškeré změny provedené příliš*basetenantdb* jsou použité tooany nové klienty zřízený po tomto datu. V tématu hello [Správa schématu kurzu](sql-database-saas-tutorial-schema-management.md) toosee jak změní schéma toomake příliš*existující* klienta databáze (včetně hello *basetenantdb* databáze).

## <a name="simulate-usage-on-all-tenant-databases"></a>Simulace využití ve všech databázích tenantů

Hello *ukázku PerformanceMonitoringAndManagement.ps1* je skript zadaný, který simuluje zatížení spuštěným pro všechny databáze klienta. Hello zatížení je generována pomocí jednoho z dostupných zatížení scénářů hello:

| Ukázka | Scénář |
|:--|:--|
| 2 | Generování normální intenzity zatížení (asi 40 DTU) |
| 3 | Generování zatížení s delšími a častějšími nárůsty zatížení na databázi|
| 4 | Generování zatížení s vyššími nárůsty DTU na databázi (přibližně 80 DTU)|
| 5 | Generování normálního a vysokého zatížení v jednom tenantovi (přibližně 95 DTU)|
| 6 | Generování nevyváženého zatížení mezi více fondy|

Generátor zatížení Hello se vztahuje *syntetické* jen procesoru zatížení tooevery klienta databáze. Generátor Hello spustí úlohu pro každého klienta databáze, která volá uložené procedury pravidelně který generuje hello zatížení. mezi všechny databáze, simulaci aktivity nepředvídatelným klienta jsou nastaveny úrovně zatížení Hello (v Edtu), doba trvání a intervaly.

1. Otevřete... \\Learning moduly\\sledování výkonu a správy\\*ukázku PerformanceMonitoringAndManagement.ps1* v hello *prostředí PowerShell ISE*. Tento skript nechte otevřený, protože během tohoto kurzu budete spouštět několik scénářů.
1. Nastavit **$DemoScenario** = **2**, *generování normální intenzitou zatížení*.
1. Stiskněte klávesu **F5** tooapply tooall zatížení databáze klienta.

Adresář Wingtip je aplikace SaaS a hello reálného zatížení aplikace SaaS je obvykle ojediněle a nepředvídatelným. toosimulate to hello zatížení generátor vytváří náhodnou zatížení rozložené mezi všechny klienty. Několik minut, je potřeba pro tooemerge vzor zatížení hello, takže spustit hello zatížení generátor pro 3 až 5 minut před pokusem o toomonitor hello zatížení v následující části hello.

> [!IMPORTANT]
> Generátor zatížení Hello běží jako řadu úloh v místní relace prostředí PowerShell. Zachovat hello *ukázku PerformanceMonitoringAndManagement.ps1* otevřenou kartou! Pokud zavřete kartu hello nebo pozastavit váš počítač, generátor zatížení hello se zastaví. Generátor Hello zatížení zůstává ve *vyvolání úlohy* stavu, kde vygeneruje zatížení žádné nové klienty, které jsou zřízené po spuštění generátor hello. Použití *Ctrl-C* toostop vyvolání nové úlohy a ukončení hello skriptu. Generátor zatížení Hello bude toorun, ale jenom na stávající klienty.

## <a name="monitor-resource-usage-using-hello-azure-portal"></a>Sledování využití prostředků pomocí hello portálu Azure

využití prostředků hello toomonitor, která způsobují hello načíst aplikovány, otevřete hello portálu toohello fond, který obsahuje hello klienta databází:

1. Otevřete hello [portál Azure](https://portal.azure.com) a procházet toohello *tenants1 -&lt;uživatele&gt;*  serveru.
1. Přejděte dolů na elastické fondy a klikněte na **Pool1**. Tento fond obsahuje všechny databáze klienta hello dosud vytvořili.

Sledovat hello **elastického fondu monitorování** a **elastické databáze monitorování** grafy.

Hello využití prostředků fondu je hello agregační databáze využití pro všechny databáze ve fondu hello. Hello databáze graf znázorňuje hello pět nejprodávanějších databází:

![](./media/sql-database-saas-tutorial-performance-monitoring/pool1.png)

Vzhledem k tomu, že existují další databáze ve fondu hello nad rámec hello nejvyšší pět, využití fondu hello zobrazuje aktivity, která se neodrazí v hello nejvyšší pět databáze grafu. Další podrobnosti získáte kliknutím na **využití prostředků databáze**:

![](./media/sql-database-saas-tutorial-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-hello-pool"></a>Nastavit výstrahy výkonu u fondu hello

Nastavit výstrahy na hello fondu, která aktivuje na \>75 % využití následujícím způsobem:

1. Otevřete *Pool1* (na hello *tenants1 -\<uživatele\>*  serveru) v hello [portál Azure](https://portal.azure.com).
1. Klikněte na **Pravidla výstrah** a potom na **+ Přidat výstrahu**:

   ![přidání výstrahy](media/sql-database-saas-tutorial-performance-monitoring/add-alert.png)

1. Zadejte název, například **High DTU**,
1. Nastavte hello následující hodnoty:
   * **Metrika = procento eDTU**
   * **Podmínka = je větší než**.
   * **Prahová hodnota = 75**.
   * **Období = přes hello posledních 30 minut**.
1. Přidat e-mailovou adresu toohello *další správce email(s)* pole a klikněte na tlačítko **OK**.

   ![nastavení upozornění](media/sql-database-saas-tutorial-performance-monitoring/alert-rule.png)


## <a name="scale-up-a-busy-pool"></a>Vertikální navýšení kapacity zaneprázdněného fondu

Úroveň agregační zatížení hello zvyšuje v bodě toohello fondu, maxes out hello fondu a dosáhne využití eDTU 100 %, pak jednotlivé databáze ovlivňuje výkon, potenciálně zpomalení odezvy dotazů pro všechny databáze ve fondu hello.

**Krátkodobý**, zvažte vertikálním navýšení kapacity hello fondu tooprovide další prostředky, nebo odebráním databází z fondu hello (jejich přesunutí tooother fondy, nebo mimo hello fondu tooa samostatnou službu úroveň).

**Dlouhodobější**, zvažte optimalizaci dotazů nebo index výkonu databáze tooimprove využití. V závislosti na hello tooperformance citlivosti aplikačním vydává jeho osvědčených postupů tooscale fond si před dosažením využití eDTU 100 %. Použití výstrah toowarn můžete předem.

Můžete simulovat zaneprázdněn fondu většího zatížení hello vyprodukované generátor hello. Což hello databáze tooburst častěji a pro delší a roste hello agregační zatížení ve fondu hello beze změny hello požadavky jednotlivých databází hello. Vertikálním navýšení kapacity fondu hello se provádí snadno hello portálu nebo z prostředí PowerShell. Tento postup používá portál hello.

1. Nastavit *$DemoScenario* = **3**, _generování zatížení s delší a častější shluky na databázi_ tooincrease hello intenzitou hello agregační zatížení fond Hello beze změny zátěž ve špičce hello vyžadovanou každou databázi.
1. Stiskněte klávesu **F5** tooapply tooall zatížení databáze klienta.

1. Přejděte příliš**Pool1** v hello portálu Azure.

Monitorování hello vyšší využití eDTU fondu v horní grafu hello. Trvá několik minut hello nové vyšší zatížení tookick, ale měli byste vidět rychle začít toohit maximální využití fondu hello a jako zatížení hello steadies hello nové vzoru, rychle přetížení hello fondu.

1. Klikněte na tlačítko tooscale až hello fondu, **konfigurace fondu** hello horní části hello **Pool1** stránky.
1. Upravit hello **eDTU fondu** nastavení příliš**100**. Změna eDTU fondu hello nezmění nastavení pro jednotlivé databáze hello, (což je stále 50 eDTU max na databázi). Nastavení pro jednotlivé databáze hello uvidíte na pravé straně hello hello **konfigurace fondu** stránky.
1. Klikněte na tlačítko **Uložit** toosubmit hello požadavek tooscale hello fondu.

Přejděte zpět příliš**Pool1** > **přehled** tooview hello monitorování grafy. Sledování účinku hello hello fondu poskytování více prostředků (i když několik databází a náhodnou zatížení není vždy snadno toosee jednoznačně dokud nespustíte nějakou dobu). Když se díváte hello grafy opatřeny Pamatujte, že 100 % na horním hello teď grafu představuje 100 Edtu, při na hello nižší grafu 100 %, je stále 50 Edtu jako hello jednotlivé databáze maximální je stále 50 Edtu.

Databáze zůstat online a plně dostupné v celém procesu hello. V hello poslední chvíli jako každou databázi je připraven toobe povolit nové eDTU fondu hello žádné aktivní připojení jsou přerušená. Kód aplikace vždy budou zasílány připojení tooretry vyřadit a tak se znovu připojit databázi toohello ve vertikálním navýšením kapacity fondu hello.

## <a name="load-balance-between-pools"></a>Vyrovnávání zatížení mezi fondy

Alternativní tooscaling až hello fondu, vytvořte druhý fond a přesunutí databází do ní toobalance hello zatížení mezi hello dvou fondů. toodo, které tento nový fond hello se musí vytvořit na stejném serveru jako hello hello nejdřív.

1. V hello [portál Azure](https://portal.azure.com), otevřete hello **tenants1 -&lt;uživatele&gt;**  serveru.
1. Klikněte na tlačítko **+ nový fond** toocreate fondu na aktuální server hello.
1. Na hello **fond elastické databáze** šablony:

    1. Nastavit **název** příliš*Pool2*.
    1. Nechte hello cenová úroveň jako **standardní fond**.
    1. Klikněte na **Konfigurovat fond**
    1. Nastavit **eDTU fondu** příliš*50 eDTU*.
    1. Klikněte na tlačítko **přidat databáze** toosee seznam databází na serveru hello, který jde přidat příliš*Pool2*.
    1. Vyberte všechny toomove 10 databáze tyto toohello nový fond a pak klikněte na tlačítko **vyberte**. Pokud jste byla spuštěna hello zatížení generátor, hello služby již ví, že váš profil výkonu vyžaduje fond větší než velikost 50 eDTU hello výchozí a doporučuje od 100 nastavení eDTU.

    ![Doporučení](media/sql-database-saas-tutorial-performance-monitoring/configure-pool.png)

    1. V tomto kurzu ponechte výchozí hello 50 Edtu a klikněte na tlačítko **vyberte** znovu.
    1. Vyberte **OK** hello hello toocreate nový fond a toomove vybrané databáze do ní.

Vytvoření fondu hello a přesunutí databází hello trvá několik minut. Jak je, že zůstat online a plně dostupné až hello velmi poslední chvíli přesunu databází, okamžiku jsou uzavřeny žádné otevřené připojení. Tak dlouho, dokud máte některé logika opakovaných pokusů, klienti pak připojí toohello databáze v hello nový fond.

Procházet příliš**Pool2** (na hello *tenants1* serveru) tooopen hello fondu a sledovat jeho výkon. Pokud ho nevidíte, počkejte zřizování nový fond toocomplete hello.

Nyní uvidíte že využití prostředků na *Pool1* klesla a že *Pool2* je nyní podobně načtena.

## <a name="manage-performance-of-a-single-database"></a>Správa výkonu služby jedné databáze

Pokud jedné databáze ve fondu vyskytne dlouhodobě vysoké zatížení, v závislosti na konfiguraci fondu hello, může mívají toodominate hello prostředky ve fondu hello a mít vliv na jiné databáze. Pokud aktivita hello pravděpodobně toocontinue nějakou dobu, může hello databáze dočasně přesunout mimo hello fondu. To umožňuje hello databáze toohave hello další prostředky, které potřebuje a izoluje od hello jiné databáze.

Toto cvičení simuluje hello účinku Hall vzájemné součinnosti Contoso zaznamenat vysokého zatížení při lístků, přejděte na prodej pro oblíbené vzájemné součinnosti.

1. Otevřete hello... \\ *Ukázku PerformanceMonitoringAndManagement.ps1* skriptu.
1. Nastavte **$DemoScenario = 5, Generování normálního a vysokého zatížení v jednom tenantovi (přibližně 95 DTU).**
1. Nastavte **$SingleTenantDatabaseName = contosoconcerthall**
1. Spuštění pomocí skriptu hello **F5**.


1. V hello [portál Azure](https://portal.azure.com) otevřete **Pool1**.
1. Zkontrolujte hello **elastického fondu monitorování** grafu a vyhledejte hello vyšší využití eDTU fondu. Po minutu nebo dvě hello vyšší zatížení by se měl spustit tookick v a rychle byste měli vidět, že fond hello dotkne využití 100 %.
1. Zkontrolujte hello **elastické databáze monitorování** zobrazení, která zobrazuje hello nejprodávanějších databází v hello poslední hodinu. Hello *contosoconcerthall* databáze mají brzy zobrazit jako jeden z pěti hello nejprodávanějších databáze.
1. **Klikněte na monitorování elastické databáze hello** **grafu** a otevře se hello **využití prostředků databáze** stránky, kde můžete sledovat některé z databází hello. Díky tomu můžete izolovat hello zobrazení pro hello *contosoconcerthall* databáze.
1. Ze seznamu hello databáze, klikněte na tlačítko **contosoconcerthall**.
1. Klikněte na tlačítko **cenová úroveň (škálování Dtu)** tooopen hello **konfigurace výkonu** stránky, kde můžete nastavit úroveň samostatné výkonu pro databázi hello.
1. Klikněte na hello **standardní** kartě Možnosti škálování hello tooopen ve standardní vrstvě hello.
1. Vysuňte hello **DTU posuvníku** tooright tooselect **100** Dtu. Poznámka: odpovídá cíl služby toohello, **S3**.
1. Klikněte na tlačítko **použít** toomove hello databáze mimo hello fondu a nastavte jej *úrovně Standard S3* databáze.
1. Škálování po dokončení, monitorování hello vliv na hello contosoconcerthall databáze a Pool1 v elastickém fondu a databáze oknech hello.

Jakmile subvence hello vysokým zatížením hello contosoconcerthall databáze by měl neprodleně vrátí toohello fondu tooreduce jeho náklady. Pokud je při tom nejasné po který se stane, můžete nastavit upozornění na hello databáze, se aktivuje při jeho využití v jednotkách DTU klesne pod hello jednotlivé databáze ve fondu hello maximální. Přesunutí databáze do fondu je popsáno v cvičení 5.

## <a name="other-performance-management-patterns"></a>Další vzorce správy výkonu

**Preemptivní škálování** v výše uvedeného cvičení hello, kde jste prozkoumali jak tooscale izolované databáze, budete vědět, které toolook databáze pro. Pokud hello správu Contoso vzájemné součinnosti Hall měl informován Wingtips hello brzké prodej lístků, databáze hello přesunuta mimo hello fondu pre-emptively. Jinak by pravděpodobně mít vyžadovala výstrahu na fond hello nebo hello databáze toospot co se děje. Chcete nebude toolearn informace z hello jiných klientů v hello fondu nesouhlasících z snížení výkonu. A pokud hello klienta můžete odhadnout, jak dlouho budou potřebovat další prostředky, můžete nastavení databáze Azure Automation runbook toomove hello mimo hello fondu a pak znovu zpět na definovaný plán.

**Samoobslužná služba klienta škálování** škálování je úloha snadno vytvořit prostřednictvím rozhraní API pro správu hello, a proto snadno vytvářet hello možnost databáze klienta tooscale do své aplikace na straně klienta a nabízet ho jako součást služby SaaS. Například umožnit klientům self-administer škálování nahoru a dolů, případně přímo spojeny fakturace tootheir!

**Škálování fondu nahoru a dolů na vzorce používání toomatch plán**

Použití agregační klienta odpovídá vzorce používání předvídatelné, můžete použít Azure Automation tooscale fond nahoru a dolů podle plánu. Můžete například snížit kapacitu fondu po 6. hodině večer a navýšit před 6. hodinou ráno ve dnech, kdy víte, že existuje pokles požadavků na prostředky.



## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Simulovat využití v databázích hello klienta spuštěním generátor zadaný zatížení
> * Databáze monitorování hello klienta jako jejich reagovat toohello zvýšení zatížení
> * Škálování hello elastického fondu v odpovědi toohello databáze zvýšené zatížení
> * Zřídit druhý elastický fond tooload vyrovnávání hello databáze aktivitu

[Kurz Obnovení jednoho tenanta](sql-database-saas-tutorial-restore-single-tenant.md)


## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které stavějí hello nasazení Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastické fondy SQL](sql-database-elastic-pool.md)
* [Azure Automation](../automation/automation-intro.md)
* [Log Analytics](sql-database-saas-tutorial-log-analytics.md) – kurz Nastavení a používání Log Analytics
