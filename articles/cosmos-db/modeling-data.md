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
# <a name="modeling-document-data-for-nosql-databases"></a><span data-ttu-id="ded11-104">Data dokumentu pro databáze NoSQL modelování</span><span class="sxs-lookup"><span data-stu-id="ded11-104">Modeling document data for NoSQL databases</span></span>
<span data-ttu-id="ded11-105">Při databází bez schémat, jako je Azure Cosmos DB, snadno super tooembrace změny tooyour datový model, který byste se měli stále něco některé čas přemýšlení o vaše data.</span><span class="sxs-lookup"><span data-stu-id="ded11-105">While schema-free databases, like Azure Cosmos DB, make it super easy tooembrace changes tooyour data model you should still spend some time thinking about your data.</span></span> 

<span data-ttu-id="ded11-106">Jak se data bude toobe uložené?</span><span class="sxs-lookup"><span data-stu-id="ded11-106">How is data going toobe stored?</span></span> <span data-ttu-id="ded11-107">Jak je vaše aplikace má tooretrieve a dotaz data?</span><span class="sxs-lookup"><span data-stu-id="ded11-107">How is your application going tooretrieve and query data?</span></span> <span data-ttu-id="ded11-108">Je vaše aplikace silná čtení nebo zápisu těžká?</span><span class="sxs-lookup"><span data-stu-id="ded11-108">Is your application read heavy, or write heavy?</span></span> 

<span data-ttu-id="ded11-109">Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:</span><span class="sxs-lookup"><span data-stu-id="ded11-109">After reading this article, you will be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="ded11-110">Jak by měla myslíte o dokument v databázi dokumentů?</span><span class="sxs-lookup"><span data-stu-id="ded11-110">How should I think about a document in a document database?</span></span>
* <span data-ttu-id="ded11-111">Co je modelování dat a proč by měl vědět?</span><span class="sxs-lookup"><span data-stu-id="ded11-111">What is data modeling and why should I care?</span></span> 
* <span data-ttu-id="ded11-112">Jak je modelování dat v relační databázi dokumentů databáze různých tooa?</span><span class="sxs-lookup"><span data-stu-id="ded11-112">How is modeling data in a document database different tooa relational database?</span></span>
* <span data-ttu-id="ded11-113">Jak express relace mezi daty v jiných relační databázi?</span><span class="sxs-lookup"><span data-stu-id="ded11-113">How do I express data relationships in a non-relational database?</span></span>
* <span data-ttu-id="ded11-114">Když vložit data a kdy se propojení toodata?</span><span class="sxs-lookup"><span data-stu-id="ded11-114">When do I embed data and when do I link toodata?</span></span>

## <a name="embedding-data"></a><span data-ttu-id="ded11-115">Vkládání dat</span><span class="sxs-lookup"><span data-stu-id="ded11-115">Embedding data</span></span>
<span data-ttu-id="ded11-116">Když spustíte modelování dat v dokumentu úložiště, jako je Azure Cosmos DB, zkuste tootreat vaší entity jako **nezávislý dokumenty** reprezentované ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ded11-116">When you start modeling data in a document store, such as Azure Cosmos DB, try tootreat your entities as **self-contained documents** represented in JSON.</span></span>

<span data-ttu-id="ded11-117">Před jsme podrobné informace příliš mnohem dál, dejte nám provést pár kroků zpět a podívejte se na tom, jak něco v relační databázi, předmět, který se řadu nám obeznámeni s může modelu jsme.</span><span class="sxs-lookup"><span data-stu-id="ded11-117">Before we dive in too much further, let us take a few steps back and have a look at how we might model something in a relational database, a subject many of us are already familiar with.</span></span> <span data-ttu-id="ded11-118">Hello následující příklad ukazuje, jak mohou být uloženy osoby v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="ded11-118">hello following example shows how a person might be stored in a relational database.</span></span> 

![Model relační databáze.](./media/documentdb-modeling-data/relational-data-model.png)

<span data-ttu-id="ded11-120">Při práci s relačních databází, jsme jste byla výukové pro let toonormalize normalizaci, normalizují.</span><span class="sxs-lookup"><span data-stu-id="ded11-120">When working with relational databases, we've been taught for years toonormalize, normalize, normalize.</span></span>

