---
title: "aaaCreate clusterů systému Hadoop pomocí hello příkazového řádku - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate clusterů HDInsight pomocí hello a platformy Azure CLI 1.0."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 50b01483-455c-4d87-b754-2229005a8ab9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 5295b01054b8c23df0e3b75a3e0e8c933ac48b3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-using-hello-azure-cli"></a>Vytvoření clusterů HDInsight pomocí hello rozhraní příkazového řádku Azure

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Hello kroky tohoto návodu dokumentu vytváření clusteru HDInsight 3.5 pomocí hello Azure CLI 1.0.

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).


## <a name="prerequisites"></a>Požadavky

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure CLI**. pomocí Azure CLI verze 0.10.14 poslední testovali Hello kroky v tomto dokumentu.

    > [!IMPORTANT]
    > pomocí Azure CLI 2.0 nefungují Hello kroky v tomto dokumentu. Azure CLI 2.0 nepodporuje vytváření clusteru služby HDInsight.

## <a name="log-in-tooyour-azure-subscription"></a>Přihlaste se tooyour předplatného Azure

Postupujte podle kroků hello popsaných v [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure (Azure CLI) hello](../xplat-cli-connect.md) a připojte se pomocí hello předplatné tooyour **přihlášení** metoda.

## <a name="create-a-cluster"></a>Vytvoření clusteru

z příkazového řádku, jako je například PowerShell nebo Bash je třeba provést následující kroky Hello.

1. Použijte následující příkaz tooauthenticate tooyour předplatného Azure hello:

        azure login

    Můžete se výzvami tooprovide své jméno a heslo. Pokud máte víc předplatných Azure, použijte `azure account set <subscriptionname>` použijte příkazy tooset hello předplatné, které hello rozhraní příkazového řádku Azure.

2. Přepnutí režimu Resource Manager tooAzure pomocí hello následující příkaz:

        azure config mode arm

3. Vytvořte skupinu prostředků. Tato skupina prostředků obsahuje hello HDInsight cluster a související účet úložiště.

        azure group create groupname location

    * Nahraďte `groupname` s jedinečným názvem skupiny hello.

    * Nahraďte `location` s hello zeměpisnou oblast, která chcete v toocreate hello skupiny.

       Seznam platných umístění, použijte hello `azure location list` příkaz a potom použijte jednu z umístění hello z hello `Name` sloupce.

4. Vytvoření účtu úložiště. Tento účet úložiště se používá jako hello výchozí úložiště pro hello HDInsight cluster.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * Nahraďte `groupname` hello názvem skupiny hello vytvořili v předchozím kroku hello.

    * Nahraďte `location` s hello používá stejné umístění v předchozím kroku hello.

    * Nahraďte `storagename` s jedinečným názvem pro účet úložiště hello.

        > [!NOTE]
        > Další informace o hello parametry použité v tomto příkazu použít `azure storage account create -h` tooview nápovědu pro tento příkaz.

5. Načtení hello klíč používá účet úložiště tooaccess hello.

        azure storage account keys list -g groupname storagename

    * Nahraďte `groupname` s názvem skupiny prostředků hello.
    * Nahraďte `storagename` hello název účtu úložiště hello.

     V hello data, která je vrácena, uložte hello `key` hodnota `key1`.

6. Vytvoření clusteru HDInsight.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Nahraďte `groupname` s názvem skupiny prostředků hello.

    * Nahraďte `Hadoop` s typem clusteru hello chcete toocreate. Například `Hadoop`, `HBase`, `Kafka`, `Spark`, nebo `Storm`.

     > [!IMPORTANT]
     > HDInsight clustery jsou v různých typů, které odpovídají toohello zatížení a technologie, která hello clusteru je přizpůsobená pro. Neexistuje žádná podporovaná metoda toocreate clusteru, který kombinuje více typů, jako je například Storm a HBase v jednom clusteru.

    * Nahraďte `location` s hello používá stejné umístění v předchozích krocích.

    * Nahraďte `storagename` hello názvem účtu úložiště.

    * Nahraďte `storagekey` klíčem hello získaných v předchozím kroku hello.

    * Pro hello `--defaultStorageContainer` parametr, hello použijte stejný název jako používáte pro hello cluster.

    * Nahraďte `admin` a `httppassword` hello jméno a heslo chcete toouse při přístupu k hello clusteru prostřednictvím protokolu HTTPS.

    * Nahraďte `sshuser` a `sshuserpassword` s hello uživatelského jména a hesla nechcete toouse při přístupu k hello clusteru pomocí protokolu SSH

    > [!IMPORTANT]
    > Tento příklad vytvoří cluster s dvě poznámky pracovního procesu. Hello počet uzlů pracovního procesu můžete také změnit po vytvoření clusteru provedením operace škálování. Pokud máte v úmyslu používat více než 32 uzlů pracovního procesu, je nutné vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti RAM. Velikost hlavního uzlu hello lze nastavit pomocí hello `--headNodeSize` parametru během vytváření clusteru.
    >
    > Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

    To může trvat několik minut, než toofinish procesu vytváření clusteru hello. Obvykle přibližně 15.

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky

Teď, když jste úspěšně vytvořili clusteru HDInsight pomocí hello rozhraní příkazového řádku Azure, použijte následující toolearn jak hello toowork k vašemu clusteru:

### <a name="hadoop-clusters"></a>Clustery Hadoop

* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>Clustery HBase

* [Začínáme s HBase v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Vývoj aplikací v jazyce Java pro HBase v HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Clustery Storm

* [Vývoj topologie Java pro Storm v HDInsight](hdinsight-storm-develop-java-topology.md)
* [Použití komponent, Python v Storm v HDInsight](hdinsight-storm-develop-python-topology.md)
* [Nasazení a monitorování topologie se Storm v HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
