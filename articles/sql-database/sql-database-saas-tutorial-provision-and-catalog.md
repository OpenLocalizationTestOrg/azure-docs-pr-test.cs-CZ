---
title: "nové klienty aaaProvision ve víceklientské aplikaci, která používá Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak tooprovision a katalog nové klienty ve hello Wingtip SaaS aplikace"
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
ms.date: 08/11/2017
ms.author: sstein
ms.openlocfilehash: eb26f523305650c2124e36707d187dfcdad06fcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provision-new-tenants-and-register-them-in-hello-catalog"></a>Zřídit nové klienty a zaregistrujte je v katalogu hello

V tomto kurzu můžete další informace o zřizování hello a vzory SaaS katalogu a jak jsou implementované v hello Wingtip SaaS aplikace. Vytvoření a inicializace nové databáze klienta a zaregistrujte je v katalogu aplikací hello klienta. katalog Hello je databáze, která udržuje hello mapování mezi aplikace SaaS hello velký počet klientů a jejich data. katalog Hello hraje důležitou roli odkazovat aplikace požadavky toohello správné databázi.  

V tomto kurzu se naučíte:

> [!div class="checklist"]

> * Zřídit jednoho nového klienta, včetně procházení jak tato možnost je implementovaná
> * Zřídit dávku dalších tenantů.


toocomplete dokončení tohoto kurzu, ujistěte se, hello následující požadavky:

