---
title: "aaaManage Hadoop clusterů v HDInsight pomocí prostředí PowerShell – Azure | Microsoft Docs"
description: "Zjistěte, jak tooperform administrativní úlohy pro hello clusterů systému Hadoop v HDInsight pomocí Azure PowerShell."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: bfdfa754-18e5-4ef9-b0d6-2dbdcebc0283
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 3df082d752fa8c703db82a54b82b740290af6729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Správa clusterů systému Hadoop v HDInsight pomocí prostředí Azure PowerShell
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Prostředí Azure PowerShell je výkonná skriptovací prostředí, můžete použít toocontrol a automatizovat hello nasazení a správy vašich zatížení v Azure. V tomto článku se dozvíte, jak používat clusterů systému Hadoop toomanage v Azure HDInsight pomocí místní konzoly Azure PowerShell prostřednictvím hello prostředí Windows PowerShell. Seznam hello hello rutiny prostředí HDInsight PowerShell, naleznete v části [reference k rutině HDInsight][hdinsight-powershell-reference].

**Požadavky**

Před zahájením tohoto článku, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Instalace prostředí Azure PowerShell
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Pokud jste nainstalovali Azure PowerShell verze 0,9 x, odinstalujte jej před instalací na novější verzi.

nainstalovaná verze hello toocheck hello prostředí PowerShell:

    Get-Module *azure*

toouninstall hello starší verze, spouštět programy a funkce v Ovládacích panelech hello.