<span data-ttu-id="ded11-121">Normalizaci dat obvykle zahrnuje přebírá entity, jako je osoba a jeho rozdělení v toodiscrete kusy data.</span><span class="sxs-lookup"><span data-stu-id="ded11-121">Normalizing your data typically involves taking an entity, such as a person, and breaking it down in toodiscrete pieces of data.</span></span> <span data-ttu-id="ded11-122">V předchozím příkladu hello uživatel může mít více podrobností kontaktujte záznamů, jakož i více záznamů adresu.</span><span class="sxs-lookup"><span data-stu-id="ded11-122">In hello example above, a person can have multiple contact detail records as well as multiple address records.</span></span> <span data-ttu-id="ded11-123">Jsme i další jít o krok a rozdělení kontaktní údaje další extrahováním běžné jako typ pole.</span><span class="sxs-lookup"><span data-stu-id="ded11-123">We even go one step further and break down contact details by further extracting common fields like a type.</span></span> <span data-ttu-id="ded11-124">Stejné pro adresu, každý záznam tady má typu like *Domů* nebo *firmy*</span><span class="sxs-lookup"><span data-stu-id="ded11-124">Same for address, each record here has a type like *Home* or *Business*</span></span> 

<span data-ttu-id="ded11-125">Hello vedení místní normalizace dat je příliš**vyhnout ukládání redundantních dat** na každý záznam a místo odkazovat toodata.</span><span class="sxs-lookup"><span data-stu-id="ded11-125">hello guiding premise when normalizing data is too**avoid storing redundant data** on each record and rather refer toodata.</span></span> <span data-ttu-id="ded11-126">V tomto příkladu tooread osoba, s jejich kontaktní údaje a adresy, je nutné toouse spojení tooeffectively agregovaná data na dobu běhu.</span><span class="sxs-lookup"><span data-stu-id="ded11-126">In this example, tooread a person, with all their contact details and addresses, you need toouse JOINS tooeffectively aggregate your data at run time.</span></span>

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

<span data-ttu-id="ded11-127">Aktualizace jedna osoba s jejich kontaktní údaje a adresy vyžaduje operace zápisu napříč mnoha jednotlivé tabulky.</span><span class="sxs-lookup"><span data-stu-id="ded11-127">Updating a single person with their contact details and addresses requires write operations across many individual tables.</span></span> 

<span data-ttu-id="ded11-128">Nyní Pojďme proveďte podívejte se na tom, jak jsme by modelu hello stejná data jako samostatná entitu v databázi dokumentů.</span><span class="sxs-lookup"><span data-stu-id="ded11-128">Now let's take a look at how we would model hello same data as a self-contained entity in a document database.</span></span>

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

<span data-ttu-id="ded11-129">Pomocí výše uvedených hello přístup máme teď **nenormalizované** hello záznam osoby kde jsme **embedded** všechny informace týkající se toothis osoba, jako je například jejich kontaktní údaje a adresy v jednom tooa hello Dokument JSON.</span><span class="sxs-lookup"><span data-stu-id="ded11-129">Using hello approach above we have now **denormalized** hello person record where we **embedded** all hello information relating toothis person, such as their contact details and addresses, in tooa single JSON document.</span></span>
<span data-ttu-id="ded11-130">Kromě toho protože jsme nejsou omezen tooa pevné schéma, které máme hello flexibilitu toodo věcmi, jako jsou zcela nutnosti kontaktní údaje různých tvarů.</span><span class="sxs-lookup"><span data-stu-id="ded11-130">In addition, because we're not confined tooa fixed schema we have hello flexibility toodo things like having contact details of different shapes entirely.</span></span> 

<span data-ttu-id="ded11-131">Načítání dokončení osoba záznam z databáze hello je nyní jedné operace proti jedinou kolekci a pro jednotlivý dokument čtení.</span><span class="sxs-lookup"><span data-stu-id="ded11-131">Retrieving a complete person record from hello database is now a single read operation against a single collection and for a single document.</span></span> <span data-ttu-id="ded11-132">Aktualizace záznamu osoby s jejich kontaktní údaje a adresy, je také operace zápisu jeden pro jednotlivý dokument.</span><span class="sxs-lookup"><span data-stu-id="ded11-132">Updating a person record, with their contact details and addresses, is also a single write operation against a single document.</span></span>

<span data-ttu-id="ded11-133">Podle denormalizing dat, vaše aplikace může být nutné tooissue méně dotazů a aktualizace toocomplete běžných operací.</span><span class="sxs-lookup"><span data-stu-id="ded11-133">By denormalizing data, your application may need tooissue fewer queries and updates toocomplete common operations.</span></span> 

### <a name="when-tooembed"></a><span data-ttu-id="ded11-134">Když tooembed</span><span class="sxs-lookup"><span data-stu-id="ded11-134">When tooembed</span></span>
<span data-ttu-id="ded11-135">Vložená data se obvykle používá při modelů:</span><span class="sxs-lookup"><span data-stu-id="ded11-135">In general, use embedded data models when:</span></span>

