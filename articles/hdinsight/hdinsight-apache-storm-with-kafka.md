---
title: "Pomocí Apache Kafka Storm v HDInsight - Azure | Microsoft Docs"
description: "Apache Kafka se instaluje s Apache Storm v HDInsight. Naučte se zapsat do Kafka a pak čtení z něj, pomocí KafkaBolt a KafkaSpout součásti, které jsou součástí Storm. Také další informace o použití rozhraní tok definovat a odeslání topologie Storm."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e4941329-1580-4cd8-b82e-a2258802c1a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: larryfr
ms.openlocfilehash: e8895ef3c11aea48513e4060a20f5f49b11fc961
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a><span data-ttu-id="43ca0-105">Storm v HDInsight pomocí Apache Kafka (preview)</span><span class="sxs-lookup"><span data-stu-id="43ca0-105">Use Apache Kafka (preview) with Storm on HDInsight</span></span>

<span data-ttu-id="43ca0-106">Další informace o použití Apache Storm číst a zapisovat do Apache Kafka.</span><span class="sxs-lookup"><span data-stu-id="43ca0-106">Learn how to use Apache Storm to read from and write to Apache Kafka.</span></span> <span data-ttu-id="43ca0-107">Tento příklad také ukazuje, jak k uložení dat od topologie Storm do systému souborů HDFS kompatibilního používané HDInsight.</span><span class="sxs-lookup"><span data-stu-id="43ca0-107">This example also demonstrates how to save data from a Storm topology to the HDFS-compatible file system used by HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="43ca0-108">Kroky v tomto dokumentu vytvořte skupinu prostředků Azure, která obsahuje oba Storm v HDInsight a Kafka na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="43ca0-108">The steps in this document create an Azure resource group that contains both a Storm on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="43ca0-109">Tyto clustery jsou obě nachází v rámci virtuální síť Azure, což umožňuje clusteru Storm přímo komunikovat s Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-109">These clusters are both located within an Azure Virtual Network, which allows the Storm cluster to directly communicate with the Kafka cluster.</span></span>
> 
> <span data-ttu-id="43ca0-110">Po dokončení kroků v tomto dokumentu, nezapomeňte odstranit clustery nadbytečné náklady.</span><span class="sxs-lookup"><span data-stu-id="43ca0-110">When you are done with the steps in this document, remember to delete the clusters to avoid excess charges.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="43ca0-111">Získání kódu</span><span class="sxs-lookup"><span data-stu-id="43ca0-111">Get the code</span></span>

