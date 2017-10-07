---
title: "aaaCopy tooand data z WASB do Data Lake Store pomocí Distcp | Microsoft Docs"
description: "Použití Distcp nástroj toocopy data tooand z Azure úložiště objektů BLOB tooData Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: ec23410bbab0f82449a475412bc3b097c4a8fccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-distcp-toocopy-data-between-azure-storage-blobs-and-data-lake-store"></a>Použití Distcp toocopy dat mezi objektů BLOB služby Azure Storage a Data Lake Store
> [!div class="op_single_selector"]
> * [Pomocí DistCp](data-lake-store-copy-data-wasb-distcp.md)
> * [Pomocí AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Po vytvoření clusteru služby HDInsight, který má přístup k účtu Data Lake Store tooa, můžete použít nástroje ekosystém Hadoop jako Distcp toocopy data **tooand z** úložišti clusteru HDInsight (WASB) do účtu Data Lake Store. Tento článek obsahuje pokyny, jak tooachieve to.

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure Data Lake Store**. Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight cluster** s tooa přístup k účtu Data Lake Store. V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Ujistěte se, že povolení vzdálené plochy pro hello clusteru.

## <a name="do-you-learn-fast-with-videos"></a>Pomáhají vám při učení videa?
[Přehrát toto video](https://mix.office.com/watch/1liuojvdx6sie) o toocopy dat mezi objektů BLOB služby Azure Storage a Data Lake Store pomocí DistCp.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Použití Distcp z clusteru služby HDInsight Linux

Cluster služby HDInsight se dodává s hello Distcp nástroj, který lze použít toocopy data z různých zdrojů do clusteru HDInsight. Pokud jste nakonfigurovali hello HDInsight clusteru toouse Data Lake Store jako další úložiště, hello Distcp nástroj lze použít tooand dat na více systémů pole toocopy z účet Data Lake Store. V této části podíváme na tom, jak toouse hello Distcp nástroj.

1. Z plochy použijte SSH tooconnect toohello clusteru. V tématu [clusteru HDInsight se systémem Linux tooa Connect](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Spusťte příkazy hello z řádku SSH hello.

2. Ověřte, zda máte přístup hello Azure úložiště objektů BLOB (WASB). Spusťte následující příkaz hello:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    To by mělo poskytovat seznam obsah v hello storage blob.
3. Stejně tak ověřte, zda máte přístup z clusteru hello hello účtu Data Lake Store. Spusťte následující příkaz hello:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    To by mělo poskytovat seznam souborů a složek v účtu Data Lake Store hello.
4. Použití Distcp toocopy data z WASB tooa účtu Data Lake Store.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Tím dojde ke zkopírování hello obsah hello **/příklad/data/gutenberg/** složky v WASB příliš**/myfolder** v hello účtu Data Lake Store.
5. Podobně použití Distcp toocopy data z tooWASB účet Data Lake Store.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Tím dojde ke zkopírování obsahu hello **/myfolder** v hello Data Lake Store účtu příliš**/příklad/data/gutenberg/** složky v WASB.

## <a name="performance-considerations-while-using-distcp"></a>Faktory ovlivňující výkon při použití DistCp

Vzhledem k tomu, že je nejnižší DistCp členitost je jeden soubor, nastavení hello maximální počet souběžných kopie je nejdůležitější parametr toooptimize hello ho s Data Lake Store. Toto se řídí nastavení hello počtu mappers ('m ') parametr na příkazovém řádku hello. Tento parametr určuje maximální počet mappers, které budou použité toocopy data hello. Výchozí hodnota je 20.

**Příklad**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-hello-number-of-mappers-toouse"></a>Jak je možné zjistit počet hello mappers toouse?

Tady je několik rad, kterými se můžete řídit.

* **Krok 1: Určení celkové paměti YARN** -hello prvním krokem je toodetermine hello YARN paměti k dispozici toohello clusteru kde spuštění úlohy DistCp hello. Tyto informace jsou k dispozici v portálu Ambari hello přidruženého k hello clusteru. Přejděte tooYARN a zobrazit hello konfigurací karta toosee hello YARN paměti. tooget hello celkové YARN paměti, násobkem hello YARN paměti na jeden uzel hello počtu uzlů, že máte v clusteru.

* **Krok 2: Vypočítat hello počet mappers** -hello hodnotu **m** je rovna toohello podíl celkové paměti YARN dělený hello velikost YARN kontejneru. Hello informace o velikosti kontejneru YARN je k dispozici také hello Ambari portálu. Přejděte tooYARN a zobrazení hello konfigurací kartě hello velikost YARN kontejner se zobrazí v tomto okně. Hello rovnice tooarrive v hello počet mappers (**m**) je

        m = (number of nodes * YARN memory for each node) / YARN container size

**Příklad**

Předpokládejme, že máte 4 D14v2s uzly v clusteru hello a pokoušíte tootransfer 10TB dat z 10 jiné složky. Každý hello složek obsahovat různých objemy dat a velikosti souborů hello v rámci každé složky se liší.

* Celkové paměti YARN - z hello Ambari portál zjistíte, že hello YARN paměti je 96GB pro D14 uzel. Ano je celková paměť YARN u clusteru se 4 uzly: 

        YARN memory = 4 * 96GB = 384GB

* Počet mappers - z hello Ambari portál zjistíte, že tento hello YARN kontejneru velikost je 3072 pro uzel clusteru s podporou D14. Ano počet mappers je:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Pokud jiné aplikace používají paměť, potom můžete pomocí tooonly část paměti YARN váš cluster pro DistCp.

### <a name="copying-large-datasets"></a>Kopírování rozsáhlých datových sad

Při přesunutí hello velikost toobe hello datové sady je moc velké (například > 1TB) nebo pokud máte mnoho různých složek, měli byste zvážit použití více DistCp úloh. Je pravděpodobné žádné výkonnější, ale bude snažte se úlohy hello, takže pokud se všechny úlohy nezdaří, bude stačí toorestart této konkrétní úlohy a spíš než celý projekt hello.

### <a name="limitations"></a>Omezení

* DistCp pokusí mappers toocreate, které se podobají velikost toooptimize výkonu. Zvýšení hello počet mappers nemusí vždy zvýšit výkon.

* DistCp je omezená tooonly jeden mapper na soubor. Proto by neměl mít další mappers, než kolik máte soubory. Vzhledem k tomu, že DistCp lze přiřadit pouze jeden mapper tooa souboru, toto nastavení omezuje hello množství souběžnosti, kterou lze použít toocopy velkých souborů.

* Pokud máte malý počet velkých souborů, pak je třeba rozdělit do 256MB souboru bloky toogive jste další potenciální souběžnosti. 
 
* Pokud kopírujete z účtu úložiště objektů Blob Azure, může vaše úlohu kopírování omezeny na straně úložiště objektů blob hello. To způsobí snížení výkonu hello kopírování úlohy. toolearn Další informace o omezení hello Azure Blob Storage, najdete v části omezení Azure Storage v [předplatného Azure a omezení služby](../azure-subscription-service-limits.md).

## <a name="see-also"></a>Viz také
* [Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
