---
title: "Nahrání dat pro úlohy Hadoop v HDInsight | Microsoft Docs"
description: "Zjistěte, jak nahrát a přístup k datům pro úlohy Hadoop v HDInsight pomocí rozhraní příkazového řádku Azure, Azure Storage Explorer, prostředí Azure PowerShell, příkazový řádek Hadoop nebo Sqoop."
keywords: "ETL hadoop, získávání dat do hadoop, hadoop načítání dat"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 56b913ee-0f9a-4e9f-9eaf-c571f8603dd6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: jgao
ms.openlocfilehash: 6867f96c8ea0e31ed0e682cef48e7aa5e3f65f86
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Nahrání dat úloh Hadoopu do služby HDInsight
Azure HDInsight nabízí plnohodnotné distribuované systému souborů Hadoop (HDFS) v porovnání s Azure Blob storage. Slouží jako rozšíření HDFS a poskytuje bezproblémové prostředí pro zákazníky. Umožňuje úplnou sadu součástí ekosystému Hadoop pracovat přímo na data, která spravuje. Azure Blob storage a HDFS jsou systémy různých souborů, které jsou optimalizované pro úložiště dat a výpočty na tato data. Informace o výhodách používání úložiště objektů Blob v Azure najdete v tématu [použití Azure Blob storage s HDInsight][hdinsight-storage].

**Požadavky**

Než začnete, vezměte na vědomí následující požadavky:

* Cluster Azure HDInsight. Pokyny najdete v tématu [Začínáme s Azure HDInsight] [ hdinsight-get-started] nebo [zřizování clusterů HDInsight][hdinsight-provision].

## <a name="why-blob-storage"></a>Proč úložiště objektů blob?
Azure HDInsight clustery jsou obvykle nasazují ke spuštění úloh MapReduce, a clustery jsou vyřazen po dokončení těchto úloh. Zachovat data v HDFS clustery po dokončení se výpočty by nákladné způsob uložení dat této. Azure Blob storage je vysoce dostupný, vysoce škálovatelné a vysoce kapacitu, možnosti nízkonákladového a ke sdílení úložiště pro data, která se má zpracovat pomocí HDInsight. Ukládání dat do objektu BLOB umožní clusterů HDInsight, které jsou používány pro výpočty bezpečně vydané bez ztráty dat.

### <a name="directories"></a>Adresáře
Kontejnery Azure Blob storage ukládají data jako páry klíč/hodnota a neexistuje žádná hierarchie adresářů. Znak "/" můžete použít název klíče, ale jevit jako soubor jsou uložená v do struktury adresářů. HDInsight uvidí tyto jako v případě, že jsou skutečné adresáře.

Klíč k objektu blob může být například *input/log1.txt*. Žádný skutečný "vstupní" adresář existuje, ale z důvodu přítomnosti znak "/" v názvu klíče, připomíná zobrazení cesty k souboru.

Z toho důvodu Pokud pomocí nástroje Průzkumník Azure může dojít k některé soubory 0 bajtů. Tyto soubory mají dva účely:

* Pokud jsou prázdné složky, označte existence složky. Azure Blob storage je dostatečně inteligentní vědět, pokud existuje objekt blob názvem foo/panelu, se složku s názvem **foo**. Jediný způsob, jak označily k prázdné složce názvem, ale **foo** je tak, že tento soubor speciální 0 bajtů na místě.
* Drží speciální metadata, která je potřeba systému souborů Hadoop, zejména oprávnění a vlastníků složek.

## <a name="command-line-utilities"></a>Nástroje příkazového řádku
Společnost Microsoft poskytuje následující nástroje pro práci s Azure Blob storage:

| Nástroj | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Rozhraní příkazového řádku Azure][azurecli] |✔ |✔ |✔ |
| [Prostředí Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] | | |✔ |
| [Příkaz Hadoop](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> Při rozhraní příkazového řádku Azure, Azure PowerShell a AzCopy můžete všechny možné použít mimo Azure, Hadoop příkaz je dostupná pouze na clusteru HDInsight a umožňuje pouze načítání dat z místního systému souborů do úložiště objektů Blob v Azure.
>
>

### <a id="xplatcli"></a>Rozhraní příkazového řádku Azure
Rozhraní příkazového řádku Azure je napříč platformami nástroj, který vám umožní spravovat služby Azure. Odeslání dat do úložiště objektů Blob v Azure pomocí následujících kroků:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Instalace a konfigurace rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows](../cli-install-nodejs.md).
2. Otevřete příkazový řádek, bash nebo jiné prostředí a k ověření vašeho předplatného Azure použijte následující postupy.

        azure login

    Po zobrazení výzvy zadejte uživatelské jméno a heslo pro vaše předplatné.
3. Zadejte následující příkaz k zobrazení seznamu účtů úložiště pro vaše předplatné:

        azure storage account list
4. Vyberte účet úložiště, který obsahuje objekt blob, které chcete pracovat, potom načíst klíč pro tento účet použijte následující příkaz:

        azure storage account keys list <storage-account-name>

    To by měla vrátit **primární** a **sekundární** klíče. Kopírování **primární** hodnotu klíče, protože se použije v dalších krocích.