* <span data-ttu-id="ded11-136">Existují **obsahuje** vztahy mezi entitami.</span><span class="sxs-lookup"><span data-stu-id="ded11-136">There are **contains** relationships between entities.</span></span>
* <span data-ttu-id="ded11-137">Existují **jeden několik** vztahy mezi entitami.</span><span class="sxs-lookup"><span data-stu-id="ded11-137">There are **one-to-few** relationships between entities.</span></span>
* <span data-ttu-id="ded11-138">Není vložená data, **zřídka mění**.</span><span class="sxs-lookup"><span data-stu-id="ded11-138">There is embedded data that **changes infrequently**.</span></span>
* <span data-ttu-id="ded11-139">Je vložených dat nebude růst **bez vazby**.</span><span class="sxs-lookup"><span data-stu-id="ded11-139">There is embedded data won't grow **without bound**.</span></span>
* <span data-ttu-id="ded11-140">Není vložená data, která je **integrální** toodata v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ded11-140">There is embedded data that is **integral** toodata in a document.</span></span>

> [!NOTE]
> <span data-ttu-id="ded11-141">Obvykle nenormalizovanou datové modely poskytují lepší **číst** výkonu.</span><span class="sxs-lookup"><span data-stu-id="ded11-141">Typically denormalized data models provide better **read** performance.</span></span>
> 
> 

### <a name="when-not-tooembed"></a><span data-ttu-id="ded11-142">Pokud není tooembed</span><span class="sxs-lookup"><span data-stu-id="ded11-142">When not tooembed</span></span>
<span data-ttu-id="ded11-143">Při hello pravidlo v databázi dokumentů je toodenormalize všechno a vložit všechna data v jednom dokumentu tooa, to může způsobit toosome situace, které by se mělo zabránit.</span><span class="sxs-lookup"><span data-stu-id="ded11-143">While hello rule of thumb in a document database is toodenormalize everything and embed all data in tooa single document, this can lead toosome situations that should be avoided.</span></span>

<span data-ttu-id="ded11-144">Trvat tohoto fragmentu kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="ded11-144">Take this JSON snippet.</span></span>

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

<span data-ttu-id="ded11-145">To může být to, co bude entita post s embedded komentáře vypadat jsme modelování byly typické blogu nebo CMS, systému.</span><span class="sxs-lookup"><span data-stu-id="ded11-145">This might be what a post entity with embedded comments would look like if we were modeling a typical blog, or CMS, system.</span></span> <span data-ttu-id="ded11-146">Hello problém s v tomto příkladu je tento hello pole komentáře je **bez vazby**, znamená, že nebude žádná (praktické) omezení toohello počet komentáře může mít žádné jeden post.</span><span class="sxs-lookup"><span data-stu-id="ded11-146">hello problem with this example is that hello comments array is **unbounded**, meaning that there is no (practical) limit toohello number of comments any single post can have.</span></span> <span data-ttu-id="ded11-147">To začnou problém, protože velikost hello hello dokumentu může výrazně zvýší.</span><span class="sxs-lookup"><span data-stu-id="ded11-147">This will become a problem as hello size of hello document could grow significantly.</span></span>

<span data-ttu-id="ded11-148">Jako hello velikost hello dokumentu zvětšování hello možnost tootransmit hello data přes přenosu hello, jakož i čtení a aktualizaci hello dokumentu ve velkém měřítku, bude mít vliv na.</span><span class="sxs-lookup"><span data-stu-id="ded11-148">As hello size of hello document grows hello ability tootransmit hello data over hello wire as well as reading and updating hello document, at scale, will be impacted.</span></span>

<span data-ttu-id="ded11-149">V takovém případě je lepší tooconsider hello následující modelu.</span><span class="sxs-lookup"><span data-stu-id="ded11-149">In this case it would be better tooconsider hello following model.</span></span>

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

<span data-ttu-id="ded11-150">Tento model má hello tři nejnovější komentáře vložených na hello post sebe, což je pole s pevnou hranice to čas.</span><span class="sxs-lookup"><span data-stu-id="ded11-150">This model has hello three most recent comments embedded on hello post itself, which is an array with a fixed bound this time.</span></span> <span data-ttu-id="ded11-151">Hello jiných komentáře jsou seskupené do toobatches 100 komentáře a uložené v samostatné dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ded11-151">hello other comments are grouped in toobatches of 100 comments and stored in separate documents.</span></span> <span data-ttu-id="ded11-152">Hello velikost dávky hello jste vybrali jako 100, protože naše fiktivní aplikace umožňuje hello 100 komentáře tooload uživatelů najednou.</span><span class="sxs-lookup"><span data-stu-id="ded11-152">hello size of hello batch was chosen as 100 because our fictitious application allows hello user tooload 100 comments at a time.</span></span>  

<span data-ttu-id="ded11-153">Jiné případu, kdy vnoření dat není vhodné je při hello vložených dat se často používá na dokumentech a se často mění.</span><span class="sxs-lookup"><span data-stu-id="ded11-153">Another case where embedding data is not a good idea is when hello embedded data is used often across documents and will change frequently.</span></span> 

