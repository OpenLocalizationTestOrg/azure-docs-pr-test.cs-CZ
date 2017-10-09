---
title: "aaaUpload dat pro úlohy Hadoop v HDInsight | Microsoft Docs"
description: "Zjistěte, jak hello tooupload a přístup dat pro úlohy Hadoop v HDInsight pomocí rozhraní příkazového řádku Azure, Azure Storage Explorer, Azure PowerShell, hello Hadoop příkazového řádku nebo Sqoop."
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
ms.openlocfilehash: 15da602085d41c19789e34800f3d9e238d7d1de8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Nahrání dat úloh Hadoopu do služby HDInsight
Azure HDInsight nabízí plnohodnotné distribuované systému souborů Hadoop (HDFS) v porovnání s Azure Blob storage. Je určený jako rozšíření tooprovide HDFS bezproblémové prostředí toocustomers. Umožňuje hello úplnou sadu součástí toooperate ekosystém Hadoop hello přímo na hello dat, které spravuje. Azure Blob storage a HDFS jsou systémy různých souborů, které jsou optimalizované pro úložiště dat a výpočty na tato data. Informace o hello výhody používání úložiště objektů Blob v Azure najdete v tématu [použití Azure Blob storage s HDInsight][hdinsight-storage].

**Požadavky**

Vezměte na vědomí následující požadavky, než začnete hello:

* Cluster Azure HDInsight. Pokyny najdete v tématu [Začínáme s Azure HDInsight] [ hdinsight-get-started] nebo [zřizování clusterů HDInsight][hdinsight-provision].

## <a name="why-blob-storage"></a>Proč úložiště objektů blob?
Azure HDInsight clustery jsou obvykle nasadit úloh MapReduce toorun a hello clustery jsou vyřazen po dokončení těchto úloh. Zachovat hello data v clusterech HDFS hello po dokončení se výpočty by toostore nákladné způsob, jak tato data. Azure Blob storage je vysoce dostupný, vysoce škálovatelné a vysoce kapacitu, možnosti nízkonákladového a ke sdílení úložiště pro data, která je toobe zpracované pomocí HDInsight. Ukládání dat do objektu BLOB umožní hello clusterů HDInsight, které se používají pro výpočet toobe bezpečně vydaná bez ztráty dat.

### <a name="directories"></a>Adresáře
Kontejnery Azure Blob storage ukládají data jako páry klíč/hodnota a neexistuje žádná hierarchie adresářů. Ale hello "/" znak lze použít v rámci toomake hello název klíče se zobrazí, jako kdyby je soubor uložit do do struktury adresářů. HDInsight uvidí tyto jako v případě, že jsou skutečné adresáře.

Klíč k objektu blob může být například *input/log1.txt*. Žádný skutečný "vstupní" adresář existuje, ale z důvodu přítomnosti toohello hello "/" znak v názvu klíče hello má hello vzhled cestu k souboru.

Z toho důvodu Pokud pomocí nástroje Průzkumník Azure může dojít k některé soubory 0 bajtů. Tyto soubory mají dva účely:

* Pokud jsou prázdné složky, označte hello existence hello složky. Azure Blob storage je dostatečně inteligentní tooknow, že pokud objekt blob názvem foo/panelu existuje, je k dispozici složku s názvem **foo**. Ale hello pouze toosignify způsob, jak volat k prázdné složce **foo** je tak, že tento soubor speciální 0 bajtů na místě.
* Budou uloženy speciální metadata, která je potřeba pomocí hello Hadoop systému souborů, zejména oprávnění hello a vlastníků složek hello.

## <a name="command-line-utilities"></a>Nástroje příkazového řádku
Společnost Microsoft poskytuje následující nástroje toowork s úložištěm Azure Blob hello:

| Nástroj | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Rozhraní příkazového řádku Azure][azurecli] |✔ |✔ |✔ |
| [Prostředí Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] | | |✔ |
| [Příkaz Hadoop](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> Při hello rozhraní příkazového řádku Azure, Azure PowerShell a AzCopy můžete všechny možné použít mimo Azure, hello Hadoop příkaz je dostupná pouze na clusteru HDInsight hello a umožňuje pouze načítání dat z hello místního systému souborů do úložiště objektů Blob v Azure.
>
>

### <a id="xplatcli"></a>Rozhraní příkazového řádku Azure
Hello rozhraní příkazového řádku Azure je napříč platformami nástroj, který vám umožní toomanage Azure services. Použijte následující kroky tooupload data tooAzure Blob storage hello:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Instalace a konfigurace hello příkazového řádku Azure CLI pro Mac, Linux a Windows](../cli-install-nodejs.md).
2. Otevřete příkazový řádek, bash nebo jiné prostředí a použít hello následující tooauthenticate tooyour předplatného Azure.

        azure login

    Po zobrazení výzvy zadejte hello uživatelské jméno a heslo pro vaše předplatné.
3. Zadejte následující příkaz toolist hello účty úložiště pro vaše předplatné hello:

        azure storage account list
4. Vyberte účet úložiště hello, která obsahuje objekt blob hello, které chcete toowork s, potom použijte následující příkaz tooretrieve hello klíč pro tento účet hello:

        azure storage account keys list <storage-account-name>

    To by měla vrátit **primární** a **sekundární** klíče. Kopírování hello **primární** hodnotu klíče, protože se použije v dalších krocích hello.
5. Použijte následující příkaz tooretrieve seznam kontejnery objektů blob v rámci účtu úložiště hello hello:

        azure storage container list -a <storage-account-name> -k <primary-key>
6. Použijte následující příkazy tooupload hello a stáhnout soubory toohello blob:

   * tooupload souboru:

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * toodownload souboru:

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> Pokud bude vždy pracovat s hello stejný účet úložiště, můžete nastavit hello následující proměnné prostředí místo zadávání hello účtu a klíče pro každý příkaz:
>
> * **AZURE\_úložiště\_účet**: název účtu úložiště hello
> * **AZURE\_úložiště\_přístup\_klíč**: hello klíče účtu úložiště.
>
>

### <a id="powershell"></a>Prostředí Azure PowerShell
Prostředí Azure PowerShell je skriptovací prostředí, můžete použít toocontrol a automatizovat hello nasazení a správy vašich zatížení v Azure. Informace o konfiguraci vašeho pracovní stanice toorun prostředí Azure PowerShell najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**tooupload místního souboru tooAzure úložiště objektů Blob**

1. Konzoly Azure PowerShell otevřené hello podle pokynů v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).
2. Nastavení hodnoty hello hello prvních pět proměnných v hello následující skript:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get hello storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create hello storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy hello file from local workstation toohello Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. Hello vložení do toorun konzoly Azure PowerShell hello ho toocopy hello soubor skriptu.

