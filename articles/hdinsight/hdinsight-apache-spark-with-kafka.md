---
title: "aaaApache Spark streamování s Kafka - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse Spark Apache Spark toostream data do nebo z Apache Kafka pomocí DStreams. V tomto příkladu stream dat pomocí poznámkového bloku Jupyter z Spark v HDInsight."
keywords: "Příklad kafka, kafka zookeeper, spark, streamování kafka, například kafka vysílání datového proudu spark"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd8f53c1-bdee-4921-b683-3be4c46c2039
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/13/2017
ms.author: larryfr
ms.openlocfilehash: f48b37aadafa4979cd27af68e8417db6acc8a0e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="c151d-105">Apache Spark streamování (DStream) příklad s Kafka (preview) v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c151d-105">Apache Spark streaming (DStream) example with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="c151d-106">Zjistěte, jak toouse Spark Apache Spark toostream data do nebo z Apache Kafka v HDInsight pomocí DStreams.</span><span class="sxs-lookup"><span data-stu-id="c151d-106">Learn how toouse Spark Apache Spark toostream data into or out of Apache Kafka on HDInsight using DStreams.</span></span> <span data-ttu-id="c151d-107">Tento příklad používá poznámkového bloku Jupyter, která běží na clusteru Spark hello.</span><span class="sxs-lookup"><span data-stu-id="c151d-107">This example uses a Jupyter notebook that runs on hello Spark cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="c151d-108">Hello kroky v tomto dokumentu vytvořte skupinu prostředků Azure, která obsahuje oba Spark v HDInsight a Kafka na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c151d-108">hello steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="c151d-109">Tyto clustery jsou obě nachází v rámci virtuální síť Azure, což umožňuje hello toodirectly clusteru Spark komunikovat s hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="c151d-109">These clusters are both located within an Azure Virtual Network, which allows hello Spark cluster toodirectly communicate with hello Kafka cluster.</span></span>
>
> <span data-ttu-id="c151d-110">Až skončíte s hello kroky v tomto dokumentu, mějte na paměti toodelete hello clustery tooavoid nadbytečné poplatky.</span><span class="sxs-lookup"><span data-stu-id="c151d-110">When you are done with hello steps in this document, remember toodelete hello clusters tooavoid excess charges.</span></span>

## <a name="create-hello-clusters"></a><span data-ttu-id="c151d-111">Vytvoření clusterů se hello</span><span class="sxs-lookup"><span data-stu-id="c151d-111">Create hello clusters</span></span>

<span data-ttu-id="c151d-112">Apache Kafka v HDInsight neposkytuje přístup toohello Kafka zprostředkovatelé oproti hello veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="c151d-112">Apache Kafka on HDInsight does not provide access toohello Kafka brokers over hello public internet.</span></span> <span data-ttu-id="c151d-113">Všechno, co hello rozhovory tooKafka musí být ve stejné virtuální síti Azure jako uzly hello v hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="c151d-113">Anything that talks tooKafka must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="c151d-114">V tomto příkladu jsou umístěny hello Kafka a clustery Spark v Azure virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c151d-114">For this example, both hello Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="c151d-115">Hello následující diagram znázorňuje tok komunikace mezi clustery hello:</span><span class="sxs-lookup"><span data-stu-id="c151d-115">hello following diagram shows how communication flows between hello clusters:</span></span>

