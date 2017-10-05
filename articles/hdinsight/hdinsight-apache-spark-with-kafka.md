---
title: "Apache Spark streamování s Kafka - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Spark Apache Spark na datový proud dat do nebo z Apache Kafka pomocí DStreams. V tomto příkladu stream dat pomocí poznámkového bloku Jupyter z Spark v HDInsight."
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
ms.openlocfilehash: 81fa319f6fb94bdabacd8f68d14b9a1063a9749a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="90b6c-105">Apache Spark streamování (DStream) příklad s Kafka (preview) v HDInsight</span><span class="sxs-lookup"><span data-stu-id="90b6c-105">Apache Spark streaming (DStream) example with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="90b6c-106">Další informace o použití Spark Apache Spark na datový proud dat do nebo z Apache Kafka v HDInsight pomocí DStreams.</span><span class="sxs-lookup"><span data-stu-id="90b6c-106">Learn how to use Spark Apache Spark to stream data into or out of Apache Kafka on HDInsight using DStreams.</span></span> <span data-ttu-id="90b6c-107">Tento příklad používá poznámkového bloku Jupyter, která běží na clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="90b6c-107">This example uses a Jupyter notebook that runs on the Spark cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="90b6c-108">Kroky v tomto dokumentu vytvořte skupinu prostředků Azure, která obsahuje oba Spark v HDInsight a Kafka na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="90b6c-108">The steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="90b6c-109">Tyto clustery jsou obě nachází v rámci virtuální síť Azure, což umožňuje clusteru Spark přímo komunikovat s Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="90b6c-109">These clusters are both located within an Azure Virtual Network, which allows the Spark cluster to directly communicate with the Kafka cluster.</span></span>
>
> <span data-ttu-id="90b6c-110">Po dokončení kroků v tomto dokumentu, nezapomeňte odstranit clustery nadbytečné náklady.</span><span class="sxs-lookup"><span data-stu-id="90b6c-110">When you are done with the steps in this document, remember to delete the clusters to avoid excess charges.</span></span>

## <a name="create-the-clusters"></a><span data-ttu-id="90b6c-111">Vytváření clusterů</span><span class="sxs-lookup"><span data-stu-id="90b6c-111">Create the clusters</span></span>

<span data-ttu-id="90b6c-112">Apache Kafka v HDInsight neposkytuje přístup k zprostředkovatelé Kafka prostřednictvím veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="90b6c-112">Apache Kafka on HDInsight does not provide access to the Kafka brokers over the public internet.</span></span> <span data-ttu-id="90b6c-113">Všechno, co komunikuje se Kafka musí být ve stejné virtuální síti Azure jako uzly v clusteru Kafka.</span><span class="sxs-lookup"><span data-stu-id="90b6c-113">Anything that talks to Kafka must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="90b6c-114">V tomto příkladu jsou Kafka i Spark clusterů umístěné v virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="90b6c-114">For this example, both the Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="90b6c-115">Následující diagram znázorňuje tok komunikace mezi clustery:</span><span class="sxs-lookup"><span data-stu-id="90b6c-115">The following diagram shows how communication flows between the clusters:</span></span>