* Hello Wingtip SaaS aplikace je nasazená. toodeploy za méně než pět minut, najdete v části [nasazení a seznamte se s hello Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-toohello-saas-catalog-pattern"></a>Úvod toohello vzor katalogu SaaS

V databázi zálohovanou víceklientské aplikaci SaaS je důležité tooknow se uloží informace pro každého klienta. V katalogu vzoru hello SaaS databáze katalogu je použité toohold hello mapování mezi každého klienta a kde jsou uchovávána data. aplikace Wingtip SaaS Hello používá jednoho klienta za architektura databáze, ale základní vzor hello ukládání mapování klienta do databáze v katalogu platí, jestli se používá databázi víceklientské nebo jednoho klienta.

Každý klient je přiřazen klíč, který označuje je v katalogu hello a který je namapovaný toohello umístění hello příslušné databáze. V aplikaci Wingtip SaaS hello hello klíč je vytvořen z hodnota hash název klienta hello. Díky tomu, že část názvu klienta hello toobe adresu URL aplikace hello používá klíč tooconstruct hello. Může použít jiná schémata klíče klienta.  

katalog Hello umožňuje hello název nebo umístění hello databáze toobe změnit s minimálním dopadem na aplikace hello.  V databázi víceklientského modelu to rovněž umožňuje, 'Přesun' klienta mezi databázemi.  katalog Hello může být také použít tooindicate zda klienta nebo databáze je offline z důvodu údržby nebo jiné akce. To je prozkoumali v hello [obnovení jednoho klienta kurzu](sql-database-saas-tutorial-restore-single-tenant.md).

Kromě toho hello katalogu, který je v platnosti databáze správy pro aplikace SaaS, můžete ukládat další metadata klienta nebo databáze, jako je například hello vrstvy nebo edici databáze, verzi schématu, plán služeb nebo SLA nabízí tootenants a další informace Umožňuje správu aplikací, na zákaznickou podporu nebo devops procesy.  

Nad rámec hello aplikace SaaS můžete povolit hello katalogu Nástroje databáze.  V ukázce Wingtip SaaS hello hello katalog je použité tooenable mezi klienta dotaz, prozkoumali v hello [ad hoc analytics kurzu](sql-database-saas-tutorial-adhoc-analytics.md). Správa úloh mezidatabázové je prozkoumali v hello [Správa schématu](sql-database-saas-tutorial-schema-management.md) a [klienta analytics](sql-database-saas-tutorial-tenant-analytics.md) kurzy. 

V aplikaci Wingtip SaaS hello hello katalogu je implementovaná pomocí funkce pro správu horizontálního oddílu hello hello [elastické databáze klienta knihovny (EDCL)](sql-database-elastic-database-client-library.md). Hello EDCL umožňuje toocreate aplikace, spravovat a použít mapování horizontálních databáze zálohována. Mapování horizontálních obsahuje seznam horizontálních oddílů (databáze) a hello mapování mezi klíče (klienty) a databázemi.  Funkce EDCL během zřizování toocreate hello položky v mapě horizontálního oddílu hello klienta lze z aplikace nebo skripty prostředí PowerShell a z aplikace tooefficiently připojit toohello správné databázi. EDCL ukládá do mezipaměti databáze katalogu toohello hello připojení informace toominimize provozu a zrychlit aplikace hello.  

> [!IMPORTANT]
> Hello mapování dat je přístupný v databázi hello katalogu, ale *nemáte upravit*! K jejich úpravě používejte jen rozhraní API klientské knihovny Elastic Database. Přímá manipulace s hello mapování dat rizika poškozování hello katalogu a není podporována.


## <a name="introduction-toohello-saas-provisioning-pattern"></a>Vzor SaaS zřizování toohello Úvod

Při připojování nového klienta v aplikaci SaaS, která používá model databáze jednoho klienta novou databázi klienta musí být zřízená.  Musí být vytvořený v příslušné umístění hello a vrstvy služeb, inicializován s odpovídající schématu a referenčních dat a pak zaregistrována v katalogu hello pod klíčem hello odpovídající klienta.  

Různý přístup se dá použít toodatabase zřizování, který by mohl zahrnovat provádění skripty SQL, nasazení souboru bacpac nebo kopírování databáze "zlatá" šablony.  

Hello zřizování přístupů, které používáte musí comprehended v strategie pro správy celkové schéma, které musíte zajistit, že nové databáze zřízený s nejnovější schématu hello.  To je prozkoumali v hello [schématu správu kurzu](sql-database-saas-tutorial-schema-management.md).  

Hello Wingtip SaaS aplikace zřizuje nové klienty tak, že zkopírujete zlaté databáze s názvem basetenantdb, nasazeny na server hello katalogu.  Zřizování může být integrovaná do aplikace hello jako součást registrace prostředí, a podporované v režimu offline pomocí skriptů. V tomto kurzu bude prozkoumejte zřizování pomocí prostředí PowerShell. skripty pro zřizování Hello zkopírujte hello basetenantdb toocreate novou databázi klienta v elastickém fondu, pak provést jeho inicializaci s informacemi o konkrétního klienta a zaregistrovat ji hello katalogu horizontálního oddílu mapy.  V hello ukázkové aplikace, databáze mají názvy na základě názvu hello klienta, ale nejedná se o důležitou součástí hello vzor – hello použití katalogu hello umožňuje všechny databáze toohello toobe přiřazen název. + 


## <a name="get-hello-wingtip-application-scripts"></a>Získat hello Wingtip aplikační skripty

Hello Wingtip SaaS skripty a zdrojový kód aplikace jsou k dispozici v hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Kroky toodownload hello Wingtip SaaS skripty](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).


## <a name="provision-and-catalog-detailed-walkthrough"></a>Podrobný postup zřizování a přidání tenantů do katalogu

toounderstand jak hello Wingtip aplikace implementuje nového klienta zřizování, přidejte zarážku a krok v postupu hello při zřizování klienta:

1. Otevřete... \\Learning moduly\\ProvisionAndCatalog\\_ukázku ProvisionAndCatalog.ps1_ a sadu hello následující parametry:
   * **$TenantName** = hello název nové místo hello (například *Bushwillow modré*).
   * **$VenueType** = jeden z typů předem definované místo hello: *modré*, classicalmusic, tance, jazz, judo, motorracing víceúčelových, opera, rockmusic, fotbalový.
   * **$DemoScenario** = **1**, nastavte příliš**1** příliš*zřídit jednoho klienta*.

