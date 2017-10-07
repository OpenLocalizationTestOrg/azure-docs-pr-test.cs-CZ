---
title: "aaaCreate databázi Azure Cosmos DB graf s Javou | Microsoft Docs"
description: "Uvede ukázka tooconnect tooand dotazu grafu data můžete použít v Azure DB Cosmos pomocí Gremlin kódu Java."
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/24/2017
ms.author: denlee
ms.openlocfilehash: 595c0fb108f3dbe8c83674f0c9c4b0cdd3ab4c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-graph-database-using-java-and-hello-azure-portal"></a>Azure Cosmos DB: Vytvoření databáze grafu pomocí Java a hello portálu Azure

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento rychlý start vytvoří graf databázi pomocí hello nástroje Azure portálu pro Azure Cosmos DB. Tento rychlý start také ukazuje, jak vytvořit tooquickly konzolovou aplikaci Java pomocí graf databázi pomocí hello OSS [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) ovladačů. Hello pokyny v tento rychlý start platí pro všechny operační systémy, které podporují Javu. Tento rychlý start vás seznámí s vytvoření a úprava prostředků grafu v hello uživatelského rozhraní nebo prostřednictvím kódu programu, podle toho, co je vaši volbu. 

## <a name="prerequisites"></a>Požadavky

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Ubuntu, spusťte `apt-get install default-jdk` tooinstall hello JDK.
    * Být jisti tooset hello JAVA_HOME prostředí proměnné toopoint toohello složka nainstalovanou hello JDK.
