---
title: "aaaHow toodelete clusteru služby HDInsight - Azure | Microsoft Docs"
description: "Informace o hello různé způsoby, kterými můžete odstranit cluster služby HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 55f7838b-9786-47ff-96db-1b64437bd0bb
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 5b9d9a09eecfdcfaed7a1f5ebab440e13bd358b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-hello-azure-cli"></a>Odstranění clusteru služby HDInsight pomocí vašeho prohlížeče, prostředí PowerShell nebo hello rozhraní příkazového řádku Azure

Jakmile clusteru je vytvořen a zastaví, když odstranění clusteru hello začne běžet fakturace clusteru HDInsight. Fakturuje se fakturují za minutu, proto byste měli vždy odstranit cluster, když je již používán. V tomto dokumentu zjistíte, jak hello toodelete clusteru pomocí portálu Azure, Azure PowerShell a hello Azure CLI 1.0.

> [!IMPORTANT]
> Odstranění clusteru služby HDInsight neodstraní hello účty Azure Storage nebo Data Lake Store přidruženého k hello clusteru. Data uložená v těchto služeb v hello budoucí můžete znovu použít.

## <a name="azure-portal"></a>portál Azure

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) a vyberte clusteru HDInsight. Pokud váš cluster HDInsight není vázaný toohello řídicí panel, můžete je vyhledat podle názvu pomocí pole hledání hello.
   
    ![hledání portálu](./media/hdinsight-delete-cluster/navbar.png)

2. Po otevření okna hello hello clusteru vybrat hello **odstranit** ikonu. Po zobrazení výzvy vyberte **Ano** toodelete hello clusteru.
   
    ![Odstranit ikonu](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Z řádku prostředí PowerShell použijte následující příkaz toodelete hello clusteru hello:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight.

## <a name="azure-cli-10"></a>Azure CLI 1.0

Z řádku použijte následující toodelete hello clusteru hello:

    azure hdinsight cluster delete CLUSTERNAME

Nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight.

> [!NOTE]
> Azure CLI 2.0 nepodporuje odstraňování clusterů HDInsight v tuto chvíli (31 července 2017).