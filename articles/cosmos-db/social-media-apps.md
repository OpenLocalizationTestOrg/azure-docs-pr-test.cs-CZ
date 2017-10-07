---
title: "Vzor návrhu Azure Cosmos DB: sociálních médií aplikace | Microsoft Docs"
description: "Další informace o vzoru návrhu pro sociální sítě s využitím hello flexibilitu úložiště Azure Cosmos DB a jinými službami Azure."
keywords: "Aplikace v sociálních sítích"
services: cosmos-db
author: ealsur
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 2dbf83a7-512a-4993-bf1b-ea7d72e095d9
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: mimig
ms.openlocfilehash: 47a22f2c5762d62b176921c8052e7bd75d8cf6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="going-social-with-azure-cosmos-db"></a>Budete sociálních s Azure Cosmos DB
Žijí v společnosti massively propojeny znamená, že v určitém okamžiku v životnosti stane součástí **sociálních sítí**. Tookeep sociálních sítí ve spojení se přátel, kolegy, rodina nebo někdy tooshare používáme s lidmi s společné zájmy naše nadšení.

Jako technici nebo vývojáře jsme může mít zajímalo, jak tyto sítě úložiště a propojení naše data, nebo může mít i byla musí toocreate nebo architekt nové sociální sítě pro konkrétní volné místo na trhu sami sobě. Je při vzniká hello big Otázka: jak jsou tato data uložena?

Předpokládejme, že vytváříme nové a lesklý sociálních sítí, kde můžete naši uživatelé odesílat články s související média jako obrázky, videa nebo i Hudba. Uživatelé komentář k příspěvkům a poskytují body pro hodnocení. Nebudou příspěvky, které uživatelé uvidí informační kanál a být schopný toointeract s na cílovou stránku hello hlavní webové stránky. Toto není zvukových skutečně komplexní (na první), ale pro hello zájmu jednoduchost, můžeme zastavit existuje (jsme může pustíte do vlastní uživatelské kanály vliv na relace, ale jeho velikost překračuje limit hello cílem v tomto článku).

Ano jak to nám uložit a kde?

Řada z vás mohl být prostředí v databázích SQL nebo mít aspoň představu o [relační modelování dat](https://en.wikipedia.org/wiki/Relational_model) a může být tendenci toostart kreslení přibližně takto:

![Diagram ilustrující relativní relační modelu](./media/social-media-apps/social-media-apps-sql.png) 

Dokonale normalizovaný a velmi datová struktura... které nemá škálování. 

Nezískávat mi nesprávný I jste pracovali s databází SQL všechny život, jsou skvělé, ale jako každé platformě vzor, postupů a softwaru není ideální pro každý scénář.

Proč není SQL hello nejlepší volbou v tomto scénáři? Podívejme se na hello struktura jeden post, pokud měla být tooshow, která se na webu nebo v aplikaci, bude nutné toodo dotaz s... Tabulka 8 spojení (!) jenom tooshow jeden jeden post, nyní, proud příspěvky dynamicky načíst a zobrazí na úvodní obrazovka a může dojít, kam bude obrázek.

Může samozřejmě používáme ABC instance SQL s dostatek power toosolve tisíc dotazů s tyto mnoho tooserve spojení náš obsah, ale skutečně, proč by jsme při jednodušší řešení existuje?

## <a name="hello-nosql-road"></a>silniční NoSQL Hello
Tento článek vám pomohou do modelování sociální platforma dat pomocí Azure, je databáze NoSQL [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) v nákladově efektivní způsob, jak současně využívat další funkce Azure Cosmos DB jako hello [Gremlin Graph API ](../cosmos-db/graph-introduction.md). Použití [NoSQL](https://en.wikipedia.org/wiki/NoSQL) přístup, ukládání dat ve formátu JSON a použití [denormalization](https://en.wikipedia.org/wiki/Denormalization), lze je transformovat naše dříve složitá post do jednoho [dokumentu](https://en.wikipedia.org/wiki/Document-oriented_database):


    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["http://myfirstimage.png","http://mysecondimage.png"],
        "videos":[
            {"url":"http://myfirstvideo.mp4", "title":"hello first video"},
            {"url":"http://mysecondvideo.mp4", "title":"hello second video"}
        ],
        "audios":[
            {"url":"http://myfirstaudio.mp3", "title":"hello first audio"},
            {"url":"http://mysecondaudio.mp3", "title":"hello second audio"}
        ]
    }

A jej můžete získat pomocí jednoho dotazu a s žádné spojení. Toto je mnohem víc jednoduchá a přímočará a budget-wise, vyžaduje méně prostředků tooachieve lepší výsledek.

Azure Cosmos DB zajišťuje, že všechny vlastnosti hello jsou indexované jeho automatické indexování, může být i [přizpůsobené](indexing-policies.md). bez přístupu umožňuje nám uložit jiné dokumenty a dynamické struktury, možná zítra chceme toohave příspěvky, který zpracovává seznam kategorií nebo hashtags spojené s nimi Cosmos DB hello nové dokumenty s hello schémat Hello přidat atributy se žádné další Práce nutná tím, abychom.

Komentáře v příspěvku na lze považovat za právě dalších bodů se nadřazený klíč (zjednodušuje naše mapování objektu). 

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":User2,
        "parent":"ew12-res2-234e-544f"
    }

    {
        "id":"asd2-fee4-23gc-jh67",
        "title":"Ditto!",
        "date":"2016-01-03",
        "createdBy":User3,
        "parent":"ew12-res2-234e-544f"
    }

