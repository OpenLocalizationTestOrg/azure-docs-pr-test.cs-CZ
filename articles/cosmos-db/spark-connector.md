---
title: Apache Spark tooAzure aaaConnecting Cosmos DB | Microsoft Docs
description: "Použijte tento kurz toolearn o konektoru hello Azure Cosmos DB Spark, která vám umožní tooconnect Apache Spark tooAzure Cosmos DB tooperform distribuované agregací a věd dat v systému globálně distribuované databáze víceklientské hello od společnosti Microsoft který je určený pro hello cloud."
keywords: Apache spark
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: c4f46007-2606-4273-ab16-29d0e15c0736
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: denlee
ms.openlocfilehash: 70b496fc5ca8f65675f0224e749637f5d533c346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accelerate-real-time-big-data-analytics-with-hello-spark-tooazure-cosmos-db-connector"></a>Urychlit analýzy velkých objemů dat v reálném čase s konektorem Cosmos DB tooAzure Spark hello

Hello Spark tooAzure Cosmos DB konektor umožňuje tooact Azure Cosmos DB jako vstupní zdroj nebo výstupní jímku pro úlohy Apache Spark. Připojení [Spark](http://spark.apache.org/) příliš[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) zrychluje vaše možnost toosolve přesun fast vědecké zpracování dat problémy, kde můžete použít Azure Cosmos DB tooquickly zachovat a zadávat dotazy na data. Hello Spark tooAzure Cosmos DB konektor efektivně využívá nativní indexů Azure DB Cosmos spravované hello. indexy Hello povolit aktualizovatelné sloupce, pokud provádět analýzy a nabízená dolů predikát filtrování fast změna globálně distribuované data, která v rozsahu od vědy a analýzy toodata scénáře Internetu věcí (IoT).

Práce s Spark, GraphX a hello Gremlin grafu rozhraní API databázi Cosmos Azure, najdete v části [provádět analýzy grafu pomocí Spark a Apache TinkerPop Gremlin](spark-connector-graph.md).

## <a name="download"></a>Ke stažení

spuštění tooget, stahovat hello Spark tooAzure Cosmos DB Connectoru (preview) z hello [azure. cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/) úložišti na Githubu.

## <a name="connector-components"></a>Konektor součásti

konektor Hello využívá hello následující součásti:

* [Azure Cosmos DB](http://documentdb.com) umožňuje zákazníkům tooprovision a Elasticky škálovat propustnost a úložiště napříč libovolný počet zeměpisné oblasti. Hello nabídky služeb:
   * Aktivujte klíč [globální distribuční](distribute-data-globally.md) a horizontální škálování
   * Zaručit latence jediná číslice v 99th percentilu hello
   * [Více dobře definovaný konzistence modelů](consistency-levels.md)
   * Zaručit vysoká dostupnost s více funkci Možnosti
   * Všechny funkce jsou zajišťované špičkový, komplexní [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLA).

* [Apache Spark](http://spark.apache.org/) modul zpracování výkonné s otevřeným zdrojem, který je vytvořena na rychlost, snadné použití a sofistikované analytics.

* [Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) tak, aby Apache Spark v cloudu hello důležité nasazení můžete nasadit pomocí [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).

Oficiálně podporované verze:

| Komponenta | Verze |
|---------|-------|
|Apache Spark|2.0+|
| Scala| 2.11|
| Azure DocumentDB Java SDK | 1.10.0 |

Tento článek vám pomůže spustit některé jednoduché ukázky pomocí Python (prostřednictvím pydocumentdb v tuto) a hello Scala rozhraní.

Existují dva přístupy tooconnect Apache Spark a Cosmos databázi Azure:
- Použít pydocumentdb v tuto prostřednictvím hello [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).
- Vytvořit konektor Cosmos DB tooAzure založené na jazyce Java Spark s využitím hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).

## <a name="pydocumentdb-implementation"></a>pydocumentdb v tuto implementaci
Hello aktuální [pydocumentdb v tuto sadu SDK](https://github.com/Azure/azure-documentdb-python) umožní vám tooconnect Spark tooAzure Cosmos DB, jak je znázorněno v následujícím diagramu hello:

![Spark tooAzure Cosmos DB tok dat prostřednictvím pydocumentdb v tuto DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-hello-pydocumentdb-implementation"></a>Tok dat hello pydocumentdb v tuto implementaci

tok dat Hello vypadá takto:

1. Hello Spark hlavní uzel připojí uzel brány Azure Cosmos DB toohello prostřednictvím pydocumentdb v tuto. Uživatel Určuje pouze hello Spark a Azure Cosmos DB připojení. Uzly toohello připojení příslušné hlavní a brány jsou transparentní toohello uživatele.
2. uzel brány Hello díky hello dotaz na databázi Cosmos Azure, kde hello dotazu následně spouští hello kolekce oddílů v hello datové uzly. Hello odpověď pro tyto dotazy je odeslána zpět, uzel brány toohello a že sadu výsledků dotazu je vrácen toohello Spark hlavní uzel.
3. Následující dotazy (například pro Spark DataFrame) jsou odesílány toohello Spark pracovním uzlům pro zpracování.

Komunikace mezi Spark a Azure Cosmos DB je omezený toohello hlavní uzel Spark a uzly brány Azure Cosmos DB.  dotazy Hello přejděte tak rychle, jak umožňuje hello transportní vrstva mezi těmito dvěma uzly.

### <a name="install-pydocumentdb"></a>Nainstalujte pydocumentdb v tuto
Pydocumentdb v tuto vašeho uzlu ovladačů můžete nainstalovat pomocí **pip**, například:

```
pip install pyDocumentDB
```


### <a name="connect-spark-tooazure-cosmos-db-via-pydocumentdb"></a>Připojit Spark tooAzure Cosmos DB prostřednictvím pydocumentdb v tuto
jednoduchost Hello hello komunikace přenosu umožňuje spuštění dotazu z Spark tooAzure Cosmos DB pomocí poměrně jednoduché pydocumentdb v tuto.

Následující kód fragment kódu ukazuje, jak Hello toouse pydocumentdb v tuto v kontextu Spark.

```
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring hello connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]


# Set keys tooconnect tooAzure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

Jak jsme uvedli v hello fragment kódu:

* Hello Azure Cosmos DB Python SDK (`pyDocumentDB`) obsahuje hello všechny hello parametrů nezbytné připojení. Například hello upřednostňovaná, že umístění parametr rozhodne, že hello číst repliky a s prioritou pořadí.
*  Importovat knihovny potřebné hello a konfigurace vaší **hlavního klíče** a **hostitele** toocreate hello Azure Cosmos DB *klienta* (**pydocumentdb.document_ Klient**).


### <a name="execute-spark-queries-via-pydocumentdb"></a>Spuštění dotazů Spark prostřednictvím pydocumentdb v tuto
Následující příklady použití hello Azure Cosmos DB instanci, která byla vytvořena v předchozím fragmentu kódu hello pomocí hello Hello zadané klíče jen pro čtení. Hello následující fragment kódu připojí toohello **airports.codes** kolekce v účtu DoctorWho hello jako dříve zadaný a spustí na dotaz tooextract hello letiště města ve státě Washington.

```
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations hello Azure Cosmos DB client will use tooconnect toohello database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Push into list `elements`
elements = list(query)
```

Po provedení dotazu hello prostřednictvím **dotazu**, hello výsledkem je, **query_iterable. QueryIterable** tedy převedený tooa Python seznamu. Seznam Python lze snadno převedený tooa Spark DataFrame pomocí hello následující kód:

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-hello-pydocumentdb-tooconnect-spark-tooazure-cosmos-db"></a>Proč používat hello pydocumentdb v tuto tooconnect Spark tooAzure Cosmos DB?
Připojení Spark tooAzure DB Cosmos pomocí pydocumentdb v tuto je obvykle pro scénáře, kde:

* Chcete toouse Python.
* Jsou vrací poměrně malý výslednou sadu z tooSpark Azure Cosmos DB. Všimněte si, že hello podkladové datové sady v Azure Cosmos DB mohou být značně velké. Jsou použití filtrů, tedy spuštěným filtry predikátů pro váš zdroj Azure Cosmos DB.  

## <a name="spark-tooazure-cosmos-db-connector"></a>Spark tooAzure Cosmos DB konektoru

Hello Spark tooAzure Cosmos DB konektor využívá hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) a přesouvá data mezi uzly pracovního procesu hello Spark a Azure Cosmos DB, jak je znázorněno v následujícím diagramu hello:

![Tok dat v hello Spark tooAzure Cosmos DB konektoru](./media/spark-connector/spark-connector.png)

tok dat Hello vypadá takto:

1. Hello Spark hlavní uzel připojí toohello Azure Cosmos DB brány uzlu tooobtain hello mapa oddílu. Uživatel Určuje pouze hello Spark a Azure Cosmos DB připojení. Uzly toohello připojení příslušné hlavní a brány jsou transparentní toohello uživatele.
2. Tyto informace jsou poskytovány back toohello Spark hlavní uzel.  V tomto okamžiku byste měli mít tooparse hello dotazu toodetermine hello oddílů a jejich umístění v Azure DB Cosmos, je nutné, aby tooaccess.
3. Tyto informace jsou přenášená toohello Spark pracovním uzlům.
4. Hello Spark pracovním uzlům připojit toohello Azure Cosmos DB oddíly přímo tooextract hello data a vrátí hello data toohello oddíly Spark v hello Spark pracovním uzlům.

Komunikace mezi Spark a Azure Cosmos DB je výrazně rychlejší, protože je hello přesun dat mezi uzly pracovního procesu hello Spark a hello Azure Cosmos DB datové uzly (oddíly).

### <a name="build-hello-spark-tooazure-cosmos-db-connector"></a>Sestavení hello Spark tooAzure Cosmos DB konektoru
V současné době používá hello konektor projekt maven. konektor hello toobuild bez závislosti, můžete spustit:
```
mvn clean package
```
Můžete si taky stáhnout nejnovější verze hello JAR hello z hello *uvolní* složky.

### <a name="include-hello-azure-cosmos-db-spark-jar"></a>Zahrnout hello Azure Cosmos DB Spark JAR
Než spustíte všechny kód, je třeba tooinclude hello Azure Cosmos DB Spark JAR.  Pokud používáte hello **spark prostředí**, potom můžete zahrnout hello JAR pomocí hello **– JAR** možnost.  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

Pokud chcete, aby tooexecute hello JAR bez závislostí, použijte hello následující kód:

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

Pokud používáte služby Poznámkový blok, jako je služba poznámkového bloku Azure HDInsight Jupyter, můžete použít hello **spark magic** příkazy:

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

Hello **JAR** příkaz umožňuje vám tooinclude hello dva JARs, které jsou potřebné pro **azure. cosmosdb spark** (samostatně a hello Azure DocumentDB Java SDK) a vyloučit **scala-odráží** tak, že nebudou v konfliktu s hello volá Livy (Poznámkový blok Jupyter > Livy > Spark).

### <a name="connect-spark-tooazure-cosmos-db-using-hello-connector"></a>Připojit Spark pomocí Cosmos DB tooAzure hello konektoru
I když hello komunikace přenosu je trochu složitější, provádění dotazu z Spark tooAzure DB Cosmos pomocí konektoru hello je výrazně rychlejší.

Hello následující fragment kódu ukazuje, jak toouse hello konektor v kontextu Spark.

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection tooyour collection
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Jak jsme uvedli v hello fragment kódu:

- **Azure-cosmosdb-spark** obsahuje hello všechny hello parametrů potřeby připojení, mezi které patří hello preferované umístění. Můžete například, že hello číst repliky a s prioritou pořadí.
- Právě importovat hello potřebné knihovny a nakonfigurujte masterKey a hostitele Azure Cosmos DB klienta hello toocreate.

### <a name="execute-spark-queries-via-hello-connector"></a>Spuštění dotazů Spark prostřednictvím konektoru hello

Následující příklad používá hello Azure Cosmos DB instance, který byl vytvořen v předchozím fragmentu kódu hello pomocí hello Hello zadané klíče jen pro čtení. Hello následující fragment kódu připojí toohello DepartureDelays.flights_pcoll kolekce (v hello DoctorWho účtu uvedeného výše) a spustí dotaz tooextract hello zpoždění informace o letu letů, které jsou jednotlivé Praha.

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-hello-spark-tooazure-cosmos-db-connector-implementation"></a>Proč používat hello Spark tooAzure Cosmos DB konektor implementace?

Připojení Spark tooAzure DB Cosmos pomocí konektoru hello je obvykle pro scénáře, kde:

* Chcete toouse Scala a aktualizovat tooinclude obálku Python, jak jsme uvedli v [problém 3: Přidání Python obálku a příklady](https://github.com/Azure/azure-cosmosdb-spark/issues/3).
* Máte velké množství dat tootransfer mezi Apache Spark a Azure Cosmos DB.

toogive představu o hello rozdíl ve výkonu dotazů, uvidíte hello [dotazu testovací spouští wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="distributed-aggregation-example"></a>Příklad distribuované agregace
Tato část obsahuje příklady jak to distribuované agregací a analýzy pomocí Apache Spark a Azure Cosmos DB společně. Azure Cosmos DB již podporuje agregace, která je popsána v hello [planetu agregace škálování s Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/). Zde je, jak můžete si ho toohello další úrovně s Apache Spark.

Všimněte si, že tyto agregace v odkazu toohello [Spark tooAzure konektor DB Cosmos poznámkového bloku](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).

### <a name="connect-tooflights-sample-data"></a>Připojit tooflights ukázková data
Tyto příklady agregace přístup k cestě výkonu data, která je uložena v našem **DoctorWho** databáze Azure Cosmos DB. tooconnect tooit, je třeba tooutilize hello následující fragment kódu:

```
// Import Spark tooAzure Cosmos DB connector
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Connect tooAzure Cosmos DB Database
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US 2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Tento fragment kódu jsme jsou také probíhající toorun základní dotazu, že přenosy hello filtrované sady dat z Azure Cosmos DB tooSpark kde hello pozdější můžete provést distribuované agregace. V takovém případě vás žádáme pro lety, které se liší od Praha (SEA).

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

Hello následující výsledky byly vygenerovány spuštěním hello dotazy z hello služby poznámkového bloku Jupyter.  Upozorňujeme, že jsou všechny fragmenty kódu hello obecné a nejsou specifické tooany služby.

### <a name="running-limit-and-count-queries"></a>LIMIT spuštěné a počet dotazů
Právě, jako je třeba to, co se používá tooin SQL nebo Spark SQL, můžeme začít s **LIMIT** dotazu:

![Spark LIMIT dotazu](./media/spark-connector/spark-sql-query.png)

Hello další dotaz je jednoduchý a rychlé **počet** dotazu:

![Dotazu Spark COUNT](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a>GROUP BY dotazu
V této sadě další můžeme jednoduše spustit **GROUP BY** dotazy pro Azure Cosmos DB databáze:

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY dotazu grafu.](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a>JEDINEČNÉ, ORDER BY dotazu
A tady je **DISTINCT, ORDER BY** dotazu:

![Spark GROUP BY dotazu grafu.](./media/spark-connector/order-by-query.png)

### <a name="continue-hello-flight-data-analysis"></a>Analýza dat letu hello pokračovat
Můžete použít následující příklad dotazy toocontinue analýzy dat letu hello hello:

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a>TOP 5 Zpožděné cíle (města) jednotlivé Praha
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark nejvyšší zpoždění grafu](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a>Vypočítat střední zpoždění podle měst cílové jednotlivé Praha
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark střední zpoždění grafu](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a>Další kroky

Pokud jste tak ještě neučinili, stáhněte hello Spark tooAzure Cosmos DB konektor z hello [azure. cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) úložiště GitHub a seznamte se s hello další prostředky v úložišti hello:

* [Příklady distribuované agregace](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Ukázkové skripty a poznámkových bloků](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

Můžete také tooreview hello [Apache Spark SQL, průvodce datové sady a DataFrames](http://spark.apache.org/docs/latest/sql-programming-guide.html) a hello [Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) článku.
