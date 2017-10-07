---
title: aaaUse Kafka Apache se Storm v HDInsight - Azure | Microsoft Docs
description: "Apache Kafka se instaluje s Apache Storm v HDInsight. Zjistěte, jak toowrite tooKafka a pak pro čtení z něj pomocí hello KafkaBolt a KafkaSpout součásti, které jsou součástí Storm. Také zjistěte, jak toouse hello tok framework toodefine a odesílání topologie Storm."
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
ms.openlocfilehash: 95701f51dfdf6f1a859dcde96d7053df4f21701f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a><span data-ttu-id="b9dd2-105">Storm v HDInsight pomocí Apache Kafka (preview)</span><span class="sxs-lookup"><span data-stu-id="b9dd2-105">Use Apache Kafka (preview) with Storm on HDInsight</span></span>

<span data-ttu-id="b9dd2-106">Zjistěte, jak toouse Apache Storm tooread z a zapisovat tooApache Kafka.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-106">Learn how toouse Apache Storm tooread from and write tooApache Kafka.</span></span> <span data-ttu-id="b9dd2-107">Tento příklad také ukazuje, jak toosave data z toohello topologie Storm HDFS kompatibilního souboru systému používaného HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-107">This example also demonstrates how toosave data from a Storm topology toohello HDFS-compatible file system used by HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="b9dd2-108">Hello kroky v tomto dokumentu vytvořte skupinu prostředků Azure, která obsahuje oba Storm v HDInsight a Kafka na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-108">hello steps in this document create an Azure resource group that contains both a Storm on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="b9dd2-109">Tyto clustery jsou obě nachází v rámci virtuální síť Azure, což umožňuje hello toodirectly clusteru Storm komunikovat s hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-109">These clusters are both located within an Azure Virtual Network, which allows hello Storm cluster toodirectly communicate with hello Kafka cluster.</span></span>
> 
> <span data-ttu-id="b9dd2-110">Až skončíte s hello kroky v tomto dokumentu, mějte na paměti toodelete hello clustery tooavoid nadbytečné poplatky.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-110">When you are done with hello steps in this document, remember toodelete hello clusters tooavoid excess charges.</span></span>

## <a name="get-hello-code"></a><span data-ttu-id="b9dd2-111">Získat kód hello</span><span class="sxs-lookup"><span data-stu-id="b9dd2-111">Get hello code</span></span>