A všechny sociálních interakce může být uložený na samostatný objekt jako čítače:

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

Vytváření informační kanály stačí vytváření dokumenty, které mohou být uloženy seznam ID post s danou relevance pořadí:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

Máme může "posledního" datového proudu s hraniční seřazené podle data vytvoření, "nejprodávanějších" datového proudu s těmi, která odešle s další líbí v hello posledních 24 hodin, může i implementaci vlastního datového proudu pro každého uživatele na základě logiky jako délky a zájmů a stále je seznam  příspěvky. Je řádu jak toobuild tyto obsahuje seznam, ale pořád nerušený hello čtení výkonu. Jakmile jsme získat jeden z těchto seznamů, jsme vydání tooCosmos jeden dotaz databáze pomocí hello [v operátoru](documentdb-sql-query.md#WhereClause) tooobtain stránkách příspěvcích najednou.

Hello kanálu datové proudy, může být postavená pomocí [Azure App Services](https://azure.microsoft.com/services/app-service/) procesy na pozadí: [Webjobs](../app-service-web/web-sites-create-web-jobs.md). Po vytvoření příspěvku na zpracování na pozadí můžete spustit pomocí [Azure Storage](https://azure.microsoft.com/services/storage/) [fronty](../storage/queues/storage-dotnet-how-to-use-queues.md) a webové úlohy aktivaci pomocí hello [Azure Webjobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md), implementující Hello post šíření uvnitř streamů v závislosti na vlastní vlastní logiky. 

Body a líbí na příspěvek lze zpracovat odložené způsobem pomocí této stejným způsobem toocreate nakonec byl konzistentní prostředí.

Délky jsou trickier. Cosmos databáze může mít dokument maximální velikost a čtení/zápis velkých dokumentů může mít vliv na škálovatelnost hello vaší aplikace. Proto si promyslet ukládání délky jako dokument s tuto strukturu:

    {
        "id":"234d-sd23-rrf2-552d",
        "followersOf": "dse4-qwe2-ert4-aad2",
        "followers":[
            "ewr5-232d-tyrg-iuo2",
            "qejh-2345-sdf1-ytg5",
            //...
            "uie0-4tyg-3456-rwjh"
        ]
    }

To může fungovat pro uživatele s několika tisíc délky, ale některé celebrit spojí naše pořadí, tento přístup povede velikost tooa velké dokumentu, a může cap velikost dokumentu nakonec přístupů hello.

toosolve, můžeme použít smíšený přístup. Jako součást hello statistické údaje o uživatelích dokumentu ukládáme hello počet délky:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

A hello skutečné graf délky mohou být uloženy pomocí Azure Cosmos DB [Gremlin Graph API](../cosmos-db/graph-introduction.md), toocreate [bodů uchycení](http://mathworld.wolfram.com/GraphVertex.html) pro každého uživatele a [okraje](http://mathworld.wolfram.com/GraphEdge.html) , Udržovat hello " Vztahy A-způsobem B". Hello rozhraní Graph API umožňuje jste nejen získat hello délky určitého uživatele, ale vytvořit složitější dotazy tooeven společné navrhnout osoby. Pokud přidáme toohello grafu hello kategorií obsah, který lidé jako nebo získejte, můžeme začít tkaní prostředí, které zahrnují zjišťování inteligentní obsahu, k obsahu, která se, budeme postupovat podle jako, nebo při hledání osob, se kterými může máme mnohem společné.

Hello statistické údaje o uživatelích dokument může být stále použité toocreate karty v hello uživatelského rozhraní nebo rychlé profil verze Preview.

## <a name="hello-ladder-pattern-and-data-duplication"></a>Hello "Žebříku" duplikace vzor a data
Jak je možná jste si všimli v dokumentu JSON hello, který odkazuje na metodu POST směřující, se vyskytuje víckrát uživatele. A jste by mít uhádnout správné, že to znamená, že hello informace, který reprezentuje uživatele, danou tento denormalization, může být k dispozici více než jednom místě.

V pořadí tooallow pro rychlejší dotazy budeme způsobit odstranění duplicitních dat. Hello problém s vedlejším účinkem je, pokud některé akce, změny dat uživatele, potřebujeme toofind všechny aktivity hello že někdy nebyla a všechny jejich aktualizaci. Není zvuk velmi praktické, pravé?

Snažíme se probíhající toosolve se tím, že určíte hello klíč atributy uživatele, který ukážeme v naší aplikaci pro každou aktivitu. Pokud jsme vizuálně zobrazit metodu POST směřující v naší aplikaci a zobrazují jenom hello creator název a obrázek, proč uložit všechny hello uživatelská data do atribut vytvořená"hello"? Pokud pro každý komentář jsme právě hello uživatele obrázek zobrazit, jsme nepotřebujeme hello zbytek jeho informace. To je, kde něco že můžu volat hello "žebříku vzor" dodává do play.

Podívejme se na informace o uživateli, jako například:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "address":"742 Evergreen Terrace",
        "birthday":"1983-05-07",
        "email":"john@doe.com",
        "twitterHandle":"@john",
        "username":"johndoe",
        "password":"some_encrypted_phrase",
        "totalPoints":100,
        "totalPosts":24
    }

Pohledem na tyto informace jsme můžete rychle zjistit, což je důležité informace a které není, čímž vytvoříte "Žebříku":

![Diagram žebříku vzor](./media/social-media-apps/social-media-apps-ladder.png)

Hello nejmenší krok se nazývá UserChunk, hello minimální část informace, které identifikují uživatele a používá se pro odstranění duplicitních dat. Zmenšení velikosti hello duplicitní data tooonly hello informace "ukážeme" hello, jsme snížit možnost hello masivní aktualizací.

Střední krok Hello se nazývá hello uživatele, je hello úplné dat, které se použijí na většinu závislé na výkonu dotazů na Cosmos DB hello nejčastěji používaná a kritické. Obsahuje informace hello reprezentována UserChunk.

největší Hello je hello Extended uživatele. Obsahuje informace o všech důležitých uživatelů hello plus další data, která nevyžaduje skutečně toobe čtení rychle nebo jeho využití je případný (např. proces přihlášení hello). Tato data mohou být uloženy mimo Cosmos databáze, v Azure SQL Database nebo úložiště tabulek Azure.

Proč by jsme rozdělit hello uživatele a i tyto informace ukládaly na různých místech? Vzhledem k tomu, že z výkonu hlediska, hello větší hello dokumenty hello costlier hello dotazy. Zachovat dokumenty tenký s hello správné informace toodo všechny vaše výkonu závislém dotazuje na sociálních sítí a úložiště hello jiné doplňující informace pro případné scénáře jako, úplná profil úpravy, přihlášení, a to i dolování dat pro analýzy využití a Big Data iniciativy. Jsme skutečně nezajímá je, pokud je pomalejší hello shromažďování dat pro dolování dat, protože v databázi SQL Azure je spuštěná, budeme mít se týkají ale že naši uživatelé mají rychlé a tenký prostředí. Uživatel, uložený na Cosmos databáze, bude vypadat takto:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"@john"
    }

A bude vypadat příspěvku na:

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
            "id":"dse4-qwe2-ert4-aad2",
            "username":"johndoe"
        }
    }