<span data-ttu-id="ded11-154">Trvat tohoto fragmentu kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="ded11-154">Take this JSON snippet.</span></span>

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

<span data-ttu-id="ded11-155">To by mohl představovat uložených portfolií osoby.</span><span class="sxs-lookup"><span data-stu-id="ded11-155">This could represent a person's stock portfolio.</span></span> <span data-ttu-id="ded11-156">V dokumentu portfolií tooeach jsme vybrali tooembed hello uložené informace.</span><span class="sxs-lookup"><span data-stu-id="ded11-156">We have chosen tooembed hello stock information in tooeach portfolio document.</span></span> <span data-ttu-id="ded11-157">V prostředí, kde související data mění často, jako je populace obchodních aplikací vkládání dat, která se často mění přechází toomean pokaždé, když se prodávají stock jsou neustále aktualizuje každý dokument portfolií.</span><span class="sxs-lookup"><span data-stu-id="ded11-157">In an environment where related data is changing frequently, like a stock trading application, embedding data that changes frequently is going toomean that you are constantly updating each portfolio document every time a stock is traded.</span></span>

<span data-ttu-id="ded11-158">Stock *zaza* může být prodávají mnoho stokrát v jediném den a tisíce uživatelů může mít *zaza* na jejich portfolií.</span><span class="sxs-lookup"><span data-stu-id="ded11-158">Stock *zaza* may be traded many hundreds of times in a single day and thousands of users could have *zaza* on their portfolio.</span></span> <span data-ttu-id="ded11-159">S datovým modelem jako hello výše nám tooupdate mnoho tisíc portfolií dokumentů mnohokrát každý den, počáteční tooa systém, který nebude velmi dobře škálování.</span><span class="sxs-lookup"><span data-stu-id="ded11-159">With a data model like hello above we would have tooupdate many thousands of portfolio documents many times every day leading tooa system that won't scale very well.</span></span> 

## <span data-ttu-id="ded11-160"><a id="Refer"></a>Odkazy na data</span><span class="sxs-lookup"><span data-stu-id="ded11-160"><a id="Refer"></a>Referencing data</span></span>
<span data-ttu-id="ded11-161">Ano vkládání dat vhodně funguje u mnoha případech ale je zřejmé, že existují scénáře při denormalizing data způsobí, že další problémy, než je vhodné.</span><span class="sxs-lookup"><span data-stu-id="ded11-161">So, embedding data works nicely for many cases but it is clear that there are scenarios when denormalizing your data will cause more problems than it is worth.</span></span> <span data-ttu-id="ded11-162">Proto co můžeme udělat teď?</span><span class="sxs-lookup"><span data-stu-id="ded11-162">So what do we do now?</span></span> 

<span data-ttu-id="ded11-163">Relační databáze nejsou hello jediným místem, kde můžete vytvořit relace mezi entitami.</span><span class="sxs-lookup"><span data-stu-id="ded11-163">Relational databases are not hello only place where you can create relationships between entities.</span></span> <span data-ttu-id="ded11-164">V databázi dokumentů může mít informace v jednom z těchto dokumentů ve skutečnosti související s toodata v jiné dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ded11-164">In a document database you can have information in one document that actually relates toodata in other documents.</span></span> <span data-ttu-id="ded11-165">Nyní I není zasazovat o i jednu minutu, využijeme systémy, které by bylo lepší vhodná tooa relační databáze v Azure Cosmos DB nebo jakékoli jiné databáze dokumentu, ale jsou jednoduché vztahy a může být velmi užitečná.</span><span class="sxs-lookup"><span data-stu-id="ded11-165">Now, I am not advocating for even one minute that we build systems that would be better suited tooa relational database in Azure Cosmos DB, or any other document database, but simple relationships are fine and can be very useful.</span></span> 

<span data-ttu-id="ded11-166">V hello JSON níže jsme zvolili toouse hello ukázka uložených portfolia z dříve ale tentokrát označujeme toohello skladová položka na hello portfolií místo vkládání.</span><span class="sxs-lookup"><span data-stu-id="ded11-166">In hello JSON below we chose toouse hello example of a stock portfolio from earlier but this time we refer toohello stock item on hello portfolio instead of embedding it.</span></span> <span data-ttu-id="ded11-167">Tímto způsobem, při skladová položka hello často mění v rámci hello den hello pouze dokument, který musí aktualizovat toobe je hello jeden uložených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="ded11-167">This way, when hello stock item changes frequently throughout hello day hello only document that needs toobe updated is hello single stock document.</span></span> 

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


