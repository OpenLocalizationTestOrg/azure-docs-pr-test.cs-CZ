---
title: "data dokumentu aaaModeling pro databáze NoSQL | Microsoft Docs"
description: "Další informace o modelování dat pro databáze NoSQL"
keywords: "modelování dat"
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig1
documentationcenter: 
ms.assetid: 69521eb9-590b-403c-9b36-98253a4c88b5
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2016
ms.author: arramac
ms.openlocfilehash: 2e388c833f204287896dfa8e6f79c88073731b6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="modeling-document-data-for-nosql-databases"></a>Data dokumentu pro databáze NoSQL modelování
Při databází bez schémat, jako je Azure Cosmos DB, snadno super tooembrace změny tooyour datový model, který byste se měli stále něco některé čas přemýšlení o vaše data. 

Jak se data bude toobe uložené? Jak je vaše aplikace má tooretrieve a dotaz data? Je vaše aplikace silná čtení nebo zápisu těžká? 

Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:

* Jak by měla myslíte o dokument v databázi dokumentů?
* Co je modelování dat a proč by měl vědět? 
* Jak je modelování dat v relační databázi dokumentů databáze různých tooa?
* Jak express relace mezi daty v jiných relační databázi?
* Když vložit data a kdy se propojení toodata?

## <a name="embedding-data"></a>Vkládání dat
Když spustíte modelování dat v dokumentu úložiště, jako je Azure Cosmos DB, zkuste tootreat vaší entity jako **nezávislý dokumenty** reprezentované ve formátu JSON.

Před jsme podrobné informace příliš mnohem dál, dejte nám provést pár kroků zpět a podívejte se na tom, jak něco v relační databázi, předmět, který se řadu nám obeznámeni s může modelu jsme. Hello následující příklad ukazuje, jak mohou být uloženy osoby v relační databázi. 

![Model relační databáze.](./media/documentdb-modeling-data/relational-data-model.png)

Při práci s relačních databází, jsme jste byla výukové pro let toonormalize normalizaci, normalizují.

Normalizaci dat obvykle zahrnuje přebírá entity, jako je osoba a jeho rozdělení v toodiscrete kusy data. V předchozím příkladu hello uživatel může mít více podrobností kontaktujte záznamů, jakož i více záznamů adresu. Jsme i další jít o krok a rozdělení kontaktní údaje další extrahováním běžné jako typ pole. Stejné pro adresu, každý záznam tady má typu like *Domů* nebo *firmy* 

Hello vedení místní normalizace dat je příliš**vyhnout ukládání redundantních dat** na každý záznam a místo odkazovat toodata. V tomto příkladu tooread osoba, s jejich kontaktní údaje a adresy, je nutné toouse spojení tooeffectively agregovaná data na dobu běhu.

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Aktualizace jedna osoba s jejich kontaktní údaje a adresy vyžaduje operace zápisu napříč mnoha jednotlivé tabulky. 

Nyní Pojďme proveďte podívejte se na tom, jak jsme by modelu hello stejná data jako samostatná entitu v databázi dokumentů.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "addresses": [
            {            
                "line1": "100 Some Street",
                "line2": "Unit 1",
                "city": "Seattle",
                "state": "WA",
                "zip": 98012
            }
        ],
        "contactDetails": [
            {"email: "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ] 
    }

Pomocí výše uvedených hello přístup máme teď **nenormalizované** hello záznam osoby kde jsme **embedded** všechny informace týkající se toothis osoba, jako je například jejich kontaktní údaje a adresy v jednom tooa hello Dokument JSON.
Kromě toho protože jsme nejsou omezen tooa pevné schéma, které máme hello flexibilitu toodo věcmi, jako jsou zcela nutnosti kontaktní údaje různých tvarů. 

Načítání dokončení osoba záznam z databáze hello je nyní jedné operace proti jedinou kolekci a pro jednotlivý dokument čtení. Aktualizace záznamu osoby s jejich kontaktní údaje a adresy, je také operace zápisu jeden pro jednotlivý dokument.

Podle denormalizing dat, vaše aplikace může být nutné tooissue méně dotazů a aktualizace toocomplete běžných operací. 

### <a name="when-tooembed"></a>Když tooembed
Vložená data se obvykle používá při modelů:

* Existují **obsahuje** vztahy mezi entitami.
* Existují **jeden několik** vztahy mezi entitami.
* Není vložená data, **zřídka mění**.
* Je vložených dat nebude růst **bez vazby**.
* Není vložená data, která je **integrální** toodata v dokumentu.

> [!NOTE]
> Obvykle nenormalizovanou datové modely poskytují lepší **číst** výkonu.
> 
> 

### <a name="when-not-tooembed"></a>Pokud není tooembed
Při hello pravidlo v databázi dokumentů je toodenormalize všechno a vložit všechna data v jednom dokumentu tooa, to může způsobit toosome situace, které by se mělo zabránit.

