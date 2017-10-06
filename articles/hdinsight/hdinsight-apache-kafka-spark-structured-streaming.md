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
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a>Použít Spark strukturovaných streamování s Kafka (preview) v HDInsight

Zjistěte, jak toouse Spark strukturovaných tooread z datových proudů z Apache Kafka v Azure HDInsight.

Vysílání datového proudu strukturovaná Spark je modul zpracování datového proudu, který je založený na Spark SQL. Je možné, že jste tooexpress streamování výpočty hello stejné jako výpočetní batch na statických dat. Další informace o strukturovaných streamování najdete v tématu hello [strukturovaných streamování Průvodce programováním [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) na Apache.org.

> [!IMPORTANT]
> Tento příklad používá Spark 2.1 na HDInsight 3.6. Strukturované Streaming se považuje za __alpha__ 2.1 Spark.
>
> Hello kroky v tomto dokumentu vytvořte skupinu prostředků Azure, která obsahuje oba Spark v HDInsight a Kafka na clusteru HDInsight. Tyto clustery jsou obě nachází v rámci virtuální síť Azure, což umožňuje hello toodirectly clusteru Spark komunikovat s hello Kafka clusteru.
>
> Až skončíte s hello kroky v tomto dokumentu, mějte na paměti toodelete hello clustery tooavoid nadbytečné poplatky.

## <a name="create-hello-clusters"></a>Vytvoření clusterů se hello

Apache Kafka v HDInsight neposkytuje přístup toohello Kafka zprostředkovatelé oproti hello veřejného Internetu. Všechno, co hello rozhovory tooKafka musí být ve stejné virtuální síti Azure jako uzly hello v hello Kafka clusteru. V tomto příkladu jsou umístěny hello Kafka a clustery Spark v Azure virtuální sítě. Hello následující diagram znázorňuje tok komunikace mezi clustery hello:

![Diagram clustery Spark a Kafka v virtuální sítě Azure](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Hello Kafka služby je omezený toocommunication v rámci virtuální sítě hello. Další služby v hello clusteru, například SSH a Ambari, může být přístupné přes hello internet. Další informace o veřejné porty hello k dispozici s HDInsight naleznete v tématu [porty a identifikátory URI používají v prostředí HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Můžete vytvořit virtuální síť Azure, Kafka a Spark clustery ručně, je snazší toouse šablonu Azure Resource Manager. Použití hello následující kroky toodeploy virtuální síť Azure, Kafka, a clustery Spark tooyour předplatného Azure.

1. Použijte hello tlačítko toosign v tooAzure a otevřete hello šablony v hello portálu Azure.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    Hello šablony Azure Resource Manageru se nachází v **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.

    Tato šablona vytvoří hello následující prostředky:

    * Kafka v clusteru HDInsight 3.5.
    * Spark v HDInsight 3.6 clusteru.
    * Virtuální síť Azure, který obsahuje clustery HDInsight hello.

    > [!IMPORTANT]
    > Hello strukturovaných streamování poznámkového bloku použitý v tomto příkladu vyžaduje Spark v HDInsight 3.6. Pokud používáte starší verzi Spark v HDInsight, zobrazí se chyba při použití hello Poznámkový blok.

2. Hello použijte následující informace toopopulate hello položky na hello **vlastní nasazení** okno:
   
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující. Tato skupina obsahuje hello clusteru HDInsight.

    * **Umístění**: Vyberte tooyou geograficky zavřít umístění.

    * **Základní název clusteru**: Tato hodnota se používá jako hello základní název pro clustery Spark a Kafka hello. Například zadáním **hdi** vytvoří Spark clusteru s názvem spark hdi__ a Kafka clusteru s názvem **kafka hdi**.

    * **Uživatelské jméno přihlášení clusteru**: hello uživatelské jméno správce pro clustery Spark a Kafka hello.

    * **Heslo pro přihlášení clusteru**: heslo uživatele hello správce pro clustery Spark a Kafka hello.

    * **Uživatelské jméno SSH**: hello toocreate uživatele SSH pro clustery Spark a Kafka hello.

    * **Heslo SSH**: hello heslo pro uživatele hello SSH pro clustery Spark a Kafka hello.

3. Čtení hello **podmínky a ujednání**a potom vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**.

4. Nakonec zkontrolujte **Pin toodashboard** a pak vyberte **nákupu**. Trvá přibližně 20 minut toocreate hello clustery.

Po vytvoření hello prostředky jste přesměrovaného toohello okně skupiny prostředků.

![Okno skupiny prostředků pro virtuální síť hello a clustery](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Všimněte si, že jsou názvy hello clusterů HDInsight hello **spark BASENAME** a **kafka BASENAME**, kde BASENAME je hello jméno, které jste zadali toohello šablony. Použít tyto názvy v dalších krocích při připojení toohello clustery.

## <a name="get-hello-kafka-brokers"></a>Získat hello Kafka zprostředkovatelé

Hello kód v tomto příkladu se připojí toohello Kafka zprostředkovatel hostitelé v clusteru Kafka hello. toofind hello zprostředkovatele hostitelů Kafka, použijte následující příklad PowerShell nebo Bash hello:

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
> Tento příklad předpokládá, že `$PASSWORD` toocontain hello heslo pro přihlášení hello clusteru, a `$CLUSTERNAME` toocontain hello název hello Kafka clusteru.
>
> Tento příklad používá hello [jq](https://stedolan.github.io/jq/) nástroj tooparse dat mimo hello dokumentu JSON.

Hello výstup je podobné toohello následující text:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn2-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn3-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Tyto informace uložte, protože se používá v následujících částech tohoto dokumentu hello.

## <a name="get-hello-notebooks"></a>Získat poznámkových bloků hello

Kód Hello například hello popsané v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).

## <a name="upload-hello-notebooks"></a>Nahrát poznámkových bloků hello

Pomocí následujících kroků tooupload hello poznámkových bloků z projektu tooyour hello Spark na clusteru HDInsight hello:

1. Ve webovém prohlížeči připojte toohello Poznámkový blok Jupyter v clusteru Spark. Následující adresa URL, nahraďte v hello `CLUSTERNAME` s názvem hello Kafka clusteru:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Po zobrazení výzvy zadejte přihlašovací údaje clusteru hello (správce) a heslo použité při vytváření clusteru hello.

2. Hello horní pravé části stránky hello se použije hello __nahrát__ tlačítko tooupload hello __datového proudu-Tweetů-To_Kafka.ipynb__ souboru toohello clusteru. Vyberte __otevřete__ toostart hello nahrávání.

    ![Použijte hello nahrávání tlačítko tooselect a nahrát Poznámkový blok](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Vyberte soubor KafkaStreaming.ipynb hello](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Najde hello __datového proudu-Tweetů-To_Kafka.ipynb__ položku v seznamu hello poznámkových bloků a vyberte __nahrát__ tlačítko vedle ní.

    ![Nahrávání hello použijte tlačítko vedle hello KafkaStreaming.ipynb položka tooupload ho toohello poznámkového bloku serveru](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. Opakujte kroky 1 – 3 tooload hello __Spark-strukturovaná-streamování-z – Kafka.ipynb__ poznámkového bloku.

## <a name="load-tweets-into-kafka"></a>Načtení tweetů do Kafka

Po odeslali soubory hello vybrat hello __datového proudu-Tweetů-To_Kafka.ipynb__ položka tooopen hello poznámkového bloku. Postupujte podle kroků hello v hello poznámkového bloku tooload tweetů do Kafka.

## <a name="process-tweets-using-spark-structured-streaming"></a>Proces tweetů pomocí Spark strukturovaných streamování

Hello Poznámkový blok Jupyter domovské stránky, vyberte hello __Spark-strukturovaná-streamování-z – Kafka.ipynb__ položku. Postupujte podle kroků hello v hello poznámkového bloku tooload tweetů z Kafka pomocí Spark strukturovaných streamování.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak zjistit hello následující dokumenty toolearn informace o práci s Spark a Kafka, toouse Spark strukturovaných streamování:

* [Jak toouse Spark streamování (DStream) s Kafka](hdinsight-apache-spark-with-kafka.md).
* [Začněte s Poznámkový blok Jupyter a Spark v HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md)