<span data-ttu-id="ded11-168">Přístup toothis okamžitou nevýhodou ale je, pokud je vaše aplikace vyžaduje tooshow informace o každé populace, která se nachází při zobrazení osoby portfolií; v takovém případě musíte toomake více cest toohello tooload hello informace o databázi pro každý dokument uložené.</span><span class="sxs-lookup"><span data-stu-id="ded11-168">An immediate downside toothis approach though is if your application is required tooshow information about each stock that is held when displaying a person's portfolio; in this case you would need toomake multiple trips toohello database tooload hello information for each stock document.</span></span> <span data-ttu-id="ded11-169">Zde jsme provedli efektivitu rozhodnutí tooimprove hello operací zápisu, které dojít mnohokrát hello den, ale pak dojde k ohrožení na hello čtení operací, které můžou mít menší dopad na výkon hello tohoto konkrétního systému.</span><span class="sxs-lookup"><span data-stu-id="ded11-169">Here we've made a decision tooimprove hello efficiency of write operations, which happen frequently throughout hello day, but in turn compromised on hello read operations that potentially have less impact on hello performance of this particular system.</span></span>

> [!NOTE]
> <span data-ttu-id="ded11-170">Normalized datové modely **může vyžadovat další odezev** toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="ded11-170">Normalized data models **can require more round trips** toohello server.</span></span>
> 
> 

### <a name="what-about-foreign-keys"></a><span data-ttu-id="ded11-171">Co cizí klíče?</span><span class="sxs-lookup"><span data-stu-id="ded11-171">What about foreign keys?</span></span>
<span data-ttu-id="ded11-172">Vzhledem k tomu, že aktuálně neexistuje žádná koncepce omezení, cizí klíč nebo jinak, všechny vztahy mezi dokumentu, které máte v dokumentech jsou efektivně "slabé odkazy" a nebude ověřit pomocí samotná databáze hello.</span><span class="sxs-lookup"><span data-stu-id="ded11-172">Because there is currently no concept of a constraint, foreign-key or otherwise, any inter-document relationships that you have in documents are effectively "weak links" and will not be verified by hello database itself.</span></span> <span data-ttu-id="ded11-173">Pokud chcete, aby tooensure, který hello data, která odkazuje na dokument tooactually existuje, pak tuto funkci potřebujete toodo ve vaší aplikaci, nebo prostřednictvím hello serverové aktivační události nebo uložené procedury v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ded11-173">If you want tooensure that hello data a document is referring tooactually exists, then you need toodo this in your application, or through hello use of server-side triggers or stored procedures on Azure Cosmos DB.</span></span>

### <a name="when-tooreference"></a><span data-ttu-id="ded11-174">Když tooreference</span><span class="sxs-lookup"><span data-stu-id="ded11-174">When tooreference</span></span>
<span data-ttu-id="ded11-175">Normalizovaný data se obvykle používá při modelů:</span><span class="sxs-lookup"><span data-stu-id="ded11-175">In general, use normalized data models when:</span></span>

* <span data-ttu-id="ded11-176">Představující **na více** vztahy.</span><span class="sxs-lookup"><span data-stu-id="ded11-176">Representing **one-to-many** relationships.</span></span>
* <span data-ttu-id="ded11-177">Představující **m: n** vztahy.</span><span class="sxs-lookup"><span data-stu-id="ded11-177">Representing **many-to-many** relationships.</span></span>
* <span data-ttu-id="ded11-178">Související data **často mění**.</span><span class="sxs-lookup"><span data-stu-id="ded11-178">Related data **changes frequently**.</span></span>
* <span data-ttu-id="ded11-179">Odkazovaná dat může být **bez vazby**.</span><span class="sxs-lookup"><span data-stu-id="ded11-179">Referenced data could be **unbounded**.</span></span>

> [!NOTE]
> <span data-ttu-id="ded11-180">Obvykle normalizace poskytuje lepší **zápisu** výkonu.</span><span class="sxs-lookup"><span data-stu-id="ded11-180">Typically normalizing provides better **write** performance.</span></span>
> 
> 

### <a name="where-do-i-put-hello-relationship"></a><span data-ttu-id="ded11-181">Kam mohu dát hello relace?</span><span class="sxs-lookup"><span data-stu-id="ded11-181">Where do I put hello relationship?</span></span>
<span data-ttu-id="ded11-182">Hello růstu hello relace vám pomohou určit v které odkaz na dokument toostore hello.</span><span class="sxs-lookup"><span data-stu-id="ded11-182">hello growth of hello relationship will help determine in which document toostore hello reference.</span></span>

<span data-ttu-id="ded11-183">Pokud se podíváme na hello JSON modelů vydavatele a knih.</span><span class="sxs-lookup"><span data-stu-id="ded11-183">If we look at hello JSON below that models publishers and books.</span></span>

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