* [Stáhněte](http://maven.apache.org/download.cgi) a [nainstalujte](http://maven.apache.org/install.html) binární archiv [Maven](http://maven.apache.org/).
    * Na Ubuntu, můžete spustit `apt-get install maven` tooinstall Maven.
* [Git](https://www.git-scm.com/)
    * Na Ubuntu, můžete spustit `sudo apt-get install git` tooinstall Git.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

Před vytvořením grafu databáze, musíte toocreate databázového účtu Gremlin (grafu) s Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Přidání grafu

Teď můžete použít nástroj Průzkumník dat hello v hello Azure portálu toocreate databázi grafu. 

1. V hello portál Azure, v nabídce hello navigaci vlevo, klikněte na **Data Explorer (Preview)**. 
2. V hello **Data Explorer (Preview)** okně klikněte na tlačítko **nový graf**, potom vyplňte stránku hello pomocí hello následující informace:

    ![Průzkumník dat v hello portálu Azure](./media/create-graph-java/azure-cosmosdb-data-explorer.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID databáze|sample-database|Hello ID pro novou databázi. Názvy databází musí mít délku 1 až 255 znaků a nesmí obsahovat znaky `/ \ # ?` ani koncové mezery.
    ID grafu|sample-graph|Hello ID pro nový graf. Názvy grafu mají hello znak stejné požadavky jako ID databáze.
    Kapacita úložiště| 10 GB|Ponechte výchozí hodnotu hello. Toto je kapacita úložiště hello hello databáze.
    Propustnost|400 RU/s|Ponechte výchozí hodnotu hello. Je možné škálovat nahoru propustnost hello později Pokud chcete, aby tooreduce latence.
    Klíč oddílu|Ponechte prázdné|Za účelem hello tento rychlý start nezadáte klíč oddílu hello.

3. Jakmile vyplňování formuláře hello, klikněte na možnost **OK**.

## <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

Nyní Pojďme klonovat grafu aplikace z githubu, nastavte hello připojovací řetězec a spusťte ho. Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.  

2. Spusťte následující příkaz tooclone hello Ukázka úložiště hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-hello-code"></a>Zkontrolujte hello kódu

Provedeme jejich stručný přehled o dění v aplikaci hello. Otevřete hello `Program.java` soubor ze složky \src\GetStarted hello a najděte tyto řádky kódu. 

* Hello Gremlin `Client` je inicializován ze hello konfigurace v `src/remote.yaml`.

    ```java
    cluster = Cluster.build(new File("src/remote.yaml")).create();
    ...
    client = cluster.connect();
    ```

* Sérii kroků Gremlin jsou spouštěny pomocí hello `client.submit` metoda.

    ```java
    ResultSet results = client.submit(gremlin);

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

1. Otevřete hello src/remote.yaml soubor. 

3. Vyplňte vaše *hostitele*, *uživatelské jméno*, a *heslo* hodnot v souboru src/remote.yaml hello. Hello zbytek hello nastavení nemusí toobe změnit.

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    Hostitelé|[***.graphs.azure.com]|Podívejte se na snímek obrazovky hello za touto tabulkou. Tato hodnota je hodnota identifikátoru URI Gremlin hello na stránce Přehled hello hello portál Azure, v hranatých závorkách s koncové hello: 443 / odebrané.<br><br>Tuto hodnotu můžete také načíst z karty hello klíče, hodnota identifikátoru URI hello pomocí odebrání https://, změna toographs dokumenty a odebrání hello koncové: 443 /.
    Uživatelské jméno|/dbs/sample-database/colls/sample-graph|Hello prostředků hello formuláře `/dbs/<db>/colls/<coll>` kde `<db>` je název vaší existující databáze a `<coll>` je název vaší existující kolekci.
    Heslo|*Primární hlavní klíč*|Podívejte se na hello druhý snímek obrazovky za touto tabulkou. Tato hodnota je primární klíč, který může načíst ze stránky klíče hello hello portál Azure, v poli hello primární klíč. Zkopírujte hodnotu hello pomocí hello tlačítko Kopírovat na pravé straně hello hello pole.

    Pro hodnotu hello hostitele, zkopírujte hello **Gremlin URI** hodnotu z hello **přehled** stránky. Pokud je prázdná, najdete v pokynech hello v řádku hello hostitelů v předcházející tabulce o vytváření hello Gremlin URI z okna klíče hello hello.
![Zobrazení a zkopírování hodnota identifikátoru URI Gremlin hello na stránce Přehled hello hello portálu Azure](./media/create-graph-java/gremlin-uri.png)

    Pro hello hodnota hesla, zkopírujte hello **primární klíč** z hello **klíče** okno: ![zobrazení a zkopírování klíče vaší primární klíč v hello portál Azure, stránka](./media/create-graph-java/keys.png)

## <a name="run-hello-console-app"></a>Spusťte konzolovou aplikaci hello

1. V okně terminálu hello git `cd` toohello azure-cosmos-db-graph-java-getting-started složky.

2. Zadejte v okně terminálu hello git, `mvn package` tooinstall hello požadované balíčky jazyka Java.

3. Okno terminálu hello git, spusťte `mvn exec:java -D exec.mainClass=GetStarted.Program` hello okno terminálu toostart aplikace v jazyce Java.

Zobrazí okno terminálu Hello hello vrcholy přidávané toohello grafu. Po dokončení programu hello přepněte zpět toohello portálu Azure v internetovém prohlížeči. 

<a id="add-sample-data"></a>
## <a name="review-and-add-sample-data"></a>Kontrola a přidání ukázkových dat

Teď můžete přejít zpět tooData Průzkumníka a vrcholy hello přidat graf toohello a přidat další datové body.

1. V Průzkumníku dat rozbalte hello **ukázkové databáze**/**Ukázka grafu**, klikněte na tlačítko **grafu**a pak klikněte na tlačítko **použít filtr**. 

   ![Vytváření nových dokumentů v Průzkumníku dat v hello portálu Azure](./media/create-graph-java/azure-cosmosdb-data-explorer-expanded.png)

2. V hello **výsledky** seznamu si všimněte, přidat nové uživatele hello toohello grafu. Vyberte **ben** a Všimněte si, že mu byl připojen toorobin. Můžete pohyb hello vrcholy na Průzkumníka hello grafu, přiblížení a oddálení a rozbalit hello velikost hello grafu explorer prostor. 

   ![Nové vrcholy v grafu hello v Průzkumníku dat v hello portálu Azure](./media/create-graph-java/azure-cosmosdb-graph-explorer-new.png)

3. Umožňuje přidat pár nový graf toohello uživatele pomocí Průzkumníku dat hello. Klikněte na tlačítko hello **nový vrchol** tlačítko tooadd data tooyour grafu.

   ![Vytváření nových dokumentů v Průzkumníku dat v hello portálu Azure](./media/create-graph-java/azure-cosmosdb-data-explorer-new-vertex.png)

4. Zadejte popisek z *osoba* zadejte hello následující klíče a hodnoty toocreate hello první vrchol v grafu hello. Všimněte si, že pro každou osobu v grafu můžete vytvořit jedinečné vlastnosti. Je vyžadován pouze klíč id hello.

    key|hodnota|Poznámky
    ----|----|----
    id|ashley|Hello jedinečný identifikátor pro vrchol hello. Pokud identifikátor nezadáte, vygeneruje se pro vás.
    gender (pohlaví)|female (žena)| 
    tech (technologie) | java | 

    > [!NOTE]
    > V tomto rychlém startu vytváříme kolekci bez oddílů. Ale pokud vytvoříte tak, že zadáte klíč oddílu během vytváření kolekce hello dělenou kolekci, budete potřebovat klíč oddílu hello tooinclude jako klíč v každé nové vrchol. 

5. Klikněte na **OK**. Může být nutné tooexpand vaše obrazovky toosee **OK** na konci hello úvodní obrazovka.

6. Znovu klikněte na **Nový vrchol** a přidejte dalšího nového uživatele. Zadejte popisek z *osoba* zadejte hello následující klíče a hodnoty:

    key|hodnota|Poznámky
    ----|----|----
    id|rakesh|Hello jedinečný identifikátor pro vrchol hello. Pokud identifikátor nezadáte, vygeneruje se pro vás.
    gender (pohlaví)|male (muž)| 
    school (škola)|MIT| 

7. Klikněte na **OK**. 

8. Klikněte na tlačítko **použít filtr** hello výchozí `g.V()` filtru. Všichni uživatelé hello nyní zobrazit ve hello **výsledky** seznamu. Při přidávání více dat, můžete použít filtry toolimit výsledky. Ve výchozím Průzkumníku dat používá `g.V()` tooretrieve všechny vrcholy grafu, ale můžete změnit této tooa jiný [grafu dotazu](tutorial-query-graph.md), například `g.V().count()`, tooreturn počet všechny vrcholy hello v grafu hello ve formátu JSON.

9. Teď můžeme propojit uživatele rakesh a ashley. Ujistěte se, **pracovník Novák** ve vybraných v hello **výsledky** seznamu a pak klikněte na tlačítko Upravit hello vedle příliš**cíle** na pravé straně nižší. Může být nutné toowiden vaše hello toosee okno **vlastnosti** oblasti.

   ![Změnit cíl hello vrcholu v grafu.](./media/create-graph-java/azure-cosmosdb-data-explorer-edit-target.png)

10. V hello **cíl** zadejte *rakesh*a v hello **Edge popisek** zadejte *zná*a potom klikněte na zaškrtávací políčko hello.

   ![Přidání propojení mezi uživateli ashley a rakesh v Průzkumníku dat](./media/create-graph-java/azure-cosmosdb-data-explorer-set-target.png)

11. Nyní vyberte **rakesh** z seznam výsledků hello a zjistit, zda pracovník Novák a rakesh propojeni. 

   ![Dva propojené vrcholy v Průzkumníku dat](./media/create-graph-java/azure-cosmosdb-graph-explorer.png)

    Můžete také použít Průzkumníku dat toocreate uložené procedury, funkce UDF a aktivační události tooperform serverovou obchodní logiku také jako propustnost škálování. Průzkumník dat zpřístupní všechny hello předdefinované programový přístup k datům v hello rozhraní API k dispozici, ale poskytuje snadný přístup k datům tooyour v hello portálu Azure.



## <a name="review-slas-in-hello-azure-portal"></a>Zkontrolujte SLA v hello portálu Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky: 

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tento rychlý start když jste se naučili toocreate účet Azure Cosmos DB vytvoření grafu pomocí hello Průzkumníku dat a spusťte aplikaci. Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů. 

> [!div class="nextstepaction"]
> [Dotazování pomocí konzoly Gremlin](tutorial-query-graph.md)

