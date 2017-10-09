---
title: "aaaQuery data z HDFS kompatibilního úložiště Azure - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooquery dat z úložiště Azure a Azure Data Lake Store toostore výsledky analýzy."
keywords: blob storage, hdfs, structured data, unstructured data, data lake store, Hadoop input, Hadoop output, hadoop storage, hdfs input, hdfs output, hdfs storage, wasb azure
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1d2e65f2-16de-449e-915f-3ffbc230f815
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: jgao
ms.openlocfilehash: 1032d60424b65e3c0c54a25c7c15970b017a788f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Použití úložiště Azure s clustery Azure HDInsight

tooanalyze data v clusteru HDInsight, můžete uložit data hello buď do úložiště Azure, Azure Data Lake Store nebo obojí. Obě možnosti úložiště povolit toosafely odstranění clusterů HDInsight, jsou používány pro výpočty, aniž by se ztratila uživatelská data.

Hadoop podporuje hodnoty z hello výchozí systém souborů. Hello výchozí systém souborů znamená výchozí schéma a autoritu. Lze také použít tooresolve relativní cesty. Během procesu vytváření clusteru HDInsight hello zadáte kontejner objektů blob v Azure Storage jako hello výchozí systém souborů, nebo s HDInsight 3.5, můžete vybrat Azure Storage nebo Azure Data Lake Store jako hello výchozí systém souborů s několika výjimkami. Hello možnosti použití Data Lake Store jako výchozí hello a propojené úložiště, najdete v části [dostupnosti pro HDInsight cluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).