<span data-ttu-id="ded11-184">Pokud je číslo hello hello knih na vydavatele s omezenou růstu malé, může být užitečné pak ukládání hello kniha odkaz uvnitř hello vydavatele dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ded11-184">If hello number of hello books per publisher is small with limited growth, then storing hello book reference inside hello publisher document may be useful.</span></span> <span data-ttu-id="ded11-185">Ale pokud je číslo hello knih na vydavatele bez vazby, pak tento datový model nezkopírujete, dojde toomutable, ročně zvýší pole, jako hello příklad vydavatele dokumentu výše.</span><span class="sxs-lookup"><span data-stu-id="ded11-185">However, if hello number of books per publisher is unbounded, then this data model would lead toomutable, growing arrays, as in hello example publisher document above.</span></span> 

<span data-ttu-id="ded11-186">Přepínání věcí kolem chvilku by výsledek v modelu stále představuje ale nyní hello stejná data zabraňuje těchto velkých měnitelný kolekcí.</span><span class="sxs-lookup"><span data-stu-id="ded11-186">Switching things around a bit would result in a model that still represents hello same data but now avoids these large mutable collections.</span></span>

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

<span data-ttu-id="ded11-187">V hello výše příkladu jsme vyřadit hello kolekce bez vazby na hello vydavatele dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ded11-187">In hello above example, we have dropped hello unbounded collection on hello publisher document.</span></span> <span data-ttu-id="ded11-188">Místo toho jsme právě mít vydavatel toohello odkaz na každý dokument adresáře.</span><span class="sxs-lookup"><span data-stu-id="ded11-188">Instead we just have a a reference toohello publisher on each book document.</span></span>

### <a name="how-do-i-model-manymany-relationships"></a><span data-ttu-id="ded11-189">Jak modelu relace m: n?</span><span class="sxs-lookup"><span data-stu-id="ded11-189">How do I model many:many relationships?</span></span>
<span data-ttu-id="ded11-190">V relační databázi *m: n* vztahy jsou často modelován s tabulkami spojení, které právě spojení záznamů z jiných tabulek.</span><span class="sxs-lookup"><span data-stu-id="ded11-190">In a relational database *many:many* relationships are often modeled with join tables, which just join records from other tables together.</span></span> 

![Spojování tabulek](./media/documentdb-modeling-data/join-table.png)

<span data-ttu-id="ded11-192">Můžete mít tendenci tooreplicate hello stejnou věc pomocí dokumenty a vytvoření datového modelu, který vypadá podobně jako následující toohello.</span><span class="sxs-lookup"><span data-stu-id="ded11-192">You might be tempted tooreplicate hello same thing using documents and produce a data model that looks similar toohello following.</span></span>

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

<span data-ttu-id="ded11-193">To by fungovat.</span><span class="sxs-lookup"><span data-stu-id="ded11-193">This would work.</span></span> <span data-ttu-id="ded11-194">Ale načítání buď Autor s jejich seznamů nebo načtení knihy s jeho autor by vždy vyžadují alespoň dva další dotazy na databázi hello.</span><span class="sxs-lookup"><span data-stu-id="ded11-194">However, loading either an author with their books, or loading a book with its author, would always require at least two additional queries against hello database.</span></span> <span data-ttu-id="ded11-195">Propojení dokumentu a pak další dotaz toofetch hello skutečné dokumentu je připojený k toohello jeden dotaz.</span><span class="sxs-lookup"><span data-stu-id="ded11-195">One query toohello joining document and then another query toofetch hello actual document being joined.</span></span> 

<span data-ttu-id="ded11-196">Pokud všechny, které se provádí v této tabulce spojení je připevnění společně dva kusy dat, pak Proč ne ho můžete vypustit úplně?</span><span class="sxs-lookup"><span data-stu-id="ded11-196">If all this join table is doing is gluing together two pieces of data, then why not drop it completely?</span></span>
<span data-ttu-id="ded11-197">Vezměte v úvahu následující hello.</span><span class="sxs-lookup"><span data-stu-id="ded11-197">Consider hello following.</span></span>

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents: 
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in tooAzure Cosmos DB", "authors": ["a2"]}

<span data-ttu-id="ded11-198">Nyní I měli Autor, okamžitě vím knihy, které uživatel vytvořil a naopak Pokud I měli načíst kniha dokumentu by vím hello ID autory hello.</span><span class="sxs-lookup"><span data-stu-id="ded11-198">Now, if I had an author, I immediately know which books they have written, and conversely if I had a book document loaded I would know hello ids of hello author(s).</span></span> <span data-ttu-id="ded11-199">To umožňuje ušetřit tento zprostředkující dotaz na tabulku spojení hello snížení počtu hello serveru odezev toomake má vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="ded11-199">This saves that intermediary query against hello join table reducing hello number of server round trips your application has toomake.</span></span> 