Trvat tohoto fragmentu kódu JSON.

    {
        "id": "1",
        "name": "What's new in hello coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from hello interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

To může být to, co bude entita post s embedded komentáře vypadat jsme modelování byly typické blogu nebo CMS, systému. Hello problém s v tomto příkladu je tento hello pole komentáře je **bez vazby**, znamená, že nebude žádná (praktické) omezení toohello počet komentáře může mít žádné jeden post. To začnou problém, protože velikost hello hello dokumentu může výrazně zvýší.

Jako hello velikost hello dokumentu zvětšování hello možnost tootransmit hello data přes přenosu hello, jakož i čtení a aktualizaci hello dokumentu ve velkém měřítku, bude mít vliv na.

V takovém případě je lepší tooconsider hello následující modelu.

    Post document:
    {
        "id": "1",
        "name": "What's new in hello coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from hello interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment documents:
    {
        "postId": "1"
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from hello field"},
            ...
            {"id": 99, "author": "angry", "comment": "blah angry blah angry"}
        ]
    },
    {
        "postId": "1"
        "comments": [
            {"id": 100, "author": "anon", "comment": "yet more"},
            ...
            {"id": 199, "author": "bored", "comment": "will this ever end?"}
        ]
    }

Tento model má hello tři nejnovější komentáře vložených na hello post sebe, což je pole s pevnou hranice to čas. Hello jiných komentáře jsou seskupené do toobatches 100 komentáře a uložené v samostatné dokumenty. Hello velikost dávky hello jste vybrali jako 100, protože naše fiktivní aplikace umožňuje hello 100 komentáře tooload uživatelů najednou.  

Jiné případu, kdy vnoření dat není vhodné je při hello vložených dat se často používá na dokumentech a se často mění. 

Trvat tohoto fragmentu kódu JSON.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            {
                "numberHeld": 100,
                "stock": { "symbol": "zaza", "open": 1, "high": 2, "low": 0.5 }
            },
            {
                "numberHeld": 50,
                "stock": { "symbol": "xcxc", "open": 89, "high": 93.24, "low": 88.87 }
            }
        ]
    }

To by mohl představovat uložených portfolií osoby. V dokumentu portfolií tooeach jsme vybrali tooembed hello uložené informace. V prostředí, kde související data mění často, jako je populace obchodních aplikací vkládání dat, která se často mění přechází toomean pokaždé, když se prodávají stock jsou neustále aktualizuje každý dokument portfolií.

Stock *zaza* může být prodávají mnoho stokrát v jediném den a tisíce uživatelů může mít *zaza* na jejich portfolií. S datovým modelem jako hello výše nám tooupdate mnoho tisíc portfolií dokumentů mnohokrát každý den, počáteční tooa systém, který nebude velmi dobře škálování. 

## <a id="Refer"></a>Odkazy na data
Ano vkládání dat vhodně funguje u mnoha případech ale je zřejmé, že existují scénáře při denormalizing data způsobí, že další problémy, než je vhodné. Proto co můžeme udělat teď? 

Relační databáze nejsou hello jediným místem, kde můžete vytvořit relace mezi entitami. V databázi dokumentů může mít informace v jednom z těchto dokumentů ve skutečnosti související s toodata v jiné dokumenty. Nyní I není zasazovat o i jednu minutu, využijeme systémy, které by bylo lepší vhodná tooa relační databáze v Azure Cosmos DB nebo jakékoli jiné databáze dokumentu, ale jsou jednoduché vztahy a může být velmi užitečná. 

V hello JSON níže jsme zvolili toouse hello ukázka uložených portfolia z dříve ale tentokrát označujeme toohello skladová položka na hello portfolií místo vkládání. Tímto způsobem, při skladová položka hello často mění v rámci hello den hello pouze dokument, který musí aktualizovat toobe je hello jeden uložených dokumentů. 

    Person document:
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            { "numberHeld":  100, "stockId": 1},
            { "numberHeld":  50, "stockId": 2}
        ]
    }

    Stock documents:
    {
        "id": "1",
        "symbol": "zaza",
        "open": 1,
        "high": 2,
        "low": 0.5,
        "vol": 11970000,
        "mkt-cap": 42000000,
        "pe": 5.89
    },
    {
        "id": "2",
        "symbol": "xcxc",
        "open": 89,
        "high": 93.24,
        "low": 88.87,
        "vol": 2970200,
        "mkt-cap": 1005000,
        "pe": 75.82
    }


