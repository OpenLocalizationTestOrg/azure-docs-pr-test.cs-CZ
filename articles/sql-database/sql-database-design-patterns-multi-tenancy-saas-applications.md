---
title: "aaaDesign vzory pro víceklientské aplikace SaaS a Azure SQL Database | Microsoft Docs"
description: "Tento článek popisuje požadavky na hello a běžných vzorů architektura dat databáze víceklientské aplikací běžících v cloudovém prostředí potřebovat tooconsider a hello různé kompromisy přidružené tyto vzory. Také vysvětluje, jak Azure SQL Database, s jeho elastické fondy a elastické nástroje vám budou snadněji řešit tyto požadavky způsobem ucelená."
keywords: 
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 1dd20c6b-ddbb-40ef-ad34-609d398d008a
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-design
ms.date: 02/01/2017
ms.author: srinia
ms.openlocfilehash: a4b58935b08cb78792e65a675d680de708b709fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-patterns-for-multi-tenant-saas-applications-and-azure-sql-database"></a>Vzory pro víceklientské aplikace SaaS a Azure SQL Database návrhu
V tomto článku se dozvíte hello požadavky a běžných vzorů architektura dat víceklientské softwaru jako databáze aplikace služby (SaaS), které běží v prostředí cloudu. Také vysvětluje hello faktory potřebujete tooconsider a hello kompromis vzory návrhu jiný. Elastické fondy a elastické nástroje ve službě Azure SQL Database vám může pomoct podle konkrétních požadavků bez kompromisů jiné cíle.

Vývojář podá někdy volby, které pracují s jejich zájmů dlouhodobé nejlepší při návrhu klientů modely pro hello datové vrstvy víceklientské aplikací. Na začátku minimálně vývojář může vnímat usnadnění vývoje a nižší náklady na poskytovatele cloudové služby jako důležitější než izolace nebo hello škálovatelnosti klientů aplikace. Tato volba může způsobit toocustomer spokojenost otázky a nákladná oprava kurzu později.

Hello stejné nastavení služby toohundreds nebo tisíce klienty, kteří sdílet nebo vidět uživatele toho druhého data, která poskytuje víceklientské aplikace je aplikace hostované v cloudovém prostředí. Příkladem je aplikace SaaS, která poskytuje služby tootenants v prostředí hostovaných v cloudu.

## <a name="multi-tenant-applications"></a>Víceklientským aplikacím
V víceklientským aplikacím datům a zatížení můžete snadno rozdělit na oddíly. Vám může oddílu data a úlohy, například podél hranice klienta, protože většina požadavků se provádějí v rámci hello rámec klienta. Tuto vlastnost je vlastní hello dat a hello zatížení a jeho upřednostňuje vzory aplikace hello popsané v tomto článku.

Vývojáři použít tento typ aplikace napříč celou spektrum hello cloudové aplikace, včetně:

* Databázové aplikace partnera, které se kvůli toohello cloud jako aplikace SaaS
* Aplikace SaaS, které jsou vytvořené pro hello cloudu hello pozadí
* Přímé, zákazník směřujících aplikací
* Zaměstnanecké podnikové aplikace

Aplikace SaaS, které jsou navrženy pro hello cloudu a ty s kořeny jako partner databázové aplikace jsou obvykle víceklientským aplikacím. Tyto aplikace SaaS poskytovat specializované softwarová aplikace jako klienty tootheir služby. Klienty můžete získat přístup k službě aplikace hello a mít úplná vlastnictví přidružená data uložená v rámci aplikace hello. Ale tootake výhod hello výhod SaaS, klienti musí předat některé kontrolu nad svá vlastní data. Důvěřují tookeep poskytovatele služby SaaS hello jejich data zabezpečená a izolované od ostatních klientů data. Příkladem tento druh víceklientské aplikace SaaS jsou MYOB, SnelStart a Salesforce.com. Každá z těchto aplikací můžou být dělené podél hranic klienta a podpora vzorů návrhu aplikace v hello, které v tomto článku probereme.