## <span data-ttu-id="ded11-200"><a id="WrapUp"></a>Hybridní datové modely</span><span class="sxs-lookup"><span data-stu-id="ded11-200"><a id="WrapUp"></a>Hybrid data models</span></span>
<span data-ttu-id="ded11-201">Jste nyní hledá vložení (nebo denormalizing) a odkazující (nebo normalizace) dat, mají jejich upsides a mají ohrožení jak jsme mohli vidět.</span><span class="sxs-lookup"><span data-stu-id="ded11-201">We've now looked embedding (or denormalizing) and referencing (or normalizing) data, each have their upsides and each have compromises as we have seen.</span></span> 

<span data-ttu-id="ded11-202">Ji není vždy máte toobe buď nebo, nemusíte být Vystrašené toomix věcí trochu nahoru.</span><span class="sxs-lookup"><span data-stu-id="ded11-202">It doesn't always have toobe either or, don't be scared toomix things up a little.</span></span> 

<span data-ttu-id="ded11-203">Na základě konkrétní vzorce a úloh, které můžou nastat případy, kdy kombinování vložených vaší aplikace a smysl využívaných dat a může realizace toosimpler aplikační logiku s méně serveru zaokrouhlit služebních cest současně se zachovává funkční úroveň výkonu .</span><span class="sxs-lookup"><span data-stu-id="ded11-203">Based on your application's specific usage patterns and workloads there may be cases where mixing embedded and referenced data makes sense and could lead toosimpler application logic with fewer server round trips while still maintaining a good level of performance.</span></span>

<span data-ttu-id="ded11-204">Vezměte v úvahu následující JSON hello.</span><span class="sxs-lookup"><span data-stu-id="ded11-204">Consider hello following JSON.</span></span> 

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

<span data-ttu-id="ded11-205">Zde jsme jste (většinou) a potom hello vložený model, kdy data z jiné entity jsou vloženy do nejvyšší úrovně dokumentu hello, ale je odkazován další data.</span><span class="sxs-lookup"><span data-stu-id="ded11-205">Here we've (mostly) followed hello embedded model, where data from other entities are embedded in hello top-level document, but other data is referenced.</span></span> 

<span data-ttu-id="ded11-206">Pokud se podíváte na dokument hello adresáře, uvidíme několik zajímavé pole, když se podíváme na pole hello autorů.</span><span class="sxs-lookup"><span data-stu-id="ded11-206">If you look at hello book document, we can see a few interesting fields when we look at hello array of authors.</span></span> <span data-ttu-id="ded11-207">Došlo *id* pole, která je v poli hello používáme také toorefer back tooan Autor dokumentu, běžnou praxí ve model normalizovaný, ale pak jsme *název* a *thumbnailUrl*.</span><span class="sxs-lookup"><span data-stu-id="ded11-207">There is an *id* field which is hello field we use toorefer back tooan author document, standard practice in a normalized model, but then we also have *name* and *thumbnailUrl*.</span></span> <span data-ttu-id="ded11-208">Jsme může s právě jsme *id* a levé tooget aplikace hello jakýchkoli dalších informací ho nepotřebují z hello příslušných Autor dokumentu s použitím hello "link", ale protože naše aplikace zobrazí jméno autora hello a miniatur obrázků s každou kniha zobrazí jsme uložit odezvy serveru toohello za adresáře v seznamu denormalizing **některé** data od autora hello.</span><span class="sxs-lookup"><span data-stu-id="ded11-208">We could've just stuck with *id* and left hello application tooget any additional information it needed from hello respective author document using hello "link", but because our application displays hello author's name and a thumbnail picture with every book displayed we can save a round trip toohello server per book in a list by denormalizing **some** data from hello author.</span></span>

<span data-ttu-id="ded11-209">Opravdu, pokud změníte jméno autora hello nebo jejich fotografii požadují tooupdate nám toogo aktualizace každých adresáře se někdy publikované ale pro naši aplikaci založené na hello předpokládá, že autoři neměnit jejich názvy velmi často, to je přijatelné návrhu rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="ded11-209">Sure, if hello author's name changed or they wanted tooupdate their photo we'd have toogo an update every book they ever published but for our application, based on hello assumption that authors don't change their names very often, this is an acceptable design decision.</span></span>  

<span data-ttu-id="ded11-210">V příkladu hello existují **předem vypočítat agregace** hodnoty toosave nákladné zpracování na operace čtení.</span><span class="sxs-lookup"><span data-stu-id="ded11-210">In hello example there are **pre-calculated aggregates** values toosave expensive processing on a read operation.</span></span> <span data-ttu-id="ded11-211">V příkladu hello některá data hello vložených v hello Autor dokumentu jsou data, která se vypočítává za běhu.</span><span class="sxs-lookup"><span data-stu-id="ded11-211">In hello example, some of hello data embedded in hello author document is data that is calculated at run-time.</span></span> <span data-ttu-id="ded11-212">Pokaždé, když je publikována nového seznamu, se vytvoří dokument adresáře **a** hello countOfBooks pole je nastavena hodnota tooa počítá na základě počtu hello dokumentů knihy, která platí pro konkrétní autora.</span><span class="sxs-lookup"><span data-stu-id="ded11-212">Every time a new book is published, a book document is created **and** hello countOfBooks field is set tooa calculated value based on hello number of book documents that exist for a particular author.</span></span> <span data-ttu-id="ded11-213">Tato optimalizace by dobré v systémech čtení velkou kde toodo výpočty můžete dovolit na zápis v pořadí toooptimize čtení.</span><span class="sxs-lookup"><span data-stu-id="ded11-213">This optimization would be good in read heavy systems where we can afford toodo computations on writes in order toooptimize reads.</span></span>