<span data-ttu-id="b9dd2-112">Kód Hello hello příkladu v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span><span class="sxs-lookup"><span data-stu-id="b9dd2-112">hello code for hello example used in this document is available at [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span></span>

<span data-ttu-id="b9dd2-113">toocompile tento projekt, musíte hello následující konfigurace pro vývojové prostředí:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-113">toocompile this project, you need hello following configuration for your development environment:</span></span>

* <span data-ttu-id="b9dd2-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span> <span data-ttu-id="b9dd2-115">HDInsight 3.5 nebo vyšší vyžadují Java 8.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-115">HDInsight 3.5 or higher require Java 8.</span></span>

* [<span data-ttu-id="b9dd2-116">Maven 3.x</span><span class="sxs-lookup"><span data-stu-id="b9dd2-116">Maven 3.x</span></span>](https://maven.apache.org/download.cgi)

* <span data-ttu-id="b9dd2-117">Klientem SSH (třeba hello `ssh` a `scp` příkazy) – informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="b9dd2-117">An SSH client (you need hello `ssh` and `scp` commands) - For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="b9dd2-118">Textového editoru nebo IDE.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-118">A text editor or IDE.</span></span>

<span data-ttu-id="b9dd2-119">Hello následující proměnné prostředí může být nastaven při instalaci Java a hello JDK na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-119">hello following environment variables may be set when you install Java and hello JDK on your development workstation.</span></span> <span data-ttu-id="b9dd2-120">Ale byste měli zkontrolovat, že existují a že obsahují hello správné hodnoty pro váš systém.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-120">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="b9dd2-121">`JAVA_HOME`-by měla odkazovat toohello adresář, kam hello JDK je nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-121">`JAVA_HOME` - should point toohello directory where hello JDK is installed.</span></span>
* <span data-ttu-id="b9dd2-122">`PATH`-musí obsahovat hello následující cesty:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-122">`PATH` - should contain hello following paths:</span></span>
  
    * <span data-ttu-id="b9dd2-123">`JAVA_HOME`(nebo ekvivalentní cesta hello).</span><span class="sxs-lookup"><span data-stu-id="b9dd2-123">`JAVA_HOME` (or hello equivalent path).</span></span>
    * <span data-ttu-id="b9dd2-124">`JAVA_HOME\bin`(nebo ekvivalentní cesta hello).</span><span class="sxs-lookup"><span data-stu-id="b9dd2-124">`JAVA_HOME\bin` (or hello equivalent path).</span></span>
    * <span data-ttu-id="b9dd2-125">Hello adresář, kde je nainstalován Maven.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-125">hello directory where Maven is installed.</span></span>

## <a name="create-hello-clusters"></a><span data-ttu-id="b9dd2-126">Vytvoření clusterů se hello</span><span class="sxs-lookup"><span data-stu-id="b9dd2-126">Create hello clusters</span></span>

<span data-ttu-id="b9dd2-127">Apache Kafka v HDInsight neposkytuje přístup toohello Kafka zprostředkovatelé oproti hello veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-127">Apache Kafka on HDInsight does not provide access toohello Kafka brokers over hello public internet.</span></span> <span data-ttu-id="b9dd2-128">Všechno, co hello rozhovory tooKafka musí být ve stejné virtuální síti Azure jako uzly hello v hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-128">Anything that talks tooKafka must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="b9dd2-129">V tomto příkladu hello Kafka a clustery Storm jsou umístěny ve virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-129">For this example, both hello Kafka and Storm clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="b9dd2-130">Hello následující diagram znázorňuje tok komunikace mezi clustery hello:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-130">hello following diagram shows how communication flows between hello clusters:</span></span>

![Diagram clustery Storm a Kafka v virtuální sítě Azure](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="b9dd2-132">Další služby v clusteru hello, jako je SSH a Ambari je přístupná přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-132">Other services on hello cluster such as SSH and Ambari can be accessed over hello internet.</span></span> <span data-ttu-id="b9dd2-133">Další informace o veřejné porty hello k dispozici s HDInsight naleznete v tématu [porty a identifikátory URI používají v prostředí HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="b9dd2-133">For more information on hello public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="b9dd2-134">Když vytvoříte virtuální síť Azure, Kafka, a clusterů Storm se ručně, je snazší toouse šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-134">While you can create an Azure virtual network, Kafka, and Storm clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="b9dd2-135">Použití hello následující kroky toodeploy virtuální síť Azure, Kafka, a clusterů Storm tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-135">Use hello following steps toodeploy an Azure virtual network, Kafka, and Storm clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="b9dd2-136">Použijte hello tlačítko toosign v tooAzure a otevřete hello šablony v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-136">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    <span data-ttu-id="b9dd2-137">Hello šablony Azure Resource Manageru se nachází v **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-137">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span></span> <span data-ttu-id="b9dd2-138">Vytvoří hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-138">It creates hello following resources:</span></span>
    
    * <span data-ttu-id="b9dd2-139">Skupina prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="b9dd2-139">Azure resource group</span></span>
    * <span data-ttu-id="b9dd2-140">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="b9dd2-140">Azure Virtual Network</span></span>
    * <span data-ttu-id="b9dd2-141">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b9dd2-141">Azure Storage account</span></span>
    * <span data-ttu-id="b9dd2-142">Kafka v HDInsight verze 3.6 (tři uzly pracovního procesu)</span><span class="sxs-lookup"><span data-stu-id="b9dd2-142">Kafka on HDInsight version 3.6 (three worker nodes)</span></span>
    * <span data-ttu-id="b9dd2-143">Storm v HDInsight verze 3.6 (tři uzly pracovního procesu)</span><span class="sxs-lookup"><span data-stu-id="b9dd2-143">Storm on HDInsight version 3.6 (three worker nodes)</span></span>

  > [!WARNING]
  > <span data-ttu-id="b9dd2-144">dostupnost tooguarantee Kafka v HDInsight, cluster musí obsahovat aspoň tři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-144">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="b9dd2-145">Tato šablona vytvoří cluster Kafka, který obsahuje tři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-145">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="b9dd2-146">Hello použijte následující pokyny toopopulate hello položky na hello **vlastní nasazení** okno:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-146">Use hello following guidance toopopulate hello entries on hello **Custom deployment** blade:</span></span>
   
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * <span data-ttu-id="b9dd2-148">**Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-148">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="b9dd2-149">Tato skupina obsahuje hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-149">This group contains hello HDInsight cluster.</span></span>
   
    * <span data-ttu-id="b9dd2-150">**Umístění**: Vyberte tooyou geograficky zavřít umístění.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-150">**Location**: Select a location geographically close tooyou.</span></span>

    * <span data-ttu-id="b9dd2-151">**Základní název clusteru**: Tato hodnota se používá jako hello základní název pro clustery Storm a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-151">**Base Cluster Name**: This value is used as hello base name for hello Storm and Kafka clusters.</span></span> <span data-ttu-id="b9dd2-152">Například zadáním **hdi** vytvoří cluster Storm s názvem **storm hdi** a Kafka clusteru s názvem **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-152">For example, entering **hdi** creates a Storm cluster named **storm-hdi** and a Kafka cluster named **kafka-hdi**.</span></span>
   
    * <span data-ttu-id="b9dd2-153">**Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce hello u clusterů Storm a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-153">**Cluster Login User Name**: hello admin user name for hello Storm and Kafka clusters.</span></span>
   
    * <span data-ttu-id="b9dd2-154">**Heslo pro přihlášení clusteru**: heslo uživatele správce hello u clusterů Storm a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-154">**Cluster Login Password**: hello admin user password for hello Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="b9dd2-155">**Uživatelské jméno SSH**: hello toocreate uživatele SSH pro clustery Storm a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-155">**SSH User Name**: hello SSH user toocreate for hello Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="b9dd2-156">**Heslo SSH**: hello heslo pro uživatele SSH hello u clusterů Storm a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-156">**SSH Password**: hello password for hello SSH user for hello Storm and Kafka clusters.</span></span>

3. <span data-ttu-id="b9dd2-157">Čtení hello **podmínky a ujednání**a potom vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-157">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="b9dd2-158">Nakonec zkontrolujte **Pin toodashboard** a pak vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-158">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="b9dd2-159">Trvá přibližně 20 minut toocreate hello clustery.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-159">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="b9dd2-160">Po vytvoření hello prostředky, zobrazí se okno hello pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-160">Once hello resources have been created, hello blade for hello resource group is displayed.</span></span>

![Okno skupiny prostředků pro virtuální síť hello a clustery](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="b9dd2-162">Všimněte si, že jsou názvy hello clusterů HDInsight hello **storm BASENAME** a **kafka BASENAME**, kde BASENAME je hello jméno, které jste zadali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-162">Notice that hello names of hello HDInsight clusters are **storm-BASENAME** and **kafka-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="b9dd2-163">Použít tyto názvy v dalších krocích při připojení toohello clustery.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-163">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="understanding-hello-code"></a><span data-ttu-id="b9dd2-164">Pochopení kódu hello</span><span class="sxs-lookup"><span data-stu-id="b9dd2-164">Understanding hello code</span></span>

<span data-ttu-id="b9dd2-165">Tento projekt obsahuje dvě topologie:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-165">This project contains two topologies:</span></span>

* <span data-ttu-id="b9dd2-166">**KafkaWriter**: definované hello **writer.yaml** souboru, zapisuje Tato topologie náhodných věty tooKafka pomocí hello KafkaBolt s Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-166">**KafkaWriter**: Defined by hello **writer.yaml** file, this topology writes random sentences tooKafka using hello KafkaBolt provided with Apache Storm.</span></span>

    <span data-ttu-id="b9dd2-167">Tato topologie používá vlastní **SentenceSpout** součást toogenerate náhodných věty.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-167">This topology uses a custom **SentenceSpout** component toogenerate random sentences.</span></span>

* <span data-ttu-id="b9dd2-168">**KafkaReader**: definované hello **reader.yaml** souborů, tato topologie čte data z Kafka pomocí hello KafkaSpout s Apache Storm a protokoly hello data toostdout.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-168">**KafkaReader**: Defined by hello **reader.yaml** file, this topology reads data from Kafka using hello KafkaSpout provided with Apache Storm, then logs hello data toostdout.</span></span>

    <span data-ttu-id="b9dd2-169">Tato topologie používá hello Storm HdfsBolt toowrite data toodefault úložiště pro hello Storm cluster.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-169">This topology uses hello Storm HdfsBolt toowrite data toodefault storage for hello Storm cluster.</span></span>
### <a name="flux"></a><span data-ttu-id="b9dd2-170">Tok</span><span class="sxs-lookup"><span data-stu-id="b9dd2-170">Flux</span></span>

<span data-ttu-id="b9dd2-171">topologie Hello jsou definovány pomocí [tok](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="b9dd2-171">hello topologies are defined using [Flux](https://storm.apache.org/releases/1.1.0/flux.html).</span></span> <span data-ttu-id="b9dd2-172">Tok byla zavedena v Storm 0.10.x a umožňuje vám tooseparate hello topologie konfigurace z kódu hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-172">Flux was introduced in Storm 0.10.x and allows you tooseparate hello topology configuration from hello code.</span></span> <span data-ttu-id="b9dd2-173">Pro topologie, které používají hello tok framework hello topologie je definována v souboru YAML.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-173">For Topologies that use hello Flux framework, hello topology is defined in a YAML file.</span></span> <span data-ttu-id="b9dd2-174">Hello YAML soubor může být součástí hello topologie.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-174">hello YAML file can be included as part of hello topology.</span></span> <span data-ttu-id="b9dd2-175">Také může být samostatný soubor používá při odesílání topologie hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-175">It can also be a standalone file used when you submit hello topology.</span></span> <span data-ttu-id="b9dd2-176">Tok také podporuje nahrazení proměnné za běhu, který se používá v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-176">Flux also supports variable substitution at run-time, which is used in this example.</span></span>

<span data-ttu-id="b9dd2-177">Hello následující parametry jsou nastavené v době běhu pro tyto topologie:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-177">hello following parameters are set at run time for these topologies:</span></span>

* <span data-ttu-id="b9dd2-178">`${kafka.topic}`: název hello hello Kafka téma, které hello topologie pro čtení a zápis k.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-178">`${kafka.topic}`: hello name of hello Kafka topic that hello topologies read/write to.</span></span>

* <span data-ttu-id="b9dd2-179">`${kafka.broker.hosts}`: hello hostitelem tohoto hello Kafka zprostředkovává spustili.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-179">`${kafka.broker.hosts}`: hello hosts that hello Kafka brokers run on.</span></span> <span data-ttu-id="b9dd2-180">Hello zprostředkovatele informace využívají hello KafkaBolt při zápisu tooKafka.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-180">hello broker information is used by hello KafkaBolt when writing tooKafka.</span></span>

* <span data-ttu-id="b9dd2-181">`${kafka.zookeeper.hosts}`: hello hostitele, na kterých běží Zookeeper na v hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-181">`${kafka.zookeeper.hosts}`: hello hosts that Zookeeper runs on in hello Kafka cluster.</span></span>

<span data-ttu-id="b9dd2-182">Další informace o topologiích tok najdete v tématu [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="b9dd2-182">For more information on Flux topologies, see [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span></span>

## <a name="download-and-compile-hello-project"></a><span data-ttu-id="b9dd2-183">Stáhněte si a kompilaci hello projektu</span><span class="sxs-lookup"><span data-stu-id="b9dd2-183">Download and compile hello project</span></span>

1. <span data-ttu-id="b9dd2-184">Na vašem vývojovém prostředí, stáhněte si projekt hello z [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), otevřete příkazového řádku a změňte umístění toohello adresáře, který jste si stáhli hello projektu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-184">On your development environment, download hello project from [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), open a command-line, and change directories toohello location that you downloaded hello project.</span></span>

2. <span data-ttu-id="b9dd2-185">Z hello **hdinsight storm java kafka** adresář, použijte hello následující příkaz toocompile hello projektu a vytvořit balíček pro nasazení:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-185">From hello **hdinsight-storm-java-kafka** directory, use hello following command toocompile hello project and create a package for deployment:</span></span>

  ```bash
  mvn clean package
  ```

    <span data-ttu-id="b9dd2-186">proces balíčku Hello vytvoří soubor s názvem `KafkaTopology-1.0-SNAPSHOT.jar` v hello `target` adresáře.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-186">hello package process creates a file named `KafkaTopology-1.0-SNAPSHOT.jar` in hello `target` directory.</span></span>

3. <span data-ttu-id="b9dd2-187">Použijte následující příkazy toocopy hello balíček tooyour Storm v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-187">Use hello following commands toocopy hello package tooyour Storm on HDInsight cluster.</span></span> <span data-ttu-id="b9dd2-188">Nahraďte **uživatelské jméno** hello uživatelským jménem SSH pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-188">Replace **USERNAME** with hello SSH user name for hello cluster.</span></span> <span data-ttu-id="b9dd2-189">Nahraďte **BASENAME** s názvem základní hello jste použili při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-189">Replace **BASENAME** with hello base name you used when creating hello cluster.</span></span>

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    <span data-ttu-id="b9dd2-190">Po zobrazení výzvy zadejte heslo hello, který jste použili při vytváření clusterů hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-190">When prompted, enter hello password you used when creating hello clusters.</span></span>

## <a name="configure-hello-topology"></a><span data-ttu-id="b9dd2-191">Konfigurace hello topologie</span><span class="sxs-lookup"><span data-stu-id="b9dd2-191">Configure hello topology</span></span>

1. <span data-ttu-id="b9dd2-192">Použijte jednu z následujících metod toodiscover hello hello hostitelů Kafka zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-192">Use one of hello following methods toodiscover hello Kafka broker hosts:</span></span>

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
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
    > <span data-ttu-id="b9dd2-193">Hello Bash příklad předpokládá, že `$CLUSTERNAME` obsahuje název hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-193">hello Bash example assumes that `$CLUSTERNAME` contains hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="b9dd2-194">Dále předpokládá, že [jq](https://stedolan.github.io/jq/) je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-194">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="b9dd2-195">Po zobrazení výzvy zadejte hello heslo pro účet přihlášení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-195">When prompted, enter hello password for hello cluster login account.</span></span>

    <span data-ttu-id="b9dd2-196">Vrácená hodnota Hello je podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-196">hello value returned is similar toohello following text:</span></span>

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > <span data-ttu-id="b9dd2-197">Ačkoli může existovat více než dva hostitele zprostředkovatele pro váš cluster, není nutné tooprovide úplný seznam všech tooclients hostitele.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-197">While there may be more than two broker hosts for your cluster, you do not need tooprovide a full list of all hosts tooclients.</span></span> <span data-ttu-id="b9dd2-198">Jeden nebo dva kusy je dost.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-198">One or two is enough.</span></span>

2. <span data-ttu-id="b9dd2-199">Použijte jednu z hello následující metody toodiscover hello Kafka Zookeeper hostitelů:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-199">Use one of hello following methods toodiscover hello Kafka Zookeeper hosts:</span></span>

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
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
    > <span data-ttu-id="b9dd2-200">Hello Bash příklad předpokládá, že `$CLUSTERNAME` obsahuje název hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-200">hello Bash example assumes that `$CLUSTERNAME` contains hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="b9dd2-201">Dále předpokládá, že [jq](https://stedolan.github.io/jq/) je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-201">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="b9dd2-202">Po zobrazení výzvy zadejte hello heslo pro účet přihlášení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-202">When prompted, enter hello password for hello cluster login account.</span></span>

    <span data-ttu-id="b9dd2-203">Vrácená hodnota Hello je podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-203">hello value returned is similar toohello following text:</span></span>

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > <span data-ttu-id="b9dd2-204">Existuje více než dva uzly Zookeeper, není nutné tooprovide úplný seznam všech tooclients hostitele.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-204">While there are more than two Zookeeper nodes, you do not need tooprovide a full list of all hosts tooclients.</span></span> <span data-ttu-id="b9dd2-205">Jeden nebo dva kusy je dost.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-205">One or two is enough.</span></span>

    <span data-ttu-id="b9dd2-206">Tato hodnota, uložte, protože se později používá.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-206">Save this value, as it is used later.</span></span>

3. <span data-ttu-id="b9dd2-207">Upravit hello `dev.properties` soubor v kořenovém hello hello projektu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-207">Edit hello `dev.properties` file in hello root of hello project.</span></span> <span data-ttu-id="b9dd2-208">Přidejte hello zprostředkovatele a odpovídající řádky toohello, Zookeeper hostitele informace v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-208">Add hello Broker and Zookeeper hosts information toohello matching lines in this file.</span></span> <span data-ttu-id="b9dd2-209">Hello následujícím příkladu je nakonfigurované použití hello ukázkové hodnoty z předchozích kroků hello:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-209">hello following example is configured using hello sample values from hello previous steps:</span></span>

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. <span data-ttu-id="b9dd2-210">Uložit hello `dev.properties` souboru a pak použít hello následující příkaz tooupload ho toohello Storm cluster:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-210">Save hello `dev.properties` file and then use hello following command tooupload it toohello Storm cluster:</span></span>

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="b9dd2-211">Nahraďte **uživatelské jméno** hello uživatelským jménem SSH pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-211">Replace **USERNAME** with hello SSH user name for hello cluster.</span></span> <span data-ttu-id="b9dd2-212">Nahraďte **BASENAME** s názvem základní hello jste použili při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-212">Replace **BASENAME** with hello base name you used when creating hello cluster.</span></span>

## <a name="start-hello-writer"></a><span data-ttu-id="b9dd2-213">Spustit zapisovače hello</span><span class="sxs-lookup"><span data-stu-id="b9dd2-213">Start hello writer</span></span>

1. <span data-ttu-id="b9dd2-214">Použijte hello následující tooconnect toohello Storm clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-214">Use hello following tooconnect toohello Storm cluster using SSH.</span></span> <span data-ttu-id="b9dd2-215">Nahraďte **uživatelské jméno** s uživatelským jménem SSH hello používá při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-215">Replace **USERNAME** with hello SSH user name used when creating hello cluster.</span></span> <span data-ttu-id="b9dd2-216">Nahraďte **BASENAME** základní názvem hello používá při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-216">Replace **BASENAME** with hello base name used when creating hello cluster.</span></span>

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    <span data-ttu-id="b9dd2-217">Po zobrazení výzvy zadejte heslo hello, který jste použili při vytváření clusterů hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-217">When prompted, enter hello password you used when creating hello clusters.</span></span>
   
    <span data-ttu-id="b9dd2-218">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="b9dd2-218">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="b9dd2-219">Z hello připojení SSH použijte následující příkaz toocreate hello Kafka tématu používané hello topologie hello:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-219">From hello SSH connection, use hello following command toocreate hello Kafka topic used by hello topology:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    <span data-ttu-id="b9dd2-220">Nahraďte `$KAFKAZKHOSTS` s hello Zookeeper hostitele informace, které jste získali v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-220">Replace `$KAFKAZKHOSTS` with hello Zookeeper host information you retrieved in hello previous section.</span></span>

2. <span data-ttu-id="b9dd2-221">Z hello SSH připojení toohello Storm clusteru použijte následující příkaz toostart hello zapisovače topologie hello:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-221">From hello SSH connection toohello Storm cluster, use hello following command toostart hello writer topology:</span></span>

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    <span data-ttu-id="b9dd2-222">Hello parametry tohoto příkazu jsou:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-222">hello parameters used with this command are:</span></span>

    * <span data-ttu-id="b9dd2-223">`org.apache.storm.flux.Flux`: Použije tok tooconfigure a spustí se tato topologie.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-223">`org.apache.storm.flux.Flux`: Use Flux tooconfigure and run this topology.</span></span>

    * <span data-ttu-id="b9dd2-224">`--remote`: Odešlete tooNimbus topologie hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-224">`--remote`: Submit hello topology tooNimbus.</span></span> <span data-ttu-id="b9dd2-225">Hello topologie se distribuuje do hello uzlů pracovního procesu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-225">hello topology is distributed across hello worker nodes in hello cluster.</span></span>

    * <span data-ttu-id="b9dd2-226">`-R /writer.yaml`: Použití hello `writer.yaml` souboru tooconfigure hello topologie.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-226">`-R /writer.yaml`: Use hello `writer.yaml` file tooconfigure hello topology.</span></span> <span data-ttu-id="b9dd2-227">`-R`Označuje, že tento prostředek je součástí soubor jar hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-227">`-R` indicates that this resource is included in hello jar file.</span></span> <span data-ttu-id="b9dd2-228">Je v kořenovém adresáři hello hello jar, takže `/writer.yaml` je cesta tooit hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-228">It's in hello root of hello jar, so `/writer.yaml` is hello path tooit.</span></span>

    * <span data-ttu-id="b9dd2-229">`--filter`: Naplnění položek v hello `writer.yaml` topologie pomocí hodnot v hello `dev.properties` souboru.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-229">`--filter`: Populate entries in hello `writer.yaml` topology using values in hello `dev.properties` file.</span></span> <span data-ttu-id="b9dd2-230">Například hello hodnotu hello `kafka.topic` záznam v souboru hello je použité tooreplace hello `${kafka.topic}` položku v definici topologie hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-230">For example, hello value of hello `kafka.topic` entry in hello file is used tooreplace hello `${kafka.topic}` entry in hello topology definition.</span></span>

5. <span data-ttu-id="b9dd2-231">Po zahájení hello topologie, použijte následující příkaz tooverify, zapisuje data toohello Kafka tématu hello:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-231">Once hello topology has started, use hello following command tooverify that it is writing data toohello Kafka topic:</span></span>

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    <span data-ttu-id="b9dd2-232">Nahraďte `$KAFKAZKHOSTS` s hello Zookeeper hostitele informace, které jste získali v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-232">Replace `$KAFKAZKHOSTS` with hello Zookeeper host information you retrieved in hello previous section.</span></span>

    <span data-ttu-id="b9dd2-233">Tento příkaz používá skript dodaný s Kafka toomonitor hello tématu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-233">This command uses a script shipped with Kafka toomonitor hello topic.</span></span> <span data-ttu-id="b9dd2-234">Po chvíli by se měl spustit vrací náhodné věty, který byl toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-234">After a moment, it should start returning random sentences that have been written toohello topic.</span></span> <span data-ttu-id="b9dd2-235">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-235">hello output is similar toohello following example:</span></span>

        i am at two with nature             
        an apple a day keeps hello doctor away
        snow white and hello seven dwarfs     
        hello cow jumped over hello moon        
        an apple a day keeps hello doctor away
        an apple a day keeps hello doctor away
        hello cow jumped over hello moon        
        an apple a day keeps hello doctor away
        an apple a day keeps hello doctor away
        four score and seven years ago      
        snow white and hello seven dwarfs     
        snow white and hello seven dwarfs     
        i am at two with nature             
        an apple a day keeps hello doctor away

    <span data-ttu-id="b9dd2-236">Pomocí kombinace kláves Ctrl + c toostop hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-236">Use Ctrl+c toostop hello script.</span></span>

## <a name="start-hello-reader"></a><span data-ttu-id="b9dd2-237">Spustit čtečky hello</span><span class="sxs-lookup"><span data-stu-id="b9dd2-237">Start hello reader</span></span>

1. <span data-ttu-id="b9dd2-238">Z hello SSH relace toohello Storm clusteru použijte následující příkaz toostart hello čtečky topologie hello:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-238">From hello SSH session toohello Storm cluster, use hello following command toostart hello reader topology:</span></span>

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. <span data-ttu-id="b9dd2-239">Jakmile se spustí hello topologie, otevřete hello uživatelské rozhraní Storm.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-239">Once hello topology starts, open hello Storm UI.</span></span> <span data-ttu-id="b9dd2-240">Tato webového uživatelského rozhraní se nachází v https://storm-BASENAME.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-240">This web UI is located at https://storm-BASENAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="b9dd2-241">Nahraďte __BASENAME__ základní názvem hello použita při vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-241">Replace __BASENAME__ with hello base name used when hello cluster was created.</span></span> 

    <span data-ttu-id="b9dd2-242">Pokud budete vyzváni, použijte přihlašovací jméno správce hello (výchozím `admin`) a heslo používané při vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-242">When prompted, use hello admin login name (default, `admin`) and password used when hello cluster was created.</span></span> <span data-ttu-id="b9dd2-243">Zobrazí webové stránky podobné toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-243">You see a web page similar toohello following image:</span></span>

    ![Storm uživatelského rozhraní](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. <span data-ttu-id="b9dd2-245">Hello uživatelské rozhraní Storm, vyberte hello __kafka čtečky__ odkaz v hello __souhrn topologie__ části toodisplay informace o hello __kafka čtečky__ topologie.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-245">From hello Storm UI, select hello __kafka-reader__ link in hello __Topology Summary__ section toodisplay information about hello __kafka-reader__ topology.</span></span>

    ![Topologie části Souhrn hello Storm webového uživatelského rozhraní](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. <span data-ttu-id="b9dd2-247">toodisplay informace o instancích hello hello protokolovacího nástroje bolt součásti, vyberte hello __protokolovač bolt__ odkaz v hello __funkce Bolts (všechny čas)__ části.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-247">toodisplay information about hello instances of hello logger-bolt component, select hello __logger-bolt__ link in hello __Bolts (All time)__ section.</span></span>

    ![Protokolovač bolt odkaz v části funkce bolts hello](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. <span data-ttu-id="b9dd2-249">V hello __vykonavatelů__ vyberte odkaz v hello __Port__ sloupec toodisplay protokolování informací o tuto instanci součásti hello.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-249">In hello __Executors__ section, select a link in hello __Port__ column toodisplay logging information about this instance of hello component.</span></span>

    ![Vykonavatelů odkaz](./media/hdinsight-apache-storm-with-kafka/executors.png)

    <span data-ttu-id="b9dd2-251">Hello protokolu obsahuje protokolu hello data načtená z hello Kafka tématu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-251">hello log contains a log of hello data read from hello Kafka topic.</span></span> <span data-ttu-id="b9dd2-252">Hello informace v protokolu hello jsou podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-252">hello information in hello log is similar toohello following text:</span></span>

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] hello cow jumped over hello moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: hello cow jumped over hello moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps hello doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps hello doctor away

## <a name="stop-hello-topologies"></a><span data-ttu-id="b9dd2-253">Zastavení topologie hello</span><span class="sxs-lookup"><span data-stu-id="b9dd2-253">Stop hello topologies</span></span>

<span data-ttu-id="b9dd2-254">Z clusteru Storm SSH relace toohello použijte následující topologie Storm hello toostop příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="b9dd2-254">From an SSH session toohello Storm cluster, use hello following commands toostop hello Storm topologies:</span></span>

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-hello-cluster"></a><span data-ttu-id="b9dd2-255">Odstranění clusteru hello</span><span class="sxs-lookup"><span data-stu-id="b9dd2-255">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="b9dd2-256">Vzhledem k tomu, že hello kroky v tomto dokumentu vytvořte oba clustery v hello stejnou skupinu prostředků Azure, můžete odstranit skupinu prostředků hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-256">Since hello steps in this document create both clusters in hello same Azure resource group, you can delete hello resource group in hello Azure portal.</span></span> <span data-ttu-id="b9dd2-257">Odstraněním skupiny prostředků hello odebere všechny prostředky, které jsou vytvořené pomocí následujících tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b9dd2-257">Deleting hello resource group removes all resources created by following this document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9dd2-258">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9dd2-258">Next steps</span></span>

<span data-ttu-id="b9dd2-259">Další příklad topologie, které lze použít s Storm v HDInsight, naleznete v části [topologie Storm příklad a součásti](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="b9dd2-259">For more example topologies that can be used with Storm on HDInsight, see [Example Storm topologies and components](hdinsight-storm-example-topology.md).</span></span>

<span data-ttu-id="b9dd2-260">Informace o nasazení a monitorování topologií v HDInsight se systémem Linux najdete v tématu [nasazení a správa topologií Apache Storm v HDInsight se systémem Linux](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="b9dd2-260">For information on deploying and monitoring topologies on Linux-based HDInsight, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>