Aplikace, které poskytují přímé služby toocustomers nebo tooemployees v rámci organizace (často označují tooas uživatele, nikoli klientů) jsou jiné kategorii na spektrum víceklientské aplikace hello. Zákazníci přihlášení k odběru služby toohello a nejste vlastníkem hello data, která hello poskytovatele služeb, shromažďuje a ukládá. Poskytovatelé služeb mají méně přísná tookeep požadavky zákazníků data izolované od sebe navzájem nad rámec jakákoli ochranu osobních údajů. Tento druh aplikaci zaměřené zákazníka víceklientské příklady poskytovatelů obsahu média jako Netflix, Spotify a Xbox LIVE. Další příklady snadno rozdělený aplikací jsou zákazníkem, internetových aplikací nebo aplikace Internet věcí (IoT) v které každého zákazníka či zařízení může sloužit jako oddílu. Hranice oddílů můžete oddělit uživatelů a zařízení.

Ne všechny aplikace oddílu snadno podél jedné vlastnosti, například klienta, zákazníka, uživatele nebo zařízení. Komplexní enterprise prostředek plánování (ERP) aplikace, například má produktů, objednávek a zákazníků. Má obvykle komplexní schéma s tisíci vysoce vzájemně propojené tabulky.

Žádná strategie jednoho oddílu můžete použít tooall tabulky a fungovat na všech úloh aplikace. Tento článek se zaměřuje na více klientů aplikace, které mají snadno rozdělený data a úlohy.

## <a name="multi-tenant-application-design-trade-offs"></a>Kompromis víceklientské aplikace návrhu
Hello návrhový vzor, který obvykle vybírá vývojář víceklientské aplikace je založena na zvážení hello následující faktory:

* **Izolaci klientů**. Hello vývojáře musí tooensure zda žádný klient má nežádoucí přístup tooother klientů data. požadavek izolace Hello rozšiřuje tooother vlastnosti, jako je například poskytování ochrany z aktivní Sousedé BGP, je možné toorestore dat klienta a implementace vlastního nastavení konkrétního klienta.
* **Cloudové náklady prostředků**. Aplikace SaaS potřebuje toobe cenově konkurenceschopným. Vývojář aplikace více klientů může zvolte toooptimize nižší náklady v hello použití prostředků cloudu, jako je například úložiště a výpočetní náklady.
* **Snadné DevOps**. Vývojář víceklientské aplikace potřebuje tooincorporate izolace ochranu, údržbě a sledování stavu hello jejich aplikace a schématu databáze a řešení problémů klienta. Složitostí při vývoji aplikací a operaci překládá přímo tooincreased náklady a vyzve s spokojenost klienta.
* **Škálovatelnost**. možnost tooincrementally Hello přidat další klienty a kapacity pro klienty, kteří vyžadují, že je nutné tooa úspěšná operace SaaS.

Každý z těchto faktorů má tooanother kompromis porovnání. Nabídka nemusí nabízet hello nejpohodlnější vývojového prostředí cloudu nižší cenu Hello. Je důležité pro vývojář toomake volby informován o těchto možnostech a jejich kompromis během procesu návrhu aplikace hello.

Vzor oblíbených vývoj je toopack několik klientů do jednoho nebo několika databází. Hello výhody tohoto přístupu jsou nižší náklady, protože platíte za několik databází a relativní jednoduchost hello pracovat s omezený počet databází. Ale v čase, vývojář víceklientské aplikace SaaS bude Uvědomte si, že tato volba má významné downsides v izolaci mezi klienty a škálovatelnost. Důležité přestane být izolaci klientů, je další úsilí požadované tooprotect klienta data ve sdíleném úložišti před neoprávněným přístupem nebo aktivní okolí. Tato další úsilí může výrazně zvýšit úsilí vývoj a náklady na údržbu izolace. Podobně pokud přidávání klientů je potřeba, tento vzor návrhu obvykle vyžaduje znalosti tooredistribute klienta dat mezi databází tooproperly škálování hello datové vrstvy aplikace.  