<span data-ttu-id="43ca0-112">Kód pro tento příklad v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span><span class="sxs-lookup"><span data-stu-id="43ca0-112">The code for the example used in this document is available at [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span></span>

<span data-ttu-id="43ca0-113">Kompilace projektu, potřebujete následující konfigurace pro vývojové prostředí:</span><span class="sxs-lookup"><span data-stu-id="43ca0-113">To compile this project, you need the following configuration for your development environment:</span></span>

* <span data-ttu-id="43ca0-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="43ca0-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span> <span data-ttu-id="43ca0-115">HDInsight 3.5 nebo vyšší vyžadují Java 8.</span><span class="sxs-lookup"><span data-stu-id="43ca0-115">HDInsight 3.5 or higher require Java 8.</span></span>

* [<span data-ttu-id="43ca0-116">Maven 3.x</span><span class="sxs-lookup"><span data-stu-id="43ca0-116">Maven 3.x</span></span>](https://maven.apache.org/download.cgi)

* <span data-ttu-id="43ca0-117">Klientem SSH (je třeba `ssh` a `scp` příkazy) – informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="43ca0-117">An SSH client (you need the `ssh` and `scp` commands) - For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="43ca0-118">Textového editoru nebo IDE.</span><span class="sxs-lookup"><span data-stu-id="43ca0-118">A text editor or IDE.</span></span>

<span data-ttu-id="43ca0-119">Následující proměnné prostředí může být nastaven při instalaci Java a sadu JDK na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="43ca0-119">The following environment variables may be set when you install Java and the JDK on your development workstation.</span></span> <span data-ttu-id="43ca0-120">Nicméně byste měli zkontrolovat, že existují a že obsahují správné hodnoty pro váš systém.</span><span class="sxs-lookup"><span data-stu-id="43ca0-120">However, you should check that they exist and that they contain the correct values for your system.</span></span>

* <span data-ttu-id="43ca0-121">`JAVA_HOME`-by měla odkazovat na adresář, kam nainstalovat sadu JDK.</span><span class="sxs-lookup"><span data-stu-id="43ca0-121">`JAVA_HOME` - should point to the directory where the JDK is installed.</span></span>
* <span data-ttu-id="43ca0-122">`PATH`-musí obsahovat následující cesty:</span><span class="sxs-lookup"><span data-stu-id="43ca0-122">`PATH` - should contain the following paths:</span></span>
  
    * <span data-ttu-id="43ca0-123">`JAVA_HOME`(nebo ekvivalentní cesta).</span><span class="sxs-lookup"><span data-stu-id="43ca0-123">`JAVA_HOME` (or the equivalent path).</span></span>
    * <span data-ttu-id="43ca0-124">`JAVA_HOME\bin`(nebo ekvivalentní cesta).</span><span class="sxs-lookup"><span data-stu-id="43ca0-124">`JAVA_HOME\bin` (or the equivalent path).</span></span>
    * <span data-ttu-id="43ca0-125">Adresář, kde je nainstalován Maven.</span><span class="sxs-lookup"><span data-stu-id="43ca0-125">The directory where Maven is installed.</span></span>

## <a name="create-the-clusters"></a><span data-ttu-id="43ca0-126">Vytváření clusterů</span><span class="sxs-lookup"><span data-stu-id="43ca0-126">Create the clusters</span></span>

<span data-ttu-id="43ca0-127">Apache Kafka v HDInsight neposkytuje přístup k zprostředkovatelé Kafka prostřednictvím veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-127">Apache Kafka on HDInsight does not provide access to the Kafka brokers over the public internet.</span></span> <span data-ttu-id="43ca0-128">Všechno, co komunikuje se Kafka musí být ve stejné virtuální síti Azure jako uzly v clusteru Kafka.</span><span class="sxs-lookup"><span data-stu-id="43ca0-128">Anything that talks to Kafka must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="43ca0-129">V tomto příkladu jsou Kafka i Storm clusterů umístěné v virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="43ca0-129">For this example, both the Kafka and Storm clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="43ca0-130">Následující diagram znázorňuje tok komunikace mezi clustery:</span><span class="sxs-lookup"><span data-stu-id="43ca0-130">The following diagram shows how communication flows between the clusters:</span></span>

![Diagram clustery Storm a Kafka v virtuální sítě Azure](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="43ca0-132">Jiné služby v clusteru, například SSH a Ambari je přístupná prostřednictvím Internetu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-132">Other services on the cluster such as SSH and Ambari can be accessed over the internet.</span></span> <span data-ttu-id="43ca0-133">Další informace o veřejné porty, které jsou k dispozici s HDInsight naleznete v tématu [porty a identifikátory URI používají v prostředí HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="43ca0-133">For more information on the public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="43ca0-134">Když vytvoříte virtuální síť Azure, Kafka, a clusterů Storm se ručně, je jednodušší použít šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="43ca0-134">While you can create an Azure virtual network, Kafka, and Storm clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="43ca0-135">Použijte následující kroky k nasazení virtuální sítě Azure, Kafka a Storm clusterů do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="43ca0-135">Use the following steps to deploy an Azure virtual network, Kafka, and Storm clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="43ca0-136">Na následující tlačítko použijte pro přihlášení do Azure a otevřete šablonu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="43ca0-136">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    <span data-ttu-id="43ca0-137">Šablona Azure Resource Manager je umístěna ve **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span><span class="sxs-lookup"><span data-stu-id="43ca0-137">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span></span> <span data-ttu-id="43ca0-138">Vytvoří v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="43ca0-138">It creates the following resources:</span></span>
    
    * <span data-ttu-id="43ca0-139">Skupina prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="43ca0-139">Azure resource group</span></span>
    * <span data-ttu-id="43ca0-140">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="43ca0-140">Azure Virtual Network</span></span>
    * <span data-ttu-id="43ca0-141">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="43ca0-141">Azure Storage account</span></span>
    * <span data-ttu-id="43ca0-142">Kafka v HDInsight verze 3.6 (tři uzly pracovního procesu)</span><span class="sxs-lookup"><span data-stu-id="43ca0-142">Kafka on HDInsight version 3.6 (three worker nodes)</span></span>
    * <span data-ttu-id="43ca0-143">Storm v HDInsight verze 3.6 (tři uzly pracovního procesu)</span><span class="sxs-lookup"><span data-stu-id="43ca0-143">Storm on HDInsight version 3.6 (three worker nodes)</span></span>

  > [!WARNING]
  > <span data-ttu-id="43ca0-144">Pokud chcete zajistit dostupnost Kafka v HDInsightu, musí cluster obsahovat aspoň tři pracovní uzly.</span><span class="sxs-lookup"><span data-stu-id="43ca0-144">To guarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="43ca0-145">Tato šablona vytvoří cluster Kafka, který obsahuje tři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-145">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="43ca0-146">Použijte následující pokyny k naplnění položek na **vlastní nasazení** okno:</span><span class="sxs-lookup"><span data-stu-id="43ca0-146">Use the following guidance to populate the entries on the **Custom deployment** blade:</span></span>
   
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * <span data-ttu-id="43ca0-148">**Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="43ca0-148">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="43ca0-149">Tato skupina obsahuje clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="43ca0-149">This group contains the HDInsight cluster.</span></span>
   
    * <span data-ttu-id="43ca0-150">**Umístění**: Vyberte umístění geograficky blízko vás.</span><span class="sxs-lookup"><span data-stu-id="43ca0-150">**Location**: Select a location geographically close to you.</span></span>

    * <span data-ttu-id="43ca0-151">**Základní název clusteru**: Tato hodnota se používá jako základní název pro Storm a Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="43ca0-151">**Base Cluster Name**: This value is used as the base name for the Storm and Kafka clusters.</span></span> <span data-ttu-id="43ca0-152">Například zadáním **hdi** vytvoří cluster Storm s názvem **storm hdi** a Kafka clusteru s názvem **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="43ca0-152">For example, entering **hdi** creates a Storm cluster named **storm-hdi** and a Kafka cluster named **kafka-hdi**.</span></span>
   
    * <span data-ttu-id="43ca0-153">**Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce pro clustery Storm a Kafka.</span><span class="sxs-lookup"><span data-stu-id="43ca0-153">**Cluster Login User Name**: The admin user name for the Storm and Kafka clusters.</span></span>
   
    * <span data-ttu-id="43ca0-154">**Heslo pro přihlášení clusteru**: uživatelské heslo správce pro clustery Storm a Kafka.</span><span class="sxs-lookup"><span data-stu-id="43ca0-154">**Cluster Login Password**: The admin user password for the Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="43ca0-155">**Uživatelské jméno SSH**: SSH, aby uživatel vytvořil pro clustery Storm a Kafka.</span><span class="sxs-lookup"><span data-stu-id="43ca0-155">**SSH User Name**: The SSH user to create for the Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="43ca0-156">**Heslo SSH**: heslo pro uživatele SSH pro clustery Storm a Kafka.</span><span class="sxs-lookup"><span data-stu-id="43ca0-156">**SSH Password**: The password for the SSH user for the Storm and Kafka clusters.</span></span>

3. <span data-ttu-id="43ca0-157">Pro čtení **podmínky a ujednání**a potom vyberte **souhlasím s podmínkami a ujednáními výše uvedených**.</span><span class="sxs-lookup"><span data-stu-id="43ca0-157">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="43ca0-158">Nakonec zkontrolujte **připnout na řídicí panel** a pak vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="43ca0-158">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="43ca0-159">Chcete-li vytvořit clustery trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="43ca0-159">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="43ca0-160">Po vytvoření prostředky, zobrazí se okno pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="43ca0-160">Once the resources have been created, the blade for the resource group is displayed.</span></span>

![Okno skupiny prostředků pro virtuální síť a clustery](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="43ca0-162">Všimněte si, že jsou názvy clusterů HDInsight **storm BASENAME** a **kafka BASENAME**, kde BASENAME je jméno, které jste zadali v šabloně.</span><span class="sxs-lookup"><span data-stu-id="43ca0-162">Notice that the names of the HDInsight clusters are **storm-BASENAME** and **kafka-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="43ca0-163">Názvy těchto používat v dalších krocích při připojování k clustery.</span><span class="sxs-lookup"><span data-stu-id="43ca0-163">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="understanding-the-code"></a><span data-ttu-id="43ca0-164">Pochopení kódu</span><span class="sxs-lookup"><span data-stu-id="43ca0-164">Understanding the code</span></span>

<span data-ttu-id="43ca0-165">Tento projekt obsahuje dvě topologie:</span><span class="sxs-lookup"><span data-stu-id="43ca0-165">This project contains two topologies:</span></span>

* <span data-ttu-id="43ca0-166">**KafkaWriter**: určené **writer.yaml** souborů, tato topologie zapíše náhodných věty Kafka pomocí KafkaBolt s Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="43ca0-166">**KafkaWriter**: Defined by the **writer.yaml** file, this topology writes random sentences to Kafka using the KafkaBolt provided with Apache Storm.</span></span>

    <span data-ttu-id="43ca0-167">Tato topologie používá vlastní **SentenceSpout** součásti pro generování náhodných věty.</span><span class="sxs-lookup"><span data-stu-id="43ca0-167">This topology uses a custom **SentenceSpout** component to generate random sentences.</span></span>

* <span data-ttu-id="43ca0-168">**KafkaReader**: určené **reader.yaml** soubor, tato topologie čte data z Kafka pomocí KafkaSpout s Apache Storm a poté protokoluje data do stdout.</span><span class="sxs-lookup"><span data-stu-id="43ca0-168">**KafkaReader**: Defined by the **reader.yaml** file, this topology reads data from Kafka using the KafkaSpout provided with Apache Storm, then logs the data to stdout.</span></span>

    <span data-ttu-id="43ca0-169">Tato topologie Storm HdfsBolt používá k zápisu dat do výchozího úložiště pro Storm cluster.</span><span class="sxs-lookup"><span data-stu-id="43ca0-169">This topology uses the Storm HdfsBolt to write data to default storage for the Storm cluster.</span></span>
### <a name="flux"></a><span data-ttu-id="43ca0-170">Tok</span><span class="sxs-lookup"><span data-stu-id="43ca0-170">Flux</span></span>

<span data-ttu-id="43ca0-171">Uvedené topologie jsou definovány pomocí [tok](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="43ca0-171">The topologies are defined using [Flux](https://storm.apache.org/releases/1.1.0/flux.html).</span></span> <span data-ttu-id="43ca0-172">Tok byla zavedena v Storm 0.10.x a umožňuje oddělit konfiguraci topologie z kódu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-172">Flux was introduced in Storm 0.10.x and allows you to separate the topology configuration from the code.</span></span> <span data-ttu-id="43ca0-173">Pro topologie, které používají rozhraní tok topologii je definována v souboru YAML.</span><span class="sxs-lookup"><span data-stu-id="43ca0-173">For Topologies that use the Flux framework, the topology is defined in a YAML file.</span></span> <span data-ttu-id="43ca0-174">Soubor YAML může být součástí topologii.</span><span class="sxs-lookup"><span data-stu-id="43ca0-174">The YAML file can be included as part of the topology.</span></span> <span data-ttu-id="43ca0-175">Také může být samostatný soubor používá při odesílání topologie.</span><span class="sxs-lookup"><span data-stu-id="43ca0-175">It can also be a standalone file used when you submit the topology.</span></span> <span data-ttu-id="43ca0-176">Tok také podporuje nahrazení proměnné za běhu, který se používá v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-176">Flux also supports variable substitution at run-time, which is used in this example.</span></span>

<span data-ttu-id="43ca0-177">Následující parametry jsou nastavené v době běhu pro tyto topologie:</span><span class="sxs-lookup"><span data-stu-id="43ca0-177">The following parameters are set at run time for these topologies:</span></span>

* <span data-ttu-id="43ca0-178">`${kafka.topic}`: Název tématu Kafka, které topologie pro čtení a zápis k.</span><span class="sxs-lookup"><span data-stu-id="43ca0-178">`${kafka.topic}`: The name of the Kafka topic that the topologies read/write to.</span></span>

* <span data-ttu-id="43ca0-179">`${kafka.broker.hosts}`: Hostitelů, které zprostředkovává Kafka na spustit.</span><span class="sxs-lookup"><span data-stu-id="43ca0-179">`${kafka.broker.hosts}`: The hosts that the Kafka brokers run on.</span></span> <span data-ttu-id="43ca0-180">Zprostředkovatel informace využívají KafkaBolt při zápisu do Kafka.</span><span class="sxs-lookup"><span data-stu-id="43ca0-180">The broker information is used by the KafkaBolt when writing to Kafka.</span></span>

* <span data-ttu-id="43ca0-181">`${kafka.zookeeper.hosts}`: Hostitelů, které Zookeeper běží na clusteru Kafka.</span><span class="sxs-lookup"><span data-stu-id="43ca0-181">`${kafka.zookeeper.hosts}`: The hosts that Zookeeper runs on in the Kafka cluster.</span></span>

<span data-ttu-id="43ca0-182">Další informace o topologiích tok najdete v tématu [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="43ca0-182">For more information on Flux topologies, see [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span></span>

## <a name="download-and-compile-the-project"></a><span data-ttu-id="43ca0-183">Stáhněte si a kompilaci projektu</span><span class="sxs-lookup"><span data-stu-id="43ca0-183">Download and compile the project</span></span>

1. <span data-ttu-id="43ca0-184">Na vašem vývojovém prostředí, stáhněte si projekt z [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), otevřete příkazového řádku a změňte adresáře na umístění, který jste si stáhli projektu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-184">On your development environment, download the project from [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), open a command-line, and change directories to the location that you downloaded the project.</span></span>

2. <span data-ttu-id="43ca0-185">Z **hdinsight storm java kafka** adresáře, použijte následující příkaz pro kompilaci projektu a vytvořit balíček pro nasazení:</span><span class="sxs-lookup"><span data-stu-id="43ca0-185">From the **hdinsight-storm-java-kafka** directory, use the following command to compile the project and create a package for deployment:</span></span>

  ```bash
  mvn clean package
  ```

    <span data-ttu-id="43ca0-186">Proces balíčku vytvoří soubor s názvem `KafkaTopology-1.0-SNAPSHOT.jar` v `target` adresáře.</span><span class="sxs-lookup"><span data-stu-id="43ca0-186">The package process creates a file named `KafkaTopology-1.0-SNAPSHOT.jar` in the `target` directory.</span></span>

3. <span data-ttu-id="43ca0-187">Použijte následující příkazy ke zkopírování balíčku pro váš cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="43ca0-187">Use the following commands to copy the package to your Storm on HDInsight cluster.</span></span> <span data-ttu-id="43ca0-188">Nahraďte **uživatelské jméno** s uživatelským jménem SSH pro cluster.</span><span class="sxs-lookup"><span data-stu-id="43ca0-188">Replace **USERNAME** with the SSH user name for the cluster.</span></span> <span data-ttu-id="43ca0-189">Nahraďte **BASENAME** s základní název, který jste použili při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-189">Replace **BASENAME** with the base name you used when creating the cluster.</span></span>

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    <span data-ttu-id="43ca0-190">Po zobrazení výzvy zadejte heslo, které jste použili při vytváření clusterů.</span><span class="sxs-lookup"><span data-stu-id="43ca0-190">When prompted, enter the password you used when creating the clusters.</span></span>

## <a name="configure-the-topology"></a><span data-ttu-id="43ca0-191">Konfigurace topologie</span><span class="sxs-lookup"><span data-stu-id="43ca0-191">Configure the topology</span></span>

1. <span data-ttu-id="43ca0-192">Použijte jednu z následujících metod zjišťování hostitelů Kafka zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="43ca0-192">Use one of the following methods to discover the Kafka broker hosts:</span></span>

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($brokerHosts -join ":9092,") + ":9092"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="43ca0-193">Bash příklad předpokládá, že `$CLUSTERNAME` obsahuje název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="43ca0-193">The Bash example assumes that `$CLUSTERNAME` contains the name of the HDInsight cluster.</span></span> <span data-ttu-id="43ca0-194">Dále předpokládá, že [jq](https://stedolan.github.io/jq/) je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="43ca0-194">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="43ca0-195">Po zobrazení výzvy zadejte heslo pro účet přihlášení clusteru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-195">When prompted, enter the password for the cluster login account.</span></span>

    <span data-ttu-id="43ca0-196">Hodnota vrácená je podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="43ca0-196">The value returned is similar to the following text:</span></span>

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > <span data-ttu-id="43ca0-197">Ačkoli může existovat více než dva hostitele zprostředkovatele pro váš cluster, není potřeba poskytnout úplný seznam všech hostitelů na klienty.</span><span class="sxs-lookup"><span data-stu-id="43ca0-197">While there may be more than two broker hosts for your cluster, you do not need to provide a full list of all hosts to clients.</span></span> <span data-ttu-id="43ca0-198">Jeden nebo dva kusy je dost.</span><span class="sxs-lookup"><span data-stu-id="43ca0-198">One or two is enough.</span></span>

2. <span data-ttu-id="43ca0-199">Ke zjištění hostitelů Kafka Zookeeper, použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="43ca0-199">Use one of the following methods to discover the Kafka Zookeeper hosts:</span></span>

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $zookeeperHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($zookeeperHosts -join ":2181,") + ":2181"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="43ca0-200">Bash příklad předpokládá, že `$CLUSTERNAME` obsahuje název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="43ca0-200">The Bash example assumes that `$CLUSTERNAME` contains the name of the HDInsight cluster.</span></span> <span data-ttu-id="43ca0-201">Dále předpokládá, že [jq](https://stedolan.github.io/jq/) je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="43ca0-201">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="43ca0-202">Po zobrazení výzvy zadejte heslo pro účet přihlášení clusteru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-202">When prompted, enter the password for the cluster login account.</span></span>

    <span data-ttu-id="43ca0-203">Hodnota vrácená je podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="43ca0-203">The value returned is similar to the following text:</span></span>

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > <span data-ttu-id="43ca0-204">Existuje více než dva uzly Zookeeper, není potřeba poskytnout úplný seznam všech hostitelů na klienty.</span><span class="sxs-lookup"><span data-stu-id="43ca0-204">While there are more than two Zookeeper nodes, you do not need to provide a full list of all hosts to clients.</span></span> <span data-ttu-id="43ca0-205">Jeden nebo dva kusy je dost.</span><span class="sxs-lookup"><span data-stu-id="43ca0-205">One or two is enough.</span></span>

    <span data-ttu-id="43ca0-206">Tato hodnota, uložte, protože se později používá.</span><span class="sxs-lookup"><span data-stu-id="43ca0-206">Save this value, as it is used later.</span></span>

3. <span data-ttu-id="43ca0-207">Upravit `dev.properties` souboru v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-207">Edit the `dev.properties` file in the root of the project.</span></span> <span data-ttu-id="43ca0-208">Přidáte hostitele informace, zprostředkovatele a Zookeeper odpovídající řádky v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-208">Add the Broker and Zookeeper hosts information to the matching lines in this file.</span></span> <span data-ttu-id="43ca0-209">V následujícím příkladu je nakonfigurované použití ukázkové hodnoty z předchozích kroků:</span><span class="sxs-lookup"><span data-stu-id="43ca0-209">The following example is configured using the sample values from the previous steps:</span></span>

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. <span data-ttu-id="43ca0-210">Uložit `dev.properties` souboru a pak ji nahrát do clusteru Storm použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="43ca0-210">Save the `dev.properties` file and then use the following command to upload it to the Storm cluster:</span></span>

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="43ca0-211">Nahraďte **uživatelské jméno** s uživatelským jménem SSH pro cluster.</span><span class="sxs-lookup"><span data-stu-id="43ca0-211">Replace **USERNAME** with the SSH user name for the cluster.</span></span> <span data-ttu-id="43ca0-212">Nahraďte **BASENAME** s základní název, který jste použili při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-212">Replace **BASENAME** with the base name you used when creating the cluster.</span></span>

## <a name="start-the-writer"></a><span data-ttu-id="43ca0-213">Spusťte modul pro zápis</span><span class="sxs-lookup"><span data-stu-id="43ca0-213">Start the writer</span></span>

1. <span data-ttu-id="43ca0-214">Použijte následující se připojit ke clusteru Storm pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="43ca0-214">Use the following to connect to the Storm cluster using SSH.</span></span> <span data-ttu-id="43ca0-215">Nahraďte **uživatelské jméno** s uživatelským jménem SSH použít při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-215">Replace **USERNAME** with the SSH user name used when creating the cluster.</span></span> <span data-ttu-id="43ca0-216">Nahraďte **BASENAME** s základní název použít při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-216">Replace **BASENAME** with the base name used when creating the cluster.</span></span>

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    <span data-ttu-id="43ca0-217">Po zobrazení výzvy zadejte heslo, které jste použili při vytváření clusterů.</span><span class="sxs-lookup"><span data-stu-id="43ca0-217">When prompted, enter the password you used when creating the clusters.</span></span>
   
    <span data-ttu-id="43ca0-218">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="43ca0-218">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="43ca0-219">Připojení SSH můžete vytvořit téma Kafka používané topologii následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="43ca0-219">From the SSH connection, use the following command to create the Kafka topic used by the topology:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    <span data-ttu-id="43ca0-220">Nahraďte `$KAFKAZKHOSTS` s Zookeeper hostitele informace, které jste získali v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="43ca0-220">Replace `$KAFKAZKHOSTS` with the Zookeeper host information you retrieved in the previous section.</span></span>

2. <span data-ttu-id="43ca0-221">Připojení SSH do clusteru Storm můžete spustit topologie zapisovače následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="43ca0-221">From the SSH connection to the Storm cluster, use the following command to start the writer topology:</span></span>

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    <span data-ttu-id="43ca0-222">Parametry tohoto příkazu jsou:</span><span class="sxs-lookup"><span data-stu-id="43ca0-222">The parameters used with this command are:</span></span>

    * <span data-ttu-id="43ca0-223">`org.apache.storm.flux.Flux`: Pomocí tok pro konfiguraci a spuštění této topologii.</span><span class="sxs-lookup"><span data-stu-id="43ca0-223">`org.apache.storm.flux.Flux`: Use Flux to configure and run this topology.</span></span>

    * <span data-ttu-id="43ca0-224">`--remote`: Odešlete topologii Nimbus.</span><span class="sxs-lookup"><span data-stu-id="43ca0-224">`--remote`: Submit the topology to Nimbus.</span></span> <span data-ttu-id="43ca0-225">Topologie se distribuuje do uzlů pracovního procesu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-225">The topology is distributed across the worker nodes in the cluster.</span></span>

    * <span data-ttu-id="43ca0-226">`-R /writer.yaml`: Pomocí `writer.yaml` souboru ke konfiguraci topologie.</span><span class="sxs-lookup"><span data-stu-id="43ca0-226">`-R /writer.yaml`: Use the `writer.yaml` file to configure the topology.</span></span> <span data-ttu-id="43ca0-227">`-R`Označuje, že tento prostředek je součástí na soubor jar.</span><span class="sxs-lookup"><span data-stu-id="43ca0-227">`-R` indicates that this resource is included in the jar file.</span></span> <span data-ttu-id="43ca0-228">Proto je v kořenovém jar, `/writer.yaml` je cesta k němu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-228">It's in the root of the jar, so `/writer.yaml` is the path to it.</span></span>

    * <span data-ttu-id="43ca0-229">`--filter`: Naplnění položek v `writer.yaml` topologie pomocí hodnot v `dev.properties` souboru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-229">`--filter`: Populate entries in the `writer.yaml` topology using values in the `dev.properties` file.</span></span> <span data-ttu-id="43ca0-230">Například hodnota `kafka.topic` položku v souboru se používá k nahrazení `${kafka.topic}` položku v definici topologie.</span><span class="sxs-lookup"><span data-stu-id="43ca0-230">For example, the value of the `kafka.topic` entry in the file is used to replace the `${kafka.topic}` entry in the topology definition.</span></span>

5. <span data-ttu-id="43ca0-231">Po zahájení topologii, použijte následující příkaz k ověření, že je zápis dat do tématu Kafka:</span><span class="sxs-lookup"><span data-stu-id="43ca0-231">Once the topology has started, use the following command to verify that it is writing data to the Kafka topic:</span></span>

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    <span data-ttu-id="43ca0-232">Nahraďte `$KAFKAZKHOSTS` s Zookeeper hostitele informace, které jste získali v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="43ca0-232">Replace `$KAFKAZKHOSTS` with the Zookeeper host information you retrieved in the previous section.</span></span>

    <span data-ttu-id="43ca0-233">Tento příkaz skriptu součástí Kafka používá k monitorování tématu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-233">This command uses a script shipped with Kafka to monitor the topic.</span></span> <span data-ttu-id="43ca0-234">Po chvíli by se měl spustit vrací náhodné věty, které byly zapsány do tématu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-234">After a moment, it should start returning random sentences that have been written to the topic.</span></span> <span data-ttu-id="43ca0-235">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="43ca0-235">The output is similar to the following example:</span></span>

        i am at two with nature             
        an apple a day keeps the doctor away
        snow white and the seven dwarfs     
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        four score and seven years ago      
        snow white and the seven dwarfs     
        snow white and the seven dwarfs     
        i am at two with nature             
        an apple a day keeps the doctor away

    <span data-ttu-id="43ca0-236">Zastavit tento skript pomocí kombinace kláves Ctrl + c.</span><span class="sxs-lookup"><span data-stu-id="43ca0-236">Use Ctrl+c to stop the script.</span></span>

## <a name="start-the-reader"></a><span data-ttu-id="43ca0-237">Spustit program pro čtení</span><span class="sxs-lookup"><span data-stu-id="43ca0-237">Start the reader</span></span>

1. <span data-ttu-id="43ca0-238">Z relace SSH do clusteru Storm použijte následující příkaz spusťte čtečky topologie:</span><span class="sxs-lookup"><span data-stu-id="43ca0-238">From the SSH session to the Storm cluster, use the following command to start the reader topology:</span></span>

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. <span data-ttu-id="43ca0-239">Jakmile se spustí topologii, otevřete uživatelské rozhraní Storm.</span><span class="sxs-lookup"><span data-stu-id="43ca0-239">Once the topology starts, open the Storm UI.</span></span> <span data-ttu-id="43ca0-240">Tato webového uživatelského rozhraní se nachází v https://storm-BASENAME.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="43ca0-240">This web UI is located at https://storm-BASENAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="43ca0-241">Nahraďte __BASENAME__ s základní název použita při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-241">Replace __BASENAME__ with the base name used when the cluster was created.</span></span> 

    <span data-ttu-id="43ca0-242">Pokud budete vyzváni, použijte přihlašovací jméno správce (výchozím `admin`) a heslo používané při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="43ca0-242">When prompted, use the admin login name (default, `admin`) and password used when the cluster was created.</span></span> <span data-ttu-id="43ca0-243">Zobrazí na webové stránce, podobně jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="43ca0-243">You see a web page similar to the following image:</span></span>

    ![Storm uživatelského rozhraní](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. <span data-ttu-id="43ca0-245">Uživatelské rozhraní Storm, vyberte __kafka čtečky__ na odkaz v __souhrn topologie__ části zobrazíte informace o __kafka čtečky__ topologie.</span><span class="sxs-lookup"><span data-stu-id="43ca0-245">From the Storm UI, select the __kafka-reader__ link in the __Topology Summary__ section to display information about the __kafka-reader__ topology.</span></span>

    ![Souhrn část topologie Storm webového uživatelského rozhraní](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. <span data-ttu-id="43ca0-247">Chcete-li zobrazit informace o instancích protokolovacího nástroje bolt součásti, vyberte __protokolovacího nástroje bolt__ na odkaz v __funkce Bolts (všechny čas)__ části.</span><span class="sxs-lookup"><span data-stu-id="43ca0-247">To display information about the instances of the logger-bolt component, select the __logger-bolt__ link in the __Bolts (All time)__ section.</span></span>

    ![Protokolovač bolt odkaz v části funkce bolts](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. <span data-ttu-id="43ca0-249">V __vykonavatelů__ vyberte pomocí odkazu v __Port__ sloupec pro zobrazení protokolování informací o tuto instanci součásti.</span><span class="sxs-lookup"><span data-stu-id="43ca0-249">In the __Executors__ section, select a link in the __Port__ column to display logging information about this instance of the component.</span></span>

    ![Vykonavatelů odkaz](./media/hdinsight-apache-storm-with-kafka/executors.png)

    <span data-ttu-id="43ca0-251">V protokolu obsahuje data načtená z tématu Kafka protokolu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-251">The log contains a log of the data read from the Kafka topic.</span></span> <span data-ttu-id="43ca0-252">Informace v protokolu je podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="43ca0-252">The information in the log is similar to the following text:</span></span>

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] the cow jumped over the moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: the cow jumped over the moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps the doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps the doctor away

## <a name="stop-the-topologies"></a><span data-ttu-id="43ca0-253">Zastavit uvedené topologie</span><span class="sxs-lookup"><span data-stu-id="43ca0-253">Stop the topologies</span></span>

<span data-ttu-id="43ca0-254">Z relace SSH do clusteru Storm použijte následující příkazy k zastavení topologie Storm:</span><span class="sxs-lookup"><span data-stu-id="43ca0-254">From an SSH session to the Storm cluster, use the following commands to stop the Storm topologies:</span></span>

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-the-cluster"></a><span data-ttu-id="43ca0-255">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="43ca0-255">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="43ca0-256">Vzhledem k tomu, že kroky v tomto dokumentu vytvořit oba clustery ve stejné skupině prostředků Azure, můžete odstranit skupinu prostředků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="43ca0-256">Since the steps in this document create both clusters in the same Azure resource group, you can delete the resource group in the Azure portal.</span></span> <span data-ttu-id="43ca0-257">Odstraňuje se skupina prostředků odebere všechny prostředky, které jsou vytvořené pomocí následujících tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="43ca0-257">Deleting the resource group removes all resources created by following this document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43ca0-258">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43ca0-258">Next steps</span></span>

<span data-ttu-id="43ca0-259">Další příklad topologie, které lze použít s Storm v HDInsight, naleznete v části [topologie Storm příklad a součásti](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="43ca0-259">For more example topologies that can be used with Storm on HDInsight, see [Example Storm topologies and components](hdinsight-storm-example-topology.md).</span></span>

<span data-ttu-id="43ca0-260">Informace o nasazení a monitorování topologií v HDInsight se systémem Linux najdete v tématu [nasazení a správa topologií Apache Storm v HDInsight se systémem Linux](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="43ca0-260">For information on deploying and monitoring topologies on Linux-based HDInsight, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>