## <a name="create-clusters"></a>Vytváření clusterů
V tématu [vytvořit systémem Linux clusterů v HDInsight pomocí Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Seznam clustery
Použijte následující příkaz toolist hello všechny clustery v aktuálním předplatném hello:

    Get-AzureRmHDInsightCluster

## <a name="show-cluster"></a>Zobrazit clusteru
Použijte následující příkaz tooshow podrobnosti o konkrétní cluster v aktuálním předplatném hello hello:

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

## <a name="delete-clusters"></a>Odstranění clusterů
Použijte následující příkaz toodelete cluster hello:

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

Můžete také odstranit cluster odebráním hello skupinu prostředků, který obsahuje hello clusteru. Poznámka: Tato akce odstraní všechny hello prostředky ve skupině hello včetně hello výchozí účet úložiště.

    Remove-AzureRmResourceGroup -Name <Resource Group Name>

## <a name="scale-clusters"></a>Škálování clusterů
Hello clusteru škálování funkce vám umožní toochange hello počet uzlů pracovního procesu používá cluster, který běží v Azure HDInsight bez nutnosti toore – vytvoření clusteru hello.

> [!NOTE]
> Pouze clustery s HDInsight verze 3.1.3 nebo vyšší nejsou podporovány. Pokud si nejste jistí hello verze vašeho clusteru, můžete zkontrolovat hello stránku vlastností.  V tématu [seznamu a zobrazit clustery](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
>
>

Hello dopad změny hello počet uzlů dat pro každý typ clusteru podporuje HDInsight:

* Hadoop

    Můžete zvýšit bezproblémově hello počet uzlů pracovního procesu v clusteru Hadoop, který běží bez dopadu na všechny úlohy čekající na vyřízení nebo spuštěné. Nové úlohy můžete také odeslány, když probíhá operace hello. Selhání v rámci operace škálování pohodlné zpracování tak, aby hello clusteru vždy zůstává ve funkčním stavu.

    Pokud se Hadoop cluster měřítko snížením hello počet uzlů data, některé služby hello v clusteru hello restartovat. To způsobí, že všechny spuštěné a čeká se na úlohy toofail na dokončení hello hello operace škálování. Po dokončení operace hello může, ale odešlete znovu hello úlohy.
* HBase

    Můžete bezproblémově přidat nebo odebrat uzly clusteru HBase tooyour, když je spuštěná. Místní servery jsou automaticky vyváženy během několika minut od hello operace škálování. Ale můžete také ručně vyvážit hello místní servery přihlášením toohello headnode clusteru a spuštěné hello z okna příkazového řádku následující příkazy:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* Storm

    Můžete bez problémů přidat nebo odebrat cluster Storm tooyour uzly dat je spuštěna. Ale po úspěšném dokončení hello operace škálování, budete potřebovat toorebalance hello topologie.

    Vyrovnává lze dosáhnout dvěma způsoby:

  * Storm webového uživatelského rozhraní
  * Nástroj pro rozhraní příkazového řádku (CLI)

    Podrobnosti najdete toohello [dokumentaci Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) další podrobnosti.

    Hello Storm webového uživatelského rozhraní není k dispozici v clusteru HDInsight hello:

    ![Obnovte rovnováhu škálování HDInsight storm](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    Tady je příklad jak toouse hello rozhraní příkazového řádku příkaz topologie Storm toorebalance hello:

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

toochange hello velikost clusteru Hadoop pomocí prostředí Azure PowerShell, spusťte následující příkaz z klientský počítač hello:

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>


## <a name="grantrevoke-access"></a>Udělení nebo odvolání přístupu
Clustery HDInsight mít hello následující HTTP webové služby (všechny tyto služby mají RESTful koncových bodů):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Ve výchozím nastavení jsou tyto služby oprávnění pro přístup. Můžete můžete odvolat nebo udělit přístup hello. toorevoke:

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

toogrant:

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter hello Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter hello HTTP username and password:" -UserName "admin"

    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

> [!NOTE]
> Pomocí udělení nebo odvolání přístupu hello, obnoví hello clusteru uživatelské jméno a heslo.
>
>

To lze provést také prostřednictvím hello portálu. V tématu [hello spravovat HDInsight pomocí portálu Azure][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>Aktualizovat pověření uživatele HTTP
Je hello stejný postup jako [HTTP udělení nebo odvolání přístupu](#grant/revoke-access). Pokud hello clusteru byla udělena hello přístup protokolu HTTP, musí se nejdřív odvolat.  A pak udělují přístup hello s novými pověřeními uživatele HTTP.

## <a name="find-hello-default-storage-account"></a>Najít hello výchozí účet úložiště
Hello následující skript prostředí Powershell ukazuje, jak tooget hello výchozí název účtu úložiště a hello výchozí klíč účtu úložiště pro cluster.

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

## <a name="find-hello-resource-group"></a>Najít skupinu prostředků hello
V režimu Resource Manager hello patří každý cluster HDInsight tooan skupina prostředků Azure.  Skupina prostředků toofind hello:

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


## <a name="submit-jobs"></a>Odesílání úloh
**úlohy MapReduce toosubmit**

V tématu [ukázky spustit Hadoop MapReduce v HDInsight se systémem Windows](hdinsight-run-samples.md).

**toosubmit úloh Hive**

V tématu [spouštění dotazů Hive pomocí prostředí PowerShell](hdinsight-hadoop-use-hive-powershell.md).

**úlohy Pig toosubmit**

V tématu [úlohy Pig spuštění pomocí prostředí PowerShell](hdinsight-hadoop-use-pig-powershell.md).

**toosubmit Sqoop úlohy**

V tématu [použití nástroje Sqoop se HDInsight](hdinsight-use-sqoop.md).

**toosubmit Oozie úlohy**

V tématu [Oozie použití s Hadoop toodefine a spouštění pracovních postupů v prostředí HDInsight](hdinsight-use-oozie.md).

## <a name="upload-data-tooazure-blob-storage"></a>Nahrání dat tooAzure Blob storage
V tématu [nahrát data tooHDInsight][hdinsight-upload-data].

## <a name="see-also"></a>Viz také
* [HDInsight rutiny referenční dokumentaci k nástroji][hdinsight-powershell-reference]
* [Spravovat HDInsight pomocí hello portálu Azure][hdinsight-admin-portal]
* [Spravovat HDInsight pomocí rozhraní příkazového řádku][hdinsight-admin-cli]
* [Vytvoření clusterů HDInsight][hdinsight-provision]
* [Nahrání dat tooHDInsight][hdinsight-upload-data]
* [Odesílání úloh Hadoop prostřednictvím kódu programu][hdinsight-submit-jobs]
* [Začínáme se službou Azure HDInsight][hdinsight-get-started]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