Často izolaci klientů je základní požadavek ve víceklientské aplikace SaaS, které nahrazovat toobusinesses a organizace. Vývojář může zvážit podle dosahovaný výhody v jednoduchost a náklady na přes izolaci mezi klienty a škálovatelnost. Tato změna potvrdit složité a nákladné jako služba hello zvětšování a požadavky na izolaci klientů se více důležité a spravovaných v hello aplikační vrstvu. V aplikacích více klientů, které poskytují služby přímé, určených toocustomers, ale izolaci klientů může být s nižší prioritou než optimalizace nákladů cloudu prostředku.

## <a name="multi-tenant-data-models"></a>Víceklientské datové modely
Běžné postupy pro návrhový pro umístění dat klienta podle tři odlišné modely na obrázku 1.

![datové modely víceklientské aplikace](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)

Obrázek 1: Běžné postupy pro návrhový pro více klientů datové modely

* **Databáze za klienta**. Každý klient má svou vlastní databázi. Všechna data konkrétního klienta je databáze uzavřeného toohello klienta a jsou izolovány od ostatních klientů a jejich data.
* **Sdílené horizontálně dělené databáze**. Několik klientů sdílet mezi více databází. Odlišnou sadu klientů je přiřazen tooeach databáze pomocí strategie dělení například hash, rozsah nebo seznamu dělení. Tato strategie distribuční data je často označují tooas horizontálního dělení.
* **Sdílené databáze jedním**. Jedinou, někdy velký, databázi obsahuje data pro všechny klienty, které jsou jednoznačně rozlišit ve sloupci ID klienta.

> [!NOTE]
> Vývojář aplikace může zvolit tooplace jiných klientů v jiné databázi schémata a potom pomocí hello schématu název toodisambiguate hello různými klienty. Vzhledem k tomu obvykle vyžaduje použití hello dynamické SQL a nemůže být efektivní při ukládání do mezipaměti plánu nedoporučujeme tento přístup. V hello zbývající část tohoto článku zaměříme na hello sdíleného přístupu tabulky pro tuto kategorii víceklientské aplikace.
> 
> 

## <a name="popular-multi-tenant-data-models"></a>Oblíbené víceklientské datové modely
Je důležité tooevaluate hello různé typy víceklientské datové modely z hlediska hello aplikace návrhu kompromisy, které jsme jste určili. Tyto faktory pomoci charakterizovat hello tři nejběžnější víceklientské datové modely popsané výše a jejich využití databáze, jak je znázorněno na obrázku 2.

* **Izolace.** Hello stupeň izolaci mezi klienty lze míru izolace kolik klientů lze dosáhnout datový model.
* **Cloudové náklady prostředků**. Hello množství sdílení prostředků mezi klienty můžete optimalizovat náklady prostředků cloudu. Prostředek může být definován jako hello výpočetního prostředí a úložiště náklady.
* **Náklady na DevOps**. Hello snadný vývoj aplikací, nasazení a správy snižuje celkové náklady na operaci SaaS.  

Obrázek 2 ukazuje osy hello Y hello úroveň izolace klienta. ukazuje osy Hello X hello úroveň sdílení prostředků. Šedá Hello diagonálních šipku uprostřed hello označuje směr hello náklady DevOps, které sledují tooincrease nebo snížení.

![Vzory návrhu oblíbených víceklientské aplikace](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png)

Obrázek 2: Oblíbených víceklientské datové modely