1. Přidejte zarážku kdekoli umístěním kurzor na řádek 48, hello řádku s upozorněním: *nového klienta,*a stiskněte klávesu **F9**.

   ![přerušení](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. stiskněte klávesu skriptu hello toorun **F5**.

1. Po spuštění skriptu hello zastaví u hello zarážky, stiskněte klávesu **F11** toostep do kódu hello.

   ![přerušení](media/sql-database-saas-tutorial-provision-and-catalog/debug.png)



Trasování provádění skriptu hello pomocí hello **ladění** možnosti nabídky - **F10** a **F11** toostep přes nebo do hello volat funkce. Další informace o ladění skriptů prostředí PowerShell najdete v tématu [tipy k ladění skriptů prostředí PowerShell a práce s](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Následující Hello nejsou tooexplicitly postupujte podle kroků, ale vysvětlení hello pracovního postupu, které jednotlivé kroky při ladění skriptu hello:

1. **Import hello SubscriptionManagement.psm1** modul, který obsahuje funkce pro přihlášení tooAzure a výběr hello pracujete s předplatným Azure.
1. **Import hello CatalogAndDatabaseManagement.psm1** modul, který nabízí v porovnání hello katalogu a klienta úrovni abstrakce [horizontálního oddílu správu](sql-database-elastic-scale-shard-map-management.md) funkce. To je důležité modul, který zapouzdřuje většinu vzor hello katalogu a je vhodné využít.
1. **Získejte podrobnosti o konfiguraci**. Krok do Get-konfigurace (s F11), abyste viděli, jak je zadána konfigurace aplikace hello. Názvy prostředků a jiné hodnoty, konkrétní aplikace jsou zde definované, ale neměňte některou z těchto hodnot dokud jste se seznámili s hello skripty.
1. **Získejte objekt katalogu hello**. Krok do Get-katalogu, která vytvoří a vrátí objekt katalog, který se používá ve skriptu hello vyšší úrovně.  Tato funkce využívá funkce správy horizontálního oddílu, které jsou importovány z **AzureShardManagement.psm1**. objekt katalogu Hello se skládá z následujících hello:
   * $catalogServerFullyQualifiedName je vytvořený pomocí standardní stem hello plus svoje uživatelské jméno: _katalogu -\<uživatele\>. database.windows.net_.
   * $catalogDatabaseName se načítají z konfigurace hello: *tenantcatalog*.
   * objekt $shardMapManager je inicializovat z databáze katalogu hello.
   * objekt $shardMap je inicializován ze hello *tenantcatalog* horizontálního oddílu mapy ve databáze katalogu hello.
   Objekt katalogu je sestavit a vrátí a použít ve skriptu hello vyšší úrovně.
1. **Vypočítat nový klíč klienta hello**. Funkce hash je klíč tenanta použít toocreate hello od hello název klienta.
1. **Zkontrolujte, jestli už existuje klíč tenanta hello**. Hello katalogu se kontroluje, že je k dispozici tooensure hello klíč.
1. **databáze klienta Hello je opatřen TenantDatabase nový.** Použití **F11** toostep v a v tématu Jak hello databáze je zřízený pomocí [šablony Azure Resource Manageru](../azure-resource-manager/resource-manager-template-walkthrough.md).

Název databáze Hello se vytvářejí na základě hello klienta název toomake ji vymazat které horizontálního oddílu patří toowhich klienta. (Další strategie pro pojmenování databáze by mohl snadno použít.) + Šablonu Resource Manager je použité toocreate databázi klienta zkopírováním zlaté databáze (baseTenantDB) na serveru katalogu hello. Alternativní způsob může být toocreate prázdnou databázi a poté provést jeho inicializaci importováním souboru bacpac nebo tooexecute inicializaci skriptu z dobře známé umístění.  

šablony Resource Manageru Hello je ve složce Modules\Common\ ...\Learning hello: *tenantdatabasecopytemplate.json*

Po vytvoření databáze klienta hello je pak další **inicializovat hello místo (klient) názvem a typem místo hello**. Zde také můžete provést další inicializaci.

Hello **databáze klienta je zaregistrován v katalogu hello** s *přidat TenantDatabaseToCatalog* pomocí klíče tenanta hello. Použití **F11** toostep do hello podrobnosti:

* databáze katalogu Hello se přidá toohello horizontálního oddílu mapy (hello seznam známých databází).
* mapování tohoto odkazy hello hodnota klíče toohello horizontálních Hello je vytvořen.
* Další metadata (název místo hello) o hello klienta se přidá toohello klienty tabulky v katalogu hello.  Hello klientům tabulky není součástí hello ShardManagement schématu a není nainstalovaná v rámci hello EDCL.  Tato tabulka ukazuje, jak databáze katalogu hello je možné rozšířit toosupport další data specifické pro aplikaci.   


Jakmile bude zřizování dokončeno, provádění vrátí toohello původní *ukázku ProvisionAndCatalog* skript, který otevře hello **události** stránku hello nového klienta v prohlížeči hello:

   ![stránka events](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Zřídit dávky klientů

Toto cvičení zřídí dávky 17 klientů. Doporučuje se, že zřídit tuto dávku klienty před zahájením dalších kurzech Wingtip SaaS, je více než několik databází toowork s.

1. Otevřete... \\Learning moduly\\ProvisionAndCatalog\\*ukázku ProvisionAndCatalog.ps1* v hello *prostředí PowerShell ISE* a změňte hello *$ DemoScenario* too3 parametr:
   * **$DemoScenario** = **3**, změňte příliš**3** příliš*zřídit dávky klienty*.
1. Stiskněte klávesu **F5** a spusťte skript hello.

skript Hello nasadí dávky další klienty. Používá [šablony Azure Resource Manageru](../azure-resource-manager/resource-manager-template-walkthrough.md) , řídí hello batch a potom deleguje zřizování každé šabloně propojené tooa databáze. Pomocí šablon tímto způsobem umožňuje Azure Resource Manager toobroker hello zřizování vašeho skriptu. Šablony zřídit databáze paralelně, kde můžete a zpracovává opakování v případě potřeby optimalizace hello celkové procesů. Hello skriptu je idempotent, pokud selže nebo zastaví z jakéhokoli důvodu potom ho spusťte znovu.

### <a name="verify-hello-batch-of-tenants-successfully-deployed"></a>Ověřte dávku hello klientům byla úspěšně zavedena

* Otevřete hello *tenants1* serveru procházením tooyour seznamu serverů v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **databází SQL**a zkontrolujte hello dávku 17 další databáze, jsou nyní v seznamu hello:

   ![seznam databází](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Další způsoby zřizování

K dalším způsobům zřizování, které nejsou zahrnuty do tohoto kurzu, patří:

**Předběžné zřizování databází.** Hello předem zřizování vzor zneužije hello skutečnost, že databáze v elastickém fondu nepřidávejte zpoplatněné. Fakturuje se pro elastický fond hello, není hello databází a nečinnosti databáze využívat žádné prostředky. Předem zřizování databází ve fondu a přidělování je podle potřeby, může výrazně snížit čas registrace klienta. Hello počet databází předem zřizovat může upravit podle potřeby tookeep do vyrovnávací paměti, který je vhodný pro hello očekává zřizování rychlost.

**Automatické zřizování.** Ve vzoru automatické zřizování hello vyhrazené zřizování služby je použité tooprovision servery, fondy a databáze automaticky podle potřeby – včetně předem zřizování databází v elastické fondy, v případě potřeby. A pokud jsou databáze zrušte uvedena do provozu a odstranit, mezery v elastické fondy může být vyplněny hello zřizování služby podle potřeby. Tato služba může být jednoduché nebo komplexní – například zpracování zřizování napříč několika zeměpisných oblastí a může nastavit geografická replikace automaticky pokud této strategie se používá pro zotavení po havárii. Pomocí vzoru hello automatické zřizování klientská aplikace nebo skriptu by odeslání zřizování požadavek tooa fronty toobe zpracovává hello zřizování služby a by pak dotazování hello služby toodetermine dokončení. Pokud se používá předem zřizování, by požadavky zpracovávány službou hello Správa zřizování nahrazení databáze spuštěné v pozadí hello rychle.



## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Zřídit jednoho nového tenanta.
> * Zřídit dávku dalších tenantů.
> * Krokovat s vnořením hello podrobnosti o zřizování klientů a jejich registrace do katalogu hello

Zkuste hello [kurzu monitorování výkonu](sql-database-saas-tutorial-performance-monitoring.md).

## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které stavějí hello Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Klientská knihovna Elastic Database](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [Jak tooDebug skriptů v systému Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
