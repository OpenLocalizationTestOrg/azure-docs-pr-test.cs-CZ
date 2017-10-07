---
title: "aaaCreate clusterů systému Hadoop pomocí prostředí PowerShell – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak se toocreate Hadoop, HBase, Storm a Spark clustery v Linuxu pro HDInsight pomocí prostředí Azure PowerShell."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4208deca-d64a-45e1-8948-2673d5d7678c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 53afe4702f6b61a0720ceda48a4a34d7fa8797d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Vytvořit clustery se systémem Linux v HDInsight pomocí Azure PowerShell

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Prostředí Azure PowerShell je výkonná skriptovací prostředí, můžete použít toocontrol a automatizovat hello nasazení a správy vašich úloh ve službě Microsoft Azure. Tento dokument obsahuje informace o tom, jak toocreate HDInsight se systémem Linux clusteru pomocí prostředí Azure PowerShell. Zahrnuje také ukázkový skript.

> [!NOTE]
> Prostředí Azure PowerShell je dostupná pouze na klienty se systémem Windows. Pokud používáte klienta Linux, Unix nebo Mac OS X, přečtěte si téma [vytvořit cluster HDInsight se systémem Linux pomocí příkazového řádku Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) informace o používání rozhraní příkazového řádku Azure toocreate hello clusteru.

## <a name="prerequisites"></a>Požadavky
Musí mít před zahájením tohoto postupu hello následující:

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Azure PowerShell](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali. kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.
    >
    > Postupujte podle kroků hello v [nainstalovat Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooinstall hello nejnovější verzi prostředí Azure PowerShell. Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) Další informace.

## <a name="create-cluster"></a>Vytvoření clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

toocreate clusteru HDInsight pomocí prostředí Azure PowerShell, je třeba provést následující postupy hello:

* Vytvoření skupiny prostředků Azure.
* Vytvoření účtu služby Azure Storage
* Vytvořit kontejner objektů Blob v Azure
* Vytvoření clusteru HDInsight

ukazuje, Hello následující skript jak toocreate do nového clusteru:

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

Hello hodnoty, které zadáte pro přihlášení hello clusteru jsou použité toocreate hello uživatelský účet Hadoop pro hello cluster. Použijte tento účet tooconnect tooservices hostovaná v clusteru hello jako jsou webová uživatelská rozhraní nebo rozhraní REST API.

Hello hodnoty, které zadáte pro uživatele SSH hello jsou použité toocreate hello SSH uživatele pro hello cluster. Použít účet toostart a vzdálené relace SSH na hello clusteru a spouštění úloh. Další informace najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.

> [!IMPORTANT]
> Pokud máte v plánu toouse víc než 32 uzlů pracovního procesu (buď při vytváření clusteru, nebo škálování hello clusteru po vytvoření), musíte také zadat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti RAM.
>
> Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

Může trvat až minut toocreate too20 clusteru.

## <a name="create-cluster-configuration-object"></a>Vytvoření clusteru: objekt konfigurace

Můžete taky vytvořit objekt konfigurace HDInsight pomocí `New-AzureRmHDInsightClusterConfig` rutiny. Poté můžete upravit tato konfigurace objektu tooenable další možnosti konfigurace pro váš cluster. Nakonec použijte hello `-Config` parametr hello `New-AzureRmHDInsightCluster` rutiny toouse hello konfigurace.

Hello následující skript vytvoří objekt tooconfigure konfigurace serveru R na typ clusteru HDInsight. Konfigurace Hello umožňuje hraniční uzel, Rstudia a účet úložiště.

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> Použití účtu úložiště v jiném umístění než hello HDInsight cluster není podporováno. Při použití v tomto příkladu, vytvořit účet úložiště hello hello stejné umístění jako hello server.

## <a name="customize-clusters"></a>Přizpůsobení clusterů

* V tématu [clusterů HDInsight přizpůsobit pomocí Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
* V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-hello-cluster"></a>Odstranění clusteru hello

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky

Teď, když jste úspěšně vytvořili clusteru služby HDInsight, použijte následující prostředky toolearn jak hello toowork k vašemu clusteru.

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

### <a name="spark-clusters"></a>Clustery Spark

* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)