V tomto článku se dozvíte, jak služba Azure Storage pracuje s clustery HDInsight. toolearn jak funguje Data Lake Store s clustery HDInsight, najdete v části [clusterů pomocí Azure Data Lake Store s Azure HDInsight](hdinsight-hadoop-use-data-lake-store.md). Další informace o vytvoření clusteru HDInsight najdete v tématu [Vytváření clusterů Hadoop ve službě HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

Azure Storage je robustní řešení úložiště pro obecné účely, které se jednoduše integruje se službou HDInsight. HDInsight můžete použít kontejner objektů blob v Azure Storage jako hello výchozí systém souborů pro hello cluster. Pomocí rozhraní Hadoop system (HDFS) souborů DFS hello úplnou sadu součásti v HDInsight můžete pracovat přímo strukturovaných nebo nestrukturovaných dat uložené jako objekty BLOB.

> [!WARNING]
> Při vytváření účtu služby Azure Storage je k dispozici několik možností. Hello následující tabulka obsahuje informace o jaké možnosti jsou podporovány v prostředí HDInsight:
> 
> | Typ účtu úložiště | Úroveň úložiště | Podporováno se službou HDInsight |
> | ------- | ------- | ------- |
> | Účet služby Storage pro obecné účely | Standard | __Ano__ |
> | &nbsp; | Premium | Ne |
> | Účet služby Blob Storage | Hot | Ne |
> | &nbsp; | Cool | Ne |

Nedoporučujeme používat k ukládání firemních dat hello výchozí kontejner objektu blob. Odstranění výchozího kontejneru blob hello po každé použití tooreduce náklady na úložiště je vhodné. Všimněte si, že hello výchozí kontejner obsahuje aplikace a systému protokoly. Ujistěte se, že tooretrieve hello protokoly před odstraněním hello kontejneru.

Sdílení jednoho kontejneru objektů blob pro několik clusterů se nepodporuje.

## <a name="hdinsight-storage-architecture"></a>Architektura úložiště HDInsight
Hello následující diagram představuje abstraktní zobrazení Dobrý den architektura úložiště HDInsight pomocí Azure Storage:

![Clusterů systému Hadoop pomocí rozhraní API HDFS tooaccess hello a ukládání strukturovaných a nestrukturovaných dat v úložišti objektů Blob. ] (./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "Architektura úložiště HDInsight")

HDInsight poskytuje, aby byl systém souborů toohello distribuované přístup, který je místně připojen toohello výpočetních uzlů. V tomto systému souborů je přístupný pomocí hello plně kvalifikovaný identifikátor URI, třeba:

    hdfs://<namenodehost>/<path>

Kromě toho HDInsight umožňuje tooaccess data, která je uložená ve službě Azure Storage. Hello syntaxe je:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

Při použití účtu Azure Storage s clustery HDInsight je potřeba zvážit tyto aspekty.

* **Kontejnery v účtech úložiště hello, které jsou připojené tooa clusteru:** protože hello název účtu a klíč jsou přidružené hello clusteru během vytváření, máte plný přístup toohello objektů BLOB v těchto kontejnerech.

* **Veřejné kontejnery nebo veřejné objekty BLOB v účtech úložiště, které nejsou připojené tooa clusteru:** máte oprávnění jen pro čtení toohello objekty BLOB v kontejnerech hello.
  
  > [!NOTE]
  > Veřejné kontejnery umožňují tooget seznam všech objektů BLOB, které jsou k dispozici v tomto kontejneru a získat metadata kontejneru. Veřejné objekty BLOB umožní objekty BLOB hello tooaccess pouze v případě, že znáte přesnou adresu URL hello. Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">omezit přístup toocontainers a objekty BLOB</a>.
  > 
  > 
* **Privátní kontejnery v účtech úložiště, které nejsou připojené tooa clusteru:** hello objekty BLOB v kontejnerech hello nemáte přístup, dokud nedefinujete účet úložiště hello při odesílání úlohy WebHCat hello. To se vysvětluje dále v tomhle článku.

Hello účty úložiště, které jsou definovány v procesu vytváření hello a jejich klíče jsou uloženy v %HADOOP_HOME%/conf/core-site.xml na uzlech clusteru hello. Hello výchozím chováním služby HDInsight je účty úložiště hello toouse definované v souboru core-site.xml hello. Toto nastavení můžete upravit pomocí [Ambari](./hdinsight-hadoop-manage-ambari.md).

Více úloh WebHCat, včetně Hive, MapReduce, streamování Hadoop a Pig, může obsahovat popis účtů úložiště a spojených metadat. (To aktuálně funguje pro Pig s účty úložiště, ale ne pro metadata.) Více informací najdete v části [Použití clusteru HDInsight s alternativními účty úložiště a metaúložišti](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Objekty blob lze použít pro strukturovaná i nestrukturovaná data. Kontejnery objektů blob ukládají data jako páry klíč/hodnota a neexistuje žádná hierarchie adresářů. Ale hello lomítko (/) lze použít v rámci hello toomake název klíče se zobrazí, jako kdyby je soubor uložit do do struktury adresářů. Klíč k objektu blob může být například *input/log1.txt*. Žádný skutečný *vstupní* adresář existuje, ale z důvodu přítomnosti toohello hello lomítku v názvu klíče hello, má hello vzhled cestu k souboru.

## <a id="benefits"></a>Výhody služby Azure Storage
Hello znamenalo, že způsob hello hello výpočetní clustery jsou vytvořeny zavřít toohello prostředků účtu úložiště uvnitř oblasti Azure, kde vysokorychlostní síť hello umožňuje hello zmírnit výkonová náročnost společně vyhledáním výpočetní clustery a prostředky úložiště efektivní pro výpočetní uzly hello tooaccess hello data do úložiště Azure.

Existuje více výhod spojených s ukládáním hello dat v úložišti Azure místo HDFS:

* **Opakované použití dat a sdílení:** hello data v HDFS se nachází uvnitř výpočetního clusteru hello. Pouze hello aplikace, které mají přístup toohello výpočetní cluster může používat hello data pomocí rozhraní API HDFS. Hello data v úložišti Azure je přístupná prostřednictvím hello rozhraní API HDFS nebo prostřednictvím hello [rozhraní API REST úložiště objektů Blob][blob-storage-restAPI]. Proto s větším počtem aplikací (včetně jiných clusterů HDInsight) a nástroje může být použité tooproduce a využívají hello data.
* **Archivace dat:** ukládání dat v úložišti Azure umožňuje clustery HDInsight hello používá pro výpočet toobe bezpečně odstranit, aniž by se ztratila uživatelská data.
* **Náklady na úložiště dat:** ukládání dat v systému souborů DFS pro hello dlouhodobého hlediska dražší než ukládání hello dat v úložišti Azure, protože hello náklady na výpočetním clusteru je vyšší než hello náklady na úložiště Azure. Kromě toho protože hello dat nemá toobe znovu pro každou generaci výpočetních clusterů, můžete se také ukládání dat načítání náklady.
* **Elastické škálování:** i když HDFS poskytuje rozšířené souborové systému, hello škála se určuje podle hello počet uzlů, které vytvoříte pro svůj cluster. Změna měřítka hello se může stát složitější než využití elastického hello škálování, které jste získali automaticky v úložišti Azure.
* **Geografická replikace:** Službu Azure Storage je možné geograficky replikovat. I když to vám dává geografické obnovení a redundanci dat, převzetí služeb při selhání toohello geograficky replikovaného umístění vážně ovlivňuje výkon a mohou být účtovány další poplatky. Doporučujeme proto toochoose hello geografickou replikaci dobře a pouze v případě, že hodnota hello hello dat je vhodné hello dalších poplatků.

Některé úlohy a balíčky MapReduce můžou vytvořit mezilehlé výsledky, že nechcete, aby skutečně toostore v úložišti Azure. V tomto případě můžete vybrat, zda toostore hello data v hello místní HDFS. Ve skutečnosti služba HDInsight používá DFS pro některé z těchto mezilehlých výsledků v úlohách Hive a jiných procesech.

> [!NOTE]
> Většina příkazů HDFS (například <b>ls</b>, <b>copyFromLocal</b> a <b>mkdir</b>) bude i nadále fungovat podle očekávání. Pouze hello příkazy, které jsou specifické toohello nativní implementaci HDFS (což je odkazované tooas systému souborů DFS), například <b>fschk</b> a <b>dfsadmin</b>, zobrazit různé chování v úložišti Azure.
> 
> 

## <a name="create-blob-containers"></a>Vytvoření kontejnerů objektů Blob
toouse objekty BLOB, nejdřív vytvoříte [účet úložiště Azure][azure-storage-create]. Jako součást tohoto postupu zadejte oblast Azure, kde se má vytvořit účet úložiště hello. Hello clusteru a účet úložiště hello musí být uloženy ve hello stejné oblasti. databáze serveru SQL metaúložiště Hive Hello a metaúložiště Oozie databáze musí být také umístěny v systému SQL Server hello stejné oblasti.

Bez ohledu na jeho žije, patří každý objekt blob, které vytvoříte tooa kontejneru v účtu úložiště Azure. Tento kontejner může být existující objekt blob, který se vytvořil mimo HDInsight, nebo to může být kontejner, který se vytvořil pro cluster služby HDInsight.

Hello výchozí kontejner objektu Blob ukládá informace o specifických pro cluster například historie úlohy a protokoly. Výchozí kontejner objektu Blob nesdílejte s více clustery služby HDInsight. Může dojít k poškození historie úlohy. Doporučujeme toouse jiný kontejner pro každý cluster a umístit sdílená data na propojený účet úložiště zadaný v nasazení všech příslušných clusterů, nikoli hello výchozí účet úložiště. Další informace o konfiguraci propojených účtů úložiště najdete v tématu [Tvorba clusterů HDInsight][hdinsight-creation]. Ale můžete znovu použít výchozí kontejner úložiště po odstranění původního clusteru HDInsight hello. Pro clustery HBase můžete zachovat hello schématu tabulky HBase a data vytvořením nového clusteru HBase pomocí hello výchozí kontejner objektu blob používaný clusterem HBase, který byl odstraněn.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-hello-azure-portal"></a>Hello použití portálu Azure
Při vytváření clusteru služby HDInsight z hello portál, máte hello možnosti (jak je znázorněno níže) tooprovide hello podrobnosti o účtu úložiště. Můžete také, zda má účet další úložiště přidruženého k hello clusteru a pokud ano, vybírat Data Lake Store nebo jiný objekt blob úložiště Azure jako dodatečné úložiště hello.

![Zdroj dat pro vytvoření hadoopu HDInsight](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> Použití účtu další úložiště v jiném umístění než hello HDInsight cluster není podporováno.


### <a name="use-azure-powershell"></a>Použití Azure Powershell
Pokud jste [instalace a konfigurace prostředí Azure PowerShell][powershell-install], můžete použít hello následujících z hello prostředí Azure PowerShell výzva toocreate účtu úložiště a kontejneru:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

### <a name="use-azure-cli"></a>Použití Azure CLI

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

Pokud máte [nainstalováno a nakonfigurováno rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md), hello následující příkaz, může být použité tooa účtu úložiště a kontejneru.

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> Hello `--type` parametr určuje, jak se replikují hello účet úložiště. Další informace najdete v tématu [Replikace Azure Storage](../storage/storage-redundancy.md). Nepoužívejte ZRS, protože nepodporuje objekt blob stránky, soubor, tabulku ani frontu.
> 
> 

Jste hello výzvami toospecify zeměpisnou se oblast, která je vytvoření účtu úložiště hello v. Měli byste vytvořit účet úložiště hello v hello stejné oblasti, kterou chcete použít k vytvoření clusteru služby HDInsight.

Po vytvoření účtu úložiště hello použijte hello následující klíče účtu úložiště hello tooretrieve příkaz:

    azure storage account keys list <storageaccountname>

toocreate kontejner, hello použijte následující příkaz:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="address-files-in-azure-storage"></a>Adresování souborů ve službě Azure Storage
Schéma identifikátoru URI Hello pro přístup k souborům v úložišti Azure z prostředí HDInsight je:

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

Hello schéma identifikátoru URI poskytuje nezašifrovaný přístup (s hello *wasb:* předponu) a zašifrovaný přístup SSL (s *wasbs*). Doporučujeme používat *wasbs* kdykoli je to možné, i když hello přístupem dat, které je umístěn uvnitř stejné oblasti v Azure.

Hello &lt;BlobStorageContainerName&gt; identifikuje název hello hello kontejner objektů blob v úložišti Azure.
Hello &lt;StorageAccountName&gt; identifikuje název účtu úložiště Azure hello. Vyžaduje se plně kvalifikovaný název domény (FQDN).

Pokud ani &lt;BlobStorageContainerName&gt; ani &lt;StorageAccountName&gt; byl zadán, hello výchozí systém souborů se používá. U souborů hello na hello výchozí systém souborů můžete použít relativní cestu nebo absolutní cesta. Například hello *hadoop-mapreduce-examples.jar* soubor, který se dodává s clustery HDInsight lze odkazované tooby pomocí jedné z následujících hello:

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> Název souboru Hello je <i>hadoop-examples.jar</i> v clusterech HDInsight verze 2.1 a 1.6.
> 
> 

Hello &lt;cesta&gt; je název cesty HDFS hello pro soubor nebo adresář. Vzhledem k tomu, že kontejnery ve službě Azure Storage jsou jednoduše úložiště párů klíč-hodnota, neexistuje žádný opravdový hierarchický systém souborů. Lomítko ( / ) uvnitř klíče objektu blob se považuje za oddělovač adresářů. Například název hello objektu blob pro *hadoop-mapreduce-examples.jar* je:

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> Při práci s objekty BLOB mimo HDInsight, většina nástrojů nerozpoznávají hello formát WASB a místo toho očekávají základní formát cesty, jako například `example/jars/hadoop-mapreduce-examples.jar`.
> 
> 

## <a name="access-blobs"></a>Přístup k objektům blob 


### <a name="access-blobs-using-azure-powershell"></a> Použití Azure Powershellu
> [!NOTE]
> Hello příkazy v této části jsou ukázkami základních příkladů použití prostředí PowerShell tooaccess data ukládají do objektů BLOB. Více plnohodnotný příklad, který je přizpůsobený pro práci s HDInsight, naleznete v části hello [nástroje HDInsight](https://github.com/Blackmist/hdinsight-tools).
> 
> 

Použijte následující příkaz toolist hello týkajících se objektu blob rutiny hello:

    Get-Command *blob*

![Seznam rutin prostředí PowerShell týkajících se objektu blob.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a>Nahrání souborů
V tématu [nahrát data tooHDInsight][hdinsight-upload-data].

#### <a name="download-files"></a>Stažení souborů
Hello následující skript stáhne aktuální složku toohello objekt blob bloku. Před spouštění skriptu hello změňte složku tooa hello kde máte oprávnění k zápisu.

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # hello storage account used for hello default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # hello default file system container has hello same name as hello cluster.
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    # Use Add-AzureAccount if you haven't connected tooyour Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List hello downloaded file ..." -ForegroundColor Green
    cat "./$blob"

Poskytuje název skupiny prostředků hello a hello název clusteru, můžete použít následující kód hello:

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force


#### <a name="delete-files"></a>Odstranění souborů
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a>Zobrazení souborů
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a>Spuštění dotazů Hive pomocí nedefinovaného účtu úložiště
Tento příklad ukazuje, jak hello toolist složky z účtu úložiště, které není definováno během procesu vytváření.
$clusterName = “<HDInsightClusterName>“

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

### <a name="use-azure-cli"></a>Použití Azure CLI
Použijte následující příkaz toolist hello týkajících se objektu blob příkazy hello:

    azure storage blob

**Příklad použití Azure CLI tooupload soubor**

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Příklad použití Azure CLI toodownload soubor**

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

**Příklad použití Azure CLI toodelete soubor**

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Příklad použití Azure CLI toolist soubory**

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="use-additional-storage-accounts"></a>Použití dalších účtů úložiště

Při vytváření clusteru služby HDInsight, zadejte účet služby Azure Storage hello, že chcete tooassociate s ním. Kromě toho toothis účet úložiště, můžete přidat další úložiště účtů z hello stejné předplatné Azure nebo různých předplatných Azure během procesu vytváření hello nebo po vytvoření clusteru. Pokyny pro přidání dalších účtů úložiště najdete v tématu [Vytváření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> Použití účtu další úložiště v jiném umístění než hello HDInsight cluster není podporováno.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak toouse HDFS kompatibilní úložiště Azure s HDInsight. To vám umožní toobuild, škálovatelnou a dlouhodobé, archivace řešení pro získávání dat a používání HDInsight toounlock hello informací uvnitř hello uložené strukturovaných a nestrukturovaných dat.

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
