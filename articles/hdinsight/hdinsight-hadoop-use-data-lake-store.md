---
title: "aaaUse Data Lake Store s Hadoop v prostředí Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooquery dat z Azure Data Lake Store a toostore výsledky analýzy."
keywords: "blob storage, hdfs, strukturovaná data, nestrukturovaná data, data lake store"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: jgao
ms.openlocfilehash: 89633218a37a2fe05043e05d61199dcc0252d7f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-data-lake-store-with-azure-hdinsight-clusters"></a>Použití služby Data Lake Store s clustery Azure HDInsight

tooanalyze data v clusteru HDInsight, hello data můžete ukládat buď v [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md), nebo obojí. Obě možnosti úložiště povolit toosafely odstranění clusterů HDInsight, jsou používány pro výpočty, aniž by se ztratila uživatelská data.

V tomto článku se dozvíte, jak služba Data Lake Store pracuje s clustery HDInsight. toolearn jak funguje Azure Storage s clustery HDInsight, najdete v části [použití služby Azure Storage s Azure HDInsight clustery](hdinsight-hadoop-use-blob-storage.md). Další informace o vytvoření clusteru HDInsight najdete v tématu [Vytváření clusterů Hadoop ve službě HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!NOTE]
> Ke službě Data Lake Store se vždy přistupuje prostřednictvím zabezpečeného kanálu, takže se nepoužívá název schématu systému souborů `adls`. Vždy používáte `adl`.
> 
> 

## <a name="availabilities-for-hdinsight-clusters"></a>Dostupnosti pro clustery HDInsight

Hadoop podporuje hodnoty z hello výchozí systém souborů. Hello výchozí systém souborů znamená výchozí schéma a autoritu. Lze také použít tooresolve relativní cesty. Během procesu vytváření clusteru HDInsight hello, můžete zadat kontejner objektů blob v Azure Storage jako hello výchozí systém souborů, nebo s HDInsight 3.5 a novější verze, můžete vybrat Azure Storage nebo Azure Data Lake Store jako hello výchozí systém souborů s několik výjimek. 

Clustery HDInsight můžou službu Data Lake Store využívat dvěma způsoby:

* Jako výchozí úložiště hello
* Jako další úložiště, přičemž Azure Storage Blob je výchozí úložiště.

Od nyní jenom některé z hello HDInsight clusteru podporu typy nebo verze pomocí Data Lake Store jako výchozí úložiště a dalších účtů úložiště:

| Typ clusteru HDInsight | Data Lake Store jako výchozí úložiště | Data Lake Store jako další úložiště| Poznámky |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight verze 3.6 | Ano | Ano | |
| HDInsight verze 3.5 | Ano | Ano | S výjimkou hello HBase|
| HDInsight verze 3.4 | Ne | Ano | |
| HDInsight verze 3.3 | Ne | Ne | |
| HDInsight verze 3.2 | Ne | Ano | |
| HDInsight Premium (úroveň)| Ne | Ne | |
| Storm | | |Data Lake Store toowrite dat můžete použít z topologie Storm. Data Lake Store můžete také použít pro referenční data, která pak může číst topologie Storm.|

Pomocí Data Lake Store jako další úložiště účet ovlivnit výkon nebo hello možnost tooread nebo zápisu tooAzure úložiště z clusteru hello.


## <a name="use-data-lake-store-as-default-storage"></a>Použití služby Data Lake Store jako výchozího úložiště

Po nasazení HDInsight s Data Lake Store jako výchozí úložiště hello clusteru související soubory jsou uložené v Data Lake Store v hello následující umístění:

    adl://mydatalakestore/<cluster_root_path>/

kde `<cluster_root_path>` je hello název složky vytvoříte v Data Lake Store. Zadat cestu ke kořenové pro každý cluster, můžete použít stejný účet Data Lake Store pro více než jeden cluster hello. Takže máte nastavení, kde:

* Cluster1 hello cestu můžete použít.`adl://mydatalakestore/cluster1storage`
* Cluster2 hello cestu můžete použít.`adl://mydatalakestore/cluster2storage`

Všimněte si, že obě hello clustery pomocí hello stejný účet Data Lake Store **mydatalakestore**. Každý cluster má přístup tooits vlastní kořenové systému souborů v Data Lake Store. Hello prostředí nasazení portálu Azure na konkrétní vyzve vás toouse název složky, jako **/clusters/\<clustername >** pro kořenovou cestu hello.

toobe možné toouse jako výchozí úložiště Data Lake Store, musí udělit hello služby hlavní přístup toohello následující cesty:

- Hello kořenového účtu Data Lake Store.  Například: adl://mydatalakestore/.
- Hello složka pro všechny složky clusteru.  Například: adl://mydatalakestore/clusters.
- Hello složka pro hello clusteru.  Například: adl://mydatalakestore/clusters/cluster1storage.