5. Použijte následující příkaz k načtení seznamu kontejnery objektů blob v rámci účtu úložiště:

        azure storage container list -a <storage-account-name> -k <primary-key>
6. Pomocí následujících příkazů nahrávání a stahování souborů na objekt blob:

   * Pro nahrání souboru:

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * Ke stažení souboru:

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> Pokud budete pracovat vždy pomocí stejného účtu úložiště, můžete nastavit následující proměnné prostředí místo zadání účtu a klíče pro každý příkaz:
>
> * **AZURE\_úložiště\_účet**: název účtu úložiště
> * **AZURE\_úložiště\_přístup\_klíč**: klíče účtu úložiště.
>
>

### <a id="powershell"></a>Prostředí Azure PowerShell
Prostředí Azure PowerShell je skriptovací prostředí, které můžete řídit a automatizovat nasazení a správy vašich zatížení v Azure. Informace o konfiguraci pracovní stanice ke spuštění prostředí Azure PowerShell najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Nahrát do úložiště objektů Blob Azure do místního souboru**

1. Otevřete konzolu prostředí Azure PowerShell podle pokynů v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).
2. Nastavte hodnoty prvních pět proměnných v následující skript:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. Vložte skript do konzoly Azure PowerShell spouštět se zkopírovat soubor.

