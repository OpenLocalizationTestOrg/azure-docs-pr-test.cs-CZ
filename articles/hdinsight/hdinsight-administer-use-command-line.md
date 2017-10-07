---
title: "aaaManage clusterů systému Hadoop pomocí rozhraní příkazového řádku Azure - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse hello rozhraní příkazového řádku Azure toomanage Hadoop clusterů v Azure HDInsight. Hello rozhraní příkazového řádku Azure funguje v systému Windows, Mac a Linux."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: 4f26c79f-8540-44bd-a470-84722a9e4eca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 03b0cff9331c1c581095b80cc6d1177d843ffa83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-hello-azure-cli"></a>Správa clusterů systému Hadoop v HDInsight pomocí hello rozhraní příkazového řádku Azure
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Zjistěte, jak toouse hello [rozhraní příkazového řádku Azure](../cli-install-nodejs.md) toomanage Hadoop clusterů v Azure HDInsight. Hello rozhraní příkazového řádku Azure je implementované v Node.js. Dá se použít na jakékoli platformě, která podporuje Node.js, včetně systému Windows, Mac a Linux.

Tento článek se týká jenom používání hello rozhraní příkazového řádku Azure s HDInsight. Obecné informace o tom, toouse používáte Azure CLI, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure][azure-command-line-tools].

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Rozhraní příkazového řádku Azure** -najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) informace týkající se instalace a konfigurace.
* **Připojit tooAzure**pomocí hello následující příkaz:
  
        azure login
  
    Další informace týkající se ověřování pomocí pracovního nebo školního účtu najdete v tématu [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure hello](../xplat-cli-connect.md).
* **Přepínač režimu Azure Resource Manager toohello**pomocí hello následující příkaz:
  
        azure config mode arm

tooget nápovědy použijte hello **-h** přepínače.  Například:

    azure hdinsight cluster create -h

## <a name="create-clusters-with-hello-cli"></a>Vytvoření clusterů s hello rozhraní příkazového řádku
V tématu [hello vytvořit clusterů v HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Seznam a zobrazit podrobnosti o clusteru
Použijte následující příkazy toolist hello a zobrazit podrobnosti o clusteru:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![Příkazového řádku zobrazení seznamu clusteru][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Odstranění clusterů
Použijte následující příkaz toodelete cluster hello:

    azure hdinsight cluster delete <Cluster Name>

Můžete také odstranit cluster odstraněním hello skupinu prostředků, který obsahuje hello clusteru. Poznámka: Tato akce odstraní všechny hello prostředky ve skupině hello včetně hello výchozí účet úložiště.

    azure group delete <Resource Group Name>

## <a name="scale-clusters"></a>Škálování clusterů
toochange hello velikost clusteru Hadoop:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Povolí nebo zakáže přístup protokolu HTTP pro cluster
    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Povolí nebo zakáže přístup RDP pro cluster
      azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
      azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak tooperform různé HDInsight clusteru úlohy správy. toolearn více, najdete v části hello následující články:

* [Spravovat HDInsight pomocí hello portálu Azure][hdinsight-admin-portal]
* [Spravovat HDInsight pomocí prostředí Azure PowerShell][hdinsight-admin-powershell]
* [Začínáme se službou Azure HDInsight][hdinsight-get-started]
* [Jak toouse hello rozhraní příkazového řádku Azure][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Seznam a zobrazit clustery"