Hello pravém dolním kvadrantu obrázek 2 ukazuje aplikaci vzor, který používá potenciálně velkého sdílené jedné databáze a hello sdílet tabulky (nebo samostatné schéma) přístup. Je vhodné pro sdílení, protože všichni klienti používají hello stejné databáze prostředků (procesor, paměť, vstup/výstup) v jediné databázi prostředků. Ale izolaci klientů je omezená. Může být nutné tootake klienty další kroky tooprotect od sebe navzájem v hello aplikační vrstvu. Tyto další kroky může výrazně zvýšit hello DevOps náklady na vývoj a správu aplikace hello. Škálovatelnost je omezena hello škálování hello hardwaru, který je hostitelem databáze hello.

Hello dolním kvadrantu obrázek 2 ukazuje několik klientů horizontálně dělené napříč více databází (obvykle jiný hardware jednotek škálování). Každou databázi hostuje podmnožinu klientů, které nemají starosti hello škálovatelnost jiných vzorů. Pokud další kapacity se vyžaduje pro více klientů, můžete snadno umístit hello klientům na nové databáze přidělené jednotky škálování toonew hardwaru. Ale tím se snižuje množství hello sdílení prostředků. Pouze klienti umístit na hello stejné jednotek škálování sdílet prostředky. Tento přístup poskytuje málo zlepšování tootenant izolace, protože velký počet klientů jsou stále společně umístěná, bez automaticky chráněn z druhé strany akcí. Složitost aplikace zůstává vysoké.

Hello levém kvadrantu na obrázku 2 je třetí přístup hello. Každý klient data umístí do svou vlastní databázi. Tento přístup má vlastnosti dobrý izolaci klientů, ale omezuje sdílení prostředků při každé databáze má svou vlastní vyhrazených prostředcích. Tento přístup je vhodný, pokud všichni klienti předvídatelný úlohy. Pokud klientské úlohy se méně předvídatelný, nejde optimalizovat hello zprostředkovatele sdílení prostředků. Nepředvídatelnost je běžné pro SaaS aplikace. Zprostředkovatel Hello musí buď přepsání zřídit toomeet požadavky nebo nižší prostředky. Náklady na vyšší nebo nižší spokojenost klienta buď výsledky akce. Vyšší stupeň sdílení prostředků mezi klienty stane žádoucí toomake hello řešení cenově výhodnější. Se zvyšující číslo hello databáze také zvyšuje náklady toodeploy DevOps a udržovat aplikace hello. Bez ohledu na tyto otázky tato metoda poskytuje nejlepší a nejjednodušší izolace hello klientům.

Tyto faktory ovlivňují také hello návrhový vzor, který vybere zákazníka:

* **Vlastnictví dat klienta**. Aplikace, ve kterém klienti zachovat vlastnictví svá vlastní data upřednostňuje hello vzor služby jedné databáze každého klienta.
* **Škálování**. Aplikace, která cílí stovky tisíc nebo miliony klienty upřednostňuje přístupy například horizontálního dělení sdílení databází. Požadavky na izolaci stále může představovat problémy.
* **Hodnota a obchodní model**. Pokud aplikace výnosy za klienta Pokud malá (méně než dolar), požadavky na izolaci stát méně kritický a sdílenou databázi, má smysl. Pokud za klienta výnos několik Kč, více možné je model databáze za klienta. Může pomoci snížit náklady na vývoj.

S ohledem hello návrhu kompromis znázorněno na obrázku 2, ideální víceklientského modelu musí vlastnosti izolace tooincorporate funkčním klientovi s sdílení mezi klienty optimální prostředků. Tento model se vejde hello kategorie popsané v pravém horním kvadrantu hello obrázku 2.