Například skripty prostředí PowerShell, které jsou vytvořeny pro práci s HDInsight, najdete v části [nástroje HDInsight](https://github.com/blackmist/hdinsight-tools).

### <a id="azcopy"></a>AzCopy
AzCopy je nástroj příkazového řádku, který je navržený pro zjednodušit přenosu dat do a z účtu úložiště Azure. Můžete používat jako samostatný nástroj nebo začlenit tento nástroj v existující aplikaci. [Stáhněte si nástroj AzCopy][azure-azcopy-download].

AzCopy syntaxe je:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Další informace najdete v tématu [AzCopy - nahrávání nebo stahování souborů pro objekty BLOB Azure][azure-azcopy].

### <a id="commandline"></a>Hadoop příkazového řádku
Hadoop příkazového řádku využijete pouze pro ukládání dat do úložiště objektů blob, když se data nachází již hlavního uzlu clusteru.

Chcete-li použít příkaz Hadoop, musíte nejdřív připojit k headnode pomocí jedné z následujících metod:

* **HDInsight se systémem Windows**: [připojit pomocí vzdálené plochy](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **HDInsight se systémem Linux**: připojení pomocí protokolu SSH ([příkazu SSH](hdinsight-hadoop-linux-use-ssh-unix.md) nebo [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))

Po připojení můžete nahrát soubor do úložiště syntaxi.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Například `hadoop fs -copyFromLocal data.txt /example/data/data.txt`.

Protože výchozí systém souborů pro HDInsight v úložišti objektů Azure Blob, /example/data.txt ve skutečnosti je v úložišti objektů Blob Azure. Můžete se také podívat na soubor jako:

    wasb:///example/data/data.txt

nebo

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Seznam dalších Hadoop příkazy svou práci se soubory, najdete v části [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> Na clustery HBase blok výchozí velikost použitou při zápisu dat je 256KB. Když to funguje bez problémů při používání rozhraní API HBase nebo rozhraní REST API, pomocí `hadoop` nebo `hdfs dfs` příkazy k zápisu dat je větší než ~ 12 GB výsledkem chyba. Najdete v článku [pro zápis na objekt blob úložiště výjimka](#storageexception) části níže Další informace.
>
>

## <a name="graphical-clients"></a>Grafické klientů
Existují také několik aplikací, které poskytují grafické rozhraní pro práci s Azure Storage. Následuje seznam několik z těchto aplikací:

| Klient | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Microsoft Visual Studio Tools pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |✔ |✔ |✔ |
| [Azure Storage Explorer](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Cloudové úložiště Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Průzkumník Azure](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

### <a name="visual-studio-tools-for-hdinsight"></a>Visual Studio Tools pro HDInsight
Další informace najdete v tématu [procházejte propojené prostředky](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

### <a id="storageexplorer"></a>Azure Storage Explorer
*Azure Storage Explorer* je užitečným nástrojem pro kontrolu a změna data do objektů BLOB. Je bezplatný nástroj, který si můžete stáhnout z [http://storageexplorer.com/](http://storageexplorer.com/). Zdrojový kód je k dispozici také tento odkaz.

Před použitím nástroje, musíte znát klíč účet a název účtu úložiště Azure. Pokyny o načtení těchto informací najdete v tématu "postupy: zobrazení, kopírování a opětovné vytváření přístupových klíčů úložiště" části [vytvořit, spravovat nebo odstranit účet úložiště][azure-create-storage-account].

1. Spuštění Průzkumníka úložiště Azure. Pokud je to poprvé spustíte Průzkumníka úložiště, zobrazí se výzva pro **název účtu _Storage** a **klíč účtu úložiště**. Pokud spustíte ji před použít **přidat** tlačítko přidáte název nového účtu úložiště a klíč.

    Zadejte název a klíč pro účet úložiště používá váš cluster HDInsight a pak vyberte **Uložit & OTEVŘETE**.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. V seznamu nalevo od rozhraní kontejnery klikněte na název kontejneru, který je přidružený k vašemu clusteru HDInsight. Ve výchozím nastavení to je název clusteru HDInsight, ale může lišit, pokud jste zadali při vytváření clusteru určitý název.
3. Z panelu nástrojů vyberte ikonu nahrávání.

    ![Panel nástrojů se zvýrazněnou ikonou nahrávání](./media/hdinsight-upload-data/toolbar.png)
4. Zadejte soubor, a pak klikněte na **otevřete**. Po zobrazení výzvy vyberte **nahrát** pro nahrání souboru do kořenového adresáře kontejner úložiště. Pokud chcete nahrát soubor do určité cesty, zadejte cestu v **cílové** pole a pak vyberte **nahrát**.

    ![Dialogové okno nahrání souboru](./media/hdinsight-upload-data/fileupload.png)

    Po dokončení nahrávání souboru můžete z úloh v clusteru HDInsight.

## <a name="mount-azure-blob-storage-as-local-drive"></a>Připojit jako místní disk úložiště objektů Blob v Azure
V tématu [úložiště objektů Blob Azure připojit jako místní disk](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

## <a name="services"></a>Služby
### <a name="azure-data-factory"></a>Azure Data Factory
Služba Azure Data Factory je plně spravovaná služba pro sestavování služby pro úložiště, zpracování dat a dat přesun dat do efektivní, škálovatelného a spolehlivého datových kanálů produkční.

Azure Data Factory slouží pro přesun dat do úložiště objektů Blob v Azure nebo k vytvoření datových kanálů, které přímo používat HDInsight funkce jako je například Hive a Pig.

Další informace najdete v tématu [dokumentace Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

### <a id="sqoop"></a>Apache Sqoop
Sqoop je nástroj sloužící k přenosu dat mezi Hadoop a relačními databázemi. Můžete ho pro import dat ze systému správy relačních databází (RDBMS), například SQL Server, MySQL a Oracle do systému souborů Hadoop distributed (HDFS), transformovat data v Hadoop pomocí MapReduce nebo Hive a pak exportovat data zpět do relační.

Další informace najdete v tématu [Sqoop použití s HDInsight][hdinsight-use-sqoop].

## <a name="development-sdks"></a>Vývoj sady SDK
Azure Blob storage můžete také získat přístup pomocí sady Azure SDK z těchto programovacích jazycích:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Další informace o instalaci sady Azure SDK najdete v tématu [stáhne Azure](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Řešení potíží
### <a id="storageexception"></a>Výjimka úložiště pro zápis u objektu blob
**Příznaky**: při použití `hadoop` nebo `hdfs dfs` příkazy k zápisu souborů, které jsou ~ 12 GB nebo větší na HBase cluster, může dojít k následující chybě:

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**Příčina**: HBase v HDInsight clustery výchozí velikost bloku 256 KB při zápisu do úložiště Azure. Když tento postup funguje pro rozhraní API HBase nebo rozhraní REST API, výsledkem bude k chybě při použití `hadoop` nebo `hdfs dfs` nástroje příkazového řádku.

**Řešení**: použití `fs.azure.write.request.size` k určení větší velikost bloku. To provedete na základě za použití pomocí `-D` parametr. Následuje příklad použití tohoto parametru se `hadoop` příkaz:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Můžete taky zvýšit hodnotu `fs.azure.write.request.size` globálně pomocí Ambari. Následující postup slouží ke změně hodnoty v uživatelském rozhraní Ambari Web:

1. V prohlížeči přejděte na webové uživatelské rozhraní Ambari pro váš cluster. Toto je https://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je název clusteru.

    Po zobrazení výzvy zadejte jméno správce a heslo pro cluster.
2. Na levé straně obrazovky vyberte **HDFS**a pak vyberte **konfigurací** kartě.
3. V **filtru...**  zadejte `fs.azure.write.request.size`. Tato akce zobrazí pole a aktuální hodnotu uprostřed stránky.
4. Změňte hodnotu z 262144 (256KB) na novou hodnotu. Například 4194304 (4MB).

![Obrázek změna prostřednictvím webové uživatelské rozhraní Ambari](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Další informace o používání Ambari najdete v tématu [Správa clusterů HDInsight pomocí webového uživatelského rozhraní Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Další kroky
Teď, když chápete, jak získat data do HDInsight, přečtěte si zjistěte, jak provádět analýzy v těchto článcích:

* [Začínáme se službou Azure HDInsight][hdinsight-get-started]
* [Odesílání úloh Hadoop prostřednictvím kódu programu][hdinsight-submit-jobs]
* [Použití Hivu se službou HDInsight][hdinsight-use-hive]
* [Použití Pigu se službou HDInsight][hdinsight-use-pig]

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