Přístup toothis okamžitou nevýhodou ale je, pokud je vaše aplikace vyžaduje tooshow informace o každé populace, která se nachází při zobrazení osoby portfolií; v takovém případě musíte toomake více cest toohello tooload hello informace o databázi pro každý dokument uložené. Zde jsme provedli efektivitu rozhodnutí tooimprove hello operací zápisu, které dojít mnohokrát hello den, ale pak dojde k ohrožení na hello čtení operací, které můžou mít menší dopad na výkon hello tohoto konkrétního systému.

> [!NOTE]
> Normalized datové modely **může vyžadovat další odezev** toohello serveru.
> 
> 

### <a name="what-about-foreign-keys"></a>Co cizí klíče?
Vzhledem k tomu, že aktuálně neexistuje žádná koncepce omezení, cizí klíč nebo jinak, všechny vztahy mezi dokumentu, které máte v dokumentech jsou efektivně "slabé odkazy" a nebude ověřit pomocí samotná databáze hello. Pokud chcete, aby tooensure, který hello data, která odkazuje na dokument tooactually existuje, pak tuto funkci potřebujete toodo ve vaší aplikaci, nebo prostřednictvím hello serverové aktivační události nebo uložené procedury v Azure Cosmos DB.

### <a name="when-tooreference"></a>Když tooreference
Normalizovaný data se obvykle používá při modelů:

* Představující **na více** vztahy.
* Představující **m: n** vztahy.
* Související data **často mění**.
* Odkazovaná dat může být **bez vazby**.

> [!NOTE]
> Obvykle normalizace poskytuje lepší **zápisu** výkonu.
> 
> 

### <a name="where-do-i-put-hello-relationship"></a>Kam mohu dát hello relace?
Hello růstu hello relace vám pomohou určit v které odkaz na dokument toostore hello.

Pokud se podíváme na hello JSON modelů vydavatele a knih.

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": "1", "name": "Azure Cosmos DB 101" }
    {"id": "2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "3", "name": "Taking over hello world one JSON doc at a time" }
    ...
    {"id": "100", "name": "Learn about Azure Cosmos DB" }
    ...
    {"id": "1000", "name": "Deep Dive in tooAzure Cosmos DB" }

Pokud je číslo hello hello knih na vydavatele s omezenou růstu malé, může být užitečné pak ukládání hello kniha odkaz uvnitř hello vydavatele dokumentu. Ale pokud je číslo hello knih na vydavatele bez vazby, pak tento datový model nezkopírujete, dojde toomutable, ročně zvýší pole, jako hello příklad vydavatele dokumentu výše. 

Přepínání věcí kolem chvilku by výsledek v modelu stále představuje ale nyní hello stejná data zabraňuje těchto velkých měnitelný kolekcí.

    Publisher document: 
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }

    Book documents: 
    {"id": "1","name": "Azure Cosmos DB 101", "pub-id": "mspress"}
    {"id": "2","name": "Azure Cosmos DB for RDBMS Users", "pub-id": "mspress"}
    {"id": "3","name": "Taking over hello world one JSON doc at a time"}
    ...
    {"id": "100","name": "Learn about Azure Cosmos DB", "pub-id": "mspress"}
    ...
    {"id": "1000","name": "Deep Dive in tooAzure Cosmos DB", "pub-id": "mspress"}

V hello výše příkladu jsme vyřadit hello kolekce bez vazby na hello vydavatele dokumentu. Místo toho jsme právě mít vydavatel toohello odkaz na každý dokument adresáře.

### <a name="how-do-i-model-manymany-relationships"></a>Jak modelu relace m: n?
V relační databázi *m: n* vztahy jsou často modelován s tabulkami spojení, které právě spojení záznamů z jiných tabulek. 

![Spojování tabulek](./media/documentdb-modeling-data/join-table.png)

Můžete mít tendenci tooreplicate hello stejnou věc pomocí dokumenty a vytvoření datového modelu, který vypadá podobně jako následující toohello.

    Author documents: 
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over hello world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive in tooAzure Cosmos DB" }

    Joining documents: 
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

To by fungovat. Ale načítání buď Autor s jejich seznamů nebo načtení knihy s jeho autor by vždy vyžadují alespoň dva další dotazy na databázi hello. Propojení dokumentu a pak další dotaz toofetch hello skutečné dokumentu je připojený k toohello jeden dotaz. 

Pokud všechny, které se provádí v této tabulce spojení je připevnění společně dva kusy dat, pak Proč ne ho můžete vypustit úplně?
Vezměte v úvahu následující hello.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents: 
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in tooAzure Cosmos DB", "authors": ["a2"]}

Nyní I měli Autor, okamžitě vím knihy, které uživatel vytvořil a naopak Pokud I měli načíst kniha dokumentu by vím hello ID autory hello. To umožňuje ušetřit tento zprostředkující dotaz na tabulku spojení hello snížení počtu hello serveru odezev toomake má vaše aplikace. 

