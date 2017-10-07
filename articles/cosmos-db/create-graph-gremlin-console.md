---
title: "Kurz ke službě Azure Cosmos DB: Vytváření, zadávání dotazů a procházení v konzole Apache TinkerPops Gremlin | Dokumentace Microsoftu"
description: "Databázi Cosmos Azure rychlý start toocreates vrcholy, okraje a dotazy pomocí hello Azure Cosmos DB Graph API."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: terminal
ms.topic: hero-article
ms.date: 07/27/2017
ms.author: denlee
ms.openlocfilehash: 9de64c97fec89c45cecba9e14214db472ec76f57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-query-and-traverse-a-graph-in-hello-gremlin-console"></a>Azure Cosmos DB: Vytvoření dotazu a procházení graf v konzole Gremlin hello

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento rychlý start předvádí, jak toocreate účet Azure Cosmos DB, databáze a graf (kontejner) pomocí hello portál Azure a pak použijte hello [Gremlin konzoly](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) z [Apache TinkerPop](http://tinkerpop.apache.org) toowork s Data grafu rozhraní API (preview). V tomto kurzu vytvoříte a dotaz vrcholy a hranami, aktualizaci vlastnosti vrchol dotaz vrcholy, procházení hello grafu a vyřadit vrchol.

![Azure DB Cosmos z konzoly Apache Gremlin hello](./media/create-graph-gremlin-console/gremlin-console.png)

Hello Gremlin konzoly je Groovy nebo Java a běží na systému Linux, Mac a Windows. Můžete ji stáhnout z hello [Apache TinkerPop lokality](https://www.apache.org/dyn/closer.lua/tinkerpop/3.2.5/apache-tinkerpop-gremlin-console-3.2.5-bin.zip).

## <a name="prerequisites"></a>Požadavky

Pro tento rychlý start potřebovat toohave předplatnému Azure toocreate účet Azure Cosmos DB.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Musíte taky tooinstall hello [Gremlin konzoly](http://tinkerpop.apache.org/). Použijte verzi 3.2.5 nebo vyšší.

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Přidání grafu

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a id="ConnectAppService"></a>Připojit tooyour aplikační služby
1. Před zahájením hello Gremlin konzoly, vytvoří nebo upraví hello vzdálené secure.yaml konfigurační soubor v adresáři apache-tinkerpop-gremlin-console-3.2.5/conf hello.
2. Vyplňte parametry *Hostitel*, *Port*, *Uživatelské jméno*, *Heslo*, *Fond připojení* a *Serializátor*:

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    hostitelé|[***.graphs.azure.com]|Viz snímek obrazovky níže. Toto je hodnota identifikátoru URI Gremlin hello na stránce Přehled hello hello portál Azure, v hranatých závorkách s koncové hello: 443 / odebrané.<br><br>Tuto hodnotu můžete také načíst z karty hello klíče, hodnota identifikátoru URI hello pomocí odebrání https://, změna toographs dokumenty a odebrání hello koncové: 443 /.
    port|443|Nastavit too443.
    uživatelské jméno|*Vaše uživatelské jméno*|Hello prostředků hello formuláře `/dbs/<db>/colls/<coll>` kde `<db>` je název databáze a `<coll>` je název vaší kolekce.
    heslo|*Váš primární klíč*| Viz druhý snímek obrazovky níže. Toto je primární klíč, který může načíst ze stránky klíče hello hello portál Azure, v poli hello primární klíč. Použijte tlačítko Kopírovat hello na levé straně hello hello pole toocopy hello hodnoty.
    fond připojení|{enableSsl: true}|Nastavení fondu připojení pro protokol SSL.
    serializátor|{ className: org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Nastavte hodnotu toothis a odstraní všechny `\n` konce řádků při vkládání v hodnotě hello.

    Pro hodnotu hello hostitele, zkopírujte hello **Gremlin URI** hodnotu z hello **přehled** stránky: ![zobrazení a zkopírování hodnota identifikátoru URI Gremlin hello na stránce Přehled hello hello portálu Azure](./media/create-graph-gremlin-console/gremlin-uri.png)

    Hodnota hesla hello, zkopírujte hello **primární klíč** z hello **klíče** stránky: ![zobrazení a zkopírování klíče vaší primární klíč v hello portál Azure, stránka](./media/create-graph-gremlin-console/keys.png)


3. V terminálu, spusťte `bin/gremlin.bat` nebo `bin/gremlin.sh` toostart hello [Gremlin konzoly](http://tinkerpop.apache.org/docs/3.2.5/tutorials/getting-started/).
4. V terminálu, spusťte `:remote connect tinkerpop.server conf/remote-secure.yaml` tooconnect tooyour aplikační služby.

    > [!TIP]
    > Pokud se zobrazí chyba hello `No appenders could be found for logger` zajistěte aktualizovala hello serializátor hodnota hello secure.yaml vzdáleného souboru, jak je popsáno v kroku 2. 

Výborně! Teď když jsme dokončili hello instalace, Začněme spuštění některých příkazů konzoly.

Vyzkoušejme jednoduchý příkaz count(). Zadejte následující hello do konzoly hello hello příkazového řádku:
```
:> g.V().count()
```

> [!TIP]
> Všimněte si hello `:>` který předchází hello `g.V().count()` text? 
>
> Toto je část příkazu hello, které že budete potřebovat tootype. Je důležité při použití konzole Gremlin hello s Azure Cosmos DB.  
>
> Vynechání to `:>` předponu dá pokyn, příkaz hello tooexecute hello konzoly lokálně, často proti grafu v paměti.
> Pomocí této `:>` informuje hello konzoly tooexecute vzdáleného příkazu, v takovém případě proti Cosmos DB (buď hello localhost emulátoru, nebo > Azure instance).


## <a name="create-vertices-and-edges"></a>Vytváření vrcholů a okrajů

Začněme přidáním pěti osob pro nastavení vrcholů: *Tomáš*, *Marie*, *Robin*, *Petr* a *Jan*.

Vstup (Tomáš):

```
:> g.addV('person').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44).property('userid', 1)
```

Výstup:

```
==>[id:796cdccc-2acd-4e58-a324-91d6f6f5ed6d,label:person,type:vertex,properties:[firstName:[[id:f02a749f-b67c-4016-850e-910242d68953,value:Thomas]],lastName:[[id:f5fa3126-8818-4fda-88b0-9bb55145ce5c,value:Andersen]],age:[[id:f6390f9c-e563-433e-acbf-25627628016e,value:44]],userid:[[id:796cdccc-2acd-4e58-a324-91d6f6f5ed6d|userid,value:1]]]]
```
Vstup (Marie):

```
:> g.addV('person').property('firstName', 'Mary Kay').property('lastName', 'Andersen').property('age', 39).property('userid', 2)

```

Výstup:

```
==>[id:0ac9be25-a476-4a30-8da8-e79f0119ea5e,label:person,type:vertex,properties:[firstName:[[id:ea0604f8-14ee-4513-a48a-1734a1f28dc0,value:Mary Kay]],lastName:[[id:86d3bba5-fd60-4856-9396-c195ef7d7f4b,value:Andersen]],age:[[id:bc81b78d-30c4-4e03-8f40-50f72eb5f6da,value:39]],userid:[[id:0ac9be25-a476-4a30-8da8-e79f0119ea5e|userid,value:2]]]]

```

Vstup (Robin):

```
:> g.addV('person').property('firstName', 'Robin').property('lastName', 'Wakefield').property('userid', 3)
```

Výstup:

```
==>[id:8dc14d6a-8683-4a54-8d74-7eef1fb43a3e,label:person,type:vertex,properties:[firstName:[[id:ec65f078-7a43-4cbe-bc06-e50f2640dc4e,value:Robin]],lastName:[[id:a3937d07-0e88-45d3-a442-26fcdfb042ce,value:Wakefield]],userid:[[id:8dc14d6a-8683-4a54-8d74-7eef1fb43a3e|userid,value:3]]]]
```

Vstup (Petr):

```
:> g.addV('person').property('firstName', 'Ben').property('lastName', 'Miller').property('userid', 4)

```

Výstup:

```
==>[id:ee86b670-4d24-4966-9a39-30529284b66f,label:person,type:vertex,properties:[firstName:[[id:a632469b-30fc-4157-840c-b80260871e9a,value:Ben]],lastName:[[id:4a08d307-0719-47c6-84ae-1b0b06630928,value:Miller]],userid:[[id:ee86b670-4d24-4966-9a39-30529284b66f|userid,value:4]]]]
```

Vstup (Jan):

```
:> g.addV('person').property('firstName', 'Jack').property('lastName', 'Connor').property('userid', 5)
```

Výstup:

```
==>[id:4c835f2a-ea5b-43bb-9b6b-215488ad8469,label:person,type:vertex,properties:[firstName:[[id:4250824e-4b72-417f-af98-8034aa15559f,value:Jack]],lastName:[[id:44c1d5e1-a831-480a-bf94-5167d133549e,value:Connor]],userid:[[id:4c835f2a-ea5b-43bb-9b6b-215488ad8469|userid,value:5]]]]
```


V dalším kroku přidejme pro vztahy mezi osobami okraje.

Vstup (Tomáš -> Marie):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Mary Kay'))
```

Výstup:

```
==>[id:c12bf9fb-96a1-4cb7-a3f8-431e196e702f,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:0d1fa428-780c-49a5-bd3a-a68d96391d5c,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

Vstup (Tomáš -> Robin):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Robin'))
```

Výstup:

```
==>[id:58319bdd-1d3e-4f17-a106-0ddf18719d15,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:3e324073-ccfc-4ae1-8675-d450858ca116,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

Vstup (Robin -> Petr):

```
:> g.V().hasLabel('person').has('firstName', 'Robin').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Ben'))
```

Výstup:

```
==>[id:889c4d3c-549e-4d35-bc21-a3d1bfa11e00,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:40fd641d-546e-412a-abcc-58fe53891aab,outV:3e324073-ccfc-4ae1-8675-d450858ca116]
```

## <a name="update-a-vertex"></a>Aktualizace vrcholu

Umožňuje aktualizovat hello *Thomas* vrchol s novou stáří *45*.

Vstup:
```
:> g.V().hasLabel('person').has('firstName', 'Thomas').property('age', 45)
```
Výstup:

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

## <a name="query-your-graph"></a>Dotazování grafu

Teď spustíme pro graf celou řadu dotazů.

Nejprve nyní si vyzkoušíte dotazu s jenom lidé filtru tooreturn, kteří jsou starší než 40 let.

Vstup (dotaz s filtrem):

```
:> g.V().hasLabel('person').has('age', gt(40))
```

Výstup:

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

Dále umožňuje projektu hello křestní jméno pro hello lidí, kteří jsou starší než 40 let.

Vstup (filtru + dotaz projekce):

```
:> g.V().hasLabel('person').has('age', gt(40)).values('firstName')
```

Výstup:

```
==>Thomas
```

## <a name="traverse-your-graph"></a>Procházení grafu

Umožňuje procházet hello grafu tooreturn všechny na blogu Thomase přátel.

Vstup (přátelé uživatele Tomáš):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person')
```

Výstup: 

```
==>[id:f04bc00b-cb56-46c4-a3bb-a5870c42f7ff,label:person,type:vertex,properties:[firstName:[[id:14feedec-b070-444e-b544-62be15c7167c,value:Mary Kay]],lastName:[[id:107ab421-7208-45d4-b969-bbc54481992a,value:Andersen]],age:[[id:4b08d6e4-58f5-45df-8e69-6b790b692e0a,value:39]]]]
==>[id:91605c63-4988-4b60-9a30-5144719ae326,label:person,type:vertex,properties:[firstName:[[id:f760e0e6-652a-481a-92b0-1767d9bf372e,value:Robin]],lastName:[[id:352a4caa-bad6-47e3-a7dc-90ff342cf870,value:Wakefield]]]]
```

V dalším kroku Pojďme hello další vrstva vrcholy. Procházení všech hello přátelích přátel, na blogu Thomase tooreturn hello grafu.

Vstup (přátelé přátel uživatele Tomáš):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```
Výstup:

```
==>[id:a801a0cb-ee85-44ee-a502-271685ef212e,label:person,type:vertex,properties:[firstName:[[id:b9489902-d29a-4673-8c09-c2b3fe7f8b94,value:Ben]],lastName:[[id:e084f933-9a4b-4dbc-8273-f0171265cf1d,value:Miller]]]]
```

## <a name="drop-a-vertex"></a>Vyřazení vrcholu

Nyní odstranit umožňuje vrchol z databáze hello grafu.

Vstup (vyřazení vrcholu Jan):

```
:> g.V().hasLabel('person').has('firstName', 'Jack').drop()
```

## <a name="clear-your-graph"></a>Resetování grafu

Umožňuje vymazat nakonec databáze hello všechny vrcholy a okrajů.

Vstup:

```
:> g.E().drop()
:> g.V().drop()
```

Blahopřejeme! Dokončili jste tento kurz rozhraní Graph API služby Azure Cosmos DB!

## <a name="review-slas-in-hello-azure-portal"></a>Zkontrolujte SLA v hello portálu Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:  

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tento rychlý start když jste se naučili jak toocreate účet Azure Cosmos DB vytvoření grafu pomocí hello Průzkumníku dat, vytvořte vrcholy a okrajů a procházení grafu pomocí konzoly Gremlin hello. Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů. 

> [!div class="nextstepaction"]
> [Dotazování pomocí konzoly Gremlin](tutorial-query-graph.md)
