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
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a>Apache Spark streamování (DStream) příklad s Kafka (preview) v HDInsight

Zjistěte, jak toouse Spark Apache Spark toostream data do nebo z Apache Kafka v HDInsight pomocí DStreams. Tento příklad používá poznámkového bloku Jupyter, která běží na clusteru Spark hello.
> [!NOTE]
> Hello kroky v tomto dokumentu vytvořte skupinu prostředků Azure, která obsahuje oba Spark v HDInsight a Kafka na clusteru HDInsight. Tyto clustery jsou obě nachází v rámci virtuální síť Azure, což umožňuje hello toodirectly clusteru Spark komunikovat s hello Kafka clusteru.
>
> Až skončíte s hello kroky v tomto dokumentu, mějte na paměti toodelete hello clustery tooavoid nadbytečné poplatky.

## <a name="create-hello-clusters"></a>Vytvoření clusterů se hello

Apache Kafka v HDInsight neposkytuje přístup toohello Kafka zprostředkovatelé oproti hello veřejného Internetu. Všechno, co hello rozhovory tooKafka musí být ve stejné virtuální síti Azure jako uzly hello v hello Kafka clusteru. V tomto příkladu jsou umístěny hello Kafka a clustery Spark v Azure virtuální sítě. Hello následující diagram znázorňuje tok komunikace mezi clustery hello:

![Diagram clustery Spark a Kafka v virtuální sítě Azure](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Když je omezená toocommunication v rámci virtuální sítě hello Kafka, samotné, dalších služeb v hello clusteru například SSH a Ambari je přístupná přes hello Internetu. Další informace o veřejné porty hello k dispozici s HDInsight naleznete v tématu [porty a identifikátory URI používají v prostředí HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Můžete vytvořit virtuální síť Azure, Kafka a Spark clustery ručně, je snazší toouse šablonu Azure Resource Manager. Použití hello následující kroky toodeploy virtuální síť Azure, Kafka, a clustery Spark tooyour předplatného Azure.

1. Použijte hello tlačítko toosign v tooAzure a otevřete hello šablony v hello portálu Azure.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    Hello šablony Azure Resource Manageru se nachází v **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > dostupnost tooguarantee Kafka v HDInsight, cluster musí obsahovat aspoň tři uzly pracovního procesu. Tato šablona vytvoří cluster Kafka, který obsahuje tři uzly pracovního procesu.

    Tato šablona vytvoří cluster služby HDInsight 3.6 Kafka a Spark.

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

Po vytvoření hello prostředky jste přesměrovaného tooa okně hello skupinu prostředků, která obsahuje hello clustery a webové řídicí panel.

![Okno skupiny prostředků pro virtuální síť hello a clustery](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Všimněte si, že jsou názvy hello clusterů HDInsight hello **spark BASENAME** a **kafka BASENAME**, kde BASENAME je hello jméno, které jste zadali toohello šablony. Použít tyto názvy v dalších krocích při připojení toohello clustery.

## <a name="use-hello-notebooks"></a>Použití poznámkových bloků hello

Kód Hello například hello popsané v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).

Postupujte podle kroků hello v hello `README.md` souboru toocomplete v tomto příkladu.

## <a name="delete-hello-cluster"></a>Odstranění clusteru hello

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Vzhledem k tomu, že hello kroky v tomto dokumentu vytvořte oba clustery v hello stejnou skupinu prostředků Azure, můžete odstranit skupinu prostředků hello hello portálu Azure. Odstraněním skupiny hello odebere všechny prostředky, které jsou vytvořené pomocí následujících tento dokument, hello virtuální sítě Azure a účet úložiště, které jsou používané clustery hello.

## <a name="next-steps"></a>Další kroky

V tomto příkladu jste zjistili, jak toouse Spark tooread a zápis tooKafka. Použijte následující odkazy toodiscover hello jiné způsoby toowork s Kafka:

* [Začínáme s Apache Kafka v HDInsight](hdinsight-apache-kafka-get-started.md)
* [Použít MirrorMaker toocreate repliku Kafka v HDInsight](hdinsight-apache-kafka-mirroring.md)
* [Použití Apache Stormu se systémem Kafka ve službě HDInsight](hdinsight-apache-storm-with-kafka.md)