<span data-ttu-id="ded11-214">Hello možnost toohave model s předem počítaných polí je možné, protože nepodporuje Azure Cosmos DB **transakcí několika dokumentů**.</span><span class="sxs-lookup"><span data-stu-id="ded11-214">hello ability toohave a model with pre-calculated fields is made possible because Azure Cosmos DB supports **multi-document transactions**.</span></span> <span data-ttu-id="ded11-215">Mnoho NoSQL úložiště nelze provést transakce mezi dokumenty a proto doporučují, jako je například "vždy Vložit vše", z důvodu omezení toothis rozhodnutí o návrhu.</span><span class="sxs-lookup"><span data-stu-id="ded11-215">Many NoSQL stores cannot do transactions across documents and therefore advocate design decisions, such as "always embed everything", due toothis limitation.</span></span> <span data-ttu-id="ded11-216">V Azure DB Cosmos můžete použít aktivační události na straně serveru nebo uložené procedury, které vložit knihy a aktualizovat autoři všechny v transakci ACID.</span><span class="sxs-lookup"><span data-stu-id="ded11-216">With Azure Cosmos DB, you can use server-side triggers, or stored procedures, that insert books and update authors all within an ACID transaction.</span></span> <span data-ttu-id="ded11-217">Teď je prostě **mít** tooembed všechno ve tooone dokumentu jenom toobe jistotu, že data zůstávají konzistentní.</span><span class="sxs-lookup"><span data-stu-id="ded11-217">Now you don't **have** tooembed everything in tooone document just toobe sure that your data remains consistent.</span></span>

## <span data-ttu-id="ded11-218"><a name="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="ded11-218"><a name="NextSteps"></a>Next steps</span></span>
<span data-ttu-id="ded11-219">Hello největších takeaways z tohoto článku je toounderstand, modelování ve světě bez schémat dat je stejně důležité jako někdy.</span><span class="sxs-lookup"><span data-stu-id="ded11-219">hello biggest takeaways from this article is toounderstand that data modeling in a schema-free world is just as important as ever.</span></span> 

<span data-ttu-id="ded11-220">Stejně jako neexistuje žádný jediný způsob toorepresent část dat na obrazovce, neexistuje žádný jediný způsob toomodel vaše data.</span><span class="sxs-lookup"><span data-stu-id="ded11-220">Just as there is no single way toorepresent a piece of data on a screen, there is no single way toomodel your data.</span></span> <span data-ttu-id="ded11-221">Potřebujete toounderstand vaší aplikace a jak ji vytvoří, spotřebovat a zpracovat hello data.</span><span class="sxs-lookup"><span data-stu-id="ded11-221">You need toounderstand your application and how it will produce, consume, and process hello data.</span></span> <span data-ttu-id="ded11-222">Pak použitím některé hello můžete nastavit zde uvedené pokyny o vytváření model, který řeší hello okamžitou potřebám vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ded11-222">Then, by applying some of hello guidelines presented here you can set about creating a model that addresses hello immediate needs of your application.</span></span> <span data-ttu-id="ded11-223">Pokud vaše aplikace potřebují toochange, můžete využít flexibilitu hello tooembrace bez schémat databáze, změňte a snadno vyvíjet datový model.</span><span class="sxs-lookup"><span data-stu-id="ded11-223">When your applications need toochange, you can leverage hello flexibility of a schema-free database tooembrace that change and evolve your data model easily.</span></span> 

<span data-ttu-id="ded11-224">toolearn Další informace o databázi Cosmos Azure, najdete v toohello služby [dokumentace](https://azure.microsoft.com/documentation/services/cosmos-db/) stránky.</span><span class="sxs-lookup"><span data-stu-id="ded11-224">toolearn more about Azure Cosmos DB, refer toohello service's [documentation](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span> 

<span data-ttu-id="ded11-225">toounderstand jak tooshard data napříč více oddílů, najdete v příliš[dělení dat v Azure Cosmos DB](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="ded11-225">toounderstand how tooshard your data across multiple partitions, refer too[Partitioning Data in Azure Cosmos DB](documentdb-partition-data.md).</span></span> 