## <a id="WrapUp"></a>Hybridní datové modely
Jste nyní hledá vložení (nebo denormalizing) a odkazující (nebo normalizace) dat, mají jejich upsides a mají ohrožení jak jsme mohli vidět. 

Ji není vždy máte toobe buď nebo, nemusíte být Vystrašené toomix věcí trochu nahoru. 

Na základě konkrétní vzorce a úloh, které můžou nastat případy, kdy kombinování vložených vaší aplikace a smysl využívaných dat a může realizace toosimpler aplikační logiku s méně serveru zaokrouhlit služebních cest současně se zachovává funkční úroveň výkonu .

Vezměte v úvahu následující JSON hello. 

    Author documents: 
    {
        "id": "a1",
        "firstName": "Thomas",
        "lastName": "Andersen",        
        "countOfBooks": 3,
         "books": ["b1", "b2", "b3"],
        "images": [
            {"thumbnail": "http://....png"}
            {"profile": "http://....png"}
            {"large": "http://....png"}
        ]
    },
    {
        "id": "a2",
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": ["b1"],
        "images": [
            {"thumbnail": "http://....png"}
        ]
    }

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
            {"id": "a2", "name": "William Wakefield", "thumbnailUrl": "http://....png"}
        ]
    },
    {
        "id": "b2",
        "name": "Azure Cosmos DB for RDBMS Users",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
        ]
    }

Zde jsme jste (většinou) a potom hello vložený model, kdy data z jiné entity jsou vloženy do nejvyšší úrovně dokumentu hello, ale je odkazován další data. 

Pokud se podíváte na dokument hello adresáře, uvidíme několik zajímavé pole, když se podíváme na pole hello autorů. Došlo *id* pole, která je v poli hello používáme také toorefer back tooan Autor dokumentu, běžnou praxí ve model normalizovaný, ale pak jsme *název* a *thumbnailUrl*. Jsme může s právě jsme *id* a levé tooget aplikace hello jakýchkoli dalších informací ho nepotřebují z hello příslušných Autor dokumentu s použitím hello "link", ale protože naše aplikace zobrazí jméno autora hello a miniatur obrázků s každou kniha zobrazí jsme uložit odezvy serveru toohello za adresáře v seznamu denormalizing **některé** data od autora hello.

Opravdu, pokud změníte jméno autora hello nebo jejich fotografii požadují tooupdate nám toogo aktualizace každých adresáře se někdy publikované ale pro naši aplikaci založené na hello předpokládá, že autoři neměnit jejich názvy velmi často, to je přijatelné návrhu rozhodnutí.  

V příkladu hello existují **předem vypočítat agregace** hodnoty toosave nákladné zpracování na operace čtení. V příkladu hello některá data hello vložených v hello Autor dokumentu jsou data, která se vypočítává za běhu. Pokaždé, když je publikována nového seznamu, se vytvoří dokument adresáře **a** hello countOfBooks pole je nastavena hodnota tooa počítá na základě počtu hello dokumentů knihy, která platí pro konkrétní autora. Tato optimalizace by dobré v systémech čtení velkou kde toodo výpočty můžete dovolit na zápis v pořadí toooptimize čtení.

Hello možnost toohave model s předem počítaných polí je možné, protože nepodporuje Azure Cosmos DB **transakcí několika dokumentů**. Mnoho NoSQL úložiště nelze provést transakce mezi dokumenty a proto doporučují, jako je například "vždy Vložit vše", z důvodu omezení toothis rozhodnutí o návrhu. V Azure DB Cosmos můžete použít aktivační události na straně serveru nebo uložené procedury, které vložit knihy a aktualizovat autoři všechny v transakci ACID. Teď je prostě **mít** tooembed všechno ve tooone dokumentu jenom toobe jistotu, že data zůstávají konzistentní.

## <a name="NextSteps"></a>Další kroky
Hello největších takeaways z tohoto článku je toounderstand, modelování ve světě bez schémat dat je stejně důležité jako někdy. 

Stejně jako neexistuje žádný jediný způsob toorepresent část dat na obrazovce, neexistuje žádný jediný způsob toomodel vaše data. Potřebujete toounderstand vaší aplikace a jak ji vytvoří, spotřebovat a zpracovat hello data. Pak použitím některé hello můžete nastavit zde uvedené pokyny o vytváření model, který řeší hello okamžitou potřebám vaší aplikace. Pokud vaše aplikace potřebují toochange, můžete využít flexibilitu hello tooembrace bez schémat databáze, změňte a snadno vyvíjet datový model. 

toolearn Další informace o databázi Cosmos Azure, najdete v toohello služby [dokumentace](https://azure.microsoft.com/documentation/services/cosmos-db/) stránky. 

toounderstand jak tooshard data napříč více oddílů, najdete v příliš[dělení dat v Azure Cosmos DB](documentdb-partition-data.md). 