Další informace o vytvoření instančního objektu a udělení přístupu najdete v části [Konfigurace přístupu ke službě Data Lake Store](#configure-data-lake-store-access).


## <a name="use-data-lake-store-as-additional-storage"></a>Použití služby Data Lake Store jako dalšího úložiště

Data Lake Store můžete použít jako další úložiště pro cluster hello také. V takových případech hello clusteru výchozí úložiště může být objektu Blob úložiště Azure nebo účtu Data Lake Store. Pokud používáte úloh HDInsight proti hello data uložená v Data Lake Store jako další úložiště, musíte použít soubory toohello hello plně kvalifikovanou cestu. Například:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Všimněte si, že neexistuje žádná **cluster_root_path** v adrese URL hello teď. Je to způsobeno Data Lake Store není výchozí úložiště v tomto případě tak, aby všechny potřebné toodo zadejte hello cesta toohello soubory.

toobe možné toouse Data Lake Store jako další úložiště, potřebujete jenom toogrant hello služby hlavní toohello cesty přístupu níž jsou uloženy soubory.  Například:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Další informace o vytvoření instančního objektu a udělení přístupu najdete v části [Konfigurace přístupu ke službě Data Lake Store](#configure-data-lake-store-access).


## <a name="use-more-than-one-data-lake-store-accounts"></a>Použití více účtů Data Lake Store

Přidání účtu Data Lake Store jako další a přidání více než jeden Data Lake Store jsou účty dosáhnout tím, že oprávnění clusteru HDInsight hello na data v jedné nebo více účtů Data Lake Store. Viz [Konfigurace přístupu ke službě Data Lake Store](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Konfigurace přístupu ke službě Data Lake Store

tooconfigure Data Lake store přístup z clusteru HDInsight, musí mít Azure Active directory (Azure AD) instanční objekt. Instanční objekt může vytvořit pouze správce Azure AD. Hello instanční objekt musí být vytvořeny s certifikátem. Další informace najdete v části [Konfigurace přístupu ke službě Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access) a [Vytvoření instančního objektu s certifikátem podepsaným svým držitelem](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).

> [!NOTE]
> Pokud chcete toouse Azure Data Lake Store jako další úložiště pro HDInsight cluster, důrazně doporučujeme, abyste to provedli při vytváření clusteru hello, jak je popsáno v tomto článku. Přidání Azure Data Lake Store jako další úložiště tooan stávající cluster HDInsight je složitý proces a tooerrors náchylné k chybám.
>

## <a name="access-files-from-hello-cluster"></a>Přístup k souborům z clusteru hello

Existuje několik způsobů hello souborů v Data Lake Store můžete přistupovat z clusteru služby HDInsight.

* **Pomocí plně kvalifikovaného názvu hello**. S tímto přístupem, můžete zadat hello úplná cesta toohello souboru, které chcete tooaccess.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Pomocí formát zkrácení cesty hello**. S tímto přístupem je nahradit hello cesty až kořenový cluster toohello adl: / / /. V předchozím příkladu hello, takže můžete nahradit `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` s `adl:///`.

        adl:///<file path>

* **Pomocí relativní cesty hello**. S tímto přístupem zadáte pouze toohello hello relativní cestě k souboru, které chcete tooaccess. Například, pokud je soubor toohello hello úplnou cestu:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    Dostanete hello stejný soubor sample.log pomocí tuto relativní cestu.

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-toodata-lake-store"></a>Vytvoření clusterů HDInsight pomocí získat přístup k tooData Lake Store

Použijte následující odkazy pro podrobné pokyny v přístupu tooData Lake Store toocreate, které clusterů HDInsight pomocí hello.

* [Pomocí portálu](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [Pomocí PowerShellu (se službou Data Lake Store jako výchozím úložištěm)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Pomocí PowerShellu (se službou Data Lake Store jako dalším úložištěm)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Pomocí šablon Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak toouse HDFS kompatibilní s Azure Data Lake Store s HDInsight. To vám umožní toobuild, škálovatelnou a dlouhodobé, archivace řešení pro získávání dat a používání HDInsight toounlock hello informací uvnitř hello uložené strukturovaných a nestrukturovaných dat.

Další informace naleznete v tématu:

* [Začínáme se službou Azure HDInsight][hdinsight-get-started]
* [Začínáme se službou Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md)
* [Nahrání dat tooHDInsight][hdinsight-upload-data]
* [Použití Hivu se službou HDInsight][hdinsight-use-hive]
* [Použití Pigu se službou HDInsight][hdinsight-use-pig]
* [Použijte Azure Storage sdílené přístupové podpisy toorestrict přístup toodata s HDInsight][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