![Diagram clustery Spark a Kafka v virtuální sítě Azure](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="c151d-117">Když je omezená toocommunication v rámci virtuální sítě hello Kafka, samotné, dalších služeb v hello clusteru například SSH a Ambari je přístupná přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="c151d-117">Though Kafka itself is limited toocommunication within hello virtual network, other services on hello cluster such as SSH and Ambari can be accessed over hello internet.</span></span> <span data-ttu-id="c151d-118">Další informace o veřejné porty hello k dispozici s HDInsight naleznete v tématu [porty a identifikátory URI používají v prostředí HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="c151d-118">For more information on hello public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="c151d-119">Můžete vytvořit virtuální síť Azure, Kafka a Spark clustery ručně, je snazší toouse šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c151d-119">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="c151d-120">Použití hello následující kroky toodeploy virtuální síť Azure, Kafka, a clustery Spark tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c151d-120">Use hello following steps toodeploy an Azure virtual network, Kafka, and Spark clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="c151d-121">Použijte hello tlačítko toosign v tooAzure a otevřete hello šablony v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c151d-121">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    <span data-ttu-id="c151d-122">Hello šablony Azure Resource Manageru se nachází v **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="c151d-122">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="c151d-123">dostupnost tooguarantee Kafka v HDInsight, cluster musí obsahovat aspoň tři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="c151d-123">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="c151d-124">Tato šablona vytvoří cluster Kafka, který obsahuje tři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="c151d-124">This template creates a Kafka cluster that contains three worker nodes.</span></span>

    <span data-ttu-id="c151d-125">Tato šablona vytvoří cluster služby HDInsight 3.6 Kafka a Spark.</span><span class="sxs-lookup"><span data-stu-id="c151d-125">This template creates an HDInsight 3.6 cluster for both Kafka and Spark.</span></span>

2. <span data-ttu-id="c151d-126">Hello použijte následující informace toopopulate hello položky na hello **vlastní nasazení** okno:</span><span class="sxs-lookup"><span data-stu-id="c151d-126">Use hello following information toopopulate hello entries on hello **Custom deployment** blade:</span></span>
   
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="c151d-128">**Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="c151d-128">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="c151d-129">Tato skupina obsahuje hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c151d-129">This group contains hello HDInsight cluster.</span></span>

    * <span data-ttu-id="c151d-130">**Umístění**: Vyberte tooyou geograficky zavřít umístění.</span><span class="sxs-lookup"><span data-stu-id="c151d-130">**Location**: Select a location geographically close tooyou.</span></span>

    * <span data-ttu-id="c151d-131">**Základní název clusteru**: Tato hodnota se používá jako hello základní název pro clustery Spark a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="c151d-131">**Base Cluster Name**: This value is used as hello base name for hello Spark and Kafka clusters.</span></span> <span data-ttu-id="c151d-132">Například zadáním **hdi** vytvoří Spark clusteru s názvem spark hdi__ a Kafka clusteru s názvem **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="c151d-132">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="c151d-133">**Uživatelské jméno přihlášení clusteru**: hello uživatelské jméno správce pro clustery Spark a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="c151d-133">**Cluster Login User Name**: hello admin user name for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="c151d-134">**Heslo pro přihlášení clusteru**: heslo uživatele hello správce pro clustery Spark a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="c151d-134">**Cluster Login Password**: hello admin user password for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="c151d-135">**Uživatelské jméno SSH**: hello toocreate uživatele SSH pro clustery Spark a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="c151d-135">**SSH User Name**: hello SSH user toocreate for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="c151d-136">**Heslo SSH**: hello heslo pro uživatele hello SSH pro clustery Spark a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="c151d-136">**SSH Password**: hello password for hello SSH user for hello Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="c151d-137">Čtení hello **podmínky a ujednání**a potom vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**.</span><span class="sxs-lookup"><span data-stu-id="c151d-137">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="c151d-138">Nakonec zkontrolujte **Pin toodashboard** a pak vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="c151d-138">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="c151d-139">Trvá přibližně 20 minut toocreate hello clustery.</span><span class="sxs-lookup"><span data-stu-id="c151d-139">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="c151d-140">Po vytvoření hello prostředky jste přesměrovaného tooa okně hello skupinu prostředků, která obsahuje hello clustery a webové řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="c151d-140">Once hello resources have been created, you are redirected tooa blade for hello resource group that contains hello clusters and web dashboard.</span></span>

![Okno skupiny prostředků pro virtuální síť hello a clustery](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="c151d-142">Všimněte si, že jsou názvy hello clusterů HDInsight hello **spark BASENAME** a **kafka BASENAME**, kde BASENAME je hello jméno, které jste zadali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="c151d-142">Notice that hello names of hello HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="c151d-143">Použít tyto názvy v dalších krocích při připojení toohello clustery.</span><span class="sxs-lookup"><span data-stu-id="c151d-143">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="use-hello-notebooks"></a><span data-ttu-id="c151d-144">Použití poznámkových bloků hello</span><span class="sxs-lookup"><span data-stu-id="c151d-144">Use hello notebooks</span></span>

<span data-ttu-id="c151d-145">Kód Hello například hello popsané v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span><span class="sxs-lookup"><span data-stu-id="c151d-145">hello code for hello example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span></span>

<span data-ttu-id="c151d-146">Postupujte podle kroků hello v hello `README.md` souboru toocomplete v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="c151d-146">Follow hello steps in hello `README.md` file toocomplete this example.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="c151d-147">Odstranění clusteru hello</span><span class="sxs-lookup"><span data-stu-id="c151d-147">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="c151d-148">Vzhledem k tomu, že hello kroky v tomto dokumentu vytvořte oba clustery v hello stejnou skupinu prostředků Azure, můžete odstranit skupinu prostředků hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c151d-148">Since hello steps in this document create both clusters in hello same Azure resource group, you can delete hello resource group in hello Azure portal.</span></span> <span data-ttu-id="c151d-149">Odstraněním skupiny hello odebere všechny prostředky, které jsou vytvořené pomocí následujících tento dokument, hello virtuální sítě Azure a účet úložiště, které jsou používané clustery hello.</span><span class="sxs-lookup"><span data-stu-id="c151d-149">Deleting hello group removes all resources created by following this document, hello Azure Virtual Network, and storage account used by hello clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c151d-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c151d-150">Next steps</span></span>

<span data-ttu-id="c151d-151">V tomto příkladu jste zjistili, jak toouse Spark tooread a zápis tooKafka.</span><span class="sxs-lookup"><span data-stu-id="c151d-151">In this example, you learned how toouse Spark tooread and write tooKafka.</span></span> <span data-ttu-id="c151d-152">Použijte následující odkazy toodiscover hello jiné způsoby toowork s Kafka:</span><span class="sxs-lookup"><span data-stu-id="c151d-152">Use hello following links toodiscover other ways toowork with Kafka:</span></span>

* [<span data-ttu-id="c151d-153">Začínáme s Apache Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c151d-153">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="c151d-154">Použít MirrorMaker toocreate repliku Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c151d-154">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="c151d-155">Použití Apache Stormu se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="c151d-155">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

