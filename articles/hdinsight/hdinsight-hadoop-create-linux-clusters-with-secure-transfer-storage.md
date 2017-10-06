---
title: "aaaCreate Hadoop cluster s účty úložiště bezpečnému přenosu v Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak clustery HDInsight toocreate s bezpečnému přenosu povolené účty úložiště Azure."
keywords: hadoop getting started, hadoop linux, hadoop quickstart, secure transfer, azure storage account
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: jgao
ms.openlocfilehash: 0acb8814ad0d5d5b5652d930b2e3da90f9d7978d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Vytvoření clusteru Hadoop s účty úložiště s bezpečným přenosem ve službě Azure HDInsight

Hello [zabezpečení přenosu požadované](../storage/common/storage-require-secure-transfer.md) funkce zlepšuje hello zabezpečení vašeho účtu úložiště Azure vynucením všechny požadavky tooyour účet prostřednictvím zabezpečeného připojení. Tato funkce a hello wasbs schématu jsou podporovány pouze verze clusteru HDInsight, 3,6 nebo novější. 

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít:

* **Předplatné Azure**: toocreate Bezplatný zkušební účet jeden měsíc, procházet příliš[azure.microsoft.com/free](https://azure.microsoft.com/free).
* **Účet služby Azure Storage s povoleným zabezpečeným přenosem**. Hello pokyny najdete v tématu [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) a [vyžadují zabezpečený přenos](../storage/common/storage-require-secure-transfer.md).
* **Kontejner objektů Blob v účtu úložiště hello**. 
## <a name="create-cluster"></a>Vytvoření clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


V této části vytvoříte cluster Hadoop ve službě HDInsight pomocí [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md). Šablona Hello se nachází v [Gibhubu](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/). Zkušenosti s šablonou Resource Manageru nejsou pro postup dle tohoto kurzu vyžadovány. Další metody vytváření clusterů a principy vlastnosti hello používané v tomto kurzu, najdete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klikněte na tlačítko hello toosign bitové kopie v tooAzure a otevřete hello šablony Resource Manageru v hello portálu Azure. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. Postupujte podle hello pokyny toocreate hello clusteru s hello následující specifikace: 

    - Zadejte verzi služby HDInsight 3.6.  je Hello výchozí verze 3.5. Vyžaduje se verze 3.6 nebo novější.
    - Zadejte účet úložiště s povoleným zabezpečeným přenosem.
    - Použijte krátký název pro účet úložiště hello.
    - Hello účtu úložiště a kontejneru objektů blob hello musí být vytvořen předem. 

    Hello pokyny najdete v tématu [vytvořit cluster](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 

Pokud používáte skript akce tooprovide vlastní konfigurační soubory, musíte použít wasbs v hello následující nastavení:

- fs.defaultFS (základní web)
- spark.eventLog.dir 
- spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Přidání dalších účtů úložiště

Existuje několik možností tooadd další bezpečnému přenosu povoleno úložiště účtů:

- Úprava šablony Azure Resource Manager hello v poslední části hello.
- Vytvoření clusteru s podporou pomocí hello [portál Azure](https://portal.azure.com) a zadejte propojený účet úložiště.
- Použití skriptu akce tooadd další zabezpečené povoleno úložiště účtů tooan stávajícího clusteru HDInsight.  Další informace najdete v tématu [přidejte další úložiště účtů tooHDInsight](hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili jak toocreate clusteru služby HDInsight a povolit zabezpečený přenos toohello účty úložiště.

toolearn Další informace o analýze dat pomocí HDInsight, najdete v části hello následující články:

* toolearn Další informace o používání Hive s HDInsight, včetně jak dotazuje tooperform Hive ze sady Visual Studio, najdete v části [používání Hive s HDInsight][hdinsight-use-hive].
* toolearn o Pig jazyk používaný datový tootransform najdete v tématu [použijte Pig s HDInsight][hdinsight-use-pig].
* toolearn o MapReduce, způsob, jakým toowrite programy, které zpracovávají data v Hadoop, najdete v části [používání MapReduce s HDInsight][hdinsight-use-mapreduce].
* toolearn o používání hello nástroje HDInsight pro Visual Studio tooanalyze data v HDInsight, najdete v části [začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Další informace o tom, jak HDInsight ukládá data toolearn nebo jak tooget data do HDInsight, najdete v části hello následující články:

* Informace o tom, jak HDInsight používá Azure Storage, najdete v tématu [Používání Azure Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Informace o tom tooupload data tooHDInsight, najdete v části [nahrát data tooHDInsight][hdinsight-upload-data].

toolearn Další informace o vytváření a správě clusteru služby HDInsight najdete hello následující články:

* toolearn o správě clusteru HDInsight se systémem Linux, najdete v části [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md).
* toolearn Další informace o možnosti hello můžete vybrat při vytváření clusteru HDInsight, najdete v části [vytváření HDInsight v Linuxu pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md).
* Pokud jste obeznámeni s Linux a Hadoop, ale chcete tooknow podrobnosti o Hadoop na hello HDInsight, přečtěte si téma [práce s HDInsight v Linuxu](hdinsight-hadoop-linux-information.md). Tento článek obsahuje informace o:
  
  * Adresách URL služeb hostovaných na hello clusteru, například Ambari a WebHCat
  * Hello umístění souborů Hadoop a příkladech v hello místního systému souborů
  * Hello použití služby Azure Storage (WASB) namísto HDFS jako úložiště dat výchozí hello

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