## <a name="multi-tenancy-support-in-azure-sql-database"></a>Podpora více klientů ve službě Azure SQL Database
Azure SQL Database podporuje všechny vzorky víceklientské aplikace, které jsou uvedené na obrázku 2. S elastické fondy podporuje také application vzor, který kombinuje sdílení dobrý prostředků a výhody izolace hello hello databáze za-klienta přístupu (viz hello pravém kvadrantu na obrázku 3). Elastické databáze nástroje a možnosti služby SQL Database pomoct snížit náklady na toodevelop hello a fungují aplikace, která má mnoho databází (zobrazené v oblasti hello stínované na obrázku 3). Tyto nástroje můžete vytvářet a spravovat aplikace, které používají některá z hello více databáze vzory.

![Vzorce v databázi Azure SQL](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png)

Obrázek 3: Vzory víceklientské aplikace ve službě Azure SQL Database

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Model databáze na klienta s elastické fondy a nástroje
Elastické fondy v databázi SQL kombinovat s sdílení mezi klienta databází toobetter podporu hello databáze za klienta přístup prostředků izolaci klientů. Databáze SQL je dat vrstvy řešení SaaS poskytovatelů, kteří sestavení víceklientským aplikacím. Posune Hello zatížení prostředku sdílení mezi klienty z hello aplikační vrstvy toohello databáze služby vrstvu. elastické úlohy, elastické dotazu, elastické transakce a klientské knihovny pro elastické databáze hello je zjednodušený Hello složitost správy a dotazování na škálování mezi databázemi.

| Požadavky na aplikace | Možnosti databáze SQL |
| --- | --- |
| Izolaci mezi klienty a sdílení prostředků |[Elastické fondy](sql-database-elastic-pool.md): přidělte fond prostředků, databáze SQL a sdílení prostředků hello mezi různými databázemi. Navíc jednotlivé databáze můžete kreslení tolik prostředky z fondu hello jako potřebné tooaccommodate kapacity vyžádání špičky kvůli toochanges v klientské úlohy. Hello elastického fondu, samotné můžete podle potřeby škálovat nahoru nebo dolů. Elastické fondy také poskytují snadné správy a monitorování a řešení potíží na úrovni fondu hello. |
| Snadné DevOps mezi databázemi |[Elastické fondy](sql-database-elastic-pool.md): jak si předtím poznamenali. |
| | [Elastické dotazu](sql-database-elastic-query-horizontal-partitioning.md): dotazu mezi databázemi pro vytváření sestav nebo analýza cross klient. |
| | [Elastické úlohy](sql-database-elastic-jobs-overview.md): balíček a spolehlivě nasazovat operací údržby databáze nebo databáze toomultiple změní schéma databáze. |
| | [Elastické transakce](sql-database-elastic-transactions-overview.md): proces změny databáze tooseveral atomic a izolované způsobem. Pokud aplikace potřebují "všechny nebo nic" záruky přes několik operací databáze, je nutné elastické transakce. |
| | [Klientská knihovna pro elastické databáze](sql-database-elastic-database-client-library.md): Spravovat distribuce, data a mapy klienty toodatabases. |

## <a name="shared-models"></a>Sdílené modely
Jak je popsáno výše, pro většinu poskytovatelů SaaS, může představovat sdílený model přístup problémy s klienta izolace problémů a složité kroky se vývoj aplikací a údržbu. Ale pro víceklientské aplikace, které poskytují služby přímo klienta tooconsumers, nemusí být požadavky na izolaci jako vysokou prioritu jako minimalizovat náklady. Mohou být schopný toopack klientům v jedné nebo více databází v vysokou hustotou tooreduce náklady. Modely sdílené databáze pomocí jedné databáze nebo více horizontálně dělené databází může způsobit další efektivity prostředků sdílení a celkové náklady. Azure SQL Database zajišťuje, že některé funkce, které pomáhají zákazníkům sestavení izolace pro lepší zabezpečení a správu ve velkém měřítku v hello datové vrstvy.

