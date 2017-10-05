---
title: "Připojení k Azure Cosmos DB Apache Spark | Microsoft Docs"
description: "Pomocí tohoto kurzu se dozvíte o konektoru Azure Cosmos DB Spark, který vám umožní připojit Apache Spark pro Azure DB Cosmos k provedení distribuované věd agregacemi a daty na více klientů globálně distribuované databázový systém od společnosti Microsoft to je navržené pro cloud."
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
ms.openlocfilehash: 8ecbb478c81cde25bbd0d1c9ee07ae02b07f8cc7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="accelerate-real-time-big-data-analytics-with-the-spark-to-azure-cosmos-db-connector"></a><span data-ttu-id="aee73-104">Urychlit analýzy velkých objemů dat v reálném čase s Spark pro konektor Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="aee73-104">Accelerate real-time big-data analytics with the Spark to Azure Cosmos DB connector</span></span>

<span data-ttu-id="aee73-105">Spark pro Azure Cosmos DB konektor umožňuje Azure DB Cosmos tak, aby fungoval jako vstupní zdroj nebo výstupní jímku pro úlohy Apache Spark.</span><span class="sxs-lookup"><span data-stu-id="aee73-105">The Spark to Azure Cosmos DB connector enables Azure Cosmos DB to act as an input source or output sink for Apache Spark jobs.</span></span> <span data-ttu-id="aee73-106">Připojení [Spark](http://spark.apache.org/) k [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) zrychluje schopnost řešení fast přesouvání dat vědecké účely problémů, kde můžete použít Azure Cosmos DB k rychlému zachovat a zadávat dotazy na data.</span><span class="sxs-lookup"><span data-stu-id="aee73-106">Connecting [Spark](http://spark.apache.org/) to [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) accelerates your ability to solve fast-moving data science problems where you can use Azure Cosmos DB to quickly persist and query data.</span></span> <span data-ttu-id="aee73-107">Spark pro konektor Azure Cosmos DB efektivně využívá nativní indexů Azure DB Cosmos spravované.</span><span class="sxs-lookup"><span data-stu-id="aee73-107">The Spark to Azure Cosmos DB connector efficiently utilizes the native Azure Cosmos DB managed indexes.</span></span> <span data-ttu-id="aee73-108">Indexy povolit aktualizovatelné sloupce, pokud provádět analýzy a scénáře vědy a analýzy dat nabízená dolů predikátem filtrování fast změna globálně distribuují data, která v rozsahu od Internetu věcí (IoT).</span><span class="sxs-lookup"><span data-stu-id="aee73-108">The indexes enable updateable columns when you perform analytics and push-down predicate filtering against fast-changing globally distributed data, which range from Internet of Things (IoT) to data science and analytics scenarios.</span></span>

<span data-ttu-id="aee73-109">Práce s Spark, GraphX a graf Gremlin rozhraní API databázi Cosmos Azure, najdete v části [provádět analýzy grafu pomocí Spark a Apache TinkerPop Gremlin](spark-connector-graph.md).</span><span class="sxs-lookup"><span data-stu-id="aee73-109">For working with Spark GraphX and the Gremlin graph APIs of Azure Cosmos DB, see [Perform graph analytics using Spark and Apache TinkerPop Gremlin](spark-connector-graph.md).</span></span>

## <a name="download"></a><span data-ttu-id="aee73-110">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="aee73-110">Download</span></span>

<span data-ttu-id="aee73-111">Abyste mohli začít, stáhnout konektor Azure Cosmos DB (preview) Spark z [azure. cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/) úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="aee73-111">To get started, download the Spark to Azure Cosmos DB connector (preview) from the [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) repository on GitHub.</span></span>

## <a name="connector-components"></a><span data-ttu-id="aee73-112">Konektor součásti</span><span class="sxs-lookup"><span data-stu-id="aee73-112">Connector components</span></span>

<span data-ttu-id="aee73-113">Konektor používá následující součásti:</span><span class="sxs-lookup"><span data-stu-id="aee73-113">The connector utilizes the following components:</span></span>

* <span data-ttu-id="aee73-114">[Azure Cosmos DB](http://documentdb.com) umožňuje zákazníkům zřídit a Elasticky škálovat propustnost a úložiště napříč libovolný počet zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="aee73-114">[Azure Cosmos DB](http://documentdb.com) enables customers to provision and elastically scale both throughput and storage across any number of geographical regions.</span></span> <span data-ttu-id="aee73-115">Nabízí služba:</span><span class="sxs-lookup"><span data-stu-id="aee73-115">The service offers:</span></span>
   * <span data-ttu-id="aee73-116">Aktivujte klíč [globální distribuční](distribute-data-globally.md) a horizontální škálování</span><span class="sxs-lookup"><span data-stu-id="aee73-116">Turn key [global distribution](distribute-data-globally.md) and horizontal scale</span></span>
   * <span data-ttu-id="aee73-117">Zaručit latence jediná číslice v 99th percentilu</span><span class="sxs-lookup"><span data-stu-id="aee73-117">Guaranteed single digit latencies at the 99th percentile</span></span>
   * [<span data-ttu-id="aee73-118">Více dobře definovaný konzistence modelů</span><span class="sxs-lookup"><span data-stu-id="aee73-118">Multiple well-defined consistency models</span></span>](consistency-levels.md)
   * <span data-ttu-id="aee73-119">Zaručit vysoká dostupnost s více funkci Možnosti</span><span class="sxs-lookup"><span data-stu-id="aee73-119">Guaranteed high availability with multi-homing capabilities</span></span>
   * <span data-ttu-id="aee73-120">Všechny funkce jsou zajišťované špičkový, komplexní [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLA).</span><span class="sxs-lookup"><span data-stu-id="aee73-120">All features are backed by industry-leading, comprehensive [service level agreements](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLAs).</span></span>

* <span data-ttu-id="aee73-121">[Apache Spark](http://spark.apache.org/) modul zpracování výkonné s otevřeným zdrojem, který je vytvořena na rychlost, snadné použití a sofistikované analytics.</span><span class="sxs-lookup"><span data-stu-id="aee73-121">[Apache Spark](http://spark.apache.org/) is a powerful open source processing engine that's built around speed, ease of use, and sophisticated analytics.</span></span>

* <span data-ttu-id="aee73-122">[Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) tak, aby Apache Spark v cloudu pro kritické nasazení můžete nasadit pomocí [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="aee73-122">[Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) so that you can deploy Apache Spark in the cloud for mission-critical deployments by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="aee73-123">Oficiálně podporované verze:</span><span class="sxs-lookup"><span data-stu-id="aee73-123">Officially supported versions:</span></span>

| <span data-ttu-id="aee73-124">Komponenta</span><span class="sxs-lookup"><span data-stu-id="aee73-124">Component</span></span> | <span data-ttu-id="aee73-125">Verze</span><span class="sxs-lookup"><span data-stu-id="aee73-125">Version</span></span> |
|---------|-------|
|<span data-ttu-id="aee73-126">Apache Spark</span><span class="sxs-lookup"><span data-stu-id="aee73-126">Apache Spark</span></span>|<span data-ttu-id="aee73-127">2.0+</span><span class="sxs-lookup"><span data-stu-id="aee73-127">2.0+</span></span>|
| <span data-ttu-id="aee73-128">Scala</span><span class="sxs-lookup"><span data-stu-id="aee73-128">Scala</span></span>| <span data-ttu-id="aee73-129">2.11</span><span class="sxs-lookup"><span data-stu-id="aee73-129">2.11</span></span>|
| <span data-ttu-id="aee73-130">Azure DocumentDB Java SDK</span><span class="sxs-lookup"><span data-stu-id="aee73-130">Azure DocumentDB Java SDK</span></span> | <span data-ttu-id="aee73-131">1.10.0</span><span class="sxs-lookup"><span data-stu-id="aee73-131">1.10.0</span></span> |

<span data-ttu-id="aee73-132">Tento článek vám pomůže spustit některé jednoduché ukázky pomocí Python (prostřednictvím pydocumentdb v tuto) a rozhraní Scala.</span><span class="sxs-lookup"><span data-stu-id="aee73-132">This article helps you run some simple samples by using Python (via pyDocumentDB) and the Scala interfaces.</span></span>

<span data-ttu-id="aee73-133">Existují dva přístupy k připojení Apache Spark a Cosmos databázi Azure:</span><span class="sxs-lookup"><span data-stu-id="aee73-133">There are two approaches to connect Apache Spark and Azure Cosmos DB:</span></span>
- <span data-ttu-id="aee73-134">Použít pydocumentdb v tuto prostřednictvím [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span><span class="sxs-lookup"><span data-stu-id="aee73-134">Use pyDocumentDB via the [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span></span>
- <span data-ttu-id="aee73-135">Vytvoření Spark založené na jazyce Java do Azure Cosmos DB konektoru s využitím [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="aee73-135">Create a Java-based Spark to Azure Cosmos DB connector by utilizing the [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

## <a name="pydocumentdb-implementation"></a><span data-ttu-id="aee73-136">pydocumentdb v tuto implementaci</span><span class="sxs-lookup"><span data-stu-id="aee73-136">pyDocumentDB implementation</span></span>
<span data-ttu-id="aee73-137">Aktuální [pydocumentdb v tuto sadu SDK](https://github.com/Azure/azure-documentdb-python) umožňuje připojení k databázi Cosmos Azure Spark, jak je znázorněno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="aee73-137">The current [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) enables you to connect Spark to Azure Cosmos DB as shown in the following diagram:</span></span>

![Spark pro Azure Cosmos DB tok dat prostřednictvím pydocumentdb v tuto DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-the-pydocumentdb-implementation"></a><span data-ttu-id="aee73-139">Tok dat pydocumentdb v tuto implementaci</span><span class="sxs-lookup"><span data-stu-id="aee73-139">Data flow of the pyDocumentDB implementation</span></span>

<span data-ttu-id="aee73-140">Tok dat je následující:</span><span class="sxs-lookup"><span data-stu-id="aee73-140">The data flow is as follows:</span></span>

1. <span data-ttu-id="aee73-141">Připojí se ke uzel brány Azure Cosmos DB prostřednictvím pydocumentdb v tuto hlavní uzel Spark.</span><span class="sxs-lookup"><span data-stu-id="aee73-141">The Spark master node connects to the Azure Cosmos DB gateway node via pyDocumentDB.</span></span> <span data-ttu-id="aee73-142">Uživatel Určuje pouze připojení Spark a Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="aee73-142">A user specifies only the Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="aee73-143">Připojení k příslušnými uzly hlavní a brány jsou transparentní pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="aee73-143">Connections to the respective master and gateway nodes are transparent to the user.</span></span>
2. <span data-ttu-id="aee73-144">Uzel brány vytvoří dotaz na databázi Cosmos Azure, kde dotaz následně spouští kolekce oddílů v datové uzly.</span><span class="sxs-lookup"><span data-stu-id="aee73-144">The gateway node makes the query against Azure Cosmos DB where the query subsequently runs against the collection's partitions in the data nodes.</span></span> <span data-ttu-id="aee73-145">Odpovědi na tyto dotazy budou odeslána zpět do uzlu brány, a že sadu výsledků dotazu je vrácen do hlavní uzel Spark.</span><span class="sxs-lookup"><span data-stu-id="aee73-145">The response for those queries is sent back to the gateway node, and that result set is returned to the Spark master node.</span></span>
3. <span data-ttu-id="aee73-146">Následující dotazy (například pro Spark DataFrame) odešlou k pracovním uzlům Spark pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="aee73-146">Subsequent queries (for example, against a Spark DataFrame) are sent to the Spark worker nodes for processing.</span></span>

<span data-ttu-id="aee73-147">Komunikace mezi Spark a Azure Cosmos DB je omezený na hlavní uzel Spark a uzly brány Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="aee73-147">Communication between Spark and Azure Cosmos DB is limited to the Spark master node and Azure Cosmos DB gateway nodes.</span></span>  <span data-ttu-id="aee73-148">Dotazy přejděte tak rychle, jak umožňuje transportní vrstva mezi těmito dvěma uzly.</span><span class="sxs-lookup"><span data-stu-id="aee73-148">The queries go as fast as the transport layer between these two nodes allows.</span></span>

### <a name="install-pydocumentdb"></a><span data-ttu-id="aee73-149">Nainstalujte pydocumentdb v tuto</span><span class="sxs-lookup"><span data-stu-id="aee73-149">Install pyDocumentDB</span></span>
<span data-ttu-id="aee73-150">Pydocumentdb v tuto vašeho uzlu ovladačů můžete nainstalovat pomocí **pip**, například:</span><span class="sxs-lookup"><span data-stu-id="aee73-150">You can install pyDocumentDB on your driver node by using **pip**, for example:</span></span>

```
pip install pyDocumentDB
```


### <a name="connect-spark-to-azure-cosmos-db-via-pydocumentdb"></a><span data-ttu-id="aee73-151">Připojení k databázi Cosmos Azure Spark prostřednictvím pydocumentdb v tuto</span><span class="sxs-lookup"><span data-stu-id="aee73-151">Connect Spark to Azure Cosmos DB via pyDocumentDB</span></span>
<span data-ttu-id="aee73-152">Jednoduchost přenosu komunikace provede spuštění dotazu z Spark Azure Cosmos DB pomocí poměrně jednoduché pydocumentdb v tuto.</span><span class="sxs-lookup"><span data-stu-id="aee73-152">The simplicity of the communication transport makes execution of a query from Spark to Azure Cosmos DB by using pyDocumentDB relatively simple.</span></span>

<span data-ttu-id="aee73-153">Následující fragment kódu ukazuje, jak používat pydocumentdb v tuto v kontextu Spark.</span><span class="sxs-lookup"><span data-stu-id="aee73-153">The following code snippet shows how to use pyDocumentDB in a Spark context.</span></span>

```
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]


# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

<span data-ttu-id="aee73-154">Jak jsme uvedli v fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="aee73-154">As noted in the code snippet:</span></span>

* <span data-ttu-id="aee73-155">Azure Python Cosmos DB SDK (`pyDocumentDB`) obsahuje všechny potřebné parametry připojení.</span><span class="sxs-lookup"><span data-stu-id="aee73-155">The Azure Cosmos DB Python SDK (`pyDocumentDB`) contains the all the necessary connection parameters.</span></span> <span data-ttu-id="aee73-156">Například parametr upřednostňované umístění zvolí čtení pořadí repliky a s prioritou.</span><span class="sxs-lookup"><span data-stu-id="aee73-156">For example, the preferred locations parameter chooses the read replica and priority order.</span></span>
*  <span data-ttu-id="aee73-157">Potřebné knihovny pro import a nakonfigurování vaše **masterKey** a **hostitele** k vytvoření Azure Cosmos DB *klienta* (**pydocumentdb.document_client** ).</span><span class="sxs-lookup"><span data-stu-id="aee73-157">Import the necessary libraries and configure your **masterKey** and **host** to create the Azure Cosmos DB *client* (**pydocumentdb.document_client**).</span></span>


### <a name="execute-spark-queries-via-pydocumentdb"></a><span data-ttu-id="aee73-158">Spuštění dotazů Spark prostřednictvím pydocumentdb v tuto</span><span class="sxs-lookup"><span data-stu-id="aee73-158">Execute Spark Queries via pyDocumentDB</span></span>
<span data-ttu-id="aee73-159">Následující příklady použití Azure Cosmos DB instanci, která byla vytvořena v předchozím fragmentu kódu pomocí zadaného klíče jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="aee73-159">The following examples use the Azure Cosmos DB instance that was created in the previous snippet by using the specified read-only keys.</span></span> <span data-ttu-id="aee73-160">Následující fragment kódu se připojuje k **airports.codes** kolekce v účtu DoctorWho jako dříve zadaný a spustí dotaz k extrakci města letiště ve státě Washington.</span><span class="sxs-lookup"><span data-stu-id="aee73-160">The following code snippet connects to the **airports.codes** collection in the DoctorWho account as specified earlier and runs a query to extract the airport cities in Washington state.</span></span>

```
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
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

<span data-ttu-id="aee73-161">Po provedení dotazu prostřednictvím **dotazu**, výsledkem je, **query_iterable. QueryIterable** , jsou převedeny na seznam Python.</span><span class="sxs-lookup"><span data-stu-id="aee73-161">After the query has been executed via **query**, the result is a **query_iterable.QueryIterable** that is converted to a Python list.</span></span> <span data-ttu-id="aee73-162">Seznam Python lze snadno převést na Spark DataFrame pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="aee73-162">A Python list can be easily converted to a Spark DataFrame by using the following code:</span></span>

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-the-pydocumentdb-to-connect-spark-to-azure-cosmos-db"></a><span data-ttu-id="aee73-163">Proč používat pydocumentdb v tuto pro připojení k databázi Cosmos Azure Spark?</span><span class="sxs-lookup"><span data-stu-id="aee73-163">Why use the pyDocumentDB to connect Spark to Azure Cosmos DB?</span></span>
<span data-ttu-id="aee73-164">Spark se připojit k databázi Cosmos Azure pomocí pydocumentdb v tuto obvykle pro scénáře, kde:</span><span class="sxs-lookup"><span data-stu-id="aee73-164">Connecting Spark to Azure Cosmos DB by using pyDocumentDB is typically for scenarios where:</span></span>

* <span data-ttu-id="aee73-165">Chcete používat jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="aee73-165">You want to use Python.</span></span>
* <span data-ttu-id="aee73-166">Jsou vrácení výsledku poměrně malý, nastavte databázi Cosmos Azure Spark.</span><span class="sxs-lookup"><span data-stu-id="aee73-166">You are returning a relatively small result set from Azure Cosmos DB to Spark.</span></span> <span data-ttu-id="aee73-167">Všimněte si, že může mít poměrně značnou podkladové datové sady v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="aee73-167">Note that the underlying dataset in Azure Cosmos DB can be quite large.</span></span> <span data-ttu-id="aee73-168">Jsou použití filtrů, tedy spuštěným filtry predikátů pro váš zdroj Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="aee73-168">You are applying filters, that is, running predicate filters, against your Azure Cosmos DB source.</span></span>  

## <a name="spark-to-azure-cosmos-db-connector"></a><span data-ttu-id="aee73-169">Spark pro konektor Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="aee73-169">Spark to Azure Cosmos DB connector</span></span>

<span data-ttu-id="aee73-170">Spark pro konektor Azure Cosmos DB využívá [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) a přesouvá data mezi uzly pracovního procesu Spark a Azure Cosmos DB, jak je znázorněno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="aee73-170">The Spark to Azure Cosmos DB connector utilizes the [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) and moves data between the Spark worker nodes and Azure Cosmos DB as shown in the following diagram:</span></span>

![Tok dat v Spark pro konektor Azure Cosmos DB](./media/spark-connector/spark-connector.png)

<span data-ttu-id="aee73-172">Tok dat je následující:</span><span class="sxs-lookup"><span data-stu-id="aee73-172">The data flow is as follows:</span></span>

1. <span data-ttu-id="aee73-173">Hlavní uzel Spark se připojí k uzel brány Azure Cosmos DB získat mapa oddílu.</span><span class="sxs-lookup"><span data-stu-id="aee73-173">The Spark master node connects to the Azure Cosmos DB gateway node to obtain the partition map.</span></span> <span data-ttu-id="aee73-174">Uživatel Určuje pouze připojení Spark a Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="aee73-174">A user specifies only the Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="aee73-175">Připojení k příslušnými uzly hlavní a brány jsou transparentní pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="aee73-175">Connections to the respective master and gateway nodes are transparent to the user.</span></span>
2. <span data-ttu-id="aee73-176">Tyto informace jsou poskytovány zpět do hlavního uzlu Spark.</span><span class="sxs-lookup"><span data-stu-id="aee73-176">This information is provided back to the Spark master node.</span></span>  <span data-ttu-id="aee73-177">V tomto okamžiku nyní byste měli mít analyzovat dotaz a zjistit oddíly a jejich umístění v Azure DB Cosmos, které potřebujete získat přístup.</span><span class="sxs-lookup"><span data-stu-id="aee73-177">At this point, you should be able to parse the query to determine the partitions and their locations in Azure Cosmos DB that you need to access.</span></span>
3. <span data-ttu-id="aee73-178">Tyto informace se přenesou do uzlů pracovního procesu Spark.</span><span class="sxs-lookup"><span data-stu-id="aee73-178">This information is transmitted to the Spark worker nodes.</span></span>
4. <span data-ttu-id="aee73-179">Pracovní uzly Spark se připojit k Azure Cosmos DB oddíly, které přímo mají extrahovat data a vrátit data do oddílů Spark v pracovním uzlům Spark.</span><span class="sxs-lookup"><span data-stu-id="aee73-179">The Spark worker nodes connect to the Azure Cosmos DB partitions directly to extract the data and return the data to the Spark partitions in the Spark worker nodes.</span></span>

<span data-ttu-id="aee73-180">Komunikace mezi Spark a Azure Cosmos DB je výrazně rychlejší, protože je přesun dat mezi uzly pracovního procesu Spark a datové uzly Azure Cosmos DB (oddíly).</span><span class="sxs-lookup"><span data-stu-id="aee73-180">Communication between Spark and Azure Cosmos DB is significantly faster because the data movement is between the Spark worker nodes and the Azure Cosmos DB data nodes (partitions).</span></span>

### <a name="build-the-spark-to-azure-cosmos-db-connector"></a><span data-ttu-id="aee73-181">Sestavení Spark pro konektor Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="aee73-181">Build the Spark to Azure Cosmos DB connector</span></span>
<span data-ttu-id="aee73-182">V současné době používá konektor projekt maven.</span><span class="sxs-lookup"><span data-stu-id="aee73-182">Currently, the connector project uses maven.</span></span> <span data-ttu-id="aee73-183">Pokud chcete vytvořit konektor bez závislosti, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="aee73-183">To build the connector without dependencies, you can run:</span></span>
```
mvn clean package
```
<span data-ttu-id="aee73-184">Můžete si taky stáhnout nejnovější verze JAR z *uvolní* složky.</span><span class="sxs-lookup"><span data-stu-id="aee73-184">You can also download the latest versions of the JAR from the *releases* folder.</span></span>

### <a name="include-the-azure-cosmos-db-spark-jar"></a><span data-ttu-id="aee73-185">Zahrnout Spark DB Azure Cosmos JAR</span><span class="sxs-lookup"><span data-stu-id="aee73-185">Include the Azure Cosmos DB Spark JAR</span></span>
<span data-ttu-id="aee73-186">Před spuštěním žádný kód, musíte zahrnout JAR Azure Cosmos pro Spark DB.</span><span class="sxs-lookup"><span data-stu-id="aee73-186">Before you execute any code, you need to include the Azure Cosmos DB Spark JAR.</span></span>  <span data-ttu-id="aee73-187">Pokud používáte **spark prostředí**, potom můžete zahrnout JAR pomocí **– JAR** možnost.</span><span class="sxs-lookup"><span data-stu-id="aee73-187">If you are using the **spark-shell**, then you can include the JAR by using the **--jars** option.</span></span>  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

<span data-ttu-id="aee73-188">Pokud chcete provést JAR bez závislostí, použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="aee73-188">If you want to execute the JAR without dependencies, use the following code:</span></span>

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

<span data-ttu-id="aee73-189">Pokud používáte služby Poznámkový blok, jako je služba poznámkového bloku Azure HDInsight Jupyter, můžete použít **spark magic** příkazy:</span><span class="sxs-lookup"><span data-stu-id="aee73-189">If you are using a notebook service such as Azure HDInsight Jupyter notebook service, you can use the **spark magic** commands:</span></span>

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

<span data-ttu-id="aee73-190">**JAR** příkaz umožňuje zahrnout dva JAR, které jsou potřebné pro **azure. cosmosdb spark** (samostatně a Azure DocumentDB Java SDK) a vyloučit **scala-odráží** tak, že nebudou v konfliktu s volá Livy (Poznámkový blok Jupyter > Livy > Spark).</span><span class="sxs-lookup"><span data-stu-id="aee73-190">The **jars** command enables you to include the two JARs that are needed for **azure-cosmosdb-spark** (itself and the Azure DocumentDB Java SDK) and exclude **scala-reflect** so that it does not interfere with the Livy calls (Jupyter notebook > Livy > Spark).</span></span>

### <a name="connect-spark-to-azure-cosmos-db-using-the-connector"></a><span data-ttu-id="aee73-191">Spark připojit k databázi Cosmos Azure pomocí služby connector</span><span class="sxs-lookup"><span data-stu-id="aee73-191">Connect Spark to Azure Cosmos DB using the connector</span></span>
<span data-ttu-id="aee73-192">I když přenos komunikace je trochu složitější, provádění dotazu z Spark pro Azure Cosmos DB pomocí konektoru je výrazně rychlejší.</span><span class="sxs-lookup"><span data-stu-id="aee73-192">Although the communication transport is a little more complicated, executing a query from Spark to Azure Cosmos DB by using the connector is significantly faster.</span></span>

<span data-ttu-id="aee73-193">Následující fragment kódu ukazuje, jak k používání konektoru v kontextu Spark.</span><span class="sxs-lookup"><span data-stu-id="aee73-193">The following code snippet shows how to use the connector in a Spark context.</span></span>

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection to your collection
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

<span data-ttu-id="aee73-194">Jak jsme uvedli v fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="aee73-194">As noted in the code snippet:</span></span>

- <span data-ttu-id="aee73-195">**Azure-cosmosdb-spark** obsahuje všechny potřebné parametry připojení, mezi které patří upřednostňované umístění.</span><span class="sxs-lookup"><span data-stu-id="aee73-195">**azure-cosmosdb-spark** contains the all the necessary connection parameters, which include the preferred locations.</span></span> <span data-ttu-id="aee73-196">Například můžete čtení pořadí repliky a s prioritou.</span><span class="sxs-lookup"><span data-stu-id="aee73-196">For example, you can choose the read replica and priority order.</span></span>
- <span data-ttu-id="aee73-197">Stačí importovat potřebné knihovny a nakonfigurujte masterKey a hostitele pro vytvoření klienta Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="aee73-197">Just import the necessary libraries and configure your masterKey and host to create the Azure Cosmos DB client.</span></span>

### <a name="execute-spark-queries-via-the-connector"></a><span data-ttu-id="aee73-198">Spuštění dotazů Spark prostřednictvím konektoru</span><span class="sxs-lookup"><span data-stu-id="aee73-198">Execute Spark queries via the connector</span></span>

<span data-ttu-id="aee73-199">Následující příklad používá Azure Cosmos DB instanci, která byla vytvořena v předchozím fragmentu kódu pomocí zadaného klíče jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="aee73-199">The following example uses the Azure Cosmos DB instance that was created in the previous snippet by using the specified read-only keys.</span></span> <span data-ttu-id="aee73-200">Následující fragment kódu připojí ke kolekci DepartureDelays.flights_pcoll (v účtu DoctorWho uvedeného výše) a spustí dotaz extrahovat informace o letu zpoždění letů, které jsou jednotlivé Praha.</span><span class="sxs-lookup"><span data-stu-id="aee73-200">The following code snippet connects to the DepartureDelays.flights_pcoll collection (in the DoctorWho account as specified earlier) and runs a query to extract the flight delay information of flights that are departing from Seattle.</span></span>

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-the-spark-to-azure-cosmos-db-connector-implementation"></a><span data-ttu-id="aee73-201">Proč používat Spark pro Azure Cosmos DB konektor implementaci?</span><span class="sxs-lookup"><span data-stu-id="aee73-201">Why use the Spark to Azure Cosmos DB connector implementation?</span></span>

<span data-ttu-id="aee73-202">Spark se připojit k databázi Cosmos Azure pomocí konektoru obvykle pro scénáře, kde:</span><span class="sxs-lookup"><span data-stu-id="aee73-202">Connecting Spark to Azure Cosmos DB by using the connector is typically for scenarios where:</span></span>

* <span data-ttu-id="aee73-203">Chcete použít Scala a aktualizovat tak, aby obsahovala obálku Python, jak jsme uvedli v [problém 3: Přidání Python obálku a příklady](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span><span class="sxs-lookup"><span data-stu-id="aee73-203">You want to use Scala and update it to include a Python wrapper as noted in [Issue 3: Add Python wrapper and examples](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span></span>
* <span data-ttu-id="aee73-204">Máte velký objem dat pro přenos mezi Apache Spark a Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="aee73-204">You have a large amount of data to transfer between Apache Spark and Azure Cosmos DB.</span></span>

<span data-ttu-id="aee73-205">Poskytnout základní přehled rozdíl výkonu dotazu, najdete v článku [dotazu testovací spouští wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span><span class="sxs-lookup"><span data-stu-id="aee73-205">To give you an idea of the query performance difference, see the [Query Test Runs wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span></span>

## <a name="distributed-aggregation-example"></a><span data-ttu-id="aee73-206">Příklad distribuované agregace</span><span class="sxs-lookup"><span data-stu-id="aee73-206">Distributed aggregation example</span></span>
<span data-ttu-id="aee73-207">Tato část obsahuje příklady jak to distribuované agregací a analýzy pomocí Apache Spark a Azure Cosmos DB společně.</span><span class="sxs-lookup"><span data-stu-id="aee73-207">This section provides some examples of how you can do distributed aggregations and analytics by using Apache Spark and Azure Cosmos DB together.</span></span> <span data-ttu-id="aee73-208">Azure Cosmos DB již podporuje agregace, která je popsána v [planetu agregace škálování s Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span><span class="sxs-lookup"><span data-stu-id="aee73-208">Azure Cosmos DB already supports aggregations, which is discussed in the [Planet scale aggregates with Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span></span> <span data-ttu-id="aee73-209">Zde je, jak může trvat na další úroveň s Apache Spark.</span><span class="sxs-lookup"><span data-stu-id="aee73-209">Here is how you can take it to the next level with Apache Spark.</span></span>

<span data-ttu-id="aee73-210">Všimněte si, že tyto agregace jsou reference [Spark do poznámkového bloku konektoru DB Cosmos Azure](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span><span class="sxs-lookup"><span data-stu-id="aee73-210">Note that these aggregations are in reference to the [Spark to Azure Cosmos DB Connector notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span></span>

### <a name="connect-to-flights-sample-data"></a><span data-ttu-id="aee73-211">Připojení k lety ukázková data</span><span class="sxs-lookup"><span data-stu-id="aee73-211">Connect to flights sample data</span></span>
<span data-ttu-id="aee73-212">Tyto příklady agregace přístup k cestě výkonu data, která je uložena v našem **DoctorWho** databáze Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="aee73-212">These aggregation examples access some flight performance data that's stored in our **DoctorWho** Azure Cosmos DB database.</span></span> <span data-ttu-id="aee73-213">Chcete-li se k němu připojit, je potřeba využívat následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="aee73-213">To connect to it, you need to utilize the following code snippet:</span></span>

```
// Import Spark to Azure Cosmos DB connector
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Connect to Azure Cosmos DB Database
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

<span data-ttu-id="aee73-214">Tato fragmentem také přidáme ke spuštění základní dotazu, který převádí filtrovanou sadu dat z Azure Cosmos DB Spark kde k tomu můžete provést distribuované agregace.</span><span class="sxs-lookup"><span data-stu-id="aee73-214">With this snippet, we are also going to run a base query that transfers the filtered set of data from Azure Cosmos DB to Spark where the latter can perform distributed aggregates.</span></span> <span data-ttu-id="aee73-215">V takovém případě vás žádáme pro lety, které se liší od Praha (SEA).</span><span class="sxs-lookup"><span data-stu-id="aee73-215">In this case, we are asking for flights that depart from Seattle (SEA).</span></span>

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

<span data-ttu-id="aee73-216">Následující výsledky byly vygenerovány spuštěním dotazy ze služby poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="aee73-216">The following results were generated by running the queries from the Jupyter notebook service.</span></span>  <span data-ttu-id="aee73-217">Všimněte si, že všechny fragmenty kódu jsou obecné a není specifická pro libovolnou službu.</span><span class="sxs-lookup"><span data-stu-id="aee73-217">Note that all the code snippets are generic and not specific to any service.</span></span>

### <a name="running-limit-and-count-queries"></a><span data-ttu-id="aee73-218">LIMIT spuštěné a počet dotazů</span><span class="sxs-lookup"><span data-stu-id="aee73-218">Running LIMIT and COUNT queries</span></span>
<span data-ttu-id="aee73-219">Podobně jako jste zvyklí v SQL nebo Spark SQL, můžeme začít s **LIMIT** dotazu:</span><span class="sxs-lookup"><span data-stu-id="aee73-219">Just like you're used to in SQL/Spark SQL, let's start off with a **LIMIT** query:</span></span>

![Spark LIMIT dotazu](./media/spark-connector/spark-sql-query.png)

<span data-ttu-id="aee73-221">Další dotaz je jednoduchý a rychlé **počet** dotazu:</span><span class="sxs-lookup"><span data-stu-id="aee73-221">The next query is a simple and fast **COUNT** query:</span></span>

![Dotazu Spark COUNT](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a><span data-ttu-id="aee73-223">GROUP BY dotazu</span><span class="sxs-lookup"><span data-stu-id="aee73-223">GROUP BY query</span></span>
<span data-ttu-id="aee73-224">V této sadě další můžeme jednoduše spustit **GROUP BY** dotazy pro Azure Cosmos DB databáze:</span><span class="sxs-lookup"><span data-stu-id="aee73-224">In this next set, we can easily run **GROUP BY** queries against our Azure Cosmos DB database:</span></span>

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY dotazu grafu.](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a><span data-ttu-id="aee73-226">JEDINEČNÉ, ORDER BY dotazu</span><span class="sxs-lookup"><span data-stu-id="aee73-226">DISTINCT, ORDER BY query</span></span>
<span data-ttu-id="aee73-227">A tady je **DISTINCT, ORDER BY** dotazu:</span><span class="sxs-lookup"><span data-stu-id="aee73-227">And here is a **DISTINCT, ORDER BY** query:</span></span>

![Spark GROUP BY dotazu grafu.](./media/spark-connector/order-by-query.png)

### <a name="continue-the-flight-data-analysis"></a><span data-ttu-id="aee73-229">Analýza dat letu pokračovat</span><span class="sxs-lookup"><span data-stu-id="aee73-229">Continue the flight data analysis</span></span>
<span data-ttu-id="aee73-230">Následující příklady dotazů můžete pokračovat analýzu dat cestě:</span><span class="sxs-lookup"><span data-stu-id="aee73-230">You can use the following example queries to continue analysis of the flight data:</span></span>

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a><span data-ttu-id="aee73-231">TOP 5 Zpožděné cíle (města) jednotlivé Praha</span><span class="sxs-lookup"><span data-stu-id="aee73-231">Top 5 delayed destinations (cities) departing from Seattle</span></span>
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark nejvyšší zpoždění grafu](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a><span data-ttu-id="aee73-233">Vypočítat střední zpoždění podle měst cílové jednotlivé Praha</span><span class="sxs-lookup"><span data-stu-id="aee73-233">Calculate median delays by destination cities departing from Seattle</span></span>
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark střední zpoždění grafu](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a><span data-ttu-id="aee73-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aee73-235">Next steps</span></span>

<span data-ttu-id="aee73-236">Pokud jste tak ještě neučinili, stáhněte Spark pro Azure Cosmos DB konektor z [azure. cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) úložiště GitHub a seznamte se s další prostředky v úložišti:</span><span class="sxs-lookup"><span data-stu-id="aee73-236">If you haven't already, download the Spark to Azure Cosmos DB connector from the [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub repository and explore the additional resources in the repo:</span></span>

* [<span data-ttu-id="aee73-237">Příklady distribuované agregace</span><span class="sxs-lookup"><span data-stu-id="aee73-237">Distributed Aggregations Examples</span></span>](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [<span data-ttu-id="aee73-238">Ukázkové skripty a poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="aee73-238">Sample Scripts and Notebooks</span></span>](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

<span data-ttu-id="aee73-239">Můžete také zkontrolovat [Apache Spark SQL, průvodce datové sady a DataFrames](http://spark.apache.org/docs/latest/sql-programming-guide.html) a [Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) článku.</span><span class="sxs-lookup"><span data-stu-id="aee73-239">You might also want to review the [Apache Spark SQL, DataFrames, and Datasets Guide](http://spark.apache.org/docs/latest/sql-programming-guide.html) and the [Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) article.</span></span>
