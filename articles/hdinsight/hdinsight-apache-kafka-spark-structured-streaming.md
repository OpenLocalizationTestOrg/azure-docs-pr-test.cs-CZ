---
title: "aaaApache strukturovaných streamování Spark s Kafka - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse Apache Spark (DStream) tooget dat do nebo z Apache Kafka. V tomto příkladu stream dat pomocí poznámkového bloku Jupyter z Spark v HDInsight."
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
ms.openlocfilehash: 0837e8fc5ea314e644daed029d596feeb2b02c68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="3edf5-104">Použít Spark strukturovaných streamování s Kafka (preview) v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3edf5-104">Use Spark Structured Streaming with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="3edf5-105">Zjistěte, jak toouse Spark strukturovaných tooread z datových proudů z Apache Kafka v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3edf5-105">Learn how toouse Spark Structured Streaming tooread data from Apache Kafka on Azure HDInsight.</span></span>

<span data-ttu-id="3edf5-106">Vysílání datového proudu strukturovaná Spark je modul zpracování datového proudu, který je založený na Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="3edf5-106">Spark structured streaming is a stream processing engine built on Spark SQL.</span></span> <span data-ttu-id="3edf5-107">Je možné, že jste tooexpress streamování výpočty hello stejné jako výpočetní batch na statických dat.</span><span class="sxs-lookup"><span data-stu-id="3edf5-107">It allows you tooexpress streaming computations hello same as batch computation on static data.</span></span> <span data-ttu-id="3edf5-108">Další informace o strukturovaných streamování najdete v tématu hello [strukturovaných streamování Průvodce programováním [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) na Apache.org.</span><span class="sxs-lookup"><span data-stu-id="3edf5-108">For more information on Structured Streaming, see hello [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) at Apache.org.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3edf5-109">Tento příklad používá Spark 2.1 na HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="3edf5-109">This example used Spark 2.1 on HDInsight 3.6.</span></span> <span data-ttu-id="3edf5-110">Strukturované Streaming se považuje za __alpha__ 2.1 Spark.</span><span class="sxs-lookup"><span data-stu-id="3edf5-110">Structured Streaming is considered __alpha__ on Spark 2.1.</span></span>
>
> <span data-ttu-id="3edf5-111">Hello kroky v tomto dokumentu vytvořte skupinu prostředků Azure, která obsahuje oba Spark v HDInsight a Kafka na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3edf5-111">hello steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="3edf5-112">Tyto clustery jsou obě nachází v rámci virtuální síť Azure, což umožňuje hello toodirectly clusteru Spark komunikovat s hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="3edf5-112">These clusters are both located within an Azure Virtual Network, which allows hello Spark cluster toodirectly communicate with hello Kafka cluster.</span></span>
>
> <span data-ttu-id="3edf5-113">Až skončíte s hello kroky v tomto dokumentu, mějte na paměti toodelete hello clustery tooavoid nadbytečné poplatky.</span><span class="sxs-lookup"><span data-stu-id="3edf5-113">When you are done with hello steps in this document, remember toodelete hello clusters tooavoid excess charges.</span></span>

## <a name="create-hello-clusters"></a><span data-ttu-id="3edf5-114">Vytvoření clusterů se hello</span><span class="sxs-lookup"><span data-stu-id="3edf5-114">Create hello clusters</span></span>

<span data-ttu-id="3edf5-115">Apache Kafka v HDInsight neposkytuje přístup toohello Kafka zprostředkovatelé oproti hello veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="3edf5-115">Apache Kafka on HDInsight does not provide access toohello Kafka brokers over hello public internet.</span></span> <span data-ttu-id="3edf5-116">Všechno, co hello rozhovory tooKafka musí být ve stejné virtuální síti Azure jako uzly hello v hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="3edf5-116">Anything that talks tooKafka must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="3edf5-117">V tomto příkladu jsou umístěny hello Kafka a clustery Spark v Azure virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3edf5-117">For this example, both hello Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="3edf5-118">Hello následující diagram znázorňuje tok komunikace mezi clustery hello:</span><span class="sxs-lookup"><span data-stu-id="3edf5-118">hello following diagram shows how communication flows between hello clusters:</span></span>

![Diagram clustery Spark a Kafka v virtuální sítě Azure](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="3edf5-120">Hello Kafka služby je omezený toocommunication v rámci virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="3edf5-120">hello Kafka service is limited toocommunication within hello virtual network.</span></span> <span data-ttu-id="3edf5-121">Další služby v hello clusteru, například SSH a Ambari, může být přístupné přes hello internet.</span><span class="sxs-lookup"><span data-stu-id="3edf5-121">Other services on hello cluster, such as SSH and Ambari, can be accessed over hello internet.</span></span> <span data-ttu-id="3edf5-122">Další informace o veřejné porty hello k dispozici s HDInsight naleznete v tématu [porty a identifikátory URI používají v prostředí HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="3edf5-122">For more information on hello public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="3edf5-123">Můžete vytvořit virtuální síť Azure, Kafka a Spark clustery ručně, je snazší toouse šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3edf5-123">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="3edf5-124">Použití hello následující kroky toodeploy virtuální síť Azure, Kafka, a clustery Spark tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="3edf5-124">Use hello following steps toodeploy an Azure virtual network, Kafka, and Spark clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="3edf5-125">Použijte hello tlačítko toosign v tooAzure a otevřete hello šablony v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3edf5-125">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    <span data-ttu-id="3edf5-126">Hello šablony Azure Resource Manageru se nachází v **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span><span class="sxs-lookup"><span data-stu-id="3edf5-126">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span></span>

    <span data-ttu-id="3edf5-127">Tato šablona vytvoří hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="3edf5-127">This template creates hello following resources:</span></span>

    * <span data-ttu-id="3edf5-128">Kafka v clusteru HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="3edf5-128">A Kafka on HDInsight 3.5 cluster.</span></span>
    * <span data-ttu-id="3edf5-129">Spark v HDInsight 3.6 clusteru.</span><span class="sxs-lookup"><span data-stu-id="3edf5-129">A Spark on HDInsight 3.6 cluster.</span></span>
    * <span data-ttu-id="3edf5-130">Virtuální síť Azure, který obsahuje clustery HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="3edf5-130">An Azure Virtual Network, which contains hello HDInsight clusters.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3edf5-131">Hello strukturovaných streamování poznámkového bloku použitý v tomto příkladu vyžaduje Spark v HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="3edf5-131">hello structured streaming notebook used in this example requires Spark on HDInsight 3.6.</span></span> <span data-ttu-id="3edf5-132">Pokud používáte starší verzi Spark v HDInsight, zobrazí se chyba při použití hello Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="3edf5-132">If you use an earlier version of Spark on HDInsight, you receive errors when using hello notebook.</span></span>

2. <span data-ttu-id="3edf5-133">Hello použijte následující informace toopopulate hello položky na hello **vlastní nasazení** okno:</span><span class="sxs-lookup"><span data-stu-id="3edf5-133">Use hello following information toopopulate hello entries on hello **Custom deployment** blade:</span></span>
   
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="3edf5-135">**Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="3edf5-135">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="3edf5-136">Tato skupina obsahuje hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3edf5-136">This group contains hello HDInsight cluster.</span></span>

    * <span data-ttu-id="3edf5-137">**Umístění**: Vyberte tooyou geograficky zavřít umístění.</span><span class="sxs-lookup"><span data-stu-id="3edf5-137">**Location**: Select a location geographically close tooyou.</span></span>

    * <span data-ttu-id="3edf5-138">**Základní název clusteru**: Tato hodnota se používá jako hello základní název pro clustery Spark a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="3edf5-138">**Base Cluster Name**: This value is used as hello base name for hello Spark and Kafka clusters.</span></span> <span data-ttu-id="3edf5-139">Například zadáním **hdi** vytvoří Spark clusteru s názvem spark hdi__ a Kafka clusteru s názvem **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="3edf5-139">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="3edf5-140">**Uživatelské jméno přihlášení clusteru**: hello uživatelské jméno správce pro clustery Spark a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="3edf5-140">**Cluster Login User Name**: hello admin user name for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="3edf5-141">**Heslo pro přihlášení clusteru**: heslo uživatele hello správce pro clustery Spark a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="3edf5-141">**Cluster Login Password**: hello admin user password for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="3edf5-142">**Uživatelské jméno SSH**: hello toocreate uživatele SSH pro clustery Spark a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="3edf5-142">**SSH User Name**: hello SSH user toocreate for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="3edf5-143">**Heslo SSH**: hello heslo pro uživatele hello SSH pro clustery Spark a Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="3edf5-143">**SSH Password**: hello password for hello SSH user for hello Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="3edf5-144">Čtení hello **podmínky a ujednání**a potom vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**.</span><span class="sxs-lookup"><span data-stu-id="3edf5-144">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="3edf5-145">Nakonec zkontrolujte **Pin toodashboard** a pak vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="3edf5-145">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="3edf5-146">Trvá přibližně 20 minut toocreate hello clustery.</span><span class="sxs-lookup"><span data-stu-id="3edf5-146">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="3edf5-147">Po vytvoření hello prostředky jste přesměrovaného toohello okně skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="3edf5-147">Once hello resources have been created, you are redirected toohello resource group blade.</span></span>

![Okno skupiny prostředků pro virtuální síť hello a clustery](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="3edf5-149">Všimněte si, že jsou názvy hello clusterů HDInsight hello **spark BASENAME** a **kafka BASENAME**, kde BASENAME je hello jméno, které jste zadali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="3edf5-149">Notice that hello names of hello HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="3edf5-150">Použít tyto názvy v dalších krocích při připojení toohello clustery.</span><span class="sxs-lookup"><span data-stu-id="3edf5-150">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="get-hello-kafka-brokers"></a><span data-ttu-id="3edf5-151">Získat hello Kafka zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="3edf5-151">Get hello Kafka brokers</span></span>

<span data-ttu-id="3edf5-152">Hello kód v tomto příkladu se připojí toohello Kafka zprostředkovatel hostitelé v clusteru Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="3edf5-152">hello code in this example connects toohello Kafka broker hosts in hello Kafka cluster.</span></span> <span data-ttu-id="3edf5-153">toofind hello zprostředkovatele hostitelů Kafka, použijte následující příklad PowerShell nebo Bash hello:</span><span class="sxs-lookup"><span data-stu-id="3edf5-153">toofind hello Kafka broker hosts, use hello following PowerShell or Bash example:</span></span>

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
$clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
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
> <span data-ttu-id="3edf5-154">Tento příklad předpokládá, že `$PASSWORD` toocontain hello heslo pro přihlášení hello clusteru, a `$CLUSTERNAME` toocontain hello název hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="3edf5-154">This example expects `$PASSWORD` toocontain hello password for hello cluster login, and `$CLUSTERNAME` toocontain hello name of hello Kafka cluster.</span></span>
>
> <span data-ttu-id="3edf5-155">Tento příklad používá hello [jq](https://stedolan.github.io/jq/) nástroj tooparse dat mimo hello dokumentu JSON.</span><span class="sxs-lookup"><span data-stu-id="3edf5-155">This example uses hello [jq](https://stedolan.github.io/jq/) utility tooparse data out of hello JSON document.</span></span>

<span data-ttu-id="3edf5-156">Hello výstup je podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="3edf5-156">hello output is similar toohello following text:</span></span>

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn2-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn3-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

<span data-ttu-id="3edf5-157">Tyto informace uložte, protože se používá v následujících částech tohoto dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="3edf5-157">Save this information, as it is used in hello following sections of this document.</span></span>

## <a name="get-hello-notebooks"></a><span data-ttu-id="3edf5-158">Získat poznámkových bloků hello</span><span class="sxs-lookup"><span data-stu-id="3edf5-158">Get hello notebooks</span></span>

<span data-ttu-id="3edf5-159">Kód Hello například hello popsané v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span><span class="sxs-lookup"><span data-stu-id="3edf5-159">hello code for hello example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span></span>

## <a name="upload-hello-notebooks"></a><span data-ttu-id="3edf5-160">Nahrát poznámkových bloků hello</span><span class="sxs-lookup"><span data-stu-id="3edf5-160">Upload hello notebooks</span></span>

<span data-ttu-id="3edf5-161">Pomocí následujících kroků tooupload hello poznámkových bloků z projektu tooyour hello Spark na clusteru HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="3edf5-161">Use hello following steps tooupload hello notebooks from hello project tooyour Spark on HDInsight cluster:</span></span>

1. <span data-ttu-id="3edf5-162">Ve webovém prohlížeči připojte toohello Poznámkový blok Jupyter v clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="3edf5-162">In your web browser, connect toohello Jupyter notebook on your Spark cluster.</span></span> <span data-ttu-id="3edf5-163">Následující adresa URL, nahraďte v hello `CLUSTERNAME` s názvem hello Kafka clusteru:</span><span class="sxs-lookup"><span data-stu-id="3edf5-163">In hello following URL, replace `CLUSTERNAME` with hello name of your Kafka cluster:</span></span>

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    <span data-ttu-id="3edf5-164">Po zobrazení výzvy zadejte přihlašovací údaje clusteru hello (správce) a heslo použité při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="3edf5-164">When prompted, enter hello cluster login (admin) and password used when you created hello cluster.</span></span>

2. <span data-ttu-id="3edf5-165">Hello horní pravé části stránky hello se použije hello __nahrát__ tlačítko tooupload hello __datového proudu-Tweetů-To_Kafka.ipynb__ souboru toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="3edf5-165">From hello upper right side of hello page, use hello __Upload__ button tooupload hello __Stream-Tweets-To_Kafka.ipynb__ file toohello cluster.</span></span> <span data-ttu-id="3edf5-166">Vyberte __otevřete__ toostart hello nahrávání.</span><span class="sxs-lookup"><span data-stu-id="3edf5-166">Select __Open__ toostart hello upload.</span></span>

    ![Použijte hello nahrávání tlačítko tooselect a nahrát Poznámkový blok](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Vyberte soubor KafkaStreaming.ipynb hello](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. <span data-ttu-id="3edf5-169">Najde hello __datového proudu-Tweetů-To_Kafka.ipynb__ položku v seznamu hello poznámkových bloků a vyberte __nahrát__ tlačítko vedle ní.</span><span class="sxs-lookup"><span data-stu-id="3edf5-169">Find hello __Stream-Tweets-To_Kafka.ipynb__ entry in hello list of notebooks, and select __Upload__ button beside it.</span></span>

    ![Nahrávání hello použijte tlačítko vedle hello KafkaStreaming.ipynb položka tooupload ho toohello poznámkového bloku serveru](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. <span data-ttu-id="3edf5-171">Opakujte kroky 1 – 3 tooload hello __Spark-strukturovaná-streamování-z – Kafka.ipynb__ poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="3edf5-171">Repeat steps 1-3 tooload hello __Spark-Structured-Streaming-From-Kafka.ipynb__ notebook.</span></span>

## <a name="load-tweets-into-kafka"></a><span data-ttu-id="3edf5-172">Načtení tweetů do Kafka</span><span class="sxs-lookup"><span data-stu-id="3edf5-172">Load tweets into Kafka</span></span>

<span data-ttu-id="3edf5-173">Po odeslali soubory hello vybrat hello __datového proudu-Tweetů-To_Kafka.ipynb__ položka tooopen hello poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="3edf5-173">Once hello files have been uploaded, select hello __Stream-Tweets-To_Kafka.ipynb__ entry tooopen hello notebook.</span></span> <span data-ttu-id="3edf5-174">Postupujte podle kroků hello v hello poznámkového bloku tooload tweetů do Kafka.</span><span class="sxs-lookup"><span data-stu-id="3edf5-174">Follow hello steps in hello notebook tooload tweets into Kafka.</span></span>

## <a name="process-tweets-using-spark-structured-streaming"></a><span data-ttu-id="3edf5-175">Proces tweetů pomocí Spark strukturovaných streamování</span><span class="sxs-lookup"><span data-stu-id="3edf5-175">Process tweets using Spark Structured Streaming</span></span>

<span data-ttu-id="3edf5-176">Hello Poznámkový blok Jupyter domovské stránky, vyberte hello __Spark-strukturovaná-streamování-z – Kafka.ipynb__ položku.</span><span class="sxs-lookup"><span data-stu-id="3edf5-176">From hello Jupyter Notebook home page, select hello __Spark-Structured-Streaming-From-Kafka.ipynb__ entry.</span></span> <span data-ttu-id="3edf5-177">Postupujte podle kroků hello v hello poznámkového bloku tooload tweetů z Kafka pomocí Spark strukturovaných streamování.</span><span class="sxs-lookup"><span data-stu-id="3edf5-177">Follow hello steps in hello notebook tooload tweets from Kafka using Spark Structured Streaming.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3edf5-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3edf5-178">Next steps</span></span>

<span data-ttu-id="3edf5-179">Teď, když jste se naučili, jak zjistit hello následující dokumenty toolearn informace o práci s Spark a Kafka, toouse Spark strukturovaných streamování:</span><span class="sxs-lookup"><span data-stu-id="3edf5-179">Now that you have learned how toouse Spark Structured Streaming, see hello following documents toolearn more about working with Spark and Kafka:</span></span>

* <span data-ttu-id="3edf5-180">[Jak toouse Spark streamování (DStream) s Kafka](hdinsight-apache-spark-with-kafka.md).</span><span class="sxs-lookup"><span data-stu-id="3edf5-180">[How toouse Spark streaming (DStream) with Kafka](hdinsight-apache-spark-with-kafka.md).</span></span>
* [<span data-ttu-id="3edf5-181">Začněte s Poznámkový blok Jupyter a Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3edf5-181">Start with Jupyter Notebook and Spark on HDInsight</span></span>](hdinsight-apache-spark-jupyter-spark-sql.md)