---
title: "Apache Spark strukturovaných streamování s Kafka - Azure HDInsight | Microsoft Docs"
description: "Naučte se používat Apache Spark streamování (DStream) k získání dat do nebo z Apache Kafka. V tomto příkladu stream dat pomocí poznámkového bloku Jupyter z Spark v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/09/2017
ms.author: larryfr
ms.openlocfilehash: 02b49e13e8f54c3d55310f4d2b21c7e09c91fe81
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="894e9-104">Použít Spark strukturovaných streamování s Kafka (preview) v HDInsight</span><span class="sxs-lookup"><span data-stu-id="894e9-104">Use Spark Structured Streaming with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="894e9-105">Naučte se používat Spark strukturovaných streamování číst data z Apache Kafka v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="894e9-105">Learn how to use Spark Structured Streaming to read data from Apache Kafka on Azure HDInsight.</span></span>

<span data-ttu-id="894e9-106">Vysílání datového proudu strukturovaná Spark je modul zpracování datového proudu, který je založený na Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="894e9-106">Spark structured streaming is a stream processing engine built on Spark SQL.</span></span> <span data-ttu-id="894e9-107">Umožňuje express streamování výpočty stejná jako výpočetní batch na statických dat.</span><span class="sxs-lookup"><span data-stu-id="894e9-107">It allows you to express streaming computations the same as batch computation on static data.</span></span> <span data-ttu-id="894e9-108">Další informace o strukturovaných streamování najdete v tématu [strukturovaných streamování Průvodce programováním [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) na Apache.org.</span><span class="sxs-lookup"><span data-stu-id="894e9-108">For more information on Structured Streaming, see the [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) at Apache.org.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="894e9-109">Tento příklad používá Spark 2.1 na HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="894e9-109">This example used Spark 2.1 on HDInsight 3.6.</span></span> <span data-ttu-id="894e9-110">Strukturované Streaming se považuje za __alpha__ 2.1 Spark.</span><span class="sxs-lookup"><span data-stu-id="894e9-110">Structured Streaming is considered __alpha__ on Spark 2.1.</span></span>
>
> <span data-ttu-id="894e9-111">Kroky v tomto dokumentu vytvořte skupinu prostředků Azure, která obsahuje oba Spark v HDInsight a Kafka na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="894e9-111">The steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="894e9-112">Tyto clustery jsou obě nachází v rámci virtuální síť Azure, což umožňuje clusteru Spark přímo komunikovat s Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="894e9-112">These clusters are both located within an Azure Virtual Network, which allows the Spark cluster to directly communicate with the Kafka cluster.</span></span>
>
> <span data-ttu-id="894e9-113">Po dokončení kroků v tomto dokumentu, nezapomeňte odstranit clustery nadbytečné náklady.</span><span class="sxs-lookup"><span data-stu-id="894e9-113">When you are done with the steps in this document, remember to delete the clusters to avoid excess charges.</span></span>

## <a name="create-the-clusters"></a><span data-ttu-id="894e9-114">Vytváření clusterů</span><span class="sxs-lookup"><span data-stu-id="894e9-114">Create the clusters</span></span>

<span data-ttu-id="894e9-115">Apache Kafka v HDInsight neposkytuje přístup k zprostředkovatelé Kafka prostřednictvím veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="894e9-115">Apache Kafka on HDInsight does not provide access to the Kafka brokers over the public internet.</span></span> <span data-ttu-id="894e9-116">Všechno, co komunikuje se Kafka musí být ve stejné virtuální síti Azure jako uzly v clusteru Kafka.</span><span class="sxs-lookup"><span data-stu-id="894e9-116">Anything that talks to Kafka must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="894e9-117">V tomto příkladu jsou Kafka i Spark clusterů umístěné v virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="894e9-117">For this example, both the Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="894e9-118">Následující diagram znázorňuje tok komunikace mezi clustery:</span><span class="sxs-lookup"><span data-stu-id="894e9-118">The following diagram shows how communication flows between the clusters:</span></span>

![Diagram clustery Spark a Kafka v virtuální sítě Azure](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="894e9-120">Kafka služby je omezený na komunikaci v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="894e9-120">The Kafka service is limited to communication within the virtual network.</span></span> <span data-ttu-id="894e9-121">Jiné služby v clusteru, například SSH a Ambari, jsou přístupné přes internet.</span><span class="sxs-lookup"><span data-stu-id="894e9-121">Other services on the cluster, such as SSH and Ambari, can be accessed over the internet.</span></span> <span data-ttu-id="894e9-122">Další informace o veřejné porty, které jsou k dispozici s HDInsight naleznete v tématu [porty a identifikátory URI používají v prostředí HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="894e9-122">For more information on the public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="894e9-123">Když vytvoříte virtuální síť Azure, Kafka, a clustery Spark ručně, je jednodušší použít šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="894e9-123">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="894e9-124">Použijte následující kroky k nasazení virtuální sítě Azure, Kafka a clustery k předplatnému Azure z Spark.</span><span class="sxs-lookup"><span data-stu-id="894e9-124">Use the following steps to deploy an Azure virtual network, Kafka, and Spark clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="894e9-125">Na následující tlačítko použijte pro přihlášení do Azure a otevřete šablonu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="894e9-125">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    <span data-ttu-id="894e9-126">Šablona Azure Resource Manager je umístěna ve **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span><span class="sxs-lookup"><span data-stu-id="894e9-126">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span></span>

    <span data-ttu-id="894e9-127">Tato šablona vytváří v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="894e9-127">This template creates the following resources:</span></span>

    * <span data-ttu-id="894e9-128">Kafka v clusteru HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="894e9-128">A Kafka on HDInsight 3.5 cluster.</span></span>
    * <span data-ttu-id="894e9-129">Spark v HDInsight 3.6 clusteru.</span><span class="sxs-lookup"><span data-stu-id="894e9-129">A Spark on HDInsight 3.6 cluster.</span></span>
    * <span data-ttu-id="894e9-130">Virtuální síť Azure, který obsahuje clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="894e9-130">An Azure Virtual Network, which contains the HDInsight clusters.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="894e9-131">Strukturované streamování poznámkového bloku použitý v tomto příkladu vyžaduje Spark v HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="894e9-131">The structured streaming notebook used in this example requires Spark on HDInsight 3.6.</span></span> <span data-ttu-id="894e9-132">Pokud používáte starší verzi Spark v HDInsight, zobrazí se chyba při použití poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="894e9-132">If you use an earlier version of Spark on HDInsight, you receive errors when using the notebook.</span></span>

2. <span data-ttu-id="894e9-133">Následující informace slouží k naplnění položek na **vlastní nasazení** okno:</span><span class="sxs-lookup"><span data-stu-id="894e9-133">Use the following information to populate the entries on the **Custom deployment** blade:</span></span>
   
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="894e9-135">**Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="894e9-135">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="894e9-136">Tato skupina obsahuje clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="894e9-136">This group contains the HDInsight cluster.</span></span>

    * <span data-ttu-id="894e9-137">**Umístění**: Vyberte umístění geograficky blízko vás.</span><span class="sxs-lookup"><span data-stu-id="894e9-137">**Location**: Select a location geographically close to you.</span></span>

    * <span data-ttu-id="894e9-138">**Základní název clusteru**: Tato hodnota se používá jako základní název pro Spark a Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="894e9-138">**Base Cluster Name**: This value is used as the base name for the Spark and Kafka clusters.</span></span> <span data-ttu-id="894e9-139">Například zadáním **hdi** vytvoří Spark clusteru s názvem spark hdi__ a Kafka clusteru s názvem **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="894e9-139">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="894e9-140">**Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce pro clustery Spark a Kafka.</span><span class="sxs-lookup"><span data-stu-id="894e9-140">**Cluster Login User Name**: The admin user name for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="894e9-141">**Heslo pro přihlášení clusteru**: uživatelské heslo správce pro clustery Spark a Kafka.</span><span class="sxs-lookup"><span data-stu-id="894e9-141">**Cluster Login Password**: The admin user password for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="894e9-142">**Uživatelské jméno SSH**: SSH, aby uživatel vytvořil pro clustery Spark a Kafka.</span><span class="sxs-lookup"><span data-stu-id="894e9-142">**SSH User Name**: The SSH user to create for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="894e9-143">**Heslo SSH**: heslo pro uživatele SSH pro clustery Spark a Kafka.</span><span class="sxs-lookup"><span data-stu-id="894e9-143">**SSH Password**: The password for the SSH user for the Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="894e9-144">Pro čtení **podmínky a ujednání**a potom vyberte **souhlasím s podmínkami a ujednáními výše uvedených**.</span><span class="sxs-lookup"><span data-stu-id="894e9-144">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="894e9-145">Nakonec zkontrolujte **připnout na řídicí panel** a pak vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="894e9-145">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="894e9-146">Chcete-li vytvořit clustery trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="894e9-146">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="894e9-147">Po vytvoření prostředky budete přesměrováni na okně skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="894e9-147">Once the resources have been created, you are redirected to the resource group blade.</span></span>

![Okno skupiny prostředků pro virtuální síť a clustery](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="894e9-149">Všimněte si, že jsou názvy clusterů HDInsight **spark BASENAME** a **kafka BASENAME**, kde BASENAME je jméno, které jste zadali v šabloně.</span><span class="sxs-lookup"><span data-stu-id="894e9-149">Notice that the names of the HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="894e9-150">Názvy těchto používat v dalších krocích při připojování k clustery.</span><span class="sxs-lookup"><span data-stu-id="894e9-150">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="get-the-kafka-brokers"></a><span data-ttu-id="894e9-151">Získat Kafka zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="894e9-151">Get the Kafka brokers</span></span>

<span data-ttu-id="894e9-152">Kód v tomto příkladu se připojí k Kafka zprostředkovatele hostitelů Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="894e9-152">The code in this example connects to the Kafka broker hosts in the Kafka cluster.</span></span> <span data-ttu-id="894e9-153">Když Pokud chcete najít Kafka zprostředkovatele hostitele, použijte následující příklad PowerShell nebo Bash:</span><span class="sxs-lookup"><span data-stu-id="894e9-153">To find the Kafka broker hosts, use the following PowerShell or Bash example:</span></span>

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name
($brokerHosts -join ":9092,") + ":9092"
```

```bash
curl -u admin:$PASSWORD -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")'
```

> [!NOTE]
> <span data-ttu-id="894e9-154">Tento příklad předpokládá, že `$PASSWORD` obsahovat heslo pro přihlášení clusteru, a `$CLUSTERNAME` tak, aby obsahovala název clusteru Kafka.</span><span class="sxs-lookup"><span data-stu-id="894e9-154">This example expects `$PASSWORD` to contain the password for the cluster login, and `$CLUSTERNAME` to contain the name of the Kafka cluster.</span></span>
>
> <span data-ttu-id="894e9-155">Tento příklad používá [jq](https://stedolan.github.io/jq/) nástroj analyzovat data z dokumentu JSON.</span><span class="sxs-lookup"><span data-stu-id="894e9-155">This example uses the [jq](https://stedolan.github.io/jq/) utility to parse data out of the JSON document.</span></span>

<span data-ttu-id="894e9-156">Výstup se bude podobat následujícímu:</span><span class="sxs-lookup"><span data-stu-id="894e9-156">The output is similar to the following text:</span></span>

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn2-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn3-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

<span data-ttu-id="894e9-157">Tyto informace uložte, protože se používá v následujících částech tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="894e9-157">Save this information, as it is used in the following sections of this document.</span></span>

## <a name="get-the-notebooks"></a><span data-ttu-id="894e9-158">Získat poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="894e9-158">Get the notebooks</span></span>

<span data-ttu-id="894e9-159">Kód pro tento příklad popsané v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span><span class="sxs-lookup"><span data-stu-id="894e9-159">The code for the example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span></span>

## <a name="upload-the-notebooks"></a><span data-ttu-id="894e9-160">Nahrát poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="894e9-160">Upload the notebooks</span></span>

<span data-ttu-id="894e9-161">Nahrát poznámkových bloků z projektu do vaší Spark v clusteru HDInsight pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="894e9-161">Use the following steps to upload the notebooks from the project to your Spark on HDInsight cluster:</span></span>

1. <span data-ttu-id="894e9-162">Ve webovém prohlížeči připojte do poznámkového bloku Jupyter v clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="894e9-162">In your web browser, connect to the Jupyter notebook on your Spark cluster.</span></span> <span data-ttu-id="894e9-163">V následující adresu URL, nahraďte `CLUSTERNAME` s názvem vašeho clusteru Kafka:</span><span class="sxs-lookup"><span data-stu-id="894e9-163">In the following URL, replace `CLUSTERNAME` with the name of your Kafka cluster:</span></span>

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    <span data-ttu-id="894e9-164">Po zobrazení výzvy zadejte přihlašovací clusteru (správce) a heslo použité při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="894e9-164">When prompted, enter the cluster login (admin) and password used when you created the cluster.</span></span>

2. <span data-ttu-id="894e9-165">Použijte v horní pravé části stránky se __nahrát__ tlačítko Odeslat __datového proudu-Tweetů-To_Kafka.ipynb__ souboru do clusteru.</span><span class="sxs-lookup"><span data-stu-id="894e9-165">From the upper right side of the page, use the __Upload__ button to upload the __Stream-Tweets-To_Kafka.ipynb__ file to the cluster.</span></span> <span data-ttu-id="894e9-166">Vyberte __otevřete__ spusťte.</span><span class="sxs-lookup"><span data-stu-id="894e9-166">Select __Open__ to start the upload.</span></span>

    ![Pomocí tlačítka nahrávání vyberte a odeslat Poznámkový blok](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Vyberte soubor KafkaStreaming.ipynb](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. <span data-ttu-id="894e9-169">Najít __datového proudu-Tweetů-To_Kafka.ipynb__ položku v seznamu poznámkových bloků a vyberte __nahrát__ tlačítko vedle ní.</span><span class="sxs-lookup"><span data-stu-id="894e9-169">Find the __Stream-Tweets-To_Kafka.ipynb__ entry in the list of notebooks, and select __Upload__ button beside it.</span></span>

    ![Pomocí tlačítka nahrávání vedle položky KafkaStreaming.ipynb nahrát na server poznámkového bloku](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. <span data-ttu-id="894e9-171">Opakujte kroky 1 – 3 načíst __Spark-strukturovaná-streamování-z – Kafka.ipynb__ poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="894e9-171">Repeat steps 1-3 to load the __Spark-Structured-Streaming-From-Kafka.ipynb__ notebook.</span></span>

## <a name="load-tweets-into-kafka"></a><span data-ttu-id="894e9-172">Načtení tweetů do Kafka</span><span class="sxs-lookup"><span data-stu-id="894e9-172">Load tweets into Kafka</span></span>

<span data-ttu-id="894e9-173">Jakmile soubory byly odeslány, vyberte __datového proudu-Tweetů-To_Kafka.ipynb__ záznam, tím otevřete Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="894e9-173">Once the files have been uploaded, select the __Stream-Tweets-To_Kafka.ipynb__ entry to open the notebook.</span></span> <span data-ttu-id="894e9-174">Postupujte podle kroků v poznámkovém bloku a načte tweetů do Kafka.</span><span class="sxs-lookup"><span data-stu-id="894e9-174">Follow the steps in the notebook to load tweets into Kafka.</span></span>

## <a name="process-tweets-using-spark-structured-streaming"></a><span data-ttu-id="894e9-175">Proces tweetů pomocí Spark strukturovaných streamování</span><span class="sxs-lookup"><span data-stu-id="894e9-175">Process tweets using Spark Structured Streaming</span></span>

<span data-ttu-id="894e9-176">Na domovské stránce poznámkového bloku Jupyter, vyberte __Spark-strukturovaná-streamování-z – Kafka.ipynb__ položku.</span><span class="sxs-lookup"><span data-stu-id="894e9-176">From the Jupyter Notebook home page, select the __Spark-Structured-Streaming-From-Kafka.ipynb__ entry.</span></span> <span data-ttu-id="894e9-177">Postupujte podle kroků v poznámkovém bloku se načíst tweetů z Kafka pomocí Spark strukturovaných streamování.</span><span class="sxs-lookup"><span data-stu-id="894e9-177">Follow the steps in the notebook to load tweets from Kafka using Spark Structured Streaming.</span></span>

## <a name="next-steps"></a><span data-ttu-id="894e9-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="894e9-178">Next steps</span></span>

<span data-ttu-id="894e9-179">Teď, když jste se naučili použití Spark strukturovaných streamování, najdete v následujících dokumentech Další informace o práci s Spark a Kafka:</span><span class="sxs-lookup"><span data-stu-id="894e9-179">Now that you have learned how to use Spark Structured Streaming, see the following documents to learn more about working with Spark and Kafka:</span></span>

* <span data-ttu-id="894e9-180">[Jak používat Spark streamování (DStream) s Kafka](hdinsight-apache-spark-with-kafka.md).</span><span class="sxs-lookup"><span data-stu-id="894e9-180">[How to use Spark streaming (DStream) with Kafka](hdinsight-apache-spark-with-kafka.md).</span></span>
* [<span data-ttu-id="894e9-181">Začněte s Poznámkový blok Jupyter a Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="894e9-181">Start with Jupyter Notebook and Spark on HDInsight</span></span>](hdinsight-apache-spark-jupyter-spark-sql.md)