A pokud upravíte vznikne, kde jeden z atributů hello od hello bloku má vliv, je snadné toofind hello vliv na dokumenty pomocí dotazů, které odkazují toohello indexované atributy (vybrat * FROM odešle p kde p.createdBy.id == "edited_user_id") a potom aktualizace bloky Hello.

## <a name="hello-search-box"></a>Hello vyhledávacího pole
Uživatelé budou naštěstí vygeneroval velké množství obsahu. A jsme by mělo být možné tooprovide hello možnost toosearch a najít obsah, který nemusí být přímo v jejich obsahu datové proudy, možná vzhledem k tomu, že jsme nemáte podle hello tvůrcích, nebo možná právě snažíme toofind tento starý post, kterou jsme provedli 6 měsíců před.

Naštěstí a protože používáme Azure Cosmos DB, můžeme snadno implementovat modul vyhledávání pomocí [Azure Search](https://azure.microsoft.com/services/search/) během několika minut a aniž by museli zadávat jeden řádek kódu (jiné než samozřejmě hello vyhledávání proces a uživatelského rozhraní).

Proč je to tak snadno?

Služba Azure Search implementuje co volání [indexery](https://msdn.microsoft.com/library/azure/dn946891.aspx), pozadí zpracovává této háku v datových úložišť a automagically přidání, aktualizace nebo odebrání vašich objektů v hello indexy. Podporují [Azure SQL Database indexery](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [objektů BLOB služby Azure indexery](../search/search-howto-indexing-azure-blob-storage.md) a naštěstí [indexery Azure Cosmos DB](../search/search-howto-index-documentdb.md). Hello přechod informací z databáze Cosmos tooAzure vyhledávání je jednoduché, jako i informace o úložišti ve formátu JSON, potřebujeme příliš[vytvořit Index](../search/search-create-index-portal.md) a mapovat atributy, které z našich dokumentů chceme indexované a je to, během několika minut (závisí na velikosti hello naše data), náš obsah bude k dispozici toobe hledaných hello nejlepší řešení vyhledávání jako služby v infrastruktuře cloudu. 

Další informace o službě Azure Search, můžete navštívit hello [Hitchhiker na Průvodce tooSearch](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/).

## <a name="hello-underlying-knowledge"></a>základní znalosti Hello
Po uložení tohoto obsahu, který zvětšování a zvětšování každý den, nám může najít označována myslím: jak s tímto proudem informací z mé uživatelé?

Hello odpovědí je jednoduchý: toowork pro něj a zjistěte z něj.

Ale co jsme další? Několik snadno příklady [postojích analysis](https://en.wikipedia.org/wiki/Sentiment_analysis)obsahu doporučení na základě uživatelské předvolby nebo i automatizované obsahu moderátora, která zajišťuje, že všechny hello obsahu, které zveřejnil naše sociální sítě je bezpečné pro přístup z hello rodiny.

Teď, když se zobrazí chybové jste připojili, budete pravděpodobně domníváte, že potřebujete některé aplikace PhotoDraw v matematické vědecké účely tooextract tyto vzory a informace o jednoduché databáze a soubory, ale by být nesprávné.

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), součástí hello [Cortana Intelligence Suite](https://www.microsoft.com/en/server-cloud/cortana-analytics-suite/overview.aspx), je plně spravovaná Cloudová služba, která umožňuje vytvořit pracovní postupy pomocí algoritmů v jednoduché rozhraní přetahování myší, kód vlastní algoritmy hello v [R](https://en.wikipedia.org/wiki/R_\(programming_language\)) nebo používat některé z hello už sestavené a toouse rozhraní API, jako připravené: [Analýza textu](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [obsahu moderátora](https://www.microsoft.com/moderator) nebo [doporučení](https://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

tooachieve žádné z těchto scénářů Machine Learning, můžeme použít [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) tooingest hello informace z různých zdrojů a používat [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) tooprocess hello informace a generovat výstup který může zpracovat Azure Machine Learning.

K dispozici další možností je toouse [kognitivní služby Microsoft](https://www.microsoft.com/cognitive-services) tooanalyze naši uživatelé obsahu; nejen můžete Chápeme, je lepší (prostřednictvím analýza zapisují s [Text Analytics API](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), ale jsme může také rozpoznání nežádoucí nebo vyspělá obsahu a odpovídajícím způsobem fungují s [počítače vize API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Kognitivní služby zahrnují spoustu out-of-the-box řešení, které nevyžadují jakýkoli druh toouse znalostní báze Machine Learning.

## <a name="a-planet-scale-social-experience"></a>V sociálních rozhraní planetu škálování
Je poslední, ale zase důležité tématu I, musí řešit: **škálovatelnost**. Při navrhování architekturu, kterou je třeba, že jednotlivé komponenty můžete škálovat sama o sobě, buď protože potřebujeme tooprocess více dat, nebo protože chceme toohave větší zeměpisném pokrytí (nebo oba!). Naštěstí je dosažení složité úlohy **připraveného prostředí** s Cosmos DB.

Podporuje cosmos DB [dynamické rozdělení](https://azure.microsoft.com/blog/10-things-to-know-about-documentdb-partitioned-collections/) out-of-the-box automaticky vytvořením oddíly na základě danou **klíč oddílu** (definován jako jeden z atributů hello v dokumentech). Definování hello správný klíč oddílu je třeba provést v době návrhu a udržuje v paměti hello [osvědčené postupy](../cosmos-db/partition-data.md#designing-for-partitioning) není k dispozici; v případě hello sociálních prostředí, musí být zarovnána strategie dělení hello způsob dotazu (čtení v rámci hello je žádoucí stejného oddílu) a zápis (vyhnout "aktivní body" tak, že se zápisy na více oddílů). Některé možnosti jsou: oddíly založené na dočasný klíč (den/měsíc/týden), podle obsahu kategorie podle zeměpisné oblasti uživatelem; všechny skutečně závisí na tom, jak bude dotazování dat hello a zobrazit v sociálních prostředí. 

Jeden je zajímavé bod důležité zmínit, Cosmos DB bude spuštěn své dotazy (včetně [agregace](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)) pro všechny oddíly transparentně, nepotřebujete tooadd veškeré logiky s růstem vaše data.

S časem, můžete se nakonec zvýší v provozu a vaší spotřeby prostředků (měřeno v [RUs](request-units.md), nebo jednotky žádosti) se zvýší. Bude číst a zapisovat častěji, jako vaše userbase zvětšování a začnou vytváření a čtení víc obsahu; Hello schopnost **škálování vašeho propustnost** je životně důležité. Zvýšení naše RUs je velmi snadné, jsme můžete provést pomocí několika kliknutí na hello portálu Azure nebo pomocí [vydávání příkazů prostřednictvím hello rozhraní API](https://docs.microsoft.com/rest/api/documentdb/replace-an-offer).

![Škálování a definování klíč oddílu](./media/social-media-apps/social-media-apps-scaling.png)

Co se stane, když věcí zachovat zlepšuje a uživatelé z jiné oblasti, země nebo kontinentě, Všimněte si vaši platformu a začít používat ji, skvělé neočekávaném!

Počkejte..., ale brzy zjistíte, své zkušenosti s vaši platformu není optimální; jsou dosavadní od vaší provozní oblasti hello latence je strašlivých, a samozřejmě nechcete, aby je tooquit. Pokud jenom došlo snadný způsob **rozšíření globální sítě**... ale!

Cosmos DB umožňuje [replikovat data globálně](../cosmos-db/tutorial-global-distribution-documentdb.md) a transparentně pomocí několika kliknutí a automaticky vybrat mezi dostupné oblasti hello z vaší [kód klienta](../cosmos-db/tutorial-global-distribution-documentdb.md). To také znamená, že můžete mít [více oblastí převzetí služeb při selhání](regional-failover.md). 

Při replikaci dat globálně, je nutné toomake jistotu, že vaši klienti mohou využít výhod ho. Pokud používáte front-endu webové nebo accesing rozhraní API z mobilních klientů, můžete nasadit [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) a klonování Azure App Service na všech oblastech hello potřeby, použití [konfigurace výkonu](../app-service-web/web-sites-traffic-manager.md)toosupport rozšířené globální pokrytí. Pokud vaši klienti přístup k rozhraní API nebo front-endu, budou směrované toohello nejbližší aplikace služby, která se pak připojí toohello místní repliky databáze Cosmos.

![Přidání globální pokrytí tooyour sociální platforma](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Závěr
Tento článek se pokusí tooshed některé light do hello alternativy úplně vytváření sociálních sítí v Azure s nízkými náklady službami a poskytuje skvělé výsledky podle podporovat využití víceúrovňová úložiště řešení a data distribuční názvem "Žebříku" hello.

![Diagram interakce mezi Azure services pro sociálních sítí](./media/social-media-apps/social-media-apps-azure-solution.png)

pravdivosti Hello je, že neexistuje žádná stříbrným odrážka pro tento druh scénářů, má hello součinnosti vytvořené kombinace hello kvalitních služeb, která nám umožňují toobuild skvělé prostředí: hello rychlost a volný Azure Cosmos DB tooprovide skvělé sociálních aplikaci, Hello intelligence za řešení prvotřídní vyhledávání, například Azure Search, hello flexibilitu toohost Azure App Services není i bez ohledu na jazyk aplikace ale procesy na pozadí výkonný a hello rozšíření Azure Storage a Azure SQL Database pro ukládání masivní objemy dat a hello analýzy výkonu Azure Machine Learning toocreate znalostní báze a intelligence, která může poskytnout zpětnou vazbu tooour procesy a Pomozte nám poskytovat hello právo obsahu toohello ti správní uživatelé.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o případů použití pro databáze Cosmos, najdete v části [případy použití běžné DB Cosmos](use-cases.md).