Například toowork vytvořit skripty prostředí PowerShell s HDInsight, najdete v části [nástroje HDInsight](https://github.com/blackmist/hdinsight-tools).

### <a id="azcopy"></a>AzCopy
AzCopy je nástroj příkazového řádku, který je určený toosimplify hello úlohy přenosu dat do a z účtu úložiště Azure. Můžete používat jako samostatný nástroj nebo začlenit tento nástroj v existující aplikaci. [Stáhněte si nástroj AzCopy][azure-azcopy-download].

Hello AzCopy syntaxe je:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Další informace najdete v tématu [AzCopy - nahrávání nebo stahování souborů pro objekty BLOB Azure][azure-azcopy].

### <a id="commandline"></a>Hadoop příkazového řádku
Hello Hadoop příkazového řádku využijete pouze pro ukládání dat do úložiště objektů blob při hello dat již existuje hello hlavního uzlu clusteru.

V pořadí toouse hello příkaz Hadoop nejprve je nutné připojit pomocí některého z následujících metod hello headnode toohello:

* **HDInsight se systémem Windows**: [připojit pomocí vzdálené plochy](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **HDInsight se systémem Linux**: připojení pomocí protokolu SSH ([hello příkazu SSH](hdinsight-hadoop-linux-use-ssh-unix.md) nebo [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))

Po připojení můžete použít následující syntaxi tooupload toostorage souboru hello.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Například `hadoop fs -copyFromLocal data.txt /example/data/data.txt`.

Protože hello výchozí systém souborů pro HDInsight v úložišti objektů Azure Blob, /example/data.txt ve skutečnosti je v úložišti objektů Blob Azure. Můžete se také podívat toohello souboru jako:

    wasb:///example/data/data.txt

nebo

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Seznam dalších Hadoop příkazy svou práci se soubory, najdete v části [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> Na clustery HBase použít výchozí velikost bloku hello při zápisu dat je 256KB. Když to funguje bez problémů při používání rozhraní API HBase nebo rozhraní REST API, pomocí hello `hadoop` nebo `hdfs dfs` větší než ~ 12 GB dat toowrite příkazy, které jsou výsledkem chyba. V tématu hello [pro zápis na objekt blob úložiště výjimka](#storageexception) části níže Další informace.
>
>

## <a name="graphical-clients"></a>Grafické klientů
Existují také několik aplikací, které poskytují grafické rozhraní pro práci s Azure Storage. Hello následuje seznam několik z těchto aplikací:

| Klient | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Microsoft Visual Studio Tools pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |✔ |✔ |✔ |
| [Azure Storage Explorer](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Cloudové úložiště Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Průzkumník Azure](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

### <a name="visual-studio-tools-for-hdinsight"></a>Visual Studio Tools pro HDInsight
Další informace najdete v tématu [přejděte hello propojené prostředky](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

### <a id="storageexplorer"></a>Azure Storage Explorer
*Azure Storage Explorer* je užitečným nástrojem pro kontrolu a změna hello data do objektů BLOB. Je bezplatný nástroj, který si můžete stáhnout z [http://storageexplorer.com/](http://storageexplorer.com/). Hello zdrojový kód je k dispozici také tento odkaz.

Před použitím nástroje hello, musíte znát klíč účet a název účtu úložiště Azure. Pokyny k načtení těchto informací naleznete v tématu hello "postupy: zobrazení, kopírování a opětovné vytváření přístupových klíčů úložiště" části [vytvořit, spravovat nebo odstranit účet úložiště][azure-create-storage-account].

1. Spuštění Průzkumníka úložiště Azure. Pokud je tato hello poprvé spustíte hello Storage Explorer, zobrazí se výzva pro hello **název účtu _Storage** a **klíč účtu úložiště**. Pokud spustíte ji před použijte hello **přidat** tlačítko tooadd název nového účtu úložiště a klíč.

    Zadejte hello názvů a klíče pro účet úložiště hello používá váš cluster HDInsight a pak vyberte **Uložit & OTEVŘETE**.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. V seznamu hello kontejnery toohello nalevo od rozhraní hello klikněte na název hello hello kontejneru, který je přidružený k vašemu clusteru HDInsight. Ve výchozím nastavení to je název hello hello clusteru HDInsight, ale může lišit, pokud jste zadali při vytváření clusteru hello určitý název.
3. Panel nástrojů hello vyberte ikonu nahrávání hello.

    ![Panel nástrojů se zvýrazněnou ikonou nahrávání](./media/hdinsight-upload-data/toolbar.png)
4. Zadejte soubor tooupload a pak klikněte na tlačítko **otevřete**. Po zobrazení výzvy vyberte **nahrát** tooupload hello souboru toohello kořenovém kontejneru úložiště hello. Pokud chcete, aby tooupload hello tooa konkrétní cesta, zadejte cestu hello v hello **cílové** pole a pak vyberte **nahrát**.

    ![Dialogové okno nahrání souboru](./media/hdinsight-upload-data/fileupload.png)

    Po dokončení nahrávání souboru hello můžete z úloh na clusteru HDInsight hello.

## <a name="mount-azure-blob-storage-as-local-drive"></a>Připojit jako místní disk úložiště objektů Blob v Azure
V tématu [úložiště objektů Blob Azure připojit jako místní disk](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

## <a name="services"></a>Služby
### <a name="azure-data-factory"></a>Azure Data Factory
Hello služby Azure Data Factory je plně spravovaná služba pro sestavování služby pro úložiště, zpracování dat a dat přesun dat do efektivní, škálovatelného a spolehlivého datových kanálů produkční.

Azure Data Factory lze použít toomove data do úložiště objektů Blob v Azure nebo toocreate datových kanálů, které přímo pomocí HDInsight funkcí, jako například Hive a vepřových.

Další informace najdete v tématu hello [dokumentace Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

### <a id="sqoop"></a>Apache Sqoop
Sqoop je dat tootransfer nástroj navržený mezi Hadoop a relačními databázemi. Můžete ji použít tooimport data ze systému správy relačních databází (RDBMS), jako je SQL Server, MySQL a Oracle do systému souborů Hadoop distributed hello (HDFS), transformovat hello data v Hadoop pomocí MapReduce nebo Hive a poté exportujte hello data zpět do RELAČNÍ.

Další informace najdete v tématu [Sqoop použití s HDInsight][hdinsight-use-sqoop].

## <a name="development-sdks"></a>Vývoj sady SDK
Azure Blob storage můžete také získat přístup pomocí sady Azure SDK z hello následující programovací jazyky:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Další informace o instalaci hello sadami SDK služby Azure najdete v tématu [stáhne Azure](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Řešení potíží
### <a id="storageexception"></a>Výjimka úložiště pro zápis u objektu blob
**Příznaky**: při použití hello `hadoop` nebo `hdfs dfs` příkazy toowrite soubory, které jsou ~ 12 GB nebo větší v clusteru služby HBase můžete setkat s hello následující chybě:

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
    Caused by: com.microsoft.azure.storage.StorageException: hello request body is too large and exceeds hello maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**Příčina**: HBase v HDInsight clustery výchozí velikost bloku tooa 256 kB při zápisu tooAzure úložiště. Když tento postup funguje pro rozhraní API HBase nebo rozhraní REST API, výsledkem bude k chybě při použití hello `hadoop` nebo `hdfs dfs` nástroje příkazového řádku.

**Řešení**: použití `fs.azure.write.request.size` toospecify větší velikost bloku. To provedete na základě za použití pomocí hello `-D` parametr. Hello následuje příklad použití tohoto parametru s hello `hadoop` příkaz:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Můžete taky zvýšit hodnotu hello `fs.azure.write.request.size` globálně pomocí Ambari. Hello následující postup může být použitá hodnota hello toochange v hello webové uživatelské rozhraní Ambari:

1. V prohlížeči přejděte toohello webové uživatelské rozhraní Ambari pro váš cluster. Toto je https://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je hello názvem vašeho clusteru.

    Po zobrazení výzvy zadejte hello správce jméno a heslo pro hello cluster.
2. Hello levé straně obrazovky hello, vyberte **HDFS**a potom vyberte hello **konfigurací** kartě.
3. V hello **filtru...**  zadejte `fs.azure.write.request.size`. Tato akce zobrazí pole hello a aktuální hodnota uprostřed hello stránku hello.
4. Změňte hodnotu hello z 262144 (256KB) toohello novou hodnotu. Například 4194304 (4MB).

![Obrázek změna hello prostřednictvím webové uživatelské rozhraní Ambari](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Další informace o používání Ambari najdete v tématu [clusterů HDInsight spravovat pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Další kroky
Teď, když znáte jak číst tooget data do HDInsight, hello následující články toolearn jak tooperform analýzy:

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