![Diagram clustery Spark a Kafka v virtuální sítě Azure](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="90b6c-117">I když Kafka, samotné je omezený na komunikaci v rámci virtuální sítě, dalších služeb v clusteru, například SSH a Ambari jsou přístupné přes internet.</span><span class="sxs-lookup"><span data-stu-id="90b6c-117">Though Kafka itself is limited to communication within the virtual network, other services on the cluster such as SSH and Ambari can be accessed over the internet.</span></span> <span data-ttu-id="90b6c-118">Další informace o veřejné porty, které jsou k dispozici s HDInsight naleznete v tématu [porty a identifikátory URI používají v prostředí HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="90b6c-118">For more information on the public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="90b6c-119">Když vytvoříte virtuální síť Azure, Kafka, a clustery Spark ručně, je jednodušší použít šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="90b6c-119">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="90b6c-120">Použijte následující kroky k nasazení virtuální sítě Azure, Kafka a clustery k předplatnému Azure z Spark.</span><span class="sxs-lookup"><span data-stu-id="90b6c-120">Use the following steps to deploy an Azure virtual network, Kafka, and Spark clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="90b6c-121">Na následující tlačítko použijte pro přihlášení do Azure a otevřete šablonu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="90b6c-121">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    <span data-ttu-id="90b6c-122">Šablona Azure Resource Manager je umístěna ve **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="90b6c-122">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="90b6c-123">Pokud chcete zajistit dostupnost Kafka v HDInsightu, musí cluster obsahovat aspoň tři pracovní uzly.</span><span class="sxs-lookup"><span data-stu-id="90b6c-123">To guarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="90b6c-124">Tato šablona vytvoří cluster Kafka, který obsahuje tři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="90b6c-124">This template creates a Kafka cluster that contains three worker nodes.</span></span>

    <span data-ttu-id="90b6c-125">Tato šablona vytvoří cluster služby HDInsight 3.6 Kafka a Spark.</span><span class="sxs-lookup"><span data-stu-id="90b6c-125">This template creates an HDInsight 3.6 cluster for both Kafka and Spark.</span></span>

2. <span data-ttu-id="90b6c-126">Následující informace slouží k naplnění položek na **vlastní nasazení** okno:</span><span class="sxs-lookup"><span data-stu-id="90b6c-126">Use the following information to populate the entries on the **Custom deployment** blade:</span></span>
   
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="90b6c-128">**Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="90b6c-128">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="90b6c-129">Tato skupina obsahuje clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="90b6c-129">This group contains the HDInsight cluster.</span></span>

    * <span data-ttu-id="90b6c-130">**Umístění**: Vyberte umístění geograficky blízko vás.</span><span class="sxs-lookup"><span data-stu-id="90b6c-130">**Location**: Select a location geographically close to you.</span></span>

    * <span data-ttu-id="90b6c-131">**Základní název clusteru**: Tato hodnota se používá jako základní název pro Spark a Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="90b6c-131">**Base Cluster Name**: This value is used as the base name for the Spark and Kafka clusters.</span></span> <span data-ttu-id="90b6c-132">Například zadáním **hdi** vytvoří Spark clusteru s názvem spark hdi__ a Kafka clusteru s názvem **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="90b6c-132">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="90b6c-133">**Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce pro clustery Spark a Kafka.</span><span class="sxs-lookup"><span data-stu-id="90b6c-133">**Cluster Login User Name**: The admin user name for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="90b6c-134">**Heslo pro přihlášení clusteru**: uživatelské heslo správce pro clustery Spark a Kafka.</span><span class="sxs-lookup"><span data-stu-id="90b6c-134">**Cluster Login Password**: The admin user password for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="90b6c-135">**Uživatelské jméno SSH**: SSH, aby uživatel vytvořil pro clustery Spark a Kafka.</span><span class="sxs-lookup"><span data-stu-id="90b6c-135">**SSH User Name**: The SSH user to create for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="90b6c-136">**Heslo SSH**: heslo pro uživatele SSH pro clustery Spark a Kafka.</span><span class="sxs-lookup"><span data-stu-id="90b6c-136">**SSH Password**: The password for the SSH user for the Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="90b6c-137">Pro čtení **podmínky a ujednání**a potom vyberte **souhlasím s podmínkami a ujednáními výše uvedených**.</span><span class="sxs-lookup"><span data-stu-id="90b6c-137">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="90b6c-138">Nakonec zkontrolujte **připnout na řídicí panel** a pak vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="90b6c-138">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="90b6c-139">Chcete-li vytvořit clustery trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="90b6c-139">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="90b6c-140">Po vytvoření prostředky budete přesměrováni do okna pro skupinu prostředků, která obsahuje clustery a webový řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="90b6c-140">Once the resources have been created, you are redirected to a blade for the resource group that contains the clusters and web dashboard.</span></span>

![Okno skupiny prostředků pro virtuální síť a clustery](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="90b6c-142">Všimněte si, že jsou názvy clusterů HDInsight **spark BASENAME** a **kafka BASENAME**, kde BASENAME je jméno, které jste zadali v šabloně.</span><span class="sxs-lookup"><span data-stu-id="90b6c-142">Notice that the names of the HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="90b6c-143">Názvy těchto používat v dalších krocích při připojování k clustery.</span><span class="sxs-lookup"><span data-stu-id="90b6c-143">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="use-the-notebooks"></a><span data-ttu-id="90b6c-144">Použití poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="90b6c-144">Use the notebooks</span></span>

<span data-ttu-id="90b6c-145">Kód pro tento příklad popsané v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span><span class="sxs-lookup"><span data-stu-id="90b6c-145">The code for the example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span></span>

<span data-ttu-id="90b6c-146">Postupujte podle kroků v `README.md` soubor dokončete tento příklad.</span><span class="sxs-lookup"><span data-stu-id="90b6c-146">Follow the steps in the `README.md` file to complete this example.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="90b6c-147">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="90b6c-147">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="90b6c-148">Vzhledem k tomu, že kroky v tomto dokumentu vytvořit oba clustery ve stejné skupině prostředků Azure, můžete odstranit skupinu prostředků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="90b6c-148">Since the steps in this document create both clusters in the same Azure resource group, you can delete the resource group in the Azure portal.</span></span> <span data-ttu-id="90b6c-149">Odstraňuje se skupina odebere všechny prostředky, které jsou vytvořené pomocí následujících tento dokument, Azure Virtual Network a účet úložiště, které jsou používané clustery.</span><span class="sxs-lookup"><span data-stu-id="90b6c-149">Deleting the group removes all resources created by following this document, the Azure Virtual Network, and storage account used by the clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90b6c-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90b6c-150">Next steps</span></span>

<span data-ttu-id="90b6c-151">V tomto příkladu jste zjistili, jak používat Spark ke čtení a zápisu do Kafka.</span><span class="sxs-lookup"><span data-stu-id="90b6c-151">In this example, you learned how to use Spark to read and write to Kafka.</span></span> <span data-ttu-id="90b6c-152">Chcete-li zjistit další způsoby, jak pracovat s Kafka pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="90b6c-152">Use the following links to discover other ways to work with Kafka:</span></span>

* [<span data-ttu-id="90b6c-153">Začínáme s Apache Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="90b6c-153">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="90b6c-154">Vytvoření repliky Kafka ve službě HDInsight pomocí MirrorMakeru</span><span class="sxs-lookup"><span data-stu-id="90b6c-154">Use MirrorMaker to create a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="90b6c-155">Použití Apache Stormu se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="90b6c-155">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

