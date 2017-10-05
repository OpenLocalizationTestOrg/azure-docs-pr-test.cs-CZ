---
title: "Správa clusterů systému Hadoop pomocí rozhraní příkazového řádku Azure - Azure HDInsight | Microsoft Docs"
description: "Naučte se používat rozhraní příkazového řádku pro správu clusterů systému Hadoop v prostředí Azure HDInsight. Rozhraní příkazového řádku Azure funguje v systému Windows, Mac a Linux."
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
ms.openlocfilehash: 0ee9f2f28978b207dcaf8f77950bd82a897d3fd1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Správa clusterů systému Hadoop v HDInsight pomocí rozhraní příkazového řádku Azure
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Další informace o použití [rozhraní příkazového řádku Azure](../cli-install-nodejs.md) ke správě clusterů systému Hadoop v prostředí Azure HDInsight. Rozhraní příkazového řádku Azure je implementované v Node.js. Dá se použít na jakékoli platformě, která podporuje Node.js, včetně systému Windows, Mac a Linux.

Tento článek se týká jenom používání rozhraní příkazového řádku Azure s HDInsight. Obecné informace o tom, jak používat rozhraní příkazového řádku Azure najdete v tématu [instalace a konfigurace rozhraní příkazového řádku Azure][azure-command-line-tools].

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

## <a name="prerequisites"></a>Požadavky
Je nutné, abyste před zahájením tohoto článku měli tyto položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Rozhraní příkazového řádku Azure** – Informace týkající se instalace a konfigurace najdete v tématu [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).
* **Připojit k Azure**, pomocí následujícího příkazu:
  
        azure login
  
    Další informace týkající se ověřování pomocí pracovního nebo školního účtu najdete v tématu [Připojení k předplatnému Azure z rozhraní příkazového řádku Azure](../xplat-cli-connect.md).
* **Přepněte do režimu Azure Resource Manager**, a to pomocí následujícího příkazu:
  
        azure config mode arm

Chcete-li získat nápovědu, použijte **-h** přepínače.  Například:

    azure hdinsight cluster create -h

## <a name="create-clusters-with-the-cli"></a>Vytvoření clusterů pomocí rozhraní příkazového řádku
V tématu [Vytvoření clusterů v HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Seznam a zobrazit podrobnosti o clusteru
Použijte následující příkazy na seznamu a zobrazit podrobnosti o clusteru:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![Příkazového řádku zobrazení seznamu clusteru][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Odstranění clusterů
Pokud chcete odstranit cluster použijte následující příkaz:

    azure hdinsight cluster delete <Cluster Name>

Odstraněním skupiny prostředků, která obsahuje clusteru můžete také odstranit cluster. Poznámka: Tato akce odstraní všechny prostředky ve skupině, včetně výchozí účet úložiště.

    azure group delete <Resource Group Name>

## <a name="scale-clusters"></a>Škálování clusterů
Chcete-li změnit velikost clusteru Hadoop:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Povolí nebo zakáže přístup protokolu HTTP pro cluster
    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Povolí nebo zakáže přístup RDP pro cluster
      azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
      azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili, jak provádět různé úlohy správy clusteru HDInsight. Další informace naleznete v následujících článcích:

* [Spravovat HDInsight pomocí portálu Azure][hdinsight-admin-portal]
* [Spravovat HDInsight pomocí prostředí Azure PowerShell][hdinsight-admin-powershell]
* [Začínáme se službou Azure HDInsight][hdinsight-get-started]
* [Jak používat rozhraní příkazového řádku Azure][azure-command-line-tools]

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
