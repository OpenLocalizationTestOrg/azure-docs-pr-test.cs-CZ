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
# <a name="accelerate-real-time-big-data-analytics-with-hello-spark-tooazure-cosmos-db-connector"></a><span data-ttu-id="d9054-104">Urychlit analýzy velkých objemů dat v reálném čase s konektorem Cosmos DB tooAzure Spark hello</span><span class="sxs-lookup"><span data-stu-id="d9054-104">Accelerate real-time big-data analytics with hello Spark tooAzure Cosmos DB connector</span></span>

<span data-ttu-id="d9054-105">Hello Spark tooAzure Cosmos DB konektor umožňuje tooact Azure Cosmos DB jako vstupní zdroj nebo výstupní jímku pro úlohy Apache Spark.</span><span class="sxs-lookup"><span data-stu-id="d9054-105">hello Spark tooAzure Cosmos DB connector enables Azure Cosmos DB tooact as an input source or output sink for Apache Spark jobs.</span></span> <span data-ttu-id="d9054-106">Připojení [Spark](http://spark.apache.org/) příliš[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) zrychluje vaše možnost toosolve přesun fast vědecké zpracování dat problémy, kde můžete použít Azure Cosmos DB tooquickly zachovat a zadávat dotazy na data.</span><span class="sxs-lookup"><span data-stu-id="d9054-106">Connecting [Spark](http://spark.apache.org/) too[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) accelerates your ability toosolve fast-moving data science problems where you can use Azure Cosmos DB tooquickly persist and query data.</span></span> <span data-ttu-id="d9054-107">Hello Spark tooAzure Cosmos DB konektor efektivně využívá nativní indexů Azure DB Cosmos spravované hello.</span><span class="sxs-lookup"><span data-stu-id="d9054-107">hello Spark tooAzure Cosmos DB connector efficiently utilizes hello native Azure Cosmos DB managed indexes.</span></span> <span data-ttu-id="d9054-108">indexy Hello povolit aktualizovatelné sloupce, pokud provádět analýzy a nabízená dolů predikát filtrování fast změna globálně distribuované data, která v rozsahu od vědy a analýzy toodata scénáře Internetu věcí (IoT).</span><span class="sxs-lookup"><span data-stu-id="d9054-108">hello indexes enable updateable columns when you perform analytics and push-down predicate filtering against fast-changing globally distributed data, which range from Internet of Things (IoT) toodata science and analytics scenarios.</span></span>

<span data-ttu-id="d9054-109">Práce s Spark, GraphX a hello Gremlin grafu rozhraní API databázi Cosmos Azure, najdete v části [provádět analýzy grafu pomocí Spark a Apache TinkerPop Gremlin](spark-connector-graph.md).</span><span class="sxs-lookup"><span data-stu-id="d9054-109">For working with Spark GraphX and hello Gremlin graph APIs of Azure Cosmos DB, see [Perform graph analytics using Spark and Apache TinkerPop Gremlin](spark-connector-graph.md).</span></span>

## <a name="download"></a><span data-ttu-id="d9054-110">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="d9054-110">Download</span></span>

<span data-ttu-id="d9054-111">spuštění tooget, stahovat hello Spark tooAzure Cosmos DB Connectoru (preview) z hello [azure. cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/) úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="d9054-111">tooget started, download hello Spark tooAzure Cosmos DB connector (preview) from hello [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) repository on GitHub.</span></span>

## <a name="connector-components"></a><span data-ttu-id="d9054-112">Konektor součásti</span><span class="sxs-lookup"><span data-stu-id="d9054-112">Connector components</span></span>

<span data-ttu-id="d9054-113">konektor Hello využívá hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="d9054-113">hello connector utilizes hello following components:</span></span>

* <span data-ttu-id="d9054-114">[Azure Cosmos DB](http://documentdb.com) umožňuje zákazníkům tooprovision a Elasticky škálovat propustnost a úložiště napříč libovolný počet zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="d9054-114">[Azure Cosmos DB](http://documentdb.com) enables customers tooprovision and elastically scale both throughput and storage across any number of geographical regions.</span></span> <span data-ttu-id="d9054-115">Hello nabídky služeb:</span><span class="sxs-lookup"><span data-stu-id="d9054-115">hello service offers:</span></span>
   * <span data-ttu-id="d9054-116">Aktivujte klíč [globální distribuční](distribute-data-globally.md) a horizontální škálování</span><span class="sxs-lookup"><span data-stu-id="d9054-116">Turn key [global distribution](distribute-data-globally.md) and horizontal scale</span></span>
   * <span data-ttu-id="d9054-117">Zaručit latence jediná číslice v 99th percentilu hello</span><span class="sxs-lookup"><span data-stu-id="d9054-117">Guaranteed single digit latencies at hello 99th percentile</span></span>
   * [<span data-ttu-id="d9054-118">Více dobře definovaný konzistence modelů</span><span class="sxs-lookup"><span data-stu-id="d9054-118">Multiple well-defined consistency models</span></span>](consistency-levels.md)
   * <span data-ttu-id="d9054-119">Zaručit vysoká dostupnost s více funkci Možnosti</span><span class="sxs-lookup"><span data-stu-id="d9054-119">Guaranteed high availability with multi-homing capabilities</span></span>
   * <span data-ttu-id="d9054-120">Všechny funkce jsou zajišťované špičkový, komplexní [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLA).</span><span class="sxs-lookup"><span data-stu-id="d9054-120">All features are backed by industry-leading, comprehensive [service level agreements](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLAs).</span></span>

* <span data-ttu-id="d9054-121">[Apache Spark](http://spark.apache.org/) modul zpracování výkonné s otevřeným zdrojem, který je vytvořena na rychlost, snadné použití a sofistikované analytics.</span><span class="sxs-lookup"><span data-stu-id="d9054-121">[Apache Spark](http://spark.apache.org/) is a powerful open source processing engine that's built around speed, ease of use, and sophisticated analytics.</span></span>

* <span data-ttu-id="d9054-122">[Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) tak, aby Apache Spark v cloudu hello důležité nasazení můžete nasadit pomocí [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="d9054-122">[Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) so that you can deploy Apache Spark in hello cloud for mission-critical deployments by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="d9054-123">Oficiálně podporované verze:</span><span class="sxs-lookup"><span data-stu-id="d9054-123">Officially supported versions:</span></span>

| <span data-ttu-id="d9054-124">Komponenta</span><span class="sxs-lookup"><span data-stu-id="d9054-124">Component</span></span> | <span data-ttu-id="d9054-125">Verze</span><span class="sxs-lookup"><span data-stu-id="d9054-125">Version</span></span> |
|---------|-------|
|<span data-ttu-id="d9054-126">Apache Spark</span><span class="sxs-lookup"><span data-stu-id="d9054-126">Apache Spark</span></span>|<span data-ttu-id="d9054-127">2.0+</span><span class="sxs-lookup"><span data-stu-id="d9054-127">2.0+</span></span>|
| <span data-ttu-id="d9054-128">Scala</span><span class="sxs-lookup"><span data-stu-id="d9054-128">Scala</span></span>| <span data-ttu-id="d9054-129">2.11</span><span class="sxs-lookup"><span data-stu-id="d9054-129">2.11</span></span>|
| <span data-ttu-id="d9054-130">Azure DocumentDB Java SDK</span><span class="sxs-lookup"><span data-stu-id="d9054-130">Azure DocumentDB Java SDK</span></span> | <span data-ttu-id="d9054-131">1.10.0</span><span class="sxs-lookup"><span data-stu-id="d9054-131">1.10.0</span></span> |

<span data-ttu-id="d9054-132">Tento článek vám pomůže spustit některé jednoduché ukázky pomocí Python (prostřednictvím pydocumentdb v tuto) a hello Scala rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d9054-132">This article helps you run some simple samples by using Python (via pyDocumentDB) and hello Scala interfaces.</span></span>

<span data-ttu-id="d9054-133">Existují dva přístupy tooconnect Apache Spark a Cosmos databázi Azure:</span><span class="sxs-lookup"><span data-stu-id="d9054-133">There are two approaches tooconnect Apache Spark and Azure Cosmos DB:</span></span>
- <span data-ttu-id="d9054-134">Použít pydocumentdb v tuto prostřednictvím hello [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span><span class="sxs-lookup"><span data-stu-id="d9054-134">Use pyDocumentDB via hello [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span></span>
- <span data-ttu-id="d9054-135">Vytvořit konektor Cosmos DB tooAzure založené na jazyce Java Spark s využitím hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="d9054-135">Create a Java-based Spark tooAzure Cosmos DB connector by utilizing hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

## <a name="pydocumentdb-implementation"></a><span data-ttu-id="d9054-136">pydocumentdb v tuto implementaci</span><span class="sxs-lookup"><span data-stu-id="d9054-136">pyDocumentDB implementation</span></span>
<span data-ttu-id="d9054-137">Hello aktuální [pydocumentdb v tuto sadu SDK](https://github.com/Azure/azure-documentdb-python) umožní vám tooconnect Spark tooAzure Cosmos DB, jak je znázorněno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="d9054-137">hello current [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) enables you tooconnect Spark tooAzure Cosmos DB as shown in hello following diagram:</span></span>

![Spark tooAzure Cosmos DB tok dat prostřednictvím pydocumentdb v tuto DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-hello-pydocumentdb-implementation"></a><span data-ttu-id="d9054-139">Tok dat hello pydocumentdb v tuto implementaci</span><span class="sxs-lookup"><span data-stu-id="d9054-139">Data flow of hello pyDocumentDB implementation</span></span>

<span data-ttu-id="d9054-140">tok dat Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d9054-140">hello data flow is as follows:</span></span>

1. <span data-ttu-id="d9054-141">Hello Spark hlavní uzel připojí uzel brány Azure Cosmos DB toohello prostřednictvím pydocumentdb v tuto.</span><span class="sxs-lookup"><span data-stu-id="d9054-141">hello Spark master node connects toohello Azure Cosmos DB gateway node via pyDocumentDB.</span></span> <span data-ttu-id="d9054-142">Uživatel Určuje pouze hello Spark a Azure Cosmos DB připojení.</span><span class="sxs-lookup"><span data-stu-id="d9054-142">A user specifies only hello Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="d9054-143">Uzly toohello připojení příslušné hlavní a brány jsou transparentní toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="d9054-143">Connections toohello respective master and gateway nodes are transparent toohello user.</span></span>
2. <span data-ttu-id="d9054-144">uzel brány Hello díky hello dotaz na databázi Cosmos Azure, kde hello dotazu následně spouští hello kolekce oddílů v hello datové uzly.</span><span class="sxs-lookup"><span data-stu-id="d9054-144">hello gateway node makes hello query against Azure Cosmos DB where hello query subsequently runs against hello collection's partitions in hello data nodes.</span></span> <span data-ttu-id="d9054-145">Hello odpověď pro tyto dotazy je odeslána zpět, uzel brány toohello a že sadu výsledků dotazu je vrácen toohello Spark hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="d9054-145">hello response for those queries is sent back toohello gateway node, and that result set is returned toohello Spark master node.</span></span>
3. <span data-ttu-id="d9054-146">Následující dotazy (například pro Spark DataFrame) jsou odesílány toohello Spark pracovním uzlům pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="d9054-146">Subsequent queries (for example, against a Spark DataFrame) are sent toohello Spark worker nodes for processing.</span></span>

<span data-ttu-id="d9054-147">Komunikace mezi Spark a Azure Cosmos DB je omezený toohello hlavní uzel Spark a uzly brány Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d9054-147">Communication between Spark and Azure Cosmos DB is limited toohello Spark master node and Azure Cosmos DB gateway nodes.</span></span>  <span data-ttu-id="d9054-148">dotazy Hello přejděte tak rychle, jak umožňuje hello transportní vrstva mezi těmito dvěma uzly.</span><span class="sxs-lookup"><span data-stu-id="d9054-148">hello queries go as fast as hello transport layer between these two nodes allows.</span></span>

### <a name="install-pydocumentdb"></a><span data-ttu-id="d9054-149">Nainstalujte pydocumentdb v tuto</span><span class="sxs-lookup"><span data-stu-id="d9054-149">Install pyDocumentDB</span></span>
<span data-ttu-id="d9054-150">Pydocumentdb v tuto vašeho uzlu ovladačů můžete nainstalovat pomocí **pip**, například:</span><span class="sxs-lookup"><span data-stu-id="d9054-150">You can install pyDocumentDB on your driver node by using **pip**, for example:</span></span>

```
pip install pyDocumentDB
```


### <a name="connect-spark-tooazure-cosmos-db-via-pydocumentdb"></a><span data-ttu-id="d9054-151">Připojit Spark tooAzure Cosmos DB prostřednictvím pydocumentdb v tuto</span><span class="sxs-lookup"><span data-stu-id="d9054-151">Connect Spark tooAzure Cosmos DB via pyDocumentDB</span></span>
<span data-ttu-id="d9054-152">jednoduchost Hello hello komunikace přenosu umožňuje spuštění dotazu z Spark tooAzure Cosmos DB pomocí poměrně jednoduché pydocumentdb v tuto.</span><span class="sxs-lookup"><span data-stu-id="d9054-152">hello simplicity of hello communication transport makes execution of a query from Spark tooAzure Cosmos DB by using pyDocumentDB relatively simple.</span></span>

<span data-ttu-id="d9054-153">Následující kód fragment kódu ukazuje, jak Hello toouse pydocumentdb v tuto v kontextu Spark.</span><span class="sxs-lookup"><span data-stu-id="d9054-153">hello following code snippet shows how toouse pyDocumentDB in a Spark context.</span></span>

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

<span data-ttu-id="d9054-154">Jak jsme uvedli v hello fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="d9054-154">As noted in hello code snippet:</span></span>

* <span data-ttu-id="d9054-155">Hello Azure Cosmos DB Python SDK (`pyDocumentDB`) obsahuje hello všechny hello parametrů nezbytné připojení.</span><span class="sxs-lookup"><span data-stu-id="d9054-155">hello Azure Cosmos DB Python SDK (`pyDocumentDB`) contains hello all hello necessary connection parameters.</span></span> <span data-ttu-id="d9054-156">Například hello upřednostňovaná, že umístění parametr rozhodne, že hello číst repliky a s prioritou pořadí.</span><span class="sxs-lookup"><span data-stu-id="d9054-156">For example, hello preferred locations parameter chooses hello read replica and priority order.</span></span>
*  <span data-ttu-id="d9054-157">Importovat knihovny potřebné hello a konfigurace vaší **hlavního klíče** a **hostitele** toocreate hello Azure Cosmos DB *klienta* (**pydocumentdb.document_ Klient**).</span><span class="sxs-lookup"><span data-stu-id="d9054-157">Import hello necessary libraries and configure your **masterKey** and **host** toocreate hello Azure Cosmos DB *client* (**pydocumentdb.document_client**).</span></span>


### <a name="execute-spark-queries-via-pydocumentdb"></a><span data-ttu-id="d9054-158">Spuštění dotazů Spark prostřednictvím pydocumentdb v tuto</span><span class="sxs-lookup"><span data-stu-id="d9054-158">Execute Spark Queries via pyDocumentDB</span></span>
<span data-ttu-id="d9054-159">Následující příklady použití hello Azure Cosmos DB instanci, která byla vytvořena v předchozím fragmentu kódu hello pomocí hello Hello zadané klíče jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="d9054-159">hello following examples use hello Azure Cosmos DB instance that was created in hello previous snippet by using hello specified read-only keys.</span></span> <span data-ttu-id="d9054-160">Hello následující fragment kódu připojí toohello **airports.codes** kolekce v účtu DoctorWho hello jako dříve zadaný a spustí na dotaz tooextract hello letiště města ve státě Washington.</span><span class="sxs-lookup"><span data-stu-id="d9054-160">hello following code snippet connects toohello **airports.codes** collection in hello DoctorWho account as specified earlier and runs a query tooextract hello airport cities in Washington state.</span></span>

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

<span data-ttu-id="d9054-161">Po provedení dotazu hello prostřednictvím **dotazu**, hello výsledkem je, **query_iterable. QueryIterable** tedy převedený tooa Python seznamu.</span><span class="sxs-lookup"><span data-stu-id="d9054-161">After hello query has been executed via **query**, hello result is a **query_iterable.QueryIterable** that is converted tooa Python list.</span></span> <span data-ttu-id="d9054-162">Seznam Python lze snadno převedený tooa Spark DataFrame pomocí hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="d9054-162">A Python list can be easily converted tooa Spark DataFrame by using hello following code:</span></span>

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-hello-pydocumentdb-tooconnect-spark-tooazure-cosmos-db"></a><span data-ttu-id="d9054-163">Proč používat hello pydocumentdb v tuto tooconnect Spark tooAzure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="d9054-163">Why use hello pyDocumentDB tooconnect Spark tooAzure Cosmos DB?</span></span>
<span data-ttu-id="d9054-164">Připojení Spark tooAzure DB Cosmos pomocí pydocumentdb v tuto je obvykle pro scénáře, kde:</span><span class="sxs-lookup"><span data-stu-id="d9054-164">Connecting Spark tooAzure Cosmos DB by using pyDocumentDB is typically for scenarios where:</span></span>

* <span data-ttu-id="d9054-165">Chcete toouse Python.</span><span class="sxs-lookup"><span data-stu-id="d9054-165">You want toouse Python.</span></span>
* <span data-ttu-id="d9054-166">Jsou vrací poměrně malý výslednou sadu z tooSpark Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d9054-166">You are returning a relatively small result set from Azure Cosmos DB tooSpark.</span></span> <span data-ttu-id="d9054-167">Všimněte si, že hello podkladové datové sady v Azure Cosmos DB mohou být značně velké.</span><span class="sxs-lookup"><span data-stu-id="d9054-167">Note that hello underlying dataset in Azure Cosmos DB can be quite large.</span></span> <span data-ttu-id="d9054-168">Jsou použití filtrů, tedy spuštěným filtry predikátů pro váš zdroj Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d9054-168">You are applying filters, that is, running predicate filters, against your Azure Cosmos DB source.</span></span>  

## <a name="spark-tooazure-cosmos-db-connector"></a><span data-ttu-id="d9054-169">Spark tooAzure Cosmos DB konektoru</span><span class="sxs-lookup"><span data-stu-id="d9054-169">Spark tooAzure Cosmos DB connector</span></span>

<span data-ttu-id="d9054-170">Hello Spark tooAzure Cosmos DB konektor využívá hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) a přesouvá data mezi uzly pracovního procesu hello Spark a Azure Cosmos DB, jak je znázorněno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="d9054-170">hello Spark tooAzure Cosmos DB connector utilizes hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) and moves data between hello Spark worker nodes and Azure Cosmos DB as shown in hello following diagram:</span></span>

![Tok dat v hello Spark tooAzure Cosmos DB konektoru](./media/spark-connector/spark-connector.png)

<span data-ttu-id="d9054-172">tok dat Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d9054-172">hello data flow is as follows:</span></span>

1. <span data-ttu-id="d9054-173">Hello Spark hlavní uzel připojí toohello Azure Cosmos DB brány uzlu tooobtain hello mapa oddílu.</span><span class="sxs-lookup"><span data-stu-id="d9054-173">hello Spark master node connects toohello Azure Cosmos DB gateway node tooobtain hello partition map.</span></span> <span data-ttu-id="d9054-174">Uživatel Určuje pouze hello Spark a Azure Cosmos DB připojení.</span><span class="sxs-lookup"><span data-stu-id="d9054-174">A user specifies only hello Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="d9054-175">Uzly toohello připojení příslušné hlavní a brány jsou transparentní toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="d9054-175">Connections toohello respective master and gateway nodes are transparent toohello user.</span></span>
2. <span data-ttu-id="d9054-176">Tyto informace jsou poskytovány back toohello Spark hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="d9054-176">This information is provided back toohello Spark master node.</span></span>  <span data-ttu-id="d9054-177">V tomto okamžiku byste měli mít tooparse hello dotazu toodetermine hello oddílů a jejich umístění v Azure DB Cosmos, je nutné, aby tooaccess.</span><span class="sxs-lookup"><span data-stu-id="d9054-177">At this point, you should be able tooparse hello query toodetermine hello partitions and their locations in Azure Cosmos DB that you need tooaccess.</span></span>
3. <span data-ttu-id="d9054-178">Tyto informace jsou přenášená toohello Spark pracovním uzlům.</span><span class="sxs-lookup"><span data-stu-id="d9054-178">This information is transmitted toohello Spark worker nodes.</span></span>
4. <span data-ttu-id="d9054-179">Hello Spark pracovním uzlům připojit toohello Azure Cosmos DB oddíly přímo tooextract hello data a vrátí hello data toohello oddíly Spark v hello Spark pracovním uzlům.</span><span class="sxs-lookup"><span data-stu-id="d9054-179">hello Spark worker nodes connect toohello Azure Cosmos DB partitions directly tooextract hello data and return hello data toohello Spark partitions in hello Spark worker nodes.</span></span>

<span data-ttu-id="d9054-180">Komunikace mezi Spark a Azure Cosmos DB je výrazně rychlejší, protože je hello přesun dat mezi uzly pracovního procesu hello Spark a hello Azure Cosmos DB datové uzly (oddíly).</span><span class="sxs-lookup"><span data-stu-id="d9054-180">Communication between Spark and Azure Cosmos DB is significantly faster because hello data movement is between hello Spark worker nodes and hello Azure Cosmos DB data nodes (partitions).</span></span>

### <a name="build-hello-spark-tooazure-cosmos-db-connector"></a><span data-ttu-id="d9054-181">Sestavení hello Spark tooAzure Cosmos DB konektoru</span><span class="sxs-lookup"><span data-stu-id="d9054-181">Build hello Spark tooAzure Cosmos DB connector</span></span>
<span data-ttu-id="d9054-182">V současné době používá hello konektor projekt maven.</span><span class="sxs-lookup"><span data-stu-id="d9054-182">Currently, hello connector project uses maven.</span></span> <span data-ttu-id="d9054-183">konektor hello toobuild bez závislosti, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="d9054-183">toobuild hello connector without dependencies, you can run:</span></span>
```
mvn clean package
```
<span data-ttu-id="d9054-184">Můžete si taky stáhnout nejnovější verze hello JAR hello z hello *uvolní* složky.</span><span class="sxs-lookup"><span data-stu-id="d9054-184">You can also download hello latest versions of hello JAR from hello *releases* folder.</span></span>

### <a name="include-hello-azure-cosmos-db-spark-jar"></a><span data-ttu-id="d9054-185">Zahrnout hello Azure Cosmos DB Spark JAR</span><span class="sxs-lookup"><span data-stu-id="d9054-185">Include hello Azure Cosmos DB Spark JAR</span></span>
<span data-ttu-id="d9054-186">Než spustíte všechny kód, je třeba tooinclude hello Azure Cosmos DB Spark JAR.</span><span class="sxs-lookup"><span data-stu-id="d9054-186">Before you execute any code, you need tooinclude hello Azure Cosmos DB Spark JAR.</span></span>  <span data-ttu-id="d9054-187">Pokud používáte hello **spark prostředí**, potom můžete zahrnout hello JAR pomocí hello **– JAR** možnost.</span><span class="sxs-lookup"><span data-stu-id="d9054-187">If you are using hello **spark-shell**, then you can include hello JAR by using hello **--jars** option.</span></span>  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

<span data-ttu-id="d9054-188">Pokud chcete, aby tooexecute hello JAR bez závislostí, použijte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="d9054-188">If you want tooexecute hello JAR without dependencies, use hello following code:</span></span>

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

<span data-ttu-id="d9054-189">Pokud používáte služby Poznámkový blok, jako je služba poznámkového bloku Azure HDInsight Jupyter, můžete použít hello **spark magic** příkazy:</span><span class="sxs-lookup"><span data-stu-id="d9054-189">If you are using a notebook service such as Azure HDInsight Jupyter notebook service, you can use hello **spark magic** commands:</span></span>

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

<span data-ttu-id="d9054-190">Hello **JAR** příkaz umožňuje vám tooinclude hello dva JARs, které jsou potřebné pro **azure. cosmosdb spark** (samostatně a hello Azure DocumentDB Java SDK) a vyloučit **scala-odráží** tak, že nebudou v konfliktu s hello volá Livy (Poznámkový blok Jupyter > Livy > Spark).</span><span class="sxs-lookup"><span data-stu-id="d9054-190">hello **jars** command enables you tooinclude hello two JARs that are needed for **azure-cosmosdb-spark** (itself and hello Azure DocumentDB Java SDK) and exclude **scala-reflect** so that it does not interfere with hello Livy calls (Jupyter notebook > Livy > Spark).</span></span>

### <a name="connect-spark-tooazure-cosmos-db-using-hello-connector"></a><span data-ttu-id="d9054-191">Připojit Spark pomocí Cosmos DB tooAzure hello konektoru</span><span class="sxs-lookup"><span data-stu-id="d9054-191">Connect Spark tooAzure Cosmos DB using hello connector</span></span>
<span data-ttu-id="d9054-192">I když hello komunikace přenosu je trochu složitější, provádění dotazu z Spark tooAzure DB Cosmos pomocí konektoru hello je výrazně rychlejší.</span><span class="sxs-lookup"><span data-stu-id="d9054-192">Although hello communication transport is a little more complicated, executing a query from Spark tooAzure Cosmos DB by using hello connector is significantly faster.</span></span>

<span data-ttu-id="d9054-193">Hello následující fragment kódu ukazuje, jak toouse hello konektor v kontextu Spark.</span><span class="sxs-lookup"><span data-stu-id="d9054-193">hello following code snippet shows how toouse hello connector in a Spark context.</span></span>

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

<span data-ttu-id="d9054-194">Jak jsme uvedli v hello fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="d9054-194">As noted in hello code snippet:</span></span>

- <span data-ttu-id="d9054-195">**Azure-cosmosdb-spark** obsahuje hello všechny hello parametrů potřeby připojení, mezi které patří hello preferované umístění.</span><span class="sxs-lookup"><span data-stu-id="d9054-195">**azure-cosmosdb-spark** contains hello all hello necessary connection parameters, which include hello preferred locations.</span></span> <span data-ttu-id="d9054-196">Můžete například, že hello číst repliky a s prioritou pořadí.</span><span class="sxs-lookup"><span data-stu-id="d9054-196">For example, you can choose hello read replica and priority order.</span></span>
- <span data-ttu-id="d9054-197">Právě importovat hello potřebné knihovny a nakonfigurujte masterKey a hostitele Azure Cosmos DB klienta hello toocreate.</span><span class="sxs-lookup"><span data-stu-id="d9054-197">Just import hello necessary libraries and configure your masterKey and host toocreate hello Azure Cosmos DB client.</span></span>

### <a name="execute-spark-queries-via-hello-connector"></a><span data-ttu-id="d9054-198">Spuštění dotazů Spark prostřednictvím konektoru hello</span><span class="sxs-lookup"><span data-stu-id="d9054-198">Execute Spark queries via hello connector</span></span>

<span data-ttu-id="d9054-199">Následující příklad používá hello Azure Cosmos DB instance, který byl vytvořen v předchozím fragmentu kódu hello pomocí hello Hello zadané klíče jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="d9054-199">hello following example uses hello Azure Cosmos DB instance that was created in hello previous snippet by using hello specified read-only keys.</span></span> <span data-ttu-id="d9054-200">Hello následující fragment kódu připojí toohello DepartureDelays.flights_pcoll kolekce (v hello DoctorWho účtu uvedeného výše) a spustí dotaz tooextract hello zpoždění informace o letu letů, které jsou jednotlivé Praha.</span><span class="sxs-lookup"><span data-stu-id="d9054-200">hello following code snippet connects toohello DepartureDelays.flights_pcoll collection (in hello DoctorWho account as specified earlier) and runs a query tooextract hello flight delay information of flights that are departing from Seattle.</span></span>

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-hello-spark-tooazure-cosmos-db-connector-implementation"></a><span data-ttu-id="d9054-201">Proč používat hello Spark tooAzure Cosmos DB konektor implementace?</span><span class="sxs-lookup"><span data-stu-id="d9054-201">Why use hello Spark tooAzure Cosmos DB connector implementation?</span></span>

<span data-ttu-id="d9054-202">Připojení Spark tooAzure DB Cosmos pomocí konektoru hello je obvykle pro scénáře, kde:</span><span class="sxs-lookup"><span data-stu-id="d9054-202">Connecting Spark tooAzure Cosmos DB by using hello connector is typically for scenarios where:</span></span>

* <span data-ttu-id="d9054-203">Chcete toouse Scala a aktualizovat tooinclude obálku Python, jak jsme uvedli v [problém 3: Přidání Python obálku a příklady](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span><span class="sxs-lookup"><span data-stu-id="d9054-203">You want toouse Scala and update it tooinclude a Python wrapper as noted in [Issue 3: Add Python wrapper and examples](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span></span>
* <span data-ttu-id="d9054-204">Máte velké množství dat tootransfer mezi Apache Spark a Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d9054-204">You have a large amount of data tootransfer between Apache Spark and Azure Cosmos DB.</span></span>

<span data-ttu-id="d9054-205">toogive představu o hello rozdíl ve výkonu dotazů, uvidíte hello [dotazu testovací spouští wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span><span class="sxs-lookup"><span data-stu-id="d9054-205">toogive you an idea of hello query performance difference, see hello [Query Test Runs wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span></span>

## <a name="distributed-aggregation-example"></a><span data-ttu-id="d9054-206">Příklad distribuované agregace</span><span class="sxs-lookup"><span data-stu-id="d9054-206">Distributed aggregation example</span></span>
<span data-ttu-id="d9054-207">Tato část obsahuje příklady jak to distribuované agregací a analýzy pomocí Apache Spark a Azure Cosmos DB společně.</span><span class="sxs-lookup"><span data-stu-id="d9054-207">This section provides some examples of how you can do distributed aggregations and analytics by using Apache Spark and Azure Cosmos DB together.</span></span> <span data-ttu-id="d9054-208">Azure Cosmos DB již podporuje agregace, která je popsána v hello [planetu agregace škálování s Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span><span class="sxs-lookup"><span data-stu-id="d9054-208">Azure Cosmos DB already supports aggregations, which is discussed in hello [Planet scale aggregates with Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span></span> <span data-ttu-id="d9054-209">Zde je, jak můžete si ho toohello další úrovně s Apache Spark.</span><span class="sxs-lookup"><span data-stu-id="d9054-209">Here is how you can take it toohello next level with Apache Spark.</span></span>

<span data-ttu-id="d9054-210">Všimněte si, že tyto agregace v odkazu toohello [Spark tooAzure konektor DB Cosmos poznámkového bloku](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span><span class="sxs-lookup"><span data-stu-id="d9054-210">Note that these aggregations are in reference toohello [Spark tooAzure Cosmos DB Connector notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span></span>

### <a name="connect-tooflights-sample-data"></a><span data-ttu-id="d9054-211">Připojit tooflights ukázková data</span><span class="sxs-lookup"><span data-stu-id="d9054-211">Connect tooflights sample data</span></span>
<span data-ttu-id="d9054-212">Tyto příklady agregace přístup k cestě výkonu data, která je uložena v našem **DoctorWho** databáze Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d9054-212">These aggregation examples access some flight performance data that's stored in our **DoctorWho** Azure Cosmos DB database.</span></span> <span data-ttu-id="d9054-213">tooconnect tooit, je třeba tooutilize hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="d9054-213">tooconnect tooit, you need tooutilize hello following code snippet:</span></span>

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

<span data-ttu-id="d9054-214">Tento fragment kódu jsme jsou také probíhající toorun základní dotazu, že přenosy hello filtrované sady dat z Azure Cosmos DB tooSpark kde hello pozdější můžete provést distribuované agregace.</span><span class="sxs-lookup"><span data-stu-id="d9054-214">With this snippet, we are also going toorun a base query that transfers hello filtered set of data from Azure Cosmos DB tooSpark where hello latter can perform distributed aggregates.</span></span> <span data-ttu-id="d9054-215">V takovém případě vás žádáme pro lety, které se liší od Praha (SEA).</span><span class="sxs-lookup"><span data-stu-id="d9054-215">In this case, we are asking for flights that depart from Seattle (SEA).</span></span>

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

<span data-ttu-id="d9054-216">Hello následující výsledky byly vygenerovány spuštěním hello dotazy z hello služby poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="d9054-216">hello following results were generated by running hello queries from hello Jupyter notebook service.</span></span>  <span data-ttu-id="d9054-217">Upozorňujeme, že jsou všechny fragmenty kódu hello obecné a nejsou specifické tooany služby.</span><span class="sxs-lookup"><span data-stu-id="d9054-217">Note that all hello code snippets are generic and not specific tooany service.</span></span>

### <a name="running-limit-and-count-queries"></a><span data-ttu-id="d9054-218">LIMIT spuštěné a počet dotazů</span><span class="sxs-lookup"><span data-stu-id="d9054-218">Running LIMIT and COUNT queries</span></span>
<span data-ttu-id="d9054-219">Právě, jako je třeba to, co se používá tooin SQL nebo Spark SQL, můžeme začít s **LIMIT** dotazu:</span><span class="sxs-lookup"><span data-stu-id="d9054-219">Just like you're used tooin SQL/Spark SQL, let's start off with a **LIMIT** query:</span></span>

![Spark LIMIT dotazu](./media/spark-connector/spark-sql-query.png)

<span data-ttu-id="d9054-221">Hello další dotaz je jednoduchý a rychlé **počet** dotazu:</span><span class="sxs-lookup"><span data-stu-id="d9054-221">hello next query is a simple and fast **COUNT** query:</span></span>

![Dotazu Spark COUNT](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a><span data-ttu-id="d9054-223">GROUP BY dotazu</span><span class="sxs-lookup"><span data-stu-id="d9054-223">GROUP BY query</span></span>
<span data-ttu-id="d9054-224">V této sadě další můžeme jednoduše spustit **GROUP BY** dotazy pro Azure Cosmos DB databáze:</span><span class="sxs-lookup"><span data-stu-id="d9054-224">In this next set, we can easily run **GROUP BY** queries against our Azure Cosmos DB database:</span></span>

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY dotazu grafu.](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a><span data-ttu-id="d9054-226">JEDINEČNÉ, ORDER BY dotazu</span><span class="sxs-lookup"><span data-stu-id="d9054-226">DISTINCT, ORDER BY query</span></span>
<span data-ttu-id="d9054-227">A tady je **DISTINCT, ORDER BY** dotazu:</span><span class="sxs-lookup"><span data-stu-id="d9054-227">And here is a **DISTINCT, ORDER BY** query:</span></span>

![Spark GROUP BY dotazu grafu.](./media/spark-connector/order-by-query.png)

### <a name="continue-hello-flight-data-analysis"></a><span data-ttu-id="d9054-229">Analýza dat letu hello pokračovat</span><span class="sxs-lookup"><span data-stu-id="d9054-229">Continue hello flight data analysis</span></span>
<span data-ttu-id="d9054-230">Můžete použít následující příklad dotazy toocontinue analýzy dat letu hello hello:</span><span class="sxs-lookup"><span data-stu-id="d9054-230">You can use hello following example queries toocontinue analysis of hello flight data:</span></span>

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a><span data-ttu-id="d9054-231">TOP 5 Zpožděné cíle (města) jednotlivé Praha</span><span class="sxs-lookup"><span data-stu-id="d9054-231">Top 5 delayed destinations (cities) departing from Seattle</span></span>
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark nejvyšší zpoždění grafu](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a><span data-ttu-id="d9054-233">Vypočítat střední zpoždění podle měst cílové jednotlivé Praha</span><span class="sxs-lookup"><span data-stu-id="d9054-233">Calculate median delays by destination cities departing from Seattle</span></span>
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark střední zpoždění grafu](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a><span data-ttu-id="d9054-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9054-235">Next steps</span></span>

<span data-ttu-id="d9054-236">Pokud jste tak ještě neučinili, stáhněte hello Spark tooAzure Cosmos DB konektor z hello [azure. cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) úložiště GitHub a seznamte se s hello další prostředky v úložišti hello:</span><span class="sxs-lookup"><span data-stu-id="d9054-236">If you haven't already, download hello Spark tooAzure Cosmos DB connector from hello [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub repository and explore hello additional resources in hello repo:</span></span>

* [<span data-ttu-id="d9054-237">Příklady distribuované agregace</span><span class="sxs-lookup"><span data-stu-id="d9054-237">Distributed Aggregations Examples</span></span>](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [<span data-ttu-id="d9054-238">Ukázkové skripty a poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="d9054-238">Sample Scripts and Notebooks</span></span>](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

<span data-ttu-id="d9054-239">Můžete také tooreview hello [Apache Spark SQL, průvodce datové sady a DataFrames](http://spark.apache.org/docs/latest/sql-programming-guide.html) a hello [Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) článku.</span><span class="sxs-lookup"><span data-stu-id="d9054-239">You might also want tooreview hello [Apache Spark SQL, DataFrames, and Datasets Guide](http://spark.apache.org/docs/latest/sql-programming-guide.html) and hello [Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) article.</span></span>