| Požadavky na aplikace | Možnosti databáze SQL |
| --- | --- |
| Funkce izolace zabezpečení |[Zabezpečení na úrovni řádků](https://msdn.microsoft.com/library/dn765131.aspx) |
| [Schéma databáze](https://msdn.microsoft.com/library/dd207005.aspx) | |
| Snadné DevOps mezi databázemi |[Elastické dotazu](sql-database-elastic-query-horizontal-partitioning.md) |
| | [Elastické úlohy](sql-database-elastic-jobs-overview.md) |
| | [Elastické transakce](sql-database-elastic-transactions-overview.md) |
| | [Klientská knihovna Elastic Database](sql-database-elastic-database-client-library.md) |
| | [Rozdělení elastické databáze a sloučení](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Souhrn
Požadavky na izolaci klientů jsou důležité pro většinu víceklientské aplikace SaaS. Hello nejlepší možnost tooprovide izolace leans výraznou směrem k přístup hello databáze za klienta. Hello další dva přístupy vyžadují investice do vrstvy komplexní aplikace, které vyžadují izolaci tooprovide pracovníci zkušený vývoj, což významně zvyšuje náklady a rizika. Pokud požadavky na izolaci nejsou zahrnuté již v rané fázi v vývoj hello služby, může být modernizace je i dražší v první dva modely hello. hlavní nevýhody Hello přidružený k databázi za klienta modelu hello jsou nákladů prostředků cloudu související tooincreased kvůli tooreduced sdílení a údržbu a správu velkého počtu databází. Vývojáři aplikací SaaS často potýkat s tím zajistily při provádění těchto kompromis.

I když kompromis může být hlavní překážek s většinu poskytovatelů cloudových služeb databáze, databáze SQL Azure eliminuje hello překážek s jeho elastický fond a možnosti elastické databáze. Vývojáři SaaS můžete kombinovat hello izolace vlastnosti modelu databáze za klienta a optimalizace prostředků sdílení a hello vylepšení možností správy mnoho databází pomocí elastické fondy a související nástroje.

Víceklientské aplikace poskytovatele, kteří mají žádné požadavky na izolaci klientů a kdo může klientům pack databáze v vysokou hustotou zjistit, že sdílená data modelů výsledek v další efektivita při sdílení prostředků a snížit celkové náklady. Funkce zabezpečení, nástroje elastické databáze Azure SQL Database a knihovny horizontálního dělení pomoci poskytovatelů SaaS vytvářet a spravovat více klientů aplikace.

## <a name="next-steps"></a>Další kroky
[Začínáme s nástroje elastické databáze](sql-database-elastic-scale-get-started.md) a ukázkovou aplikaci, která demonstruje hello klientské knihovny.

Vytvoření [vlastní řídicí panel elastický fond pro SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) a ukázkovou aplikaci používající fondů elastické databáze nákladově efektivní, škálovatelné řešení.

Pomocí nástrojů Azure SQL Database hello příliš[migrovat existující databáze tooscale out](sql-database-elastic-convert-to-use-elastic-tools.md).

hello toocreate k elastického fondu pomocí portálu Azure najdete [vytvoření fondu elastické databáze](sql-database-elastic-pool-manage-portal.md).  

Zjistěte, jak příliš[sledování a správě fondu elastické databáze](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Další zdroje

* [Nasazení a prozkoumejte víceklientské aplikace, která používá Azure SQL Database – Wingtip SaaS](sql-database-saas-tutorial.md)
* [Co je Azure elastickém fondu?](sql-database-elastic-pool.md)
* [Horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md)
* [víceklientské aplikací pomocí nástroje elastické databáze a zabezpečení na úrovni řádků](sql-database-elastic-tools-multi-tenant-row-level-security.md)
* [Ověřování v víceklientské aplikace pomocí Azure Active Directory a OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [Aplikace Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)


## <a name="questions-and-feature-requests"></a>Otázky a žádosti o funkce

Dotazy se nám najít v hello [SQL Database fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Přidání žádosti o funkci v hello [fóru pro zpětnou vazbu SQL Database](https://feedback.azure.com/forums/217321-sql-database/).

